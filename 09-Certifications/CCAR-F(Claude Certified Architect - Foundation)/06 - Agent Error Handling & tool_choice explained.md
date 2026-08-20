#### The Core Pattern is the isError Flag

Define isError flag in the tool_result which completely separates from the content block.

#### Four Type of Tool Failure 

| Error Category | isRetryable State | Coordinator Action             |
| -------------- | ----------------- | ------------------------------ |
| Transient      | True              | Retry with exponential backoff |
| Validation     | sometimes         | Fix input params or escalate   |
| Business       | false             | Route to alternative flow      |
| Permission     | false             | Escalate (lacks authorization) |

#### Structured Error Metadata

Giving generic string errors cannot be parsed by the coordinator to make routing decisions
eg - 'error' : 'System failed'

Give structure error metadata
eg - 'errorCategory' : 'transient',
'isRetryable' : true,
'description' : 'Database timeout',
'alternative' : '(fallback_db)'


#### Local Recovery Before Escalation

Instead of termination the whole loop for a single failure we need to design it in such a way that it should handle minor failures on it's own.

SubAgents must handle their own transient failures locally. The coordinator is only involved when local options are exhausted.

eg -> Layer 1 - Tool Executions fails  --> Layer 2 - Subagents catches error and executes local retry for transient issues. --> Layer 3 - Local retry exhausted. Escalate structed error to coordinator


#### Tool Overload Problem

Providing a model with too many tools actively degrades its selection reliability. Bloated toolsets result in wrong tools firing and hard-to-debug logic errors.
Always it should be role-based distribution.

#### Controlling Execution via tool_choice

This has three types of tags.

| Auto          | Any                                                                                               | Tool                   |
| ------------- | ------------------------------------------------------------------------------------------------- | ---------------------- |
| Model decides | Must call at least one tool - used<br>when requirement is guaranteed structured output generation | Must call a named tool |
#### Synthesis Architecture

tool_choice : 'any'  -> allowedTools Scoping (Guarantees correct selection)  -> isError : true (Catching signal failures)  -> Local Recovery (Subagents retries) -> Coordinator escalation