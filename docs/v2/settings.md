# Settings

New in sudoAMM v2, Settings are opt-in contracts that allow creators to waive (partially or entirely) royalties for liquidity pools that meet their chosen criteria, such as a 90-day lockup period or 50:50 trading fee split.

Creators can develop Settings using the logic of their choosing, but the resulting contract should conform to the `ISettings.sol` interface:

``` sol
interface ISettings {
    struct PairInfo {
        address prevOwner;
        uint96 unlockTime;
        address prevFeeRecipient;
    }

    function getFeeSplitBps() external pure returns (uint64);

    function getRoyaltyInfo(address pairAddress) external view returns (bool, uint96);

    function settingsFeeRecipient() external returns (address payable);

    function getPrevFeeRecipientForPair(address pairAddress) external returns (address);
}

```

Once a Setting has been deployed, the creator of the relevant NFT collection should call `toggleSettingsForCollection` on the Pair Factory to ratify the Setting, thus waiving royalties for pools enrolled in that Settings.

To enroll in a Setting, liquidity providers transfer ownership of their pool to the Setting contract using the `transferOwnership` method on the pool contract. Creators should design Settings such that only pools which meet the desired criteria can be transferred to the Setting contract.

## Standard Settings

A Standard Setting is a standardized type of Setting with three criteria:

* `ethCost`, an upfront fee to the liquidity provider in gwei
* `secDuration`, a lockup period in seconds
* `feeSplitBps`, a trading fee split in basis points

Any liquidity provider who accepts these criteria can participate in the Setting. Following the lockup period, the liquidity provider can call `reclaimPair` on the Setting contract to withdraw their pool from the Setting.

Creators can deploy Standard Settings without writing any code by using the `createSettings` method on the `StandardSettingsFactory`. In addition to the criteria above, this method takes two parameters:

* `settingsFeeRecipient`, the address which will receive the creator's trading fee split
* `royaltyBps`, the waived royalty rate in basis points

### Fee Splitter

When ownership of a liquidity pool is transferred to a Standard Settings contract, a fee splitter (`Splitter.sol`) is deployed to handle the distribution of trading fees between the liqudity provider and creator.

Any address or contract can call the withdrawal methods on the Splitter, such as `withdrawETH`, to atomically withdraw all accrued fees and split them between the liqudity provider and creator per the `feeSplitBps`.

Additionally, Setting owners can call `bulkWithdrawFees` on a Setting to trigger fee withdrawal from all Fee Splitters deployed under that Setting.