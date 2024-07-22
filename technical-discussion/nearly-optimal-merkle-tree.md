# Nearly Optimal Merkle Tree

Committing to state is one of the biggest bottlenecks an execution environment can have. Re-computing the root of a merkle tree can be quite a heavy operation. To compute the merkle root for a given leaf, we not only need the adjacent inner node of the leaf, but also the other child of the root node itself. Denser the tree, more the number of nodes required to re-compute the root. Hence, improving the performance of state commitments is one of the easiest, yet the hardest way of speeding up the execution phase.

The Sovereign SDK has global state, and by definition operates under a merkle tree environment. To tackle this overhead, they have introduced the Nearly Optimal Merkle Tree(NOMT), which makes improvements across the board, such as modifications to the tree structure, layout on disk, improving the hash function, better compression algorithms, cache state aggressively, and other neat improvements.

In this section, our aim is to share the main places of improvement while not going into extreme depth. Users wanting to learn more can visit their official documentation [here](https://sovereign.mirror.xyz/jfx\_cJ\_15saejG9ZuQWjnGnG-NfahbazQH98i1J3NN8). Some of the major improvements can be understood here:

* Make the trie binary instead of a 16-ary(or more) tree
* Improve layout of trie on disk
* Compressing metadata with the help of discriminants
* Halve compressions with a padding-free hasher
* Blake3 makes leaf nodes BIG!
* Be smart and cache regularly accessed nodes
* Avoid committing to the merkle tree whenever possible.
* Write to tree in batches

SpiceNet is introducing improvements of it's own, focussed on better handling of load directed towards the database, with technologies with as async i/o and pipelining. The aim is to strategically improve state access mechanisms resulting in better read/write performance.

