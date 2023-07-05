---
description: Simple, one-line APIs. No rocket science
---

# 🟢 Simple API requests

## Create the Web1337 instance



Here you just need to import the library to your project workspace and create the instance using this snippet:

```javascript
import Web1337 from '@klyntar/web1337'


let web1337 = new Web1337({

    symbioteID:'aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa',
    workflowVersion:0,
    nodeURL:'http://localhost:7332'
    proxyURL:'http://localhost:8888' // for example 'http(s)://login:password@127.0.0.1:8080' or 'socks5h://Vlad:Cher@127.0.0.1:9150'
    
});
```

* **symbioteID** - 256-bit identificator of appropriate symbiote(something like chain id in EVM)
* **workflowVersion** - major version number of workflow of your symbiote to make sure you know the furthe logic in this version
* **nodeURL** - the endpoint of node to interact with network. It might be your own node, Node-as-a-Service provider, etc.
* **proxyURL** - the URL of proxy that will be used to interact with node. It might be HTTP(s) or SOCKS proxy(to allow connections over TOR/I2P or other SOCKS)

{% hint style="info" %}
Only three first components are required for proper work
{% endhint %}





***





{% hint style="info" %}
Note - the data here is a mock data, but the structure of responses is right
{% endhint %}

## Get the current checkpoint

#### Request

{% code fullWidth="false" %}
```javascript
await web1337.getCurrentCheckpoint())
```
{% endcode %}

#### Response

```json5
{
  header: {
    id: -1,
    payloadHash: '0123456789abcdef0123456789abcdef0123456789abcdef0123456789abcdef0123456789abcdef0123456789abcdef0123456789abcdef0123456789abcdef',
    quorumAggregatedSignersPubKey: '',
    quorumAggregatedSignature: '',
    afkVoters: []
  },
  payload: {
    prevCheckpointPayloadHash: '',
    poolsMetadata: {
      '7GPupbq1vtKUgaqVeHiDbEJcxS7sSjwPnbht4eRaDBAEJv8ZKHNCSu2Am3CuWnHjta':{index:1337,hash:'',isReserve:false},
      '75XPnpDxrAtyjcwXaATfDhkYTGBoHuonDU1tfqFc6JcNPf5sgtcsvBRXaXZGuJ8USG':{index:-1,hash:'',isReserve:false},
      '61TXxKDrBtb7bjpBym8zS9xRDoUQU6sW9aLvvqN9Bp9LVFiSxhRPd9Dwy3N3621RQ8':{index:0,hash:'',isReserve:false},
      '6YHBZxZfBPk8oDPARGT4ZM9ZUPksMUngyCBYw8Ec6ufWkR6jpnjQ9HAJRLcon76sE7':{index:1,hash:'',isReserve:true}
    },
    operations: [],
    otherSymbiotes: {}
  },
  timestamp: 1688428143239,
  completed: true,
  quorum: [
    '7GPupbq1vtKUgaqVeHiDbEJcxS7sSjwPnbht4eRaDBAEJv8ZKHNCSu2Am3CuWnHjta',
    '75XPnpDxrAtyjcwXaATfDhkYTGBoHuonDU1tfqFc6JcNPf5sgtcsvBRXaXZGuJ8USG',
    '61TXxKDrBtb7bjpBym8zS9xRDoUQU6sW9aLvvqN9Bp9LVFiSxhRPd9Dwy3N3621RQ8',
    '6YHBZxZfBPk8oDPARGT4ZM9ZUPksMUngyCBYw8Ec6ufWkR6jpnjQ9HAJRLcon76sE7'
  ],
  reassignmentChains: {
    '7GPupbq1vtKUgaqVeHiDbEJcxS7sSjwPnbht4eRaDBAEJv8ZKHNCSu2Am3CuWnHjta': [
      '6YHBZxZfBPk8oDPARGT4ZM9ZUPksMUngyCBYw8Ec6ufWkR6jpnjQ9HAJRLcon76sE7'
    ],
    '75XPnpDxrAtyjcwXaATfDhkYTGBoHuonDU1tfqFc6JcNPf5sgtcsvBRXaXZGuJ8USG': [],
    '61TXxKDrBtb7bjpBym8zS9xRDoUQU6sW9aLvvqN9Bp9LVFiSxhRPd9Dwy3N3621RQ8': []
  }
}
```



## Get aggregated finalization proof

#### Request

```javascript
await web1337.getAggregatedFinalizationProofForBlock('7GPupbq1vtKUgaqVeHiDbEJcxS7sSjwPnbht4eRaDBAEJv8ZKHNCSu2Am3CuWnHjta:0')
```

* **blockID** - id of block in form _<mark style="color:red;">**BlsPubKey:Index**</mark>_

#### Response

```json5
{
  blockID: '7GPupbq1vtKUgaqVeHiDbEJcxS7sSjwPnbht4eRaDBAEJv8ZKHNCSu2Am3CuWnHjta:0',
  blockHash: 'd6682d62f263fe16943f0c2cd36f78b3e7dda73f2dda3514b59908d75d01d769bd6be09a6ecb10e09bfc938a2a11854d6c27bd99ddacd9e684dad8bc9f0e552a',
  aggregatedPub: '6yeebqG6qBxiSx126Sg6FHqrkBW94Smmqp1ifEUK1TLQEumstVTrZFx4UTBJdvhKwb',
  aggregatedSignature: 'lMqaRrjtp5LwFwwDJbTWYiMfi4BwQ8YM7niYeRVLNaB9S/mBFiyeprQekRs8/DjSBVYDGd6pXSFlPj4WgoS26dO3/R7p/9yJn0Gr4S48yBOpJ/3mG1lQH5zbrwHr4cjc',
  afkVoters: [
    '6YHBZxZfBPk8oDPARGT4ZM9ZUPksMUngyCBYw8Ec6ufWkR6jpnjQ9HAJRLcon76sE7'
  ]
}
```

#### To verify that AFP is ok

TODO

##

## Get block by BlockID

#### Request

```javascript
await web1337.getBlockByBlockID('7GPupbq1vtKUgaqVeHiDbEJcxS7sSjwPnbht4eRaDBAEJv8ZKHNCSu2Am3CuWnHjta:0')
```

* **blockID** - id of block in form _<mark style="color:red;">**BlsPubKey:Index**</mark>_

#### Response

```json5
{
  creator: '7GPupbq1vtKUgaqVeHiDbEJcxS7sSjwPnbht4eRaDBAEJv8ZKHNCSu2Am3CuWnHjta',
  time: 1688428206254,
  checkpoint: '0123456789abcdef0123456789abcdef0123456789abcdef0123456789abcdef0123456789abcdef0123456789abcdef0123456789abcdef0123456789abcdef#-1',
  transactions: [],
  extraData: { rest: { hello: 'world' } },
  index: 0,
  prevHash: '0123456789abcdef0123456789abcdef0123456789abcdef0123456789abcdef0123456789abcdef0123456789abcdef0123456789abcdef0123456789abcdef',
  sig: 'gh9litqCOrQ89VVNMlnDu1zDa/Hy6ILBr7R5PzSkKtdH+FIlS3bcWq42l1X9bgwLCqt/mn0WFJQaQ6Gb6R5D9kKNxkQD6BqE+8LZogr04jPvQ7PCB00DLe9qg9xut/J0'
}
```

##

## Get block by GRID

#### Request

```javascript
await web1337.getBlockByGRID(0)
```

* **GRID(General ID)** - just index of block in case we imagine that KLY is linear with indexes _**0, 1 , 2...N**_&#x20;

#### Response

```json5
{
  creator: '75XPnpDxrAtyjcwXaATfDhkYTGBoHuonDU1tfqFc6JcNPf5sgtcsvBRXaXZGuJ8USG',
  time: 1688428218999,
  checkpoint: '0123456789abcdef0123456789abcdef0123456789abcdef0123456789abcdef0123456789abcdef0123456789abcdef0123456789abcdef0123456789abcdef#-1',
  transactions: [],
  extraData: { rest: { hello: 'world' } },
  index: 0,
  prevHash: '0123456789abcdef0123456789abcdef0123456789abcdef0123456789abcdef0123456789abcdef0123456789abcdef0123456789abcdef0123456789abcdef',
  sig: 'iayyJRq3vGled0pbF2RPsThPrlrGsJcGWYwzRKYbJr9hv92I3iEGPgUnI/siQbBQBd9mgo8aXZ+EUlhOvxyuBx4hv5VegLdma/YOYY3TYGUZRrzG/gXCUSIlcxByY8lw'
}
```

##

## Get block by SID

#### Request

```javascript
await web1337.getBlockBySID('7GPupbq1vtKUgaqVeHiDbEJcxS7sSjwPnbht4eRaDBAEJv8ZKHNCSu2Am3CuWnHjta',0)
```

* **subchainID** - identificator of subchain (Base58 encoded BLS pubkey of prime pool)
* **index** - index of block in this subchain

#### Response

```json5
{
  creator: '7GPupbq1vtKUgaqVeHiDbEJcxS7sSjwPnbht4eRaDBAEJv8ZKHNCSu2Am3CuWnHjta',
  time: 1688428206254,
  checkpoint: '0123456789abcdef0123456789abcdef0123456789abcdef0123456789abcdef0123456789abcdef0123456789abcdef0123456789abcdef0123456789abcdef#-1',
  transactions: [],
  extraData: { rest: { hello: 'world' } },
  index: 0,
  prevHash: '0123456789abcdef0123456789abcdef0123456789abcdef0123456789abcdef0123456789abcdef0123456789abcdef0123456789abcdef0123456789abcdef',
  sig: 'gh9litqCOrQ89VVNMlnDu1zDa/Hy6ILBr7R5PzSkKtdH+FIlS3bcWq42l1X9bgwLCqt/mn0WFJQaQ6Gb6R5D9kKNxkQD6BqE+8LZogr04jPvQ7PCB00DLe9qg9xut/J0'
}
```



## Get information about infrastructure

#### Request

```javascript
await web1337.getGeneralInfoAboutKLYInfrastructure()
```

#### Response

This data has no defined structure because node owner sets this data in configs. Here you can add the info about supported APIs, other nodes in your infrastructure, override the logic of some routes,etc. For example, some public pool can add their Telegram / mail to response to communicate with users:

```json5
{ mail: 'hello@somecoolstakingpool.com', telegram: '@some_cool_staking_pool' }
```

Also, since KLY supports routes disabling(see in configs), you can stop native route implementation, override it and inform node users about this.

Saying, you've found interesting plugin with advance filtering system before adding transactions to mempool and want to use this logic of txs acception instead of native one. For tis you should disable native route:

<pre class="language-javascript"><code class="lang-javascript"><strong>CONFIG.SYMBIOTE.TRIGGERS.MAIN.ACCEPT_TXS:false
</strong></code></pre>

{% hint style="info" %}
See workflow.json to find this option
{% endhint %}

Then, enable the plugin:

```json
"PLUGINS":["some_cool_plugin_to_improve_txs_filters/index.js"]
```

And finally, inform the API users about this redirection:

<pre class="language-json5"><code class="lang-json5"><strong>CONFIG.SYMBIOTE.INFO:{
</strong><strong>
</strong><strong>    "override":{
</strong><strong>    
</strong><strong>        "/transaction":"Guys, use this endpoint to send txs to me => https://some.another.endpoint.io:9999"
</strong><strong>    
</strong><strong>    }
</strong><strong>
</strong><strong>}
</strong></code></pre>

{% hint style="info" %}
We'll show you how to work with plugins in the section [Plugins usage](../deep-dive-into-kly/plugins-usage/)
{% endhint %}



## Get current state of node

#### Request

```javascript
await web1337.getSyncState()
```

#### Response

```json5
{
  subchain: '7GPupbq1vtKUgaqVeHiDbEJcxS7sSjwPnbht4eRaDBAEJv8ZKHNCSu2Am3CuWnHjta',
  currentAuthority: '7GPupbq1vtKUgaqVeHiDbEJcxS7sSjwPnbht4eRaDBAEJv8ZKHNCSu2Am3CuWnHjta',
  index: 9,
  hash: '333970c621724bf923e64513d81c19efdf0346b4844309a6473696b976cb920c2e8e9d229a83b1481b1162c59c9f6f093aef2316d11bb69648e340ad478561bd',
  grid: 30
}
```
