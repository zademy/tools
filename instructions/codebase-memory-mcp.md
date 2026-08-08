# Codebase Memory MCP - OpenCode

Use `codebase-memory-mcp` as the primary code-intelligence tool. Use MCP tools
directly rather than the CBM CLI when an equivalent MCP tool exists. Never
invent symbols, paths, relationships, ADRs, or query results; an empty graph
result does not prove that code is absent.

## Project Resolution

Before relying on graph evidence:

1. Run `list_projects`.
2. Match the current repository to an indexed project.
3. Run `index_status`.
4. If no index exists, run `index_repository` with the repository's absolute
   path.
5. Avoid unnecessary reindexing; auto-sync keeps indexed projects updated.

Run `delete_project` only with explicit user permission after explaining that
it removes the project and all graph data.

## Standard Change Workflow

For non-trivial work:

1. Complete project resolution above.
2. Run `get_architecture` when the repository is unfamiliar or the change spans
   modules.
3. Locate symbols with `search_graph` and text with `search_code`.
4. Trace callers, callees, or data flow with `trace_path`.
5. Read exact current code with `get_code_snippet` or native `read` before
   editing.
6. Implement the smallest correct change.
7. Run relevant formatting, linting, tests, and builds.
8. Run `detect_changes`, inspect direct and transitive impact, and review its
   risk classification and any unexpected blast radius.

## Tool Routing

### `get_architecture`

Use first for an unfamiliar repository, a multi-module change, or an overview
of languages, packages, layers, routes, hotspots, clusters, boundaries, or
existing ADRs.

### `search_graph`

Use for structured discovery of classes, functions, methods, interfaces,
routes, modules, files, name/file patterns, fan-in, fan-out, caller counts, or
degree filters. Use it first to find the qualified name required by
`get_code_snippet`. Paginate with `limit` and `offset` whenever results may be
incomplete.

### `trace_path`

Use for inbound, outbound, or bidirectional call-chain, execution-flow,
dependency, and data-flow analysis. Use depth `1` through `5`.
`trace_call_path` may be available as an alias.

### `get_graph_schema` and `query_graph`

Run `get_graph_schema` before the first custom graph query or whenever the
schema is unknown. Inspect node labels, relationship types, properties, node
and edge counts, and available relationship patterns.

Use `query_graph` only for read-only Cypher-like queries that simpler tools
cannot answer, such as advanced dependency analysis, dead-code candidates,
custom traversal, aggregations, or architectural queries. Never attempt graph
writes through `query_graph`; prefer `search_graph` or `trace_path` for simple
questions.

### `get_code_snippet`

Use for a function or method after finding its qualified name with
`search_graph`. Use native `read` when the complete file or surrounding context
is required.

### `search_code`

Use for grep-like searches in indexed files: exact strings, error messages,
configuration keys, annotations, decorators, comments, literals, and
identifiers not represented structurally. Use native search for excluded files.

### `manage_adr`

Read existing decisions before architectural changes, using query modes such as
`get` and `sections`. Create, update, or delete an ADR only when explicitly
requested or when recording an approved architectural decision. During a
same-project reindex, reads may return the previous published version; writes
remain serialized.

### `ingest_traces`

Use only with real runtime trace data to validate or improve `HTTP_CALLS`
relationships. Never fabricate traces from static assumptions.

### `detect_changes`

Before completing every non-trivial change, map the Git diff to affected
symbols, inspect direct and transitive impact, estimate blast radius, and review
the returned risk classification. Risk classification supplements, never
replaces, tests and source review.

## Native OpenCode Tools

Use native `read`, `grep`, `glob`, or equivalents for exact current content,
full-file editing context, documentation, assets, generated files,
configuration, files skipped or absent from the index, and final verification
of every modified file. Graph evidence locates and connects code; native tools
confirm the implementation. When graph evidence is missing or incomplete,
fall back to native tools.

## Completion Checks

Before completion, verify exact modified files, run relevant quality commands,
run `detect_changes`, and investigate unexpected impact. Do not claim absence
from an empty graph result or treat graph risk as proof of correctness.
