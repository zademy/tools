# Engram v2 Persistent Memory Protocol

Engram stores durable project engineering knowledge, not transcripts or activity
logs. Treat current user instructions, repository state, configuration, and
runtime evidence as truth; use memory as validated supporting context.

## Session Start And Project Resolution

For meaningful repository work:

1. Run `mem_current_project` first.
2. If a project is resolved, run `mem_context` for that project to recover recent
   work, decisions, fixes, constraints, conventions, unfinished tasks, and next
   steps.
3. Before repeating investigation, run `mem_search` with short, meaningful terms
   such as a class, service, module, configuration, error, or concept.
4. Prefer `response_format: "compact"` for routine search. Start with the default
   `match_mode: "all"`; retry with `match_mode: "any"` only when broader recall is
   useful.
5. Run `mem_get_observation` only when a relevant result requires complete,
   untruncated content.
6. Use `mem_timeline` only when chronological evolution matters AND that admin
   tool is actually exposed by the active MCP profile. Do not depend on it in the
   normal `--tools=agent` profile.

Progressive retrieval should normally stay bounded:

`mem_current_project` -> `mem_context` -> `mem_search` -> `mem_get_observation`

Use `mem_list_projects` when project discovery across known Engram projects is
needed. Use `mem_search(all_projects=true)` only when cross-project recall is
intentional; otherwise keep reads scoped to the current project.

If `mem_current_project` reports an empty project with multiple
`available_projects`, do not guess the target. Prefer operating from the intended
repository root or a repository `.engram/config.json`. If user selection is
needed, ask them to choose an exact listed project.

If a write returns `ambiguous_project`, never invent or normalize a choice. Ask
the user to choose exactly one returned `available_projects` value, then retry
the same write with:

- `project`: the exact selected value;
- `project_choice_reason: "user_selected_after_ambiguous_project"`;
- the returned `recovery_token` when the tool schema provides/requires it.

If Engram reports `project_name_collision`, `repository_binding_unavailable`, an
unknown project, or session/project mismatch, do not silently fall back. Resolve
the project identity problem explicitly. `mem_doctor` is the preferred read-only
diagnostic tool when project or store behavior is unclear.

## Source Priority

Resolve discrepancies in this order:

1. Explicit current user instructions.
2. Current repository state.
3. Current configuration.
4. Current runtime and test evidence.
5. Engram memory.
6. Agent assumptions.

Investigate conflicts rather than trusting memory. If the repository clearly
changed, update the relevant observation.

## Save Triggers And Quality

Run `mem_save` at meaningful checkpoints when another session will benefit:

- Architectural or technical decisions and their reasons.
- Non-obvious bug symptoms, root causes, fixes, and caveats.
- Important discoveries, framework limitations, or hidden dependencies.
- Reusable project patterns and conventions.
- Important configuration or environment requirements.
- Project constraints and non-obvious failed approaches worth avoiding.
- Completion of an important implementation, including the resulting current
  implementation state.

Do not save routine commands, file reads, edits, imports, formatting, temporary
debugging, build/test output, obvious code facts, speculative or disproven
hypotheses, status updates, or conversation filler. Do not call memory tools
after every edit.

Use short, searchable titles such as `Fixed N+1 in product search`,
`Authentication roles come from Keycloak resource_access`,
`Use JDBC for bulk report queries`, or `Oracle timeout caused by missing index`;
avoid `Important`, `Changes`, `Bug`, `Fixed issue`, and `New information`.

Choose the most accurate type supported by Engram v2: `bugfix`, `decision`,
`architecture`, `discovery`, `pattern`, `config`, or `learning`. Repository
knowledge should normally use `scope: project`.

For normal human/proactive saves, leave `capture_prompt` at its default `true`.
Set `capture_prompt: false` for automated/generated artifacts where associating
the current user prompt would add noise. Prompt capture is best-effort and a
save remains valid when no prompt context is available.

Structure observations as:

```text
What:
Product search was changed to use a projection query.

Why:
Loading complete Product entities caused unnecessary joins and high memory usage.

Where:
ProductoRepository
ProductoSearchService

Learned:
Avoid EntityGraph for this endpoint because only five fields are required.
```

Record what changed or was learned, why it matters, where it applies, and any
caveats, edge cases, failed approaches, or future implications. Prefer
repository-relative paths instead of absolute filesystem paths.

For a failed approach, preserve the tested result and replacement decision:

```text
What:
Using EntityGraph for the product report was tested.

Result:
Performance became worse because several OneToMany relationships were fetched.

Decision:
Use a projection query instead.

Learned:
Do not retry EntityGraph for this report unless the data requirements change.
```

## Session And Write Safety

Do not invent `session_id` values from task names, issue numbers, dates, or other
agent-generated labels. Only pass a session ID that was successfully registered
with `mem_session_start` or supplied by an authoritative runtime integration.

When no authoritative session ID is available, omit it and let Engram perform
its safe session resolution. If Engram reports an unknown or ambiguous session,
do not select another session by recency or guesswork.

A supplied explicit project and session must refer to the same normalized
project. Treat write failures as safety signals; do not bypass them by creating a
new project name.

Memory bookkeeping must never replace the user-facing answer. Complete required
memory operations before the final reply. If a memory operation fails, still
deliver the substantive answer and mention the memory failure only when useful.

## Evolving Knowledge

Avoid contradictory duplicates for evolving topics such as authentication,
database access, caching, APIs, or deployment. Reuse a stable `topic_key`, for
example `architecture/authentication`, `architecture/database`,
`architecture/session-management`, `config/keycloak`,
`pattern/error-handling`, `decision/api-pagination`, or
`decision/database-access`.

Use `mem_suggest_topic_key` when a suitable key is unclear. A repeated
`project + scope + topic_key` is an upsert of the evolving topic, so reuse keys
only for the same conceptual subject.

Use `mem_update` when a known observation needs correction or refinement because
implementation, evidence, assumptions, a decision, or a fix changed. Preserve
meaningful history rather than erasing it unnecessarily.

## Search Relations And Conflict Judgment

Treat relation annotations returned by search as context, not unquestionable
truth. Validate them against current repository and runtime evidence.

After every `mem_save`, inspect the response. If `judgment_required` is true,
process every entry in `candidates[]` using that candidate's own `judgment_id`;
never reuse a top-level or another candidate's ID.

Judge each pair as `related`, `compatible`, `scoped`, `conflicts_with`,
`supersedes`, or `not_conflict` only after validating repository,
configuration, user, and runtime evidence. Never prefer newer memory merely
because it is newer.

- Ask the user before judging when confidence is below `0.7`.
- Ask before `supersedes` or `conflicts_with` when the memory concerns an
  architecture, policy, or decision.
- Otherwise resolve silently with `mem_judge`, including concise reason,
  evidence, and confidence.

Use `mem_compare` only for proactive semantic comparison between two existing
observations when no pending `mem_save` candidate is driving the flow. Read both
memories first. Do not create cross-project relations; Engram rejects them.

## Review And Pin Lifecycle

Use `mem_review` with action `list` to find observations whose `review_after` has
passed. Validate each against current sources. Update incorrect or evolved
knowledge with `mem_update`; use `mem_review` with action `mark_reviewed` only
after a still-correct observation has been checked.

Use `mem_pin` sparingly for stable, high-value observations that should appear
before ordinary recent context. Use `mem_unpin` when that priority is no longer
useful. Pin state and review-reset state are local to the device and should not
be assumed to synchronize to other machines.

## Sensitive Information

Never persist passwords, API keys, access or refresh tokens, private keys,
cookies, session secrets, database credentials, or secret environment/config
values. Store only the non-secret conclusion, for example:

`Application authentication requires the PAYMENT_API_TOKEN environment variable.`

Never store the token value.

## Project Recovery And Compaction

After conversational loss, reset, or compaction, persist the compacted handoff
with `mem_session_summary` when available, then run `mem_context`. Use
`mem_search` for specific gaps instead of reconstructing prior work from guesses.

For cross-project recovery, use `mem_list_projects` first when the project key is
unknown. Use `mem_search(all_projects=true)` only when global recall is actually
required.

## Session Closure

Before finishing every meaningful repository session, run
`mem_session_summary`. Keep the handoff concise and include:

- **Goal**: intended outcome.
- **Instructions**: user constraints or preferences, when present.
- **Discoveries**: non-obvious findings and caveats.
- **Accomplished**: completed work and key details.
- **Next Steps**: remaining work and recommended continuation.
- **Relevant Files**: important files, classes, modules, services, or config.

Include current state, decisions, and remaining work within those sections.
Exclude the conversation transcript and trivial implementation details. The
summary must let another session continue without reconstructing the work.
