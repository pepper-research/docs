# Research Interests

Spicenet is a research driven Layer 2 that conducts protocol and scaling research to deliver better UX to our users. We take special interests in various scaling topics, such as low-level performance optimizations, faster state tries, production-grade ZK provers and literature around sharding, consensus, et al.

This section is a collection of sub-pages which describes our scaling and protocol research efforts, and timeline of integration into the protocol.&#x20;

1. Starting off with the Velocity Runtime -- a state-of-art execution client for Spicenet, consisting of multiple low-level improvements to the core computer architecture, allowing for better resource efficiency and faster processing.&#x20;
2. Asynchronous Execution -- technique which allows the sequencer to asynchronously execute inbound load, leading to better traffic management and faster pre-conf times(\~50% reduction in pre-conf latencies).
3. Sharding -- we envision sharding happening on Spicenet in two forms
   1. Compute Sharding: Allowing multiple sequencer machines to process a part of the load, allowing horizontal scaling of computational resources.
   2. State Sharding: Equivalent to the "Multiple Concurrent Proposers" in the L1 world, but for L2s. Allows permissionless appointment of sequencers, each controlling a "fragment" of state, and giving pre-confs over that state. A separate consensus chain is responsible for not only validating these state transitions across shards, but to come to agreement about the consistency of cross-shard calls.
4. Based Sequencing -- By building the based sequencing ecosystem for Celestia rollups, we bring forth a novel concept in L2 sequencers -- Sequencer optionality. By making the centralized sequencer pivotal yet redundant, we allow users to interact with the chain in multiple ways, and not force a single mechanism, either centralized or based sequencing. This allows us to preserve the low latencies provided by the centralized sequencer in happy scenarios, while providing an alternative path is (almost) equally performant, that is based sequencing.
