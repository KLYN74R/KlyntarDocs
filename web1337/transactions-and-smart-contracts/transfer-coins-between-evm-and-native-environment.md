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
const shardID = "shard_0";

const payload = {

    to: "0x4741c39e6096c192Db6E1375Ff32526512069dF5", // yes, just paste EVM address here

    amount: 13.37,
    
    shard: shardID

};

let web1337 = new Web1337({

    chainID:'aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa',
    workflowVersion:0,
    nodeURL: 'http://localhost:7332'

}); // need node endpoint to return the correct nonce. If you know your nonce - you can omit it

const keypair = {
    
    pub:"9GQ46rqY238rk2neSwgidap9ww5zbAN4dyqyC7j5ZnBK",
    
    prv:"MC4CAQAwBQYDK2VwBCIEILdhTMVYFz2GP8+uKUA+1FnZTEdN8eHFzbb8400cpEU9",

};

const fee = 0.03;

const nonce = await web1337.getAccount(shardID,keypair.pub).then(account=>account.nonce+1);

const txType = "TX";

let tx = web1337.createEd25519Transaction(shardID,txType,keypair.pub,keypair.prv,nonce,fee,payload);

console.log(tx);
```

Example output:

```code-runner-output
{
  v: 0,
  creator: '9GQ46rqY238rk2neSwgidap9ww5zbAN4dyqyC7j5ZnBK',
  type: 'TX',
  nonce: 32775,
  fee: 0.03,
  payload: {
    to: '0x4741c39e6096c192Db6E1375Ff32526512069dF5',
    amount: 13.37,
    shard: 'shard_0'
  },
  sigType: 'D',
  sig: 'Zxr68Eizt3XEsJAW+QuAn4DiBSpASl9on6TixuFzF8PpL92eyuregferLI066mipPsSE4lFSNytajXAD+qE7Bw=='
}
```

## Transfer coins from EVM to native KLY environment

If you are transferring coins from the EVM environment to the native one, you need to:

1. Initiate a transaction to the address `0xdead`
2. In the data field, you need to encode a JSON object with the `to` field

In this case, the coins will go to the native environment to the `to` address and on the same shard.

Let's use a typical web3 library to see how it should look:

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

    to: 'GUbYLN5NqmRocMBHqS183r2FQRoUjhx1p5nKyyUBpntQ', // in this case - typical native KLY ed25519 account

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
    
    gasLimit: web3.utils.toHex(23000),
    
    gasPrice: web3.utils.toHex(web3.utils.toWei('10','gwei')),

    // Set payload her - it will be parsed by KLY-EVM

    data: hexEncodedData

};


let tx = Transaction.fromTxData(txObject,{common}).sign(evmAccount0.privateKey);

console.log('Tx hash is => ','0x'+tx.hash().toString('hex'));

console.log(tx.toJSON());
```

Example output:

```code-runner-output
Tx hash is =>  0x55e69ecc9a268b704fbc5025616d73db2a38864d2ad5631dc260b0ced762bf97
{
  nonce: '0x1',
  gasPrice: '0x2540be400',
  gasLimit: '0x59d8',
  to: '0x000000000000000000000000000000000000dead',
  value: '0x128dfa6a90b28000',
  data: '0x7b22746f223a22475562594c4e354e716d526f634d4248715331383372324651526f556a68783170356e4b79795542706e7451227d',
  v: '0x3969',
  r: '0x8d59ed73abe32a8ed961d9d7a35f16047e4d42e107cbba574094b12e92c15b8e',
  s: '0x3106a5b94981172ec2b0e7d510b7fec510346cdfea63a9fb76b417705c9c2764'
}
```
