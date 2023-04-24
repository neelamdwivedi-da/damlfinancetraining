# Testing Creating Accounts

In this lesson, you will learn how to test the account creation step coded in **CreateAccount.daml** as part of the Transfer workflow. 

----------

<img src="../Images/DF-Diagram12-TestCreateAccount.png" width=500>

As shown in the figure above, we want to test the following workflow to test the CreateAccounts functionality: 

1. Create Account factory
2. Create Holding factory
3. Using the contracts ids from above
- Alice creates account request
- Bob creates account request
4. Bank accepts Alice's request
5. Bank accepts Bob's request

