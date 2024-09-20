# Your own custom explorer

## Intro

During development, it is often useful to visualize changes in the chain.

For example, when launching a private testnet, it would be interesting to know the details of the network operation - created blocks, transaction statuses, validators, shards, and so on.

That is why we decided to make our explorer open source. But in general, there are two reasons:

1. The ability to make changes to the common repository by different developers
2. The ability to simplify the process of development and interaction with the Klyntar network

Therefore, below you will see the process of launching a local explorer and a short guide on using it using the example of a local testnet with 1 shard and 1 validator

<figure><img src="../../.gitbook/assets/Thumbnail.png" alt=""><figcaption></figcaption></figure>

## Installation process

Install **Node.js** first

Then, clone repository

```bash
git clone https://github.com/KLYN74R/Explorer.git
```

Then, go to repository and install dependencies

```bash
cd Explorer && npm install
```

## Options

Now, if you need just explorer to view network - build it and run

```bash
npm run build

npm start
```

In case you want to make changes to explorer and track changes - run it in development mode

```bash
npm run dev
```

## Environmental variables

By default, explorer expects a URL for node RPC. This is how it looks like in code&#x20;

```typescript
const baseUrl = process.env.KLYNTAR_NODE_URL || 'http://localhost:7332';
```

So, set `KLYNTAR_NODE_URL` manually in Linux/Windows/Mac or just run your node on port 7332

## Usage

Go to [http://localhost:3000](http://localhost:3000) and check the main page of explorer

<figure><img src="../../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

There are 3 main areas you should pay attention to here:

1. **Searchbar**
2. **Network Parameters** section
3. **Network Info** - section with starting points to other pages that detail the explorer

## Network Parameters

This is the simplest section that will tell you about the current network parameters

<figure><img src="../../.gitbook/assets/image (10).png" alt=""><figcaption></figcaption></figure>

These parameters are specified in the genesis and they regulate the operation of the network. Here you will find information about the network ID, the bet size for the validator, the duration of the epoch, and so on.

So, for example, if you look at genesis, you will see similar indicators - they will be displayed in the interface.

```json
"NETWORK_PARAMETERS":{

        "VALIDATOR_STAKE":50000,
        "MINIMAL_STAKE_PER_ENTITY":20,
        "UNSTAKING_PERIOD":1,
        "QUORUM_SIZE":127,
        "EPOCH_TIME":86400000,
        "LEADERSHIP_TIMEFRAME":120000,
        "BLOCK_TIME":1000,
        "MAX_BLOCK_SIZE_IN_BYTES":12288000,
        "TXS_LIMIT_PER_BLOCK":30000,
        "EPOCH_EDGE_TRANSACTIONS_LIMIT_PER_BLOCK":500,
        "MAX_NUM_OF_BLOCKS_PER_SHARD_FOR_SYNC_OPS":1
    
}
```

## Searchbar

In searchbar you can find everything you need by applying a special filter. Please, click the button to expand the full list of available filters

<figure><img src="../../.gitbook/assets/image (11).png" alt=""><figcaption><p>Full list of filters</p></figcaption></figure>

Follow the prompts in the input field to find what you need :relaxed: We'll come back to the searchbar later.

## Network Info

In order to provide maximum detail, we decided to create separate pages for different data. Available sections are highlighted, and those that will be available soon are blocked for now.

<figure><img src="../../.gitbook/assets/image (12).png" alt=""><figcaption></figcaption></figure>

## Check the checkpoints to hostchain

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

<figure><img src="../../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

## Check the current epoch data

Go to page \`Epoches data\`

<figure><img src="../../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

Here you can get more details about current epoch. Using arrow buttons it's possible to track the history of epoches.

Below you can also check the list of validators and selector of shards. By changing the shard it will be possible to check assigned block creators to shards

<figure><img src="../../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

## Get more info about validator(a.k.a staking pool)

By clicking on validator above - you will be redirected to page about this pool

<figure><img src="../../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

Get the list of stakers and explore their accounts

<figure><img src="../../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>
