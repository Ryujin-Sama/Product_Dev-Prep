
### What CLAUDE.md Actually is ?

It's the persistent context loaded before every session. Claude reads it before your first message, It's a briefing document, basically a project-level Claude intelligence.

#### The Three-level Hierarchy

1. User - ~/.claude/CLAUDE.md  - No Scope, All your projects
2. Project - .claude/CLAUDE.md  - This is project level and for all teammates.
3. Directory - any-subdir/CLAUDE.md - That particular directory + children.

Keep it clean with @import statement of other .md files instead of clubbing everything into the same CLAUDE.md file.

### The .claude/rules Directory

#### Path-Specific Rules with YAML Frontmatter

Claude scans repo -> Decision Gate - Does path: matches the current file? - If yes then inject YAML Rules, if no then BYPASS the rule.

#### Glob Patterns

##### Syntax Data Table


| Syntax          | What it does                                    |
| --------------- | ----------------------------------------------- |
| terraform/**/*  | All files under terraform at any depth          |
| "**/*.test.tsx" | Any .test.tsx file anywhere in the project      |
| "**/*.spec.ts"  | Any .spec.ts file anywhere in the project       |
| "scr/api/*.ts"  | .ts file directly in src/api directory only     |
| "**/*.{ts.tsx}" | Both .ts and .tsx  file anywhere in the project |
### The /memory Command

Shows every memory file currently loaded, Your debugging tool when claude isn't following instruction
