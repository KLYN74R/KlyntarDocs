# Block API

## Get block by ID

```javascript
let blockID = "1:9GQ46rqY238rk2neSwgidap9ww5zbAN4dyqyC7j5ZnBK:15"

await web1337.getBlockByBlockID(blockID)
```

## Get block by SID

```javascript
let shard = "9GQ46rqY238rk2neSwgidap9ww5zbAN4dyqyC7j5ZnBK"

let indexInShard = "1337"

await web1337.getBlockBySID(shard,indexInShard)
```

## Get latest N blocks on shard

```javascript
let shard = "9GQ46rqY238rk2neSwgidap9ww5zbAN4dyqyC7j5ZnBK"

let startIndex = 34

let limit = 20

await web1337.getLatestNBlocksOnShard(shard,startIndex,limit)
```
