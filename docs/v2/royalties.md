# Royalties

With sudoAMM v2, royalties are enforced on liquidity pools at the contract level. Royalties are automatically enabled for collections implementing the [ERC2981 royalty standard](https://eips.ethereum.org/EIPS/eip-2981) as well as collections created with supported marketplaces such as Manifold/Foundation, Rarible, SuperRare, or Zora (limited functionality).

Additionally, creators can use the shared [Royalty Registry](https://royaltyregistry.xyz/) deployed at royaltyregistry.eth to manually override royalties, e.g. for older collections which do not implement ERC2981.

## Royalty Engine

The `RoyaltyEngine` contract determines the provisional value of royalties due for a swap of given `value` and the address that should receive them. It is a fork of the [Royalty Engine](https://github.com/manifoldxyz/royalty-registry-solidity/blob/main/contracts/RoyaltyEngineV1.sol) created by Manifold as part of the Royalty Registry initiative.

If a collection implements ERC2981, the `RoyaltyEngine` retrieves royalty information from the collection contract itself. Failing this, the `RoyaltyEngine` searches for royalty information against popular marketplace specifications:

* Manifold
* Rarible V1 and V2
* Foundation
* SuperRare
* Zora
* ArtBlocks
* KnownOriginV2

The `RoyaltyEngine` also searches for entries in the shared Royalty Registry at royaltyregistry.eth, where creators can manually override royalties.

Royalty information returned from the engine is used in the `_calculateRoyalties` function on pair contracts, which is called internally by all swap methods, unless:

* The royalty exceeds the sale price, in which case no royalty is applied
* A Setting is in place for the pool...

## Settings

When a [Setting](settings.md) is in place for a given pair, the `_calculateRoyalties` function will overwrite the royalty *value* returned by the `RoyaltyEngine` using the `getFeeSplitBps` view on the Setting contract.