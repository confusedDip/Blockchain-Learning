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

## Loops in Solidity

**For Loops in Solidity** has the following syntax:

```solidity
for (/* starting index; ending index; step count*/) {...}
```

## Import in Solidity

It is possible to import one solidity file to another using the `import` keyword. There are different ways we could do that:

- Importing the entire `.sol` file by `import "path_to_the_file";`
- Named imports, by specifying the exact contract names within curly braces: `import {contractName} from "path_to_the_file";`

It is also possible to deploy one contract from another using the `new` keyword. For example,

```solidity
contract ContractOne {}

contract ContractTwo {
    ContractOne public contractOne = new ContractOne();
}
```

## Inheritence in Solidity

Solidity supports inheritence and method override using the `is` and `override`, `virtual` keywords, respectively.

```solidity
contract ParentContract {}
contract ChildContract is ParentContract {}
```

Method override can be possible in the child contract by specifying the exact same method signature along with the keyword `override`, and specifying the original method in the parent contract as `virtual`

```solidity
contract ParentContract {
    function myFunction() public virtual {}
}
contract ChildContract is ParentContract{
    function myFunction() public override {}
}
```

## Creating Libraries in Solidity

Libraries in Solidity are very similar to contracts, just they cannot have any state variables and all variables/functions must be marked internal.

To create a library use the `library` keyword instead of `contract`. Then through named imports, the library can be imported wherever it is needed to be used. Also, the library can be attached to a specific datatype by using the following statement:

```solidity
using LibraryName for DataTypeName;
```

## The `require` keyword

The `require` keyword in Solidity is used to conditionally revert transactions. If a certain condition is not fulfilled all instructions performed prior to the `require` check are reverted and the remaining gas cost is returned.

```solidity
uint x = 5;
x += 2;
require(x < 2, "x must be less than 2");
...
```

## Optimizing Gas Cost using `constant` and `immutable`

To reduce gas usage, we can use the keywords `constant` and `immutable`. These keywords ensure variables assigned once and never change. 

- For values known at **compile time**, use the `constant` keyword. It prevents the variable from occupying a storage slot, making it cheaper and faster to read. The naming convention for `constant` variables are all caps (e.g., `MIN_USD`).
- `immutable` can be used for variables set at deployment time that will not change. The naming convention for `immutable` variables is to add the prefix `i_` to the variable name (e.g., `i_owner`).

## Modifiers in Solidity

Modifiers in Solidity allows us to execute some reusable `pre` or `post` code before the execution of a function and tie it to a single keyword. For example,

```solidity
function withdraw() public positiveBalanceOnly {
    ... // Before executing the logic, the logic in positiveBalanceOnly() will be executed
}
modifier positiveBalanceOnly() {
    require(balance > 0, "Balance must be positive");
    _;  // This will execute the logic in the function whoever calls it
}
```

## Sending ETH through a function

A function in Solidity can allow users invoking the function to send money through it. Here are the steps:

- Make the function payable by specifying the keyword `payable` in the function signature
- Wei/GWei/ETH can be sent through functions using the `value` field
- A contract can store this value and the balance can be accessed through `msg.value`

## Withdraw ETH from a contract

There are three ways the ETH balance stored in a Smart Contract can be withdrawn:

- **Using `transfer`:** This needs a casting of `payable` to the receiver's address to transfer the balance of the smart contract. 
   
```solidity
payable(msg.sender).transfer(address(this).balance);
```

The issue with `transfer` is that the gas cost for using `transfer` is capped at 2300 gas. Therefore, if more gas is needed, it will throw an error and revert the transaction.

- **Using `send`:** This is similar to `transfer`. However, in case of a failed transaction, instead of throwing an error it will return an error code in boolean.

```solidity
bool sendSuccessCode = payable(msg.sender).send(address(this).balance);
require(sendSuccessCode, "Transaction Unsuccessful");
```

- **Using `call`: *(Recommended)*** This is a low-level command that can be used to call any part of the contract using transactions. To withdraw the balance stored, we can perform an empty call with the contract balance as the `msg.value`. Interestingly, `call` does not have a gas cap unlike the other two options.

```solidity
(bool callSuccess, bytes memory returnedData) = payable(msg.sender).call{value:address(this).balance}("");
require(callSuccess, "Transaction Unsuccessful");
```

## `receive()` and `fallback()`

These are special Solidity functions that are called with every transactions. If transactions are called with no-calldata, `receive()` is triggered, otherwise `fallback()` is triggered.

`fallback` is executed either when:

- a function that does not exist is called or
- Ether is sent directly to a contract but `receive()` does not exist or `msg.data` is not empty

```solidity
                 send Ether
                      |
           msg.data is empty?
                /           \
            yes             no
             |                |
    receive() exists?     fallback()
        /        \
     yes          no
      |            |
  receive()     fallback()
```