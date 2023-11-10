# Frequently Asked Questions

This page contains some of the most frequently asked questions among sudoswap users.

### How are fees calculated?

Fees are calculated as a percentage of the net buy or sell price, after bonding curve logic has been applied to the spot price. Prices shown on the sudoswap frontend are the *final* amounts that a user pays (when buying an NFT) or receives (when selling an NFT) after fees have been accounted for.

**Example:**

- Spot Price: 0.9 ETH
- Delta: 0.1 ETH
- Net Buy Price: 1.0 ETH
- 5% Creator Royalty: 0.05 ETH
- 5% Trade Fee: 0.05 ETH
- 0.5% Protocol Fee: 0.005 ETH
- Buyer Pays: 1.105 ETH

### Why did I receive no trade fee / double the trade fee?

Users are always charged the exact trade fee when buying or selling an NFT. However, to optimize for gas usage, the trade fee remains in the pool (instead of being sent to the fee recipient) when an NFT is sold into a pool.

To compensate for this, double the trade fee is sent to the fee recipient when an NFT is bought from a pool, ensuring fees accrue to the fee recipient in the long term. This amount consists of the fee itself (as paid by the user) and an equal amount deducted from the proceeds of the trade, which would otherwise remain in the pool.

### Can I withdraw airdropped tokens from my pool?

Yes. At any time, you can withdraw any ERC20, ERC721, or ERC1155 tokens from your liqudity pools using the "Withdraw Other Tokens" feature on the pool's page. Note that you can only withdraw tokens from pools you have created.

### What's the difference between selling and listing?

When you *sell* an NFT on sudoswap, it is immediately sold into a [bonding curve](https://docs.sudoswap.xyz/pricing/) for the best price possible. This means that you receive the proceeds of the sale right away.

When you *list* an NFT on sudoswap, you can set whatever price you want, but you have to wait until someone buys it from you.

### Do I get a token representing my liquidity position?

No, you will not receive a token representing your liquidity position when creating a new pool. This is because every liquidity pool has a unique smart contract associated with and owned by its creator.

All assets pertaining to a pool are held in the pool's smart contract, so there is no need for additional tokens to keep track of deposits.

### Why is my new liquidity pool not showing up on the sudoswap website?

Sudoswap indexes on-chain data at regular intervals. If a new liquidity pool is not showing up, it is usually because sudoswap has not yet had a chance to index the pool's creation. Please check again at a later time.

### What happens to a trade pool when one side of the pool is depleted?

Trade pools remain trade pools even if one side of the pool is depleted. Nevertheless, pools can only fulfill buys and sells if they hold the necessary assets to do so.