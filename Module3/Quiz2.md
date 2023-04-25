# Quiz 2

This quiz will help you self-assess your understanding of the concepts covered in Module 3. 


Q1. Which of the following are the components of an AccountKey as defined in Daml.Finance.Interface.Types.Common.Types?

1. Account factory
2. Custodian
3. Account owner
4. Id
5. All of the above

Answer: 2, 3, 4

Q2. Which of the following element's Cids do you need to create an account using Daml Finance library's Account factory?

1. Account Factory
2. Instrument Factory
3. Holding Factory
4. Disclosure

Answer: 1 and 3


Q3. The Account template in Daml-Finance library has a field called Controllers. What is the purpose of this field?

1. List of parties that can exercise various choices on the account
2. List of parties that can instruct outgoing transfers and approve incoming transfers
3. List of parties that can create more holdings using the holdingFactoryCid stored in the account
4. None of the above

Answer: 2

Q4. How does Daml Finance library support 
different types of holdings - fungible/non-fungible, transferable / non-transferable

1. By providing a field in the view of Holding interface that stores the type of holding as Text information
2. By implementing the Holding factory interface in a way that can create different types of holding based in the parameters passed to it
3. By providing three different interfaces which are implemented in the library to create different types of holdings
4. Daml Finance library does not support different types of holdings as it needs to be defined in the client-side of the application

Answer: 3

Q5. Which of the following is stored in an account's payload created through Daml Finance library's Account factory?

1. Account factory Cid
2. Holding factory Cid
3. Account Cid
4. All of the above

Answer: 2

