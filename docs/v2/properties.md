# Properties

sudoAMM v2 introduces the ability to create property-specific liquidity pools. Properties are enforced at the `LSSVMPair` level on buy methods, meaning that only NFTs with the specified properties can be sold into pools.

Property checking logic is contained within dedicated contracts called Property Checkers. Users can develop Property Checkers using the logic of their choosing, but the resulting contracts should conform to the `IPropertyChecker.sol` interface expected by pair contracts:

``` sol
interface IPropertyChecker {
    function hasProperties(uint256[] calldata ids, bytes calldata params) external returns(bool);
}
```

The interface's single method `hasProperties` takes an array of tokenIds and optional bytes calldata, returning true or false if the specified tokenIds meet the Property Checker's criteria. The optional bytes calldata can be used to pass additional information to the Property Checker, such as proof-of-inclusion in a Merkle tree.

In order to leverage property checking, liquidity providers must specify a Property Checker when creating pools with the Pair Factory. The Property Checker that is specified for a pool at creation cannot be changed or removed.

## Default Property Checkers

sudoAMM v2 provides two default Property Checkers which can be cloned by users with custom parameters. For instructions on doing so, see [Cloning Property Checkers](../reference/property-checkers.md).

### RangePropertyChecker

`RangePropertyChecker` checks whether tokenIds passed to the `hasProperties` method lie within a `lowerBound` and `upperBound` specified when the contract was cloned.

For this Property Checker, the optional bytes calldata is not used.

### MerklePropertyChecker

`MerklePropertyChecker` checks whether tokenIds passed to the `hasProperties` method are leaves in an arbitrary Merkle tree whose root was specified when the contract was cloned.

For this Property Checker, the optional bytes calldata is used by the client to pass a Merkle proof.