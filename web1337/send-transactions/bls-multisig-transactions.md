# 🤝 BLS multisig transactions

## Useful links

{% embed url="https://mastering.klyntar.org/beginning/cryptography/multi-threshold-aggregated-signatures#bls12-381" %}
General information
{% endembed %}

{% embed url="https://mastering.klyntar.org/beginning/cryptography/multi-threshold-aggregated-signatures#demonstration" %}
Demonstration
{% endembed %}

Shortly, the typical keypair looks like this:

```javascript
{
   "privateKey":"caad28a7da57fb28879bfdd886714629e4152553a28540caed6f51531f5af61a",
   "pubKey":"7QMxLg6GJwQYjc8B2WBEvaQq3TfeBhgBqHGneFu43oLfwhovrPGykbkUGe94FacSuG"
}
```

* 48-bytes Base58 ecoded public key(used as address)
* 32-bytes hexadecimal private key
* 96-bytes Base64 encoded signature

### Generate BLS keypair(s)

{% hint style="success" %}
Read detailed information [here](default-ed25519-transactions.md#ed25519-greater-than-bls-multisig-address-transaction)
{% endhint %}

### BLS => Ed25519 transaction



### BLS => BLS(multisig address) transaction

### BLS => TBLS(thresholdsig address) transaction

### BLS => PostQuantum(Dilithium/BLISS) transaction

