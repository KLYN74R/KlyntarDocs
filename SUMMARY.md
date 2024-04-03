# Table of contents

* [KLYNTAR Docs](README.md)
* [📚 Glossary](glossary.md)

## Web1337

* [Web1337 intro](web1337/web1337-intro.md)
* [🟢 Simple API requests](web1337/simple-api-requests.md)
* [🟠 Transactions and smart-contracts](web1337/transactions-and-smart-contracts/README.md)
  * [🔐 Default Ed25519 transactions](web1337/transactions-and-smart-contracts/default-ed25519-transactions.md)
  * [🤝 BLS multisig transactions](web1337/transactions-and-smart-contracts/bls-multisig-transactions.md)
  * [🛡️ TBLS thresholdsig transactions](web1337/transactions-and-smart-contracts/tbls-thresholdsig-transactions.md)
  * [⚛️ Post-quantum transactions](web1337/transactions-and-smart-contracts/post-quantum-transactions.md)
  * [📃 KLY-WVM - deploy and interact with a smart-contract](web1337/transactions-and-smart-contracts/kly-wvm-deploy-and-interact-with-a-smart-contract.md)
  * [📃 KLY-EVM - deploy and interact with a smart-contract](web1337/transactions-and-smart-contracts/kly-evm-deploy-and-interact-with-a-smart-contract.md)
* [🔴 Advance Web1337 usage](web1337/advance-web1337-usage/README.md)
  * [💫 Epoch edge operations](web1337/advance-web1337-usage/epoch-edge-operations/README.md)
    * [🕊️ Send epoch edge operation to KLY](web1337/advance-web1337-usage/epoch-edge-operations/send-epoch-edge-operation-to-kly.md)
    * [👨‍⚖️ Become KLY validator with Web1337](web1337/advance-web1337-usage/epoch-edge-operations/become-kly-validator-with-web1337.md)
  * [☄️ Dump EVM & WASM contract storage](web1337/advance-web1337-usage/dump-evm-and-wasm-contract-storage.md)
  * [🔃 Manual deployment of the storage for your contract](web1337/advance-web1337-usage/manual-deployment-of-the-storage-for-your-contract.md)
  * [🪄 Abstraction](web1337/advance-web1337-usage/abstraction/README.md)
    * [⛓️ Chain abstraction](web1337/advance-web1337-usage/abstraction/chain-abstraction.md)
    * [🦸‍♂️ Account abstraction](web1337/advance-web1337-usage/abstraction/account-abstraction/README.md)
      * [💸 Using AFS - advanced fee system](web1337/advance-web1337-usage/abstraction/account-abstraction/using-afs-advanced-fee-system.md)
    * [💾 Storage Abstraction](web1337/advance-web1337-usage/abstraction/storage-abstraction.md)
  * [🌩️ Thundercloud](web1337/advance-web1337-usage/thundercloud/README.md)
    * [🏷️ Using KLY Aliases in transactions](web1337/advance-web1337-usage/thundercloud/using-kly-aliases-in-transactions.md)
    * [🦾 Deploy KIP](web1337/advance-web1337-usage/thundercloud/deploy-kip.md)
* [🌐 Networking](web1337/networking/README.md)
  * [🙈 Using proxy](web1337/networking/using-proxy.md)
  * [⚡ Interact with node via websockets](web1337/networking/interact-with-node-via-websockets.md)

## Smart Contracts and vms

* [Intro](smart-contracts-and-vms/intro.md)
* [👩‍💻 KLY-EVM](smart-contracts-and-vms/kly-evm/README.md)
  * [🧙‍♂️ Magic address](smart-contracts-and-vms/kly-evm/magic-address.md)
  * [➗ Features of sharding at the EVM level you should know](smart-contracts-and-vms/kly-evm/features-of-sharding-at-the-evm-level-you-should-know.md)
  * [❎ Call WASM from EVM](smart-contracts-and-vms/kly-evm/call-wasm-from-evm.md)
  * [❎ Call JS from EVM](smart-contracts-and-vms/kly-evm/call-js-from-evm.md)
* [👨‍💻 KLY-WVM](smart-contracts-and-vms/kly-wvm/README.md)
  * [🔁 Simple cross-contract call (WVM-WVM)](smart-contracts-and-vms/kly-wvm/simple-cross-contract-call-wvm-wvm.md)
  * [❎ Call EVM from WASM](smart-contracts-and-vms/kly-wvm/call-evm-from-wasm.md)
  * [❎ Call JS from WASM](smart-contracts-and-vms/kly-wvm/call-js-from-wasm.md)
* [🧠 Advanced VMs usage](smart-contracts-and-vms/advanced-vms-usage/README.md)
  * [🔐 Cryptography](smart-contracts-and-vms/advanced-vms-usage/cryptography/README.md)
    * [🎲 VRF](smart-contracts-and-vms/advanced-vms-usage/cryptography/vrf.md)
    * [⚛️ Post-quantum cryptography](smart-contracts-and-vms/advanced-vms-usage/cryptography/post-quantum-cryptography.md)
    * [👀 zkSNARK](smart-contracts-and-vms/advanced-vms-usage/cryptography/zksnark.md)
    * [🤫 Secure Secret Sharing](smart-contracts-and-vms/advanced-vms-usage/cryptography/secure-secret-sharing.md)
  * [⛈️ Thundercloud](smart-contracts-and-vms/advanced-vms-usage/thundercloud/README.md)
    * [👀 KLY Oracles](smart-contracts-and-vms/advanced-vms-usage/thundercloud/kly-oracles/README.md)
      * [⏳ Get the real time](smart-contracts-and-vms/advanced-vms-usage/thundercloud/kly-oracles/get-the-real-time.md)
      * [🌏 Call any API](smart-contracts-and-vms/advanced-vms-usage/thundercloud/kly-oracles/call-any-api.md)
* [✨ UVM - Universal Virtual Machine](smart-contracts-and-vms/uvm-universal-virtual-machine.md)

## 🗺️ RWX contracts

* [ℹ️ Intro to real-world-execution smart contracts](rwx-contracts/intro-to-real-world-execution-smart-contracts.md)
* [🤝 Create RWX contract and deploy with Web1337](rwx-contracts/create-rwx-contract-and-deploy-with-web1337.md)
* [🕵️‍♂️ Become verifier](rwx-contracts/become-verifier.md)

## deep dive into KLY

* [☁️ Run KLY node](deep-dive-into-kly/run-kly-node/README.md)
  * [Run your node over TOR](deep-dive-into-kly/run-kly-node/run-your-node-over-tor.md)
  * [Plugins usage](deep-dive-into-kly/run-kly-node/plugins-usage.md)
* [🕵️ Run KLY validator](deep-dive-into-kly/run-kly-validator/README.md)
  * [KLY staking](deep-dive-into-kly/run-kly-validator/kly-staking.md)
  * [KLY unstaking](deep-dive-into-kly/run-kly-validator/kly-unstaking.md)
  * [Multistaking](deep-dive-into-kly/run-kly-validator/multistaking.md)
* [🤟 Testnets](deep-dive-into-kly/testnets/README.md)
  * [Your own private testnet](deep-dive-into-kly/testnets/your-own-private-testnet/README.md)
    * [Run your private testnet with single validator](deep-dive-into-kly/testnets/your-own-private-testnet/run-your-private-testnet-with-single-validator.md)
    * [Run your private testnet with 4 nodes](deep-dive-into-kly/testnets/your-own-private-testnet/run-your-private-testnet-with-4-nodes.md)
    * [Run your private testnet with 21 nodes](deep-dive-into-kly/testnets/your-own-private-testnet/run-your-private-testnet-with-21-nodes.md)
  * [Public testnets](deep-dive-into-kly/testnets/public-testnets.md)
* [⚙️ Customizations](deep-dive-into-kly/customizations/README.md)
  * [Create own mutation](deep-dive-into-kly/customizations/create-own-mutation.md)
  * [Create own plugin](deep-dive-into-kly/customizations/create-own-plugin.md)
* [😦 Return of lost funds](deep-dive-into-kly/return-of-lost-funds.md)

## Bots

* [🤖 Intro](bots/intro.md)

## Other resources

* [Mastering Klyntar](https://mastering.klyntar.org)
* [KLY Services](https://services.klyntar.org)
