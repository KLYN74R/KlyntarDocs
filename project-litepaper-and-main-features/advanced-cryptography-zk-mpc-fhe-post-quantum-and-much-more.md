# Advanced cryptography - zk, mpc, fhe, post-quantum and much more!

<figure><img src="../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

## Intro

Any **crypto**currency contains the word **`crypto`**, which means that cryptography plays an important role in its basis.

In our project, we have collected the most popular and powerful hashing algorithms, signatures and others to use onchain and offchain.

## Hashing

Mainly 2 algorithms are used:

1. SHA3 - inside the EVM. SHA3 has proven itself as a reliable and widely used algorithm
2. BLAKE3 - for our needs. Amazingly fast and one of the newest hashing algorithms that even outperforms SHA3 in some metrics. BLAKE3 was chosen as the main candidate to be used as the lead hash function for getting block headers' hashes, hashes of workflows, services archives and so on. Superfast, supports **PRF**, **MAC**, **KDF**, and **XOF** modes, highly parallelizable and so on.

<figure><img src="../.gitbook/assets/image (1) (1) (1).png" alt=""><figcaption><p>Read more <a href="https://github.com/BLAKE3-team/BLAKE3">https://github.com/BLAKE3-team/BLAKE3</a></p></figcaption></figure>

Since BLAKE3 supports **XOF** mode i.e. output length of a hash might be variable(like in SHAKE hashing scheme). This is important in case of using them as a quantum secure alternative to 128 or 256 bits schemes which can be abused by Grover or BHT algorithms.

## Default signatures - Ed25519

Ed25519 was introduced in OpenSSH version 6.5. This is an implementation of EdDSA using the [<mark style="color:purple;">Twisted Edwards curve</mark>](https://en.wikipedia.org/wiki/Twisted_Edwards_curve). It uses elliptic curve cryptography which provides better security and higher performance compared to DSA or ECDSA.

{% hint style="info" %}
For any type of interaction with KLYNTAR, you must have a pair of keys. You use the private key to sign the data, while the public key is used to verify the signature. In general, the principle is familiar to everyone.
{% endhint %}

Get details here:

{% content-ref url="../web1337/transactions-and-smart-contracts/default-ed25519-transactions.md" %}
[default-ed25519-transactions.md](../web1337/transactions-and-smart-contracts/default-ed25519-transactions.md)
{% endcontent-ref %}

## Multisig and threshold-sig algorithms

To let the group of people to control one wallet and/or perform transactions with smart contracts, multi-signature functionality is required.

{% hint style="info" %}
We use BLS and TBLS algorithms based on the BLS12-381 curve
{% endhint %}

#### BLS

Multi-signatures are versatile and can be useful in many places. For example, their advantage is that it is easy to achieve efficiency in generating _<mark style="color:red;">**N of N**</mark>_ signatures - for this you need _<mark style="color:red;">**N**</mark>_ members to sign the message _**M**_ with their private keys, and then aggregate public keys and signatures and get the master key of the group _<mark style="color:purple;">**PubN**</mark>_ and signature _<mark style="color:purple;">**SigN**</mark>_.

Thanks to the properties of aggregation, it will not be necessary to store N signatures and N public keys in the blockchain (as happens in naive multi-signature implementations), and instead of N signatures, it will be enough to check only 1 aggregated one, which gives us super efficiency.

<figure><img src="../.gitbook/assets/image (2) (1).png" alt=""><figcaption><p><a href="https://asecuritysite.com/signatures/js_bls">https://asecuritysite.com/signatures/js_bls</a></p></figcaption></figure>

With BLS you can initiate:

1. N of N transactions
2. T of N transactions (T\<N)
3. You can use it for default transactions and within smart-contracts for EVM & WASM virtual machines

Get the details about BLS usage here:

{% content-ref url="../web1337/transactions-and-smart-contracts/bls-multisig-transactions.md" %}
[bls-multisig-transactions.md](../web1337/transactions-and-smart-contracts/bls-multisig-transactions.md)
{% endcontent-ref %}

#### TBLS

The problem arises when not all signers may agree with the signature decision, but it is enough just to reach a certain threshold T (that is, at least _<mark style="color:red;">**T of N**</mark>_ participants agree to sign the transaction). In this case, when using multi-signature, we need to create some additional execution rules, and the size of the proof also grows.

Let me give you an example - let's say we have 10 people who decided to generate a signature using BLS multi-signature (for a transaction, staking, fixing an unobtanium or something else). Let 6 of them agree, and 4 others - against. At the same time, you set a threshold on your public aggregated key at 6/10 in advance.

It's no problem for 6 people to sign something and then aggregate their 6 public keys into a single public BLS. With a signature, it's the same - everything comes down to one thing.

<figure><img src="../.gitbook/assets/image (3) (1).png" alt=""><figcaption></figcaption></figure>

However, to prove to the network that the 6/10 threshold is met, you must provide 4 separate 48-byte addresses of those who disagree. This is because there is no other way for the network to find out how many of the keys are included in MasterPub1-6. This leads us to difficulties because there may be not 10, but 200 signers.

It would be nice if all the "magic" was performed outside the blockchain - the nodes will not know about any thresholds, they will not disclose the public keys of dissenters, and so on. All the blockchain needs to know is the fact that the required threshold has been reached and that we have a valid signature.

Get the details about TBLS here:

{% content-ref url="../web1337/transactions-and-smart-contracts/tbls-thresholdsig-transactions.md" %}
[tbls-thresholdsig-transactions.md](../web1337/transactions-and-smart-contracts/tbls-thresholdsig-transactions.md)
{% endcontent-ref %}

## Post-quantum cryptography

Generally speaking, the most popular and used algorithms can be counted on the fingers literally in every category:

* Hashes - SHA families, old MD, BLAKE hashes
* Signatures - RSA, ECDSA, EdDSA, Ed25519
* Symmetric encryption - AES, CAST, ChaCha20
* Key exchange - ECDH, DH, X25519

They have worked well for a long time, are open source (consistency with the [_**Kerckhoffs principle**_](https://www.techtarget.com/whatis/definition/Kerckhoffs-principle)), and have been subjected to research and attacks for many years. However, nothing lasts forever and gradually the security of these algorithms will fall due to the advent of quantum computers.

As mentioned earlier, quantum computers are not some magical black box that "calculates something quickly". These are very specific mathematical algorithms, but their difference from the standard mathematical hacking paths is that they use another fundamental science - physics.

In mathematics, it can't happen that zero suddenly becomes one if you don't like the answer. In quantum mechanics, it is possible.

If earlier, within the framework of classical attacks on algorithms, a mathematical approach was used (not taking into account attacks through third-party channels such as signal interception, van Eyck interception, etc.), now physics and the nature of fundamental particles of the Standard Model come into play.

{% hint style="info" %}
Therefore, we have a very specific need for the use of post-quantum security mechanisms at KLYNTAR. We need to secure everything from signatures to architectural concepts due to the fact that we rely on the security of other chains, so if they are vulnerable, then this could threaten us (it could, because it does not threaten now).
{% endhint %}

At the initial stages, we decided to use Dilithium and BLISS signatures. Among other algorithms, their ratio of security and sizes of public keys + signatures seemed to be optimal. They show good speed and can take part in various events on KLYNTAR. We recommend using them as an advanced security address. For example, you can store large amounts on them and use them infrequently or even through a cold wallet.

_**Dilithium**_

![](<../.gitbook/assets/image (4).png>)

This algorithm is a NIST candidate and provides the ability to generate key pairs and signatures. Widely popular, studied at the highest levels. It is included in the post-quantum implementation of _<mark style="color:red;">**OpenSSL**</mark>_, is being studied by CloudFlare, and is included in their CIRCL repository.

![](<../.gitbook/assets/image (5).png>)

{% embed url="https://github.com/cloudflare/circl" %}

We just use this implementation from CloudFlare by providing a standard set of functions like generate, sign and verify. There are several implementations depending on the NIST security levels. Here are their characteristics:

![](<../.gitbook/assets/image (6).png>)

The creators themselves recommend using the Dilithium3 parameter set, but we use Dilithium5 due to greater security. However, to change the security level, it is enough to simply change one line so that such changes can be made if necessary. Here is a comparison table of security levels according to NIST

<table><thead><tr><th width="302.71790284058636" align="center">NIST security level</th><th width="267.9124899508415" align="center">Hack is so difficult as...</th></tr></thead><tbody><tr><td align="center">1</td><td align="center">Bruteforcing AES-128 key</td></tr><tr><td align="center">2</td><td align="center">Find a collision for SHA-256</td></tr><tr><td align="center">3</td><td align="center">Bruteforcing AES-192 key</td></tr><tr><td align="center">4</td><td align="center">Find a collision for SHA-384</td></tr><tr><td align="center">5</td><td align="center">Bruteforcing AES-256 key</td></tr></tbody></table>

_**BLISS**_

The second and priority post-quantum signature algorithm will be BLISS. It is also based on lattice cryptography, more specifically RLWE (Ring Learning With Errors). Although it creates a small signature and has good security, it has not been listed as a candidate for standardization by NIST. It uses the Fiat-Shamir trellis signature scheme and its improved sample selection method for parameters. It also uses Huffman encoding to compress the signature.

{% embed url="https://bliss.di.ens.fr/" %}

{% embed url="https://asecuritysite.com/signatures/go_bliss" %}

Get the details about post-quantum accounts here:

{% content-ref url="../web1337/transactions-and-smart-contracts/post-quantum-transactions.md" %}
[post-quantum-transactions.md](../web1337/transactions-and-smart-contracts/post-quantum-transactions.md)
{% endcontent-ref %}

{% content-ref url="../smart-contracts-and-vms/advanced-vms-usage/cryptography/post-quantum-cryptography.md" %}
[post-quantum-cryptography.md](../smart-contracts-and-vms/advanced-vms-usage/cryptography/post-quantum-cryptography.md)
{% endcontent-ref %}

## Zero Knowledge Proofs

### zkSNARK

We understand the importance of using zero-knowledge cryptography in various areas and strive for full integration with existing and new solutions.

Since KLYNTAR has a support of both WASM and EVM compatible virtual machines (and others in the future), you can use existing solutions to write your contracts and services using zkSNARK technologies.

Check out the libraries and repositories below that you will need to get started. All of them have good documentation, a rating on GitHub, a community of developers and are used in other projects. We recommend that you follow the guidelines and recommendations.

**Circom**

<figure><img src="../.gitbook/assets/image (95).png" alt=""><figcaption><p><a href="https://github.com/iden3/circom"><strong>https://github.com/iden3/circom</strong></a></p></figcaption></figure>

Circom is a special language that can be used to write logic that will then be used to build R1CS, verification keys and more. The new version of the compiler is written in Rust. The documentation has a sufficient number of examples and step-by-step explanations of what needs to be done to create zero-knowledge logic.

**SnarkJS**

SnarkJS can be used to perform a trusted configuration ceremony, generate a verifier Solidity contract, and much more.

<figure><img src="../.gitbook/assets/image (96).png" alt=""><figcaption></figcaption></figure>

More information can be found on the official websites. Below you will see links to repositories that will help you create your own zero-knowledge project.

<figure><img src="../.gitbook/assets/image (97).png" alt=""><figcaption></figcaption></figure>

* [https://github.com/iden3/circom](https://github.com/iden3/circom)
* [https://docs.circom.io/](https://docs.circom.io/)
* [https://github.com/iden3/snarkjs](https://github.com/iden3/snarkjs)

### zkSTARK

Coming soon

## VRF - verifiable random

A verifiable random function (VRF) is a cryptographic primitive that uses a pair of keys and, based on some input data, can generate pseudo-random values ​​while also creating proof that it was calculated correctly.

This "testable randomness" can be extremely important for special use cases within services.

Only the owner of the private key can calculate the hash and proof, but anyone with the public key can verify the hash is correct and the proof is valid.

Let's imagine that a group of people (2 people) are playing a game in which everyone receives the input string <mark style="color:purple;">**THIS IS A GAME**</mark> and generates 32 byte hashes using VRF. This continues 3 times and each time the input will be the hash obtained in the previous step. After that, they sum up the hash values. Whoever has the most amount wins. Each of them has a pair of keys and each knows the public key of the other. You can visualize it like this

<figure><img src="../.gitbook/assets/image (98).png" alt=""><figcaption></figcaption></figure>

At the same time, both Alice and Bob will be sure of the impossibility of obtaining other data and the fairness of this game.

### Where can it be useful and where is it applied?

Here we explained the VRF mechanism in a simplified way, but Algorand consensus works similarly.

{% embed url="https://medium.com/algorand/algorand-releases-first-open-source-code-of-verifiable-random-function-93c2960abd61" %}

We assume the use of VRF in smart contracts and services on KLYNTAR. Also, it's a great method for MEV-resistance or other stuff in workflows which require randomness.

## FHE - fully homomorphic encryption

<figure><img src="../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

**Fully Homomorphic Encryption (FHE)** is an advanced cryptographic technique that allows computations to be performed on encrypted data without ever decrypting it. This means sensitive data can remain secure throughout the computation process, even in untrusted environments. The result of the computation is also encrypted and can be decrypted only by the data owner.

### Key Idea

FHE enables operations such as addition and multiplication on ciphertexts, preserving the structure of the original data. For example:

Let `E(x)` denote the encryption of a value `x`. In an FHE system:

* **Addition**: `E(a + b) = E(a) + E(b)`
* **Multiplication**: `E(a * b) = E(a) * E(b)`

This property makes FHE particularly useful for scenarios where privacy is critical, such as:

* **Secure Data Analysis**: Performing analytics on encrypted datasets without exposing sensitive information.
* **Cloud Computing**: Allowing cloud providers to compute on encrypted data without learning anything about the data itself.
* **Healthcare**: Enabling secure processing of medical records to preserve patient confidentiality.

By keeping data encrypted throughout the computation, FHE provides a powerful way to enhance privacy and security in the digital age.

### Where we'll use it ?

We plan to implement FHE functionality into EVM and WASM virtual machines to allow you to use them from smart contracts.

### Which implementation we'll use ?

We will use the state-of-the-art open-source solution from Zama, which created a popular repository for working with FHE. This implementation is reliable and is already used in a various of projects.

{% embed url="https://crates.io/crates/tfhe" %}

## And one more thing

We are interested in qualitative improvement of the crypto industry, therefore more and more new cryptographic algorithms will be constantly added for their native use on the project.

Join the open-source development!

{% embed url="https://github.com/KlyntarNetwork" %}
