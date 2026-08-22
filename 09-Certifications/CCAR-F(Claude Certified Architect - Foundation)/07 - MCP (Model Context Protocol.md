
### What is MCP

An Open protocol connecting AI to tools, data and workflows, Standardizes how applications expose capabilities and how models discover and invoke them.

Tools - Atomic operations exposed by servers, Described by name, schema & description, invoked via tools/call,

Resources - Contextual data(files, docs, etc), Discover via resources/list & read. Supports live update & subscribe.

Prompts - Reusable prompt templates list with prompts/list. Retrieve via prompts/get.

### MCP Architecture

Host -> MCP Client -> MCP server

Host - AI environment & agent logic
MCP Client -> Library implementing the protocol handles connect, initialize, call
MCP server -> Exposes tools, resources & prompt Runs remote operations.

The host calls into the MCP Client, which speaks JSON-RPC with the server, Tools, resources & prompt live on the server.


### How agents communicate with MCP

Connect - Initialize - Discover - Reason - Call - Respond

Connect & initialize to establish context, discover available tools, reason over metadata and input, then invoke a tool and consume it's response.

### Two Scopes, Two Philosophies

.mcp.json -> Lives at repo root, committed to git, shares team-wide tools
~/ .claude.json -> Lives in home directory, Absolutely never shared, Stores personal tokens/preference 

Inside .mcp.json file

Local Subprocess ->
"mcpServers" : {
	"my-local-repo" : {
		"command"	: "npx",
		"args" :("-y", "local-packages")
	}
}
Stdio Server - Claude spawns a local subprocess via standard input/output (best for local packages/tools)

Remote Network Endpoint
"mcpServers" : {
	"my-remote-tool" : {
		"url" : "https://api.internal.corp/mcp"
	}
}

HTTP/SSE servers: Claude acts as a client connecting to a remote network URL (best for hosted APIs/ cloud service)

*The above choice is driven by where the server lives and not what the server does.*

### MCP resources: The Context Shortcut

Tools (Dynamic Actions) : Executable functions based on runtime inputs(searches , write operations, lookups)  - burns huge amount of tokens

Resources(Content Catalogs) - Upfront maps of static data(docs, templates), claude navigates directly saving tokens. 

### Community vs Custom Servers

Community Server -
		Service type - Standard
		Deploy time - Deploy in minutes
		Maintenance - Community maintained
		Description - Generic

Custom Server -
		Service type - Proprietary internal systems
		Deploy time - Build from scratch
		Maintenance - You own it
		Description  - Custom Auth required


### Decision Framework

Shared ---> use .mcp.json instead of ~/. claude.json
   |
Secrets ? ---> Utilize ${VAR_NAME} expansion syntax.
   |
Static Contents? ---> Resources - map via MCP resources not tools.
   |
Standard Services ---> Community + Overrides - Deploy community server + enhanced descriptions
   |
Wrong Tool Chosen ---> Rewrite Description - Fix description first, examples second.
