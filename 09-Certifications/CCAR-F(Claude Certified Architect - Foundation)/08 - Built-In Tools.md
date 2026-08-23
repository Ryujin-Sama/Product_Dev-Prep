
Claude code's built-in tools are pre-loaded and ready. Your job is to know exactly which one to use for each situation.
No MCP setup or no API key is required
The wrong choice doesn't break things, just wastes token.

The 6 Built-in Tools are - 
Grep, Glob, read, write, edit, bash

### Grep : The Content Detective

Grep opens files and searches inside them, it searches in the entire code-base.

### Glob: The File Path Finder

Glob looks at file labels - it never opens files, grep reads the docs, glob read folder labels.

### Read : Load it all in

Loads the complete contents of a specific file into context, reaches for it after grep and glob identifies the target, this can burn a lot of token if done blindly.

### Write : Create and Overwrite

Replaces an entire file in one atomic operation, 
Use case 1: Creating a brand-new file from scratch.
Use case 2: Cleanly replacing a file during a massive rewrite case,
It's the ultimate fallback when edit fails.

### Edit : Surgical Precision

Makes targeted changes without rewriting the whole files, Tokens and reliability, include 3-5 surrounding lines for better context.
Edit requires an unique anchor text when it fails, you need the fallback pattern.

### When Edit Fails : The Read + Write Fallback

Edit fails -> old_string is not unique -> triggers fallback

Read (loads full context) -> Reason (Agent modifies code in mem) -> Write(Overwrite with updated file)

Tradeoff -> More token usage but 100% reliable with zero failure mode.

### The Incremental Exploration Pattern : Grep - Read - Trace

Smart Approach -> 
Grep -> read target -> Trace dependency -> Grep

Incremental exploration preserves token window and reasoning quality
