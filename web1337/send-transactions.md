# 🟠 Send transactions

## Create and send default transaction

Here we propose the example of simple transaction from default account(ed25519) to any other account

#### Create

{% code fullWidth="false" %}
```javascript
import Web1337 from '@klyntar/web1337'


let web1337 = new Web1337({

    symbioteID:'aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa',
    workflowVersion:0,
    nodeURL: 'http://localhost:7332'

});


// Get it using await crypto.kly.generateDefaultEd25519Keypair()

const myKeyPair = {

    mnemonic: 'south badge state hedgehog carpet aerobic float million enforce opinion hungry race',
    bip44Path: "m/44'/7331'/0'/0'",
    pub: '2VEzwUdvSRuv1k2JaAEaMiL7LLNDTUf9bXSapqccCcSb',
    prv: 'MC4CAQAwBQYDK2VwBCIEIDEf/4H5iiY3ebAfWsFIFkeZrB8HpcvBYK5zjEe9/8ga'
      
}

const subchain = '7GPupbq1vtKUgaqVeHiDbEJcxS7sSjwPnbht4eRaDBAEJv8ZKHNCSu2Am3CuWnHjta'


let signedTx = await web1337.createDefaultTransaction(subchain,myKeyPair.pub,myKeyPair.prv,0,'7GPupbq1vtKUgaqVeHiDbEJcxS7sSjwPnbht4eRaDBAEJv8ZKHNCSu2Am3CuWnHjta',5,10)

console.log(signedTx)
```
{% endcode %}

#### Result(see the structure)

```json5
{
  v: 0,
  creator: '2VEzwUdvSRuv1k2JaAEaMiL7LLNDTUf9bXSapqccCcSb',
  type: 'TX',
  nonce: 0,
  fee: 5,
  payload: {
    type: 'D',
    to: '7GPupbq1vtKUgaqVeHiDbEJcxS7sSjwPnbht4eRaDBAEJv8ZKHNCSu2Am3CuWnHjta',
    amount: 10
  },
  sig: 'trZF0uHyFSMlPRkHgKsjCw5UwjacmBsNH3Zq9SLWnnEMRn5bccPhVPvMKiPhgQkPyMIBuzW76GcoDXaYB0usAg=='
}
```

#### Send

```javascript
let txStatus = await web1337.sendTransaction(signedTx)

console.log(txStatus)

//After that - you can check the tx receipt

let receipt = await web1337.getTransactionReceiptById(web1337.BLAKE3(signedTx.sig))
```



## Create and send multisig transaction





## Create and send  thresholdsig transaction





## Create and send transaction from post-quantum account



##
