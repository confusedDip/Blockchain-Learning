# Solidity Smart Contract Development 101

The notes are compiled from the Cyfrin Updraft Course for future reference

**Remix IDE:** It is a web-based IDE for Solidity development, a very good starting point to learn about Solidity basics.

- it has built-in solidity compilers, which can compile the `.sol` files we write for our smart contracts
- it has built-in ethereum deployment tool, which can be used to deploy our compiled smart contracts to test/real networks

**Basic MUST DOs for Solidity Smart contracts:**

- always mention the license information at the top: `// SPDX-License-Identifier: MIT`
- always mention the Solidiyt compiler version: `pragma solidity 0.8.24;`
- the contract is always written with the keyword: `contract`

**Basic Datatypes:** `bool`, `uint`, `int`, `address`, and `bytes`. Bytes are a collection of characters written in hexadecimal representation. For example,

```solidity
bytes myBytes = "My name is Souradip";
```

The default visibility of variables within a contract is `internal`, which means it is not visible from external (but only internally to the contract and its child contracts). To make it accessible outside of the contract, `public` can be used, e.g., 

```solidity
bytes public myBytes = "My name is Souradip";
```

**Solidity Functions:**  In Solidity, functions are written with a `function` keyword. An example of a function signature could look like:

```solidity
function myFunction (uint _localVar) public {...}
```

There are two types of functions within Solidity that do not require any gas money:

- **`view` function:** The view functions are such functions that only read the current state of the blockchain, and do not need to perform any transaction to modify the current state of the Blockchain (more like "getter" functions). By specifying a function as `view`, it disallows any modification of the storage variables within the function.

```solidity
function retrieve () public view returns (uint256) {
    return classVar;
}
```

- **`pure` function:** The pure functions is a stricter version of the view function which additionally disallows reading from the storage variable. For example,

```solidity
contract DemoContract {
    uint256 classVar = 256;

    function retrieve () public view returns (uint256) {
        return classVar;    // Will not throw an error
    }
    function retrieve () public pure returns (uint256) {
        return classVar;    // Will throw an error
    }
}
```