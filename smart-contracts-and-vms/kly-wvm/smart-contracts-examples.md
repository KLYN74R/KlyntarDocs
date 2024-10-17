---
icon: eye
---

# Smart contracts examples

## ERC-20 on AssemblyScript example

```typescript
import {JSON} from "assemblyscript-json";

// Declare imports
@external("klyntar", "getFromState")
declare function getFromState(key: string): string;

@external("klyntar", "setToState")
declare function setToState(key: string, value: string): void;

@external("klyntar", "getCallerAddress")
declare function getCallerAddress(): string;


// Constants to store data

const ACCOUNT_KEY_PREFIX = "account:";


function getParsedAccount(accountID: string):JSON.Obj {

    return <JSON.Obj>(JSON.parse(getFromState(ACCOUNT_KEY_PREFIX+accountID)));

}


export function totalSupply(): string {
    return getFromState("totalSupply");
}


export function balanceOf(address: string): string {
    
    let account: JSON.Obj = <JSON.Obj>(JSON.parse(getFromState(ACCOUNT_KEY_PREFIX+address))); // {balance:1337}

    let balance: JSON.Integer | null = account.getInteger("balance");

    if(balance!== null){

        return balance.stringify();

    } else return "0"

}


// ERC-20: Transfer from address to address
// Input example: {to:"recipientAccount",amount:1337}
export function transfer(objWithToAndAmount: string): boolean {
    
    let callerAddress = getCallerAddress();

    let handlerWithToAndAmount: JSON.Obj = <JSON.Obj>(JSON.parse(objWithToAndAmount));

    // Parse <to> and <amount>

    let recipientAddress: JSON.Str | null = handlerWithToAndAmount.getString("to");

    let amountToTransfer: JSON.Integer | null = handlerWithToAndAmount.getInteger("amount");



    if(recipientAddress !== null && amountToTransfer !== null && amountToTransfer._num > 0) {

        // Get accounts

        let senderAccount: JSON.Obj = getParsedAccount(callerAddress); // {balance:1337}

        let recipientAccount: JSON.Obj = getParsedAccount(recipientAddress._str); // {balance:1337}

        if(recipientAccount.getInteger("balance") === null){

            recipientAccount = JSON.Value.Object();

            recipientAccount.set("balance",JSON.from(0));

        }

        let senderBalance: JSON.Integer | null = senderAccount.getInteger("balance");

        if(senderBalance !== null && senderBalance >= amountToTransfer){

            senderAccount.set("balance",JSON.from(senderBalance._num-amountToTransfer._num));

            let recipientBalance: JSON.Integer | null = recipientAccount.getInteger("balance");

            if(recipientBalance !== null){

                recipientAccount.set("balance",JSON.from(recipientBalance._num-amountToTransfer._num));

            }

            return true;

        } else return false;

    } else  return false;

}


export function mint(initialSupplyStr: string): void {}

```

## ERC-20 using Rust language

## Getter/Setter using AssemblyScript

```typescript
import {JSON} from "assemblyscript-json";



@external("klyntar","getFromState")
declare function getFromState(key: string): string;

@external("klyntar","setToState")
declare function setToState(key: string, value: string): void;



/*

    1) In parameter we receive object like {name:"NewName"} in JSON serialized form
    2) Get the object {name:"OldNameValue"} from state using <getFromState> function
    3) Using <setToState> function - set new value of .name field and store the whole object to state

*/
export function changeName(objWithNewName:string):void {

    // objWithNewName has format: {name:"NewNameValue"}

    let handlerWithNewName: JSON.Obj = <JSON.Obj>(JSON.parse(objWithNewName));

    let newPotentialName: JSON.Str | null = handlerWithNewName.getString("name");


    let handlerWithOldName: JSON.Obj = <JSON.Obj>(JSON.parse(getFromState("nameHandler")));

    let oldName: JSON.Str | null = handlerWithOldName.getString("name");


    if(newPotentialName !== null && newPotentialName !== oldName) {

        setToState("nameHandler",handlerWithNewName.stringify());

    }

}

```

