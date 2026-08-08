# SubAgent Context & Session Management Architecture

---

## 1. SubAgent Context

### 1.1 Your SubAgent Wakes Up Blank
Subagents do not inherit the coordinator's history, prior tool results, previously discovered findings, or any intermediate state.

> **The Failure Pattern:**  
> Sending a vague instruction like *"synthesize the previous findings"* guarantees hallucinated or generic output because the subagent has zero access to those findings. Every subagent spawned via the tool task starts with an entirely empty context.

---

### 1.2 Why Prose Handoffs Fail

* **Attribution Loss:** Source attribution is silently dropped during summarization.
* **Architectural Rule:** Summarization naturally destroys metadata unless the output schema explicitly forbids it. Mixing findings as plain prose destroys the ability to route or verify them programmatically downstream.

#### Fix for the Above Problem

##### A. Preserving Provenance: Claim-Source Mappings
* **Core Mechanism:** Every finding is stored as a specific claim object permanently linked to a source object via a shared ID.
* **Exam-Critical Rule:** Every synthesis step in the pipeline **MUST** output both arrays intact. If the synthesis agent consumes structured data but outputs prose, the provenance chain is broken.

##### B. Resolving Reality Clashes
* We shouldn't let a subagent silently resolve conflict; always annotate it and send it to the coordinator to resolve the conflict.
* Add a **conflict object** with both claims explicitly annotated.
* Mark the resolution state as `"unresolved"`.
* Escalate the conflict back to the coordinator to make the final decision.

---

## 2. Session Management

### 2.1 Managing State Across Time
Claude loses context of the problems sometimes, and in order to resolve that we need three core patterns for session continuity:

1. **Named Session Resumption**  
   We need to start the session with a name, e.g., `claude --session-name ""` and to resume the same session use `claude --resume ""`, and in case to resume the latest session then can run with `claude --resume`.  
   *Rule:* Never resume the session without explicitly stating what were the changes.

2. **Forked Session**  
   We give a baseline session and start a session from there with a different approach. For this we can have $n$ number of approaches and branches, but a baseline session is needed to create a fork session.

3. **Fresh Session**  
   We start a fresh session when it's impossible to work and it's pretty messed up. We give a structured summary to a new session to begin.  
   Basically, when tool results are fundamentally stale, resuming it is actively counterproductive. Start fresh, but compress prior findings into an injected summary in the initial prompt.  
   * **Critical Framing Rule:** The prompt must frame the injected summary as *'hypotheses to validate not an established fact'*. This psychological cue prevents the model from blindly accepting information that may have changed.

---

### 2.2 The Session Routing Diagnosis

| Current Context State | Code Base Volatility | Required Action |
| :--- | :--- | :--- |
| Context is valid / Shared | Zero / Low Volatility | Use `--resume` + explicitly state updates. |
| Need Parallel exploration | Shared Baseline Context | Use `fork_session()`. |
| Tool results are stale | High volatility time passed | Use fresh session + inject structured hypothesis |

---

### 2.3 Session Management Action Flowchart

```text
Are existing tool results fundamentally stale?
  │
  ├──► [Yes] ──► Start New session + inject Hypothesis
  │
  └──► [No]
         │
         ├──► Are we comparing new, parallel architectures?
         │      │
         │      ├──► [Yes] ──► Execute fork_session()
         │      │
         │      └──► [No]
         │             │
         │             └──► Continuing the exact same sequential task?
         │                    │
         │                    └──► [Yes] ──► Execute --resume and explicitly inform agent of specific file changes.
```

---

## 3. Exam Critical Facts Summary

* Context is empty by default
* Pass findings as structured objects
* Require `source_url`, `retrieved_at`, `confidence`
* Preserve claim-source mapping arrays
* Unresolved conflicts strictly escalate to coordinator
* `--resume` requires explicit manual context updates
* `fork_session()` is for parallel explorations only
* Stale tool results mandate a Fresh session
* Injected prior findings must be framed as hypotheses
