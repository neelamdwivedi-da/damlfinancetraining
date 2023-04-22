# Setup Daml Finance

In this lesson, you will setup the dev environment to start used the Daml Finance library.

--------

On Unix-based systems, execute the following on the terminal:

```
daml new df-training-examples --template=quickstart-finance
cd df-training-examples
./get-dependencies.sh
daml studio
```

Once the IDE opens up, create two folders: Workflow and Scripts where we will write up code for the upcoming lessons. 

Let us now start learning about our first workflow, i.e. the [Transfer workflow](TransferWorkflow.md)