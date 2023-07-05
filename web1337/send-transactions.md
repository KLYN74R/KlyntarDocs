---
description: Let's show you how to start with different types of KLY transactions
---

# 🟠 Send transactions

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

## Create and send default transaction

{% hint style="info" %}
Here we propose the example of simple transaction from default account(ed25519) to any other account
{% endhint %}

Also, read more about our default ed25519 accounts in MasteringKlyntar

{% embed url="https://mastering.klyntar.org/beginning/cryptography/key-pairs" %}

### Ed25519 => Ed25519 transaction

Example of simplest transaction - from default account to default account

{% code fullWidth="false" %}
```javascript
// Get your own keys using:
// Method 0(via Web1337) - await crypto.kly.generateDefaultEd25519Keypair()
// Method 1(via Apollo CLI) - apollo keygen

const myKeyPair = {

    mnemonic: 'south badge state hedgehog carpet aerobic float million enforce opinion hungry race',
    bip44Path: "m/44'/7331'/0'/0'",
    pub: '2VEzwUdvSRuv1k2JaAEaMiL7LLNDTUf9bXSapqccCcSb',
    prv: 'MC4CAQAwBQYDK2VwBCIEIDEf/4H5iiY3ebAfWsFIFkeZrB8HpcvBYK5zjEe9/8ga'
      
}


const subchain = '7GPupbq1vtKUgaqVeHiDbEJcxS7sSjwPnbht4eRaDBAEJv8ZKHNCSu2Am3CuWnHjta'

const recipient = 'nXSYHp74u88zKPiRi7t22nv4WCBHXUBpGrVw3V93f2s'

const from = myKeyPair.pub

const myPrivateKey = myKeyPair.prv

const nonce = 0

const fee = 1

const amountInKLY = 13.37


let signedTx = await web1337.createDefaultTransaction(subchain,from,myPrivateKey,nonce,recipient,fee,amountInKLY)

console.log(signedTx)
```
{% endcode %}

#### Result(see the structure)

```json5
{
  v: 0,
  creator: '2VEzwUdvSRuv1k2JaAEaMiL7LLNDTUf9bXSapqccCcSb',
  type: 'TX',
  nonce: 0,
  fee: 1,
  payload: {
    type: 'D',
    to: 'nXSYHp74u88zKPiRi7t22nv4WCBHXUBpGrVw3V93f2s',
    amount: 13.37
  },
  sig: '5AGkLlK3knzYZeZwjHKPzlX25lPMd7nU+rR5XG9RZa3sDpYrYpfnzqecm5nNONnl5wDcxmjOkKMbO7ulAwTFDQ=='
}
```

#### Send

```javascript
let txStatus = await web1337.sendTransaction(signedTx)

console.log(txStatus)

// After that - you can check the tx receipt
// TxID - it's a BLAKE3 hash of transaction signature
let receipt = await web1337.getTransactionReceiptById(web1337.BLAKE3(signedTx.sig))

console.log(receipt)
```

Result

```json5
{
    "blockID": "7GPupbq1vtKUgaqVeHiDbEJcxS7sSjwPnbht4eRaDBAEJv8ZKHNCSu2Am3CuWnHjta:4228",
    "id": 0,
    "isOk": true
}
```

where:

* **blockID** - the block where tx is
* **id** - index of this tx in block
* **isOk** - was this tx successfully processed or not



### Ed25519 => BLS(multisig address) transaction

A transfer transaction to a multisig address is literally 1 step more complicated. So, when you are going to send something to a multisig address, depending on whether the recipient's account already exists on the network, you need to specify an additional `rev_t` field that indicates the `reverse threshold`



### Ed25519 => TBLS(thresholdsig address) transaction

In this transaction you send something to TBLS root public key which controled by group of <mark style="color:red;">**`N`**</mark> members

```javascript
const myKeyPair = {

    mnemonic: 'south badge state hedgehog carpet aerobic float million enforce opinion hungry race',
    bip44Path: "m/44'/7331'/0'/0'",
    pub: '2VEzwUdvSRuv1k2JaAEaMiL7LLNDTUf9bXSapqccCcSb',
    prv: 'MC4CAQAwBQYDK2VwBCIEIDEf/4H5iiY3ebAfWsFIFkeZrB8HpcvBYK5zjEe9/8ga'
      
}


const subchain = '7GPupbq1vtKUgaqVeHiDbEJcxS7sSjwPnbht4eRaDBAEJv8ZKHNCSu2Am3CuWnHjta'

const recipientTblsRootPub = 'bedc88644f0deea4c0a77ba687712f494a1af7d8869f09768a0db42284f89d17b7b9225e0c87c2cb5511907dfd5eae3a53d789298721039e833770de29595880'

const from = myKeyPair.pub

const myPrivateKey = myKeyPair.prv

const nonce = 0

const fee = 1

const amountInKLY = 13.37


let signedTx = await web1337.createDefaultTransaction(subchain,from,myPrivateKey,nonce,recipientTblsRootPub,fee,amountInKLY)

console.log(signedTx)
```

### Ed25519 => PostQuantum(Dilithium/BLISS) transaction

In this transaction you send your assets to the BLAKE3 hash of public key of some post-quantum signatures schemes like DIlithium or BLISS (we support 2 algorithms)

```javascript
const myKeyPair = {

    mnemonic: 'south badge state hedgehog carpet aerobic float million enforce opinion hungry race',
    bip44Path: "m/44'/7331'/0'/0'",
    pub: '2VEzwUdvSRuv1k2JaAEaMiL7LLNDTUf9bXSapqccCcSb',
    prv: 'MC4CAQAwBQYDK2VwBCIEIDEf/4H5iiY3ebAfWsFIFkeZrB8HpcvBYK5zjEe9/8ga'
      
}


const subchain = '7GPupbq1vtKUgaqVeHiDbEJcxS7sSjwPnbht4eRaDBAEJv8ZKHNCSu2Am3CuWnHjta'

// It might be Dilithium or BLISS, but doesn't matter for you
const recipientPQC = 'f5091405e28455880fc4191cbda9f1e57f72399e732222d4639294b66d3a5076'

const from = myKeyPair.pub

const myPrivateKey = myKeyPair.prv

const nonce = 0

const fee = 1

const amountInKLY = 13.37


let signedTx = await web1337.createDefaultTransaction(subchain,from,myPrivateKey,nonce,recipientTblsRootPub,fee,amountInKLY)

console.log(signedTx)
```

##

## Create and send multisig transaction



Hello

{% embed url="https://mastering.klyntar.org/beginning/cryptography/multi-threshold-aggregated-signatures#bls12-381" %}

Hello

{% embed url="https://mastering.klyntar.org/beginning/cryptography/multi-threshold-aggregated-signatures#demonstration" %}

Hello

###

### BLS => Ed25519 transaction

### BLS => BLS(multisig address) transaction

### BLS => TBLS(thresholdsig address) transaction

### BLS => PostQuantum(Dilithium/BLISS) transaction



## Create and send  thresholdsig transaction

Hello

{% embed url="https://mastering.klyntar.org/beginning/cryptography/multi-threshold-aggregated-signatures#threshold-signatures-tbls" %}

Hello

{% embed url="https://mastering.klyntar.org/beginning/cryptography/multi-threshold-aggregated-signatures#threshold-signatures" %}

Hello

### TBLS => Ed25519 transaction

### TBLS => BLS(multisig address) transaction

### TBLS => TBLS(thresholdsig address) transaction

### TBLS => PostQuantum(Dilithium/BLISS) transaction



## Create and send transaction from post-quantum account

{% hint style="info" %}
PQC - Post-quantum Cryptography
{% endhint %}

Hello

{% embed url="https://mastering.klyntar.org/beginning/cryptography/post-quantum-cryptography" %}

Hello

### PQC => Ed25519 transaction

### PQC => BLS(multisig address) transaction

### PQC => TBLS(thresholdsig address) transaction

### PQC => PostQuantum(Dilithium/BLISS) transaction

##
