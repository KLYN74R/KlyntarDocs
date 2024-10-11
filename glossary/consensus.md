---
icon: arrows-to-dot
---

# Consensus

## FP - Finalization Proof

## AFP - Aggregated Finalization Proof

## LRP - Leader Rotation Proof

## ALRP - Aggregated Leader Rotation Proof

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
