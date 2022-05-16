# Bonding Curves and Pricing

To determine pricing, each `LSSVMPair` is associated with a specific bonding curve set by the LP. At present, there are two choices: `LinearCurve` and `ExponentialCurve`. Both curves are parameterized by one variable, `delta`, which is set in the pair itself. More bonding curve contracts can be whitelisted in the future for use with `LSSVMPairFactory`.

After a user trades with a pair, the pair consults its bonding curve to determine what its new price should be. 

Technical note: the bonding curves are intended to be pure, i.e. they do not modify the state of pairs that call them. The actual logic for input/output validation and price updating happens in the `LSSVMPair` contract itself. 

### Linear Curve
The linear curve performs an additive operation to update the price. `delta` is assumed to be set properly by the LP to be the same precision of the pair's underlying token.

If the pair has just sold an NFT by giving out an NFT and receiving tokens, the next price it will quote to sell NFTs at will be `delta` more. Conversely, if the pair has just bought an NFT by giving out tokens and receiving an NFT, the next price it will quote to purchase NFTs at will be `delta` less. 

### Exponential Curve
The exponential curve performs a multiplicative operation. `delta` is treated as a multiplier assuming a fixed point system where `1e18` is 1.

For example, if `delta` is `1e18 + 1e17`, this represents a 10% change in price each time.

If the pair has just sold an NFT by giving out an NFT and receiving tokens, the next price it will quote to sell NFTs at will be multiplicatively `delta` more. Conversely, if the pair has just bought an NFT by giving out tokens and receiving an NFT, the next price it will quote to purchase NFTs at will be multiplicatively `delta` less. 

## Understanding Spot Price
In addition to modifying `delta` to change the pair's price reactivity, it is important to understand how the `spotPrice` variable in `LSSVMPair` behaves.

The spot price refers to the instantaneous price of *selling* 1 NFT *to* the pair. The instantaneous price of *buying* 1 NFT *from* the pair is set to be the `spotPrice` adjusted upwards by 1 unit of `delta` with respect to the pair's bonding curve.

For example, say that we have a Trade `LSSVMPair` for ETH  with a `spotPrice` of 1 ETH, and a linear curve with a `delta` of 0.1 ETH. (Assume `fee` is 0.) Then a user selling 1 NFT to the pair would receive 1 ETH, whereas a user purchasing 1 NFT from the pair would have to send (1 + 0.1) = 1.1 ETH.

In other words, the price to purchase an NFT from a pair will always be `delta` greater (either additively on multiplicatively) than the price to sell an NFT to the pair.

Pair managers who are manually adjusting `spotPrice` should keep this in mind.

(Note that for simplicity, some examples in this documentation may use "spot price" to also refer to the instantaneous buy price.)

## Pricing For Multi-Swaps
If a user buys or sells multiple NFTs in one swap transaction, the `spotPrice` will update by `delta` for each NFT bought or sold. 

For example, say that we have a Trade `LSSVMPair` for ETH  with a `spotPrice` of 1 ETH, and a linear curve with a `delta` of 0.1 ETH. (Assume `fee` is 0.)

If a user sells 5 NFTs to this pair, they will receive:

* 1 ETH for the first NFT
* 0.9 ETH for the second NFT
* 0.8 ETH for the third NFT
* 0.7 ETH for the fourth NFT
* 0.6 ETH for the fifth NFT

At the end of the multi-swap, the new `spotPrice` will be set to 0.5 ETH.