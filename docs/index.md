# sudoswap AMM

The sudoswap AMM is a minimal, gas-efficient AMM protocol for facilitating NFT (ERC721s) to token (ETH or ERC20) swaps using customizable bonding curves. Liquidity providers can deposit into single-sided buy or sell pools, or provide both sides with a spread to capture fees. 

The base unit of the protocol is the `LSSVMPair` which can hold NFTs, tokens, or both. End users then interact with `LSSVMRouter` to swap across multiple pools and manage their approvals on one contract.

Similar to other floor NFT protocols, the current sudoswap AMM protocol makes no distinction between different ERC721 IDs. Pools that are willing to buy or sell NFTs will return the same price no matter which NFT is sent in or out from the collection.
