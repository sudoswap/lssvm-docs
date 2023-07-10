# Deploying Property Checkers

The default Property Checkers introduced in sudoswap V2 can be cloned using the Property Checker Factory at [0x031b216FaBec82310FEa3426b33455609b99AfC1](https://etherscan.io/address/0x031b216fabec82310fea3426b33455609b99afc1). The contract has two write methods, one for deploying each type of Property Checker.

Both types of default Property Checker conform to the `IPropertyChecker.sol` interface expected by pair contracts:

``` sol
interface IPropertyChecker {
    function hasProperties(uint256[] calldata ids, bytes calldata params) external returns(bool);
}
```

## RangePropertyChecker

A `RangePropertyChecker` can be deployed using the `createRangePropertyChecker` method. The method's two parameters `startInclusive` and `endInclusive` are tokenIds for which all tokenIds in [`startInclusive`, `endInclusive`] will be considered valid by the Property Checker.

When interacting with a `RangePropertyChecker`, the optional bytes calldata parameter on the `hasProperties` method is not used.

## MerklePropertyChecker

A `MerklePropertyChecker` can be deployed using the `createMerklePropertyChecker` method. The method's one parameter is the  root of the Merkle tree of all tokenIds which should be considered valid by the Property Checker.

When interacting with a `MerklePropertyChecker`, the bytes calldata parameter on the `hasProperties` method is used to pass an array of Merkle proofs for the tokenIds being verified. Individual proofs should be of type `bytes32[]` and should be encoded together as a single `bytes[]`.

When interacting with a pair that implements a `MerklePropertyChecker`, traders must have access to Merkle proofs for relevant tokenIds through a frontend interface or otherwise.

### Merkle Tree Construction

`MerklePropertyChecker` implements OpenZeppelin's `MerkleProof.sol` contract to verify proofs. The hashing function used for both leaf and branch nodes is `keccak256(abi.encodePacked())`.

To generate a compatible Merkle tree, you can use a JavaScript library like [@openzeppelin/merkle-tree](https://github.com/OpenZeppelin/merkle-tree). For this specific library, you must adjust the hashing function in `standard.ts` to match what is provided above.

```
function standardLeafHash<T extends any[]>(value: T, types: string[]): Bytes {
  return keccak256(hexToBytes(defaultAbiCoder.encode(types, value)));
}
```

#### Security Note

OpenZeppelin advises against constructing Merkle trees with simple hashing functions like `keccak256(abi.encodePacked())` due to the risk of [second pre-image attacks](https://flawed.net.nz/2018/02/21/attacking-merkle-trees-with-a-second-preimage-attack/). However, `MerklePropertyChecker` is resistant to this type of attack because it is not possible to pass two concatenated `keccak256` hashes (totalling 64 bytes in length) as a `uint256` tokenId. Not to mention, most NFT contracts do not allow minting of specific tokenIds.
