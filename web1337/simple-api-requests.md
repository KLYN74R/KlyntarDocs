# 🟢 Simple API requests

## Get the current checkpoint





#### Request

{% code fullWidth="false" %}
```javascript
import Web1337 from '@klyntar/web1337'


let web1337 = new Web1337({

    symbioteID:'aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa',
    workflowVersion:0,
    nodeURL: 'http://localhost:7332'

});

console.log(`Current checkpoint is => `,await web1337.getCurrentCheckpoint())
```
{% endcode %}

#### Response

> Note - the data here is a mock data, but the structure is right&#x20;

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

## Get block by BlockID



## Get block by GRID



## Get block by SID



## Get aggregated finalization proof

## Get information about infrastructure



## Get current state of node

