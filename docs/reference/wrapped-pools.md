# Wrapped Pools

sudoswap pools can be wrapped as ERC721 NFTs using the wrapper contract deployed at [0x3767341a7519178f30281fc7D07538EDb55c05a4](https://etherscan.io/address/0x3767341a7519178f30281fc7D07538EDb55c05a4). This allows pools to be utilized within any NFT-compatible protocol, for purposes such as lending or fractionalization.

At this time, the wrapper contract only supports ERC721<>ETH pools.

## Overview

To wrap a pool, the owner transfers the pool to the wrapper contract using the `transferOwnership` method on the pool contract:

`LSSVMPair.transferOwnership(0x3767341a7519178f30281fc7D07538EDb55c05a4,)`

Then, using the `onOwnershipTransferred` callback, the wrapper contract  mints an ERC721 NFT representing ownership of the wrapped pool to its original owner. The tokenId of the NFT is the contract address of the underlying pool casted to an unsigned integer:

```tokenId = uint256(uint160(pairAddress))```

The holder of a wrapper NFT has full and exclusive control over the underlying pool. Via the wrapper contract, they can:

* change pool settings such as pricing using the `multicall` proxy method,
*  unwrap the pool using the `reclaimPairs` method, which transfers them ownership of the pool and subsequently burns the NFT.

### Example

``` sol
WrappedSudoPool wrapper = WrappedSudoPool(0x3767341a7519178f30281fc7D07538EDb55c05a4);
wrapper.reclaimPairs([LSSVMPair1,...]);
```

## Integrating Wrapped Pools

Since wrapper NFTs conform to ERC721, they are natively compatible with most NFT protocols.

To get information about an underlying pool, start by casting the NFT's tokenId back into the pool's address:

```pairAddress = address(uint160(tokenId))```

Then, read the views inherited from [`LSSVMPair.sol`](https://github.com/sudoswap/lssvm2/blob/main/src/LSSVMPair.sol) on that address to determine the pool's parameters:

* `pairVariant()` (only `ETH` pools are supported by the wrapper)
* `bondingCurve()` (linear, exponential, etc)
* `nft()` (contract address)
* `poolType()` (BUY, SELL, or TRADE)
* and more.

You can read a pool's ETH balance using the `balance` property. To determine the pool's NFT balance, call `balanceOf()` on the NFT contract itself.

### Example

``` sol
import { ILSSVMPair } from "./ILSSVMPair.sol";

contract Example {
  function _handleNFT(address _tokenContract, uint256 _tokenId) internal {
      
      // Check if NFT is a wrapped sudoswap pool
      if (_tokenContract == 0x3767341a7519178f30281fc7D07538EDb55c05a4) {
          
          // Get underlying pool
          ILSSVMPair underlyingPair = ILSSVMPair(address(uint160(_tokenId)));
  
          // Get information about pool
          address nft = underlyingPair.nft(); // Underlying NFT contract
          ILSSVMPair.PoolType type = underlyingPair.poolType(); // BUY, SELL, or TRADE
          address curve = underlyingPair.bondingCurve(); // Address of bonding curve
          uint256 balance = underlyingPair.balance; // ETH balance of pool
  
          // Do desired logic
          ...
      } else {...}
  }
}
```

## Valuing Wrapped Pools

Valuing wrapped pools can be complex due to diverse pricing curves and the risk of manipulation. To mitigate this, we recommend protocols start by only integrating certain categories of sudoswap pools.

For example, for a buy- or sell-only pool using a linear or exponential pricing curve, the total value of the pool can be estimated by multiplying its NFT balance by the NFT's floor price (as provided by an NFT valuation oracle of choice) and adding its ETH balance.

Additionally, you should check the pool's pricing at the time of valuation is equal to the NFT's floor price or better. For a linear or exponential pricing curve, this means:

* Buy-only pools: `spotPrice` ≤ `floor`
* Sell-only pools: `spoPrice` ≥ `floor`