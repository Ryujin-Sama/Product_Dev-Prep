An open protocol connecting AI models to external tools, contextual data, and custom workflows. MCP standardizes how applications expose capabilities and how models discover and invoke them.

---

## 🛠️ Core Primitives

* **Tools (Atomic Operations)**
  * Executable functions invoked via `tools/call` with strict input schemas.
  * **Use case:** Dynamic actions, searches, database writes, and API lookups.
* **Resources (Contextual Data)**
  * Static or dynamic data sources accessed via `resources/list` and `resources/read`.
  * **Use case:** Documentation, codebases, file systems, and real-time event subscriptions.
* **Prompts (Reusable Templates)**
  * Standardized prompt shortcuts listed via `prompts/list` and retrieved via `prompts/get`.
  * **Use case:** Team-wide prompt patterns and structured workflows.

---

## 🏗️ Architecture & Communication Flow

```
+-------------------------------------------------------------+
| Host Environment (AI App / Agent Logic)                     |
|  └── MCP Client (Library handling connection & JSON-RPC)    |
+------------------------------------+------------------------+
                                     |
                          JSON-RPC 2.0 Protocol
                                     |
+------------------------------------+------------------------+
| MCP Server (Exposes Tools, Resources, and Prompts)          |
+-------------------------------------------------------------+
```

### Execution Lifecycle
1. **Connect:** Client initiates transport channel to the server.
2. **Initialize:** Capabilities and protocol versions are negotiated.
3. **Discover:** Client fetches available tools, resources, and prompts.
4. **Reason:** The AI agent analyzes available tool metadata against the task.
5. **Call:** Host triggers `tools/call` or `resources/read`.
6. **Respond:** Server executes the underlying operation and returns structured data.

---

## ⚙️ Transport Modes & Configuration

Where your server lives dictates its transport mechanism:

| Transport Type | Configuration Key | Best For |
| :--- | :--- | :--- |
| **Stdio Subprocess** | `command`, `args` | Local CLI tools, scripts, and `npx` packages |
| **HTTP / SSE** | `url` | Hosted endpoints, remote microservices, and cloud APIs |

### Scope & Storage Boundaries

* **Project Scope (`.mcp.json`)**
  * Placed at the repository root and committed to version control.
  * Shared across the entire development team.
* **User Scope (`~/.claude.json`)**
  * Stored in the user home directory and kept strictly private.
  * Used for personal authentication tokens, local path overrides, and custom preferences.

#### Example Configuration (`.mcp.json`)

```json
{
  "mcpServers": {
    "local-dev-tools": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-filesystem", "/path/to/allowed/files"]
    },
    "remote-internal-api": {
      "url": "https://api.internal.corp/mcp",
      "headers": {
        "Authorization": "Bearer ${CORPORATE_API_KEY}"
      }
    }
  }
}
```

---

## 💡 Key Architectural Distinctions

### Tools vs. Resources (Token Optimization)

* **Tools:** High flexibility, dynamic execution, higher token consumption (requires multi-step LLM reasoning loops).
* **Resources:** Pre-mapped structured context, direct navigation, lower token consumption.

### Community vs. Custom Servers

| Attribute | Community Servers | Custom Servers |
| :--- | :--- | :--- |
| **Target Use Case** | Standard SaaS, public APIs, general databases | Proprietary internal systems & legacy infra |
| **Deployment Time** | Minutes (`npx` / pre-built binaries) | Days/Weeks (Built from scratch via SDKs) |
| **Maintenance** | Open-source community | In-house engineering team |
| **Authentication** | Standard API Keys / OAuth | Custom headers, internal IAM, enterprise SSO |

---

## 🎯 Architectural Decision Framework

```
Is the capability shared team-wide?
 ├── YES ──> Save in .mcp.json at repo root
 └── NO  ──> Save in ~/.claude.json

Does the tool require sensitive API keys?
 └── YES ──> Use environment variable expansion syntax: ${ENV_VAR}

Is the target data static or read-only?
 ├── YES ──> Expose as a Resource (saves tokens)
 └── NO  ──> Expose as a Tool (dynamic operations)

Is the LLM selecting the wrong tool?
 ├── 1. Rewrite the tool `description` field with explicit guidance
 └── 2. Add detailed usage examples in parameter descriptions
