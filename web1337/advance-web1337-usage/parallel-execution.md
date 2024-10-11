---
icon: grip-lines
description: Learn how to speed up your transaction in EVM / WASM
---

# Parallel execution

## Intro

Klyntar was created to implement best practices. That is why parallel transaction execution is one of the steps of multilevel sharding.

Currently, parallel transaction execution is supported for:

| Transaction type | Description                                                  | Implementation status                         |
| ---------------- | ------------------------------------------------------------ | --------------------------------------------- |
| **TX**           | Default address to address coins transfer                    | [✅](https://emojipedia.org/check-mark-button) |
| **WVM\_DEPLOY**  | Contract deployment to WASM vm                               | [✅](https://emojipedia.org/check-mark-button) |
| **WVM\_CALL**    | Call smart-contract in WASM vm                               | [✅](https://emojipedia.org/check-mark-button) |
| **EVM\_CALL**    | Interaction with EVM(default transfer, contract interaction) | [✅](https://emojipedia.org/check-mark-button) |

## How

After you have studied the process of sending transactions - you might have noticed that the transaction contains a payload.

So - in order to make your transaction run in parallel, you need to add the `touchedAccounts` field to the transaction payload. This is an array that contains the IDs of the accounts that will be changed within the transactions.

```javascript
const payload = {

    to: "recipient",

    amount: 13.37,

    touchedAccounts:["sender account", "recipient account"]

};
```

For example - here is what you need to add to the array for different types of transactions

| Transaction type | What to add to array           |
| ---------------- | ------------------------------ |
| **TX**           | \[creator, payload.to]         |
| **WVM\_DEPLOY**  | \[creator]                     |
| **WVM\_CALL**    | \[creator, payload.contractID] |
| **EVM\_CALL**    | \[from, to]                    |

This is example of parallel transaction:

<figure><img src="../../.gitbook/assets/image (36).png" alt=""><figcaption></figcaption></figure>

And this transaction is not parallel - it was executed in a sync way

<figure><img src="../../.gitbook/assets/image (37).png" alt=""><figcaption></figcaption></figure>

## Parallel execution for native transactions and WASM vm



## Parallel execution for EVM
