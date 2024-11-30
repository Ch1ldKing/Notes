# Agent
## Architecture
- multi-agent
- based on langgraph SDK, define a workflow
- mainly in three steps (maybe four in the future)
	1. extracting the **classes** with their **associations** from the natural language explanation of the work, output in **json** (may be decomposed in the future)
	2. with the json of the classes and associations, build the sysml model using XML
	3. correct the format of the XML, maybe loop multiple times (less than 10 times)
## 1st Agent Entities
### Prompt
#### System:
```
You are an expert in Sysml Modeling and Model-based Systems Engineering. You should extract the entities and their attributes and associations from the given context that are relevant to the Sysml Block Definition Diagram in a formatted json.
```
#### Few-shot:
It seems like a framework of Q&A, Questions are from our dataset, which is the natural language description of the task to be modeled.
```
human:
# The Banks

## Context

Suppose the financial system of a country is composed of banks.

* Each bank has branches, which may be spread throughout the country, but there may not be more than three branches of the same bank in the same city.

* The structure of a bank's branches is hierarchical, so that a branch may have subordinate branches (of the same bank), either in the same city or in other cities.

* The customers of a branch can be either the individuals or companies that have an account at the branch. Each customer may have one or more accounts in any of the branches of a bank. Each account can have only one account holder, the customer who opens the account.

* Each branch owns an account, in which it stores its assets.

* Each account has a balance, which must be positive, unless it is a "credit" account.

* Credit accounts allow their balance to be negative, with a limit that is established when the account is opened.

* A customer may request a change in this limit from the branch where the account is held, and the branch must request authorization from the branch directly responsible for it (except the head office, at the root of the hierarchy, which can make decisions directly). Changes in the credit limit will be authorized as long as the new limit is lower than the previous one, or if it is only 10% higher than the one the account already had, and the current balance exceeds the new limit (e.g., if the credit limit is 1,000 Euros and you request to increase it to 1,005 Euros with a balance of 1,100 euros on the account, the branch will authorize the change).

* Customer can perform transactions with their accounts (request balance, deposit or withdraw money, and transfer money to another account). hey can also open accounts at any branch. When opening an account, the initial balance is 0. If the account is a credit account, the initial limit is 10 euros.

* The accounts have a maintenance fee of 1% per year. This means that, once a year (on January 1), each branch deducts 1% from the current balance of all its accounts. If the balance is 0 and the account is not a credit account, no money is deducted. In case of credit accounts, if the resulting balance is negative the branch will deduct 1% of the account's credit limit instead. The deducted money from the becomes the property of the branch and is stored in your account.

* All customers can transfer money from any of the accounts the own, to any other account -- no matter who the owner is, or the bank the destination account belongs to. Transfers between accounts of different banks have a 2% commission, i.e., if you transfer 1,000 euros, then 980 euros are deposited in the destination account. That 2% of the money becomes the property of the origin and destination branches (1% for each). Transfers between accounts of the same bank are free of charge.

* Companies with a balance of more than 1 million euros in one of their accounts are considered VIP by that bank and have a number of advantages. Firstly, in a transfer between banks, the part corresponding to the account of origin or destination that is of that bank is not taxed with the corresponding 1%. Secondly, these accounts do not pay an annual maintenance fee. Only companies, and not individuals, can be considered VIP.

* Finally, accounts whose holders are bank branches pay neither annual fee nor transfer commission in any bank.
```
```
ai:
{

"classes": [

{

"name": "Bank",

"attributes": [

],

"associations": [

{

"name":"BankBranches",

"type": "composite",

"target": "Branch",

"multiplicity": "*"

}

]

},

{

"name": "Branch",

"attributes": [

],

"associations": [

{

"name":"Clientship",

"type": "composite",

"target": "Client",

"multiplicity": "*"

},

{

"name":"Management",

"type": "composite",

"target":"Manager",

"multiplicity": "1"

},

{

"name":"BankBranches",

"type": "association",

"target": "Bank",

"multiplicity": "1"

}

]

},

{

"name":"Client",

"attributes":[

],

"associations":[

{

"name":"Clientship",

"type":"association",

"target":"Branch",

"multiplicity":"1"

},

{

"name":"ClientRole",

"type":"association",

"target":"Person",

"multiplicity":"1"

}

]

},

{

"name":"Manager",

"attributes":[],

"associations":[

{

"name":"Management",

"type":"association",

"target":"Branch",

"multiplicity":"1"

},

{

"name":"ManagerRole",

"type":"association",

"target":"Person",

"multiplicity":"1"

}

]

},

{

"name":"Person",

"attributes":[

{

"name":"age",

"type":"Integer"

},

{

"name":"name",

"type":"String"

},

{

"name":"gender",

"type":"Gender"

}

],

"associations":[

{

"name":"ClientRole",

"type":"association",

"target":"Client",

"multiplicity":"0..1"

},

{

"name": "ManagerRole",

"type": "association",

"target": "Manager",

"multiplicity": "0..1"

}

]

}

],

"enumerations":[

{

"name":"Gender",

"literals":[

{"name":"male"},

{"name":"female"},

{"name":"other"}

]

}

]

}
```