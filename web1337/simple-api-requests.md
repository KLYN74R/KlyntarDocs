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

##

## Get the current checkpoint

#### Request

{% code fullWidth="false" %}
```javascript
await web1337.getCurrentCheckpoint())
```
{% endcode %}

#### Response

{% hint style="info" %}
Note - the data here is a mock data, but the structure is right
{% endhint %}

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

#### Response

## Get block by BlockID

#### Request

#### Response

## Get block by GRID

#### Request

#### Response

## Get block by SID

#### Request

#### Response

## Get aggregated finalization proof

#### Request

#### Response

## Get information about infrastructure

#### Request

#### Response

## Get current state of node

#### Request

#### Response
