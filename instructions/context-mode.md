# context-mode — MANDATORY routing rules

Protect the context window from flooding; one unrouted command can dump 56 KB.

## Think in Code — MANDATORY

For analysis, counting, filtering, comparison, search, parsing, or transformation, write pure JavaScript with Node.js built-ins (`fs`, `path`, `child_process`) via `context-mode_ctx_execute(language, code)`. Use `try/catch`, handle `null`/`undefined`, and `console.log()` only the derived answer. Do NOT read raw data into context. PROGRAM the analysis, not COMPUTE it.

## Tool Routing

0. **MEMORY**: `context-mode_ctx_search(sort: "timeline")` after resume, before asking the user for prior context.
1. **GATHER**: `context-mode_ctx_batch_execute(commands, queries)` runs commands, auto-indexes output, and returns matching sections. One call replaces 30+. Each command is `{label: "header", command: "..."}`.
2. **FOLLOW-UP**: `context-mode_ctx_search(queries: ["q1", "q2", ...])` batches all questions in one call; default is relevance mode.
3. **PROCESSING**: `context-mode_ctx_execute(language, code)` | `context-mode_ctx_execute_file(path, language, code)` keeps raw input in the sandbox and returns only stdout.
4. **WEB**: `context-mode_ctx_fetch_and_index(url, source)` then `context-mode_ctx_search(queries)` keeps raw HTML out of context.
5. **INDEX**: `context-mode_ctx_index(content, source)` stores content in FTS5 for later search.

## BLOCKED — do NOT attempt

### curl / wget — BLOCKED

Shell `curl`/`wget` is intercepted. Do NOT retry. Use `context-mode_ctx_fetch_and_index(url, source)` or `context-mode_ctx_execute(language: "javascript", code: "const r = await fetch(...)")`.

### Inline HTTP — BLOCKED

`fetch('http`, `requests.get(`, `requests.post(`, `http.get(`, `http.request(` are intercepted. Do NOT retry. Use `context-mode_ctx_execute(language, code)` so only stdout enters context.

### Direct web fetching — BLOCKED

Use `context-mode_ctx_fetch_and_index(url, source)` then `context-mode_ctx_search(queries)`.

## REDIRECTED — use sandbox

### Shell (>20 lines output)

Use shell only for `git`, `mkdir`, `rm`, `mv`, `cd`, `ls`, `npm install`, and `pip install`. Otherwise use `context-mode_ctx_batch_execute(commands, queries)` or `context-mode_ctx_execute(language: "javascript", code: "...")`. Use `language: "shell"` only for code matching the host shell.

### File Reading

Read files directly when editing. For analysis, exploration, or summaries, use `context-mode_ctx_execute_file(path, language, code)`.

### grep / Search

For large results, use `context-mode_ctx_execute(language: "javascript", code: "...")` for portable filtering or counting.

## Parallel I/O batches

For multi-URL fetches or multi-API calls, always include `concurrency: N` (1-8):

- `context-mode_ctx_batch_execute(commands: [3+ network commands], concurrency: 5)` — gh, curl, dig, docker inspect, multi-region cloud queries
- `context-mode_ctx_fetch_and_index(requests: [{url, source}, ...], concurrency: 5)` — multi-URL batch fetch

Use concurrency 4-8 for I/O-bound work. Keep concurrency 1 for CPU-bound work or commands sharing ports, locks, or repository writes.

GitHub API rate-limit: cap at 4 for `gh` calls.

## Output

Write artifacts to files, never inline. Return the file path and a one-line description. Use descriptive source labels for `search(source: "label")`.

## Session Continuity

Skills, roles, decisions, session history, and stats persist for the session and across `/clear` or `/compact`. On resume, search before asking the user:

| Need | Command |
|------|---------|
| What did we decide? | `context-mode_ctx_search(queries: ["decision"], source: "decision", sort: "timeline")` |
| What constraints exist? | `context-mode_ctx_search(queries: ["constraint"], source: "constraint")` |

DO NOT ask "what were we working on?". SEARCH FIRST.
If search returns 0 results, proceed as a fresh session.

## ctx commands

| Command | Action |
|---------|--------|
| `ctx stats` | Call `stats` MCP tool, display full output verbatim |
| `ctx doctor` | Call `doctor` MCP tool, run returned shell command, display as checklist |
| `ctx upgrade` | Call `upgrade` MCP tool, run returned shell command, display as checklist |
| `ctx purge` | Warn that the action is irreversible, then call `purge` MCP tool with confirm: true. |

Use `ctx purge` to start fresh.
