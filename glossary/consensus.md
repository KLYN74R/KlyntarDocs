---
icon: arrows-to-dot
---

# Consensus

## FP - Finalization Proof

Signature by some quorum member (validator) that told:

> I agree to vote for block <mark style="color:red;">**B**</mark> with index <mark style="color:green;">**I**</mark> and hash <mark style="color:purple;">**H**</mark> created by shard leader <mark style="color:orange;">**L**</mark>. The previous block hash was <mark style="color:blue;">**P**</mark>

On a more technical level - the validator that is part of the current quorum when receiving a block proposal from the shard leader generates a signature:

```javascript
import {crypto} from 'web1337';

let prevBlockHash = "b5d6a3736a146fe921e40be95b8e41a0f953a20aedb740769440be4b53795ff7";

let blockID = "0:9GQ46rqY238rk2neSwgidap9ww5zbAN4dyqyC7j5ZnBK:1618";

let blockHash = "36514c7acfd77950b23baced61f70bd7126f5dac4bc7f2eb110f364123901c42";

// Epoch Full ID = epoch.hash+"#"+epoch.index
// Just concat hash + # + index
let epochFullID = "0123456789abcdef0123456789abcdef0123456789abcdef0123456789abcdef#0";

let dataThatShouldBeSigned = prevBlockHash+blockID+blockHash+epochFullID;


// Imagine that validator holds it locally
let quorumValidatorPrivateKey = "MC4CAQAwBQYDK2VwBCIEILdhTMVYFz2GP8+uKUA+1FnZTEdN8eHFzbb8400cpEU9";


let finalizationProof = await crypto.ed25519.signEd25519(dataThatShouldBeSigned,quorumValidatorPrivateKey);
```

After quorum member generate this signature - it can be handled(by someone - since it proof is public) and aggregated to get the **AFP - aggregated finalization proof**

## AFP - Aggregated Finalization Proof

Aggregation of 2/3N of FPs gives us AFP. This is how it looks like:

```json
{
    "prevBlockHash": "b5d6a3736a146fe921e40be95b8e41a0f953a20aedb740769440be4b53795ff7",
    "blockID": "0:9GQ46rqY238rk2neSwgidap9ww5zbAN4dyqyC7j5ZnBK:1618",
    "blockHash": "36514c7acfd77950b23baced61f70bd7126f5dac4bc7f2eb110f364123901c42",
    "proofs": {
        "9GQ46rqY238rk2neSwgidap9ww5zbAN4dyqyC7j5ZnBK": "0pOSDm2Zr615gDociLowbGD3IU3eXZKqXOpGchHx8pQEBFThgKzRE2Qt+jTw4ikKJruVCMlDHjwaFOJuufJSAA==",
        "6XvZpuCDjdvSuot3eLr24C1wqzcf2w4QqeDh9BnDKsNE": "msSzIiJ9R7x+U+aLUYWyJVF2ylB8qQmel0U7XHPSRXlRZtc+AJVGctXiDZ89K54xpWL+E9TqDmj9H2Cqvz+MAg=="
    }
}
```

In our explorer you can check the AFP on the block page - look at the link next to the status

<figure><img src="../.gitbook/assets/image (11).png" alt=""><figcaption></figcaption></figure>

Clicking on the link will show you the raw version of AFP:

<figure><img src="../.gitbook/assets/image (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

Probably the most interesting field here is `proofs` - these are just the FP's from the validators that are part of the quorum of the epoch when the block was generated.

You can verify it yourself using this code:

```javascript
import {crypto} from 'web1337';

let prevBlockHash = "b5d6a3736a146fe921e40be95b8e41a0f953a20aedb740769440be4b53795ff7";

let blockID = "0:9GQ46rqY238rk2neSwgidap9ww5zbAN4dyqyC7j5ZnBK:1618";

let blockHash = "36514c7acfd77950b23baced61f70bd7126f5dac4bc7f2eb110f364123901c42";

// Epoch Full ID = epoch.hash+"#"+epoch.index
// Just concat hash + # + index
let epochFullID = "0123456789abcdef0123456789abcdef0123456789abcdef0123456789abcdef#0";

let dataThatShouldBeSigned = prevBlockHash+blockID+blockHash+epochFullID;

// Let verify FP for quorum member 9GQ46rqY238rk2neSwgidap9ww5zbAN4dyqyC7j5ZnBK

let quorumMember = "9GQ46rqY238rk2neSwgidap9ww5zbAN4dyqyC7j5ZnBK";
let fpProof = "0pOSDm2Zr615gDociLowbGD3IU3eXZKqXOpGchHx8pQEBFThgKzRE2Qt+jTw4ikKJruVCMlDHjwaFOJuufJSAA==";


let isFinalizationProofOk = await crypto.ed25519.verifyEd25519(dataThatShouldBeSigned,fpProof, quorumMember);

// In case 2/3N+1 of quorum created valid FPs - then this AFP is valid
```

## LRP - Leader Rotation Proof

Signature by some quorum member (validator) that told:

> I agree to finish FPs generation for shard leader <mark style="color:orange;">**L**</mark> on index <mark style="color:red;">**I**</mark> and hash <mark style="color:purple;">**H**</mark>. The hash of first block by this leader is <mark style="color:green;">**F**</mark>

## ALRP - Aggregated Leader Rotation Proof

Aggregation of 2/3N of LRPs gives us ALRP. This is how it looks like:

```json
{
    "firstBlockHash": "a2d375da65094d65d25cc449abd10dfcea6c1b011cd0ce190379ca09b82b842b",
    "skipIndex": 131,
    "skipHash": "fb963df17e1b7f437636e4c5c94b32d43b86049393bafdf01299a304511c59eb",
    "proofs": {
        "9GQ46rqY238rk2neSwgidap9ww5zbAN4dyqyC7j5ZnBK": "W4Otr1LGk5KVYYBp+10hi3n2l+iVyNc+BgLYh3Yi4JEGiBG+MlozqSk4RjWu638ccAkeaebk6IhgmNKu8R9zAw=="
    }
}
```

## EFP - Epoch Finalization Proof

Signature by some quorum member (validator) that told:

> I agree to finish epoch <mark style="color:red;">**X**</mark> on shard <mark style="color:purple;">**Y**</mark>. The last shard leader had index <mark style="color:green;">**Q**</mark> in leaders sequence. His last block has height **W** and hash <mark style="color:yellow;">**H**</mark>. His first block has hash <mark style="color:orange;">**F**</mark>

## AEFP - Aggregated Epoch Finalization Proof

Aggregation of 2/3N of EFPs gives us AEFP. This is how it looks like:

```json
{
    "shard": "shard_0",
    "lastLeader": 2,
    "lastIndex": 31,
    "lastHash": "ab963df17e1b7f437636e4c5c94b32d43b86049393bafdf01299a304511c59eb",
    "hashOfFirstBlockByLastLeader": "b2d375da65094d65d25cc449abd10dfcea6c1b011cd0ce190379ca09b82b842b",
    "proofs": {
        "9GQ46rqY238rk2neSwgidap9ww5zbAN4dyqyC7j5ZnBK": "W4Otr1LGk5KVYYBp+10hi3n2l+iVyNc+BgLYh3Yi4JEGiBG+MlozqSk4RjWu638ccAkeaebk6IhgmNKu8R9zAw=="
    }
}
```
