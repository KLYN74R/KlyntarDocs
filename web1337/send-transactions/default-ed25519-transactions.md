---
description: Solana compatible accounts
---

# 🔐 Default Ed25519 transactions

## Create and send default transaction

{% hint style="info" %}
Here we propose the example of simple transaction from default account(ed25519) to any other account
{% endhint %}

Also, read more about our default ed25519 accounts in MasteringKlyntar

{% embed url="https://mastering.klyntar.org/beginning/cryptography/key-pairs" %}

<figure><img src="../../.gitbook/assets/image (2) (1).png" alt=""><figcaption></figcaption></figure>

## Generate keypair

```javascript
import {crypto} from '@klyntar/web1337';

let keypair = await crypto.ed25519.generateDefaultEd25519Keypair();

console.log(keypair);
```

Output:

```json5
{
  mnemonic: 'job october hold grape novel horror stay major pledge bonus energy fringe',
  bip44Path: "m/44'/7331'/0'/0'",
  pub: 'GbjtpGjt1G9pJe667cQcBswRMGq9XogmrGEGUj94enuc',
  prv: 'MC4CAQAwBQYDK2VwBCIEIMOJVCiaURXxlZrkXe+fa2r061eqdOQAxux1/gxDGRi9'
}
```

## Tricks with BIP-44

By default, you get the new mnemonic and key pair with `m/44'/7331'/0'/0'` path. But, to generate many accounts from a single seed do this:

* Get the mnemonic from the first generation
* Change the path to 1,2,3... to build the HD chain of accounts

For example:

Get the first keypair in future chain:

```javascript
import {crypto} from '@klyntar/web1337';


const mnemonic = null;

const bip44Path = null;

const mnemoPassword = 'HelloKlyntar';


let keypair = await crypto.ed25519.generateDefaultEd25519Keypair(mnemonic,bip44Path,mnemoPassword);

console.log(keypair)
```

Output:

```json5
{
  mnemonic: 'final lottery shell supply lottery doll drive flavor awesome tool matter argue',
  bip44Path: "m/44'/7331'/0'/0'",
  pub: '5oFCA179BeABvcUx921adoU4N9mXGGGS6DaoAwcTgFzs',
  prv: 'MC4CAQAwBQYDK2VwBCIEIB5ghaD82U+RixQ9KuGtFwADQu1FMVl4dTWs1zd094Q2'
}
```

For the first pair in future chain of accounts we don't set the mnemonic and BIP-44 path. Mnemonic will be randomly generated and path will be `m/44'/7331'/0'/0'`. The last parameter is the password that will be used to get the seed from your mnemonic.

{% hint style="danger" %}
**That's why - choose the password with the high entropy, ommiting typical passwords from well known wordlists to make brute force impossible. Also, **<mark style="color:red;">**DON'T SHARE YOUR MNEMONIC PHRASE - YOU WILL LOST CONTROL OF YOUR ACCOUNT**</mark>**.**
{% endhint %}

Now, to build the chain, use this snippet:

```javascript
import {crypto} from '@klyntar/web1337';


const firstKeypairInChain = {
    mnemonic: 'final lottery shell supply lottery doll drive flavor awesome tool matter argue',
    bip44Path: "m/44'/7331'/0'/0'",
    pub: '5oFCA179BeABvcUx921adoU4N9mXGGGS6DaoAwcTgFzs',
    prv: 'MC4CAQAwBQYDK2VwBCIEIB5ghaD82U+RixQ9KuGtFwADQu1FMVl4dTWs1zd094Q2'
};

console.log('0 in chain => ',await crypto.ed25519.generateDefaultEd25519Keypair(firstKeypairInChain.mnemonic,firstKeypairInChain.bip44Path,'HelloKlyntar'));
console.log('1 in chain => ',await crypto.ed25519.generateDefaultEd25519Keypair(firstKeypairInChain.mnemonic,"m/44'/7331'/0'/1'",'HelloKlyntar'));
console.log('2 in chain => ',await crypto.ed25519.generateDefaultEd25519Keypair(firstKeypairInChain.mnemonic,"m/44'/7331'/0'/2'",'HelloKlyntar'));
```

Output:

```code-runner-output
0 in chain =>  {
  mnemonic: 'final lottery shell supply lottery doll drive flavor awesome tool matter argue',
  bip44Path: "m/44'/7331'/0'/0'",
  pub: '5oFCA179BeABvcUx921adoU4N9mXGGGS6DaoAwcTgFzs',
  prv: 'MC4CAQAwBQYDK2VwBCIEIB5ghaD82U+RixQ9KuGtFwADQu1FMVl4dTWs1zd094Q2'
}
1 in chain =>  {
  mnemonic: 'final lottery shell supply lottery doll drive flavor awesome tool matter argue',
  bip44Path: "m/44'/7331'/0'/1'",
  pub: '6GSUoBos7P3ZZLeq5jjH4AJ8vvVMJQHqGVh1ZsH8FeGF',
  prv: 'MC4CAQAwBQYDK2VwBCIEILp+iBVOXkbVy4J3Dbw8w22bJ+TNXHyRmvYIeQcBHl+f'
}
2 in chain =>  {
  mnemonic: 'final lottery shell supply lottery doll drive flavor awesome tool matter argue',
  bip44Path: "m/44'/7331'/0'/2'",
  pub: '8DZ92AyXjfyRYhWJB1rxHy38bYwtm1bW6xz3sFkLAkUU',
  prv: 'MC4CAQAwBQYDK2VwBCIEIJXTGksc61wuV1rB2YjCkgCsp0RPoN8pJ1slCmQL0igU'
}
```

## Ed25519 => Ed25519 transaction

Example of simplest transaction - from default account to default account

```javascript
// Get your own keys using:
// Method 0(via Web1337) - await crypto.ed25519.generateDefaultEd25519Keypair()
// Method 1(via Apollo CLI) - apollo keygen

const myKeyPair = {

    mnemonic: 'south badge state hedgehog carpet aerobic float million enforce opinion hungry race',
    bip44Path: "m/44'/7331'/0'/0'",
    pub: '2VEzwUdvSRuv1k2JaAEaMiL7LLNDTUf9bXSapqccCcSb',
    prv: 'MC4CAQAwBQYDK2VwBCIEIDEf/4H5iiY3ebAfWsFIFkeZrB8HpcvBYK5zjEe9/8ga'
      
};


const subchain = '7GPupbq1vtKUgaqVeHiDbEJcxS7sSjwPnbht4eRaDBAEJv8ZKHNCSu2Am3CuWnHjta';

const recipient = 'nXSYHp74u88zKPiRi7t22nv4WCBHXUBpGrVw3V93f2s';

const from = myKeyPair.pub;

const myPrivateKey = myKeyPair.prv;

const nonce = 0;

const fee = 1;

const amountInKLY = 13.37;


let signedTx = await web1337.createDefaultTransaction(subchain,from,myPrivateKey,nonce,recipient,fee,amountInKLY);

console.log(signedTx);
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
let txStatus = await web1337.sendTransaction(signedTx);

console.log(txStatus);

// After that - you can check the tx receipt
// TxID - it's a BLAKE3 hash of transaction signature
let receipt = await web1337.getTransactionReceiptById(web1337.BLAKE3(signedTx.sig));

console.log(receipt);
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

let privateKey1 = await bls.generatePrivateKey();
let publicKey1 = bls.derivePubKey(privateKey1);

let privateKey2 = await bls.generatePrivateKey();
let publicKey2 = bls.derivePubKey(privateKey2);

let privateKey3 = await bls.generatePrivateKey();
let publicKey3 = bls.derivePubKey(privateKey3);

let privateKey4 = await bls.generatePrivateKey();
let publicKey4 = bls.derivePubKey(privateKey4);


console.log(`Pair 1 => ${privateKey1} : ${publicKey1}`);
console.log(`Pair 2 => ${privateKey2} : ${publicKey2}`);
console.log(`Pair 3 => ${privateKey3} : ${publicKey3}`);
console.log(`Pair 4 => ${privateKey4} : ${publicKey4}`);
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
let publicKey1 = '6Pz5B3xLQKDxtnESqk3tWnsd4kwkVYLNsvgKJy31HssK3eGSy7Tvratk5pZNFW5z8S';
let privateKey1 = '6bcdb86cd8f9d24e9fe934905431de5c224499af8788bf12cae8597f0af4cb23';

let publicKey2 = '5uj1Sgrvo9mwF6v8Z3Kxe4arQcu53Q2z9QBYsRPWCdo972jBeL8hD3Kmo4So3dSLt5';
let privateKey2 = 'e5ca94e2c60cbb3ca9d5f9c233a99f01e3250ef62bd48fbc09caf25ae70040b5';

let publicKey3 = '6ZmLf52hp5FgCxuaNJ6Jmi2M7FSJTr115WRMuV1yCvEihepnEKJBhgopDMpUKLpe2F';
let privateKey3 = 'ead8698b5ef285e6019e22e54dfe2c592c020946e803df8c6e79f98baf849d48';

let publicKey4 = '61tPxmio9Y21GtkiyYG3qTkreKfqm6ktk7MVk2hxqiXVURXxM6qNb9vPfvPPbhpMDn';
let privateKey4 = '414230b72c59ac8db6ec47b629607057edaa5c7c553d44b4b9fd1c0090141c5a';


let rootPubKey = bls.aggregatePublicKeys([publicKey1,publicKey2,publicKey3,publicKey4]);

console.log('RootPubKey =>',rootPubKey);
```

Output:

```code-runner-output
RootPubKey => 68Bpgi6MbRX9q3T9h8DDWomPGu85HqWSfPMT23r6g29xyn1dN7qfquwxpfFNMdMpU1
```

Now you can accept payments for this address.

### From sender point of view

When you need to send something to multisig account you need to set the reverse threshold if account still not exists or use \`rev\_t\` property of already existed account

So, if account `68Bpgi6MbRX9q3T9h8DDWomPGu85HqWSfPMT23r6g29xyn1dN7qfquwxpfFNMdMpU1` still not in state - use this template:

```javascript
const myKeyPair = {

    mnemonic: 'south badge state hedgehog carpet aerobic float million enforce opinion hungry race',
    bip44Path: "m/44'/7331'/0'/0'",
    pub: '2VEzwUdvSRuv1k2JaAEaMiL7LLNDTUf9bXSapqccCcSb',
    prv: 'MC4CAQAwBQYDK2VwBCIEIDEf/4H5iiY3ebAfWsFIFkeZrB8HpcvBYK5zjEe9/8ga'
      
};


const subchain = '7GPupbq1vtKUgaqVeHiDbEJcxS7sSjwPnbht4eRaDBAEJv8ZKHNCSu2Am3CuWnHjta';

const recipient = '68Bpgi6MbRX9q3T9h8DDWomPGu85HqWSfPMT23r6g29xyn1dN7qfquwxpfFNMdMpU1';

const from = myKeyPair.pub;

const myPrivateKey = myKeyPair.prv;

const nonce = 0;

const fee = 1;

const amountInKLY = 13.37;

// In our example with 4 friends, since we want 3/4 agreements
// to use account, the reverse threshold will be 4-3=1
// Use the formula rev_t = N-T where N - number of sides, T-threshold
const reverseThreshold = 1;

let signedTx = await web1337.createDefaultTransaction(subchain,from,myPrivateKey,nonce,recipient,fee,amountInKLY,reverseThreshold);

console.log(signedTx);
```

Result:

```json5
{
  v: 0,
  creator: '2VEzwUdvSRuv1k2JaAEaMiL7LLNDTUf9bXSapqccCcSb',
  type: 'TX',
  nonce: 0,
  fee: 1,
  payload: {
    type: 'D',
    to: '68Bpgi6MbRX9q3T9h8DDWomPGu85HqWSfPMT23r6g29xyn1dN7qfquwxpfFNMdMpU1',
    amount: 13.37,
    rev_t: 1
  },
  sig: 'VxL7dEsA+TZRVkj2jn3LBH7D8wuyyseDYkPptEicRHXJ9vIDSfjxYBiz1GCIESIXSWZKlcOx5Ld2hK1FaWsLDw=='
}
```

After this transaction, new account will be added to state:

```json
"68Bpgi6MbRX9q3T9h8DDWomPGu85HqWSfPMT23r6g29xyn1dN7qfquwxpfFNMdMpU1":{
    "type":"account",
    "balance":13.37,
    "uno":0,
    "nonce":0,
    "rev_t":1,
    "subchain":"7GPupbq1vtKUgaqVeHiDbEJcxS7sSjwPnbht4eRaDBAEJv8ZKHNCSu2Am3CuWnHjta"
}
```

As you see, new multisig account is created and binded to subchain where sender sends KLY. Also, the `rev_t` is set to 1 what means that the number of dissenting sides can be 1.



In case account was already in state - get the `rev_t` from information about account:

```javascript
const subchain = '7GPupbq1vtKUgaqVeHiDbEJcxS7sSjwPnbht4eRaDBAEJv8ZKHNCSu2Am3CuWnHjta';

const recipient = '68Bpgi6MbRX9q3T9h8DDWomPGu85HqWSfPMT23r6g29xyn1dN7qfquwxpfFNMdMpU1';

let accountInfo  = await web1337.getFromState(subchain,recipient);

console.log('Account:\n\n',accountInfo);
```

```
Account

"68Bpgi6MbRX9q3T9h8DDWomPGu85HqWSfPMT23r6g29xyn1dN7qfquwxpfFNMdMpU1":{
    "type":"account",
    "balance":123456,
    "uno":0,
    "nonce":0,
    "rev_t":1,
    "subchain":"7GPupbq1vtKUgaqVeHiDbEJcxS7sSjwPnbht4eRaDBAEJv8ZKHNCSu2Am3CuWnHjta"
}
```

And then, use the value of `rev_t` to build the transaction as above

##

## Ed25519 => TBLS(thresholdsig address) transaction

In this transaction you send something to TBLS root public key which controled by group of <mark style="color:red;">**`N`**</mark> members

{% hint style="info" %}
We'll talk more about TBLS in the next parts. Just now you need to know the 96-byte root public key
{% endhint %}

```javascript
const myKeyPair = {

    mnemonic: 'south badge state hedgehog carpet aerobic float million enforce opinion hungry race',
    bip44Path: "m/44'/7331'/0'/0'",
    pub: '2VEzwUdvSRuv1k2JaAEaMiL7LLNDTUf9bXSapqccCcSb',
    prv: 'MC4CAQAwBQYDK2VwBCIEIDEf/4H5iiY3ebAfWsFIFkeZrB8HpcvBYK5zjEe9/8ga'
      
};


const subchain = '7GPupbq1vtKUgaqVeHiDbEJcxS7sSjwPnbht4eRaDBAEJv8ZKHNCSu2Am3CuWnHjta';

const recipientTblsRootPub = 'bedc88644f0deea4c0a77ba687712f494a1af7d8869f09768a0db42284f89d17b7b9225e0c87c2cb5511907dfd5eae3a53d789298721039e833770de29595880';

const from = myKeyPair.pub;

const myPrivateKey = myKeyPair.prv;

const nonce = 0;

const fee = 1;

const amountInKLY = 13.37;


let signedTx = await web1337.createDefaultTransaction(subchain,from,myPrivateKey,nonce,recipientTblsRootPub,fee,amountInKLY);

console.log(signedTx);
```





## Ed25519 => PostQuantum(Dilithium/BLISS) transaction

In this transaction you send your assets to the BLAKE3 hash of public key of some post-quantum signatures schemes like DIlithium or BLISS (we support 2 algorithms)

{% hint style="info" %}
We'll talk about PQC accounts later. Just now, as a sender you just need to know only the  address of recipient - it's 256-bit hash of public key
{% endhint %}

```javascript
const myKeyPair = {

    mnemonic: 'south badge state hedgehog carpet aerobic float million enforce opinion hungry race',
    bip44Path: "m/44'/7331'/0'/0'",
    pub: '2VEzwUdvSRuv1k2JaAEaMiL7LLNDTUf9bXSapqccCcSb',
    prv: 'MC4CAQAwBQYDK2VwBCIEIDEf/4H5iiY3ebAfWsFIFkeZrB8HpcvBYK5zjEe9/8ga'
      
};


const subchain = '7GPupbq1vtKUgaqVeHiDbEJcxS7sSjwPnbht4eRaDBAEJv8ZKHNCSu2Am3CuWnHjta';

// It might be Dilithium or BLISS, but doesn't matter for you
const recipientPQC = 'f5091405e28455880fc4191cbda9f1e57f72399e732222d4639294b66d3a5076';

const from = myKeyPair.pub;

const myPrivateKey = myKeyPair.prv;

const nonce = 0;

const fee = 1;

const amountInKLY = 13.37;


let signedTx = await web1337.createDefaultTransaction(subchain,from,myPrivateKey,nonce,recipientTblsRootPub,fee,amountInKLY);

console.log(signedTx);
```



