# Sequential execution: Our current state

## State commitment is the bigger bottleneck

The current meta in crypto twitter is all about "parallel execution" and "parallel VMs". But, in this section, we'll prove that without fast state commitment, parallelization is not sustainable. And on the other hand, a sequential execution pipeline that can commit to state faster will be much faster than a parallelized runtime that does not.

Let's understand this better. Execution of transactions refers to applying new inputs to a state database, leading to a new "global state", if the state is merkleized. In a normal execution flow, state commitment, i.e computing the merkle tree typically occupies the highest overhead. Hence, distributing work over many cores will not help much if state commitment overhead is not solved. In fact, distributing work over many cores will in fact lead to performance degradation at a point, because inputs are consistently being applied to the state database, while the database is not able to scale capacity with the induced demand.

However, sequential execution with scalable state commitment will see much higher performance because the major overhead, i.e state commitment is now being handled effectively, and does not bottleneck that easily. The Sovereign SDK, in particular, features a scalable state commitment scheme called [NOMT](../nearly-optimal-merkle-tree.md) along with sequential execution, following a similar philosophy mentioned above. Although parallel execution will be added to the protocol at same point, the core belief is that state commitment needs to be solved before to allow for sustainable parallelization.&#x20;

## When and How Parallelism?

The plans for implementing havent been laid out yet, and will probably be a part of Velocity, our new, high-performance runtime. This is because of our belief that state commitment is the major bottleneck, not parallelism. And, to take full advantage of parallelism, Spicenet's state commitment engine needs to be made robust and scalable.&#x20;

Moreover, parallelism as a concept is incredibly vague. Only parallelizing transaction processing is not sufficient to unleash maximum performance. Multiple "aspects" of execution can be parallelized. Hence, it is not completely straightforward. There needs to be an understanding of what parts of the pipeline are bottlenecking due to capacity, before horizontally scaling those processes across cores. For example, we have looked at parallelized sub-tree computation via namespaces, and horizontally scaled recursive ZK proofs. This is to illustrate that parallelization is not just parallel transaction processing, but parallelizing the whole pipeline, and hence is non-trivial.&#x20;

However, we have looked at different parallelization models for different use-cases and deeply understood the nuances between each model. For example, the Block-STM execution engine pioneered  by Aptos is highly impressive as it does not unnecessarily lock state for a specific duration(in most cases, a block), leading to inherently higher parallelism. Pessimistic execution, i.e the model used by Solana is equally exceptional and can be used in cases where it is crucial to know what state will be touched in advance.&#x20;

