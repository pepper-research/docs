# Why Spicenet does not have a VM

## VMs and their applicability in blockchains

Another meta in crypto twitter is about the different kinds of VMs used by different blockchains. SVM, EVM, MoveVM, WASM, and so on. Whole new narratives are being built around XYZ VM on Ethereum, and Multi-VM programming networks. We believe that VMs are excessively overhyped more than they should be. Let's first understand what a VM actually is. A VM stands for a Virtual Machine, which simply means a specialized program running on a computer that has it's own set of processes and instruction sets. VMs are particularly popular in blockchains because they allow blockchain runtimes to define specific processing logic, via custom opcodes(or operation codes) and instruction sets. A blockchain demands specialized processing to enable custom and expressive logic, aka "smart contracts". Smart contracts can be thought of as logic that performs various kinds of operations, given it adheres to mathematical validity(for example: user has sufficient gas to perform the operation, user has allowance to perform the operation and so on).

## Why VMs don't apply to Spicenet

The main purpose of smart contracts is to enable general purpose and permissionless programmability on top of blockchains. Spicenet, being an application rollup, does not desire general purpose programmability for it's chain. Hence, there is no explicit need for a VM whose only job is to enable expressive, blockchain-native logic.

## Other facts about VMs

Many people tend to think that VMs can achieve processing performance equivalent to native(refers to   computer architecture level instructions. For example: x86 instructions) given sufficient optimization and memory management. However, this is conceptually not true as VMs still need to compile their instructions to native instructions, which fundamentally adds an overhead, although small.

Spicenet optimizes for hyper performance and desires to eliminate any form of overhead that is unnecessary for the protocol.&#x20;

## How processing works on Spicenet

Before understanding how processing works on Spicenet, it is important to understand how application logic is written on Spicenet. Blockchains typically impose a specific smart contract language on developers. The popular ones are Solidity, Rust(Anchor), and Move. The smart contract language can be visualized as an ecosystem of tools exposed by the blockchain to allow developers to build apps on top of it.&#x20;

However, application logic on Spicenet and the Sovereign SDK is written in pure rust and compiled using the traditional rust runtime. Hence, code on Spicenet can be visualized as a lower level primitive than a smart contract language, because its much more generic and expressive, unlike a smart contract language which is specific and constrained to the blockchain's design.

Application logic is dubbed as "modules" on Spicenet, similar to the Cosmos SDK. A module is stateless, meaning that it does not store state of it's own, but instead specifies the state it has access to. This allows modules to be lightweight and easy to manage. State lives in the global merkle tree and modules perform traditional CRUD operations to it. Hence, the experience of writing modules is extremely intuitive and simple.

A transaction typically interacts with one or two modules. The runtime can be visualized as a collection of operations that transition the state, also natively known as "call messages". A transaction typically invokes one or more of these call messages within a module, and upon successful verification, leads to a state transition. As mentioned before, all of such logic is written in vanilla rust and compiled to libraries using the traditional rust compiler(over a blockchain compiler like Anchor or Solidity. Hence the target produced is also similar to vanilla rust crates instead of specialized targets produced by blockchain compilers).
