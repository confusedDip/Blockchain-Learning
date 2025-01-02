# Solidity Smart Contract Development 101

The notes are compiled from the Cyfrin Updraft Course for future reference

**Remix IDE:** It is a web-based IDE for Solidity development, a very good starting point to learn about Solidity basics.

- it has built-in solidity compilers, which can compile the `.sol` files we write for our smart contracts
- it has built-in ethereum deployment tool, which can be used to deploy our compiled smart contracts to test/real networks

> **Basic MUST DOs for Solidity Smart contracts:**
>
> - always mention the license information at the top: `// SPDX-License-Identifier: MIT`
> - always mention the Solidiyt compiler version: `pragma solidity 0.8.24;`
> - the contract is always written with the keyword: `contract`

## Basic Datatypes

`bool`, `uint`, `int`, `address`, and `bytes`. Bytes are a collection of characters written in hexadecimal representation. For example,

```solidity
bytes myBytes = "My name is Souradip";
```

The default visibility of variables within a contract is `internal`, which means it is not visible from external (but only internally to the contract and its child contracts). To make it accessible outside of the contract, `public` can be used, e.g., 

```solidity
bytes public myBytes = "My name is Souradip";
```

## Solidity Functions

In Solidity, functions are written with a `function` keyword. An example of a function signature could look like:

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

## Solidity Arrays

Declaration of arrays in Solidity is similar to other programming languages. The arrays are 0-indexed. For example,

```solidity
uint256[] myUintArray = [2, 5, 10];
```

## Solidity Structures

Like `C`, there are user-defined data types in Solidity as well, known as, `struct`. 

```solidity
struct Person {
    address pub_key;
    address pvt_key;
    string name;
    uint256 balance;
}
```

Instantiating variables of the defined structure can be done as follows:

```solidity
Person public person = Person(0x2..ab, 0x2..ff, "Souradip", 5000);
/* equals to
Person public person = Person({
    pub_key:0x2..ab,
    pvt_key:0x2..ff,
    name:"Souradip",
    balance:5000
    });
*/
```

## Solidity Data Storage and Management

In Solidity, data can be stored in 6 different locations:

1. `memory`
2. `calldata`
3. `storage`
4. `stack`
5. `logs`
6. `code`

- In Solidity, `calldata` and `memory` are temporary storage locations for variables during function execution.
- `calldata` is read-only, therefore mostly used for function input parameters.

```solidity
function myFunction (string calldata _myString) {...}
```

- `memory` provides both read-write access, allowing them to be changed within functions. By default, most variables declared are default to memory. However, for strings, it must be explicity specified as follows:

```solidity
string memory myString = "Souradip";
```

- `storage` variables are the ones that are persistent and are stored in the Blockchain. Therefore, variables declared within a function cannot be a storage variable.

## Mappings

Mappings in Solidity are equivalent to dictionaries in Python, and essentially are lists of key-value pairs. Here is the syntax to declare a mapping in Solidity:

```solidity
mapping (uint256 => string) public names;
names[1] = "Souradip";
```