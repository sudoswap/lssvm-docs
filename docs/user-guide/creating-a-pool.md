# Creating a Pool

Anyone can provide liquidity on sudoswap. You can choose whether you want to buy or sell NFTs across a range of prices, or do both to earn trading fees.

To create a pool, navigate to the [Create Pool](https://sudoswap.xyz/#/create) interface. Follow the steps below depending on the type of pool you want to create.

![](https://i.imgur.com/izZ9axI.jpg)

## Trade Pool (Buy and Sell)

1. On the [Create Pool](https://sudoswap.xyz/#/create) interface, select an NFT collection in the top-left pane.
2. Optionally, choose an ERC20 token the NFTs should trade against. By default, NFTs trade against ETH: ![](https://i.imgur.com/00maWlD.jpg)
3. Under the *Deposits* section, choose how much ETH/ERC20 token to deposit (based on how many NFTs you are willing to buy) and which of your NFTs to deposit to seed the pool: ![](https://i.imgur.com/4pN2MCA.jpg)
4. Under the *Strategy* section, choose a starting Sell price for the pool. The corresponding Buy price is automatically adjusted in the following steps.
5. Adjust the percentage Fee which will be taken on every trade.
6. Select a type of price curve from the dropdown box and adjust the curve parameter as desired. *For details about the price curves supported by sudoswap and their adjustable parameter(s), see [Bonding Curves](/docs/index.md#bonding-curves).* ![](https://i.imgur.com/GeVtrcT.jpg)
7. Click "Create Pool" and confirm the approval and pool creation transactions in your wallet: ![](https://i.imgur.com/M54MPDk.jpg)

## Buy-Only Pool

1. On the [Create Pool](https://sudoswap.xyz/#/create) interface, select the Buy pool type in the top right-pane: ![](https://i.imgur.com/Qjczvqn.jpg)
2. Follow the same steps you would to create a Trade pool, as documented above. Notice that:
    - Under the *Deposits* section, you only choose how much ETH/ERC20 token to deposit (based on how many NFTs you are willing to buy) to seed the pool.
    - Under the *Strategy* section, the starting price you choose is a Buy price.

## Sell-Only Pool

1. On the [Create Pool](https://sudoswap.xyz/#/create) interface, select the Sell pool type in the top right-pane: ![](https://i.imgur.com/jnvVNF3.jpg)
2. Follow the same steps you would to create a Trade pool, as documented above. Notice that:
    - Under the *Deposits* section, you only choose which of your NFTs to deposit to seed the pool.
    - Under the *Strategy* section, the starting price you choose is a Sell price.

## Managing Existing Pools

Once you've created a pool, you may wish to update its pricing and deposit or withdraw assets. To edit a pool, click on your address at the top-right of any page, click on [My Pools](https://sudoswap.xyz/#/pool), and select the pool.

![](https://i.imgur.com/UzxHXRM.jpg)

### Updating Pricing

1. Navigate to the [My Pools](https://sudoswap.xyz/#/pool) page.
2. Click on the pool you want to change.
3. Click on the "Edit Pool" button at the top-right of the page.
4. Enter a new start price, delta, and or swap fee (only for Trade pools) for the pool: ![](https://i.imgur.com/UD0djFb.jpg)
5. Click "Update" and confirm the transaction in your wallet.

*Note: It is not possible to change a pool's type (linear, exponential, etc). Instead, withdraw your assets from the existing pool and create a new one.*

### Depositing and Withdrawing

1. Navigate to the [My Pools](https://sudoswap.xyz/#/pool) page.
2. Click on the pool you want to deposit to or withdraw from.
3. Click on the Deposit or Withdraw button in the Tokens or NFTs pane.
    - For tokens, simply enter an amount and click the Deposit or Withdraw button: ![](https://i.imgur.com/EgplCSc.jpg)
    - For NFTs, select the NFTs you wish to deposit or withdraw and click the Deposit or Withdraw button: ![](https://i.imgur.com/co01Dkg.jpg)