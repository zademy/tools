# CodeGraph — Local Code Intelligence

## Role
Use CodeGraph for structural reasoning about the **current local repository when it is indexed**: architecture, symbols, callers/callees, execution paths, dependencies, implementations, impact, and affected tests.

## Routing
- Local indexed source → CodeGraph.
- Public remote GitHub repo → Zread.
- External library/API docs → Context7.
- Public web information → Web Search Prime / Web Reader.
- Durable project history → Engram.
- Large command processing → RTK / context-mode.
- Visual input → Z.AI Vision.

## Workflow
1. Start general structural questions with `codegraph_explore`.
2. Use specialized graph tools only for a specific unresolved need: exact node, symbol location, callers, callees, impact, files, or status.
3. Treat source returned by CodeGraph as already read. Do not duplicate the same discovery with grep/read without a reason.
4. Before risky shared-code changes, inspect impact when useful.
5. After editing, validate with the project's actual compiler, tests, linter, build, or runtime checks.

## Fallback
Use direct file/search tools when content is not indexed, exact non-structural text is required, or the graph is stale. Do not create/rebuild indexes automatically unless requested.

Use RTK for normal validation output; use context-mode when output is unusually large or needs filtering/correlation.
