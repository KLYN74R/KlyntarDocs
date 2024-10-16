# Run your private testnet with 5 nodes and 3 shards

## Intro

This page contains example files for:

1. Genesis
2. Common configs for VMs
3. Specific to all 5 validators config file

## Structure

You need to create a directory for all validators:

```bash
mkdir Node1 Node2 Node3 Node4 Node5 
```

Now go to each directory and copy appropriate files to it. Files available bellow.

For example, for `Node_1` you should

```bash
cd Node1

mkdir CONFIGS GENESIS

# Copy genesis to GENESIS dir
cp /path/to/genesis/genesis.json GENESIS

# Copy common configs for WASM vm and EVM
cp /path/to/config/kly_wvm.json CONFIGS
cp /path/to/config/kly_evm.json CONFIGS

# Copy configs specific to node
cp /path/to/config/config_1.json CONFIGS
```

## Genesis

{% file src="../../../.gitbook/assets/genesis.json" %}

## Common configs for virtual machines

{% file src="../../../.gitbook/assets/kly_evm.json" %}

{% file src="../../../.gitbook/assets/kly_wvm.json" %}

## Configs for all 5 validators

{% file src="../../../.gitbook/assets/config_1.json" %}

{% file src="../../../.gitbook/assets/config_2.json" %}

{% file src="../../../.gitbook/assets/config_3.json" %}

{% file src="../../../.gitbook/assets/config_4.json" %}

{% file src="../../../.gitbook/assets/config_5.json" %}
