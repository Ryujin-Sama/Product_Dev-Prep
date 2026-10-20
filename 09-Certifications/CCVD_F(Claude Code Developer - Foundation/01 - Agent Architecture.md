
Agent Architecture is where the Agent drives the next step based on a Agentic loop

User starts a turn and runs a agentic loop, agent will run iteration to achieve a goal and once reached it will end the loop

We define a state machine in our system prompt and the agent will internally manage it's state based on the message history

If we do not define a state machine then we need to define dynamic policy

**Dynamic Policy - is a decision rule that chooses the next action at runtime based on the current context, rather than following predefined transitions**

### Workflow Architecture 

It when a developer or user defines a series of predetermined steps

Workflow architecture is a state machine
An Agent architecture can be embedded within a workflow as a agent step

Workflow architecture are too rigid to build a capable agentic loop, they are predetermined, where an agent will dynamically choose the next step, Workflows are great as automation tasks.

### Subagents Improving Task Execution

**Specialization** - One subagent can focus on research, another on coding, another on review
**Parallelism** - several subagents can work at the same time.
**Better context management** - each subagents gets only the context relevant to its job
**Reduced cognitive load** - the main agent does not have the reason about every details at once.
**Independent verification** - a reviewer subagent can catch mistakes made by another.
**Tool specialization** - different subagents can have different tools or permissions.
**Isolation** - one difficulty task can be delegated without cluttering the main agent's working context

*The Tradeoff is that subagents also add cost, latency, coordination overhead and possible communication errors.*

### Multi-Agent Architecture

There are many different kinds of multi-agent patterns:

Router - sends a task to the best agent/loop
Sequential pipeline - Agent A -> Agent B -> Agent C
Parallel / fan-out - multiple agents work independently, then results are combines.
Reviewer / critic loop - one agent produces work, another checks it and send it back for revision
Peer-to-Peer / handoff - agents directly pass control to each other.
Backboard / shared-memory - multiple agents contribute to a shared state or workspace.
Swarm / decentralized - no single central orchestrator. agents coordinate more loosely.

