# Sovereign Rollups

## Settled rollups vs Sovereign rollups

SpiceNet is a sovereign rollup built on Celestia using the Sovereign SDK stack. This means that we are different from regular rollups, which are also termed as "settled rollups". Most of the rollups we see today, like Arbitrum, Optimism, zkSync, etc are examples of settled rollups.&#x20;

But, what are settled rollups? They are a class of rollups wherein the canonical chain of the rollup is determined by the base layer that the rollup is settling to. Settled rollups inherit the security of the base layer, but are also bound by the rules set by the social consensus of the base layer. This means that although settled rollups inherit the censorship resistance and re-org resistance properties of the base layer, on the downside they also have to follow where the social consensus of the base layer goes. In other terms, we can describe settled rollups as those rollups wherein settlement, DA and consensus are decided by the base layer, while only execution freedom is provided to the rollup. Moreover, they also cannot hardfork, and if they do, they hardfork along with the base layer(as seen with Ethereum hardforks like Dencun).

On the other spectrum are Sovereign rollups, which provide more freedom and sovereignty to the social consensus of the rollup itself. In a sovereign rollup construction, the rollup full nodes, i.e the social consensus determine the canonical chain. This means that settlement is handled by the rollup itself, with only consensus and DA offloaded to a base layer. However, contrary to popular assumptions, sovereign rollups DO inherit the security of the DA layer. For examples, a sovereign rollup still depends on the DA layer to provide censorship and re-org resistance. Also, the social consensus of a sovereign rollup also votes on hardforks. All-in-all, we can differentiate sovereign and settled rollups depending on the "involvement" and "presence" of the rollup's social consensus.&#x20;

{% hint style="info" %}
Fun fact: Most people think that sovereignty and interoperability cannot be achieved together, and one has to make a tradeoff. Although we agree, and that to interoperate, one needs to have some binding agreement with other chains which leads to eventual degradation of sovereignty, we also believe that on a pure construction-to-construction basis, sovereign rollups are actually _better_ than settled rollups in terms of interoperability.

Let's understand this a bit better: Settled rollups, in order to interoperate with each other must first need to prove their state to a (often expensive) base layer. Sovereign rollups, on the other hand, do not have to do so. They can simply extend a light client/zk proof which can be run/verified by other sovereign rollups _without_ having to go through a base layer. This provides better latencies and superior UX for interoperability.
{% endhint %}

***
