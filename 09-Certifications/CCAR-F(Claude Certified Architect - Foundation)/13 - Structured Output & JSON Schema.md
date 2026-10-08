
### Why Freeform Prompting breaks Production Pipelines

#### Markdown Wrapping

Model returns triple-backticks. The parser hits a backtick instead of a bracket and throws a fatal syntax error. Intellectually correct, but structurally incompatible.

#### Schema Drift

Complex documents cause the model to omit optional fields, change key names, or alter nesting structures. Production reliability collapses.

#### Fabrication of Required Fields

A required field is absent, To comply with the prompt, the model invents a plausible value. The parser accepts it. The data is silently corrupted.


### tool_choice

| Configuration                      | System Behavior                                         | Architecture Fit                                                                 |
| ---------------------------------- | ------------------------------------------------------- | -------------------------------------------------------------------------------- |
| auto                               | Claude decides, May call a tool, may respond with text. | Unreliable for strict extraction pipelines. Best for conversational agents.      |
| any                                | Claude must call one of the available tools             | The one-two punch. Guarantees a tool_use block in the response every single time |
| {"type" : "tool",<br>"name":"..."} | Claude must call this specific name tool                | Maximum lock-down for single-tool extraction systems.                            |

### The illusion of Perfection: Syntax vs Semantics

A strict JSON schema guarantees your output is structurally flawless. It guarantees nothing about the truth of the data. If the model extracts the 'Due Date' when you asked for the 'Invoice Date' your parser accepts it but your data is factually wrong.

### The System Boundary Matrix


|                | Syntax Errors                                                     | Semantic Errors                                                                    |
| -------------- | ----------------------------------------------------------------- | ---------------------------------------------------------------------------------- |
| What it is?    | Structural or format failure                                      | Logical or interpretation failure                                                  |
| Examples       | Missing required fields, string instead of number, malformed JSON | Reading the wrong table row, confusing similar terms, hallucinating plausible data |
| What fixes it  | Strict JSON Schemas, tool_use enforcement                         | Business logic validation, cross-field consistency checks, human review.           |
| The Root Cause | Model failing structural alignment                                | Model failing reading comprehension                                                |
