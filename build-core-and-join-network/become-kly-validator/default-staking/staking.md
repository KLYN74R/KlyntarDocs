# Staking

## Intro

TODO

## Find validator(staking pool) to stake on

First of all - search for the staking pool (validator) you want to stake on.

You have several ways to do this:

1. Own data channels - this can be your own pool, some large staking provider (for example Everstake or staking by some top-tiers exchanges).
2. Search on our resources - you can find a staking pool in the explorer and select it for staking

Visit the main page of explorer and choose `Epoch data`

<figure><img src="../../../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

Now below you should see the list of leaders on shards for current epoch:

<figure><img src="../../../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

By clicking on a specific validator you will be redirected to a page with information about it.

<figure><img src="../../../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

Below is a list of stakers of this pool. For example, for this pool, only the owner himself is a staker. The total strength of the pool is 55,000 KLY, while the owner has staked all 55,000 KLY

<figure><img src="../../../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

However, using this pool as an example, we will demonstrate the staking process below.

## Method 1 - via user interface

## Method 2 - programmatic way using SDK

### 1. Prepare your account

Imagine that you have some keypair of your account. Definitely you should have some coins on this address to stake

```javascript
let keypair = {

    pub:"3JAeBnsMedzxjCMNWQYcAXtwGVE9A5DBQyXgWBujtL9R",

    prv:"MC4CAQAwBQYDK2VwBCIEIDteWfNev7NOlNmwP8Irwg5miWKoErYGV+UU5VrFgYev"

};
```

For example, this account have `20M` KLY

<figure><img src="../../../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

### 2. Create the transaction to call staking system smart contract

Now use the Web1337 [**SDK**](../../../web1337/web1337-intro.md) to create the special transaction which calls system smart contract related to staking

```javascript
import Web1337 from 'web1337';

let web1337 = new Web1337({

    chainID:'aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa',
    workflowVersion:0,
    nodeURL: 'http://localhost:7332' // or your node URL

});


// This is your keypair

let keypair = {

    pub:"3JAeBnsMedzxjCMNWQYcAXtwGVE9A5DBQyXgWBujtL9R",

    prv:"MC4CAQAwBQYDK2VwBCIEIDteWfNev7NOlNmwP8Irwg5miWKoErYGV+UU5VrFgYev"

};


const shardID = "shard_0";

const poolToStakeOn = "9GQ46rqY238rk2neSwgidap9ww5zbAN4dyqyC7j5ZnBK";

const amountToStake = 100;


let payload = {

    shard: shardID,

    contractID:'system/staking',

    method:'stake',

    gasLimit:0,

    params:{

        poolPubKey:poolToStakeOn,
        amount: amountToStake,
        units:'kly'

    },

    imports:[]

};


const fee = 2;

const nonce = await web1337.getAccount(shardID,keypair.pub).then(account=>account.nonce+1);

const txType = "WVM_CALL";

let tx = web1337.createEd25519Transaction(shardID,txType,keypair.pub,keypair.prv,nonce,fee,payload);

console.log(tx);

web1337.sendTransaction(tx).then(value =>{

    console.log('Response from node => ',value)

    console.log(`TX ID is => `,web1337.blake3(tx.sig))

}).catch(err=>console.error('Error: ',err));
```

The structure of transaction that will be sent to node should be like this:

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
    method: 'stake',
    gasLimit: 0,
    params: {
      poolPubKey: '9GQ46rqY238rk2neSwgidap9ww5zbAN4dyqyC7j5ZnBK',
      amount: 100,
      units: 'kly'
    },
    imports: []
  },
  sigType: 'D',
  sig: 'gNT1S5kN7rJtLBSCVclgp+kSN/aScQu65VVxPGXSji2KNLVOPAOp2ya/80eDjNBMUDMFXL4Q98UQzCjTBM8xBQ=='
}
```









{% stepper %}
{% step %}



{% endstep %}

{% step %}
### Check the staking status


{% endstep %}

{% step %}
### Wait untill the next epoches


{% endstep %}

{% step %}
### Make sure you're in a staking list


{% endstep %}
{% endstepper %}
