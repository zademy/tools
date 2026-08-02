# Codebase Memory MCP — OpenCode

Use `codebase-memory-mcp` as the primary code intelligence tool in OpenCode.

## General Rules

- Prefer Codebase Memory over repeated file-by-file exploration.
- Use MCP tools directly. Do not invoke the CBM CLI through the shell when
  the equivalent MCP tool is available.
- Verify exact source code before editing.
- Use OpenCode native tools when graph evidence is missing or incomplete.
- Never invent symbols, paths, relationships, ADRs, or query results.
- An empty graph result does not prove that code does not exist.

## Project Resolution

At the beginning of codebase exploration:

1. Run `list_projects`.
2. Match the current repository with an indexed project.
3. Run `index_status` before relying on graph results.
4. If the repository is not indexed, run `index_repository` using its
   absolute repository path.
5. Avoid unnecessary reindexing. Auto-sync keeps indexed projects updated.

Never run `delete_project` without explicit user permission.
Explain that it removes the project and all its graph data.

## Tool Routing

### `get_architecture`

Use first when:

- The repository is unfamiliar.
- Understanding languages, packages, layers, routes, hotspots, clusters,
  boundaries, or existing ADRs.
- Planning a change spanning multiple modules.

### `search_graph`

Use for structured symbol discovery:

- Classes, functions, methods, interfaces, routes, modules, or files.
- Name or file patterns.
- Fan-in, fan-out, caller count, or degree filtering.
- Finding the qualified name required by `get_code_snippet`.
- Paginate with `limit` and `offset` when results may be incomplete.

### `trace_path`

Use for call-chain analysis:

- Who calls a function.
- What a function calls.
- Inbound, outbound, or bidirectional traversal.
- Execution flow and dependency chains.

Use depth `1` through `5`.
`trace_call_path` may be available as an alias.

### `detect_changes`

Use before completing non-trivial changes:

- Map the current Git diff to affected symbols.
- Estimate blast radius.
- Identify direct and transitive impact.
- Review the returned risk classification.

Do not treat risk classification as a replacement for tests or source review.

### `get_graph_schema`

Run before the first custom graph query or when the schema is unknown.

Use it to inspect:

- Node labels.
- Relationship types.
- Properties.
- Node and edge counts.
- Available relationship patterns.

### `query_graph`

Use for read-only Cypher-like queries when simpler tools are insufficient:

- Advanced dependency analysis.
- Dead-code candidates.
- Custom relationship traversal.
- Aggregations and architectural queries.

Never attempt graph writes through `query_graph`.
Prefer `search_graph` or `trace_path` for simple questions.

### `get_code_snippet`

Use to retrieve a function or method by qualified name.

Find the qualified name with `search_graph` first.
Use OpenCode native `read` when the complete file or surrounding context is
required.

### `search_code`

Use for grep-like text searches inside indexed project files:

- Exact strings.
- Error messages.
- Configuration keys.
- Annotations and decorators.
- Comments, literals, and identifiers not represented structurally.

Use OpenCode native search for files excluded from the index.

### `manage_adr`

Use for Architecture Decision Records:

- Read existing decisions before architectural changes.
- Use query modes such as `get` and `sections` for inspection.
- Create, update, or delete an ADR only when explicitly requested or when
  recording an approved architectural decision.

ADR reads may return the previous published version while a same-project
reindex is running. Writes remain serialized.

### `ingest_traces`

Use only when real runtime trace data is available.

Use it to validate or improve `HTTP_CALLS` relationships.
Do not fabricate runtime traces from static assumptions.

## Native OpenCode Tools

Use OpenCode native `read`, `grep`, `glob`, or equivalent tools for:

- Exact current file contents.
- Full-file context before editing.
- Documentation, assets, generated files, and configuration.
- Files skipped, ignored, unsupported, or absent from the index.
- Final verification of every modified file.

Graph evidence locates and connects code.
Native file tools confirm the exact implementation.

## Change Workflow

For non-trivial work:

1. Resolve the project with `list_projects`.
2. Confirm freshness with `index_status`.
3. Run `get_architecture` when repository structure is unknown.
4. Locate symbols using `search_graph` or `search_code`.
5. Trace relationships using `trace_path`.
6. Read exact code using `get_code_snippet` or native `read`.
7. Implement the smallest correct change.
8. Run relevant formatting, linting, tests, and builds.
9. Run `detect_changes`.
10. Review unexpected blast radius before completion.
