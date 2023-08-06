---
description: Get to know about the gateway to WASM
---

# 🧙♂ Magic address

## Intro

EVM and WASM are the dominant technologies in the cryptocurrency market. That is why KLY supports both EVM and WASM.

But you must admit that the project would not be complete if it were not possible to combine these 2 technologies and use them interchangeably. That is, for example, it would be cool if you could use some interesting algorithm in the EVM, the implementation of which is only available in Rust or Go, or, for example, call EVM logic directly from WASM smart contracts and vice versa.

In addition, since both VMs have access to the native runtime environment (we have Node.js since the KLY core has JS implementation), this means that both the EVM and WASM will have limited and controlled access to the JS environment. This means that it will be possible to expand both VMs by injecting the logic and functions of the native launch environment into them. Here's how it looks on the diagram

<figure><img src="../../.gitbook/assets/EVM_WASM_Native.png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
This brings us to the idea of hybrid smart contracts.
{% endhint %}



## Hybrid smart-contracts, EVM opcodes and magic address

The EVM is a stack machine that runs low-level bytecode that has been sourced from Solidity, Vyper, or Yul.

Like many other languages, EVM bytecode includes a set of opcodes, each of which performs a certain action - puts something on the stack, reads something from the memory (heap), performs mathematical operations, and so on.

{% embed url="https://www.evm.codes/" %}

However, unlike classic assembler listings, there are no syscalls, access to the file system, random number generator or other things that can lead to non-determinism and disruption of the cryptocurrency node.

