# User Guide

This guide demonstrates how to buy and sell NFTs on sudoswap, as well as how to create and modify your own liquidity pools.

## Buying NFTs

You can instantly buy NFTs from a bonding curve or make a collection offer for a seller to accept.

### How do you buy an NFT?

1. Go to the collections page and select a collection.
2. Select the NFTs you want to buy to add them to the cart:

![](https://i.imgur.com/zV0sCdB.jpg)

3. Click "> sudo swap" to initiate the swap in your wallet.

### How do you make a collection offer?

1. Go to the collections page and select a collection.
2. Click "Make Collection Offer" to open the offer manager window:

![](https://i.imgur.com/A64xnKN.jpg)

3. Enter the start price

## Selling NFTs

You can instantly sell NFTs to a bonding curve or make a listing for a buyer to take.

### How do you sell an NFT?

1. Go to the Collections page and select a collection.
2. Navigate to the "Sell" tab at the center of the page:

![](https://i.imgur.com/ZNUXQGm.jpg)

3. Select the NFTs you want to sell and click "> sudo swap".
4. Give sudoswap access to the NFTs by confirming the first transaction in your wallet.
5. Finalize the sale by confirming the second transaction in your wallet.

### How do you list an NFT?

1. Navigate to the "Your NFTs" page on the top right.
2. Select the NFTs you want to list and click "List NFTs" at the bottom of the page:

![](https://i.imgur.com/J2HuZtz.jpg)

4. Under "Start Price", enter your desired price. Optionally, if listing multiple NFTs, enter a "Price Increase" by which the price of all remaining items will be incremented each time an item is sold:

![](https://i.imgur.com/lSxSUWC.jpg)

4. Click "List NFTs".
5. Give sudoswap access to the NFTs by confirming the first transaction in your wallet.
6. Finalize the listing by confirming the second transaction in your wallet.

### What's the difference between selling and listing?

When you *sell* an NFT on sudoswap, it is immediately sold into a [bonding curve](https://docs.sudoswap.xyz/pricing/) for the best price possible. This means that you receive the proceeds of the sale right away.

When you *list* an NFT on sudoswap, you can set whatever price you want, but you have to wait until someone buys it from you.

If you want to get instant liquidity for your NFTs, you should consider selling them. However, because of the way bonding curves work, you'll some experience slippage, meaning that you get slightly less than market value for your NFTs.

Slippage gets worse the more NFTs you sell from a collection, or the lower that collection's liqudity. If you aren't satisfied with the prices you can get for selling, you should list the NFTs and wait for someone to buy them.

## Providing liquidity

Anyone can provide liquidity on sudoswap. You can choose whether you want to buy or sell NFTs across a range of prices – a bonding curve – or do both to earn trading fees.

### What is a pool?

A pool, or liquidity pool, is a smart contract that allows you to instantly swap between two assets. On sudoswap, the most common type of pool is an NFT<>ETH pool, which means that anyone holding NFTs from that collection can instantly swap them for ETH, or vice versa.

Pools use a bonding curve to determine the relative price at which one asset is traded for another. The more an asset is bought from the pool, the more expensive it becomes. Conversely, the more an asset is sold to the pool, the cheaper it becomes.

Ideally, a pool contains some amount of both assets, enabling users to swap back and forth between them. However, it's also possible to create a  pool with just one asset, meaning that users will only be able to buy that asset from the pool.

### Types of bonding curve

Sudoswap allows liqudity providers to choose from two types of bonding curve: linear and exponential curves. Depending on which type of bonding curve a pool uses, the price will be adjusted differently when an asset is bought from or sold to the pool.

#### Linear curve

With a linear bonding curve, the price of an NFT is increased by a flat amount (called `delta`) every time an item is bought from the pool. Conversely, the price of the NFT is decreased by that same flat amount every time an item is sold to the pool.

For example, a liqudity provider may create an NFT<>ETH pool with a `Start Price` of 1 ETH and a `delta` of 0.1 ETH. Assuming they provide enough liquidity, the price of an NFT will increase to 1.1 ETH after one item is purchased from the pool. After a second item is purchased, the price will increase to 1.2 ETH, and so on and so forth. At any point, if an NFT is sold to the pool, the price will decrease by 0.1 ETH.

#### Exponential curve

With an exponential bonding curve, the price of an NFT is increased by a certain percentage (also called `delta`) every time an item is bought from the pool. Conversely, the price of the NFT is decreased equivalently every time an item is sold to the pool.

To calculate the equivalent decrease, convert the the percentage to a decimal index (e.g. for 50%, the index would be 1.5) and divide the price by this number.

For example, a liqudity provider may create an NFT<>ETH pool with a `Start Price` of 2 ETH and a `delta` of 50%. Assuming they provide enough liquidity, the price of an NFT will increase to 2 + 50% = 3 ETH after one item is purchased from the pool. After a second item is purchased, the price will increase to 3 + 50% = 4.5 ETH, and so on and so forth. At any point, if an NFT is sold to the pool, the price will be divided by 1.5.

### How do you create a pool?

To create a pool, start by navigating to the [Your Pools](https://sudoswap.xyz/#/pool) tab at the top-right of the page. Click on "+ Create New Pool" and follow the instructions below depending on the type of pool you want to create.

![](https://i.imgur.com/258FrrF.jpg)

#### Buy NFTs with ETH

1. Click on the deposit drop-down box and select ETH:

![](https://i.imgur.com/6efi1G4.jpg)

2. Click on the receive drop-down box and select the NFT collection you want to buy. If you don't see the collection you want to buy, paste the contract address into the search bar, click "Add", and then select the collection.
3. Enter the start price you want to pay, choose the type of bonding curve, and enter your chosen delta. If you are only buying one item, choose either type of curve and enter any delta:

![](https://i.imgur.com/kisv5Nx.jpg)

4. Type in how many NFTs you want to buy and click "Next Step >".

*Note: The slider on the right hand side is only for visualization purposes and does not affect your pool.* 

5. Confirm the details of your pool are correct, click "Create Pool", and confirm the transaction in your wallet.

#### Sell NFTs for ETH

1. Click on the deposit drop-down box and select the NFT collection you want to sell:

![](https://i.imgur.com/GxS58xM.jpg)

2. Click on the receive drop-down box and select ETH.
3. Enter the start price you want to receive, choose the type of bonding curve, and enter your chosen delta. If you are only selling one item, choose either type of curve and enter any delta:

![](https://i.imgur.com/aJm6vlF.jpg)

4. Type in how many NFTs you want to sell and click "Next Step >".

*Note: The slider on the right hand side is only for visualization purposes and does not affect your pool.* 

5. Confirm the details of your pool are correct and select the NFTs you want to deposit:

![](https://i.imgur.com/cafdBSw.jpg)

7. Click "Approve" and confirm the transaction in your wallet.
8. Click "Create Pool" and confirm the transaction in your wallet.

#### Do both and earn trading fees

1. Click on the first drop-down box and select ETH:

![](https://i.imgur.com/R44JUUv.jpg)

2. Click on the second drop-down box and select the NFT collection you want to deposit.
3. Enter the percentage fee you want to take on every trade that uses your pool.
4. Enter the start price for the pool, choose the type of bonding curve, and enter your chosen delta:

![](https://i.imgur.com/LmxrDEu.jpg)

5. Type in how many NFTs you want to buy and sell and click "Next Step >".

*Note: The sliders on the right hand side are only for visualization purposes and do not affect your pool.* 

6. Confirm the details of your pool are correct and select the NFTs you want to deposit.
7. Click "Approve" and confirm the transaction in your wallet.
8. Click "Create Pool" and confirm the transaction in your wallet.

### How do you update the price your pool offers?

1. Navigate to the [Your Pools](https://sudoswap.xyz/#/pool) tab at the top-right of the page"

![](https://i.imgur.com/E5O9cv8.jpg)

3. Click on the pool you want to change.
4. Click on the "Edit" button at the top-right of the pool:

![](https://i.imgur.com/yjSBGmv.jpg)

4. Enter a new start price and delta for the pool:

![](https://i.imgur.com/8mczlxE.jpg)

7. Click "Update" and confirm the transaction in your wallet.

*Note: At this time it is not possible to convert a linear pool into an exponential pool or vice versa. To do this, you must withdraw your assets from the existing pool and create a new one.*

### How do you deposit NFTs into a pool?

1. Navigate to the [Your Pools](https://sudoswap.xyz/#/pool) tab at the top-right of the page.
2. Click on the pool you want to deposit to.
3. Click on the "Deposit" button at the top-left of the pool.
4. Select the NFTs you want to deposit:

![](https://i.imgur.com/AvlHDZD.jpg)

6. Click "Deposit NFTs" and confirm the transaction in your wallet.

### How do you withdraw NFTs from a pool?

1. Navigate to the [Your Pools](https://sudoswap.xyz/#/pool) tab at the top-right of the page.
2. Click on the pool you want to withdrawm from.
3. Click on the "Withdraw" button at the top-left of the pool.
4. Select the NFTs you want to withdraw:

![](https://i.imgur.com/ERzFkof.jpg)

6. Click "Withdraw NFTs" and confirm the transaction in your wallet.

### Do I get a token representing my liquidity position?

No, you will not receive a token representing your liquidity position when creating a new pool. This is because every liquidity pool has a unique smart contract associated with and owned by its creator.

All assets pertaining to a pool are held in the pool's smart contract, so there is no need for additional tokens to keep track of deposits.