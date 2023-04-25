# Test Creating Accounts

In this lesson, you will learn how to test the account creation step coded in **CreateAccount.daml** as part of the Transfer workflow. 

----------

<img src="../Images/DF-Diagram12-TestCreateAccount.png" width=800>

As shown in the figure above, we want to test the following workflow to test the CreateAccounts functionality: 

1. Create Account factory
2. Create Holding factory
3. Use the contracts ids from above for 
- Alice to create account request
- Bob to create account request
4. Bank accepts Alice's request
5. Bank accepts Bob's request


Let us first write the code to create Alice's account. 

```
module Scripts.Transfer where

import DA.Map (empty, fromList)
import DA.Set (singleton)
import Daml.Script

-- INTERFACE DEPENDENCIES --
import Daml.Finance.Interface.Account.Factory qualified as Account (F)
import Daml.Finance.Interface.Holding.Factory qualified as Holding (F)

-- IMPLEMENTATION DEPENDENCIES --
import Daml.Finance.Account.Account qualified as Account (Factory(..))
import Daml.Finance.Holding.Fungible qualified as Fungible (Factory(..))

import Workflow.CreateAccount qualified as CreateAccount

testCreateAccount: Script () 
testCreateAccount = script do 
 
    [alice, bank, public] <- mapA allocateParty ["Alice", "Bank", "Public"] 

    -- create Account Factory
    accountFactoryImplCid <- submit bank do
        createCmd Account.Factory with provider = bank; observers = empty
    -- convert the factory implementation cid to interface Cid 
    let accountFactoryInterfaceCid = toInterfaceContractId @Account.F accountFactoryImplCid

    -- create Holding factory
    holdingFactoryImplCid <- submit bank do 
        createCmd Fungible.Factory with 
            provider = bank 
            observers = fromList [("PublicObserver", singleton public)]
    -- convert the factory implementation cid to interface Cid
    let holdingFactoryInterfaceCid = toInterfaceContractId @Holding.F holdingFactoryImplCid

    -- alice requests for an account
    aliceRequestCid <- submit alice do 
        createCmd CreateAccount.Request with 
            owner = alice
            custodian = bank 
    -- bank accepts request
    aliceAccountCid <- submit bank do 
        exerciseCmd aliceRequestCid CreateAccount.Accept with 
            label = "Alice@Bank"
            description = "Alice's account"
            accountFactoryCid = accountFactoryInterfaceCid 
            holdingFactoryCid = holdingFactoryInterfaceCid
            observers = []

    return ()

```

### <a name="createAccountFactory"></a>Create Account Factory

The first important step is creating the account factory

```
    -- create Account Factory
    accountFactoryImplCid <- submit bank do
        createCmd Account.Factory with provider = bank; observers = empty
    -- convert the factory implementation Cid to interface Cid 
    let accountFactoryInterfaceCid = toInterfaceContractId @Account.F accountFactoryImplCid
```

Notice that we need to use the account factory's implementation, i.e. **Account.Factory** to create the factory. However, the account created from this factory needs its interface type in the accountFactoryCid field, so we use **toInterfaceContractCId** to convert it into interfce type. 

When we run this part of the script, the ledger gets the Account Factory contract created as shown below:

**Daml.Finance.Account.Account:Factory@373828d61ed38318304f35dbd2cf6c49d498c48c7baeea31cfa5f30cabef018a**
|id	| status |	provider |	observers| Bank |
|---|--------|-----------|-----------|-----|	
|#0:0|	active |	'Bank' |	GenMap[] | X |


### Create Holding Factory

Same logic as above applies to holding factory as well. 

```
    -- create Holding factory
    holdingFactoryImplCid <- submit bank do 
        createCmd Fungible.Factory with 
            provider = bank 
            observers = fromList [("PublicObserver", singleton public)]
    -- convert the factory implementation Cid to interface Cid
    let holdingFactoryInterfaceCid = toInterfaceContractId @Holding.F holdingFactoryImplCid
```

An important observation here is the use of **Fungible.Factory**. If you recall from previous lessons, a **holding** represents the ownership of a certain amount of an instrument by an owner at a custodian. A holding can have specific properties such as being **fungible / non-fungible**, or **transferable**. These properties are expressed in Daml Finance library as three interfaces and three implementations of the holding factory that result in the three different types of holdings. This structure is depicted in the diagram below:

<img src="../Images/DF-Diagram14-Holdings.png" width=1500>


The holding that implements the Base interface results in a Non-transferable holding. The Transferable interface when implemented gives a Transferable holding, and a Fungible interface implementation gives us a Transferable as well as Fungible holding. 

In the example code shown above, we want the account to hold cash instrument which is a fungible and a transferable instrument. And the factory that can create such a holding is in **Daml.Finance.Holding.Fungible** package. 

This part of the script creates the Holding factory contract in the ledger as shown below:

**Daml.Finance.Holding.Fungible:Factory@ff792348761d003dd4893aef20f4b6154b00acc84642c390768aeffdd96b56a7**
|id |	status	| provider	| observers	| Bank | Public |
|---|-----------|-----------|-----------|------|--------|
|#1:0 |	active	|'Bank' |	GenMap["PublicObserver"->DA.Set.Types:Set@97b883cd8a2b7f49f90d5d39c981cf6e110cf1f1c64427a28a6d58ec88c43657 with map = GenMap['Public'->{}]]	| X | X |


### Create Account

Finally, the steps for alice requesting for an account and the bank accepting the request resulting in account creation are fairly straighfroward. First, Alice submits a request:

```
   -- alice requests for an account
    aliceRequestCid <- submit alice do 
        createCmd CreateAccount.Request with 
            owner = alice
            custodian = bank
```

which results in a Request contract created in the ledger:

**Workflow.CreateAccount:Request**
|id	| status	 | custodian |	owner	| Alice | Bank |
|---|------------|-----------|----------|-------|------|
|#2:0 |	active	| 'Bank' |	'Alice' | X | X |


and then the bank accepts the request


```
    -- bank accepts request
    aliceAccountCid <- submit bank do 
        exerciseCmd aliceRequestCid CreateAccount.Accept with 
            label = "Alice@Bank"
            description = "Alice's account"
            accountFactoryCid = accountFactoryInterfaceCid 
            holdingFactoryCid = holdingFactoryInterfaceCid
            observers = []
```

which archives the Request contract created above and creates Alice's Account. 


**Workflow.CreateAccount:Request**

|id	| status	| custodian |	owner | Alice | Bank |
|---|-----------|-----------|--------|------|------|
|~~#2:0~~	| ~~archived~~ |	~~'Bank'~~ |	~~'Alice'~~ | X | X |


**Daml.Finance.Account.Account:Account@373828d61ed38318304f35dbd2cf6c49d498c48c7baeea31cfa5f30cabef018a**
|id	| status | custodian |	owner |	controllers.outgoing.map	| controllers.incoming.map	| id.unpack |	description |	holdingFactoryCid	| observers	| Alice | Bank |
|---|--------|-----------|--------|-----------------------------|---------------------------|-----------|---------------|-----------------------|-----------|-------|------|
|#3:2|	active|	'Bank'	|'Alice'|	GenMap['Alice'->{}] |	GenMap['Alice'->{}] |	"Alice@Bank"	| "Alice's account"	| #1:0	| GenMap["AccountObservers"->DA.Set.Types:Set@97b883cd8a2b7f49f90d5d39c981cf6e110cf1f1c64427a28a6d58ec88c43657 with map = GenMap[]]		| X | X |


Note that the account has the holdingFactoryCid in its payload to represent the type of instrument that the account can carry. 

We now have an account factory, a holding factory, and an account for Alice. 


Go ahead and complete the script in a similar way by having Bob submit an account request and then Bank accepting the request to create Bob's account. 
