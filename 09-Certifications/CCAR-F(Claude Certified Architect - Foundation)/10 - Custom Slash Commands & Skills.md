
### What are Slash Commands ?

They are rich, context-aware instructions which can used for a user specific and a team specific 

For Project specific - Location - .claude/commands and for user specific - ~/.claude/commands
If it's available for all contributors then it's always means Project Scope 

Slash commands are a static orders, you can write the instructions and claude follows them sequentially in your main workspace.

### Introducing Skills

Skills are configurable mini-agents, you give them exact tools and they work in a isolated space without making a mess on the main session.

#### The Anatomy of SKILL.md

The YAML frontmatter controls the configuration, the markdown body contains the prompt
we give a name, context, allowed-tools, argument-hints etc.
If we give context as fork,  then claude spins up a completely isolated sub-agent which does the exploration and analysis on a separate session and don't mess up the main session


### Tool Selection Matrix


|               | CLAUDE.md                      | SKILL.md              | Slash Command                   |
| ------------- | ------------------------------ | --------------------- | ------------------------------- |
| Trigger       | Passive                        | Active                | Active                          |
| When it Loads | Every single session           | On-demand only        | On demand only                  |
| Isolation     | No                             | Yes                   | No                              |
| Best used for | Standing project rules, guides | Multi-step procedures | Static and reusable text prompt |
