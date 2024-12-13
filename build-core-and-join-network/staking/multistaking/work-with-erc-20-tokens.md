---
icon: circle-dollar-to-slot
---

# Work with ERC-20 tokens

## Intro

By placing your ERC-20 tokens on a multistaking contract, you receive similar liquidity tokens in a 1:1 ratio.

This way, you can earn rewards on Klyntar (helping to decentralize the network) and at the same time continue to use the tokens in DeFi applications, lending protocols, etc.

In this tutorial we will show how to restake the Chainlink ERC-20 token in Sepolia testnet, but the same way it works for any other ERC-20 tokens that will be supported by our network.

To get the list of supported ERC-20 tokens - check the docs:

{% content-ref url="full-list-of-supported-assets.md" %}
[full-list-of-supported-assets.md](full-list-of-supported-assets.md)
{% endcontent-ref %}

Or via the form on our site:

{% embed url="https://klyntar.org/multistaking" %}

## 1. Stake ERC-20 token to get liquidity tokens back and earn staking points

Visit the page

<figure><img src="../../../.gitbook/assets/image (77).png" alt=""><figcaption></figcaption></figure>

You will see the following interface

<figure><img src="../../../.gitbook/assets/image (78).png" alt=""><figcaption></figcaption></figure>

Now select the chain and wallet to connect

{% hint style="info" %}
In this tutorial we'll work with Sepolia network (Ethereum testnet)
{% endhint %}

<figure><img src="../../../.gitbook/assets/image (79).png" alt=""><figcaption></figcaption></figure>

Right after connection you will see how much you already restaked on appropriate contract in dollar equivalent

<figure><img src="../../../.gitbook/assets/image (80).png" alt=""><figcaption></figcaption></figure>

Choose the appropraite operation **Deposit** and token to stake - in this case ERC-20 token of Chainlink project:

{% embed url="https://sepolia.etherscan.io/token/0x779877a7b0d9e8603169ddbd7836e478b4624789" %}

Input amount of tokens and choose the pool to stake on

<figure><img src="../../../.gitbook/assets/image (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

### Transaction details

Two subactions occured during transaction:

1. You transfer 10 LINK tokens to multistaking contract
2. You receive 10 KLINK (Klyntar LINK) tokens back to avoid liquidity freeze

<figure><img src="../../../.gitbook/assets/image (2) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

After 3 epochs your address will appear in the list of stakers for the selected pool

<figure><img src="../../../.gitbook/assets/image (3) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

{% hint style="success" %}
From now you have successfully finish the staking process. You will now receive rewards from the pool you staked on.
{% endhint %}

## 2. Unstaking ERC-20 to burn our liquidity token and get back you original tokens

To unstake - choose the token, input amount and finally the pool

<figure><img src="../../../.gitbook/assets/image (5) (1).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
As before - the unstaking process takes 3 epochs and initially the stake will be placed in the withdrawal requests
{% endhint %}

## 3. Final step - withdrawal

It's time to get your original tokens back. Select **Withdraw** and press a button to initiate the transaction

<figure><img src="../../../.gitbook/assets/image (6) (1).png" alt=""><figcaption></figcaption></figure>

If you check via explorer, you will notice that within this transaction, the multistaking contract returned 10 original Chainlink tokens back to your address.

<figure><img src="../../../.gitbook/assets/image (7) (1).png" alt=""><figcaption></figcaption></figure>

## FAQ

<details>

<summary>How to unstake from multiple pools</summary>

TODO

</details>

