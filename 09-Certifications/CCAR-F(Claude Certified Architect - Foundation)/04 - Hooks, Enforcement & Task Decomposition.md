
### What are Hooks ?
Hooks are code interceptors that runs at specific points in the agentic loop. The model cannot override or ignore them because they are not instructions.

**There are two ways to use this hooks**
1. PreToolUse -> Before Tool executes.
		Primary Uses -> Blocks calls, check prerequisites, enforce policies
		
2. PostToolUse -> After tool returns, Before model sees result
		Primary uses -> Normalize data, convert formats, embed context.

*PostToolUse* : Data Normalization -
The goal is to give the model clean, unambiguous, context-rich data, It removes interpretation surface area, the model cannot misread what it never sees raw.

*PreToolUse* : Blocking Policy Violations -
The Action - PreToolUse runs before execution, it can block calls based on parameter or session state.

The Consequence - The tool call does Not execute. A structured error is returned.

Model Impact - The model receives context explaining the block, so it knows to escalate rather than retry blindly with different parameters.

#### Prerequisite Gates

This is a PreToolUse hook that tracks session state, They block downstream tools until an upstream tool has successfully completed and set a verified flag.

### Task Decomposition 


#### Prompt Chaining
Predictable, multi-aspect analysis

Steps defined -> Upfront, before execution
Control level -> High deterministic pipeline
Best for -> Code Reviews, audits, batch reports
Example -> CI/CD review pipeline

Do not use prompt chaining for open-ended tasks. The pre-defined steps become wrong the exact moment actual findings diverge from initial assumptions.

#### Dynamic Decomposition
Steps emerge from discoveries

Steps defined -> Emergently from findings
Control level -> Lower-model navigates
Best for -> Bug investigation, code-base exploration
Example -> Monolith -to- Microservice migration.




