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
# The Banking System

## Context

Suppose a banking system is modeled with key entities such as banks, branches, clients, managers, and persons. These entities interact with one another to form a structured, hierarchical organization.

* Each bank is composed of multiple branches. These branches can be spread across different locations, creating a nationwide network of services.

* A branch serves as the operational unit of a bank. It interacts with clients who are individuals or companies. Each branch is associated with exactly one bank and is managed by a single manager.

* Gender is defined using an enumeration, with possible values being male, female, or other.

Relationships

* Banks have a composite relationship with their branches, indicating that a branch cannot exist without being part of a bank.

* Branches have composite relationships with their clients and managers. This ensures that the existence of these entities is tied to the branch.
...
```
And the output is in a json format
```json
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
}
...
```
### Refine
- The dataset used to be too long for llm to understand and memorize. So we only extract the important and unique elements, while ensuring that no contradictions.
## 2nd Agent Modelling
### Prompt

#### System
```
You are an expert in Sysml Modeling, you should create a sysml block difinition diagram use XML for the context.
```
#### Few-shot
It's a framework developed in three parts: question, COT(Chain of Thought) and Answer. 
```
human:
# The Banking System

## Context

Suppose a banking system is modeled with key entities such as banks, branches, clients, managers, and persons. These entities interact with one another to form a structured, hierarchical organization.

* Each bank is composed of multiple branches. These branches can be spread across different locations, creating a nationwide network of services.

* A branch serves as the operational unit of a bank. It interacts with clients who are individuals or companies. Each branch is associated with exactly one bank and is managed by a single manager.

* Gender is defined using an enumeration, with possible values being male, female, or other.

Relationships

* Banks have a composite relationship with their branches, indicating that a branch cannot exist without being part of a bank.

* Branches have composite relationships with their clients and managers. This ensures that the existence of these entities is tied to the branch.
...
```
The COT is here. I decompose the elements in the XML to make the llm comprehend the relationship between each element and it's description and what format does each element show in. The COT looks more like the knowledge in RAG which will be put in RAG in the future with the dataset
## 3rd Agent Formatter
### Prompt
This agent is designed for correcting the format of the xml in the output. We use a loop to check the format of xml and let the llm to re-output. After some tests, we conclude some 
```

```