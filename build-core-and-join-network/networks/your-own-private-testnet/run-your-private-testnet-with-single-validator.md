# Run your private testnet with single validator

## Intro

The fastest and easiest way to test KLY abilities is to run a local single-node network. This network contains:

* 1 node(which is validator)
* 1 shard(validator is also a shard creator)

This allows you to check transactions sending, contracts calls and major part of available KLY features. Definitely, to discover more you'll need to run multi-node network(for example, to check how sharding works, test cross-shard interactions and so on).

## Prepare genesis and configs

As you remember, to run or join any network you need genesis and configs.

Genesis is a common startpoint where state of accounts pre-set and your node take the data about the first epoch.

Configs are set of files that you can change in a flexible way to play with your node abilities, use plugins, change the node keypair, JSON-RPC routes and so on!

First of all - choose the location where genesis, configs and chain data will be stored. Imagine that it's directory `$HOME/klyntar`

```bash
mkdir $HOME/klyntar

cd $HOME/klyntar
```

Now, create the subdirectorites for genesis and configs

```bash
mkdir CONFIGS GENESIS
```

## Set configuration files

Go to CONFIGS directory and add the following files there

```sh
cd CONFIGS
```

{% file src="../../../.gitbook/assets/kly_wvm (1).json" %}

{% file src="../../../.gitbook/assets/kly_evm.json" %}

{% file src="../../../.gitbook/assets/workflow.json" %}

## Set genesis

Now, go to GENESIS directory and add the appropriate genesis file there

{% file src="../../../.gitbook/assets/genesis.json" %}

## Finally

Now, the hierarchy should looks like this

```
│
├───CONFIGS
│   ├───kly_wvm.json
│   ├───kly_evm.json
│   ├───workflow.json
└───GENESIS
    ├───genesis.json
```

Once you run your node, the structure will be

```
├───CHAINDATA
│   ├───0123456789abcdef0123456789abcdef0123456789abcdef0123456789abcdef#0
│   ├───APPROVEMENT_THREAD_METADATA
│   ├───BLOCKS
│   ├───EPOCH_DATA
│   ├───KLY_EVM
│   └───STATE
│
├───CONFIGS
│   ├───kly_wvm.json
│   ├───kly_evm.json
│   ├───workflow.json
└───GENESIS
    ├───genesis.json
```

These three directories contain everything necessary for the correct operation of the node

## Prepare the env variables

In the environment variables you should specify the core launch mode (`testnet/mainnet`) and the path to a directory with 3 subdirectories (mentioned earlier). You can set these variables in your shell configuration files (e.g., `.bashrc` or `.zshrc`), depending on your OS.

```bash
export KLY_MODE=testnet

# Choose your own path
export SYMBIOTE_DIR=$HOME/klyntar
```

## Set appropriate first epoch time

In order to run your local testnet correctly, you need to make some changes to the genesis file. Namely, you need to set the start time of the current epoch.

To get the current time, use the following JS snippet

```javascript
console.log(new Date().getTime()) // E.g: 1719237980952
```

Now, modify the `genesis.json`:

```json
"EPOCH_TIMESTAMP": 1719237980952
```

Now, you can run your node. From your console run:

```
klyntar
```

And you have to see the following:

<figure><img src="../../../.gitbook/assets/image (9) (1).png" alt=""><figcaption></figcaption></figure>

{% hint style="success" %}
The password to decrypt your private key(stored in workflow.json, encrypted by AES-256) is `qwerty`
{% endhint %}

{% hint style="danger" %}
**DO NOT USE THIS PASSWORD FOR PRODUCTION TO PREVENT BRUTEFORCING / DICTIONARY ATTACKS**
{% endhint %}

## Keep it work

Once you run node, leave this console and start to work with KLY. Now you can call APIs, create/send transactions, interact with smart-contracts and so on

## IMPORTANT

{% hint style="danger" %}
**Due to the nature of consensus, the network must generate at least 1 block per epoch in order to correctly change epochs and continue fault-tolerant work**
{% endhint %}
