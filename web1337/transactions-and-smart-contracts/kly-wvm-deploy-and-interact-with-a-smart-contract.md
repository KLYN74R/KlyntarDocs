# 📃 KLY-WVM - deploy the smart-contract to WASM vm

<figure><img src="../../.gitbook/assets/Meme_Kly_Wvm.jpg" alt=""><figcaption></figcaption></figure>

## Intro

When you have already written and compiled a smart contract in one of the languages ​​supported by WASM, you can perform the deployment procedure.

Below is an example of a deposit from an account like Ed25519, but you can do a similar procedure from other accounts - BLS, TBLS, PostQuantum.

## Code example

```javascript
let web1337 = new Web1337({

    chainID:'aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa',
    workflowVersion:0,
    nodeURL: 'http://localhost:7332'

});

let contractBytecode = fs.readFileSync('./path/to/your/contract.wasm').toString('hex');

const shardID = "shard_0"

let payload = {

    shard:shardID,

    bytecode:contractBytecode,

    lang:'AssemblyScript', // or Rust

    constructorParams:{
        
        // In case you want to add initial storage to contract - set it as object here
        initStorage:{

            nameHandler:{name:"Name_1"}

        }
    }

}


let keypair = {
    
    pub:"9GQ46rqY238rk2neSwgidap9ww5zbAN4dyqyC7j5ZnBK",
    
    prv:"MC4CAQAwBQYDK2VwBCIEILdhTMVYFz2GP8+uKUA+1FnZTEdN8eHFzbb8400cpEU9",

}

const fee = 0.03

const nonce = await web1337.getAccount(shardID,keypair.pub).then(account=>account.nonce+1)

const txType = "WVM_CONTRACT_DEPLOY"

let tx = web1337.createEd25519Transaction(shardID,txType,keypair.pub,keypair.prv,nonce,fee,payload)

// Get the ID of contract immediately
const contractID = web1337.blake3(shardID+tx.creator+tx.nonce)


console.log('Contract ID: '+contractID)

console.log(`Full contract ID: ${shardID}:${contractID}`)

console.log(`TX ID is => `,web1337.blake3(tx.sig))


// Send contract deployment transaction

web1337.sendTransaction(tx).then(()=>{

    console.log('\n\nSent')

}).catch(err=>console.error('Error during contract deployment: ',err))
```

Output:

```code-runner-output
Contract ID: 6a69ea85938d9c1dbfccc60aa7da21aa8326c650392bbe7323fabad985184e32
Full contract ID: shard_0:6a69ea85938d9c1dbfccc60aa7da21aa8326c650392bbe7323fabad985184e32
TX ID is =>  27b01c6b30b5a5ba53179f92e14b131f28caa7d25fb4c6750c1483c92522ec84


Sent
```

## Let's check the explorer

Go to explorer and paste the txid to searchbar. You can also go directly to the page of newly created contract because it's possible to calculate the contractID locally (example above):

<figure><img src="../../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

You'll see

<figure><img src="../../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

It's possible to immediately visit the page of newly created contract by clicking here:

<figure><img src="../../.gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>

The page of new contract:

<figure><img src="../../.gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>

And indeed - this contract has only one transaction - deployment:

<figure><img src="../../.gitbook/assets/image (8).png" alt=""><figcaption></figcaption></figure>

## In case your deployment failed

See[#check-the-reason-of-failed-transaction](useful-advices-and-faq.md#check-the-reason-of-failed-transaction "mention")
