---
description: Get to know how to use TBLS accounts in your DApps on KLY
---

# 🛡 TBLS thresholdsig transactions

## Useful links

{% embed url="https://mastering.klyntar.org/beginning/cryptography/multi-threshold-aggregated-signatures#threshold-signatures-tbls" %}
General info about TBLS
{% endembed %}

{% embed url="https://mastering.klyntar.org/beginning/cryptography/multi-threshold-aggregated-signatures#threshold-signatures" %}
Example of usage
{% endembed %}

Threshold signatures may seem more complex than the multi-signatures described earlier. And yet, with a large number of parties involved, it is recommended to use TBLS, since the size of such a transaction will be much smaller.

For example, if we have 100 parties that together manage a certain account and it was decided that the decision threshold is 60% (60 parties), then you will need to send 40 public keys in the `afkVoters` array.

At the same time, in TBLS, the identifier of the entire group of signers is a 96-byte root key and in order for the network to understand that the initially set threshold has been reached (it doesn’t matter whether 60 participants, 63, 87, or all 100 agree with the decision, the main thing is more than initialy defined threshold) you need to provide only 1 signature. By verifying it using the master key, the network will allow the group to complete the transaction.

## Use case

There are 6 friends who want to share some cryptocurrency when they go to the festival. At the same time, there was an agreement that the costs agree with the majority. That is, 4 out of 6 friends must agree to a certain purchase or other type of expenditure. For this, the friends agreed that each of them would replenish the joint wallet with 500 units of KLY. Well, let's help them generate such a wallet and everything else for honest money management.

### Step 1 - generation

So, consider the signature of the first function generateTBLS() It has the following parameters:

* `threshold` - threshold. In our case, it will be equal to 4
* `myPubId` - choose some initial numeric ID for yourself
* `pubKeysArr` - an array of other identifiers. Our ID must also be included in this array

Let's generate

```javascript
import {tbls} from '@klyntar/web1337'



let threshold = 4 // 4/6

let myPubId = 1 // our ID

let pubKeysArr = [1,2,3,4,5,6] // array of identifiers of all members


let {verificationVector,secretShares,id} = tbls.generateTBLS(threshold,myPubId,pubKeysArr)


console.log('Use with VV => ',verificationVector)
console.log('Array of secret shares to share among friends => ',secretShares)
console.log('Your id => ',id)

```

Output:

```code-runner-output
Use with VV =>  [
    'a7e972de2362e725831efdebe080842f9ee275915a718dea43e561fa0d1af41b3192f849e1128d69f62018b9e7a9387f8b8611488a72480305991d480dad0e11',
    '0003e06c68ba56e45db0ee64a06a6935554e0b7f4849da26223cf831cdfd341bf9fe628590eb436197145206eb8f2eb0baef673eff23f63eaa6fe74e59da7c05',
    '53ad69e15e782a60fe63ee07d9e768dff002dfc54fa6fd6e4ea4531fc00d2700472fb244c9fa740ec354e417e8bd241c875caf29c4abad251a042d7becca4d15',
    '21b090515bf15d9ec6c4f4dd4df5f49a64f21e9b9c268b91b09bd77adcf2d517303fe3cf3bdd710ab783ae413476f7ccd050c1be02524a42484243b47767b71f'
]
Array of secret shares to share among friends =>  [
    "79b359142147dd33383e32ac2476577d7760245a644fdad8730b22fad5726b0d",
    "2ba86e244fe8ad6ea665598257e1a8700b433a2e5bd967116fcf982b9db0df05",
    "af5f947ef9db70f50a8a5a5ee07170e78974c641b52942681a229605ff860e1f",
    "0f0389cb5fdc97c14d3c494ada022bafb78988efb891258b9c487b9475febd0a",
    "c8472e66991a8a1121a5f89f3ae5c1200455f77da838b965f8b9ef5af0701e07",
    "565a252bc870bdcf046fb76a25c45c56cab9d4a5624e794319dd13dbc7e9781f"

]
Your id => 4bf5122f344554c53bde2ebb8cd2b7e3d1600ad631c385a5d7cce23c7785451a
```

Cool, now let's figure out what we have:

* `verificationVector` is a verification vector that we have to send to all other 5 friends
* `secretShares` are something that we have to send to friends(1 share for 1 friend). That is, the first share(secretShares\[0]) goes to the friend with ID=1 (this is you), the second share (secretShares\[1]) goes to the second friend - the one with ID=2, and so on. There are 6 of them here, so it is clear that everyone has a share
* &#x20;`id` is our ID that we will now use for further generations

### Step 2 - friends do the same

A similar procedure is performed locally by yourself and 5 of your friends. Each of them will receive its ID, each will receive an array of 6 shares and a verification vector.

{% hint style="info" %}
An important point is to agree a set of identifiers with your friends
{% endhint %}

I mean that the other 6 friends must also use the same ID array. And your ID will be the same for all friends. That is:

* For a friend with ID=1 - takes share 1 for himself
* For a friend with ID=2 - takes layer 2 for himself

And so on

### Step 3 - get the root public key

At this stage, friends will receive 96-bytes hex encoded address in the KLY network to which they can send funds for shared use. For this purpose, they exchange verification vectors. When you receive 5 verification vectors from 5 other friends, then add your sixth one and call the `deriveGroupPubTBLS()` function. But before we break it down, let's create some imaginary dataset with the data of the other 5 friends. So, let it look like this:

```javascript
/*

------------------------------- FRIEND 1 -------------------------------


Verification vector

[
    
    "a7e972de2362e725831efdebe080842f9ee275915a718dea43e561fa0d1af41b3192f849e1128d69f62018b9e7a9387f8b8611488a72480305991d480dad0e11"
    "0003e06c68ba56e45db0ee64a06a6935554e0b7f4849da26223cf831cdfd341bf9fe628590eb436197145206eb8f2eb0baef673eff23f63eaa6fe74e59da7c05"
    "53ad69e15e782a60fe63ee07d9e768dff002dfc54fa6fd6e4ea4531fc00d2700472fb244c9fa740ec354e417e8bd241c875caf29c4abad251a042d7becca4d15"
    "21b090515bf15d9ec6c4f4dd4df5f49a64f21e9b9c268b91b09bd77adcf2d517303fe3cf3bdd710ab783ae413476f7ccd050c1be02524a42484243b47767b71f"
    
]


Array of shares

[
    "79b359142147dd33383e32ac2476577d7760245a644fdad8730b22fad5726b0d"      it's your share
    "2ba86e244fe8ad6ea665598257e1a8700b433a2e5bd967116fcf982b9db0df05"      send to friend 2
    "af5f947ef9db70f50a8a5a5ee07170e78974c641b52942681a229605ff860e1f"      send to friend 3
    "0f0389cb5fdc97c14d3c494ada022bafb78988efb891258b9c487b9475febd0a"      send to friend 4
    "c8472e66991a8a1121a5f89f3ae5c1200455f77da838b965f8b9ef5af0701e07"      send to friend 5
    "565a252bc870bdcf046fb76a25c45c56cab9d4a5624e794319dd13dbc7e9781f"      send to friend 6
]

ID = 4bf5122f344554c53bde2ebb8cd2b7e3d1600ad631c385a5d7cce23c7785451a


------------------------------- FRIEND 2 -------------------------------

Verification vector

[
    
    "6f4c90be986832a71aa5b8d25007d27adc3aa267acdca189a42b9a17efc8dd17bcde25c173d18eeba5bae067e52cc5ba16fd3099af04804c8a40b79ac595c617"
    "775449cde76ce6333c53317e6b6a1eb47ee87af2bc56548bb38fea5da266dd0704ae043c288f9fb792b870128738ad7360454724325b5260bb45e332a3d51221"
    "dd5a9d1f3236dd7e091dcb4272fb3cec624c22a0e59de283ce345603850e180e263806875158263b2bd46f0b7af4d78596e43008948431b07d174b6c8d6da388"
    "8176bdd4610dd21cdb6bfb9b526a9e3ee2804c57ac9b9094235a4c1947c2f20aa1282cc8dd5e2ec09183c677920106024aab0bd9a2ecaa5539ff9e1c4025d1a3"
]

Array of shares

[

    173fc58cb61188a8fad233a689baec3db34657c3bb8536b21cbad70db463130b        send to friend 1
    834ffe5b7794a5c8666210a98beaba4a48d344dd2e691ce4edb2196f6b2da317        it's your share
    a8e50fdc4d6f1f20047b7297ff54ac2d807cbfb6cd12e8f499e6eaf7b5feb010        send to friend 3
    50b2c925a3463136414715d0f99c594c1aa2d7e5857d7881f62dcdc7676c191a        send to friend 4
    d7f329f64448ddd56350a2a3a45d944679a2c4fc76ecb3770d760592b08c8b0a        send to friend 5
    2ddedf940ea38c9809735793cec9ded6d0d6e573181611c0f3efa604b6b92e1c        send to friend 6

]


ID=dbc1b4c900ffe48d575b5da5c638040125f65db0fe3e24494b76ea986457d906


------------------------------- FRIEND 3 -------------------------------

Verification vector

[
    "586797df712ba4dcb15e78a36e6ef4a3f9ba1b05ce3a6a9001f0cdfefbd8f8080a33a3182a9204657594ebc6749656166f119ab2a81515266f62ce10e327ca09"
    "62b9b08eadbbb20ef840923c89ac53b73aa31fa3e493097f49496693a795131b46dba854f76bfaf369beb77a7a80e2fe66525b16341a88022e1dc635bd7dc385"
    "2577181e0384bc99ceb066acf69323e236160796df37f6e688d3274e4e026a03997460021b39da01a8a1465566e08f6a455333742fb6646f11beeab2a8dd2420"
    "b2a2ace162f22b200c7aa807083bae80e69214dcdaa9708c66f81d1e8d4a0c18c3b459214292bd683c9b80809ea3e7e95288656caf0b34ec260b820a79b08520"
]

Array of shares

[

    82858da32c368bc8dea20ab9646d87f84eb478e51a45a55394710c96c71a080f        send to friend 1
    6a12a6abfe939015aca70e67c96e9768ad9af0e6541c62e82b51ca32176c8816        send to friend 2
    b607ae0d7be4bbbad607967c40841e67f8b992e15a44fc3d0dbc5a14a16f9b0e        it's your share
    73d57dfb5cd773a19a34052184d1f0b2576bc127ee1d92ae596c51bf3a6e2c24        send to friend 4
    d9dc10623e261047a0ac924595c356f5d9b192ab28f29276bf5c21206c143902        send to friend 5
    952e03949438a001aafb2b1e864635c25b229047fc85b3178dd1d049ecbdcd04        send to friend 6

]


ID=084fed08b978af4d7d196a7446a86b58009e636b611db16211b65a9aadff2905

------------------------------- FRIEND 4 -------------------------------

Verification vector

[
    "7a4a81ba14f4b32b3c7f061f718cc15b0ca3ab76196a7e9e62f70dcc0add10092906a237d27b034f541731df41fee38d2f4b5d8998678b7d06cad2532fbecc13"
    "6edfa6a70925257e952f161d941092bf30cd70d1f27a6cba53affbe9de7a450dfc02b5c500fbeb9ca2443df0a941269ad65c8bede560f033ae802d6499281a9e"
    "492a5db1fdf4f133e4d8e92c0637ae13a8da6f19bc93da4ed6b0736d8bc602063a229846a43e18b8ba39de1cdba274a11fcda3cf482e3ca908cdf3c3f7ffdb04"
    "8396e4d78a12580388c919b8a201036d48a1470c9684eb653ea89f458927730bc32e8f91763dfce581ff60125664cd6e1a20ed684677a6d8dccbb6b23c3a0182"
]

Array of shares

[

    89b3400f482b4bed1e1eba002df97c8a7933c435fdd990d043fd527255010406        send to friend 1
    36ae6a22edc96b63ae6bf5ca78445f62117d2aeb0606876b5b0cd321c5338b0a        send to friend 2
    2170ec403427f94086f4e4712aef6e0b719f58160d8acdd27e26eaaa95676e1a        send to friend 3
    88e7e3aea7506953618a4f6c3b09e52fa7e1f881f16c70b66ef67c2ba80c5c0e        it's your share
    3569f445bbc8274318a0a3e761c016610bcb8b2cb414b49a76979181f9a3480f        send to friend 5
    b7a2bc301f42f0aa7acff13015b5c28ddd13d8b3fee5f3f81978463bba5efb16        send to friend 6

]

ID=e52d9c508c502347344d8c07ad91cbd6068afc75ff6292f062a09ca381c89e11

```

We call the public key generation function of our group. In this case, we equate the concept of **public key/network address**. In any case, the address can be obtained from the public key. So, we have:

```javascript
let vv1 = 
[
    
    "a7e972de2362e725831efdebe080842f9ee275915a718dea43e561fa0d1af41b3192f849e1128d69f62018b9e7a9387f8b8611488a72480305991d480dad0e11",
    "0003e06c68ba56e45db0ee64a06a6935554e0b7f4849da26223cf831cdfd341bf9fe628590eb436197145206eb8f2eb0baef673eff23f63eaa6fe74e59da7c05",
    "53ad69e15e782a60fe63ee07d9e768dff002dfc54fa6fd6e4ea4531fc00d2700472fb244c9fa740ec354e417e8bd241c875caf29c4abad251a042d7becca4d15",
    "21b090515bf15d9ec6c4f4dd4df5f49a64f21e9b9c268b91b09bd77adcf2d517303fe3cf3bdd710ab783ae413476f7ccd050c1be02524a42484243b47767b71f"
]

let vv2 = 
[
    
    "6f4c90be986832a71aa5b8d25007d27adc3aa267acdca189a42b9a17efc8dd17bcde25c173d18eeba5bae067e52cc5ba16fd3099af04804c8a40b79ac595c617",
    "775449cde76ce6333c53317e6b6a1eb47ee87af2bc56548bb38fea5da266dd0704ae043c288f9fb792b870128738ad7360454724325b5260bb45e332a3d51221",
    "dd5a9d1f3236dd7e091dcb4272fb3cec624c22a0e59de283ce345603850e180e263806875158263b2bd46f0b7af4d78596e43008948431b07d174b6c8d6da388",
    "8176bdd4610dd21cdb6bfb9b526a9e3ee2804c57ac9b9094235a4c1947c2f20aa1282cc8dd5e2ec09183c677920106024aab0bd9a2ecaa5539ff9e1c4025d1a3"
]

let vv3 = 
[
    "586797df712ba4dcb15e78a36e6ef4a3f9ba1b05ce3a6a9001f0cdfefbd8f8080a33a3182a9204657594ebc6749656166f119ab2a81515266f62ce10e327ca09",
    "62b9b08eadbbb20ef840923c89ac53b73aa31fa3e493097f49496693a795131b46dba854f76bfaf369beb77a7a80e2fe66525b16341a88022e1dc635bd7dc385",
    "2577181e0384bc99ceb066acf69323e236160796df37f6e688d3274e4e026a03997460021b39da01a8a1465566e08f6a455333742fb6646f11beeab2a8dd2420",
    "b2a2ace162f22b200c7aa807083bae80e69214dcdaa9708c66f81d1e8d4a0c18c3b459214292bd683c9b80809ea3e7e95288656caf0b34ec260b820a79b08520"
]


let vv4 = 
[
    "7a4a81ba14f4b32b3c7f061f718cc15b0ca3ab76196a7e9e62f70dcc0add10092906a237d27b034f541731df41fee38d2f4b5d8998678b7d06cad2532fbecc13",
    "6edfa6a70925257e952f161d941092bf30cd70d1f27a6cba53affbe9de7a450dfc02b5c500fbeb9ca2443df0a941269ad65c8bede560f033ae802d6499281a9e",
    "492a5db1fdf4f133e4d8e92c0637ae13a8da6f19bc93da4ed6b0736d8bc602063a229846a43e18b8ba39de1cdba274a11fcda3cf482e3ca908cdf3c3f7ffdb04",
    "8396e4d78a12580388c919b8a201036d48a1470c9684eb653ea89f458927730bc32e8f91763dfce581ff60125664cd6e1a20ed684677a6d8dccbb6b23c3a0182"
]

let vv5 = 
[

    "16ac0490dc79eef786f88dc09c3a8fed15d371e745bdadd9c1f7a08b4d37d50aea565d857cad2b0706642f58caa09fc1073848491994f690b79e8076beca1018",
    "cad2ef53a9dfe02a92606a34f40868fd746209a4ede0c790e0d3ec5e4c90021019b65c7550174fc117aed8d0aa619b94ddd1c08b7a573b9810490e3bbc36ba01",
    "d88949fae2178d3e601abd22f47f64dfd9f8a532d6abd57b3957f0197480cc1b6effc65b04b475279c8de4c585ad8ba518cf35df406fdca8e5f51d5a6918360b",
    "019c9a96b62afb4210616244cdc42936b503814762132658a20c283b9dfe3d005c1c7415903ed953c71539961e911602c8a272d41d5d08184a5f269ee08a941e"
    
]

let vv6 = 
[
    "76e014c5dc7a2ae9d0271936ecac410bee3baf9b88b64ad6a166a1c2aab0991992fec4dc4a80447cb795fa07cd77cdce1c5680710ba016a041c6db4a782f368d",
    "117b180362500cdbd655d3bb0f97b22b57521302938ce5a34e4bcf3c2988ad10688116b0bedd6215aae314bfb240158834b87bd828c01454916d5066d1241691",
    "2c811310b69511e286bd898a6d0f5a306594357e4faee25e69b76cc21b74e5118d96a26e59263dbfe7819be40d7f404d57dca16c9c17ba3c0f95cd531c133593",
    "93d15991f4f0269886cc3e1b99bf2b79bba44ecdc00891bc4b8e6195e1b14e1557ddf84001ef06d5cc0a3f19502f320bfc6f7e11ed75eb21731f68629897461b"
]

let rootPubKey = tbls.deriveGroupPubTBLS([vv1,vv2,vv3,vv4,vv5,vv6])

console.log(rootPubKey) //8aae5ae3b51a6f4bba62f64ab44b2135339831f662f8ef9e004bffb1458faa045f2c9a640acb466c5c35e2c9af757ac7fad74e3865b8527452619236822f9797
```

### Step 4 - verify the shares

Can we now deposit funds into the wallet&#x20;

_<mark style="color:red;">8aae5ae3b51a6f4bba62f64ab44b2135339831f662f8ef9e004bffb1458faa045f2c9a640acb466c5c35e2c9af757ac7fad74e3865b8527452619236822f9 797</mark>_

and be sure that everything is OK? No, because you need to check the validity of the shares first to omit situation when maulicious actors(your "friends") can control this wallet without you. This is a VSS(Verifiable Secret Sharing) element - we need to make sure that each friend sent us a valid share. For this, there is a `verifyShareTBLS()` function in the module. Let's consider its parameters

* `hexMyId` - it's our ID from step 1
* `hexSomeSignerSecretKeyContribution` - it's share that we're going to verify(share from friend X).
* `hexSomeSignerVerificationVector` - it's verification vector by friend X

Ok, let's verify. From the friend 1 point orf view:

```javascript
/*

Verification vector

[
    
    "a7e972de2362e725831efdebe080842f9ee275915a718dea43e561fa0d1af41b3192f849e1128d69f62018b9e7a9387f8b8611488a72480305991d480dad0e11"
    "0003e06c68ba56e45db0ee64a06a6935554e0b7f4849da26223cf831cdfd341bf9fe628590eb436197145206eb8f2eb0baef673eff23f63eaa6fe74e59da7c05"
    "53ad69e15e782a60fe63ee07d9e768dff002dfc54fa6fd6e4ea4531fc00d2700472fb244c9fa740ec354e417e8bd241c875caf29c4abad251a042d7becca4d15"
    "21b090515bf15d9ec6c4f4dd4df5f49a64f21e9b9c268b91b09bd77adcf2d517303fe3cf3bdd710ab783ae413476f7ccd050c1be02524a42484243b47767b71f"
    
]


Shares

[
    "79b359142147dd33383e32ac2476577d7760245a644fdad8730b22fad5726b0d"      your share
    "2ba86e244fe8ad6ea665598257e1a8700b433a2e5bd967116fcf982b9db0df05"      send to friend 2
    "af5f947ef9db70f50a8a5a5ee07170e78974c641b52942681a229605ff860e1f"      send to friend 3
    "0f0389cb5fdc97c14d3c494ada022bafb78988efb891258b9c487b9475febd0a"      send to friend 4
    "c8472e66991a8a1121a5f89f3ae5c1200455f77da838b965f8b9ef5af0701e07"      send to friend 5
    "565a252bc870bdcf046fb76a25c45c56cab9d4a5624e794319dd13dbc7e9781f"      send to friend 6
]

ID = 4bf5122f344554c53bde2ebb8cd2b7e3d1600ad631c385a5d7cce23c7785451a

*/

```

Let's say we send the share to friend 4. So friend 4 will have the following:

```javascript
/*

Verification vector

[
    "7a4a81ba14f4b32b3c7f061f718cc15b0ca3ab76196a7e9e62f70dcc0add10092906a237d27b034f541731df41fee38d2f4b5d8998678b7d06cad2532fbecc13"
    "6edfa6a70925257e952f161d941092bf30cd70d1f27a6cba53affbe9de7a450dfc02b5c500fbeb9ca2443df0a941269ad65c8bede560f033ae802d6499281a9e"
    "492a5db1fdf4f133e4d8e92c0637ae13a8da6f19bc93da4ed6b0736d8bc602063a229846a43e18b8ba39de1cdba274a11fcda3cf482e3ca908cdf3c3f7ffdb04"
    "8396e4d78a12580388c919b8a201036d48a1470c9684eb653ea89f458927730bc32e8f91763dfce581ff60125664cd6e1a20ed684677a6d8dccbb6b23c3a0182"
]

Array of shares

[

    89b3400f482b4bed1e1eba002df97c8a7933c435fdd990d043fd527255010406        send to friend 1
    36ae6a22edc96b63ae6bf5ca78445f62117d2aeb0606876b5b0cd321c5338b0a        send to friend 2
    2170ec403427f94086f4e4712aef6e0b719f58160d8acdd27e26eaaa95676e1a        send to friend 3
    88e7e3aea7506953618a4f6c3b09e52fa7e1f881f16c70b66ef67c2ba80c5c0e        your share
    3569f445bbc8274318a0a3e761c016610bcb8b2cb414b49a76979181f9a3480f        send to friend 5
    b7a2bc301f42f0aa7acff13015b5c28ddd13d8b3fee5f3f81978463bba5efb16        send to friend 6

]

ID=e52d9c508c502347344d8c07ad91cbd6068afc75ff6292f062a09ca381c89e11

++++++++++++++++++++++++++++++++++++++++++++++++ Received share and verification vecotr from friend 1 ++++++++++++++++++++++++++++++++++++++++++++++++

SHARE_FROM_FRIEND_1 = 0f0389cb5fdc97c14d3c494ada022bafb78988efb891258b9c487b9475febd0a

VERIFICATION_VECTOR_OF_FRIEND_1 = [
    
    "a7e972de2362e725831efdebe080842f9ee275915a718dea43e561fa0d1af41b3192f849e1128d69f62018b9e7a9387f8b8611488a72480305991d480dad0e11"
    "0003e06c68ba56e45db0ee64a06a6935554e0b7f4849da26223cf831cdfd341bf9fe628590eb436197145206eb8f2eb0baef673eff23f63eaa6fe74e59da7c05"
    "53ad69e15e782a60fe63ee07d9e768dff002dfc54fa6fd6e4ea4531fc00d2700472fb244c9fa740ec354e417e8bd241c875caf29c4abad251a042d7becca4d15"
    "21b090515bf15d9ec6c4f4dd4df5f49a64f21e9b9c268b91b09bd77adcf2d517303fe3cf3bdd710ab783ae413476f7ccd050c1be02524a42484243b47767b71f"
    
]

*/
```

Ok, let's verify

```javascript
let SHARE_FROM_FRIEND_1 = "0f0389cb5fdc97c14d3c494ada022bafb78988efb891258b9c487b9475febd0a"

let VERIFICATION_VECTOR_OF_FRIEND_1 = [
    
    "a7e972de2362e725831efdebe080842f9ee275915a718dea43e561fa0d1af41b3192f849e1128d69f62018b9e7a9387f8b8611488a72480305991d480dad0e11",
    "0003e06c68ba56e45db0ee64a06a6935554e0b7f4849da26223cf831cdfd341bf9fe628590eb436197145206eb8f2eb0baef673eff23f63eaa6fe74e59da7c05",
    "53ad69e15e782a60fe63ee07d9e768dff002dfc54fa6fd6e4ea4531fc00d2700472fb244c9fa740ec354e417e8bd241c875caf29c4abad251a042d7becca4d15",
    "21b090515bf15d9ec6c4f4dd4df5f49a64f21e9b9c268b91b09bd77adcf2d517303fe3cf3bdd710ab783ae413476f7ccd050c1be02524a42484243b47767b71f"
    
]

let ID_OF_FRIEND_4 = "e52d9c508c502347344d8c07ad91cbd6068afc75ff6292f062a09ca381c89e11"

let isShareOK = tbls.verifyShareTBLS(ID_OF_FRIEND_4,SHARE_FROM_FRIEND_1,VERIFICATION_VECTOR_OF_FRIEND_1) // true

console.log(isShareOK) //true
```

Now imagine that we change a single byte of share(last `a` becomes `b`):

```javascript
let SHARE_FROM_FRIEND_1 = "0f0389cb5fdc97c14d3c494ada022bafb78988efb891258b9c487b9475febd0b"

let VERIFICATION_VECTOR_OF_FRIEND_1 = [
    
    "a7e972de2362e725831efdebe080842f9ee275915a718dea43e561fa0d1af41b3192f849e1128d69f62018b9e7a9387f8b8611488a72480305991d480dad0e11",
    "0003e06c68ba56e45db0ee64a06a6935554e0b7f4849da26223cf831cdfd341bf9fe628590eb436197145206eb8f2eb0baef673eff23f63eaa6fe74e59da7c05",
    "53ad69e15e782a60fe63ee07d9e768dff002dfc54fa6fd6e4ea4531fc00d2700472fb244c9fa740ec354e417e8bd241c875caf29c4abad251a042d7becca4d15",
    "21b090515bf15d9ec6c4f4dd4df5f49a64f21e9b9c268b91b09bd77adcf2d517303fe3cf3bdd710ab783ae413476f7ccd050c1be02524a42484243b47767b71f"
    
]

let ID_OF_FRIEND_4 = "e52d9c508c502347344d8c07ad91cbd6068afc75ff6292f062a09ca381c89e11"

let isShareOK = tbls.verifyShareTBLS(ID_OF_FRIEND_4,SHARE_FROM_FRIEND_1,VERIFICATION_VECTOR_OF_FRIEND_1) // true

console.log(isShareOK) //false

```

{% hint style="danger" %}
<mark style="color:red;">**A SIMILAR PROCEDURE IS CARRIED OUT BY EACH OF THE FRIENDS ON EACH SHARE!**</mark>
{% endhint %}

Only if there are no violations - friends can safely deposit currency into the wallet&#x20;

_<mark style="color:red;">**8aae5ae3b51a6f4bba62f64ab44b2135339831f662f8ef9e004bffb1458faa045f2c9a640acb466c5c35e2c9af757ac7fad74e3865b85274526 19236822f9797**</mark>_

And note, there will only be this public key in the blockchain - without any additional settings or conditions. The key size will be similar even if there are 20 or 100 signatory participants

### TBLS => Ed25519 transaction

### TBLS => BLS(multisig address) transaction

### TBLS => TBLS(thresholdsig address) transaction

### TBLS => PostQuantum(Dilithium/BLISS) transaction
