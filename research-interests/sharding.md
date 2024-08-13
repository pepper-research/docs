---
description: How do you horizontally scale capacity?
---

# Sharding

One of the biggest problems of blockchains is if they can scale their capacity horizontally. Horizontal scaling refers to adding more capacity to the system rather than trying to upgrade the existing system in hopes to add more capacity(which is called as vertical scaling). One of the most common ways of horizontal scaling is sharding. Although sharding as a concept is cursed in crypto, with Ethereum pivoting to L2s and new L1s demonstrating high-performance without sharding, we believe that sharding as a concept will be adopted in some form by the space in the coming years.&#x20;

But why? The answer is quite simple -- at any point of time, compute utilization is isolated to only one person in a network, the leader. This fundamentally limits the scalability of the network to the computation capacity of the leader. Sure, validators can upgrade their machines but this harms decentralization. Sharding provides a subtle approach to this problem, wherein the global compute of the network is identified as the compute resources of _multiple_ leaders combined. These leaders operate over different fragments(or "shards") of state. The "root shard" or the "consensus shard" ensures consistency and validity of shards and cross-shard calls.&#x20;

But, why are we thinking of sharding? The aim of Spicenet is to offer performant, decentralized, reliable and scalable trading services to users, and scale current DeFi experiences by atleast 10x or more. We quickly realized that continuously upgrading the centralized sequencer hits a roadblock in terms of costs and scalability. Moreover, we also want to decentralize the sequencer, and be a _truly_ decentralized network, while sustaining performance. Sharding allows us to do exactly this.&#x20;

And how are we implementing sharding? While the spec of sharding on Spicenet is not fully final, in this section we will discuss the different approaches of sharding identified by us, and their properties.&#x20;

## Sequencer Clustering

Sequencer clustering refers to decoupling the various processes undertaken by a centralized sequencer and distributing them across various specialized and hyper-optimized machines. Another way to think of this is the centralized sequencer being represented by a collection of machines with each machine handling a portion of the load. Depending on demand, more machines can be spun up.

Admission to the sequencer cluster is controlled by the protocol, so that control over configuration of the machines can be closely monitored by the protocol, allowing for quick modifications and upgrades. We aim to partner with leading node platforms and firms to run machines within the sequencer cluster. But, let's understand how the sequencer cluster fits into the transaction flow and the noticeable differences observed.

The sequencer cluster has a main entrypoint which refer to as the SequencerDAG, whose main job is to determine ordering of transactions and direct them to different includers. SequencerDAG can be thought of as a record book that monitors the transaction flow, directs transactions to different includers(also referred to as "machines") and builds the Direct Acyclic Graph(DAG). The DAG is built as a collection of all pending transactions and the includers they're allocated to.&#x20;

An includer has 3 broad responsibilities -- giving an inclusion pre-confirmation, preparing a mini batch to be posted to Celestia, and posting a micro batch to be sent to the executor. When an includer receives a transaction, it responds with a pre-confirmation and asynchronously sends a "micro" batch, consisting of a few transactions to the executor. On the other hand, it works on preparing a "mini" batch, consisting of slightly more transactions, and posts them to Celestia.&#x20;

The executor receives micro batches from includers and executes them in the order specified by SequencerDAG. It also verifies the authenticity of the includer by cross-verifying with SequencerDAG. This is possible because SequencerDAG maintains a DAG of all pending transactions and the respective includers they're allocated to. Every hundred milliseconds or so, the executor produces a new global state and disseminates the result of execution to the network of RPC providers and full nodes, which finally results in the end user seeing data update on their client.&#x20;

This process includes some, but not significant overheads in the form of includer -> executor communication, but since both parties are co-located, the latency penalty is negligible. While pre-confirmations are given instantly, it can take upto a few hundred milliseconds more for the execution outcome to be reached, and the state refreshing on the user side. Execution outcome is what matters for the end user, and not a pre-confirmation(obvious, a deposit would succeed when the user balance increases, not when a user receives a pre-confirmation). And in situations where two transactions originating from different machines collide, the primary has a right to overwrite the pre-confirmation of one of the transaction, to allow the other to pass. And this decision is made using the SequencerDAG which provides ordering integrity defined by timestamp. For example, if a market maker is trying to cancel their order and an arbitrageur is trying to take the same order, SequencerDAG provides ordering for these transactions and allows the primary to choose what transaction came first(although both may receive pre-confirmations).

Here's a glossary of different roles within the Sequencer cluster:

* SequencerDAG: Ensures ordering integrity by timestamp and builds a Direct Acylic Graph of transactions being handled by different includers(also referred to as "machines").
* Includers: Hyper-optimized machines that receive transactions and respond with a pre-confirmation. Also send their own batches to Celestia. Each batch is referred to as a "mini batch".
* Executor(also known as "primary"): Typically just one party, responsible for executing transactions pre-conf'ed by includers in the order specified by SequencerDAG.

&#x20;
