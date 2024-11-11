---
description: Solana compatible accounts
---

# 🔐 Default Ed25519 transactions

<figure><img src="../../.gitbook/assets/image (5) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

## Create and send default transaction

{% hint style="info" %}
Here we propose the example of simple transaction from default account(ed25519) to any other account
{% endhint %}

## Keypair creation process

<figure><img src="../../.gitbook/assets/image (2) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

## Generate keypair

{% tabs %}
{% tab title="Node.js" %}
```javascript
import {crypto} from 'web1337';

let mnemonic = ""; // mnemonic should be empty in case you generate a new pair

let mnemonicPassword = "HelloKlyntar"; // set the password for your mnemonic

let bip44Path = [44,7331,0,0]; // 7331 is KLY ID and the next values - derivation path

let keypair = await crypto.ed25519.generateDefaultEd25519Keypair(mnemonic,mnemonicPassword,bip44Path);

console.log(JSON.parse(keypair));
```
{% endtab %}
{% endtabs %}

Output:

```json5
{
  mnemonic: 'refuse enroll glance tree laptop hill void lobster oxygen regular salad hour ice knife lazy gap decrease hundred garden waste move hundred kitchen tissue',
  bip44Path: [ 44, 7331, 0, 0 ],
  pub: 'n2XZqcPUeXjcn9Gt5iAT4aEx8uLTKwThTPNWf8gAgbS',
  prv: 'MC4CAQAwBQYDK2VwBCIEIH7Pttf8d85IAkB0M3c2BcEthzckDnSSq4Gg8NsQI1xj'
}
```

## Tricks with BIP-44

By default, you get the new mnemonic and key pair with `m/44'/7331'/0'/0'` path. But, to generate many accounts from a single seed do this:

* Get the mnemonic from the first generation
* Change the path to 1,2,3... to build the HD chain of accounts

For example:

Get the first keypair in future chain:

{% tabs %}
{% tab title="JavaScript" %}
```javascript
import {crypto} from 'web1337';

let mnemonic = ""; // mnemonic should be empty in case you generate a new pair

let mnemonicPassword = "HelloKlyntar"; // DO NOT USE IN REAL USE CASES

let bip44Path = [44,7331,0,0]; // 7331 is KLY ID and the next values - derivation path

let keypair = await crypto.ed25519.generateDefaultEd25519Keypair(mnemonic,mnemonicPassword,bip44Path);

console.log(JSON.parse(keypair));
```
{% endtab %}
{% endtabs %}

Output:

```json5
{
  mnemonic: 'slam lab chief wire inquiry transfer trigger object segment jeans lawn trumpet tuition amount penalty provide frost tortoise display shiver desert vintage erase vague',
  bip44Path: [ 44, 7331, 0, 0 ],
  pub: 'E64Sp6NQsHHYhujdrTvGj8dkkhu9ZiDubddm7TW6NmYP',
  prv: 'MC4CAQAwBQYDK2VwBCIEIMWqtM5c1FAUA4wtb26RUlAUqNYTEE7ALLcw6KvJELrJ'
}
```

For the first pair in future chain of accounts we don't set the mnemonic and BIP-44 path. Mnemonic will be randomly generated and path will be `m/44'/7331'/0'/0'`. The last parameter is the password that will be used to get the seed from your mnemonic.

{% hint style="danger" %}
**That's why - choose the password with the high entropy, ommiting typical passwords from well known wordlists to make brute force impossible. Also, **<mark style="color:red;">**DON'T SHARE YOUR MNEMONIC PHRASE - YOU WILL LOST CONTROL OF YOUR ACCOUNT**</mark>**.**
{% endhint %}

Now, to build the chain, use this snippet:

```javascript
import {crypto} from 'web1337';


const firstKeypairInChain = {
  mnemonic: 'slam lab chief wire inquiry transfer trigger object segment jeans lawn trumpet tuition amount penalty provide frost tortoise display shiver desert vintage erase vague',
  bip44Path: [ 44, 7331, 0, 0 ],
  pub: 'E64Sp6NQsHHYhujdrTvGj8dkkhu9ZiDubddm7TW6NmYP',
  prv: 'MC4CAQAwBQYDK2VwBCIEIMWqtM5c1FAUA4wtb26RUlAUqNYTEE7ALLcw6KvJELrJ'
};


console.log('0 in chain => ',JSON.parse(await crypto.ed25519.generateDefaultEd25519Keypair(firstKeypairInChain.mnemonic,'HelloKlyntar',[44,7331,0,0])));
console.log('1 in chain => ',JSON.parse(await crypto.ed25519.generateDefaultEd25519Keypair(firstKeypairInChain.mnemonic,'HelloKlyntar',[44,7331,0,1])));
console.log('2 in chain => ',JSON.parse(await crypto.ed25519.generateDefaultEd25519Keypair(firstKeypairInChain.mnemonic,'HelloKlyntar',[44,7331,0,2])));
```

Output:

```code-runner-output
0 in chain =>  {
  mnemonic: 'slam lab chief wire inquiry transfer trigger object segment jeans lawn trumpet tuition amount penalty provide frost tortoise display shiver desert vintage erase vague',
  bip44Path: [ 44, 7331, 0, 0 ],
  pub: 'E64Sp6NQsHHYhujdrTvGj8dkkhu9ZiDubddm7TW6NmYP',
  prv: 'MC4CAQAwBQYDK2VwBCIEIMWqtM5c1FAUA4wtb26RUlAUqNYTEE7ALLcw6KvJELrJ'
}
1 in chain =>  {
  mnemonic: 'slam lab chief wire inquiry transfer trigger object segment jeans lawn trumpet tuition amount penalty provide frost tortoise display shiver desert vintage erase vague',
  bip44Path: [ 44, 7331, 0, 1 ],
  pub: 'HmLyx89V2mif1H4GNJVDEodH5kw7r78h3tfymCqCBFri',
  prv: 'MC4CAQAwBQYDK2VwBCIEIEaZiwgvgvf0naUUHfM6OidMbn6o+SuDDsgEl/NHcIH+'
}
2 in chain =>  {
  mnemonic: 'slam lab chief wire inquiry transfer trigger object segment jeans lawn trumpet tuition amount penalty provide frost tortoise display shiver desert vintage erase vague',
  bip44Path: [ 44, 7331, 0, 2 ],
  pub: '2uAts9XG7dvBj3zuLyEGFFf6315jKPKouJeX8b9bYhd7',
  prv: 'MC4CAQAwBQYDK2VwBCIEII6I6eA9G6LjHvGKlwYJTsjs1QYCvsA5g14JFGAEVx3y'
}
```

## Ed25519 => Ed25519 transaction

Example of simplest transaction - from default account to default account

```javascript
let web1337 = new Web1337({

    chainID:'aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa',
    workflowVersion:0,
    nodeURL: 'http://localhost:7332'

}); // need node endpoint to return the correct nonce. If you know your nonce - you can omit it

const keypair = {
    
    pub:"9GQ46rqY238rk2neSwgidap9ww5zbAN4dyqyC7j5ZnBK",
    
    prv:"MC4CAQAwBQYDK2VwBCIEILdhTMVYFz2GP8+uKUA+1FnZTEdN8eHFzbb8400cpEU9",

};

const shardID = "shard_0";

const payload = {

    to: "Ai1Pk9RzmgeiSAptc1W69NAmSB52GqjNqGdGQGgqxQA1",

    amount: 13.37,
    
    shard: shardID

};

const fee = 0.03;

const nonce = await web1337.getAccount(shardID,keypair.pub).then(account=>account.nonce+1);

const txType = "TX";

let tx = web1337.createEd25519Transaction(shardID,txType,keypair.pub,keypair.prv,nonce,fee,payload);

console.log(tx);
```

#### Result(see the structure)

```json5
{
  v: 0,
  creator: '9GQ46rqY238rk2neSwgidap9ww5zbAN4dyqyC7j5ZnBK',
  type: 'TX',
  nonce: 7027,
  fee: 0.03,
  payload: { to: 'Ai1Pk9RzmgeiSAptc1W69NAmSB52GqjNqGdGQGgqxQA1', amount: 13.37 },
  sigType: 'D',
  sig: 'RLKzD1oKtUd+zvb2SD0cPguWgUYh2ZBK4INf+ve2NTRcysxrv8+0jRnjBHn/9xh+SRXwTZLskQp4+K1EpmkgDw=='
}
```

#### Send

```javascript
let sendStatus = await web1337.sendTransaction(tx);

console.log(sendStatus);

// After that - you can check the tx receipt
// TxID - it's a BLAKE3 hash of transaction signature
let receipt = await web1337.getTransactionReceiptById(web1337.BLAKE3(tx.sig));

console.log(receipt);
```

#### **Result**

```json5
{
  shard: 'shard_0',
  blockID: '0:9GQ46rqY238rk2neSwgidap9ww5zbAN4dyqyC7j5ZnBK:7220',
  order: 0,
  isOk: true
}
```

where:

* **shard** - context of transaction. Account of sender and recipient also binded to this shard
* **blockID** - the block where tx is
* **order** - position of this tx in block
* **isOk** - status to check if tx successfully processed or not

Optional fields:

* **reason** - in case tx failed you can use this reason to understand why
* **createdContractAddress -** only in txs where contract was deployed. It might be WASM or EVM contract
* **extraDataToReceipt** - optional object with extra data (for example - result of contract call)

### Check explorer:

If you check this tx in explorer you should see:

<figure><img src="../../.gitbook/assets/image (9) (1).png" alt=""><figcaption><p>Picture from local explorer</p></figcaption></figure>

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
import {crypto} from 'web1337';


let privateKey1 = await crypto.bls.generatePrivateKey();
let publicKey1 = crypto.bls.derivePubKeyFromHexPrivateKey(privateKey1);

let privateKey2 = await crypto.bls.generatePrivateKey();
let publicKey2 = crypto.bls.derivePubKeyFromHexPrivateKey(privateKey2);

let privateKey3 = await crypto.bls.generatePrivateKey();
let publicKey3 = crypto.bls.derivePubKeyFromHexPrivateKey(privateKey3);

let privateKey4 = await crypto.bls.generatePrivateKey();
let publicKey4 = crypto.bls.derivePubKeyFromHexPrivateKey(privateKey4);


console.log(`Pair 1 => ${privateKey1} : ${publicKey1}`);
console.log(`Pair 2 => ${privateKey2} : ${publicKey2}`);
console.log(`Pair 3 => ${privateKey3} : ${publicKey3}`);
console.log(`Pair 4 => ${privateKey4} : ${publicKey4}`);
```

Output:

```code-runner-output
Pair 1 => 69bfb956053e034b1b7d8b634c9bb8acd1b5743fab64245fad4b1472f63799a9 : 0xabe301a20289d1e014c69906febbee42626e084dc93f70c689803e163963101fc90a3674576f86ff90ba9c71310685f1
Pair 2 => 001b9abf2e6d23e13b30a4d35242a11848530f5e40a0fea8c3c50d2902c6c056 : 0x8486ccde87fe2603815587a64bd43596d45b383d12563e6b513a9ca033ec9974490f9d577125bbd2b80ada893476a603
Pair 3 => 5eca1e38f579064091c4c84eab47b5b5f2a0256847674e6f590879c2dc1605e0 : 0x87960819a282b2092825c73ac3afbe2e1d5680dff8e00c1f10851ab0b025ae45ac46698d6c0dfda229e2a187bde9690c
Pair 4 => 1635f5c69f695673e83184aba52316a9d2a458016e791ae0180d8c42d05a144c : 0x965c986dc91c1a45b6a6d24bb7b8509e9c80f6e77827de1e70737bdeeb23ef3362864da538da57a57384c17dcc9a3fc7
```

Now, you need to aggregate your 4 public keys to get the root public key and receive payments for collective usage.

```javascript
let publicKey1 = '0xabe301a20289d1e014c69906febbee42626e084dc93f70c689803e163963101fc90a3674576f86ff90ba9c71310685f1';
let privateKey1 = '69bfb956053e034b1b7d8b634c9bb8acd1b5743fab64245fad4b1472f63799a9';

let publicKey2 = '0x8486ccde87fe2603815587a64bd43596d45b383d12563e6b513a9ca033ec9974490f9d577125bbd2b80ada893476a603';
let privateKey2 = '001b9abf2e6d23e13b30a4d35242a11848530f5e40a0fea8c3c50d2902c6c056';

let publicKey3 = '0x87960819a282b2092825c73ac3afbe2e1d5680dff8e00c1f10851ab0b025ae45ac46698d6c0dfda229e2a187bde9690c';
let privateKey3 = '5eca1e38f579064091c4c84eab47b5b5f2a0256847674e6f590879c2dc1605e0';

let publicKey4 = '0x965c986dc91c1a45b6a6d24bb7b8509e9c80f6e77827de1e70737bdeeb23ef3362864da538da57a57384c17dcc9a3fc7';
let privateKey4 = '1635f5c69f695673e83184aba52316a9d2a458016e791ae0180d8c42d05a144c';


let rootPubKey = crypto.bls.aggregatePublicKeys([publicKey1,publicKey2,publicKey3,publicKey4]);

console.log('RootPubKey => ',rootPubKey);
```

Output:

```code-runner-output
RootPubKey =>  0xb89c4bf0b9dab0224201d06d46ed6cb49b94f34f8dc8feb0d7bad77caab5b41fc16531dce9ba2cba5784359d2b701cc4
```

Now you can accept payments for this address.

### From sender's point of view

When you need to send something to multisig account you need to set the reverse threshold if account still not exists or use `rev_t` property of already existed account

So, if account `0xb89c4bf0b9dab0224201d06d46ed6cb49b94f34f8dc8feb0d7bad77caab5b41fc16531dce9ba2cba5784359d2b701cc4` still not in state - use this template:

```javascript
let web1337 = new Web1337({

    chainID:'aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa',
    workflowVersion:0,
    nodeURL: 'http://localhost:7332'

}); // need node endpoint to return the correct nonce. If you know your nonce - you can omit it

const keypair = {
    
    pub:"9GQ46rqY238rk2neSwgidap9ww5zbAN4dyqyC7j5ZnBK",
    
    prv:"MC4CAQAwBQYDK2VwBCIEILdhTMVYFz2GP8+uKUA+1FnZTEdN8eHFzbb8400cpEU9",

};


// In our example with 4 friends, since we want 3/4 agreements
// to use account, the reverse threshold will be 4-3=1
// Use the formula rev_t = N-T where N - number of sides, T-threshold
const reverseThreshold = 1;

const shardID = "shard_0";

const payload = {

    to: "0xb89c4bf0b9dab0224201d06d46ed6cb49b94f34f8dc8feb0d7bad77caab5b41fc16531dce9ba2cba5784359d2b701cc4",

    amount: 13.37,
    
    shard: shardID,

    rev_t: reverseThreshold

};

const fee = 0.03;

const nonce = await web1337.getAccount(shardID,keypair.pub).then(account=>account.nonce+1);

const txType = "TX";

let tx = web1337.createEd25519Transaction(shardID,txType,keypair.pub,keypair.prv,nonce,fee,payload);

console.log(tx);
```

Result:

```json5
{
  v: 0,
  creator: '9GQ46rqY238rk2neSwgidap9ww5zbAN4dyqyC7j5ZnBK',
  type: 'TX',
  nonce: 7028,
  fee: 0.03,
  payload: {
    to: '0xb89c4bf0b9dab0224201d06d46ed6cb49b94f34f8dc8feb0d7bad77caab5b41fc16531dce9ba2cba5784359d2b701cc4',
    shard:"shard_0",
    amount: 13.37,
    rev_t: 1
  },
  sigType: 'D',
  sig: 'm6k4I1AMLA1W8lDZWZb1UcFE1OYfe6uTvrzr6b9XyOe/MIk04R7GJQ575If3JUgNDl+1uq0PDJoVibKgsqvlAQ=='
}
```

After this transaction, new account will be added to state:

```json
"0xb89c4bf0b9dab0224201d06d46ed6cb49b94f34f8dc8feb0d7bad77caab5b41fc16531dce9ba2cba5784359d2b701cc4":{
    "type": "eoa",
    "balance": 13.37,
    "uno": 0,
    "nonce": 0,
    "gas": 0,
    "rev_t": 1,
    "shard": "shard_0"
}
```

As you see, new multisig account is created and binded to shard where sender sends KLY. Also, the `rev_t` is set to 1 what means that the number of dissenting sides can be 1.

In case account was already in state - get the `rev_t` from information about account:

```javascript
const shardID = 'shard_0';

const recipient = '0xb89c4bf0b9dab0224201d06d46ed6cb49b94f34f8dc8feb0d7bad77caab5b41fc16531dce9ba2cba5784359d2b701cc4';

let accountInfo  = await web1337.getAccount(shardID,recipient);

console.log('Account:\n\n',accountInfo);
```

```
Account

{
    "type": "eoa",
    "balance": 13.37,
    "uno": 0,
    "nonce": 0,
    "gas": 0,
    "rev_t": 1,
    "shard": "shard_0"
}
```

And then, use the value of `rev_t` to build the transaction as above

## Ed25519 => TBLS(thresholdsig address) transaction

In this transaction you send something to TBLS root public key which controled by group of <mark style="color:red;">**`N`**</mark> members

{% hint style="info" %}
We'll talk more about TBLS in the next parts. Just now you need to know the 48-byte root public key
{% endhint %}

```javascript
let web1337 = new Web1337({

    chainID:'aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa',
    workflowVersion:0,
    nodeURL: 'http://localhost:7332'

}); // need node endpoint to return the correct nonce. If you know your nonce - you can omit it

const keypair = {
    
    pub:"9GQ46rqY238rk2neSwgidap9ww5zbAN4dyqyC7j5ZnBK",
    
    prv:"MC4CAQAwBQYDK2VwBCIEILdhTMVYFz2GP8+uKUA+1FnZTEdN8eHFzbb8400cpEU9",

};

const shardID = "shard_0";

const payload = {

    to: "b89c4bf0b9dab0224201d06d46ed6cb49b94f34f8dc8feb0d7bad77caab5b41fc16531dce9ba2cba5784359d2b701cc4",

    amount: 13.37,
    
    shard:shardID

};

const fee = 0.03;

const nonce = await web1337.getAccount(shardID,keypair.pub).then(account=>account.nonce+1);

const txType = "TX";

let tx = web1337.createEd25519Transaction(shardID,txType,keypair.pub,keypair.prv,nonce,fee,payload);

console.log(tx);
```



## Ed25519 => PostQuantum(Dilithium/BLISS) transaction

In this transaction you send your assets to the BLAKE3 hash of public key of some post-quantum signatures schemes like DIlithium or BLISS (we support 2 algorithms)

#### NOW ATTENTION:

{% hint style="warning" %}
<mark style="color:red;">**In case you fund a NEW post-quantum account you need to add the post-quantum pubkey of recipient to payload**</mark>
{% endhint %}

By default, you recipient have locally keypair like this:

```javascript
let blissKeyPair = {

    pub: '001b4609d500e31a0a188911900aac07fb06f91566038104e90c01031707d6154701701a15046d07f5089f0c730c8515e712c90b5a130d10081bca0ab40c8f0027101501870ccb17041d691bac0c30162d11ff0566198710f308cd08b30be804261a040c530cb8042e16841623069200b9175410a5016a171e1ceb10f813261bae0acc06be176214471d7013530d92180a0dbd15c800fd09f700ed0a8616141a14095b08a71c3317031d78106602ef1c1f1a53097016df192905b50ad40b5c1d1c027e026b0ecb115417ae1b6f1c1101c60d3f1c12016010a309f8183411840d7d12d414071a5b10d1162111f712951b36066209500e1d137d1dbf055417e6075c0ce307460ff9040715b51d0000cc11cf1bd1194c0a2d19e901191c5306040c8002be0d19024f10b31b19152912fe06900de21b2e10110ab111f80b6403ac1b8505221bac09830e3501a1175705c7138e1db6035c09871c4c121706b70b560ac70c001d2305b0107117ef02c1178a13f010bb193004ca02bc035e036f109419770a2017f11dd00cb3016405b41604091206c61603085208fa0df0130912cb14cd187914b009e306440a3018ca0c5810c305400507103b1113016c0ead00100e3f02b6003410981cdf04c50d0213d61984110c0ba700ae0c8912f618a01a231bc81066010a1d051242103013ac05c30dae14030f890e1117b319a002a707f30923',
    prv: 'd112525a9435c29d732592e9ec90eba2ae7b1ae2c0d3ac9b6d6ce662ca5140abb02bc846c7f54f955a84fed543107c9180366d025324aac2d253ec60515cf9ee',
    address: '1826d3782d53b127c53129fe67f4a3e3c1140feb2af36a0517077297a6e867e5'

};
```

#### Case 1: His account already in state and has balance

In case recipient account already exists, just ask him for **address.**

In this case your payload of transaction will look like this:

```javascript
let payload = {

    to: "1826d3782d53b127c53129fe67f4a3e3c1140feb2af36a0517077297a6e867e5",
    
    amount: 13.37,
    
    shard:shardID
    
}
```

#### Case 2: Recipient account will be new and didn't exists before tx

So, if you fund new account the payload of your transaction should be:

```javascript
let payload = {

    to: "1826d3782d53b127c53129fe67f4a3e3c1140feb2af36a0517077297a6e867e5",
    
    amount: 13.37,
    
    shard:shardID,
    
    pqcPub:"001b4609d500e31a0a188911900aac07fb06f91566038104e90c01031707d6154701701a15046d07f5089f0c730c8515e712c90b5a130d10081bca0ab40c8f0027101501870ccb17041d691bac0c30162d11ff0566198710f308cd08b30be804261a040c530cb8042e16841623069200b9175410a5016a171e1ceb10f813261bae0acc06be176214471d7013530d92180a0dbd15c800fd09f700ed0a8616141a14095b08a71c3317031d78106602ef1c1f1a53097016df192905b50ad40b5c1d1c027e026b0ecb115417ae1b6f1c1101c60d3f1c12016010a309f8183411840d7d12d414071a5b10d1162111f712951b36066209500e1d137d1dbf055417e6075c0ce307460ff9040715b51d0000cc11cf1bd1194c0a2d19e901191c5306040c8002be0d19024f10b31b19152912fe06900de21b2e10110ab111f80b6403ac1b8505221bac09830e3501a1175705c7138e1db6035c09871c4c121706b70b560ac70c001d2305b0107117ef02c1178a13f010bb193004ca02bc035e036f109419770a2017f11dd00cb3016405b41604091206c61603085208fa0df0130912cb14cd187914b009e306440a3018ca0c5810c305400507103b1113016c0ead00100e3f02b6003410981cdf04c50d0213d61984110c0ba700ae0c8912f618a01a231bc81066010a1d051242103013ac05c30dae14030f890e1117b319a002a707f30923"
    
}
```

{% hint style="info" %}
In case PQC account of recipient do not exist - include additional field pqcPub to payload to let the network to create **pubkey:address** pair in state
{% endhint %}

See

{% content-ref url="post-quantum-transactions.md" %}
[post-quantum-transactions.md](post-quantum-transactions.md)
{% endcontent-ref %}

{% hint style="info" %}
We'll talk about PQC accounts later. Just now, as a sender you just need to know only the  address of recipient - it's 256-bit BLAKE3 hash of public key
{% endhint %}

```javascript
let web1337 = new Web1337({

    chainID:'aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa',
    workflowVersion:0,
    nodeURL: 'http://localhost:7332'

}); // need node endpoint to return the correct nonce. If you know your nonce - you can omit it

const keypair = {
    
    pub:"9GQ46rqY238rk2neSwgidap9ww5zbAN4dyqyC7j5ZnBK",
    
    prv:"MC4CAQAwBQYDK2VwBCIEILdhTMVYFz2GP8+uKUA+1FnZTEdN8eHFzbb8400cpEU9",

};

const shardID = "shard_0";

const payload = {

    to: "4218fb0aaace62c4bfafbdd9adb05b99a9bf1a33eeae074215a51cb644b9a85c",

    amount: 13.37,
    
    shard:shardID

};

const fee = 0.03;

const nonce = await web1337.getAccount(shardID,keypair.pub).then(account=>account.nonce+1);

const txType = "TX";

let tx = web1337.createEd25519Transaction(shardID,txType,keypair.pub,keypair.prv,nonce,fee,payload);

console.log(tx);
```
