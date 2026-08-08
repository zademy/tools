# Engram Persistent Memory Protocol

Engram stores durable project engineering knowledge, not transcripts or
activity logs. Use the current user instructions, repository, configuration,
and runtime evidence as truth; use memory as validated supporting context.

## Session Start And Retrieval

For meaningful repository work:

1. Run `mem_current_project` to identify the project namespace.
2. Run `mem_context` to recover recent work, decisions, fixes, constraints,
   conventions, unfinished tasks, and next steps.
3. Before repeating investigation, run `mem_search` with short, meaningful
   terms such as a class, service, module, configuration, error, or concept.
   Prefer `authentication login failure` or `Keycloak authentication` over
   `"The user says authentication is failing when trying to login and wants to know what we did last time"`.
4. Run `mem_get_observation` only when a relevant result requires its complete
   content.
5. Run `mem_timeline` only when chronological evolution matters.

This progressive order keeps retrieval bounded:
`mem_context` -> `mem_search` -> `mem_get_observation` -> `mem_timeline`.
Retrieve only enough for the current task and validate it against current
evidence. Do not blindly repeat a previously failed approach.

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
- Completion of an important or significant implementation, including the
  resulting current implementation state.

Do not save routine commands, file reads, edits, imports, formatting, temporary
debugging, build/test output, obvious code facts, speculative or disproven
hypotheses, status updates, or conversation filler. Do not call memory tools
after every edit.

Use short, searchable titles such as `Fixed N+1 in product search`,
`Authentication roles come from Keycloak resource_access`,
`Use JDBC for bulk report queries`, `Oracle timeout caused by missing index`,
or `Retiro flow state stored in SessionScope`; avoid `Important`, `Changes`,
`Bug`, `Fixed issue`, and `New information`.

Choose the most accurate type: `bugfix`, `decision`, `architecture`,
`discovery`, `pattern`, `config`, or `preference`. Repository knowledge should
normally be project-scoped.

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
caveats, edge cases, failed approaches, or future implications.

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

## Evolving Knowledge

Avoid contradictory duplicates for evolving topics such as authentication,
database access, caching, APIs, or deployment. Reuse a stable `topic_key`, for
example `architecture/authentication`, `architecture/database`,
`architecture/session-management`, `config/keycloak`,
`pattern/error-handling`, `decision/api-pagination`, or
`decision/database-access`. Use `mem_suggest_topic_key` when a suitable key is
unclear.

Use `mem_update` when a known observation needs correction or refinement
because implementation, evidence, assumptions, a decision, or a fix changed.
Preserve meaningful history rather than erasing it unnecessarily.

## Conflict Judgment

After every `mem_save`, inspect the response. If `judgment_required` is true,
process every entry in `candidates[]` using that candidate's own
`judgment_id`; never reuse one ID for multiple candidates.

Judge each pair as `related`, `compatible`, `scoped`, `conflicts_with`,
`supersedes`, or `not_conflict` only after validating repository,
configuration, user, and runtime evidence. Never prefer newer memory merely
because it is newer.

- Ask the user before judging when confidence is below `0.7`.
- Ask when choosing `supersedes` or `conflicts_with` for an architecture,
  policy, or decision memory.
- Otherwise resolve silently with `mem_judge`, including reason, evidence, and
  confidence.

## Review Lifecycle

Use `mem_review` with action `list` to find observations whose `review_after`
has passed. Validate each against current sources. Update incorrect or evolved
knowledge with `mem_update`; use `mem_review` with action `mark_reviewed` only
after a still-correct observation has been checked, which resets its review
date under the type's decay policy.

## Sensitive Information

Never persist passwords, API keys, access or refresh tokens, private keys,
cookies, session secrets, database credentials, or secret environment/config
values. Store only the non-secret conclusion, for example
`Application authentication requires the PAYMENT_API_TOKEN environment variable.`
Never store the token value.

## Project Recovery

After conversational loss, reset, or compaction, run `mem_context`, then
`mem_search` for specific gaps. Recover implementation state, decisions,
completed and remaining work, and known issues from Engram rather than guesses.
If a compacted summary is available, save it with `mem_session_summary` before
continuing recovery so pre-compaction work remains durable.

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
