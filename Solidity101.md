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

**Solidity Functions:** 