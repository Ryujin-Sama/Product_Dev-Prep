# 🤖 Agentic Loop & stop_reason

## 💳 Setup & Initialization

1. Create a credit account in Claude, add **$5**, and configure the API key locally (`.env`).
2. Create a simple Python file to echo using Claude's **Haiku model** (cheap and uses fewer tokens).
3. *Note: Python files will be committed to Git.*

---

## 🎭 The 3 Roles

Each message or prompt associated with any conversation must be tied to a specific role:

* **`1. System Role`** $\rightarrow$ Instructions given to the AI tool to define its persona, constraints, or behavioral context (something we ask the AI tool to act as).
* **`2. User Role`** $\rightarrow$ The end user asking questions and stuff (whatever we type on the AI tools using GUI is considered a User role).
* **`3. Assistant Role`** $\rightarrow$ The AI tool itself responding to prompts.

---

## 💬 Chat vs. Agents

### Standard Chat
$$\text{User sends a message} \longrightarrow \text{Claude responds} \longrightarrow \text{Done}$$

### Agentic Loop
$$\text{User sends a message} \longrightarrow \text{Claude reasons and acts} \longrightarrow \text{Code executes tool} \longrightarrow \text{Claude gets info and reasons again} \longrightarrow \text{Goes back to Step 1}$$

> **Key Difference:** An agent acts, observes, and decides **autonomously**. This loop repeats until complete.

---

## 🔄 The Agentic Loop Lifecycle

```text
[Sends Request] ---> Calls API with messages array + tools
       │
[Inspect stop_reason] ---> Check if tool_use or end_turn
       │
       ├──> If 'tool_use': Execute (Run local tool) ---> Append (Add results to history) ---> Loop to Step 1
       │
       └──> If 'end_turn': Exit                     ---> Return the final answer
```

> An agent will pause its execution for a `stop_reason`. The `stop_reason` is of two types: `tool_use` and `end_turn`. On `end_turn` it returns the answer, and on `tool_use` it will continue the execution.

---

## 📜 Why the Messages Array Matters

* The Claude API is **100% Stateless**, and every API call must include the **complete conversation history**.
* *Note: The Architect is responsible for state.*
* Missing tools $\rightarrow$ **Hallucinations**
* Mismatched IDs $\rightarrow$ **Broken loops**

### How the Message Array Grows Across Iterations

* **State 1:** `User Msg`
* **State 2:** `User Msg` $\rightarrow$ `(Assistant: tool_use)`
* **State 3:** `User Msg` $\rightarrow$ `(Assistant: tool_use)` $\rightarrow$ `User: tool_result`
* **State 4:** `User Msg` $\rightarrow$ `(Assistant: tool_use)` $\rightarrow$ `User: tool_result` $\rightarrow$ `(Assistant: end_turn)`

> **Rule 1:** Append **BOTH** the tool call AND the tool result.  
> **Rule 2:** The `tool_use_id` **MUST** perfectly match between them.

---

## 🛑 The Two `stop_reason` Values You Need to Know

| Property | `tool_use` | `end_turn` |
| :--- | :--- | :--- |
| **Status** | Claude wants to act | Claude has finished |
| **Content** | Contains tool blocks | Contains text blocks |
| **Action** | Execute tool, append result, loop again | Break loop, extract text |
| **Rule** | **Never return this to the user** | **This is your final answer** |

---

## 🧠 Why the Model Drives Tool Selection

### 🤖 Model-Driven (Agentic)
* Claude reads the full conversation context.
* Decides which tool to call next based on reasoning.
* Can call tools in any order, skip unnecessary ones.
* **Intelligence lives in the model.**

### ⚙️ Pre-Configured (Non-Agentic)
* Hardcoded `if/then` configured logic.
* Rigid, fixed sequence of tool execution.
* Fragile when novel situations or requirements change.
* **Intelligence lives in the code.**

---

## ⚡ CHEAT SHEET for this Note

1. ***`end_turn`*** is the only valid loop exit.
2. ***`tool_use`*** means execute and continue.
3. Always append **assistant** first, then **user**.
4. ***`tool_use_id`*** must perfectly match.
5. The API is **stateless**, send full history.
