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
import Web1337,{bls} from '../../index.js'


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

###

### BLS => BLS(multisig address) transaction

###

### BLS => TBLS(thresholdsig address) transaction

###

### BLS => PostQuantum(Dilithium/BLISS) transaction

