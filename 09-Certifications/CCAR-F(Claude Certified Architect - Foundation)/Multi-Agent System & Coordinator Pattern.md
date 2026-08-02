
---

## 1. System Warning: The Single-Agent Ceiling

As single-agent systems scale, they encounter structural limitations that impair reliability, speed, and accuracy:

* **Context Overflow:** Tool results accumulate rapidly. Complex tasks exhaust the context ceiling before completion, causing performance degradation and loss of critical information.
* **Sequential Bottlenecks:** Single agents execute strictly sequentially. Independent subtasks queue up sequentially, compounding overall system latency.
* **Specialization Gap:** Generic instructions yield generic results. Scoped agents with narrow, well-defined responsibilities perform measurably better.

> **The Fix:** Deploy **Multi-Agent Architectures** featuring specialized agents executing in parallel, each operating with a dedicated, scoped context window.

---

## 2. Architecture & Topology

### The Hub-and-Spoke Topology
Subagents operate in isolated contexts and are unaware of the existence of other subagents due to zero direct communication channels. Every subagent must report exclusively to the central coordinator node.

```
       [ Subagent A ]
             ^
             |
             v
 [ Subagent B ] <---> [ Central Coordinator ] <---> [ Subagent C ]
             ^
             |
             v
       [ Subagent D ]
```

* **The Routing Mandate:** All communication routes exclusively through the central coordinator.
* **The Core Rule:** Subagents **never** communicate directly with each other.

---

## 3. The Four Coordinator Responsibilities

The central coordinator node is governed by four primary duties:

1. **Task Decomposition:** Break overarching, complex requests into scoped, assignable subtasks.
2. **Delegation:** Spawn the precise subagent required for each individual piece using the `Task` tool.
3. **Result Aggregation:** Collect independent outputs from all subagents and synthesize them into a unified final result.
4. **Error Handling:** Instantly detect subagent failures and decide whether to retry, skip, or escalate.

---

## 4. Task Tool Permission Matrix

The `Task` tool is the mechanism by which a coordinator spawns subagents. Configuration rules govern tool permissions across the node hierarchy:

| Agent Role | Task Tool Access | Purpose & Constraints |
| :--- | :--- | :--- |
| **Coordinator** | **Allowed** | Must have `Task` tool access to spawn and delegate to subagents. |
| **Standard Subagent** | **Denied** | Denied to prevent uncontrolled recursive delegation loops that break the system. |
| **Hierarchical Sub-Coordinator** | **Conditional** | Allowed **only** if explicitly designed to delegate work to a deeper, tertiary layer. |

---

## 5. Blueprinting the Agent Definition

When a coordinator invokes the `Task` tool, it must pass a strict `AgentDefinition` object to successfully spawn a subagent:

* **`description`**: Defines the agent's exact purpose and role within the multi-agent system.
* **`prompt`**: The specific instructions, goal criteria, and explicit context required for execution.
* **`allowed_tools`**: An isolated list of specific tools this individual worker is permitted to access.
* **`model`**: The specific underlying model powering this individual node.

---

## 6. Execution Latency: Parallel vs. Sequential Spawning

### Parallel Execution (Correct Pattern)
* **Total Time:** $	ext{Total Time} = \max(S_1, S_2, S_3)$
* **Rule:** Emit multiple `Task` tool calls within a **single** coordinator response.

### Sequential Execution (Anti-Pattern)
* **Total Time:** $	ext{Total Time} = S_1 + S_2 + S_3$
* **Diagnostic:** If a multi-agent system runs $\sim 3	imes$ slower than expected, the coordinator is committing the sequential anti-pattern.

---

## 7. Context Isolation & Explicit Injection

### The Context Isolation Airlock
Subagents start with a **blank slate**—they possess an empty context window, zero historical memory, and inherit no context from the parent environment.

```
       [ Parent / Coordinator Environment ]
     (Contains: History, Prior Results, Ambient Context)
                         |
                         x  <--- Context Isolation Airlock (Blocked)
                         |
                [ Subagent Slate ]
```

* **Passed Through:** System prompt, `Task` tool prompt field, `allowed_tools` list, model setting.
* **Blocked:** Coordinator conversation history, prior tool outputs, ambient context.

### Bypassing the Airlock: Explicit Injection
* **The Fix:** If a subagent requires historical information to complete its task, the coordinator must actively extract it.
* **Action Required:** Explicitly inject all required data into the `prompt` field of the `Task` tool call.
* **Diagnostic:** If a subagent produces irrelevant, duplicated, or context-unaware output, the root cause is **missing explicit context injection**.

---

## 8. Orchestration Prompts: Goals over Steps

* **Anti-Pattern (Step-by-Step Procedure):** Do not dictate step-by-step procedures. Rigidly prescribed ladders are brittle and fail when unexpected variables arise.
* **Best Practice (Goals + Quality Criteria):** Outline the overarching goal and exact quality criteria. Empower specialized subagents to determine their own optimal approach using their assigned allowed tools.

---

## 9. Fixing the Pipeline: The Synthesis Node

* **The Root Cause:** When task decomposition is too narrow, subagents operate in isolated lanes without cross-domain synthesis, producing siloed outputs.
* **The Fix:** Add an explicit aggregation and synthesis step. Spawn a 5th cross-domain subagent or enforce a final coordinator pass that explicitly resolves intersections across subagent outputs.
* **Diagnostic:** Incomplete or siloed outputs indicate a **missing synthesis step in the pipeline**, not a model configuration issue.

---

## 10. Quick Reference Cheat Sheet

| Concept               | The Hard Rule                                                             | Diagnostic / Issue Resolved                                           |
| :-------------------- | :------------------------------------------------------------------------ | :-------------------------------------------------------------------- |
| **Topology**          | All communication routes strictly through the central coordinator.        | Enables absolute observability across the system.                     |
| **Task Tool**         | Coordinator requires `Task` tool; subagents are denied access by default. | Prevents uncontrolled recursive delegation loops.                     |
| **Parallel Spawning** | Emit multiple `Task` tool calls in a single coordinator response.         | Resolves $3	imes$ latency bottlenecks.                                |
| **Context Isolation** | Subagents start with blank slate and inherit zero parent context.         | Fixes context-unaware outputs via explicit prompt injection.          |
| **Siloed Outputs**    | Results from narrow lanes must be cross-analyzed before output.           | Indicates pipeline is missing an explicit synthesis/aggregation step. |