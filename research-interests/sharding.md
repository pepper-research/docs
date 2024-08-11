---
description: How do you horizontally scale capacity?
---

# Sharding

One of the biggest problems of blockchains is if they can scale their capacity horizontally. Horizontal scaling refers to adding more capacity to the system rather than trying to upgrade the existing system in hopes to add more capacity(which is called as vertical scaling). One of the most common ways of horizontal scaling is sharding. Although sharding as a concept is cursed in crypto, with Ethereum pivoting to L2s and new L1s demonstrating high-performance without sharding, we believe that sharding as a concept will be adopted in some form by the space in the coming years.&#x20;

But why? The answer is quite simple -- at any point of time, compute utilization is isolated to only one person in a network, the leader. This fundamentally limits the scalability of the network to the computation capacity of the leader. Sure, validators can upgrade their machines but this harms decentralization. Sharding provides a subtle approach to this problem, wherein the global compute of the network is identified as the compute resources of _multiple_ leaders combined. These leaders operate over different fragments(or "shards") of state. The "root shard" or the "consensus shard" ensures consistency and validity of shards and cross-shard calls.&#x20;

But, why are we thinking of sharding? The aim of Spicenet is to offer performant, decentralized, reliable and scalable trading services to users, and scale current DeFi experiences by atleast 10x or more. We quickly realized that continuously upgrading the centralized sequencer hits a roadblock in terms of costs and scalability. Moreover, we also want to decentralize the sequencer, and be a _truly_ decentralized network, while sustaining performance. Sharding allows us to do exactly this.&#x20;

And how are we implementing sharding? While the spec of sharding on Spicenet is not fully final, in this section we will discuss the different approaches of sharding identified by us, and their properties.&#x20;

## Sequencer clusters

Sequencer clusters refer to distributing processing within the centralized sequencer to multiple machines. Another way to think of this is the centralized sequencer being represented by a collection of hyper-optimized machines with each machine handling a portion of the load. Depending on demand, more machines can be spun up.

Admission to the sequencer cluster is controlled by the protocol, so that control over configuration of the machines can be closely monitored by the protocol, allowing for quick modifications and upgrades. We aim to partner with leading node platforms and firms to run machines within the sequencer cluster.  But, let's understand how the sequencer cluster fits into the transaction flow and the noticable differences observed.

The sequencer cluster has a main entrypoint whose main job is to determine which machine to send the transaction to. We also refer to it as the middleware, as it is responsible for load balancing between the machines, and ensuring no machine is overloaded. It also acts as a monitoring tool for the protocol to swiftly scale up resources in accordance with demand. When the sequencer receives the transaction, it instantly responds with a pre-confirmation(execution is done asynchronously following the spec of [Asynchronous Execution](asynchronous-execution.md) and machines only need to receive transactions and keep a record of them). Each machine then asynchronously sends these transactions to what we refer to as the "primary", which is responsible for batching and posting to Celestia, as well as executing transactions(following the spec of [Asynchronous Execution](asynchronous-execution.md) wherein the sequencer, or in the case, the primary can execute older transactions while parallely batching and ordering newer ones).&#x20;

To sum up, the sequencer cluster has 3 broad roles, which are

* The entrypoint, also referred to as the middleware: Responsible for distributing load between machines and ensuring machines aren't overloaded.
* The machines themselves: Receive transactions and respond with an inclusion pre-confirmation. Run by node operator teams.
* The primary: Prepare a batch for newer set of transactions and execute older ones.

