# Comprehensive Guide to CLAUDE.md & Rules Setup

`CLAUDE.md` serves as persistent context loaded before every session. Claude reads this briefing document before your first message, essentially functioning as project-level intelligence.

---

## 🏗️ 1. The Three-Level Hierarchy

| Level | File Path | Scope & Purpose |
| :--- | :--- | :--- |
| **User** | `~/.claude/CLAUDE.md` | **Global:** Applies across all your local projects. |
| **Project** | `.claude/CLAUDE.md` | **Repository-wide:** Shared with all project collaborators. |
| **Directory** | `any-subdir/CLAUDE.md` | **Scoped:** Applies only to the specific directory and its subdirectories. |

> **Pro Tip:** Keep your root `CLAUDE.md` clean. Use `@import` statements to pull in external `.md` files instead of cramming all configuration into a single file.

---

## 🎯 2. The `.claude/rules` Directory

### Path-Specific Execution Engine

```
Claude Scans Repo ──► Decision Gate: Does `paths:` match current file?
                        ├── YES ──► Inject YAML Rules
                        └── NO  ──► Bypass Rule
```

### Glob Pattern Reference Table

| Syntax | Target Scope |
| :--- | :--- |
| `terraform/**/*` | All files under `terraform/` at any depth |
| `**/*.test.tsx` | Any `.test.tsx` file anywhere in the project |
| `**/*.spec.ts` | Any `.spec.ts` file anywhere in the project |
| `src/api/*.ts` | `.ts` files directly in `src/api/` (excludes deeper subdirectories) |
| `**/*.{ts,tsx}` | All `.ts` and `.tsx` files anywhere in the project |

---

## 🛠️ 3. Diagnostic Commands

* **`/memory`**: Displays all active memory files loaded into the current session. Use this diagnostic tool whenever Claude isn't following your instructions as expected.
