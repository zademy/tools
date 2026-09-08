# Graft — Repository Context for OpenCode

Use **Graft** as the repository-context and structural navigation layer when it is available.

## Core Rule

Before broad repository exploration for a coding task, use Graft to identify the relevant subsystem, files, symbols, relationships, and likely blast radius.

Use Graft to **narrow** investigation, not to replace verification. The actual source code, tests, configuration, and runtime behavior remain authoritative.

Do not use Graft mechanically when the task is already localized to an exact file or code range, when the relevant file type is not indexed, or when Graft is unavailable.

## Preferred Tool Order

When Graft MCP tools are available, prefer them over shell commands:

| Need | Graft MCP tool |
|---|---|
| Understand an unfamiliar repository | `graft_repo_map` |
| Find where behavior is implemented | `graft_find_code` |
| Inspect a file's public/API surface | `graft_file_api` |
| Find callers, dependencies, or blast radius | `graft_trace_calls` |
| Find every occurrence of a pattern | `graft_find_all` |
| Check whether the graph is current | `graft_check_freshness` |

If MCP tools are unavailable but the Graft CLI exists, use the equivalent commands:

- `graft map`
- `graft ask "<question>" --source`
- `graft skeleton <file>`
- `graft callers <symbol>`
- `graft grep "<pattern>"`
- `graft check`

For diff-oriented impact analysis, use `graft blast` when useful.

## Working Strategy

1. **Orient only when necessary**
   - For an unfamiliar repository or subsystem, start with `graft_repo_map` or `graft map`.
   - Do not repeatedly regenerate a repository map once the relevant area is known.

2. **Ask before broad searching**
   - Use `graft_find_code` or `graft ask "<question>" --source` for implementation, architecture, behavior, and change-location questions.
   - Prefer queries containing concrete identifiers already known: class names, function names, errors, endpoints, configuration keys, filenames, or domain terms.
   - Reuse previous Graft results instead of rediscovering the same context.

3. **Inspect narrowly**
   - Use `graft_file_api` / `graft skeleton` before reading an entire large file when only its structure or signatures are needed.
   - Open source files only for details that Graft does not provide or that must be verified before editing.
   - Prefer the exact file and code range returned by Graft over whole-file reads.

4. **Understand dependencies before risky changes**
   - Before changing shared functions, services, interfaces, APIs, or heavily reused code, use `graft_trace_calls` / `graft callers`.
   - Use outbound tracing when the question is what a symbol depends on.
   - Use deeper traversal only when transitive impact matters.

5. **Use exhaustive search for exhaustive questions**
   - For requests such as “every occurrence”, “all references”, or exact pattern discovery, use `graft_find_all` / `graft grep`.
   - Fall back to normal repository search (`rg`, `grep`, or OpenCode search tools) for unindexed files, unsupported languages, generated assets, documentation, configuration, or when Graft results are incomplete.

6. **Edit the real source**
   - Never implement product changes inside `graft/`.
   - Treat `graft/` as generated repository context/cache, not as application source.
   - Do not commit generated Graft cache unless the repository explicitly requires it.

7. **Verify after editing**
   - Run the repository's relevant tests, linters, builds, or static checks.
   - For cross-cutting changes, re-check callers or use `graft blast` when it helps validate impact.
   - If Graft output conflicts with current source code, trust and verify the source code.

## Freshness and Rebuilding

Normal Graft queries refresh the structural graph when the working tree changes, so **do not run `graft build` before every query**.

Use `graft_check_freshness` / `graft check` when freshness is uncertain.

After large structural changes, `graft build` may be used to refresh the local graph.

Do **not** run `graft build --deep` automatically. It uses the configured LLM provider and may incur external API usage or cost. Use it only when the user explicitly requests it or the repository workflow clearly requires it.

## Setup Safety

Do not install, initialize, upgrade, uninstall, or reconfigure Graft as part of an unrelated coding task.

In particular, do not run commands such as:

- `graft init`
- `graft uninstall`
- `graft upgrade`
- package installation commands for Graft

unless the user explicitly asks for setup or maintenance.

If Graft is unavailable, continue with normal OpenCode repository tools rather than blocking the task.

## Principle

**Graft first for repository understanding; source code for truth; normal tools for verification and execution.**

Use the smallest amount of repository context necessary to make a correct change.
