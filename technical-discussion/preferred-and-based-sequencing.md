---
description: How to be performant and based at the same time
---

# Preferred and Based Sequencing

Centralized sequencers have the ability to offer top notch UX to users. The reason is quite simple -- it is levels of magnitude easier to optimize a single machine than a distributed set of machines. When you're dealing with a centralized sequencer, the only thing you need to worry about is whether the single machine is doing it's job correctly, and at desired speeds. In a distributed network, you need to make sure ALL machines are doing their job's correctly, while maintaining sufficient performance. By virtue, the former is easier than the latter.

However, centralized sequencers pose issues regarding safety and liveness of a rollup. Fraud proofs and permissionless sequencing have been invented as mechanisms to tackle these issues. While we discuss fraud proofs in the next section, here we'll discuss about permissionless, aka based sequencing. First, let's understand how current rollups deal with sequencer failure, i.e a liveness failure.&#x20;

## Arbitrum

Arbitrum One has a "force inclusion" mechanism wherein users can send L2 messages on the L1, that can be finalized after a pre-determined delay. This mechanism is a part of a broader mechanism which allows users to interact with the L2 via the L1, by passing "messages". Normally, a user would interact with Arbitrum by sending transactions to the sequencer. Alternatively, a user can interact with the sequencer by posting a "message" containing of the transaction on the L1. The L1 transactions adds the message to the "delayed inbox" of the Arbitrum bridge. This can be thought of as a waiting queue that messages wait in, till they are moved to the "core inbox". Now, two things may happen depending on the activity of the sequencer

1. If the sequencer is functional: The sequencer picks up these messages from the delayed inbox and includes them in a batch, resulting in the message moving to the core inbox.
2. If the sequencer is down: Once sufficient time has elapsed and the sequencer has not yet included the message in a batch, users can call a "force inclusion" method of the bridge contract, which automatically moves the message from the delayed inbox to the core inbox. The delay in Arbitrum is roughly a day.

To conclude, messages on the L1 can be included in two ways, either by the sequencer, or by force inclusion. Force inclusion may be invoked after a specific time passes during which the sequencer did not include the message in a batch, probably due to downtime. This mechanism allows users to maintain liveness of the Arbitrum protocol even when the sequencer is down.&#x20;

However, this system has a few drawbacks

1. It's not UX-friendly: It's not possible to expect users to understand how to pass messages to the bridge, and invoke force inclusion. Hence although this mechanism aims to preserve liveness, in practice it likely will not.
2. It degrades performance: Even if we expect users to understand the jargon of messages, inboxes and force inclusions, the delay is simply too high and hence is not a scalable mechanism. We need a mechanism that actually preserves liveness while not trading off performance to _such_ a large extent.&#x20;

## SpiceNet

SpiceNet has a specific "based sequencing" option powered by the Sovereign SDK that allows users to directly submit transactions to the DA layer(Celestia). A user can self-sequence their transactions provided their transactions exceed a specific size. We call the mechanism of sequencing by a single party as "preferred sequencing" as that party has control over ordering, MEV, and most importantly, control over state and over pre-confirmations.&#x20;

SpiceNet addresses the above problems revolving force inclusion in the following ways:

1. Expressing mode of sequencing: Users can express their favourable mode of sequencing within the SpiceNet wallet. While the default mode is set to use the preferred sequencer, users can modify it to self-sequence their transactions. However, transactions may fail if the transaction size is too small for self-sequencing.
2. Reduced block times and based pre-confirmations: Users experience faster performance while using based sequencing because finality is typically offered within 2-3 full Celestia blocks, which is in about 30s. Moreover, based pre-confirmations may be adopted by the protocol allowing the current Celestia L1 proposer to offer inclusion guarantees. It is still not sure whether SpiceNet will make use of based pre-confirmations alongside the pre-confirmations provided by the preferred sequencer, and if so, what would the design of it look like.

