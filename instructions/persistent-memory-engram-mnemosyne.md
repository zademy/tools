# Persistent Memory Protocol - Engram + Mnemosyne

One fact has one owner:

- Project or repository facts go to **Engram**.
- Durable user context that remains useful across projects goes to
  **Mnemosyne**.
- Temporary facts are not persisted.
- Secrets are never persisted.

Do not mirror a fact across systems. Engram is the repository brain;
Mnemosyne is durable cross-project user context.

```text
                    OpenCode Agent
                         │
           ┌─────────────┴─────────────┐
           │                           │
           ▼                           ▼
        ENGRAM                     MNEMOSYNE
           │                           │
    Repository Brain              User Brain
           │                           │
    Current project               Cross-project
    architecture                  preferences
    decisions                     workflows
    bugs                          conventions
    discoveries                   stable context
    constraints
    session state
```

## Ownership

Engram owns project architecture and implementation decisions, bug root
causes, repository conventions, project-specific database or framework
behavior, configuration, constraints, failed approaches, module relationships,
technical debt, current implementation state, and session handoffs.

Mnemosyne owns stable coding, explanation, tool, architecture, testing, and
workflow preferences; general technology preferences; cross-project
constraints and conventions; and reusable context about how the user works.

Before writing, ask:

1. Will another session benefit? If not, do not persist it.
2. Would it still matter in a completely different repository? If no, use
   Engram. If yes and it is durable user or cross-project context, use
   Mnemosyne. Otherwise do not persist it.

For example, `This repository uses PostgreSQL 18.`,
`ProductoService currently has an N+1 issue.`,
`AuthenticationController was refactored today.`, and
`The payment API timeout was caused by RetryService.` belong in Engram.
`User prefers Java examples.`, `User likes detailed architecture explanations.`,
and `User generally prefers local tools.` belong in Mnemosyne when durable.

## Source Priority

Memory supports rather than overrides current evidence. Apply this order:

1. Current explicit user instructions.
2. Current repository state.
3. Current tests and runtime evidence.
4. Current project configuration.
5. Engram project memory.
6. Mnemosyne cross-project memory.
7. Agent assumptions.

Never modify correct current code because old memory differs.

## Startup And Retrieval

For meaningful repository work:

1. Run `mem_current_project` to identify the Engram project.
2. Run `mem_context` to recover recent project context.
3. For prior project bugs, decisions, configuration, limitations, behavior,
   history, or failed approaches, run `mem_search`; use
   `mem_get_observation` for full detail and `mem_timeline` for evolution.
4. Run `mnemosyne_recall` only when durable user or cross-project context could
   materially affect the task. Do not query it automatically on every turn.

For ambiguous questions in a repository, search Engram first, then Mnemosyne
only if broader context improves the decision. Search Engram before repeating
project investigation; recall Mnemosyne before asking again for a stable user
preference. Do not query both mechanically.

```text
START PROJECT WORK
        │
        ▼
mem_current_project
        │
        ▼
mem_context
        │
        ▼
Need previous project knowledge?
        │
       YES
        ▼
mem_search
        │
        ▼
Need user/cross-project context?
        │
       YES
        ▼
mnemosyne_recall
        │
        ▼
WORK ON CURRENT REPOSITORY
        │
        ├── Project knowledge discovered
        │          │
        │          ▼
        │       mem_save
        │
        └── Durable cross-project preference discovered
                   │
                   ▼
          mnemosyne_remember
        │
        ▼
MEANINGFUL SESSION COMPLETE
        │
        ▼
mem_session_summary
```

## Saving To Engram

Run `mem_save` after durable project knowledge is discovered: decisions,
root-cause fixes, important discoveries, configuration, reusable patterns,
constraints, non-obvious behavior, or failed approaches worth avoiding.
Engram is not a tool log; omit routine files and commands, trivial edits,
formatting, imports, temporary debugging, ordinary test output, speculative
hypotheses, and status updates.

Use concise, searchable observations:

```text
Title:
Product report uses DTO projection

What:
Product reporting uses a DTO projection instead of loading Product entities.

Why:
Entity hydration produced unnecessary joins and increased memory consumption.

Where:
ProductoRepository
ProductoReportService

Learned:
Do not replace the projection with EntityGraph unless the report starts requiring complete related entities.
```

For evolving project knowledge, reuse a stable `topic_key`, such as
`architecture/authentication`, `architecture/database-access`,
`architecture/session-management`, `config/keycloak`,
`pattern/error-handling`, or `decision/api-pagination`. Use
`mem_suggest_topic_key` when needed and `mem_update` to correct or refine a
known observation.

If `mem_save` reports a possible conflict, validate current repository
evidence and resolve it with `mem_judge` under the Engram conflict protocol.

## Saving To Mnemosyne

Run `mnemosyne_remember` only for durable context useful beyond the current
repository, for example `User generally prefers constructor injection in Spring projects.`,
`User prefers examples using Java when discussing backend patterns.`,
`User prefers technical explanations that include concrete implementation examples.`,
`User normally wants local-first development tools when possible.`, or
`User prefers avoiding unnecessary dependencies.`

Do not assume Mnemosyne tools beyond those exposed at runtime.

## Split Facts And Scope Conflicts

Split statements that contain both scopes. For:

> I normally prefer WebFlux, but this application must stay MVC because it runs on Spring Boot 1.4.1.

Store `User generally prefers reactive/WebFlux approaches when technically appropriate.`
in Mnemosyne and
`This project must remain Spring MVC because it runs on Spring Boot 1.4.1 and migration is outside the current scope.`
in Engram. Do not copy the whole statement into both systems.

A project constraint overrides a general preference only in that project. For
example, Engram's
`Legacy framework in this project requires the existing field-injection pattern to remain unchanged.`
overrides Mnemosyne's `User prefers constructor injection.` locally. Keep the
Mnemosyne preference because it may apply elsewhere; explain or apply the local
exception when relevant. Distinct facts such as
`User generally prefers constructor injection in Java projects.` and
`This legacy project continues using field injection for compatibility and scope reasons.`
may coexist.

## Updating And Forgetting

An explicit current preference change takes effect immediately. If
`User prefers Maven.` becomes `From now on I prefer Gradle.`, update durable
Mnemosyne knowledge using available tools and store the replacement with
`mnemosyne_remember`.

Treat `mnemosyne_forget` as destructive. Use it only for an unambiguous target
when the user requests forgetting, a memory is incorrect, a stable preference
was replaced, sensitive data was accidentally persisted, or an exact harmful
duplicate exists. Never broadly delete by vague match or merely because a
memory is old.

## Sensitive Information

Never store passwords, API keys, access or refresh tokens, private keys,
cookies, session secrets, database credentials, `.env` secret values, or
verification codes in either system. Store only a useful non-secret conclusion:
`Payment integration requires PAYMENT_API_TOKEN.` is valid;
`PAYMENT_API_TOKEN=...` is not.

## Compaction Recovery

After context compaction or loss in a repository, save an available compacted
summary with `mem_session_summary` so pre-compaction work remains durable, then
run `mem_context` and, when needed, `mem_search`. Recover project state from
Engram rather than guesses. Run `mnemosyne_recall` only if user or cross-project
context is also required.

## Session Summary Ownership

Before finishing meaningful repository work, use `mem_session_summary` in
Engram. Capture **Goal**, **Instructions** when relevant, **Discoveries**,
**Accomplished**, **Next Steps**, and **Relevant Files**, including decisions,
current state, remaining work, and important locations. Mnemosyne must not
receive a duplicate session summary.

Memory operations support development; they do not dominate it. Search Engram
before rediscovery, recall Mnemosyne before re-asking durable preferences, save
each durable fact to its one owner, and summarize repository sessions in
Engram.
