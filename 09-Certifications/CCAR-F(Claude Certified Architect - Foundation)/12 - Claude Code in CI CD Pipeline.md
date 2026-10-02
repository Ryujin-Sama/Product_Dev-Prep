
### -p Flag

It's for Non-Interactive mode, 
Behavior : One prompt -> one response -> clean exit
Works identically with short form -p or long form --print. The single valid approach for headless CI invocations.

*Expect for -p or --print there is no other flag for non interactive mode.*


### Output format

Use --output-format json with --json-schema to strictly enforce an output shape, and this way its easier to understand the problem in pipelines.


### Session Isolation

#### Same Session
Claude retains the reasoning used to write the code - Rationalization, not critique.

#### Fresh Session
No prior context -> Reads code strictly for what is actually is -> Objective analysis.


### Institutional Memory Anchor

we maintain a claude.md file which stores the rules and regulations and a clear instructions for the agents to understands. 
Fresh sessions are objective, but they know nothing about your specific project.
Automatically loaded on every CI invocation to centralize code standards, review criteria, security policies and test requirements.
Zero prompt bloat and Zero context repetition


### The Deduplication Engine

Problem : Pushing a small fix causes Claude to re-review the entire PR, posting duplicate comments.

Fix : Hand prior findings explicitly to the fresh session context.
The Result: Objective AND non-repetitive reviews. solving developers notification fatigue.


### Standardizing Test Generation

Document testing conventions in CLAUDE.md once with Naming patterns, AAA structure, mocking rules, coverage targets. Every automated test generation run inherits these rules automatically


### The Production Pipeline Arch

All components combined into a single cohesive CI workflow.

CLaude.md(Project context)  --> Prior Findings Fetch (Deduplication) --> Fresh Session Setup (Objectivity) --> -p Flag Execution (Non-Interactive mode) --> JSON Output(Machine-readable) --> Parse + Act(Block PR/ Alert)
