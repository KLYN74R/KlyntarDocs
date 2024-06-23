# Table of contents

* [KLYNTAR Docs](README.md)
* [📚 Glossary](glossary.md)

## Web1337

* [Web1337 intro](web1337/web1337-intro.md)
* [🟢 Simple API requests](web1337/simple-api-requests/README.md)
  * [Block API](web1337/simple-api-requests/block-api.md)
  * [Epoch API](web1337/simple-api-requests/epoch-api.md)
  * [State API](web1337/simple-api-requests/state-api.md)
  * [Misc API](web1337/simple-api-requests/misc-api.md)
  * [Consensus related API](web1337/simple-api-requests/consensus-related-api.md)
  * [Transactions API](web1337/simple-api-requests/transactions-api.md)
* [🟠 Transactions and smart-contracts](web1337/transactions-and-smart-contracts/README.md)
  * [🔐 Default Ed25519 transactions](web1337/transactions-and-smart-contracts/default-ed25519-transactions.md)
  * [🤝 BLS multisig transactions](web1337/transactions-and-smart-contracts/bls-multisig-transactions.md)
  * [🛡️ TBLS thresholdsig transactions](web1337/transactions-and-smart-contracts/tbls-thresholdsig-transactions.md)
  * [⚛️ Post-quantum transactions](web1337/transactions-and-smart-contracts/post-quantum-transactions/README.md)
    * [Other PQC transactions options](web1337/transactions-and-smart-contracts/post-quantum-transactions/other-pqc-transactions-options.md)
  * [📃 KLY-WVM - deploy and interact with a smart-contract](web1337/transactions-and-smart-contracts/kly-wvm-deploy-and-interact-with-a-smart-contract.md)
  * [📃 KLY-EVM - deploy and interact with a smart-contract](web1337/transactions-and-smart-contracts/kly-evm-deploy-and-interact-with-a-smart-contract.md)
* [🔴 Advance Web1337 usage](web1337/advance-web1337-usage/README.md)
  * [💫 Epoch edge operations](web1337/advance-web1337-usage/epoch-edge-operations/README.md)
    * [🕊️ Send epoch edge operation to KLY](web1337/advance-web1337-usage/epoch-edge-operations/send-epoch-edge-operation-to-kly.md)
    * [👨‍⚖️ Become KLY validator with Web1337](web1337/advance-web1337-usage/epoch-edge-operations/become-kly-validator-with-web1337.md)
  * [🪄 Abstraction](web1337/advance-web1337-usage/abstraction/README.md)
    * [⛓️ Chain abstraction](web1337/advance-web1337-usage/abstraction/chain-abstraction.md)
    * [🦸‍♂️ Account abstraction 2.0](web1337/advance-web1337-usage/abstraction/account-abstraction-2.0/README.md)
      * [💸 Using AFS - advanced fee system](web1337/advance-web1337-usage/abstraction/account-abstraction-2.0/using-afs-advanced-fee-system.md)
    * [💾 Storage Abstraction](web1337/advance-web1337-usage/abstraction/storage-abstraction/README.md)
      * [🔃 Manual deployment of the storage for your contract](web1337/advance-web1337-usage/abstraction/storage-abstraction/manual-deployment-of-the-storage-for-your-contract.md)
      * [☄️ Dump EVM & WASM contract storage](web1337/advance-web1337-usage/abstraction/storage-abstraction/dump-evm-and-wasm-contract-storage.md)
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
    * [🤹 Using MPC](smart-contracts-and-vms/advanced-vms-usage/cryptography/using-mpc.md)
    * [🙈 Using FHE](smart-contracts-and-vms/advanced-vms-usage/cryptography/using-fhe.md)
  * [⛈️ Thundercloud](smart-contracts-and-vms/advanced-vms-usage/thundercloud/README.md)
    * [👀 KLY Oracles](smart-contracts-and-vms/advanced-vms-usage/thundercloud/kly-oracles/README.md)
      * [⏳ Get the real time](smart-contracts-and-vms/advanced-vms-usage/thundercloud/kly-oracles/get-the-real-time.md)
      * [🌏 Call any API](smart-contracts-and-vms/advanced-vms-usage/thundercloud/kly-oracles/call-any-api.md)
* [✨ XVM - Hybrid Virtual Machine](smart-contracts-and-vms/xvm-hybrid-virtual-machine.md)

## 🗺️ RWX contracts

* [ℹ️ Intro to real-world-execution smart contracts](rwx-contracts/intro-to-real-world-execution-smart-contracts.md)
* [🤝 Create RWX contract and deploy with Web1337](rwx-contracts/create-rwx-contract-and-deploy-with-web1337.md)
* [🕵️‍♂️ Become verifier](rwx-contracts/become-verifier.md)

## build core and join network

* [☁️ Run KLY node](build-core-and-join-network/run-kly-node.md)
* [🕵️ Run KLY validator](build-core-and-join-network/run-kly-validator/README.md)
  * [Default staking](build-core-and-join-network/run-kly-validator/default-staking/README.md)
    * [Staking](build-core-and-join-network/run-kly-validator/default-staking/staking.md)
    * [Unstaking](build-core-and-join-network/run-kly-validator/default-staking/unstaking.md)
  * [Multistaking](build-core-and-join-network/run-kly-validator/multistaking.md)
* [🕸️ Networks](build-core-and-join-network/networks/README.md)
  * [Your own private testnet](build-core-and-join-network/networks/your-own-private-testnet/README.md)
    * [Run your private testnet with single validator](build-core-and-join-network/networks/your-own-private-testnet/run-your-private-testnet-with-single-validator.md)
    * [Run your private testnet with 4 nodes](build-core-and-join-network/networks/your-own-private-testnet/run-your-private-testnet-with-4-nodes.md)
    * [Run your private testnet with 21 nodes](build-core-and-join-network/networks/your-own-private-testnet/run-your-private-testnet-with-21-nodes.md)
  * [Public testnets](build-core-and-join-network/networks/public-testnets.md)
* [⚙️ Customizations](build-core-and-join-network/customizations/README.md)
  * [Create own mutation](build-core-and-join-network/customizations/create-own-mutation.md)
  * [Create own plugin](build-core-and-join-network/customizations/create-own-plugin.md)
  * [Run your node over TOR](build-core-and-join-network/customizations/run-your-node-over-tor.md)
  * [Plugins usage](build-core-and-join-network/customizations/plugins-usage.md)
* [😦 Return of lost funds](build-core-and-join-network/return-of-lost-funds.md)

## Thundercloud

* [🏷️ KLY Aliases](thundercloud/kly-aliases/README.md)
  * [Web2 domain to KLY Alias](thundercloud/kly-aliases/web2-domain-to-kly-alias.md)
  * [Web3 domain to KLY Alias](thundercloud/kly-aliases/web3-domain-to-kly-alias.md)
  * [Social media handle to KLY Alias](thundercloud/kly-aliases/social-media-handle-to-kly-alias.md)

## Bots

* [🤖 Intro](bots/intro.md)

## Other resources

* [Mastering Klyntar](https://mastering.klyntar.org)
* [KLY Services](https://services.klyntar.org)
