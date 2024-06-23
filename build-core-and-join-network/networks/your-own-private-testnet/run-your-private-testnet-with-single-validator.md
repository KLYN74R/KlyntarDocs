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

```sh
cd CONFIGS
```

{% file src="../../../.gitbook/assets/kly_wvm.json" %}

{% file src="../../../.gitbook/assets/kly_evm.json" %}

{% file src="../../../.gitbook/assets/workflow.json" %}

## Set genesis

{% file src="../../../.gitbook/assets/single_node_testnet_genesis.json" %}
