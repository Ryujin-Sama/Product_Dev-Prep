> **Core Philosophy:** Choose the right tool for the job. No MCP setup or API key required. The wrong choice won't break your build, but it will burn unnecessary context tokens.

---

## 🛠️ The 6 Built-in Tools

### 🔍 Grep : The Content Detective
* **Mechanism:** Opens files and searches inside them across the entire codebase.
* **When to use:** Finding function definitions, error strings, API endpoints, or variable usages.

### 📁 Glob : The File Path Finder
* **Mechanism:** Looks strictly at file paths and folder labels — *never* opens file contents.
* **When to use:** Locating files by name, structure, or pattern (e.g., `**/*.test.ts`, `src/components/*`).

### 📖 Read : Load It All In
* **Mechanism:** Loads the complete contents of a specific file into context.
* **When to use:** Reached after Grep/Glob pinpoint the target file.
* **Warning:** Avoid reading files blindly to prevent unnecessary token consumption.

### ✍️ Write : Create and Overwrite
* **Mechanism:** Replaces an entire file in one atomic operation.
* **Use Case 1:** Creating a brand-new file from scratch.
* **Use Case 2:** Cleanly replacing a file during a massive rewrite or as the ultimate fallback when `Edit` fails.

### ✂️ Edit : Surgical Precision
* **Mechanism:** Makes targeted changes without rewriting the whole file.
* **When to use:** Modifying specific code blocks while saving tokens and preserving context.
* **Pro Tip:** Include **3–5 surrounding lines** as anchor text to ensure unique matching.

### 💻 Bash : Command Execution
* **Mechanism:** Runs shell commands directly within the working directory.
* **When to use:** Running test suites, git operations, package installations, or build scripts.

---

## ⚙️ Core Tool Matrix

| Tool | Focus | Searches Inside Files? | Context / Token Cost | Best For |
| :--- | :--- | :---: | :---: | :--- |
| **Grep** | File Content | Yes | Low | Finding where something is defined or used |
| **Glob** | File Paths | No | Minimal | Finding where files are located |
| **Read** | Full File | N/A | Medium – High | Detailed inspection before editing |
| **Write** | Complete Overwrite | N/A | High | Creating files or full rewrites |
| **Edit** | Targeted Change | N/A | Low | Surgical updates & quick fixes |
| **Bash** | Terminal Commands | N/A | Variable | Running tests, builds, and CLI tools |

---

## 🔄 Workflow & Execution Patterns

### 1. The Incremental Exploration Pattern
*Preserves token window and maintains high reasoning quality.*

```
[Grep / Glob] ──► Identify target file
        │
        ▼
   [Read File] ──► Load target file into context
        │
        ▼
 [Trace & Grep] ──► Follow imports & dependencies recursively
```

---

### 2. Surgical Precision Edit Pattern
To ensure `Edit` succeeds on the first attempt:
1. Locate the exact block with **Read**.
2. Formulate **Edit** using **3–5 surrounding lines** as anchor context to guarantee uniqueness.

```
       ┌────────────────────────┐
       │   Attempt Edit Tool    │
       └───────────┬────────────┘
                   │
         Is old_string unique?
         ┌─────────┴─────────┐
        Yes                  No
         │                   │
         ▼                   ▼
  [Edit Success]     [Trigger Fallback]
```

---

### 3. The Read + Write Fallback Pattern
When `Edit` fails because `old_string` is non-unique or ambiguous:

```
[Edit Fails] ──► [Read Full Context] ──► [Reason & Modify in Memory] ──► [Write File Overwrite]
```

> **Trade-off:** Uses more tokens, but guarantees **100% reliability** with zero string-matching failures.
