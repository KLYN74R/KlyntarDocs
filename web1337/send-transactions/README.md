---
description: Let's show you how to start with different types of KLY transactions
---

# 🟠 Send transactions

## Intro

KLY has 4 types of accounts:

* Ed25519 - default
* BLS - for multisig
* TBLS - threshold signatures
* PostQuantum - Dilithium / BLISS

{% hint style="info" %}
Due to the peculiarities of each of them, we decided to show how to conduct transactions from any type to any other (4x4, total - 16 examples)
{% endhint %}

## Create Web1337 instance

It's initial step - as previously

```javascript
import Web1337 from '@klyntar/web1337'


let web1337 = new Web1337({

    symbioteID:'aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa',
    workflowVersion:0,
    nodeURL: 'http://localhost:7332'

});
```

***

## Links

{% embed url="https://klyntar.medium.com/cryptoland-part-1-types-of-addresses-on-klyntar-post-quantum-multisig-tbls-ed25519-981277963ced" %}
