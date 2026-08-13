# Agentic Workflow Architecture: Hooks & Task Decomposition

---

## ⚓ What are Hooks?

> **Definition:** Hooks are **code interceptors** that run at specific points in the agentic loop. 
> 
> 🛡️ **Execution Security:** The model *cannot* override or ignore them because they are executed as rigid code logic, not prompts or instructions.

---

### 💡 The Two Hook Lifecycle Stages

```
   [ Model Generates Tool Call ]
                 │
                 ▼
     ┌───────────────────────┐
     │  1. PreToolUse Hook   │ ──► Primary Uses: Block calls, check prerequisites, enforce policies
     └───────────────────────┘
                 │
                 ▼
          [ Tool Executes ]
                 │
                 ▼
     ┌───────────────────────┐
     │  2. PostToolUse Hook  │ ──► Primary Uses: Normalize data, convert formats, embed context
     └───────────────────────┘
                 │
                 ▼
     [ Model Receives Output ]
```

---

### 🔍 Detailed Hook Behaviors

#### 1. `PostToolUse` : Data Normalization
* **Goal:** Give the model clean, unambiguous, context-rich data.
* **Mechanism:** It removes interpretation surface area—the model cannot misread what it never sees raw.

#### 2. `PreToolUse` : Blocking Policy Violations
* **The Action:** `PreToolUse` runs before execution; it can block calls based on parameter or session state.
* **The Consequence:** The tool call **does NOT execute**. A structured error is returned.
* **Model Impact:** The model receives context explaining the block, so it knows to escalate rather than retry blindly with different parameters.

#### 3. Prerequisite Gates
* This is a `PreToolUse` hook that tracks session state.
* They block downstream tools until an upstream tool has successfully completed and set a verified flag.

---

## 🧩 Task Decomposition

Comparison between the two core strategies for task decomposition:

| Dimension | 🔗 Prompt Chaining | 🌀 Dynamic Decomposition |
| :--- | :--- | :--- |
| **Core Concept** | Predictable, multi-aspect analysis | Steps emerge from discoveries |
| **Steps Defined** | Upfront, before execution | Emergently from findings |
| **Control Level** | High, deterministic pipeline | Lower—model navigates |
| **Best For** | Code reviews, audits, batch reports | Bug investigation, codebase exploration |
| **Example** | CI/CD review pipeline | Monolith-to-Microservice migration |

> ⚠️ **Warning on Prompt Chaining:**
> Do **not** use prompt chaining for open-ended tasks. The pre-defined steps become wrong the exact moment actual findings diverge from initial assumptions.
