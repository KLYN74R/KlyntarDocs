---
icon: user-check
---

# Create validator(staking pool)

## Intro

Creating a new validator helps the network to be more decentralized and resilient. On this page we will show you how to create a validator to prepare it for the staking process.

## 1. Prepare your keypair

Let's imagine that you have a pair of ed25519 keys that you will use to control your validator. To generate such a pair - use the tutorial here:

{% embed url="https://docs.klyntar.org/web1337/transactions-and-smart-contracts/default-ed25519-transactions#keypair-creation-process" %}

Let's say a key pair looks like this:

```javascript
let keypair = {

    pub:"GUbYLN5NqmRocMBHqS183r2FQRoUjhx1p5nKyyUBpntQ",

    prv:"MC4CAQAwBQYDK2VwBCIEILjvmDeOmyg1/VG2VKQTzsv6lkIizQpjmRsdfEEIHHU8"

}
```

## 2. Create the transaction to call staking system smart contract

Use the Web1337 SDK to create a call to system smart contract

```javascript
import Web1337 from 'web1337';


let web1337 = new Web1337({

    chainID:'aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa',
    workflowVersion:0,
    nodeURL: 'http://localhost:7332' // set the URL to your own node

});


let keypair = {

    pub:"GUbYLN5NqmRocMBHqS183r2FQRoUjhx1p5nKyyUBpntQ",

    prv:"MC4CAQAwBQYDK2VwBCIEILjvmDeOmyg1/VG2VKQTzsv6lkIizQpjmRsdfEEIHHU8"

};


let payload = {

    contractID:'system/staking',

    method:'createStakingPool',

    gasLimit:0,

    params:{
        
        shard:'shard_0',
        
        ed25519PubKey:keypair.pub,
        
        percentage:0.3,
        
        overStake:10000,
        
        poolURL:'http://localhost:7335', // set your own domain/ip
        
        wssPoolURL:'ws://localhost:9335' // set your own domain/ip

    },

    imports:[]

};


const shardID = "shard_0";

const fee = 2;

const nonce = await web1337.getAccount(shardID,keypair.pub).then(account=>account.nonce+1);

const txType = "WVM_CALL";

let tx = web1337.createEd25519Transaction(shardID,txType,keypair.pub,keypair.prv,nonce,fee,payload);

console.log(tx)

// web1337.sendTransaction(tx).then(()=>{

//     console.log('Sent')

//     console.log(`TX ID is => `,web1337.blake3(tx.sig))

// }).catch(err=>console.error('Error during deployment: ',err))
```

TODO

## 3. Wait untill the next epoches

## 4. Start staking
