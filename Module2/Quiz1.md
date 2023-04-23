# Quiz 1

This quiz will help you self-assess your understanding of the concepts covered in Module 2. 


Q1. Which of the following patterns helps decouple an application that needs to create contracts from the creational logic written in the templates? 

1. Propose-Accept pattern
2. Singleton pattern
3. Factory pattern 
4. Delegation pattern

Answer: 3

Q2. Which of the following patterns helps in creating a bilateral workflow between two parties?

1. Propose-Accept pattern
2. Singleton pattern
3. Factory pattern 
4. Delegation pattern

Answer: 1


Q3. Alice wants to create an account in a bank. She submits a request to the bank. As per the Propose-Accept pattern, if the bank accepts her request, which of the following steps should be carried out?

1. The request is converted into an account
2. The request is archived and an account is created
3. The request is archived, an account is created, but put on hold for Alice to check and accept the account
4. Alice submits another request with the Cid of the accepted request to create an account


Answer: 2

Q4. What is the purpose of 'viewtype' field in an interface? 

1. It represents data that will be 'viewable' from all the contracts created from the templates that have implemented the interface
2. It represents the list of parties that can view the contract data 
3. It represents the list of controllers that automatically get the ability to 'view' the contract related actions
4. None of the above

Answer: 1

Q5. What is an interface?

1. It is a special template that provides the API to interface with DARs in an application
2. It is an abstract type with abstract methods that need to be implemeted by the templates that instantiate the interface
3. It is a Java library class that needs to be imported into the template to use Daml Finance library
4. Templates are called interfaces in Daml Finance library. 

Answer: 2

Q6. Which of the following statements is FALSE about interfaces?

1. Interfaces help decouple the client application from Daml templates by creating a layer of abstraction.
2. Interfaces are instantiated inside a template
3. One interface can be implement by many templates
4. One template can implement more than one interface
5. None of the above

Answer: 5

Q7. What is the difference between a method and a function in an interface ?

1. There is no difference. 
2. Methods are converted into functions of the same name by making the interface map to the method signature
3. Functions are convereted into methods of the same name by making the interface map to the function signature
4. Methods are defined in interface and functions are defined in templates

Answer: 2

Q8. There is an interface named IMedia which is implemented by a template named Song. The interface has a method and a choice defined as below:

```
data MediaView = MediaView
    with
        player: Party
        title: Text
        status: Bool
Interface IMedia where
    viewtype MediaView

    play: Party -> Update (ContractId IMedia)

    choice Play: ContractId IMedia 
    ... and so on
```

The Song template implements the interface as partially shown below:  
```
template Song 
    with
        player: Party 
        title: Text
        status: Bool
    where 
        signatory player 

        interface instance IMedia for Song where 
            view = MediaView 
                player
                title

            play player = do 
                songCid <- create this with 
                    status = true

                return ()

```

Given this partial code, what should be written inside the parentheses next to return at the end?

1. return ()
2. return (songCid)
3. return (toInterfaceContractId songCid)
4. return (toInterfaceContractId IMedia)

Answer: 3



Q9. An interface has abstract method signatures. Where are these methods used? 

1. In other methods in the interface
2. In the choices defined in the interface
3. In the functions defined in the interface
4. These methods are not used in the interface 

Answer: 2


Q10. When implementing an interface, what must be done in a template?

1. Create an instance of the interface
2. Provide values for the 'view' record of the interface
3. Implement the choices and methods
4. All of the above




Answer: 4