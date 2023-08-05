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
import {crypto} from 'web1337'



let threshold = 4 // 4/6

let myPubId = 1 // our ID

let pubKeysArr = [1,2,3,4,5,6] // array of identifiers of all members


let {verificationVector,secretShares,id} = crypto.tbls.generateTBLS(threshold,myPubId,pubKeysArr)


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

let isShareOK = crypto.tbls.verifyShareTBLS(ID_OF_FRIEND_4,SHARE_FROM_FRIEND_1,VERIFICATION_VECTOR_OF_FRIEND_1) // true

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

let isShareOK = crypto.tbls.verifyShareTBLS(ID_OF_FRIEND_4,SHARE_FROM_FRIEND_1,VERIFICATION_VECTOR_OF_FRIEND_1) // true

console.log(isShareOK) //false

```

{% hint style="danger" %}
<mark style="color:red;">**A SIMILAR PROCEDURE IS CARRIED OUT BY EACH OF THE FRIENDS ON EACH SHARE!**</mark>
{% endhint %}

Only if there are no violations - friends can safely deposit currency into the wallet&#x20;

_<mark style="color:red;">**8aae5ae3b51a6f4bba62f64ab44b2135339831f662f8ef9e004bffb1458faa045f2c9a640acb466c5c35e2c9af757ac7fad74e3865b85274526 19236822f9797**</mark>_

And note, there will only be this public key in the blockchain - without any additional settings or conditions. The key size will be similar even if there are 20 or 100 signatory participants

### Step 5 - generate partial signatures

Ok, let's move on to the moment when the friends finally decided to make a purchase. The worst possible scenario is when 4 out of 6 friends agree to the purchase and 2 of them do not. Nevertheless, 4 is a sufficient threshold and each of the 4 friends must generate a partial signature, in order to then combine them and send them to the blockchain as cryptographic proof that the threshold has been reached and the network can allow spending on this wallet. To do this, each of the 4 friends must generate a partial signature and share it with the other 3 friends.

It is worth noting that the threshold _<mark style="color:red;">**T**</mark>_ indicates the minimum value of the signatories. In our example, we consider just such a case. However, funds can be spent even when 5 or all 6 agree with the decision

So, to generate a partial signature, we will use the `signTBLS()` function. Let's consider its parameters:

* `hexMyId` - it's our ID from step 1
* `sharedPayload` - it's array like this:

```
sharedPayload:[
            
    {
        verificationVector://Verification vector from signer_1 in hex format
        secretKeyShare://Share that we get from signer_1 in hex format
            
    },
            
    {
        verificationVector://Verification vector from signer_2 in hex format
        secretKeyShare://Share that we get from signer_2 in hex format
            
    },
            
    ...,
    {
                
        verificationVector://Verification vector from signer_N in hex format
        secretKeyShare://Share that we get from signer_N in hex format

    }
```

* `message` - it's message that we need to sign(transaction hash)

Let's generate partial signatures for 4 friends

```javascript
//------------------------------- FRIEND 1 -------------------------------


let vv1=[
    
    "a7e972de2362e725831efdebe080842f9ee275915a718dea43e561fa0d1af41b3192f849e1128d69f62018b9e7a9387f8b8611488a72480305991d480dad0e11",
    "0003e06c68ba56e45db0ee64a06a6935554e0b7f4849da26223cf831cdfd341bf9fe628590eb436197145206eb8f2eb0baef673eff23f63eaa6fe74e59da7c05",
    "53ad69e15e782a60fe63ee07d9e768dff002dfc54fa6fd6e4ea4531fc00d2700472fb244c9fa740ec354e417e8bd241c875caf29c4abad251a042d7becca4d15",
    "21b090515bf15d9ec6c4f4dd4df5f49a64f21e9b9c268b91b09bd77adcf2d517303fe3cf3bdd710ab783ae413476f7ccd050c1be02524a42484243b47767b71f",

]

let sharesOf1=[

    "79b359142147dd33383e32ac2476577d7760245a644fdad8730b22fad5726b0d",
    "2ba86e244fe8ad6ea665598257e1a8700b433a2e5bd967116fcf982b9db0df05",
    "af5f947ef9db70f50a8a5a5ee07170e78974c641b52942681a229605ff860e1f",
    "0f0389cb5fdc97c14d3c494ada022bafb78988efb891258b9c487b9475febd0a",
    "c8472e66991a8a1121a5f89f3ae5c1200455f77da838b965f8b9ef5af0701e07",
    "565a252bc870bdcf046fb76a25c45c56cab9d4a5624e794319dd13dbc7e9781f"

]

let ID_1 = '4bf5122f344554c53bde2ebb8cd2b7e3d1600ad631c385a5d7cce23c7785451a'


//------------------------------- FRIEND 2 -------------------------------

let vv2=[
    
    "6f4c90be986832a71aa5b8d25007d27adc3aa267acdca189a42b9a17efc8dd17bcde25c173d18eeba5bae067e52cc5ba16fd3099af04804c8a40b79ac595c617",
    "775449cde76ce6333c53317e6b6a1eb47ee87af2bc56548bb38fea5da266dd0704ae043c288f9fb792b870128738ad7360454724325b5260bb45e332a3d51221",
    "dd5a9d1f3236dd7e091dcb4272fb3cec624c22a0e59de283ce345603850e180e263806875158263b2bd46f0b7af4d78596e43008948431b07d174b6c8d6da388",
    "8176bdd4610dd21cdb6bfb9b526a9e3ee2804c57ac9b9094235a4c1947c2f20aa1282cc8dd5e2ec09183c677920106024aab0bd9a2ecaa5539ff9e1c4025d1a3"
]

let sharesOf2=[

    '173fc58cb61188a8fad233a689baec3db34657c3bb8536b21cbad70db463130b',
    '834ffe5b7794a5c8666210a98beaba4a48d344dd2e691ce4edb2196f6b2da317',
    'a8e50fdc4d6f1f20047b7297ff54ac2d807cbfb6cd12e8f499e6eaf7b5feb010',
    '50b2c925a3463136414715d0f99c594c1aa2d7e5857d7881f62dcdc7676c191a',
    'd7f329f64448ddd56350a2a3a45d944679a2c4fc76ecb3770d760592b08c8b0a',
    '2ddedf940ea38c9809735793cec9ded6d0d6e573181611c0f3efa604b6b92e1c'

]


let ID_2 = 'dbc1b4c900ffe48d575b5da5c638040125f65db0fe3e24494b76ea986457d906'


//------------------------------- FRIEND 3 -------------------------------

let vv3=[

    "586797df712ba4dcb15e78a36e6ef4a3f9ba1b05ce3a6a9001f0cdfefbd8f8080a33a3182a9204657594ebc6749656166f119ab2a81515266f62ce10e327ca09",
    "62b9b08eadbbb20ef840923c89ac53b73aa31fa3e493097f49496693a795131b46dba854f76bfaf369beb77a7a80e2fe66525b16341a88022e1dc635bd7dc385",
    "2577181e0384bc99ceb066acf69323e236160796df37f6e688d3274e4e026a03997460021b39da01a8a1465566e08f6a455333742fb6646f11beeab2a8dd2420",
    "b2a2ace162f22b200c7aa807083bae80e69214dcdaa9708c66f81d1e8d4a0c18c3b459214292bd683c9b80809ea3e7e95288656caf0b34ec260b820a79b08520"
]

let sharesOf3=[

    '82858da32c368bc8dea20ab9646d87f84eb478e51a45a55394710c96c71a080f',
    '6a12a6abfe939015aca70e67c96e9768ad9af0e6541c62e82b51ca32176c8816',
    'b607ae0d7be4bbbad607967c40841e67f8b992e15a44fc3d0dbc5a14a16f9b0e',
    '73d57dfb5cd773a19a34052184d1f0b2576bc127ee1d92ae596c51bf3a6e2c24',
    'd9dc10623e261047a0ac924595c356f5d9b192ab28f29276bf5c21206c143902',
    '952e03949438a001aafb2b1e864635c25b229047fc85b3178dd1d049ecbdcd04'

]


let ID_3 = '084fed08b978af4d7d196a7446a86b58009e636b611db16211b65a9aadff2905'

//------------------------------- FRIEND 4 -------------------------------

let vv4=[

    "7a4a81ba14f4b32b3c7f061f718cc15b0ca3ab76196a7e9e62f70dcc0add10092906a237d27b034f541731df41fee38d2f4b5d8998678b7d06cad2532fbecc13",
    "6edfa6a70925257e952f161d941092bf30cd70d1f27a6cba53affbe9de7a450dfc02b5c500fbeb9ca2443df0a941269ad65c8bede560f033ae802d6499281a9e",
    "492a5db1fdf4f133e4d8e92c0637ae13a8da6f19bc93da4ed6b0736d8bc602063a229846a43e18b8ba39de1cdba274a11fcda3cf482e3ca908cdf3c3f7ffdb04",
    "8396e4d78a12580388c919b8a201036d48a1470c9684eb653ea89f458927730bc32e8f91763dfce581ff60125664cd6e1a20ed684677a6d8dccbb6b23c3a0182"
]

let sharesOf4=[

    '89b3400f482b4bed1e1eba002df97c8a7933c435fdd990d043fd527255010406',
    '36ae6a22edc96b63ae6bf5ca78445f62117d2aeb0606876b5b0cd321c5338b0a',
    '2170ec403427f94086f4e4712aef6e0b719f58160d8acdd27e26eaaa95676e1a',
    '88e7e3aea7506953618a4f6c3b09e52fa7e1f881f16c70b66ef67c2ba80c5c0e',
    '3569f445bbc8274318a0a3e761c016610bcb8b2cb414b49a76979181f9a3480f',
    'b7a2bc301f42f0aa7acff13015b5c28ddd13d8b3fee5f3f81978463bba5efb16'

]

let ID_4 = 'e52d9c508c502347344d8c07ad91cbd6068afc75ff6292f062a09ca381c89e11'


//------------------------------- FRIEND 5 -------------------------------


let vv5=[

    "16ac0490dc79eef786f88dc09c3a8fed15d371e745bdadd9c1f7a08b4d37d50aea565d857cad2b0706642f58caa09fc1073848491994f690b79e8076beca1018",
    "cad2ef53a9dfe02a92606a34f40868fd746209a4ede0c790e0d3ec5e4c90021019b65c7550174fc117aed8d0aa619b94ddd1c08b7a573b9810490e3bbc36ba01",
    "d88949fae2178d3e601abd22f47f64dfd9f8a532d6abd57b3957f0197480cc1b6effc65b04b475279c8de4c585ad8ba518cf35df406fdca8e5f51d5a6918360b",
    "019c9a96b62afb4210616244cdc42936b503814762132658a20c283b9dfe3d005c1c7415903ed953c71539961e911602c8a272d41d5d08184a5f269ee08a941e"
]


let sharesOf5=[

    'f441e4786e7501271994d3819ff2997456c281c8de7225f856165fdf61d1270c',
    '3846038253b8099f82f8fa23632a2f41d6dbf1b41a579e59115f3a3d44a9d218',
    'fbffb23b9b08052c57b157908f8eddc61ddec2810d984e79114fac3db8f78017',
    'ba0d2e862b32e36e92a879519bc75a478305a26d292e69c00c5caaa00b4a6f1d',
    'f1271c0a82ef0173a8584b61b9991428ed5aa4e1052318827cd13bce3e076b1b',
    '455c84ebeb83ca3ef8c71d09bc310083848ca694244997dbbff4a74af3745a09'

]

let ID_5='e77b9a9ae9e30b0dbdb6f510a264ef9de781501d7b6b92ae89eb059c5ab7431b'



//------------------------------- FRIEND 6 -------------------------------


let vv6=[

    "76e014c5dc7a2ae9d0271936ecac410bee3baf9b88b64ad6a166a1c2aab0991992fec4dc4a80447cb795fa07cd77cdce1c5680710ba016a041c6db4a782f368d",
    "117b180362500cdbd655d3bb0f97b22b57521302938ce5a34e4bcf3c2988ad10688116b0bedd6215aae314bfb240158834b87bd828c01454916d5066d1241691",
    "2c811310b69511e286bd898a6d0f5a306594357e4faee25e69b76cc21b74e5118d96a26e59263dbfe7819be40d7f404d57dca16c9c17ba3c0f95cd531c133593",
    "93d15991f4f0269886cc3e1b99bf2b79bba44ecdc00891bc4b8e6195e1b14e1557ddf84001ef06d5cc0a3f19502f320bfc6f7e11ed75eb21731f68629897461b"
]

let sharesOf6=[

    'bbf907b9028b87ba335e274f0cbcad8286ea57361788859f6392aef9ea5eb124',
    'f7e2749b4803c55d42d4487916569510e50350d0bb5ec1879d8d00135cc7ae00',
    '1640c3244249f9b215a23015c5ac77bf27a1e9c39b46ca15908646742066be08',
    'aee5fb00cfaedb98c1a6846c3a25c183b57ae1938b0de18fd37503b93b982300',
    '2c84ae3f4fd9d4b8eb3c60e26b248086ff66892cda1a4fa5834cedfebee5cc0c',
    '00f055619d846105cb5de592f1b4f9b209fd8db6b29bf22fd6859dac91102322'

]

let ID_6='67586e98fad27da0b9968bc039a1ef34c939b9b8e523a8bef89d478608c5ec16'




//------------------------------------------------ Process to generate partial signatures ------------------------------------------------


// 4 friends is enough for 4/6

let sharedPayloadFor1 = [

    {
        verificationVector:vv1
        secretKeyShare:sharesOf1[0]
    },
    {
        verificationVector:vv2
        secretKeyShare:sharesOf2[0]
    },
    {
        verificationVector:vv3
        secretKeyShare:sharesOf3[0]
    },
    {
        verificationVector:vv4
        secretKeyShare:sharesOf4[0]
    },
    {
        verificationVector:vv5
        secretKeyShare:sharesOf5[0]
    },
    {
        verificationVector:vv6
        secretKeyShare:sharesOf6[0]
    }

]


let sharedPayloadFor2 = [

    {
        verificationVector:vv1
        secretKeyShare:sharesOf1[1]
    },
    {
        verificationVector:vv2
        secretKeyShare:sharesOf2[1]
    },
    {
        verificationVector:vv3
        secretKeyShare:sharesOf3[1]
    },
    {
        verificationVector:vv4
        secretKeyShare:sharesOf4[1]
    },
    {
        verificationVector:vv5
        secretKeyShare:sharesOf5[1]
    },
    {
        verificationVector:vv6
        secretKeyShare:sharesOf6[1]
    }

]


let sharedPayloadFor3 = [

    {
        verificationVector:vv1
        secretKeyShare:sharesOf1[2]
    },
    {
        verificationVector:vv2
        secretKeyShare:sharesOf2[2]
    },
    {
        verificationVector:vv3
        secretKeyShare:sharesOf3[2]
    },
    {
        verificationVector:vv4
        secretKeyShare:sharesOf4[2]
    },
    {
        verificationVector:vv5
        secretKeyShare:sharesOf5[2]
    },
    {
        verificationVector:vv6
        secretKeyShare:sharesOf6[2]
    }

]



let sharedPayloadFor4 = [

    {
        verificationVector:vv1
        secretKeyShare:sharesOf1[3]
    },
    {
        verificationVector:vv2
        secretKeyShare:sharesOf2[3]
    },
    {
        verificationVector:vv3
        secretKeyShare:sharesOf3[3]
    },
    {
        verificationVector:vv4
        secretKeyShare:sharesOf4[3]
    },
    {
        verificationVector:vv5
        secretKeyShare:sharesOf5[3]
    },
    {
        verificationVector:vv6
        secretKeyShare:sharesOf6[3]
    }

]


// Let the message will be a solution to buy a tent for "Burning Man"
let message = 'WE BUY A TENT FOR 300$'


//------------------------------------------ Here's partial signatures ------------------------------------------

let sigShare1 = crypto.tbls.signTBLS(ID_1,sharedPayloadFor1,message) //{"sigShare":"fe789c95b112099b370d0dfdb7aae4fb80087aa2cea0334266de5792ee27d617","id":"4bf5122f344554c53bde2ebb8cd2b7e3d1600ad631c385a5d7cce23c7785451a"}

let sigShare2 = crypto.tbls.signTBLS(ID_2,sharedPayloadFor2,message) //{"sigShare":"3cafeb0275cea62faf29df46c3d7bb52e1a6d33ab89aa0ad53d5bf1f53a0e285","id":"dbc1b4c900ffe48d575b5da5c638040125f65db0fe3e24494b76ea986457d906"}

let sigShare3 = crypto.tbls.signTBLS(ID_3,sharedPayloadFor3,message) //{"sigShare":"14a7a88ad49586e8c5a4e2c87b91b82f005fc595e60bd39b3a8d1cc3d20c9815","id":"084fed08b978af4d7d196a7446a86b58009e636b611db16211b65a9aadff2905"}

let sigShare4 = crypto.tbls.signTBLS(ID_4,sharedPayloadFor4,message) //{"sigShare":"6b46897a312f2e9e8f515f359f2adeb50c5c9fb5f6317c535a9a351c3e5f6e8a","id":"e52d9c508c502347344d8c07ad91cbd6068afc75ff6292f062a09ca381c89e11"}

```

### Step 6 - aggregation of partial signatures

Now, having 4 partial signatures, we can get a master signature. Later, with the message and master public key will be able to verify this signature and make sure it's valid and wished threshold was reached. It's time for the `buildSignature()` function. Traditionally, let's consider the parameters:

* `signaturesArray` - array like

```
[ {sigShare:signedShareA,id:hexIdA}, {sigShare:signedShareB,id:hexIdB},... {sigShare:signedShareX,id:hexIdX} ]

//  [+] sigShareN - partial signature by signer N in hex format
//  [+] id - ID of this signer
```

Execute the function

```javascript

let sigShareAndId1={sigShare:"fe789c95b112099b370d0dfdb7aae4fb80087aa2cea0334266de5792ee27d617",id:"4bf5122f344554c53bde2ebb8cd2b7e3d1600ad631c385a5d7cce23c7785451a"}

let sigShareAndId2={sigShare:"3cafeb0275cea62faf29df46c3d7bb52e1a6d33ab89aa0ad53d5bf1f53a0e285",id:"dbc1b4c900ffe48d575b5da5c638040125f65db0fe3e24494b76ea986457d906"}

let sigShareAndId3={sigShare:"14a7a88ad49586e8c5a4e2c87b91b82f005fc595e60bd39b3a8d1cc3d20c9815",id:"084fed08b978af4d7d196a7446a86b58009e636b611db16211b65a9aadff2905"}

let sigShareAndId4={sigShare:"6b46897a312f2e9e8f515f359f2adeb50c5c9fb5f6317c535a9a351c3e5f6e8a",id:"e52d9c508c502347344d8c07ad91cbd6068afc75ff6292f062a09ca381c89e11"}



let masterSignature = crypto.tbls.buildSignature([sigShareAndId1,sigShareAndId2,sigShareAndId3,sigShareAndId4]) //46c1f011e7d7039bb77a638e60e7e1c3acbfb3124ec2b06b918f1fd5d4a0b39c

```

Received master signature with the value&#x20;

`46c1f011e7d7039bb77a638e60e7e1c3acbfb3124ec2b06b918f1fd5d4a0b39c`

### Step 7 - verification of group signature using the group public key

So, at the previous stage, the signature of the group was obtained. Even earlier, we received the public key of the group - this is our address in the KLY network:&#x20;

_<mark style="color:red;">**8aae5ae3b51a6f4bba62f64ab44b2135339831f662f8ef9e004bffb1458faa045f2c9a640acb466c5c35e2c9af757ac7fad74e3865b85274526192 36822f9797**</mark>_

And only one what the network needs to do to accept a transaction from friends is simply to make such a check:

```javascript
let masterPub = '8aae5ae3b51a6f4bba62f64ab44b2135339831f662f8ef9e004bffb1458faa045f2c9a640acb466c5c35e2c9af757ac7fad74e3865b8527452619236822f9797'

let masterSig = '46c1f011e7d7039bb77a638e60e7e1c3acbfb3124ec2b06b918f1fd5d4a0b39c'

let message = 'WE BUY A TENT FOR 300$'

let isOk = crypto.tbls.verifyTBLS(masterPub,masterSig,message) //true
```

{% hint style="success" %}
We hope that such a step-by-step example where we describe generation and exchange of the necessary data will help developers in creating their products in the KLY ecosystem
{% endhint %}

Now, let's build the transactions

### TBLS => Ed25519 transaction

```javascript
import Web1337,{crypto} from 'web1337';


let web1337 = new Web1337({

    symbioteID:'aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa',
    workflowVersion:0,
    nodeURL: 'http://localhost:7332'

});


// 3 friends, threshold is 2/3

const tblsAccounts = {

    ALICE:{
    
        verificationVector:["79309fde56bf1e0420408b79b9adb8b675b85eddb78b675d66b1931106a3d113f34d23257c912ee1800fcc31290e525bfbaf5d18a51eabc7daefc3ff8d183d17","67c2572d5b5e9be849917ba16a2152d70dff21094915ccf03969acbc0046870ece2fe2d9afbdd6dc5b65e2e528ee702a3343de6caf3b25f0f9c571fcfd55d084"],
        shares:['f77ffec138a41033c3f278774685a53fa4f49687365d788c0efd671f29359703','f5b187d48cd56e9680985f5f0a6f7744bef11092a142bbbc0d5f0c4c92ed0a02','48f13b18936fabb681d7ed21bd0e747e9553b3777547795e9ad878734197631a'],
        id:'4bf5122f344554c53bde2ebb8cd2b7e3d1600ad631c385a5d7cce23c7785451a'
    },
    

    BOB:{

        verificationVector:["0af3b5585e92422fd02d0370043e193f258896df63b4fcf37f5323c8d0545b00e67f1b3b728751ef2dabc3378959376b83edec5d0b788c367edb195cc9bca402","29480aacb9fd7c9fc02f2f201cf2958b1bac764f1b1d9b4ad0c4f24c981a891db271723d076eb06b0a5c6bddf758b3fc9ade4542628f583796fa55b5855bf194"],
        shares:['1bfe78127fb2e4ee5eab8d0875aef9c49606535825fbb2619edb29ccd1a93508','ba706c61ab4be0b619b523021e79afddf21e124ecca08c1af4d057b3b6dd7213','a97319f37cdf2833a996334952eb73f8903260574d1ef549007a0d2de1ccb41c'],
        id:'dbc1b4c900ffe48d575b5da5c638040125f65db0fe3e24494b76ea986457d906'

    },
    
    CHARLIE:{

        verificationVector:["0e4b8473bfe882f056d3ce4de2550b2f3415ec83b04fc9580656e74e308fa207730a126afe151feef1b505b3a00bc5939d6b55a622d47fdf3e71baabe614c89a","d6d8a177714216b4e4398fad40e17b6503fc7c06fc4b669e5010c4d5b0fe0c14e179ac57824d509f50dc233a00a5752de816dfb2c1aebaaafd8700310c9c4f08"],

        shares:['4d941e9d6755581106df5189c54cffbc104709867f3c77078f3ac17c0880fc1e','cb00cad4e2c6d490d322ec0eb55ee7e381cd2e6a0e598eca12a1cf06a918791a','315dd8656357528f41e6140cf01b9f40cd01d1e2b4fafad40e706bc08aa45912'],

        id:'084fed08b978af4d7d196a7446a86b58009e636b611db16211b65a9aadff2905'

    },

    rootPub:'bedc88644f0deea4c0a77ba687712f494a1af7d8869f09768a0db42284f89d17b7b9225e0c87c2cb5511907dfd5eae3a53d789298721039e833770de29595880'

}


const rootPubKey = crypto.tbls.deriveGroupPubTBLS([tblsAccounts.ALICE.verificationVector,tblsAccounts.BOB.verificationVector,tblsAccounts.CHARLIE.verificationVector])


// ID of subchain where you want to transfer KLY or call contract
const subchain = '7GPupbq1vtKUgaqVeHiDbEJcxS7sSjwPnbht4eRaDBAEJv8ZKHNCSu2Am3CuWnHjta';

// Default Ed25519
const recipient = '6S4yLHorBUjSRjFpwxqPUXzwouZwR716CZ5uLmiy9Sze';

const nonce = 0;

const fee = 1;

const amountInKLY = 13.37;


//_____________ GENERATE SIG_SHARES _____________

const partialSignatureOfFriend1 = await web1337.buildPartialSignatureWithTxData(

    tblsAccounts.ALICE.id,
    
    [{secretKeyShare:tblsAccounts.ALICE.shares[0]},{secretKeyShare:tblsAccounts.BOB.shares[0]},{secretKeyShare:tblsAccounts.CHARLIE.shares[0]}],

    subchain, nonce, fee, recipient, amountInKLY

);

const partialSignatureOfFriend2 = await web1337.buildPartialSignatureWithTxData(

    tblsAccounts.BOB.id,
    
    [{secretKeyShare:tblsAccounts.ALICE.shares[1]},{secretKeyShare:tblsAccounts.BOB.shares[1]},{secretKeyShare:tblsAccounts.CHARLIE.shares[1]}],

    subchain, nonce, fee, recipient, amountInKLY

);

//_____________ BUILD TRANSACTION _______________


const finalTransaction = await web1337.createThresholdTransaction(
    
    rootPubKey,
    
    [partialSignatureOfFriend1,partialSignatureOfFriend2],

    nonce, recipient, amountInKLY, fee
    
);

console.log('============TBLS Transaction that can be deployed to network============\n')
console.log(finalTransaction)
```

Output:

```code-runner-output
============TBLS Transaction that can be deployed to network============

{
  v: 0,
  creator: 'bedc88644f0deea4c0a77ba687712f494a1af7d8869f09768a0db42284f89d17b7b9225e0c87c2cb5511907dfd5eae3a53d789298721039e833770de29595880',
  type: 'TX',
  nonce: 0,
  fee: 1,
  payload: {
    to: '6S4yLHorBUjSRjFpwxqPUXzwouZwR716CZ5uLmiy9Sze',
    amount: 13.37,
    type: 'T'
  },
  sig: 'bd602b821a12789cab6e191da09a813b6236fe3eb780a14c88d2a4d192b2221b'
}
```

### TBLS => BLS(multisig address) transaction

{% hint style="info" %}
The same for TBLs but remember about `rev_t` for new account
{% endhint %}

```javascript
import Web1337,{crypto} from 'web1337';


let web1337 = new Web1337({

    symbioteID:'aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa',
    workflowVersion:0,
    nodeURL: 'http://localhost:7332'

});


// 3 friends, threshold is 2/3

const tblsAccounts = {

    ALICE:{
    
        verificationVector:["79309fde56bf1e0420408b79b9adb8b675b85eddb78b675d66b1931106a3d113f34d23257c912ee1800fcc31290e525bfbaf5d18a51eabc7daefc3ff8d183d17","67c2572d5b5e9be849917ba16a2152d70dff21094915ccf03969acbc0046870ece2fe2d9afbdd6dc5b65e2e528ee702a3343de6caf3b25f0f9c571fcfd55d084"],
        shares:['f77ffec138a41033c3f278774685a53fa4f49687365d788c0efd671f29359703','f5b187d48cd56e9680985f5f0a6f7744bef11092a142bbbc0d5f0c4c92ed0a02','48f13b18936fabb681d7ed21bd0e747e9553b3777547795e9ad878734197631a'],
        id:'4bf5122f344554c53bde2ebb8cd2b7e3d1600ad631c385a5d7cce23c7785451a'
    },
    

    BOB:{

        verificationVector:["0af3b5585e92422fd02d0370043e193f258896df63b4fcf37f5323c8d0545b00e67f1b3b728751ef2dabc3378959376b83edec5d0b788c367edb195cc9bca402","29480aacb9fd7c9fc02f2f201cf2958b1bac764f1b1d9b4ad0c4f24c981a891db271723d076eb06b0a5c6bddf758b3fc9ade4542628f583796fa55b5855bf194"],
        shares:['1bfe78127fb2e4ee5eab8d0875aef9c49606535825fbb2619edb29ccd1a93508','ba706c61ab4be0b619b523021e79afddf21e124ecca08c1af4d057b3b6dd7213','a97319f37cdf2833a996334952eb73f8903260574d1ef549007a0d2de1ccb41c'],
        id:'dbc1b4c900ffe48d575b5da5c638040125f65db0fe3e24494b76ea986457d906'

    },
    
    CHARLIE:{

        verificationVector:["0e4b8473bfe882f056d3ce4de2550b2f3415ec83b04fc9580656e74e308fa207730a126afe151feef1b505b3a00bc5939d6b55a622d47fdf3e71baabe614c89a","d6d8a177714216b4e4398fad40e17b6503fc7c06fc4b669e5010c4d5b0fe0c14e179ac57824d509f50dc233a00a5752de816dfb2c1aebaaafd8700310c9c4f08"],

        shares:['4d941e9d6755581106df5189c54cffbc104709867f3c77078f3ac17c0880fc1e','cb00cad4e2c6d490d322ec0eb55ee7e381cd2e6a0e598eca12a1cf06a918791a','315dd8656357528f41e6140cf01b9f40cd01d1e2b4fafad40e706bc08aa45912'],

        id:'084fed08b978af4d7d196a7446a86b58009e636b611db16211b65a9aadff2905'

    },

    rootPub:'bedc88644f0deea4c0a77ba687712f494a1af7d8869f09768a0db42284f89d17b7b9225e0c87c2cb5511907dfd5eae3a53d789298721039e833770de29595880'

}


const rootPubKey = crypto.tbls.deriveGroupPubTBLS([tblsAccounts.ALICE.verificationVector,tblsAccounts.BOB.verificationVector,tblsAccounts.CHARLIE.verificationVector])


// ID of subchain where you want to transfer KLY or call contract
const subchain = '7GPupbq1vtKUgaqVeHiDbEJcxS7sSjwPnbht4eRaDBAEJv8ZKHNCSu2Am3CuWnHjta';

// BLS
const recipient = '7V4MxQyKRPemAUrKxrJxBoZ6eHP2n63tibaZv2N59zWM7BDaC13kqDEqoe8g5vYzQu';

const nonce = 0;

const fee = 1;

const amountInKLY = 13.37;

const rev_t = 2; // for example, imagine that 7V4MxQyKRPemAUrKxrJxBoZ6eHP2n63tibaZv2N59zWM7BDaC13kqDEqoe8g5vYzQu is under control of group of 6 members and the wished threshold is 4/6(so rev_t=6-4=2)


//_____________ GENERATE SIG_SHARES _____________

const partialSignatureOfFriend1 = await web1337.buildPartialSignatureWithTxData(

    tblsAccounts.ALICE.id,
    
    [{secretKeyShare:tblsAccounts.ALICE.shares[0]},{secretKeyShare:tblsAccounts.BOB.shares[0]},{secretKeyShare:tblsAccounts.CHARLIE.shares[0]}],

    subchain, nonce, fee, recipient, amountInKLY, rev_t

);

const partialSignatureOfFriend2 = await web1337.buildPartialSignatureWithTxData(

    tblsAccounts.BOB.id,
    
    [{secretKeyShare:tblsAccounts.ALICE.shares[1]},{secretKeyShare:tblsAccounts.BOB.shares[1]},{secretKeyShare:tblsAccounts.CHARLIE.shares[1]}],

    subchain, nonce, fee, recipient, amountInKLY, rev_t

);

//_____________ BUILD TRANSACTION _______________


const finalTransaction = await web1337.createThresholdTransaction(
    
    rootPubKey,
    
    [partialSignatureOfFriend1,partialSignatureOfFriend2],

    nonce, recipient, amountInKLY, fee, rev_t
    
);

console.log('============TBLS Transaction that can be deployed to network============\n')
console.log(finalTransaction)
```

Output:

```code-runner-output
============TBLS Transaction that can be deployed to network============

{
  v: 0,
  creator: 'bedc88644f0deea4c0a77ba687712f494a1af7d8869f09768a0db42284f89d17b7b9225e0c87c2cb5511907dfd5eae3a53d789298721039e833770de29595880',
  type: 'TX',
  nonce: 0,
  fee: 1,
  payload: {
    to: '7V4MxQyKRPemAUrKxrJxBoZ6eHP2n63tibaZv2N59zWM7BDaC13kqDEqoe8g5vYzQu',
    amount: 13.37,
    type: 'T',
    rev_t: 2
  },
  sig: '3a985ce737b26c9217ca9e516c89cd83c0bb8be827cecf95449fee1f6d366081'
}
```

### TBLS => TBLS(thresholdsig address) transaction

Here you just need to set the master pubkey of recipient. The structure as for => Ed25519 tx

```javascript
import Web1337,{crypto} from 'web1337';




let web1337 = new Web1337({

    symbioteID:'aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa',
    workflowVersion:0,
    nodeURL: 'http://localhost:7332'

});


// 3 friends, threshold is 2/3

const tblsAccounts = {

    ALICE:{
    
        verificationVector:["79309fde56bf1e0420408b79b9adb8b675b85eddb78b675d66b1931106a3d113f34d23257c912ee1800fcc31290e525bfbaf5d18a51eabc7daefc3ff8d183d17","67c2572d5b5e9be849917ba16a2152d70dff21094915ccf03969acbc0046870ece2fe2d9afbdd6dc5b65e2e528ee702a3343de6caf3b25f0f9c571fcfd55d084"],
        shares:['f77ffec138a41033c3f278774685a53fa4f49687365d788c0efd671f29359703','f5b187d48cd56e9680985f5f0a6f7744bef11092a142bbbc0d5f0c4c92ed0a02','48f13b18936fabb681d7ed21bd0e747e9553b3777547795e9ad878734197631a'],
        id:'4bf5122f344554c53bde2ebb8cd2b7e3d1600ad631c385a5d7cce23c7785451a'
    },
    

    BOB:{

        verificationVector:["0af3b5585e92422fd02d0370043e193f258896df63b4fcf37f5323c8d0545b00e67f1b3b728751ef2dabc3378959376b83edec5d0b788c367edb195cc9bca402","29480aacb9fd7c9fc02f2f201cf2958b1bac764f1b1d9b4ad0c4f24c981a891db271723d076eb06b0a5c6bddf758b3fc9ade4542628f583796fa55b5855bf194"],
        shares:['1bfe78127fb2e4ee5eab8d0875aef9c49606535825fbb2619edb29ccd1a93508','ba706c61ab4be0b619b523021e79afddf21e124ecca08c1af4d057b3b6dd7213','a97319f37cdf2833a996334952eb73f8903260574d1ef549007a0d2de1ccb41c'],
        id:'dbc1b4c900ffe48d575b5da5c638040125f65db0fe3e24494b76ea986457d906'

    },
    
    CHARLIE:{

        verificationVector:["0e4b8473bfe882f056d3ce4de2550b2f3415ec83b04fc9580656e74e308fa207730a126afe151feef1b505b3a00bc5939d6b55a622d47fdf3e71baabe614c89a","d6d8a177714216b4e4398fad40e17b6503fc7c06fc4b669e5010c4d5b0fe0c14e179ac57824d509f50dc233a00a5752de816dfb2c1aebaaafd8700310c9c4f08"],

        shares:['4d941e9d6755581106df5189c54cffbc104709867f3c77078f3ac17c0880fc1e','cb00cad4e2c6d490d322ec0eb55ee7e381cd2e6a0e598eca12a1cf06a918791a','315dd8656357528f41e6140cf01b9f40cd01d1e2b4fafad40e706bc08aa45912'],

        id:'084fed08b978af4d7d196a7446a86b58009e636b611db16211b65a9aadff2905'

    },

    rootPub:'bedc88644f0deea4c0a77ba687712f494a1af7d8869f09768a0db42284f89d17b7b9225e0c87c2cb5511907dfd5eae3a53d789298721039e833770de29595880'

}


const rootPubKey = crypto.tbls.deriveGroupPubTBLS([tblsAccounts.ALICE.verificationVector,tblsAccounts.BOB.verificationVector,tblsAccounts.CHARLIE.verificationVector])


// ID of subchain where you want to transfer KLY or call contract
const subchain = '7GPupbq1vtKUgaqVeHiDbEJcxS7sSjwPnbht4eRaDBAEJv8ZKHNCSu2Am3CuWnHjta';

// Rootkey of TBLS
const recipient = '8aae5ae3b51a6f4bba62f64ab44b2135339831f662f8ef9e004bffb1458faa045f2c9a640acb466c5c35e2c9af757ac7fad74e3865b85274526192 36822f9797';

const nonce = 0;

const fee = 1;

const amountInKLY = 13.37;


//_____________ GENERATE SIG_SHARES _____________

const partialSignatureOfFriend1 = await web1337.buildPartialSignatureWithTxData(

    tblsAccounts.ALICE.id,
    
    [{secretKeyShare:tblsAccounts.ALICE.shares[0]},{secretKeyShare:tblsAccounts.BOB.shares[0]},{secretKeyShare:tblsAccounts.CHARLIE.shares[0]}],

    subchain, nonce, fee, recipient, amountInKLY

);

const partialSignatureOfFriend2 = await web1337.buildPartialSignatureWithTxData(

    tblsAccounts.BOB.id,
    
    [{secretKeyShare:tblsAccounts.ALICE.shares[1]},{secretKeyShare:tblsAccounts.BOB.shares[1]},{secretKeyShare:tblsAccounts.CHARLIE.shares[1]}],

    subchain, nonce, fee, recipient, amountInKLY

);

//_____________ BUILD TRANSACTION _______________


const finalTransaction = await web1337.createThresholdTransaction(
    
    rootPubKey,
    
    [partialSignatureOfFriend1,partialSignatureOfFriend2],

    nonce, recipient, amountInKLY, fee
    
);

console.log('============TBLS Transaction that can be deployed to network============\n')
console.log(finalTransaction)
```

Output:

```code-runner-output
============TBLS Transaction that can be deployed to network============

{
  v: 0,
  creator: 'bedc88644f0deea4c0a77ba687712f494a1af7d8869f09768a0db42284f89d17b7b9225e0c87c2cb5511907dfd5eae3a53d789298721039e833770de29595880',
  type: 'TX',
  nonce: 0,
  fee: 1,
  payload: {
    to: '8aae5ae3b51a6f4bba62f64ab44b2135339831f662f8ef9e004bffb1458faa045f2c9a640acb466c5c35e2c9af757ac7fad74e3865b8527452619236822f9797',
    amount: 13.37,
    type: 'T'
  },
  sig: 'a8c80ca9bae1cf9431d692cfa6cdc2e89342711132ebc263530c48bcaed8b594'
}
```

### TBLS => PostQuantum(Dilithium/BLISS) transaction

Finally, PQC. As usual - default structure as for => Ed25519 or => TBLS

{% hint style="info" %}
The address of PQC account is BLAKE3 256-bit hash of public key&#x20;
{% endhint %}

```javascript

import Web1337,{crypto} from 'web1337';




let web1337 = new Web1337({

    symbioteID:'aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa',
    workflowVersion:0,
    nodeURL: 'http://localhost:7332'

});


// 3 friends, threshold is 2/3

const tblsAccounts = {

    ALICE:{
    
        verificationVector:["79309fde56bf1e0420408b79b9adb8b675b85eddb78b675d66b1931106a3d113f34d23257c912ee1800fcc31290e525bfbaf5d18a51eabc7daefc3ff8d183d17","67c2572d5b5e9be849917ba16a2152d70dff21094915ccf03969acbc0046870ece2fe2d9afbdd6dc5b65e2e528ee702a3343de6caf3b25f0f9c571fcfd55d084"],
        shares:['f77ffec138a41033c3f278774685a53fa4f49687365d788c0efd671f29359703','f5b187d48cd56e9680985f5f0a6f7744bef11092a142bbbc0d5f0c4c92ed0a02','48f13b18936fabb681d7ed21bd0e747e9553b3777547795e9ad878734197631a'],
        id:'4bf5122f344554c53bde2ebb8cd2b7e3d1600ad631c385a5d7cce23c7785451a'
    },
    

    BOB:{

        verificationVector:["0af3b5585e92422fd02d0370043e193f258896df63b4fcf37f5323c8d0545b00e67f1b3b728751ef2dabc3378959376b83edec5d0b788c367edb195cc9bca402","29480aacb9fd7c9fc02f2f201cf2958b1bac764f1b1d9b4ad0c4f24c981a891db271723d076eb06b0a5c6bddf758b3fc9ade4542628f583796fa55b5855bf194"],
        shares:['1bfe78127fb2e4ee5eab8d0875aef9c49606535825fbb2619edb29ccd1a93508','ba706c61ab4be0b619b523021e79afddf21e124ecca08c1af4d057b3b6dd7213','a97319f37cdf2833a996334952eb73f8903260574d1ef549007a0d2de1ccb41c'],
        id:'dbc1b4c900ffe48d575b5da5c638040125f65db0fe3e24494b76ea986457d906'

    },
    
    CHARLIE:{

        verificationVector:["0e4b8473bfe882f056d3ce4de2550b2f3415ec83b04fc9580656e74e308fa207730a126afe151feef1b505b3a00bc5939d6b55a622d47fdf3e71baabe614c89a","d6d8a177714216b4e4398fad40e17b6503fc7c06fc4b669e5010c4d5b0fe0c14e179ac57824d509f50dc233a00a5752de816dfb2c1aebaaafd8700310c9c4f08"],

        shares:['4d941e9d6755581106df5189c54cffbc104709867f3c77078f3ac17c0880fc1e','cb00cad4e2c6d490d322ec0eb55ee7e381cd2e6a0e598eca12a1cf06a918791a','315dd8656357528f41e6140cf01b9f40cd01d1e2b4fafad40e706bc08aa45912'],

        id:'084fed08b978af4d7d196a7446a86b58009e636b611db16211b65a9aadff2905'

    },

    rootPub:'bedc88644f0deea4c0a77ba687712f494a1af7d8869f09768a0db42284f89d17b7b9225e0c87c2cb5511907dfd5eae3a53d789298721039e833770de29595880'

}


const rootPubKey = crypto.tbls.deriveGroupPubTBLS([tblsAccounts.ALICE.verificationVector,tblsAccounts.BOB.verificationVector,tblsAccounts.CHARLIE.verificationVector])


// ID of subchain where you want to transfer KLY or call contract
const subchain = '7GPupbq1vtKUgaqVeHiDbEJcxS7sSjwPnbht4eRaDBAEJv8ZKHNCSu2Am3CuWnHjta';

// PQC account
const recipient = '8b9cf608fdc183625334896b054f6421690d5891ddc2e1cee13285f6723823c2';

const nonce = 0;

const fee = 1;

const amountInKLY = 13.37;


//_____________ GENERATE SIG_SHARES _____________

const partialSignatureOfFriend1 = await web1337.buildPartialSignatureWithTxData(

    tblsAccounts.ALICE.id,
    
    [{secretKeyShare:tblsAccounts.ALICE.shares[0]},{secretKeyShare:tblsAccounts.BOB.shares[0]},{secretKeyShare:tblsAccounts.CHARLIE.shares[0]}],

    subchain, nonce, fee, recipient, amountInKLY

);

const partialSignatureOfFriend2 = await web1337.buildPartialSignatureWithTxData(

    tblsAccounts.BOB.id,
    
    [{secretKeyShare:tblsAccounts.ALICE.shares[1]},{secretKeyShare:tblsAccounts.BOB.shares[1]},{secretKeyShare:tblsAccounts.CHARLIE.shares[1]}],

    subchain, nonce, fee, recipient, amountInKLY

);

//_____________ BUILD TRANSACTION _______________


const finalTransaction = await web1337.createThresholdTransaction(
    
    rootPubKey,
    
    [partialSignatureOfFriend1,partialSignatureOfFriend2],

    nonce, recipient, amountInKLY, fee
    
);

console.log('============TBLS Transaction that can be deployed to network============\n')
console.log(finalTransaction)
```

Output:

```code-runner-output
============TBLS Transaction that can be deployed to network============

{
  v: 0,
  creator: 'bedc88644f0deea4c0a77ba687712f494a1af7d8869f09768a0db42284f89d17b7b9225e0c87c2cb5511907dfd5eae3a53d789298721039e833770de29595880',
  type: 'TX',
  nonce: 0,
  fee: 1,
  payload: {
    to: '8b9cf608fdc183625334896b054f6421690d5891ddc2e1cee13285f6723823c2',
    amount: 13.37,
    type: 'T'
  },
  sig: 'fd538ca7876777b99b21fcc9e9f6d93f4b109a5d679c29b888528568309e9417'
}
```
