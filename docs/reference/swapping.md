# Swapping Across Pairs 

The recommended way to make swaps across various pairs is to use the `LSSVMRouter`. Users can set allowances for tokens and NFTs once for the router, instead of for each new pool they wish to swap with.

On the protocol level, the sudoswap AMM does not perform any routing optimization on-chain. Users are expected to know their desired swap paths when calling the router, e.g. via the use of an off-chain indexing service.

## NFT<>Token Swaps

When swapping tokens for NFTs, users can either specify which NFT IDs they want from each pair, or they can ask for any ID from the pair.

When swapping from NFT to tokens or from tokens to NFTs, the `LSSVMRouter` has two types of swaps: a Normal Swap and a Robust Swap.

### Normal Swap
A Normal Swap is conceptually similar to token-to-token swaps on other DEXs. The user sends the router a maximum input amount or minimum output amount (i.e. allowed slippage), as well as a swap route and a deadline. The router will then swap across the various pairs specified. At the end of all the swaps, the router will total all tokens to be received or sent, and revert if the total is beyond the user's specified slippage.

### Robust Swap
In contrast, a Robust Swap does a slippage check *per* swap pair rather than an *aggregate* check at the end. If the price for a specified swap pair is past the allowable slippage, the router will silently skip that route and move on to the next one, with no reverts or errors.

This is intended for situations where the price can move quickly between your transaction submission and execution. 

### Normal vs Robust Swap
To see the difference between these swap types, consider this example: say there are 2 pairs for NFT and ETH. The first pair has a spot price of 1 ETH and a linear curve with a delta of 0.1 ETH. The second pair has a spot price of 1 ETH and a linear curve with a delta of 1 ETH.

A user wishes to purchase NFTs, one from each pool for 1 ETH each, with 10% slippage. 

The user submits a swap transaction. Before this transaction is executed, someone else goes and buys 1 NFT from each pool for 1 ETH each.

The new spot price for the first pair is 1.1 ETH, and for the second pair is 2 ETH.

If the user had submitted a Normal Swap, they would have sent 2.2 ETH (2 ETH + 0.2 ETH to cover the additional 10% slippage), and the transaction would fail, as they would have sent enough ETH to cover the first swap at the new price of 1.1 ETH, but not enough to cover the second swap at 2 ETH.

In contrast, if the user had submitted a Robust Swap, they would have also sent 2.2 ETH, but with a *per-swap* max cost of 1.1 ETH. The router would have enough to cover the first swap at 1.1 ETH. Then, upon seeing the second swap would cost 2 ETH, the router would skip this pair entirely. The transaction would succeed, refunding 1.1 ETH back to the user, as well as sending them the one NFT they purchased for 1.1 ETH.

Thus, the Robust Swap is generally recommended for a better user experience in higher volatility environments, at the cost of slightly more gas.

## NFT<>NFT Swaps
As another added convenience, the `LSSVMRouter` also supports swapping NFTs for tokens, and then tokens to other NFTs in one transaction.

Like the token to NFT swap, users can either specify specific NFT IDs from a pair, or ask for any NFT ID.