# Agent
## Architecture
- multi-agent
- based on langgraph SDK, define a workflow
- mainly in three steps (maybe four in the future)
	1. extracting the **classes** with their **associations** from the natural language explanation of the work, output in **json** (may be decomposed in the future)
	2. with the json of the classes and associations, build the sysml model using XML
	3. correct the format of the XML, maybe loop multiple times (less than 10 times)
## 1st Agent Entities