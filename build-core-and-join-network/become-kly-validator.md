# 🕵️ Become a validator

## Intro

Creating a new validator helps the network to be more decentralized and resilient. On this page we will show you how to create a validator to prepare it for the staking process.

## 1. Prepare your keypair

Let's imagine that you have a pair of ed25519 keys that you will use to control your validator. To generate such a pair - use the tutorial here:

{% embed url="https://docs.klyntar.org/web1337/transactions-and-smart-contracts/default-ed25519-transactions#keypair-creation-process" %}

Let's say a key pair looks like this:

```javascript
const keypair = {

    pub:"3JAeBnsMedzxjCMNWQYcAXtwGVE9A5DBQyXgWBujtL9R",

    prv:"MC4CAQAwBQYDK2VwBCIEIDteWfNev7NOlNmwP8Irwg5miWKoErYGV+UU5VrFgYev"

}
```

## 2. Create the transaction to call staking system smart contract

Use the Web1337 SDK to create a call to system smart contract

```javascript
import Web1337 from 'web1337';


const web1337 = new Web1337({

    chainID:'aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa',
    workflowVersion:0,
    nodeURL: 'http://localhost:7332' // set the URL to your own node

});


const keypair = {

    pub:"3JAeBnsMedzxjCMNWQYcAXtwGVE9A5DBQyXgWBujtL9R",

    prv:"MC4CAQAwBQYDK2VwBCIEIDteWfNev7NOlNmwP8Irwg5miWKoErYGV+UU5VrFgYev"

};

const shardID = "shard_0";

const fee = 2;


let payload = {

    shard:'shard_0',

    contractID:'system/staking',

    method:'createStakingPool',

    gasLimit:0,

    params:{
        
        percentage:0.3,
        
        poolURL:'http://localhost:7335', // set your own domain/ip
        
        wssPoolURL:'ws://localhost:9335' // set your own domain/ip

    },

    imports:[]

};

const nonce = await web1337.getAccount(shardID,keypair.pub).then(account=>account.nonce+1);

const txType = "WVM_CALL";

let tx = web1337.createEd25519Transaction(shardID,txType,keypair.pub,keypair.prv,nonce,fee,payload);

console.log(tx);

web1337.sendTransaction(tx).then(()=>{

     console.log('Sent status => ok')
     console.log(`TX ID is => `,web1337.blake3(tx.sig))

}).catch(err=>console.error('Error during pool creation: ',err))
```

#### Output example:

```code-runner-output
{
  v: 0,
  creator: '3JAeBnsMedzxjCMNWQYcAXtwGVE9A5DBQyXgWBujtL9R',
  type: 'WVM_CALL',
  nonce: 1,
  fee: 2,
  payload: {
    shard: 'shard_0',
    contractID: 'system/staking',
    method: 'createStakingPool',
    gasLimit: 0,
    params: {
      percentage: 0.3,
      poolURL: 'http://localhost:7335',
      wssPoolURL: 'ws://localhost:9335'
    },
    imports: []
  },
  sigType: 'D',
  sig: 'T9uU7l58HEPgzcQMUObvZYxLKb28WRN2F21ec5Bt++wAJTZynUh1ofI0Pi98SMjLxOqQqmyLuFimJNaV7zCNDg=='
}


Sent status => ok
TX ID is =>  731c5f2ca6e1690b447da956c1a306692232e2101e8f89e8813be5bee3c5c37e
```

### Check the transaction status

Go to explorer, choose the filter and paste the transaction ID to searchbar

<figure><img src="../.gitbook/assets/image (70).png" alt=""><figcaption></figcaption></figure>

Yes, as expected the pool creation process was successful

<figure><img src="../.gitbook/assets/image (71).png" alt=""><figcaption></figcaption></figure>

Please note that this transaction was in a block that was in epoch **89** (see the first part of the block identifier before the colon sign)

<figure><img src="../.gitbook/assets/image (72).png" alt=""><figcaption></figcaption></figure>

## 3. Wait untill the next epoches

So as soon as 3 epochs have passed, you can go and check whether your pool has appeared in the state

<figure><img src="../.gitbook/assets/image (73).png" alt=""><figcaption></figcaption></figure>

Great, as you can see the pool was indeed added to the state

<figure><img src="../.gitbook/assets/image (74).png" alt=""><figcaption></figcaption></figure>

At the moment this pool is empty - it has no staked native coins or staking points(see [multistaking](staking/multistaking/ "mention")).

<figure><img src="../.gitbook/assets/image (76).png" alt=""><figcaption></figcaption></figure>

Also, when creating a pool, the address of the pool owner is automatically added to the list of stakers, although the initial stake values ​​are zero

<figure><img src="../.gitbook/assets/image (75).png" alt=""><figcaption></figcaption></figure>

{% hint style="success" %}
Now everything is ready to move on to the most interesting part - staking
{% endhint %}

## 4. Start staking

Go to the next sections to learn more about staking and multistaking. How to do it via user interface or via SDK - details are in the next sections :relaxed:

{% content-ref url="staking/default-staking/" %}
[default-staking](staking/default-staking/)
{% endcontent-ref %}

{% content-ref url="staking/multistaking/" %}
[multistaking](staking/multistaking/)
{% endcontent-ref %}
