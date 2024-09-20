# Hostchain checkpoints

**Hostchains** are independent L0-L1 blockchains that rely on their own security, fault tolerance and decentralization.

> ### We use their <mark style="color:red;">TOTAL POWER</mark> as the maximum source of liquidity and decentralization

Problem: PoS/BFT blockchains have an attack called a long-range attack.

Its essence is that when you start synchronizing your node with the current state of the blockchain, you have to start checking blocks 0,1,2,... and so on. At the same time, you need to take these blocks from somewhere and here is the problem - how to make sure which block is valid?

If, say, block 1337th was generated and confirmed by the network more than 3 years ago, then with a high degree of probability those old validators are no longer active and may have even removed their shares from staking. A problem arises - you cannot clearly determine which block with index 1337 was correct - anyone can offer you any valid fork.

Solution: In order for you to be able to check the relevance of your copy of the blockchain, the current quorum (current validators who have frozen their stake and who can be trusted because they have something to risk) makes "checkpoints" to other blockchains once per epoch (or with other frequency).

A checkpoint is a fact that looks like this, for example:

{ epoch: 167, shard: shard\_0, lastShardLeaderPubkey: 27, lastBlockIndexByShardLeader: 56, lastBlockHashByShardLeader: 'aaaa' }

If such a message is saved on many blockchains, then even after many years it can be trusted, because in order to potentially change it, attackers would need to compromise most of the blockchains.

TLDR - having found such a message in 2/3 of the blockchains that we will use to maintain checkpoints, you can be confident about the validity of your own copy of the state + the network blockchain.

<figure><img src="../../../../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

