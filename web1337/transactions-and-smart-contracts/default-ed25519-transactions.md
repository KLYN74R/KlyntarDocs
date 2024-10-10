---
description: Solana compatible accounts
---

# 🔐 Default Ed25519 transactions

<figure><img src="../../.gitbook/assets/image (5) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

## Create and send default transaction

{% hint style="info" %}
Here we propose the example of simple transaction from default account(ed25519) to any other account
{% endhint %}

## Keypair creation process

<figure><img src="../../.gitbook/assets/image (2) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

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

{% tab title="Golang" %}
```go
import "fmt"
import ed25519 "github.com/KLYN74R/Web1337Golang/crypto_primitives/ed25519"

func main() {

	mnemonic := "" // mnemonic should be empty in case you generate a new pair

	mnemonicPassword := "" // set the password for your mnemonic

	bip44Path := []uint32{44, 7331, 0, 0} // 7331 is KLY ID and the next values - derivation path

	keypair := ed25519.GenerateKeyPair(mnemonic, mnemonicPassword, bip44Path)

 	fmt.Println(keypair)
	
}
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

{% tab title="Golang" %}
```go
import "fmt"
import ed25519 "github.com/KLYN74R/Web1337Golang/crypto_primitives/ed25519"

func main() {

	mnemonic := ""

	mnemonicPassword := "HelloKlyntar"

	bip44Path := []uint32{}

	keypair := ed25519.GenerateKeyPair(mnemonic, mnemonicPassword, bip44Path)

 	fmt.Println(keypair)
	
}
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


const payload = {

    to: "Ai1Pk9RzmgeiSAptc1W69NAmSB52GqjNqGdGQGgqxQA1",

    amount: 13.37

};

const shardID = "shard_0";

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
* **createdContractAddress - only in txs where contract was deployed. It might be WASM or EVM contract**
* **extraDataToReceipt** - optional object with extra data (for example - result of contract call)

### Check explorer:

If you check this tx in explorer you should see:

<figure><img src="../../.gitbook/assets/image.png" alt=""><figcaption><p>Picture from local explorer</p></figcaption></figure>

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
import {bls} from 'web1337'

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

### From sender's point of view

When you need to send something to multisig account you need to set the reverse threshold if account still not exists or use \`rev\_t\` property of already existed account

So, if account `68Bpgi6MbRX9q3T9h8DDWomPGu85HqWSfPMT23r6g29xyn1dN7qfquwxpfFNMdMpU1` still not in state - use this template:

```javascript
const myKeyPair = {

    mnemonic: 'south badge state hedgehog carpet aerobic float million enforce opinion hungry race',
    bip44Path: "m/44'/7331'/0'/0'",
    pub: '2VEzwUdvSRuv1k2JaAEaMiL7LLNDTUf9bXSapqccCcSb',
    prv: 'MC4CAQAwBQYDK2VwBCIEIDEf/4H5iiY3ebAfWsFIFkeZrB8HpcvBYK5zjEe9/8ga'
      
};


const shardID = '7GPupbq1vtKUgaqVeHiDbEJcxS7sSjwPnbht4eRaDBAEJv8ZKHNCSu2Am3CuWnHjta';

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

let signedTx = await web1337.createDefaultTransaction(shardID,from,myPrivateKey,nonce,recipient,fee,amountInKLY,reverseThreshold);

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
    "shard":"7GPupbq1vtKUgaqVeHiDbEJcxS7sSjwPnbht4eRaDBAEJv8ZKHNCSu2Am3CuWnHjta"
}
```

As you see, new multisig account is created and binded to shard where sender sends KLY. Also, the `rev_t` is set to 1 what means that the number of dissenting sides can be 1.



In case account was already in state - get the `rev_t` from information about account:

```javascript
const shardID = '7GPupbq1vtKUgaqVeHiDbEJcxS7sSjwPnbht4eRaDBAEJv8ZKHNCSu2Am3CuWnHjta';

const recipient = '68Bpgi6MbRX9q3T9h8DDWomPGu85HqWSfPMT23r6g29xyn1dN7qfquwxpfFNMdMpU1';

let accountInfo  = await web1337.getFromState(shardID,recipient);

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
    "shard":"7GPupbq1vtKUgaqVeHiDbEJcxS7sSjwPnbht4eRaDBAEJv8ZKHNCSu2Am3CuWnHjta"
}
```

And then, use the value of `rev_t` to build the transaction as above

## Ed25519 => TBLS(thresholdsig address) transaction

In this transaction you send something to TBLS root public key which controled by group of <mark style="color:red;">**`N`**</mark> members

{% hint style="info" %}
We'll talk more about TBLS in the next parts. Just now you need to know the 48-byte root public key
{% endhint %}

```javascript
const myKeyPair = {

  mnemonic: 'pudding record forget once lunch cable shiver million tobacco window detail chicken account bring talk water antenna fortune fox story grain fortune error wine',
  bip44Path: [ 44, 7331, 0, 0 ],
  pub: '2hXpBF5MUY3cPmkNrsB6DUf2pdf3uwPLTP2znCFGMbRn',
  prv: 'MC4CAQAwBQYDK2VwBCIEIBSVvzGJAMtc+XNPh0sCTUdx9VQ0xnfr5YEATkVFo2E5'

};


const shardID = 'shard_0';

const recipientTblsRootPub = 'bedc88644f0deea4c0a77ba687712f494a1af7d8869f09768a0db42284f89d17b7b9225e0c87c2cb5511907dfd5eae3a';

const from = myKeyPair.pub;

const myPrivateKey = myKeyPair.prv;

const nonce = await web1337.getAccount(shardID,keypair.pub).then(account=>account.nonce+1);

const fee = 1;

const amountInKLY = 13.37;

let tx = web1337.createEd25519Transaction(shardID,txType,keypair.pub,keypair.prv,nonce,fee,payload)

web1337.sendTransaction(tx).then(()=>{

    console.log(`Transaction was sent. The TxID is ${web1337.blake3(tx.sig)}`)

}).catch(err=>console.error('Error: ',err))
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


const shardID = '7GPupbq1vtKUgaqVeHiDbEJcxS7sSjwPnbht4eRaDBAEJv8ZKHNCSu2Am3CuWnHjta';

// It might be Dilithium or BLISS, but doesn't matter for you
const recipientPQC = 'f5091405e28455880fc4191cbda9f1e57f72399e732222d4639294b66d3a5076';

const from = myKeyPair.pub;

const myPrivateKey = myKeyPair.prv;

const nonce = 0;

const fee = 1;

const amountInKLY = 13.37;


let signedTx = await web1337.createDefaultTransaction(shardID,from,myPrivateKey,nonce,recipientTblsRootPub,fee,amountInKLY);

console.log(signedTx);
```



