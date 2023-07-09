# 🤝 BLS multisig transactions

## Useful links

{% embed url="https://mastering.klyntar.org/beginning/cryptography/multi-threshold-aggregated-signatures#bls12-381" %}
General information
{% endembed %}

{% embed url="https://mastering.klyntar.org/beginning/cryptography/multi-threshold-aggregated-signatures#demonstration" %}
Demonstration
{% endembed %}

Shortly, the typical keypair looks like this:

```javascript
{
   "privateKey":"caad28a7da57fb28879bfdd886714629e4152553a28540caed6f51531f5af61a",
   "pubKey":"7QMxLg6GJwQYjc8B2WBEvaQq3TfeBhgBqHGneFu43oLfwhovrPGykbkUGe94FacSuG"
}
```

* 48-bytes Base58 ecoded public key(used as address)
* 32-bytes hexadecimal private key
* 96-bytes Base64 encoded signature

### Generate BLS keypair(s)

{% hint style="success" %}
Read detailed information [here](default-ed25519-transactions.md#ed25519-greater-than-bls-multisig-address-transaction)
{% endhint %}

### BLS => Ed25519 transaction

```javascript
import Web1337,{bls} from '@klyntar/web1337';


let web1337 = new Web1337({

    symbioteID:'aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa',
    workflowVersion:0,
    nodeURL: 'http://localhost:7332'

});



const publicKey1 = '6Pz5B3xLQKDxtnESqk3tWnsd4kwkVYLNsvgKJy31HssK3eGSy7Tvratk5pZNFW5z8S';
const privateKey1 = '6bcdb86cd8f9d24e9fe934905431de5c224499af8788bf12cae8597f0af4cb23';

const publicKey2 = '5uj1Sgrvo9mwF6v8Z3Kxe4arQcu53Q2z9QBYsRPWCdo972jBeL8hD3Kmo4So3dSLt5';
const privateKey2 = 'e5ca94e2c60cbb3ca9d5f9c233a99f01e3250ef62bd48fbc09caf25ae70040b5';

const publicKey3 = '6ZmLf52hp5FgCxuaNJ6Jmi2M7FSJTr115WRMuV1yCvEihepnEKJBhgopDMpUKLpe2F';
const privateKey3 = 'ead8698b5ef285e6019e22e54dfe2c592c020946e803df8c6e79f98baf849d48';

const publicKey4 = '61tPxmio9Y21GtkiyYG3qTkreKfqm6ktk7MVk2hxqiXVURXxM6qNb9vPfvPPbhpMDn';
const privateKey4 = '414230b72c59ac8db6ec47b629607057edaa5c7c553d44b4b9fd1c0090141c5a';

// General pubkey retrieved from 4 friends public keys
const rootPubKey = bls.aggregatePublicKeys([publicKey1,publicKey2,publicKey3,publicKey4]);

// Root pubkey of 3 friends who want to use account
const aggregatePubKeyOfThreeFriends = bls.aggregatePublicKeys([publicKey1,publicKey2,publicKey3]);

// Array of pubkeys of friends who dissenting to sign
const arrayOfAfkSigners = [publicKey4];


// ID of subchain where you want to transfer KLY or call contract
const subchain = '7GPupbq1vtKUgaqVeHiDbEJcxS7sSjwPnbht4eRaDBAEJv8ZKHNCSu2Am3CuWnHjta';

// Typical Ed25519
const recipient = 'nXSYHp74u88zKPiRi7t22nv4WCBHXUBpGrVw3V93f2s';

const from = rootPubKey;

const nonce = 0;

const fee = 1;

const amountInKLY = 13.37;



// Now 3 sides need to sign the same tx data locally(on their devices)

let signature1 = await web1337.signDataForMultisigTxAsOneOfTheActiveSigners(subchain,privateKey1,aggregatePubKeyOfThreeFriends,arrayOfAfkSigners,nonce,fee,recipient,amountInKLY);

let signature2 = await web1337.signDataForMultisigTxAsOneOfTheActiveSigners(subchain,privateKey2,aggregatePubKeyOfThreeFriends,arrayOfAfkSigners,nonce,fee,recipient,amountInKLY);

let signature3 = await web1337.signDataForMultisigTxAsOneOfTheActiveSigners(subchain,privateKey3,aggregatePubKeyOfThreeFriends,arrayOfAfkSigners,nonce,fee,recipient,amountInKLY);


console.log('\n============ Partial signatures ============\n');
console.log(`[0] ${signature1}`);
console.log(`[1] ${signature2}`);
console.log(`[2] ${signature3}`);


// Now, build the transaction, aggregate signatures and send to network

let aggregatedSignatureOfActive = bls.aggregateSignatures([signature1,signature2,signature3]);

let finalTx = await web1337.createMultisigTransaction(from,aggregatePubKeyOfThreeFriends,aggregatedSignatureOfActive,arrayOfAfkSigners,nonce,fee,recipient,amountInKLY);



console.log('\n============ Final signed tx ready to be deployed ============\n');
console.log(finalTx);
```

Output:

```code-runner-output
============ Partial signatures ============

[0] pmrIRfD4+F9bn4zh79wnsc06Q4qaaKO6i305OCoths7orSLMYuM2Drvt4D05mjEyBwku7ePFXubj++syTgf9go8S9ZKexLh9BGlxjzjJB8o41437wAYUNexqeDHsAM3k
[1] gpveFYOucx7yW9ON0vLRUpnkBmDcJL1cAuHee8glj6w1JCmeDf5gQvUbfgtJHI7ACBHjViGe0kE8tks1gRfxknklaJ9e4y4rtCipXUBYr/nYkSHhCSpOFqUm/hO43LF1
[2] kbwUBydyWyPF4/ufRTSSvTIL2mPNyynKHsZdoS+b4sHplZTwKZs6lqtebBmTzadZE0TqtxTXxATe6WdSfipS5Ky8zAdBHH4FvbFFhhpoBOr4lbBit8gPpqkDCbvH6FNq

============ Final signed tx ready to be deployed ============

{
  v: 0,
  creator: '68Bpgi6MbRX9q3T9h8DDWomPGu85HqWSfPMT23r6g29xyn1dN7qfquwxpfFNMdMpU1',
  type: 'TX',
  nonce: 0,
  fee: 1,
  payload: {
    type: 'M',
    active: '6Li9DhTfbx9f6Z964ApK6b8Y6MZGrgik2Gf2scXM1bTKsKBddiLL4Duba8qP1rLK26',
    afk: [
      '61tPxmio9Y21GtkiyYG3qTkreKfqm6ktk7MVk2hxqiXVURXxM6qNb9vPfvPPbhpMDn'
    ],
    to: 'nXSYHp74u88zKPiRi7t22nv4WCBHXUBpGrVw3V93f2s',
    amount: 13.37
  },
  sig: 'lWGvpM+eKRc3bz0SXp6hX/8zxcASlp1A/ip/RrcRkqDaBpR3QJ8OIeTiD7V1Xm05FjudsOVADJJJP/Cvrx//cn8t2gdDu00zMbCmqreCDu4Hq1Ap8ZTSwFIpIINKf5my'
}
```

Now, you can deploy this transaction:

```javascript
let result = await web1337.sendTransaction(finalTx)
```



### BLS => BLS(multisig address) transaction

```javascript
import Web1337,{bls} from '@klyntar/web1337';


let web1337 = new Web1337({

    symbioteID:'aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa',
    workflowVersion:0,
    nodeURL: 'http://localhost:7332'

});



const publicKey1 = '6Pz5B3xLQKDxtnESqk3tWnsd4kwkVYLNsvgKJy31HssK3eGSy7Tvratk5pZNFW5z8S';
const privateKey1 = '6bcdb86cd8f9d24e9fe934905431de5c224499af8788bf12cae8597f0af4cb23';

const publicKey2 = '5uj1Sgrvo9mwF6v8Z3Kxe4arQcu53Q2z9QBYsRPWCdo972jBeL8hD3Kmo4So3dSLt5';
const privateKey2 = 'e5ca94e2c60cbb3ca9d5f9c233a99f01e3250ef62bd48fbc09caf25ae70040b5';

const publicKey3 = '6ZmLf52hp5FgCxuaNJ6Jmi2M7FSJTr115WRMuV1yCvEihepnEKJBhgopDMpUKLpe2F';
const privateKey3 = 'ead8698b5ef285e6019e22e54dfe2c592c020946e803df8c6e79f98baf849d48';

const publicKey4 = '61tPxmio9Y21GtkiyYG3qTkreKfqm6ktk7MVk2hxqiXVURXxM6qNb9vPfvPPbhpMDn';
const privateKey4 = '414230b72c59ac8db6ec47b629607057edaa5c7c553d44b4b9fd1c0090141c5a';

// General pubkey retrieved from 4 friends public keys
const rootPubKey = bls.aggregatePublicKeys([publicKey1,publicKey2,publicKey3,publicKey4]);

// Root pubkey of 3 friends who want to use account
const aggregatePubKeyOfThreeFriends = bls.aggregatePublicKeys([publicKey1,publicKey2,publicKey3]);

// Array of pubkeys of friends who dissenting to sign
const arrayOfAfkSigners = [publicKey4];


// ID of subchain where you want to transfer KLY or call contract
const subchain = '7GPupbq1vtKUgaqVeHiDbEJcxS7sSjwPnbht4eRaDBAEJv8ZKHNCSu2Am3CuWnHjta';

// Now it's multisig
const recipient = '7QMxLg6GJwQYjc8B2WBEvaQq3TfeBhgBqHGneFu43oLfwhovrPGykbkUGe94FacSuG';

// Since recipient is multisig - add the final <rev_t> param. Imagine that in this case it's 1(for example, when 2/3, so 1 address of 3 can be dissenting)
const rev_t = 1;

const from = rootPubKey;

const nonce = 0;

const fee = 1;

const amountInKLY = 13.37;


// Now 3 sides need to sign the same tx data locally(on their devices)

let signature1 = await web1337.signDataForMultisigTxAsOneOfTheActiveSigners(subchain,privateKey1,aggregatePubKeyOfThreeFriends,arrayOfAfkSigners,nonce,fee,recipient,amountInKLY,rev_t);

let signature2 = await web1337.signDataForMultisigTxAsOneOfTheActiveSigners(subchain,privateKey2,aggregatePubKeyOfThreeFriends,arrayOfAfkSigners,nonce,fee,recipient,amountInKLY,rev_t);

let signature3 = await web1337.signDataForMultisigTxAsOneOfTheActiveSigners(subchain,privateKey3,aggregatePubKeyOfThreeFriends,arrayOfAfkSigners,nonce,fee,recipient,amountInKLY,rev_t);


console.log('\n============ Partial signatures ============\n');
console.log(`[0] ${signature1}`);
console.log(`[1] ${signature2}`);
console.log(`[2] ${signature3}`);


// Now, build the transaction, aggregate signatures and send to network

let aggregatedSignatureOfActive = bls.aggregateSignatures([signature1,signature2,signature3]);

let finalTx = await web1337.createMultisigTransaction(from,aggregatePubKeyOfThreeFriends,aggregatedSignatureOfActive,arrayOfAfkSigners,nonce,fee,recipient,amountInKLY,rev_t);



console.log('\n============ Final signed tx ready to be deployed ============\n');
console.log(finalTx);
```

Output:

```code-runner-output
============ Partial signatures ============

[0] mcJ1bbEGPmSb0hKDlNUkPKfk+lSKU+KABoc10GRxJtK/4Ak9U7pdAXtS+UV6M9RqGIHZ/kAV7oi3wr8B0TURerUWxYgl/WQjKinIyGPmJJmd5RNHyYkgr+N5GeS3/KzG
[1] sSOR7RPtWbtTAgw41R146x85Xdg9JCnCGAQ1/kzE0LsyDpdtFTisQ1Y+5/J9mlsBBY6EmiQopGHT/puSIZGR76/ErG7FJnpTnh6dAyW4xyidT5/8Zv4VAYPsBGXdlnsZ
[2] spfLhunZ8GhDGB5LiUwwL1jZp0ZFzMXlhfwPvsFQvOc7Ef5DMgv3YZIUK3JK+IalAZivbPHHPGp9tN15rPdh4SeHXEz/6DuoUhzrr0Syr9Ze9a//qeHQ5DvhEIvg9Lrq

============ Final signed tx ready to be deployed ============

{
  v: 0,
  creator: '68Bpgi6MbRX9q3T9h8DDWomPGu85HqWSfPMT23r6g29xyn1dN7qfquwxpfFNMdMpU1',
  type: 'TX',
  nonce: 0,
  fee: 1,
  payload: {
    type: 'M',
    active: '6Li9DhTfbx9f6Z964ApK6b8Y6MZGrgik2Gf2scXM1bTKsKBddiLL4Duba8qP1rLK26',
    afk: [
      '61tPxmio9Y21GtkiyYG3qTkreKfqm6ktk7MVk2hxqiXVURXxM6qNb9vPfvPPbhpMDn'
    ],
    to: '7QMxLg6GJwQYjc8B2WBEvaQq3TfeBhgBqHGneFu43oLfwhovrPGykbkUGe94FacSuG',
    amount: 13.37,
    rev_t: 1
  },
  sig: 'iXJTgKSBfsykK9upqWBAeb/D+FF0t0Ho6NwxyvqsSOQCGs/bvtabqLIvuVztk/EmGC+hnKPD/PTJaop5xP1wx0yVQXvcivWb7dLvoWHmdr25p8uZk5YMD9d0gFZY+txE'
}
```

###

### BLS => TBLS(thresholdsig address) transaction

Here we omit the part of explanations because you just need to know the TBLS root public key of group to send transactions to.

```javascript
import Web1337,{bls} from '@klyntar/web1337';


let web1337 = new Web1337({

    symbioteID:'aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa',
    workflowVersion:0,
    nodeURL: 'http://localhost:7332'

});



const publicKey1 = '6Pz5B3xLQKDxtnESqk3tWnsd4kwkVYLNsvgKJy31HssK3eGSy7Tvratk5pZNFW5z8S';
const privateKey1 = '6bcdb86cd8f9d24e9fe934905431de5c224499af8788bf12cae8597f0af4cb23';

const publicKey2 = '5uj1Sgrvo9mwF6v8Z3Kxe4arQcu53Q2z9QBYsRPWCdo972jBeL8hD3Kmo4So3dSLt5';
const privateKey2 = 'e5ca94e2c60cbb3ca9d5f9c233a99f01e3250ef62bd48fbc09caf25ae70040b5';

const publicKey3 = '6ZmLf52hp5FgCxuaNJ6Jmi2M7FSJTr115WRMuV1yCvEihepnEKJBhgopDMpUKLpe2F';
const privateKey3 = 'ead8698b5ef285e6019e22e54dfe2c592c020946e803df8c6e79f98baf849d48';

const publicKey4 = '61tPxmio9Y21GtkiyYG3qTkreKfqm6ktk7MVk2hxqiXVURXxM6qNb9vPfvPPbhpMDn';
const privateKey4 = '414230b72c59ac8db6ec47b629607057edaa5c7c553d44b4b9fd1c0090141c5a';

// General pubkey retrieved from 4 friends public keys
const rootPubKey = bls.aggregatePublicKeys([publicKey1,publicKey2,publicKey3,publicKey4]);

// Root pubkey of 3 friends who want to use account
const aggregatePubKeyOfThreeFriends = bls.aggregatePublicKeys([publicKey1,publicKey2,publicKey3]);

// Array of pubkeys of friends who dissenting to sign
const arrayOfAfkSigners = [publicKey4];


// ID of subchain where you want to transfer KLY or call contract
const subchain = '7GPupbq1vtKUgaqVeHiDbEJcxS7sSjwPnbht4eRaDBAEJv8ZKHNCSu2Am3CuWnHjta';

// Now it's TBLS
const recipient = 'bedc88644f0deea4c0a77ba687712f494a1af7d8869f09768a0db42284f89d17b7b9225e0c87c2cb5511907dfd5eae3a53d789298721039e833770de29595880';

const from = rootPubKey;

const nonce = 0;

const fee = 1;

const amountInKLY = 13.37;


// Now 3 sides need to sign the same tx data locally(on their devices)

let signature1 = await web1337.signDataForMultisigTxAsOneOfTheActiveSigners(subchain,privateKey1,aggregatePubKeyOfThreeFriends,arrayOfAfkSigners,nonce,fee,recipient,amountInKLY);

let signature2 = await web1337.signDataForMultisigTxAsOneOfTheActiveSigners(subchain,privateKey2,aggregatePubKeyOfThreeFriends,arrayOfAfkSigners,nonce,fee,recipient,amountInKLY);

let signature3 = await web1337.signDataForMultisigTxAsOneOfTheActiveSigners(subchain,privateKey3,aggregatePubKeyOfThreeFriends,arrayOfAfkSigners,nonce,fee,recipient,amountInKLY);


console.log('\n============ Partial signatures ============\n');
console.log(`[0] ${signature1}`);
console.log(`[1] ${signature2}`);
console.log(`[2] ${signature3}`);


// Now, build the transaction, aggregate signatures and send to network

let aggregatedSignatureOfActive = bls.aggregateSignatures([signature1,signature2,signature3]);

let finalTx = await web1337.createMultisigTransaction(from,aggregatePubKeyOfThreeFriends,aggregatedSignatureOfActive,arrayOfAfkSigners,nonce,fee,recipient,amountInKLY);



console.log('\n============ Final signed tx ready to be deployed ============\n');
console.log(finalTx);

```

Output:

```code-runner-output
============ Partial signatures ============

[0] qA5ToDOzpWAYLHY2LB2HuMqxmEXHFyXpKYhFr/0lQe7sF+BLpwKjsN17qBi54aRRGZtFGcOPDfxmGmx59z5qyJPjB1vY/kkiBrFLPE+UqnxRfRY5m+17msu+gagr67zU
[1] hKY+R9E1RTSWg8DnCO7QW4EU8s+jE+MiWTaq7rwg9LMMrCFQc/Cn+wBJf7CJBJh3AGoeO2/sUCdCoNemjY8yMk3XkevWF1r9kqMhsX3PQyr0CI5YR42pOpS3mK2xtGew
[2] mIUSk/MjcEC1y0dtZ2vgXUuV1IJtIanWNxVJ+wSG0U053L5/cbBr3BycRUPRx/5oENz4+6z0ToQL1OSesHHxtFsTUUyfhI9k7E1YPAgJ+zAvvi0ObzeUM3LThWkhP4OA

============ Final signed tx ready to be deployed ============

{
  v: 0,
  creator: '68Bpgi6MbRX9q3T9h8DDWomPGu85HqWSfPMT23r6g29xyn1dN7qfquwxpfFNMdMpU1',
  type: 'TX',
  nonce: 0,
  fee: 1,
  payload: {
    type: 'M',
    active: '6Li9DhTfbx9f6Z964ApK6b8Y6MZGrgik2Gf2scXM1bTKsKBddiLL4Duba8qP1rLK26',
    afk: [
      '61tPxmio9Y21GtkiyYG3qTkreKfqm6ktk7MVk2hxqiXVURXxM6qNb9vPfvPPbhpMDn'
    ],
    to: 'bedc88644f0deea4c0a77ba687712f494a1af7d8869f09768a0db42284f89d17b7b9225e0c87c2cb5511907dfd5eae3a53d789298721039e833770de29595880',
    amount: 13.37
  },
  sig: 'ugCi0fPlhXJFa0xwHQBCsJ+p+vjZtM0ebD2JMXJLZYaphx1Wm0Sc9aJKV5ed9EFiB+ov9z/AqSg4vakie+wieu5TCRNrxg30/hJaMwrfWzIjdctn/u7QufFViJBC3XND'
}
```

###

### BLS => PostQuantum(Dilithium/BLISS) transaction

Here we omit the part of explanations because you just need to know the hash of public key to send transactions to.

```javascript
import Web1337,{bls} from '@klyntar/web1337';


let web1337 = new Web1337({

    symbioteID:'aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa',
    workflowVersion:0,
    nodeURL: 'http://localhost:7332'

});



const publicKey1 = '6Pz5B3xLQKDxtnESqk3tWnsd4kwkVYLNsvgKJy31HssK3eGSy7Tvratk5pZNFW5z8S';
const privateKey1 = '6bcdb86cd8f9d24e9fe934905431de5c224499af8788bf12cae8597f0af4cb23';

const publicKey2 = '5uj1Sgrvo9mwF6v8Z3Kxe4arQcu53Q2z9QBYsRPWCdo972jBeL8hD3Kmo4So3dSLt5';
const privateKey2 = 'e5ca94e2c60cbb3ca9d5f9c233a99f01e3250ef62bd48fbc09caf25ae70040b5';

const publicKey3 = '6ZmLf52hp5FgCxuaNJ6Jmi2M7FSJTr115WRMuV1yCvEihepnEKJBhgopDMpUKLpe2F';
const privateKey3 = 'ead8698b5ef285e6019e22e54dfe2c592c020946e803df8c6e79f98baf849d48';

const publicKey4 = '61tPxmio9Y21GtkiyYG3qTkreKfqm6ktk7MVk2hxqiXVURXxM6qNb9vPfvPPbhpMDn';
const privateKey4 = '414230b72c59ac8db6ec47b629607057edaa5c7c553d44b4b9fd1c0090141c5a';

// General pubkey retrieved from 4 friends public keys
const rootPubKey = bls.aggregatePublicKeys([publicKey1,publicKey2,publicKey3,publicKey4]);

// Root pubkey of 3 friends who want to use account
const aggregatePubKeyOfThreeFriends = bls.aggregatePublicKeys([publicKey1,publicKey2,publicKey3]);

// Array of pubkeys of friends who dissenting to sign
const arrayOfAfkSigners = [publicKey4];


// ID of subchain where you want to transfer KLY or call contract
const subchain = '7GPupbq1vtKUgaqVeHiDbEJcxS7sSjwPnbht4eRaDBAEJv8ZKHNCSu2Am3CuWnHjta';

// Now it's PQC
const recipient = 'f5091405e28455880fc4191cbda9f1e57f72399e732222d4639294b66d3a5076';

const from = rootPubKey;

const nonce = 0;

const fee = 1;

const amountInKLY = 13.37;


// Now 3 sides need to sign the same tx data locally(on their devices)

let signature1 = await web1337.signDataForMultisigTxAsOneOfTheActiveSigners(subchain,privateKey1,aggregatePubKeyOfThreeFriends,arrayOfAfkSigners,nonce,fee,recipient,amountInKLY);

let signature2 = await web1337.signDataForMultisigTxAsOneOfTheActiveSigners(subchain,privateKey2,aggregatePubKeyOfThreeFriends,arrayOfAfkSigners,nonce,fee,recipient,amountInKLY);

let signature3 = await web1337.signDataForMultisigTxAsOneOfTheActiveSigners(subchain,privateKey3,aggregatePubKeyOfThreeFriends,arrayOfAfkSigners,nonce,fee,recipient,amountInKLY);


console.log('\n============ Partial signatures ============\n');
console.log(`[0] ${signature1}`);
console.log(`[1] ${signature2}`);
console.log(`[2] ${signature3}`);


// Now, build the transaction, aggregate signatures and send to network

let aggregatedSignatureOfActive = bls.aggregateSignatures([signature1,signature2,signature3]);

let finalTx = await web1337.createMultisigTransaction(from,aggregatePubKeyOfThreeFriends,aggregatedSignatureOfActive,arrayOfAfkSigners,nonce,fee,recipient,amountInKLY);



console.log('\n============ Final signed tx ready to be deployed ============\n');
console.log(finalTx);
```

Output:

```code-runner-output
============ Partial signatures ============

[0] k9/aKDekSg61Q/YYcFcwxqIKYFo6FCLYBQQ7T3Sqo4z/a0d3SmSN7bjK/e0g02EnGRcdngynj5ysK8sbYggk4XxJL/dhaKflkVRiKcyHn7iWuDxCVwIwaLDJz0jLWF7z
[1] suEEb7Tq6aQYpLV8HUe817WFpERqRsxsAivv2UdWdgA019BD1jsnZZAq524rgFMmGQl/TZQsTBVw6EvMul3/UTu2PU7EKeqof+Id/J+qfW269j7zrQciPu/tQLHXb6zw
[2] sd98Ri/V6H2Ww5JH4c+jKwA8Q3sajL9gtruYc/MDWHT5gX8p9wUV8dhhqowKtCAsDy3TdF9SuCh5cjggLaH/fsQuCyPU4w07wRnqteo926KVG02B/4Kt+Bm0zej7mLdT

============ Final signed tx ready to be deployed ============

{
  v: 0,
  creator: '68Bpgi6MbRX9q3T9h8DDWomPGu85HqWSfPMT23r6g29xyn1dN7qfquwxpfFNMdMpU1',
  type: 'TX',
  nonce: 0,
  fee: 1,
  payload: {
    type: 'M',
    active: '6Li9DhTfbx9f6Z964ApK6b8Y6MZGrgik2Gf2scXM1bTKsKBddiLL4Duba8qP1rLK26',
    afk: [
      '61tPxmio9Y21GtkiyYG3qTkreKfqm6ktk7MVk2hxqiXVURXxM6qNb9vPfvPPbhpMDn'
    ],
    to: 'f5091405e28455880fc4191cbda9f1e57f72399e732222d4639294b66d3a5076',
    amount: 13.37
  },
  sig: 'i1JskWEYiJvJQDMhi4rDBUI1he8kefOOiDJJa4fYoTxrPBVtIzSK5FXlBnGeEKwXDiw+c9bzYpMcMqu8LGJ8Pm7cWjCP6DYhKBnFqBN5Bs5tvngF+UWmPg8xzgeWejEg'
}
```
