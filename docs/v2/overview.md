# Overview

sudoAMM v2 introduces four new major features for users and creators:

* [**Royalty support**](royalties.md) for ERC2981-compliant collections, plus fallback royalty solutions
* LP agreements called [**Settings**](settings.md) whereby creators can opt-in to waive royalties for eligible pools
* On-chain [**property filtering**](properties.md) powered by Merkle trees, tokenId ranges, or custom smart contract logic
* [**ERC1155 support**](erc1155.md)

## Minor Changes

Additionally, v2 brings the following tweaks and optimizations, which may be of relevance to builders:

* Fee accounting: When creating a TRADE pool, users can now specify a separate address to receive fees by passing an `_assetRecipient`. If unspecified, fees still accrue to the pair contract.
* *VeryFastRouter*: A new router which handles all swap types (i.e. ERC721<>ETH, ERC721<>ERC20, ERC1155<>ETH, ERC1155<>ERC20), as well as partial fills when buying/selling multiple items from the same pool.
* New swap events: `SwapNFTInPair` and `SwapNFTOutPair` distinguish between the direction of swaps, also logging the input/output value and tokenIds (or number of NFTs in case of ERC1155).
* Gas optimizations: Various minor fixes and gas optimizations have been implemented.