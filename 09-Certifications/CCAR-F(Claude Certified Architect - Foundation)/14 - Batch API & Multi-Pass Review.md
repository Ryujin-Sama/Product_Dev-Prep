
### When to use Batch API ?

If a user or pipeline is not actively blocked or the task doesn't require multi-turn tool calling then we can use Batch API

Ex - Overnight code review reports, weekly security audits etc
In Short if the model needs back-and-forth conversation then we can't use Batch API.

#### Tracking Requests: the custom_id Pattern

Every request in a batch requires a unique custom_id, we should always give a meaningful tag for the custom_id, randomly generated UUID will confuse the same.

When the batch completes, the output carries the exact same ID back to you.

#### The Multi-Turn tool calling limitation

Standard API -
Prompt -> Claude use Tool 'X' -> System runs Tool 'X' -> Claude continues reasoning -> Final Output

In the above the Claude reasoning runs on loop until reaching a final output.

Batch API - 
Prompt -> Claude: "I need to use Tool X" --> End of Process.

with Batch API, there is no back and forth conversation, if the model wants to call a tool, there is nobody listening to execute it and feed the result back.


### The Self-Review Trap

**Concept** - A Claude instance reviewing it' s own output retains it's original conversation history and reasoning context.

**Result** - It is biased. It is highly unlikely to question or spot errors in the choices it has already justified to itself.

*Same session review will cause Biased review*

We must perform independent review - No prior context, Approaches code with completely fresh perspective.


### Multi-Pass Architecture 

Pass 1 : Independent instances, Parallel execution, deep focus on style, bugs and security per file.
Pass 2 : Catches dependency mismatches and integration issues across files.

*Per-file review give you depth, Cross-file review give you breadth. Each instance starts with clean context, eliminating self-review bias.*