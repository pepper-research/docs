# DA Layer Considerations

## Understanding DA layers

Rollups typically operate over a base layer and utilize the base layer for different purposes. Settled rollups use the base layer to provide settlement, ordering and DA(Data Availability) services while Sovereign rollups like Spicenet use the base layer to only provide ordering and DA, while the settlement is handled by the social consensus of the rollup.

The most popular DA layer is the Ethereum L1, traditionally used by rollups for the purpose of storing and retrieving data. However, Ethereum poses multiple challenges which limit the scaling of rollups. Currently, high throughput rollups will face multiple problems pushing high data volume to Ethereum, in the form of high fees and finality periods. Although EIP-4844 has provided much needed relief to rollups by decreasing fees by 10x-100x, it remains to be seen how the 4844 blob market sustains ever increasing demand, without charging excessive fees.&#x20;

Enter Alt-DA layers. Alt-DA layers are services, mostly L1s that provide specialized DA services to chains. The main role of these chains is to provide dedicated bandwidth for rollups to publish and retrieve their data, at extremely low fees. Celestia, EigenDA, and Avail are a few examples of specialized DA layers.

## Why Celestia?

Spicenet uses Celestia as the DA layer of choice, because of a few reasons. We believe that DA layers should have a few key properties. First, they should offer high DA bandwidth for rollups to consume and use. Second, fees offered by the DA layer should be sustainably low, with less to no fee spikes. Third, rollups should have a pleasant, congestion-free base layer experience. Fourth, the DA layer should be sufficiently battle tested and also likely use DAS. We desire this property because Sovereign rollups inherit the security and liveness properties of the DA layer and DAS(via light clients) enables rollups themselves to securely verify and validate the DA layer state.

Celestia checks all of the boxes for us. Not only does it have sufficiently high data bandwidth(and plans to scale to 1GB blocks), it also charges very low fees to rollups. Third, congestion on Celestia is quite rare, because it does not feature a VM(like Spicenet) and does not explicitly execute transactions. Lastly, it is the first DA layer to go live on mainnet last year, and has a record of proven uptime.
