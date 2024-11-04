---
icon: coins
---

# Claiming rewards

## Intro

When you stake your coins on a validator, you help it get the opportunity to generate blocks and vote for other creators' blocks.

In return, the validator will share part of its profits with you.

When the validator is working, rewards are accrued to the validator's storage.

{% hint style="success" %}
However, only you have the right to take your rewards - so your funds are safe. Neither the validator itself, nor any of the stakers will be able to take what was earned by your stake.
{% endhint %}

In this tutorial, we will show you how to take your reward after you have been a staker of the pool for some time

### Method 1 - via user interface <a href="#method-1-via-user-interface" id="method-1-via-user-interface"></a>

### Method 2 - programmatic way using SDK <a href="#method-2-programmatic-way-using-sdk" id="method-2-programmatic-way-using-sdk"></a>

In order to receive your rewards, you need to call the `getRewardFromPool()` method of the system smart contract `system/staking`

```javascript
import Web1337 from 'web1337';


let web1337 = new Web1337({

    chainID:'aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa',
    workflowVersion:0,
    nodeURL: 'http://localhost:7332' // set your own URL

});


let keypair = {

    pub:"3JAeBnsMedzxjCMNWQYcAXtwGVE9A5DBQyXgWBujtL9R",

    prv:"MC4CAQAwBQYDK2VwBCIEIDteWfNev7NOlNmwP8Irwg5miWKoErYGV+UU5VrFgYev"

};

const shardID = "shard_0";


let payload = {

    shard: shardID,

    contractID:'system/staking',

    method:'getRewardFromPool',

    gasLimit:0,

    params:{

        poolToGetRewardsFrom:'9GQ46rqY238rk2neSwgidap9ww5zbAN4dyqyC7j5ZnBK'

    },

    imports:[]

};


const fee = 2;

const nonce = await web1337.getAccount(shardID,keypair.pub).then(account=>account.nonce+1);

const txType = "WVM_CALL";

let tx = web1337.createEd25519Transaction(shardID,txType,keypair.pub,keypair.prv,nonce,fee,payload);

console.log(tx);

web1337.sendTransaction(tx).then(value =>{

    console.log('Sent => ',value)

    console.log(`TX ID is => `,web1337.blake3(tx.sig))

}).catch(err=>console.error('Error: ',err));
```

Output:

```code-runner-output
Sent =>  { status: 'OK' }
TX ID is =>  ac19fd2f0c98df692884e278b55e0d6cdf50a00c71ce2cdffa7e4ffbd883e72a
```

Using this TXID you can also check the status via explorer:

<figure><img src="../../.gitbook/assets/image (56).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (57).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
If the transaction is successful, you will receive your rewards in 2 epochs.
{% endhint %}
