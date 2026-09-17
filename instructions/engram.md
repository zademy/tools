# Engram — Durable Project Memory

## Role
Engram is the **canonical persistent memory** for engineering knowledge that must survive sessions, resets, or compaction.

Store durable decisions, architecture, non-obvious fixes, constraints, conventions, important discoveries, environment requirements, and meaningful project state. Do not use Engram as a transcript, command log, raw-output store, or temporary scratchpad.

## Source Priority
Resolve conflicts in this order:
1. Current explicit user instructions.
2. Current repository/configuration.
3. Current runtime, build, and test evidence.
4. Current official documentation when relevant.
5. Engram memory.
6. Agent assumptions.

## Retrieval
For meaningful repository work:
1. `mem_current_project`
2. `mem_context`
3. `mem_search` only for specific gaps
4. `mem_get_observation` when full detail is needed
5. `mem_timeline` only when chronology matters

After `/clear`, `/compact`, or conversational loss, **Engram is the first source for project continuity**. context-mode may retain working data, but it is not the authoritative project memory.

## Saving
Use `mem_save` only at meaningful checkpoints. Prefer short searchable titles and stable topic keys. Update evolving knowledge instead of creating contradictory duplicates.

Never persist credentials, tokens, keys, cookies, or secret values.

Before closing a meaningful session, save a concise session summary covering goal, constraints, discoveries, completed work, next steps, and relevant files.
