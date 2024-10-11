---
icon: arrows-to-dot
---

# Consensus

## FP - Finalization Proof

Signature by some quorum member (validator) that told:

> I agree to vote for block <mark style="color:red;">**B**</mark> with index <mark style="color:green;">**I**</mark> and hash <mark style="color:purple;">**H**</mark> created by shard leader <mark style="color:orange;">**L**</mark>. The previous block hash was <mark style="color:blue;">**P**</mark>

## AFP - Aggregated Finalization Proof

Aggregation of 2/3N of FPs gives us AFP. This is how it looks like:

```json
{
    "prevBlockHash": "3ba7df6b3c7e49907f6b88482a5748dfc15450be72e9734c072371ca24f94339",
    "blockID": "3:9GQ46rqY238rk2neSwgidap9ww5zbAN4dyqyC7j5ZnBK:2839",
    "blockHash": "fec91901a5b2da9cf6e142c04304aae3aba2667c324c61e5820208386eacf8a5",
    "proofs": {
        "9GQ46rqY238rk2neSwgidap9ww5zbAN4dyqyC7j5ZnBK": "CnhbmXriLZqcw7zb6qabESNdlMLwrIPEt9pXH1okah2oSJCekTCV0AvA3NeOjmeOA2sYksOCV6z3y5fHxj1BAQ=="
    }
}
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
