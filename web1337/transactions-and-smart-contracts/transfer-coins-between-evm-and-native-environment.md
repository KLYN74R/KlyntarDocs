---
icon: money-bill-transfer
---

# Transfer coins between EVM and native environment

## Intro

When using the Klyntar network, you may need to work in two environments:

1. In the native environment - using WASM smart contracts, interacting with system smart contracts, etc.
2. Or use the EVM environment with its smart contracts, working with the web3 SDK and JSON-RPC interfaces for compatibility, for example, with various wallets.

Situations might be different, so you will need to know how to transfer coins from one environment to another. Let's learn!

## Transfer coins from native KLY environment to EVM

When you need to transfer coins from the native environment to the EVM - just use the standard methods of creating a transaction - as described in the previous sections :wink:

For example, you can transfer:

1. Ed25519 => EVM [default-ed25519-transactions.md](default-ed25519-transactions.md "mention")
2. BLS => EVM [bls-multisig-transactions.md](bls-multisig-transactions.md "mention")
3. TBLS => EVM [tbls-thresholdsig-transactions.md](tbls-thresholdsig-transactions.md "mention")
4. PQC => EVM [post-quantum-transactions.md](post-quantum-transactions.md "mention")

All you need is to specify in the payload, in the `to` field, a standard EVM-compatible address: with a 0x prefix and a length of 20 bytes.

For example, transaction from Ed25519 account to EVM account will be like this:

```javascript
import Web1337 from 'web1337';

let web1337 = new Web1337({

    chainID:'aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa',
    workflowVersion:0,
    nodeURL: 'http://localhost:7332'

});


const keypair = {
    
    pub:"9GQ46rqY238rk2neSwgidap9ww5zbAN4dyqyC7j5ZnBK",
    
    prv:"MC4CAQAwBQYDK2VwBCIEILdhTMVYFz2GP8+uKUA+1FnZTEdN8eHFzbb8400cpEU9",

};

const shardID = "shard_0";

const payload = {

    to: "0x407d73d8a49eeb85d32cf465507dd71d507100c1",

    amount: 13.37,
    
    shard: shardID,

    touchedAccounts:["9GQ46rqY238rk2neSwgidap9ww5zbAN4dyqyC7j5ZnBK","0x407d73d8a49eeb85d32cf465507dd71d507100c1".toLowerCase()]

};

const fee = 0.03;

const nonce = await web1337.getAccount(shardID,keypair.pub).then(account=>account.nonce+1);

const txType = "TX";

let tx = web1337.createEd25519Transaction(shardID,txType,keypair.pub,keypair.prv,nonce,fee,payload);

console.log(tx);

let sendStatus = await web1337.sendTransaction(tx);

console.log(sendStatus);
```

Example output:

```json
{
  v: 0,
  creator: '9GQ46rqY238rk2neSwgidap9ww5zbAN4dyqyC7j5ZnBK',
  type: 'TX',
  nonce: 1,
  fee: 0.03,
  payload: {
    to: '0x407d73d8a49eeb85d32cf465507dd71d507100c1',
    amount: 13.37,
    shard: 'shard_0',
    touchedAccounts: [
      '9GQ46rqY238rk2neSwgidap9ww5zbAN4dyqyC7j5ZnBK',
      '0x407d73d8a49eeb85d32cf465507dd71d507100c1'
    ]
  },
  sigType: 'D',
  sig: 'f/a/HjBR1BhKjLRLlo/aRWY9sZrUHN+c3T3m/DsLnsB3TWeSmNY6zn3Dgxq+Ycws1muWLjAe95lTY1UJKUHhDQ=='
}
```

### How to check it via explorer

Imagine that your account had such balance before tx:

<figure><img src="../../.gitbook/assets/image (5) (1) (1).png" alt=""><figcaption></figcaption></figure>

Recipient account:

<figure><img src="../../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

#### Transaction:

<figure><img src="../../.gitbook/assets/image (2) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
Note the `touchedAccounts` field in the payload - this is how we run the transaction in parallel mode :zap:
{% endhint %}

Sender account after tx:

<figure><img src="../../.gitbook/assets/image (3) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

Recipient account after tx:

<figure><img src="../../.gitbook/assets/image (4) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

## Transfer coins from EVM to native KLY environment

If you are transferring coins from the EVM environment to the native one, you need to:

1. Initiate a transaction to the address `0xdead`
2. In the `data` field, you need to encode a JSON object with the `to` field

In this case, the coins will go to the native environment to the `to` address and on the same shard.

### State before transaction

The sender account before transaction:

<figure><img src="../../.gitbook/assets/image (5) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

Recipient account before transaction:

<figure><img src="../../.gitbook/assets/image (6) (1).png" alt=""><figcaption></figcaption></figure>

Let's use a typical web3 library to see how we can transfer some coins from sender to recipient:

```javascript
import {Transaction} from '@ethereumjs/tx';
import {Common} from '@ethereumjs/common';
import Web3 from 'web3';




const web3 = new Web3('http://localhost:7332/kly_evm_rpc/shard_0');

// KLY-EVM
const common = Common.custom({name:'KLYNTAR',networkId:'0x1CA3',chainId:'0x1CA3'},{hardfork:'london'});


const evmAccount0 = {

    address:'0x069bdf66961ce2D38eBe48DD2E095f2c8015ac82',
    privateKey:Buffer.from('a06d4e98075df20d90972dfec819a8711c8d245423f9d3a13f809505f81fbcb8','hex')

};

let nonce = await web3.eth.getTransactionCount(evmAccount0.address);


let dataToPass = {

    to: 'GUbYLN5NqmRocMBHqS183r2FQRoUjhx1p5nKyyUBpntQ',

    touchedAccounts: [evmAccount0.address.toLowerCase(),'0x000000000000000000000000000000000000dead','GUbYLN5NqmRocMBHqS183r2FQRoUjhx1p5nKyyUBpntQ'],

    // rev_t(?) - for new BLS multisig accounts,

    // pqcPub(?) - for new post-quantum accounts

};

let hexEncodedData = web3.utils.asciiToHex(JSON.stringify(dataToPass));


// Build a transaction

let txObject = {

    from:evmAccount0.address,
    
    nonce:web3.utils.toHex(nonce),

    to:'0x000000000000000000000000000000000000dead',
    
    value: web3.utils.toHex(web3.utils.toWei('1.337','ether')),
    
    gasLimit: web3.utils.toHex(230000),
    
    gasPrice: web3.utils.toHex(web3.utils.toWei('100','gwei')),

    // Set payload here - it will be parsed by KLY-EVM

    data: hexEncodedData

};


let tx = Transaction.fromTxData(txObject,{common}).sign(evmAccount0.privateKey);

console.log('Tx hash is => ','0x'+tx.hash().toString('hex'));

console.log(tx.toJSON());


// Send
web3.eth.sendSignedTransaction(raw,(err,txHash) => console.log(err?`Oops,some error has been occured ${err}`:`Success ———> ${txHash}`));
```

Example output:

```code-runner-output
Tx hash is =>  0x427d61e984f219c38b9d7ba8e489ae63727085acfc4850436e93579a47b117f3
{
  nonce: '0x0',
  gasPrice: '0x174876e800',
  gasLimit: '0x38270',
  to: '0x000000000000000000000000000000000000dead',
  value: '0x128dfa6a90b28000',
  data: '0x7b22746f223a22475562594c4e354e716d526f634d4248715331383372324651526f556a68783170356e4b79795542706e7451222c22746f75636865644163636f756e7473223a5b22307830363962646636363936316365326433386562653438646432653039356632633830313561633832222c22307830303030303030303030303030303030303030303030303030303030303030303030303064656164222c22475562594c4e354e716d526f634d4248715331383372324651526f556a68783170356e4b79795542706e7451225d7d',
  v: '0x396a',
  r: '0xb05f494ae89554a2dfa35aca335d491ab7a514ea65baf59da3730196efbcb57a',
  s: '0x111a85fd923353255a67a1cebdabb8e2fdd7452496ba05a0a41a8ad3ed84dfa8'
}
```

### Check via explorer

<figure><img src="../../.gitbook/assets/image (10).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (7) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (8).png" alt=""><figcaption></figcaption></figure>

So, we succesfully transferred `1.337` KLY from sender to recipient
