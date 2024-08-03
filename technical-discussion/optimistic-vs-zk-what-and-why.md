---
description: >-
  Security, liveness and performance implications of choosing a particular type
  of rollup.
---

# Optimistic vs ZK: What and Why?

The debate regarding whether optimistic or ZK rollups are better is not completely clear. However, it is clear that optimists and the visionary type normally go the ZK route while the practical people go the optimistic route. The main argument from the ZK camp is that ZK is a hyper-scalable technology that can be used on various inputs, and can massively save resources incurred by nodes while recomputing state transitions. The counter argument from the optimistic camp is that ZK is utopian technology and is not feasible to use in production at the moment.&#x20;

As a rollup, it is important to not only pick a side, but also define a roadmap that battles the downsides introduced by optimistic or zk, whatever chosen as the preferred type of the rollup. Spicenet will initially be an optimistic rollup, with fraud proofs live from day 1.&#x20;

## Stage 0: Optimistic rollup

### What is an Optimistic rollup?

Optimistic rollups allow the sequencer to optimistically decide the validity of transactions without any formal proving. This approach aims to evade any form of continuous proving while only having to prove transactions when fraud is detected/or claimed. This is where the optimistic rollups inherit their "optimistic" name.

### Flaws with current ORU designs

In the happy case, this design also enables optimistic rollups to be extremely fast as the typical transaction flow in an optimistic rollup is not bottlenecked by expensive and slow provers. In the worst case, when a typical state transition is challenged, there is a need for a resolution mechanism, i.e a fraud proof. The current fraud proof designs are extremely complex, convoluted, slow and expensive. Moreover, fraud proof implementations that are live on mainnet, such as Arbitrum's and Optimism's, are largely different from each other, and a broader lack of understanding of the differences may make these implementations prone to bugs.&#x20;

Hence, there is a requirement for a much simpler, faster and cost-efficient fraud proof design. Introducing ZK fraud proofs. Fraud proofs today are designed as an "interactive game" where the challenger and the asserter play a game whose main aim is to deduce the fraudulent state transition, and each step of the game is represented as a transaction on-chain. ZK fraud proofs work differently.&#x20;

### Construction of a ZK fraud proof.

When full nodes receive transactions from the DA layer, they proceed to execute them in the hopes of achieving the same result. If the results achieved by the full nodes match with that of the sequencers, it unequivocally means that the state transitions are valid. However, if the full nodes achieve different results(represented by the root of the NOMT) from that of the sequencer, it means that the sequencer has made a mistake in computing the root. In such a situation, a "challenge" can be invoked by one of the full node. During this process, the transactions are fed to a ZK prover which outputs a ZK proof of the correct computation. If the merkle root calculated by the prover does not match the one computed by the sequencer, it is deemed that the sequencer incorrectly computed the state and is slashed, while the state is altered to reflect the correct computation. However, if the merkle root calculated by the prover _does_ match the one computed by the sequencer, the challenge is deemed to be invalid and the full node is slashed.

### Benefits

ZK fraud proofs have multiple benefits. First, the ZK fraud proof design is simple and straightforward to understand, and is less prone to colossal implementation bugs. Second, the collective fraud proof is just represented by a single ZK proof which takes just a few minutes to verify on slow chains like Ethereum. Compared to current designs, where the fraud proof is represented by 50 or so transactions that all need to be finalized on Ethereum. This is a root cause of the lengthy challenge periods imposed upon Optimistic rollups, because all of the transactions need to be given sufficient buffer to be verified and finalized on Ethereum. This means that the challenge periods can be drastically slashed by \~20x or so, to just a few hours.

## Risks and Precautions

The main point of risk in a ZK fraud proof system is the robustness of the prover used. Replacing IVG proofs with a custom prover does not reduce complexity, as the ZK prover can still have bugs. Custom provers are often time consuming to write, difficult and expensive to audit, and highly exposed to bugs. ZK VMs are a great solution as the same proving logic can be written in a few weeks with ZK VMs compared to a few months with custom provers. The bug surface with ZK VMs is comparitively low because they only need to be audited once and the same ZK VM can be re-used across different applications, while the same cannot be said for custom provers.

## Stage 1: A full ZK-Rollup

Spicenet is an extremely ZK-friendly rollup as understood from our usage of ZK even in our Optimistic rollup construction. We are aware of the benefits of ZK, and choose to benefit from the scalability advantages of ZK. However, we feel that ZK as a technology as of today is underused. We believe that we are still scratching the surface of the world of possibilities enabled by ZK.&#x20;

Spicenet will transition to a full ZK-rollup when the economics of continuous ZK-provability make sense to the amount of scale we are targeting. ZK today is already a viable technology, as a proof can be produced at the cost of a few cents, and verified in a few minutes. However, the main barometer of "ZK readiness" for Spicenet is the ecosystem of use-cases enabed by ZK. We are heavy believers of ZK-based bridging protocols, ZK-based interoperability solutions, and more. We are actively work on our Project Artemis that actively uses ZK technology to elevate Spicenet with unique capabilities such as trust-minimized bridging and secure interoperability.

Mainnet deployment of Project Artemis across Solana, Spicenet and Celestia will complete the transition of Spicenet to a ZK rollup, with ZK proofs being produced asynchronously(separate from transaction processing pipeline) and ingested by Artemis via Celestia. Transactions will be sent to specialized prover nodes via Spicenet's P2P layer, with the prover nodes running the actual prover software that batch-prove transactions. The proof is then assimilated by the full nodes via P2P. Upon successful verification, full nodes update their state. Artemis, as a separate process ingests this "execution" proof along with an "inclusion" proof from Celestia to enable secure interoperability between Spicenet and other rollups.&#x20;

