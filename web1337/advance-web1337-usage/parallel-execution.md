---
icon: grip-lines
description: Learn how to speed up your transaction in EVM / WASM
---

# Parallel execution

## Intro

Klyntar was created to implement best practices. That is why parallel transaction execution is one of the steps of multilevel sharding.

Currently, parallel transaction execution is supported for:

<table><thead><tr><th width="274">Transaction type</th><th width="269">Description</th><th data-type="checkbox">Implementation status</th></tr></thead><tbody><tr><td><strong>TX</strong></td><td>Default address to address coins transfer</td><td>true</td></tr><tr><td><strong>WVM_DEPLOY</strong></td><td>Contract deployment to WASM vm</td><td>true</td></tr><tr><td><strong>WVM_CALL</strong></td><td>Call smart-contract in WASM vm</td><td>true</td></tr><tr><td><strong>EVM_CALL</strong></td><td>Interaction with EVM(default transfer, contract interaction)</td><td>true</td></tr></tbody></table>

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

<table><thead><tr><th width="180">Transaction type</th><th>What to add to array</th></tr></thead><tbody><tr><td><strong>TX</strong></td><td>[creator, payload.to]</td></tr><tr><td><strong>WVM_DEPLOY</strong></td><td>[creator]</td></tr><tr><td><strong>WVM_CALL</strong></td><td>[creator, contractID, ....(all contracts addresses in case of internal calls)]</td></tr><tr><td><strong>EVM_CALL</strong></td><td>[from, to, ....(all contracts addresses in case of internal calls)]</td></tr></tbody></table>

This is example of parallel transaction:

<figure><img src="../../.gitbook/assets/image (36).png" alt=""><figcaption></figcaption></figure>

And this transaction is not parallel - it was executed in a sync way

<figure><img src="../../.gitbook/assets/image (37).png" alt=""><figcaption></figcaption></figure>

## Parallel execution for native transactions and WASM vm

## Parallel execution for EVM
