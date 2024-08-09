# Introduction

PepperDEX SpiceNet is an Optimistic sovereign rollup on Celestia, with a two-way, trust-minimized bridge to/from Solana. SpiceNet is powered by the Sovereign SDK, which provides a seamless way for developers to spin up rollups on any DA layer(for eg: Celestia, Solana, Bitcoin, etc).&#x20;

The main goal of SpiceNet is to provide a full-fledged and performant base for our modular derivatives trading protocol Dexterity. Dexterity initially is an offering developed by the Hxro Network on Solana. PepperDEX plans to use Dexterity as a starting point and make modifications to it.

This documentation aims to provide a holistic view of SpiceNet's architecture, design decisions and approaches that enable our end-goal of <1ms soft-confirmation times, and 30-200ms E2E latency for users.

SpiceNet architecture can be broken down into these important constructions and developments:

* [Technical Discussion](technical-discussion/): Contains details about different components and design considerations of SpiceNet.
* [Sovereign Rollups](sovereign-rollups.md): Understanding the differences between Sovereign rollups and Settled rollups.
* [Research interests](research-interests/): Sharing our research that aims to scale the capacity and performance of Spicenet.
* Project Artemis
  * Trust-minimized bridging
    * blobstream-solana
    * solana spv-client
  * Trust-minimized interoperability

We believe that the above architecture provides a path towards scalability for decentralized exchanges, all the while maintaining the decentralization status-quo. Our north star would always be to maximize performance **and** decentralization while minimizing downtime and clunky UX for the end-user. We do not aim to make SpiceNet another general-purpose rollup, although we do want to make it permissionless, there is a fine line between the both. General purpose computation refers to arbitrary computation in a pre-defined environment, this would add huge engineering overhead and destroy our performance benefits. Permissionless innovation refers to the ability of anyone to add new features to the SpiceNet trading engine.&#x20;

SpiceNet uses the Sovereign SDK as a base, and is written in pure rust. The subsequent sections dive into the design choices we've made while building this and what it means for you, the reader.
