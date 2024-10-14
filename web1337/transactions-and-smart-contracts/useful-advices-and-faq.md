---
icon: messages-question
---

# Useful advices & FAQ

## How to calculate the TxID locally

TxID is a unique identifier of transaction. For our native tx format the tx id is a

> **BLAKE3 hash of transaction signature - 256 bit in length, hex-encoded**

Code snippet:

```javascript
let txID = web1337.blake3(tx.sig)
```

See also [#find-your-transaction-by-tx-id](../../build-core-and-join-network/explorers-and-how-to-use-them/usage-guide/searchbar.md#find-your-transaction-by-tx-id "mention")

## How to calculate the contractID locally to predict the future contract identifier

```javascript
const contractID = web1337.blake3(shardID+tx.creator+tx.nonce)
```

See:

{% content-ref url="kly-wvm-deploy-and-interact-with-a-smart-contract/" %}
[kly-wvm-deploy-and-interact-with-a-smart-contract](kly-wvm-deploy-and-interact-with-a-smart-contract/)
{% endcontent-ref %}

## Check the reason of failed transaction

Sometimes it may happen that you will see a red Failed status next to your transaction. Like this:

<figure><img src="../../.gitbook/assets/image (38).png" alt=""><figcaption></figcaption></figure>

To find out the reason - click <mark style="color:blue;">**VIEW RAW DETAILS**</mark>. From here it will be possible to check why your transaction failed:

<figure><img src="../../.gitbook/assets/image (39).png" alt=""><figcaption></figcaption></figure>

## How to find account(user or contract - both for WVM and EVM) in explorer

Remember that Klyntar has a sharded structure, so the full ID of your account should also have prefix with **shard**.

For example: you have account \`

```json
EGU4u3Anwahbtbx8F1ZZgFQSg2u49EkrkqMERT9r3q1o
```

\`To find it on shard 0, try to search for:

```json
shard_0:EGU4u3Anwahbtbx8F1ZZgFQSg2u49EkrkqMERT9r3q1o
```

In general - to find account on **`shard_x`** - just use template:

```json
shard_x:EGU4u3Anwahbtbx8F1ZZgFQSg2u49EkrkqMERT9r3q1o
```

{% hint style="info" %}
See [#find-account-info-by-id](../../build-core-and-join-network/explorers-and-how-to-use-them/usage-guide/searchbar.md#find-account-info-by-id "mention")
{% endhint %}

