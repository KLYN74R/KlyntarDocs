# Epoch API

An epoch is a certain period of time in which the network operates.

Each epoch, the network assigns a new quorum, new sequences of shard leaders, and so on.

Use this list of APIs to get information about epoch data.&#x20;

## Get current epoch on threads

In our system, the threads of block confirmation and their processing (i.e. parsing transactions and executing them to change the state) work <mark style="color:red;">**independently of each other**</mark>.

This is logical - the network in one thread votes for accepted blocks, and in another thread - simply searches for the next block and proofs of approval of this block for its processing.

Since these tasks are asynchronous, the epochs on the threads may differ. Most of the time they will be the same, but sometimes they may still differ

To request the current epoch on the threads, call:

#### Verification Thread

```javascript
await web1337.getCurrentEpochOnThread("vt");
```

#### Approvement Thread

```javascript
await web1337.getCurrentEpochOnThread("at");
```

## Get current leaders on shards

Shard leaders are those who are currently generating blocks for a particular shard.

Each epoch, each shard is assigned a list of shard leaders who will receive their own window for generating blocks for a certain time.

To find out who is currently generating blocks on each shard, use this API

```javascript
await web1337.getCurrentLeadersOnShards();
```

## Get epoch data by epoch index

You can find out the current epoch from the API examples above. You can also find out the index of the current epoch there.

However, if you want to study the historical data of the network, you can request data on a certain epoch by its index.

For example, to request data on the very first epoch, call this API:

<pre class="language-javascript"><code class="lang-javascript"><strong>let epochIndex = 0;
</strong><strong>
</strong><strong>await web1337.getEpochDataByEpochIndex(epochIndex);
</strong></code></pre>

## Get total number of blocks, transactions and successful transactions by epoch index

This API returns stats for specific epoch. You can use this API, for example, for time series charts. It's also useful for metrics and stats tracking:

```javascript
let epochIndex = 196;

await web1337.getTotalBlocksAndTxsStatsPerEpoch();
```
