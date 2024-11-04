# Unstaking

## Intro

TODO

## Method 1 - via user interface

## Method 2 - programmatic way using SDK

Let's assume that we have this:

1. Your address - `3JAeBnsMedzxjCMNWQYcAXtwGVE9A5DBQyXgWBujtL9R`
2. Pool you staked on - `9GQ46rqY238rk2neSwgidap9ww5zbAN4dyqyC7j5ZnBK`

Let's assume that we want to do the following:

1. Unstake 30 coins - this will return to our balance
2. 70 coins remain in the pool - you will continue to receive rewards from the validator

### 1. Call system smart contract to unstake

```javascript
import Web1337 from 'web1337';


let web1337 = new Web1337({

    chainID:'aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa',
    workflowVersion:0,
    nodeURL: 'http://localhost:7332'

});


let keypair = {

    pub:"3JAeBnsMedzxjCMNWQYcAXtwGVE9A5DBQyXgWBujtL9R",

    prv:"MC4CAQAwBQYDK2VwBCIEIDteWfNev7NOlNmwP8Irwg5miWKoErYGV+UU5VrFgYev"

};

const shardID = "shard_0";


let payload = {

    shard: shardID,

    contractID:'system/staking',

    method:'unstake',

    gasLimit:0,

    params:{

        poolPubKey:'9GQ46rqY238rk2neSwgidap9ww5zbAN4dyqyC7j5ZnBK',
        amount: 30,
        units:'kly'

    },

    imports:[]

}


const fee = 2;

const nonce = await web1337.getAccount(shardID,keypair.pub).then(account=>account.nonce+1);

const txType = "WVM_CALL";

let tx = web1337.createEd25519Transaction(shardID,txType,keypair.pub,keypair.prv,nonce,fee,payload);

console.log(tx);

web1337.sendTransaction(tx).then(value =>{

    console.log('Sent => ',value)

    console.log(`TX ID is => `,web1337.blake3(tx.sig))

}).catch(err=>console.error('Error with unstaking: ',err));
```

Output:

```code-runner-output
{
  v: 0,
  creator: '3JAeBnsMedzxjCMNWQYcAXtwGVE9A5DBQyXgWBujtL9R',
  type: 'WVM_CALL',
  nonce: 2,
  fee: 2,
  payload: {
    shard: 'shard_0',
    contractID: 'system/staking',
    method: 'unstake',
    gasLimit: 0,
    params: {
      poolPubKey: '9GQ46rqY238rk2neSwgidap9ww5zbAN4dyqyC7j5ZnBK',
      amount: 30,
      units: 'kly'
    },
    imports: []
  },
  sigType: 'D',
  sig: 'YkfiMMLFopmxwuf3pmuKgNrpcdOuhxg3D+szrMQyIwk6DcUCS0Tl4taNIbGX1nGsQRMXiuNpKASxqyUCJxZ0Cg=='
}
Sent =>  { status: 'OK' }
TX ID is =>  ee8053b379a44662252afcd42a316e152f47a45353f7accd294a9ee321a38aed
```

### 2. Check the unstaking tx status

Go to explorer and try to find your transaction

<figure><img src="../../../.gitbook/assets/image (50).png" alt=""><figcaption></figcaption></figure>

Transaction finished with `Success` :green\_circle: status

<figure><img src="../../../.gitbook/assets/image (51).png" alt=""><figcaption></figcaption></figure>

### 3. Wait untill the next epoches

There is also a delay for the unstaking procedure. Since the transaction was in epoch 61

<figure><img src="../../../.gitbook/assets/image (52).png" alt=""><figcaption></figcaption></figure>

Then you will receive the coins in epoch **63**

### 4. Check that you have less coins in pool and receive your coins back

We waited for some time and now the epoch is **64** (even more than 63).

Let's try to go to the pool page and see the list of stakers now:

<figure><img src="../../../.gitbook/assets/image (54).png" alt=""><figcaption></figcaption></figure>

Now in pool you have 70 coins instead of 100

<figure><img src="../../../.gitbook/assets/image (53).png" alt=""><figcaption></figcaption></figure>

And if you visit the page of your account you will see that 30 coins was sent to your address back

<figure><img src="../../../.gitbook/assets/image (55).png" alt=""><figcaption></figcaption></figure>

Calculation:

```
Init balance: 20_000_000 (20 millions)

Balance after staking: 20_000_000 - 100(staking) - 2(fee) = 19_999_898

Balance after unstaking: 19_999_898 + 30(unstaking) - 2(fee) = 19_999_926
```

As you can see, everything is correct [✅](https://emojipedia.org/check-mark-button)
