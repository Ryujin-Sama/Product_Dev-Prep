
### SubAgent Context
#### Your SubAgent wakes up Blank

Subagents do not inherit the coordinator's history, prior tool results, previously discovered findings or any intermediate state.

**The Failure Pattern** : Sending a vague instruction like " synthesize the pervious findings" guarantees hallucinated or generic output because the subagent has zero access to those findings. Every subagent spawned via the tool task starts with an entirely empty context.


#### Why Prose Handoffs Fails

**Attribution loss** - Source attribution is silently dropped during summarization.

**Architectural Rule** - Summarization naturally destroys metadata unless the output schema explicitly forbids it.
Mixing findings as plain prose destroys the ability to route or verify them programmatically downstream.

*Fix for the above problem*

#### Preserving Provenance: Claim-Source Mappings

**Core Mechanism** : Every finding is stored as a specific claim object permanently linked to a source object via a shared ID.

**Exam-Critical Rule** : Every synthesis step in the pipeline MUST output both arrays intact. If the synthesis agent consumes structured data but outputs prose, the provenance chain is broken.

#### Resolving Reality Clashes

We shouldn't let a subagent silently resolve conflict always annotate it and send it to the coordinator to resolve the conflict.

Add a conflict object with both claims explicitly annotated.
Mark the resolution state as "unresolved".
Escalate the conflict back to the coordinator to make the final decision.


### Session Management

#### Managing State Across Time

Claude loses context of the problems sometimes and inorder to resolve that we need to three core patterns for session continuity

**Named Session Resumption** - we need to start the session with a name eg - *claude --session-name ""* and to resume the same session use *claude --resume ""* and incase to resume the latest session then can run with *claude --resume* 
never resume the session without explicitly stating what were the changes

**Forked Session** - We give a base line session and started a session from there with an different approach, for this we can have n no of approach and branch, but a baseline session is needed to create a fork session

**Fresh Session** - We start a fresh session when it's impossible to work and it's pretty messed up, we give a Structured summary to a new session to begin
Basically when tool results are fundamentally stale, resuming it actively counterproductive. Start fresh, but compress prior findings into a injected summary in the initial prompt.

*Critical Framing Rule* - The prompt must frame the injected summary as 'hypotheses to validate not a established fact'. This psychological cue prevents the model from blindly accepting information that may have changed.

##### The Session Routing Diagnosis

Current Context State - Context is valid/Shared, Code Base Volatility - Zero/Low Volatility
Required Action - Use --resume + explicitly state updates.

Current Context State - Need Parallel exploration, Code Base Volatility - Shared Baseline Context Required Action - Use fork_session().

Current Context State -Tool results are stale, Code Base Volatility - High volatility time passed. Required Action - Use fresh session + inject structured hypothesis


##### Session Management Action Flowchart

Are existing tool results fundamentally stale ?    -> Yes   -> Start New session + inject Hypothesis
                |
                No
				|
Are we comparing new, parallel architectures ?   -> Yes   -> Execute fork_session()
				|
				No
				|
Continuing the exact same sequential task?
				|
				 Yes
				|
Execute --resume and explicitly inform agent of specific file changes.

### Exam Critical Facts Summary

- Context is empty by default
- Pass findings as structured objects
- Require source_url, retrived_at, confidence.
- Preserve claim-source mapping arrays
- Unresolved conflicts strictly escalate to coordinator
- --resume requires explicit manual context updates.
- fork_session() is for parallel explorations only
- Stale tool results mandate a Fresh session
- Injected prior findings must be framed a hypotheses.
