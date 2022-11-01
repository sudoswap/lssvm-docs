# What Is Sudoswap?

The sudoswap AMM – or just *sudoswap* for short – is a minimal, gas-efficient automated market maker (AMM) protocol that facilitates NFT-to-token swaps (and vice versa) using customizable bonding curves. sudoswap supports ERC721 NFTs, as well as all ETH and ERC20 tokens.

Liquidity providers (LPs) can deposit assets into single-sided buy or sell pools, or into dual-sided trade pools which buy *and* sell NFTs with an optional spread to capture trading fees. 

Similar to other floor NFT protocols, sudoswap makes no distinction between different ERC721 IDs. Pools that are willing to buy or sell NFTs will return the same price no matter which NFT is sent in or out from the collection.

## How Does Sudoswap Work?

Sudoswap is an AMM protocol for NFTs, which means that users buy from or sell into liquidity pools instead of directly trading between themselves. If you're familiar with Uniswap, it's a similar concept but for NFTs.

Here's how it works:
1. Liquidity providers deposit NFTs and/or ETH (or an ERC20 token) into liquidity pools. They choose whether they would like to buy or sell NFTs (or both) and specify a starting price and bonding curve parameters.
2. Users can then buy NFTs from or sell NFTs into these pools. Every time an item is bought or sold, the price to buy or sell another item changes for the pool based on its bonding curve.
3. At any time, liqudity providers can change the parameters of their pool or withdraw assets.

## What Is A Liquidity Pool?

A pool, or liquidity pool, is a smart contract that allows you to instantly swap between two assets. On sudoswap, the most common type of pool is an NFT<>ETH pool, which means that anyone holding NFTs from that collection can instantly swap them for ETH, or vice versa.

Pools use a bonding curve to determine the relative price at which one asset is traded for another. The more an asset is bought from the pool, the more expensive it becomes. Conversely, the more an asset is sold to the pool, the cheaper it becomes.

Ideally, a pool contains some amount of both assets, enabling users to swap back and forth between them. However, it's also possible to create a pool with just one asset, meaning that users will only be able to buy that asset from the pool.

A bonding curve is a mathematical formula which defines the relationship between an asset's price and its supply. Bonding curves are a key feature of automated market makers since they are used to algorithmically adjust asset prices.

sudoswap supports three types of bonding curve: linear, exponential, and XYK (constant product).

### Linear

With a linear bonding curve, the price of an NFT is increased by a flat amount (called `delta`) every time an item is bought from the pool. Conversely, the price of the NFT is decreased by that same flat amount every time an item is sold to the pool.

For example, a liqudity provider may create an NFT<>ETH pool with a `Start Price` of 1 ETH and a `delta` of 0.1 ETH. Assuming they provide enough liquidity, the price of an NFT will increase to 1.1 ETH after one item is purchased from the pool. After a second item is purchased, the price will increase to 1.2 ETH, and so on and so forth. At any point, if an NFT is sold to the pool, the price will decrease by 0.1 ETH.

### Exponential

With an exponential bonding curve, the price of an NFT is increased by a certain percentage (also called `delta`) every time an item is bought from the pool. Conversely, the price of the NFT is decreased equivalently every time an item is sold to the pool.

To calculate the equivalent decrease, convert the the percentage to a decimal index (e.g. for 50%, the index would be 1.5) and divide the price by this number.

For example, a liquidity provider may create an NFT<>ETH pool with a `Start Price` of 2 ETH and a `delta` of 50%. Assuming they provide enough liquidity, the price of an NFT will increase to 2 + 50% = 3 ETH after one item is purchased from the pool. After a second item is purchased, the price will increase to 3 + 50% = 4.5 ETH, and so on and so forth. At any point, if an NFT is sold to the pool, the price will be divided by 1.5.

### XYK (Constant Product)

With an XYK curve, the price of an NFT is adjusted every time an item is bought from or sold to the pool, such that the product of two virtual reserves remains constant after every trade. These virtual reserves correspond to the number and value of NFTs the pool will buy or sell.

An additional concentration parameter allows liquidity providers to adjust (i.e. tighten or loosen) XYK curves.

For information on how exactly pricing is calculated for XYK curves, refer to the [Pricing](https://docs.sudoswap.xyz/reference/pricing/) page in the technical reference.