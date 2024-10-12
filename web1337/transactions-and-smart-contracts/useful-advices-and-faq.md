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

{% content-ref url="kly-wvm-deploy-and-interact-with-a-smart-contract.md" %}
[kly-wvm-deploy-and-interact-with-a-smart-contract.md](kly-wvm-deploy-and-interact-with-a-smart-contract.md)
{% endcontent-ref %}

## Check the reason of failed transaction

Sometimes it may happen that you will see a red Failed status next to your transaction. Like this:

<figure><img src="../../.gitbook/assets/image (38).png" alt=""><figcaption></figcaption></figure>

To find out the reason - click <mark style="color:blue;">**VIEW RAW DETAILS**</mark>. From here it will be possible to check why your transaction failed:

<figure><img src="../../.gitbook/assets/image (39).png" alt=""><figcaption></figcaption></figure>

