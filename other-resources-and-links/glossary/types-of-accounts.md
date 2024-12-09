---
icon: key
---

# Types of accounts

## User accounts

### 1. Ed25519

1. 32-bytes, Base58 encoded
2. Looks like Solana address

**Examples:**

1. `6XvZpuCDjdvSuot3eLr24C1wqzcf2w4QqeDh9BnDKsNE`
2. `9GQ46rqY238rk2neSwgidap9ww5zbAN4dyqyC7j5ZnBK`
3. `GUbYLN5NqmRocMBHqS183r2FQRoUjhx1p5nKyyUBpntQ`

### 2. BLS

1. 48-bytes, hex-encoded, 0x-prefixed

**Examples:**

1. `0x8f079049121d5e2ae885bdc6581df9fb68eab94a7aa3ae54bfe1d1ac35aceefbb202f656b0c1b56d64583630612a9970`
2. `0xb2ec32c9d7216163790ba3628a6a6b5a12db457c933b1f4627775b6dae468636233c6ad9931a8ef848a58353e60d33dd`

### 3. TBLS

1. 48-bytes, hex-encoded, looks like BLS but <mark style="color:red;">**without**</mark> **0x**-prefix. Rootkey for the whole group

**Examples:**

1. `b2ec32c9d7216163790ba3628a6a6b5a12db457c933b1f4627775b6dae468636233c6ad9931a8ef848a58353e60d33dd`

### 4. PQC - Post quantum accounts

1. 32-bytes, hex-encoded, BLAKE3 hash of post-quantum pubkey

**Examples:**

1. `4218fb0aaace62c4bfafbdd9adb05b99a9bf1a33eeae074215a51cb644b9a85c`

Check yourself:

1.  The pubkey is&#x20;

    ```json
    0012d71baf1524047e13c5006d00cf0cc3123e0ffe00941dda123a1c1806b50d261b660da60414067b13220793131b1d87099d0571175e0884092512c80d4308ab074e090502220c3519001ac10aad1126085e1c270cf815dc10dc04b508931a870b6619e0067e10cf0a7f1c3b04841452174400fc08ed0507040d1d39176b025b06d317e90057145017090e3907201dd50818020e0e74003504400a1a182c14f609f6117902981367191104050add14bd0b031af10c3e02a1160003011a5b137d00c8167b04521c4b1b9016250aeb01b7038d10a818da144406c91bca1b33195e0fd20930193e0dfa11f20f340da50b1215b51d21197c11060de009eb0c8201fb14110be00ec503bd065207a70953132d1a38115b153507da0a3e01290c8016af1d2c18a417100c1508cc112f146a130c013b014704471dbc02c20038013415621985124419ae10a501170eb70e6d0b220ee405ef17ff1c9b0dce0a1f07a204cf1b7b18b9013a0bdc00af187d169e050e0c201b5915c709b011db11170b06159b1cbe03691d860d00028d187d0e61074a1673027a047f16281bac0cfd09a00a62050c07ee1058020e006407de0adc1036136b10b417eb1b12155919b105f60b1d0bde0a57127b0007087d150c11690b7800930f1e16ec19ac0b8d1d7e1b0f02321c90148d1a47075a091113c9159e051113b403b5063001d3186e13b211c70d20
    ```
2. The formula is:

$$
BLAKE3(pubkey) = 4218fb...
$$

### 5. EVM compatible account

1. 20-bytes, hex-encoded, 0x-prefixed, looks like Ethereum address

**Examples:**

1. `0x407d73d8a49eeb85d32cf465507dd71d507100c1`
2. `0x069bdf66961ce2D38eBe48DD2E095f2c8015ac82`

## Contract accounts

### 1. WASM contract account ID

1. 32-bytes, hex-encoded, 0x-prefixed

**Examples:**

1. `0x0000000000000000000000000000000000000000000000000000000000000000`
2. `0x8f079049121d5e2ae885bdc6581df9fb68eab94a7aa3ae54bfe1d1ac35aceefb`&#x20;

### 2. EVM contract account ID

1. 20-bytes, hex-encoded, 0x-prefixed, looks like Ethereum address

**Examples:**

1. `0x1000000000000000000000000000000000000007`
2. `0x9f8E68B1974069B854eE481deC0819697B901f1D`&#x20;
