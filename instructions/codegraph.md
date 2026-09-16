# CodeGraph — Agent Tooling Instructions

> Operational instructions for AI coding agents such as OpenCode.
>
> Use this file as an agent instruction, not as end-user documentation.

## Purpose

CodeGraph is a pre-indexed structural code intelligence layer.

Use it to understand:

- symbols;
- files;
- callers and callees;
- execution flows;
- imports and dependencies;
- inheritance and implementations;
- framework-aware relationships;
- change impact;
- affected tests.

The objective is to obtain structural context directly from the graph instead of rebuilding it manually with repeated filesystem searches, `grep`, `glob`, and file reads.

---

# 1. Core Agent Rule

For an indexed codebase, prefer CodeGraph for **code discovery and structural reasoning**.

Start with:

```text
codegraph_explore
```

or, when MCP is unavailable:

```bash
codegraph explore "<query>"
```

Use normal filesystem/search tools only when:

- CodeGraph does not contain the required information;
- the target is not indexed;
- the task concerns non-source content not represented well by the graph;
- an exact textual lookup is still required;
- CodeGraph reports stale/pending data that requires a direct read.

Do not perform a large `Grep → Read → Grep → Read` exploration when CodeGraph can answer the structural question directly.

---

# 2. Tool Selection Guide

Use this decision table.

| Need | Preferred MCP tool | CLI equivalent |
|---|---|---|
| Understand how something works | `codegraph_explore` | `codegraph explore` |
| Trace a flow between components | `codegraph_explore` | `codegraph explore` |
| Survey an unfamiliar area | `codegraph_explore` | `codegraph explore` |
| Read a known symbol | `codegraph_node` | `codegraph node` |
| Read a known indexed source file | `codegraph_node` | `codegraph node` |
| Locate symbols by name | `codegraph_search` | `codegraph query` |
| Find who calls a function/method | `codegraph_callers` | `codegraph callers` |
| Find what a function/method calls | `codegraph_callees` | `codegraph callees` |
| Estimate blast radius before a change | `codegraph_impact` | `codegraph impact` |
| Inspect indexed project structure | `codegraph_files` | `codegraph files` |
| Check graph/index health | `codegraph_status` | `codegraph status` |
| Find tests affected by changed files | CLI only | `codegraph affected` |

## Fast decision tree

```text
Need to understand behavior or architecture?
    → codegraph_explore

Know the exact symbol/file and need its source?
    → codegraph_node

Do not know where a symbol is?
    → codegraph_search

Need upstream callers?
    → codegraph_callers

Need downstream calls?
    → codegraph_callees

About to modify/refactor a symbol?
    → codegraph_impact

Need indexed directory/file structure?
    → codegraph_files

Unsure whether the graph is healthy/current?
    → codegraph_status

Need tests related to changed files?
    → codegraph affected
```

---

# 3. `codegraph_explore`

## Role

This is the **primary CodeGraph tool**.

Use it for most code-understanding tasks before reaching for narrower tools.

It can return:

- relevant symbols;
- verbatim source grouped by file;
- line-numbered source when a file/symbol is explicitly named;
- relationships between symbols;
- call paths;
- framework-aware/dynamic-dispatch connections;
- blast-radius information.

## Use it for

- "How does X work?"
- "Where is X implemented?"
- "How does X reach Y?"
- "What is the request flow for this endpoint?"
- "How does authentication work?"
- "Which components participate in this feature?"
- "What happens after this method is called?"
- "What could be affected if I change this behavior?"
- initial bug investigation;
- architecture exploration;
- pre-edit context gathering.

## Good queries

Prefer focused natural language with concrete symbols when known.

```text
How does authentication flow from LoginController to token generation?
```

```text
How does OrderController.create reach OrderRepository.save?
```

```text
UserService updateUser UserRepository
```

```text
How is PaymentService.retryPayment used and what depends on it?
```

```text
src/main/java/com/example/service/UserService.java
```

## Query guidance

When possible include:

1. the behavior being investigated;
2. the known entry point;
3. the known destination;
4. exact symbol names;
5. exact file names/paths.

Prefer:

```text
How does AuthController.login reach JwtService.generateToken?
```

over:

```text
authentication
```

## Agent behavior after `codegraph_explore`

Treat returned source as already read.

Do **not** immediately use `Read` or `Grep` against the exact same code merely to reconfirm it.

If context is insufficient:

1. refine the CodeGraph query;
2. name a more exact symbol/file;
3. query the missing relationship;
4. only then use fallback tools if necessary.

---

# 4. `codegraph_node`

## Role

Use `codegraph_node` when the target is already known.

It can provide:

- a symbol's source;
- caller/callee trail;
- line-numbered source;
- a complete indexed source-file read;
- multiple overload bodies when a symbol name is ambiguous.

CLI:

```bash
codegraph node <symbol|file>
```

## Use it when

- the exact class/function/method is known;
- `codegraph_explore` identified the symbol and a focused view is now needed;
- a file path is already known;
- source needs to be read without falling back to a regular `Read` tool;
- overloads need inspection.

## Examples

```bash
codegraph node UserService
```

```bash
codegraph node UserService.updateUser
```

```bash
codegraph node src/services/user.ts
```

## Do not use it when

You still do not know what symbol matters.

Use `codegraph_explore` or `codegraph_search` first.

---

# 5. `codegraph_search`

## Role

Find indexed symbols by name.

This is primarily a **locator**, not the main reasoning tool.

CLI equivalent:

```bash
codegraph query <search>
```

Useful CLI options include:

```bash
--kind
--limit
--json
```

## Use it when

- a symbol name is partially known;
- several symbols have similar names;
- you need symbol locations;
- a previous query reported that a symbol was not found;
- you need to disambiguate a name before using `node`, `callers`, `callees`, or `impact`.

## Examples

```bash
codegraph query UserService
```

```bash
codegraph query UserService --kind class --limit 10
```

## Preferred sequence

```text
search
  ↓
identify exact symbol
  ↓
node / callers / callees / impact
```

Do not repeatedly search broad terms when `codegraph_explore` can answer the complete conceptual question.

---

# 6. `codegraph_callers`

## Role

Find what calls a function or method.

CLI:

```bash
codegraph callers <symbol>
```

## Use it to answer

- Who invokes this method?
- What are the entry points into this behavior?
- Which services/controllers depend on this function?
- Is this method externally reachable?
- What upstream paths can lead here?

## Examples

```bash
codegraph callers handleRequest
```

```bash
codegraph callers UserService.updateUser
```

For machine-readable output:

```bash
codegraph callers handleRequest --json
```

## Interpretation

Callers are **upstream dependencies**.

Think:

```text
caller
  ↓
target symbol
```

Use callers when changing the target may require reviewing code that invokes it.

---

# 7. `codegraph_callees`

## Role

Find what a function or method calls.

CLI:

```bash
codegraph callees <symbol>
```

## Use it to answer

- What does this method delegate to?
- Which downstream services are called?
- Which repositories/utilities are reached?
- What dependencies participate in this execution path?
- What does this symbol directly do through other symbols?

## Examples

```bash
codegraph callees handleRequest
```

```bash
codegraph callees CheckoutService.checkout
```

## Interpretation

Callees are **downstream dependencies**.

Think:

```text
target symbol
  ↓
callee
```

Use this to understand implementation flow and dependencies below a known symbol.

---

# 8. `codegraph_impact`

## Role

Analyze the transitive blast radius of changing a symbol.

CLI:

```bash
codegraph impact <symbol>
```

Useful option:

```bash
--depth <n>
```

## Use it before

- refactoring;
- renaming/changing contracts;
- modifying public methods;
- changing return types;
- changing shared services;
- altering core domain logic;
- removing a symbol;
- making non-trivial behavior changes.

## Examples

```bash
codegraph impact AuthMiddleware
```

```bash
codegraph impact UserService.updateUser --depth 3
```

## Agent rule

Before a non-trivial edit to a known symbol:

```text
understand target
    ↓
inspect impact
    ↓
edit
    ↓
validate
```

Do not confuse impact analysis with testing.

`codegraph_impact` predicts structural reach. It does not prove that the modified program is correct.

---

# 9. `codegraph_files`

## Role

Inspect the **indexed file structure** without scanning the filesystem manually.

CLI:

```bash
codegraph files [path]
```

Useful CLI options may include:

```bash
--format
--filter
--pattern
--max-depth
--json
```

## Use it when

- learning repository layout;
- locating an indexed package/module area;
- inspecting a subtree;
- avoiding large `Glob` or filesystem traversal;
- determining whether a source area is represented in the graph.

## Examples

```bash
codegraph files
```

```bash
codegraph files src
```

Use this for **structure**.

Use `codegraph_search` for **symbols**.

Use `codegraph_explore` for **behavior and relationships**.

---

# 10. `codegraph_status`

## Role

Check graph/index health and statistics.

CLI:

```bash
codegraph status
```

It can report information such as:

- node counts;
- edge counts;
- indexed file counts;
- database/backend status;
- graph health;
- pending synchronization information in agent/MCP sessions.

## Use it when

- CodeGraph results appear inconsistent;
- recently edited files seem missing;
- you suspect synchronization delay;
- the graph may not be initialized correctly;
- debugging CodeGraph behavior;
- deciding whether direct source reads are required temporarily.

Do not call status before every query.

Use it when index health is relevant.

---

# 11. `codegraph affected`

## Role

Find test files transitively affected by changed source files.

This is primarily a CLI workflow.

```bash
codegraph affected [files...]
```

## Use it when

- selecting tests after modifications;
- limiting CI execution to relevant tests;
- determining what test files are connected through imports;
- validating a targeted change efficiently.

## Examples

```bash
codegraph affected src/utils.ts src/api.ts
```

With Git:

```bash
git diff --name-only | codegraph affected --stdin
```

Only paths:

```bash
git diff --name-only | codegraph affected --stdin --quiet
```

Custom test pattern:

```bash
codegraph affected src/auth.ts --filter "e2e/*"
```

Common options:

```text
--stdin
--depth <n>
--filter <glob>
--json
--quiet
```

## Important

Affected tests are a **test selection aid**.

They do not replace:

- compilation;
- type checking;
- unit/integration tests;
- linting;
- runtime validation.

---

# 12. MCP Surface vs CLI Surface

By default, the CodeGraph MCP server intentionally exposes only:

```text
codegraph_explore
```

The narrower MCP tools still exist:

```text
codegraph_node
codegraph_search
codegraph_callers
codegraph_callees
codegraph_impact
codegraph_files
codegraph_status
```

They may be enabled through the CodeGraph MCP configuration, for example:

```text
CODEGRAPH_MCP_TOOLS=explore,node,search,callers
```

If a specialized MCP tool is not exposed, use:

1. `codegraph_explore` first;
2. its CLI equivalent when appropriate and allowed.

Equivalent mapping:

```text
codegraph_explore  → codegraph explore
codegraph_node     → codegraph node
codegraph_search   → codegraph query
codegraph_callers  → codegraph callers
codegraph_callees  → codegraph callees
codegraph_impact   → codegraph impact
codegraph_files    → codegraph files
codegraph_status   → codegraph status
```

Do not assume a missing specialized MCP tool means the CodeGraph capability is unavailable.

---

# 13. Recommended Agent Workflows

## A. Understand an unfamiliar feature

```text
1. codegraph_explore
2. inspect returned symbols/source/call paths
3. refine with another explore query only if needed
4. use node for an exact symbol only if deeper source is required
```

Example:

```text
How does password reset work from the HTTP endpoint through persistence?
```

Avoid starting with broad filesystem exploration.

---

## B. Find an unknown implementation

```text
1. codegraph_explore with the behavior/name
2. if name ambiguity remains → codegraph_search
3. exact symbol → codegraph_node
```

---

## C. Investigate a bug

```text
1. Identify observed behavior / error location.
2. codegraph_explore the relevant flow.
3. Inspect upstream callers if entry path is unclear.
4. Inspect downstream callees if failure propagation is unclear.
5. Inspect the exact target source with node if needed.
6. Determine likely fix.
7. Check impact before a non-trivial edit.
8. Edit.
9. Run compiler/tests/linter.
```

---

## D. Modify an existing method

```text
1. codegraph_node or codegraph_explore
2. codegraph_callers
3. codegraph_impact
4. edit
5. codegraph affected <changed-files> when useful
6. run relevant validation
```

For a simple local/private symbol, `explore` may already provide enough caller/impact context; do not mechanically call every tool.

---

## E. Refactor shared code

```text
1. codegraph_explore
2. codegraph_callers
3. codegraph_callees
4. codegraph_impact with suitable depth
5. perform focused refactor
6. inspect affected tests
7. compile/type-check
8. run tests
```

---

## F. Trace an end-to-end flow

Prefer a single explicit `codegraph_explore` query:

```text
How does ApiController.submit reach PaymentRepository.save?
```

Only split the investigation into callers/callees manually if `explore` cannot provide enough detail.

---

## G. Inspect repository architecture

```text
1. codegraph_explore with an architectural question
2. codegraph_files when directory structure matters
3. codegraph_search when locating named abstractions
```

Examples:

```text
What are the major components involved in request authentication?
```

```text
How are controllers, services, and repositories connected in this application?
```

---

# 14. Query Construction Rules

## Prefer concrete questions

Good:

```text
How does UserController.update call UserService and persist the entity?
```

Weak:

```text
user
```

Good:

```text
What calls CacheManager.invalidate and what would changing it affect?
```

Weak:

```text
cache stuff
```

## Include exact identifiers

Whenever known, include:

- class names;
- function names;
- method names;
- filenames;
- module names;
- paths;
- endpoints.

## Ask for relationships

Useful formulations:

```text
How does X reach Y?
```

```text
What calls X?
```

```text
What does X call?
```

```text
What depends on X?
```

```text
What is the blast radius of changing X?
```

```text
Where is behavior X implemented?
```

---

# 15. Avoid Redundant Exploration

When CodeGraph returns the relevant source and relationships, do not automatically:

```text
Grep the same symbol
Read the same file
Glob the same directory
Search for the same callers
Reconstruct the same call chain manually
```

Additional exploration must have a reason.

Valid reasons include:

- content is outside the graph;
- configuration/docs are needed;
- generated code must be inspected;
- CodeGraph reports pending/stale content;
- exact text not represented structurally is required;
- runtime behavior must be verified;
- tests reveal a discrepancy.

---

# 16. Index Synchronization and Staleness

CodeGraph normally watches indexed projects and incrementally syncs changes.

If results appear stale:

1. inspect warnings returned by CodeGraph;
2. use `codegraph_status` when necessary;
3. directly read only files whose current content cannot be trusted;
4. avoid throwing away valid graph context from unaffected files.

If operating through CLI outside the normal agent watcher flow, the following commands may exist for index maintenance:

```bash
codegraph index
codegraph sync
```

Do not re-index reflexively during normal agent work.

---

# 17. Indexed vs Non-Indexed Projects

CodeGraph operates on indexed projects.

If the current project is indexed, use CodeGraph normally.

If another indexed project or monorepo sub-project is required, MCP tools can use the relevant `projectPath` when supported.

If a path has no CodeGraph index:

- do not assume graph results exist;
- fall back to normal agent tools;
- do not initialize/index a project automatically unless explicitly requested.

Index creation is an environment/user decision, not an implicit code-navigation step.

---

# 18. Monorepos and Multiple Projects

When multiple indexed projects exist, make sure the query targets the correct project.

Use `projectPath` where available.

Conceptually:

```text
repository root
├── service-a     ← indexed project
├── service-b     ← indexed project
└── frontend      ← indexed project
```

A flow involving `service-b` must query the graph for `service-b`, not blindly assume the MCP server root is the correct project.

---

# 19. Relationship Semantics

CodeGraph represents code as nodes and edges.

Useful structural relationships include concepts such as:

```text
contains
calls
imports
exports
extends
implements
references
type_of
returns
instantiates
overrides
decorates
```

When reasoning about code, distinguish relationship types.

For example:

```text
A imports B
```

does not necessarily mean:

```text
A calls B
```

Likewise:

```text
A implements InterfaceB
```

is different from:

```text
A is called by InterfaceB
```

Use the graph relationship returned by CodeGraph rather than inferring a stronger relationship from filenames or proximity.

---

# 20. Dynamic / Framework-Aware Relationships

CodeGraph may surface connections that ordinary textual search cannot follow reliably, including framework or dynamic-dispatch paths.

Examples can include:

- callbacks;
- interface → implementation resolution;
- framework wiring;
- component/render relationships;
- observer-style connections.

Prefer graph-provided relationships over assumptions based only on text matching.

When CodeGraph marks a connection as heuristic or synthesized, treat that provenance as useful evidence, not as identical to a direct AST edge.

---

# 21. Editing Policy

CodeGraph is a **context and structural reasoning tool**.

It does not replace editing tools.

Before a non-trivial edit:

```text
Understand
    ↓
Locate
    ↓
Trace relationships
    ↓
Assess impact
    ↓
Edit
    ↓
Validate
```

The exact number of CodeGraph calls should remain minimal.

Do not call every specialized tool mechanically.

Use the smallest set needed to understand the change safely.

---

# 22. Validation Policy

After code changes, use the project's actual validation mechanisms.

Examples:

```text
compiler
type checker
unit tests
integration tests
linter
formatter checks
build
runtime/smoke tests
```

CodeGraph can tell the agent **what is connected**.

It cannot prove that the new implementation behaves correctly.

---

# 23. Tool Priority

For indexed source code, prefer:

```text
1. codegraph_explore
2. specialized CodeGraph tool when it answers a narrower unresolved question
3. targeted source/config read when graph information is insufficient
4. edit
5. compiler / tests / linter / build
```

Do not invert this into:

```text
grep everything
read everything
understand manually
then call CodeGraph as confirmation
```

CodeGraph should remove redundant discovery work.

---

# 24. Compact Agent Rules

Use these as strict defaults:

```text
- Use CodeGraph first for structural code questions.
- Prefer codegraph_explore for general reasoning.
- Treat source returned by CodeGraph as already read.
- Use node for an exact symbol or indexed source file.
- Use search/query to locate symbols, not to explain architecture.
- Use callers for upstream dependencies.
- Use callees for downstream calls.
- Use impact before risky/shared-symbol changes.
- Use files for indexed structure.
- Use status only when graph health matters.
- Use affected to select relevant tests after changes.
- Prefer one strong query over many weak queries.
- Do not duplicate CodeGraph output with Grep/Read without a concrete reason.
- Fall back gracefully when content is not indexed.
- Do not initialize or rebuild indexes automatically unless requested.
- CodeGraph does not replace compiler, tests, linter, build, or runtime validation.
```

---

# 25. Quick Reference

```text
QUESTION / ARCHITECTURE / FLOW
→ codegraph_explore
→ codegraph explore "<query>"

KNOWN SYMBOL OR FILE
→ codegraph_node
→ codegraph node <symbol|file>

FIND SYMBOL
→ codegraph_search
→ codegraph query <search>

WHO CALLS X?
→ codegraph_callers
→ codegraph callers <symbol>

WHAT DOES X CALL?
→ codegraph_callees
→ codegraph callees <symbol>

WHAT BREAKS IF X CHANGES?
→ codegraph_impact
→ codegraph impact <symbol>

WHAT FILES ARE INDEXED?
→ codegraph_files
→ codegraph files [path]

IS THE GRAPH HEALTHY?
→ codegraph_status
→ codegraph status

WHAT TESTS ARE AFFECTED?
→ codegraph affected [files...]
```

---

# Final Principle

CodeGraph is not another step to add on top of traditional repository exploration.

For indexed code, it should **replace unnecessary exploration**.

Ask the graph for structural context, act on that context, and use the project's real validation tools to prove the resulting change.
