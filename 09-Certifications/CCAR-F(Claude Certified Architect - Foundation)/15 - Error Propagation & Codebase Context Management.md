
### Vague Errors Paralyze Coordinators

Generic errors are like a contractor saying things didn't work out. The coordinator needs a detailed decision brief to recover the pipeline.

### Building a Recovery-Ready Error Response

#### Error Category:
What kind of failure was this? This helps to drives the recovery strategy

#### is Retryable:
The single most important Boolean. True = retry and false = structural problem.

#### Attempted Query:
What was the subagent trying to do? This identifies the research gap.

#### Partial Results + alternatives
What data was saved and what else can we try? This enables graceful degradation

### Context Degradation

After several tool calls, the context window stuffs with raw outputs. The agent quietly forgets early findings, re-read files and misses cross-file connections.

#### Persistence Beyond the Window

The Scratchpad acts as the external 'case file'. Key findings are written continuously to disk, bypassing the context window limits entirely.
We need to mention in the claude.md file to include scratchpad.md file to write the new findings on it continuously.

#### /compact

Scratchpad preserve facts. /compact preserves momentum. Manifests preserve hours of compute against total failure.
Trade fine grained detail for context headroom, and keeps the highlights.

#### Manifest file

Structured state persistence at a fixed location for total system restarts, meaning it's a checkpoint if something crashes.