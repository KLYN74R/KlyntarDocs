---
description: Simple, one-line APIs. No rocket science
---

# 🟢 Simple API requests

## Postman

All APIs that the KLY core contains are well documented and available in our Postman organization

<figure><img src="../../.gitbook/assets/image (6) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

{% embed url="https://documenter.getpostman.com/view/25402389/2s93Y2S2FJ#097a6b76-d477-48c0-90b2-edb5743ebbf7" %}

Please, follow the link to test it on your own and check the useful example for each request :relaxed:

## Multilanguage support

<figure><img src="../../.gitbook/assets/Web1337Cover.svg" alt=""><figcaption></figcaption></figure>

Web1337 SDK has implementations for various languages ​​such as JS(Node.js), Golang, Python and others!

In the initial stages, this documentation will provide an example of an API call for the JS SDK, but in other languages ​​the methods and their names have a similar meaning.

Also, don't forget to use the Postman documentation where you can learn about queries and data formats.

## Create the Web1337 instance

Here you just need to import the library to your project workspace and create the instance using this snippet:

```javascript
import Web1337 from 'web1337';


let web1337 = new Web1337({

    chainID:'aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa',
    workflowVersion:0,
    nodeURL:'http://localhost:7332'
    proxyURL:'http://localhost:8888' // optional - for example 'http(s)://login:password@127.0.0.1:8080' or 'socks5h://Vlad:Cher@127.0.0.1:9150'
    
});
```

* **chainID** - 256-bit identifier of appropriate symbiote(something like chain id in EVM). On a technical level, this is the BLAKE3 manifest hash of the chain file which includes the genesis hash, the repository version hash and so on. For maximum security, it is recommended to include the manifest in several blockchains (hostchains) at once
* **workflowVersion** - major version number of workflow of your symbiote to make sure you know the further logic in this version
* **nodeURL** - the endpoint of node to interact with network. It might be your own node, Node-as-a-Service provider, etc.
* **proxyURL **<mark style="color:red;">**(optional)**</mark> - the URL of proxy that will be used to interact with node. It might be HTTP(s) or SOCKS proxy(to allow connections over TOR/I2P or other SOCKS)

{% hint style="info" %}
Only three first components are required for proper work
{% endhint %}

## Call methods

Once you create web1337 instance you have ability to call methods without manual request construction.

For example, to get block by ID, call:

```javascript
// Block id format is <epochIndex>:<pubkeyOfCreator>:<blockIndexInEpoch>
let blockID = "1:9GQ46rqY238rk2neSwgidap9ww5zbAN4dyqyC7j5ZnBK:15";

await web1337.getBlockByBlockID(blockID);
```

For example, to get information about pool call

```javascript
let poolID = "6XvZpuCDjdvSuot3eLr24C1wqzcf2w4QqeDh9BnDKsNE(POOL)";

await web1337.getPoolStats(poolID);
```

Or, to check the synchronization status of your node, call

```javascript
await web1337.getSynchronizationStatus()
```

## Try yourself :nerd:

Try playing and studying the methods available in the SDK. If necessary, as mentioned earlier, check Postman.
