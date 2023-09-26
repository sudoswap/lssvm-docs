# Bonding Curves and Pricing

sudoswap pairs use bonding curves to determine buy and sell prices. When creating a pair, liquidity providers choose from a list of whitelisted curves. A pair's bonding curve can be read using `bondingCurve()` and cannot be changed once the pair is created.

sudoswap V2 currently has four whitelisted bonding curves:

- LINEAR_CURVE_V2: [0xe5d78fec1a7f42d2F3620238C498F088A866FdC5](https://etherscan.io/address/0xe5d78fec1a7f42d2f3620238c498f088a866fdc5)
- EXPONENTIAL_CURVE_V2: [0xfa056C602aD0C0C4EE4385b3233f2Cb06730334a](https://etherscan.io/address/0xfa056c602ad0c0c4ee4385b3233f2cb06730334a)
- XYK_CURVE_V2: [0xc7fB91B6cd3C67E02EC08013CEBb29b1241f3De5](https://etherscan.io/address/0xc7fb91b6cd3c67e02ec08013cebb29b1241f3de5)
- GDA_CURVE_V2: [0x1fD5876d4A3860Eb0159055a3b7Cb79fdFFf6B67](https://etherscan.io/address/0x1fd5876d4a3860eb0159055a3b7cb79fdfff6b67)

## How Bonding Curves Work

Bonding curves use pair state to determine the appropriate buy or sell price for a given number of NFTs. Specifically, they use a pair's `spotPrice` and `delta` to calculate net pricing (exclusive of fees) for `numItems` NFTs.

Curves also calculate the sudoswap protocol fee and, for `TRADE` pools, the LP's chosen fee/spread.

The relevant methods for this, `getBuyInfo` and `getSellInfo`, are defined in the [`ICurve` interface](https://github.com/sudoswap/lssvm2/blob/main/src/bonding-curves/ICurve.sol). Besides the name, the interface for both methods is identical:

``` sol
    function getBuyInfo(
        uint128 spotPrice,
        uint128 delta,
        uint256 numItems,
        uint256 feeMultiplier,
        uint256 protocolFeeMultiplier
    )
        external
        view
        returns (
            CurveErrorCodes.Error error,
            uint128 newSpotPrice,
            uint128 newDelta,
            uint256 inputValue,
            uint256 tradeFee,
            uint256 protocolFee
        );
```

In addition to returning the net `inputValue` for a transaction and the relevant `tradeFee` and `protocolFee`, these bonding curve methods also return a `newSpotPrice` and `newDelta` which are used to adjust the pair's pricing.

Note that bonding curves are intended to be pure: they do not modify pair state directly. Instead, pairs update their state themselves based on these return values.

### Interpreting `spotPrice` and `delta`

In the original sudoswap bonding curves – the linear and exponential curves – `spotPrice` is the instantaneous price received when *selling* 1 NFT *to* the pair. For these curves, the instantaneous price paid when *buying* 1 NFT *from* the pair is the `spotPrice` adjusted upwards by 1 unit of `delta` (additively or multiplicatively).

However, non-standard bonding curves like the XYK and GDA curves use pair state in different ways to enable unique pricing mechanisms. For these curves, `spotPrice` does not represent the price received when selling an NFT to the pair, and `delta` does not represent an additive or multiplcative modifier. As a result, users should use the `getBuyNFTQuote` and `getSellNFTQuote` methods on pairs themselves to get accurate pricing info.

### Pricing For Multi-Swaps

If a user buys or sells multiple NFTs in one transaction, the price for each NFT will be updated according to the bonding curve. For linear and exponential curves, this means the the `spotPrice` will update by `delta` (additively or multiplicatively) for each NFT bought or sold.

For example, consider a pair with the linear bonding curve, with a `spotPrice` of 1 ETH and a `delta` of 0.1 ETH. If a user sells 5 NFTs to this pair, they will receive:

* 1 ETH for the first NFT
* 0.9 ETH for the second NFT
* 0.8 ETH for the third NFT
* 0.7 ETH for the fourth NFT
* 0.6 ETH for the fifth NFT

At the end of the multi-swap, the new `spotPrice` will be set to 0.5 ETH.

For non-standard bonding curves, calculating the price of multi-swaps may be more complex; nevertheless, the price of each subsequent NFT is also adjusted according to the curve logic.

## Overview of Bonding Curves

Here is an overview of how pair state is used to calculate pricing for each of the whitelisted bonding curves:

### Linear Curve

The linear curve uses `spotPrice` to store the price received when selling an NFT to the pair. Following each transaction, the curve performs an additive operation of magnitude `delta` to update the price.

For example, if a user buys an NFT from the pair, the `spotPrice` will be incremented by `delta`, and vice-versa.

`delta` is assumed to be set properly by the LP to be the same precision of the pair's underlying token.

### Exponential Curve

The exponential curve also uses `spotPrice` to store the price received when selling an NFT to the pair. Following each transaction, the curve performs a multiplicative operation of magnitude `delta` to update the price.

For example, if a user buys an NFT from the pair, the `spotPrice` will be multiplied by `delta`, and vice-versa.

`delta` is a multiplier assuming a fixed point system where `1e18` is 1. For example, if `delta` is `1e18 + 1e17`, this represents a 10% change in price each time.

### XYK Curve

The XYK curve uses `spotPrice` and `delta` in a non-standard way: to store two virtual reserves. Following each transaction, price adjusts such that the product (multiplication) of the virtual reserves remains constant after every trade.

At pool creation, the reserves are set as follows:

* `nftBalance` (stored as `delta`): the number of NFTs being bought or sold (in the case of trade pools, whichever is greater) **plus one**
* `tokenBalance` (stored as `spotPrice`): the number of NFTs being bought or sold (or whichever is greater) multiplied by the start price

The net price exclusive of fees for a user to buy *x* NFTs is `inputValueWithoutFee = (x * tokenBalance) / (nftBalance - x)`. Conversely, the net price for a user to sell *x* NFTs is `outputValueWithoutFee = (x * tokenBalance) / (nftBalance + x)`.

Immediately after every trade, the pair state is updated:

* Pair sells: `delta = delta - x` and `spotPrice = spotPrice + outputValueWithoutFee`
* Pair buys: `delta = delta + x` and `spotPrice = spotPrice - outputValueWithoutFee`

### GDA Curve

The GDA curve also uses `spotPrice` and `delta` in a non-standard way. It is an implementation of the [discrete gradual Dutch auction](https://www.paradigm.xyz/2022/04/gda#discrete-gda), where `spotPrice` is the price of the currently "auctioned" NFT *irrespective* of time decay, and `delta` stores the auction parameters `alpha` and `lamba`, as well as the timestamp of the last trade `prevTime`.

In contrast to the specification linked above, the GDA curve calculates the price of each item as a function of the last. The net price exclusive of fees for a user to buy *x* NFTs is `inputValue_ = (spotPrice * (alpha^x - 1)) / ((alpha - 1) * 2^(lambda * timeElapsed))` where `timeElapsed` is the time elapsed since the last NFT was sold.

Conversely, the net price for a user to sell *x* NFTs is `outputValue_ = (spotPrice * 2^(lambda * timeElapsed) * (alpha^x - 1)) / (alpha^(x - 1) * (alpha - 1))` where `timeElapsed` is the time elapsed since the last NFT was sold.

Immediately after every trade, the pair state is updated:

* Pair sells: `spotPrice = (spotPrice * alpha^x) / 2^(lambda * timeElapsed)`
* Pair buys: `spotPrice = (spotPrice * 2^(lambda * timeElapsed)) / alpha^x`

In both cases, `delta` is also updated to reflect the current `block.timestamp`.

#### Parsing `delta`

The GDA curve stores three values in `delta`. They are the auction parameters `alpha` and `lamba`, as well as the timestamp of the last trade `prevTime`.

Since `delta` is of type `uint128`, this is achieved as follows:

* The highest 40 bits represent `alpha` with 9 decimals of precision.
* The middle 40 bits represent `lambda` with 9 decimals of precision.
* The lowest 48 bits represent the timestamp of the last trade `prevTime`.

For a given auction, `alpha` and `lambda` are constants, while `prevTime` (and thus `delta` as a whole) must be updated immediately after every trade.