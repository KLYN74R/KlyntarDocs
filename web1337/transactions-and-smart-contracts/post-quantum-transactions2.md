---
description: NextGen cryptography already on KLY
---

# Post-quantum transactions2

<figure><img src="../../.gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
PQC - Post-Quantum Cryptography
{% endhint %}

## Useful links

{% embed url="https://mastering.klyntar.org/beginning/cryptography/post-quantum-cryptography" %}

## Intro

As we said earlier, we use Dilithium and BLISS signatures as post-quantum signature schemes. Depending on the Dilithium mode, the size of the keys may change, but the structure of the key pair remains unchanged. Also, since post-quantum schemes usually have large public keys, the address of the post-quantum key pair will be the [_<mark style="color:red;">**BLAKE3**</mark>_](https://mastering.klyntar.org/beginning/cryptography/hash-functions) hash of the public key. Here's what it looks like:

<figure><img src="../../.gitbook/assets/image (8).png" alt=""><figcaption></figcaption></figure>

## <mark style="color:red;">Disclaimer</mark>

{% hint style="danger" %}
Post-quantum cryptography is just coming into play. Unlike other cryptography primitives which works for decades for various purposes(AES, ED25519, ECDSA, RSA, SHA, etc.), these algorithms are still going through the final stages of NIST certification. Although we have chosen the best ones, there is still the possibility of a zerodays and errors in the implementation. Be careful
{% endhint %}
