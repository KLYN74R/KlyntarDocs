---
icon: coin
---

# Supported networks and tokens

## Initial support - EVM networks

The EVM chains have the highest liquidity and TVL - there are Ethereum, Binance Smart Chain, Avalanche, etc.

That is why we will launch the multistaking logic here as a starting point. This list will be updated over time, so stay tuned.

For such chains, we plan to implement multistaking for:

1. Native coins of the network (ETH, BSC, AVAX, etc.)
2. ERC-20 tokens (stablecoins, wrapped tokens, LPs, LRTs, LSTs, etc.)
3. ERC-721 tokens
4. etc.

So, to find the full list of supported assets - go to:

{% content-ref url="full-list-of-supported-assets.md" %}
[full-list-of-supported-assets.md](full-list-of-supported-assets.md)
{% endcontent-ref %}

## PoW based networks - Bitcoin, Litecoin, Doge, Kaspa, etc.

There are also chains that do not have smart contracts, which complicates our multi-staking logic.

That is why at the initial stages we plan to use their wrapped versions in different chains. For example:

1. WBTC - wrapped BTC
2. LTC (BEP-20)
3. DOGE (BEP-20)
4. XRP (BEP-20)
5. etc.

## Move based chains - Aptos, SUI, etc.

Such chains indeed have smart contracts, but the Move language is new, which means that the practices for multistaking logic will be different from smart contracts on Solidity.

<mark style="color:red;">**So, to avoid mistakes and not take risks, we focus on smart contracts in EVM chains.**</mark>

That is why if you have assets in these chains, use bridges to receive tokens in any EVM-compatible chain that we support

## Cosmos based chains

The Cosmos ecosystem also has a large number of chains and interesting tokens that can be used for multi-staking. Therefore, use bridges to transfer your tokens to EVM chains

## Other chains - Solana, Near, Tron, TON, Cardano, etc.

We also want to bring multistaking to chains that also have a lot of weight in the crypto industry, have a large community, many interesting projects in ecosystems and much more.

Such chains include both the most popular market tops - for example, Solana, Cardano, Tron, Ton, Near, and less popular projects.

At the initial stage, we also recommend transferring assets from these chains to the EVM environment. However, in the future we may directly support these chains.

## Restaking/Liquid Restaking protocols - EigenLayer, Karak, Babylon, Allstake, etc.
