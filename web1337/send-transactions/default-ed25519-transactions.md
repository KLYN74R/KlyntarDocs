# 🔐 Default Ed25519 transactions

## Create and send default transaction

{% hint style="info" %}
Here we propose the example of simple transaction from default account(ed25519) to any other account
{% endhint %}

Also, read more about our default ed25519 accounts in MasteringKlyntar

{% embed url="https://mastering.klyntar.org/beginning/cryptography/key-pairs" %}

## Ed25519 => Ed25519 transaction

Example of simplest transaction - from default account to default account

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

#### **Result**

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

###

## Ed25519 => BLS(multisig address) transaction

A transfer transaction to a multisig address is literally 1 step more complicated. So, when you are going to send something to a multisig address, depending on whether the recipient's account already exists on the network, you need to specify an additional `rev_t` field that indicates the `reverse threshold`.&#x20;

#### What is reverse threshold?

Imagine that you and your 3 friends are going to manage some resources together, whether it be native KLY coins or some tokens. For this, it is obviously worth using a multisig address.

Let's assume that you agree that the decision is considered accepted if the voting threshold of 3/4 is reached (3 out of 4 friends agree to spend coins or call some kind of smart contract). So, if the threshold is 3/4, then the reverse threshold in this case will be 1 (because 4-3 = 1).

Here are some examples for other cases:

* Reverse threshold = 3 for a situation where you need 7/10 agreements
* Reverse threshold = 2 for a situation where you need 3/5 agreements

And so on

The `reverse threshold` was introduced in response to the ability of BLS signatures and public keys to aggregation. Since the situation is often such that

$$
T > T-N
$$

where:

* N is the number of sides of the multi-signature
* T is the threshold

and when checking the signature we need to know whether the threshold has been reached, then we need to do this:

* Present the consenting parties as an aggregated public key and an aggregated BLS signature
* In a separate array, present the public BLS keys of those who do not agree with the decision (or could not vote for some reason)

Going back to the 4 friends example, if we have a threshold of 3/4, then it makes more sense to aggregate 3 signatures and 3 public keys into 1 and separately present 1 public key of the one who disagrees than to provide 3 separate keys and signatures from 4.

### Let's look at a specific example

You and 3 your friends generate multisig pairs(public + private key) locally. Let's do it with Web1337:

```javascript
import {bls} from '@klyntar/web1337'

let privateKey1 = await bls.generatePrivateKey()
let publicKey1 = bls.derivePubKey(privateKey1)

let privateKey2 = await bls.generatePrivateKey()
let publicKey2 = bls.derivePubKey(privateKey2)

let privateKey3 = await bls.generatePrivateKey()
let publicKey3 = bls.derivePubKey(privateKey3)

let privateKey4 = await bls.generatePrivateKey()
let publicKey4 = bls.derivePubKey(privateKey4)


console.log(`Pair 1 => ${privateKey1} : ${publicKey1}`)
console.log(`Pair 2 => ${privateKey2} : ${publicKey2}`)
console.log(`Pair 3 => ${privateKey3} : ${publicKey3}`)
console.log(`Pair 4 => ${privateKey4} : ${publicKey4}`)
```

Output:

```code-runner-output
Pair 1 => 6bcdb86cd8f9d24e9fe934905431de5c224499af8788bf12cae8597f0af4cb23 : 6Pz5B3xLQKDxtnESqk3tWnsd4kwkVYLNsvgKJy31HssK3eGSy7Tvratk5pZNFW5z8S
Pair 2 => e5ca94e2c60cbb3ca9d5f9c233a99f01e3250ef62bd48fbc09caf25ae70040b5 : 5uj1Sgrvo9mwF6v8Z3Kxe4arQcu53Q2z9QBYsRPWCdo972jBeL8hD3Kmo4So3dSLt5
Pair 3 => ead8698b5ef285e6019e22e54dfe2c592c020946e803df8c6e79f98baf849d48 : 6ZmLf52hp5FgCxuaNJ6Jmi2M7FSJTr115WRMuV1yCvEihepnEKJBhgopDMpUKLpe2F
Pair 4 => 414230b72c59ac8db6ec47b629607057edaa5c7c553d44b4b9fd1c0090141c5a : 61tPxmio9Y21GtkiyYG3qTkreKfqm6ktk7MVk2hxqiXVURXxM6qNb9vPfvPPbhpMDn
```

Now, you need to aggregate your 4 public keys to get the root public key and receive payments for collective usage.

```javascript
let publicKey1 = '6Pz5B3xLQKDxtnESqk3tWnsd4kwkVYLNsvgKJy31HssK3eGSy7Tvratk5pZNFW5z8S'
let privateKey1 = '6bcdb86cd8f9d24e9fe934905431de5c224499af8788bf12cae8597f0af4cb23'

let publicKey2 = '5uj1Sgrvo9mwF6v8Z3Kxe4arQcu53Q2z9QBYsRPWCdo972jBeL8hD3Kmo4So3dSLt5'
let privateKey2 = 'e5ca94e2c60cbb3ca9d5f9c233a99f01e3250ef62bd48fbc09caf25ae70040b5'

let publicKey3 = '6ZmLf52hp5FgCxuaNJ6Jmi2M7FSJTr115WRMuV1yCvEihepnEKJBhgopDMpUKLpe2F'
let privateKey3 = 'ead8698b5ef285e6019e22e54dfe2c592c020946e803df8c6e79f98baf849d48'

let publicKey4 = '61tPxmio9Y21GtkiyYG3qTkreKfqm6ktk7MVk2hxqiXVURXxM6qNb9vPfvPPbhpMDn'
let privateKey4 = '414230b72c59ac8db6ec47b629607057edaa5c7c553d44b4b9fd1c0090141c5a'


let rootPubKey = bls.aggregatePublicKeys([publicKey1,publicKey2,publicKey3,publicKey4])

console.log('RootPubKey =>',rootPubKey)
```

Output:

```code-runner-output
RootPubKey => 68Bpgi6MbRX9q3T9h8DDWomPGu85HqWSfPMT23r6g29xyn1dN7qfquwxpfFNMdMpU1
```

Now you can accept payments for this address.&#x20;

## Ed25519 => TBLS(thresholdsig address) transaction

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





## Ed25519 => PostQuantum(Dilithium/BLISS) transaction

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



