---
description: >-
  A knowledgeable man once said - "The State Transition Function of a blockchain
  determines how closer to perfection the blockchain is."
---

# Deterministic STF and Sequencer

SpiceNet at it's core is an app-rollup designed to offer fast and reliable trading experiences. In this section, we'll be talking on the "reliability" part of SpiceNet. The problem stems from the understanding that if an exchange is the fastest in the world, but goes down once in a week, it cannot survive. To have a successful exchange, one needs to have speed and the UX benefits of it, but also be able to maintain the exchange properly.&#x20;

A lot of exchange downtimes can be traced to either various exchange components not being able to update in-time, or update shared state in-time. We refer to this shared-state as "internal state". This includes oracle prices, internal data structures, identifiers, discriminants, and so on. Validity of internal state is a crucial determinant for the smooth working of an exchange. Hence, SpiceNet enshrines rules and mechanisms into protocol that prioritize validity of internal state. These includes mechanisms such as economic backpressures, gas models, internal and full sharding, and so on.

## Economic Backpressures

One of the easiest ways to reduce downtime is to introduce economic incentives to the sequencer to include transactions that keep exchange state up-to-date. This is made possible with the "Capabilities" feature of the Sovereign SDK, which allows rollup developers to define what features they want enabled on their rollups. This typically includes new modules such as staking, DEX, etc. However, specific rules can also be incorporated into rollup capabilities. These rules are typically boolean and when false, penalize the sequencer by depriving them of their rewards.

Yes, sequencers on SpiceNet get rewards for their contributions to the network, which is sequencing of transactions. And adding specific rulesets to our rollup capabilities allows us to create economic backpressure against the sequencer, enticing them to behave according to our needs. In this case, the rulesets can be as simple as checking whether the oracle module was updated recently or not, or whether there is a future that is yet to expire, or an account that is yet to be liquidated. Each rule has a specific interval associated with it, for eg: has there been an oracle update in the last 5ms, or have there been any new liquidatable accounts in the past 5ms, etc. These rulesets economically incentivize the sequencer to behave according to our needs. For example, they are incentivized to include transactions that update oracle prices, because if they do not, the ruleset corresponding to oracle update fails which invalidates the rollup capabilities and deprives the sequencer of rewards for that specific batch.&#x20;

***

## Solving the supply side

Now, we have a way to ensure specific kinds of transactions are included and thus the state of the chain is updated at all times. But, how do we make sure that the transactions are initiated properly? For example: how do we ensure that new oracle price updates are created? In other terms, we have solved the demand side by incentivizing the sequencer to "demand" more exchange state updates, but how do we solve the supply of these state updates? The answer is -- backups!

We always try to create backups and economic incentives that ensure there is sufficient supply that can be _consumed_ by the sequencer. In the context of backups, for every internal state update, we always maintain two options -- one is an agnostic set of keeper nodes that are programmed and economically incentivized to periodically create new transactions. All nodes within the keeper network perform actions concurrently, thereby reducing chances of irresponsivenes and a single point of failure. For example, keeper nodes produce prices concurrently which are then aggregated. As a backup, we make use of the sequencer's resources to conduct these operations, assuming a complete failure of the keeper network. Because the keeper network is much more decentralized and trust-minimized, we use it as a primary option, while keeping the sequencer as a last resort.

***

## Further Research

We understood that in order to prioritize specific state transitions over others, we would need to introduce some kind of economic backpressure to the sequencer, incentivizing it to include the aforementioned state transitions over others, i.e attach a higher priority to them. The sequencer does it with the help of a formal concept in the protocol known as a "Call Message".  A Call Message can be understood as a specific state transition with pre-defined parameters and some understanding of the result thereof. An example of a call message can be `UpdateOracle`, whose parameters can be the oracle feed(s) to update, and the update(prices) itself. "Call Messages" can be thought of as a collection of different call messages, only one of which can be invoked at a time.&#x20;



