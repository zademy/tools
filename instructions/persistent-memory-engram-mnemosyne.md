# Persistent Memory Protocol — Engram + Mnemosyne

This agent has access to two persistent memory systems:

* **Engram** — project and repository memory.
* **Mnemosyne** — cross-project user and agent memory.

They have different responsibilities.

Do NOT treat them as redundant memory stores.

The fundamental rule is:

> Engram remembers the project.
> Mnemosyne remembers durable context that follows the user across projects.

A piece of information should normally have ONE primary memory store.

Do not save the same fact to both systems.

---

# 1. Memory Responsibilities

## Engram — Project Memory

Use Engram for knowledge that belongs to the CURRENT repository or project.

Examples:

| Information                                 | Store  |
| ------------------------------------------- | ------ |
| Architecture decision                       | Engram |
| Bug root cause                              | Engram |
| Important implementation decision           | Engram |
| Repository convention                       | Engram |
| Database behavior specific to project       | Engram |
| Project configuration                       | Engram |
| Framework limitation affecting this project | Engram |
| Failed implementation approach              | Engram |
| Important class/module relationship         | Engram |
| Current implementation state                | Engram |
| Project technical debt                      | Engram |
| Session handoff                             | Engram |
| Project-specific constraint                 | Engram |

Engram answers:

> What has been learned about this codebase?

---

## Mnemosyne — Cross-Project Memory

Use Mnemosyne for durable information that should remain useful even when the user opens another repository.

Examples:

| Information                                              | Store     |
| -------------------------------------------------------- | --------- |
| Stable user development preferences                      | Mnemosyne |
| Preferred coding style                                   | Mnemosyne |
| Preferred explanation style                              | Mnemosyne |
| Preferred tools                                          | Mnemosyne |
| Recurring workflow preferences                           | Mnemosyne |
| General technology preferences                           | Mnemosyne |
| Stable constraints that apply across projects            | Mnemosyne |
| Cross-project conventions requested by the user          | Mnemosyne |
| Reusable context about how the user wants agents to work | Mnemosyne |

Mnemosyne answers:

> How does this user normally want to work?

and:

> What durable context should follow the agent between projects?

---

# 2. Never Blindly Write to Both

Do not mirror memories.

BAD:

Engram:

`User prefers constructor injection.`

Mnemosyne:

`User prefers constructor injection.`

This creates duplicate retrieval, conflicting updates, and memory noise.

Instead decide where the information belongs.

If the user says:

> In all my Java projects, prefer constructor injection.

Store in Mnemosyne.

If the user says:

> In this legacy project use field injection because changing the architecture is out of scope.

Store in Engram.

Both memories may coexist because they represent DIFFERENT facts:

Mnemosyne:

`User generally prefers constructor injection in Java projects.`

Engram:

`This legacy project continues using field injection for compatibility and scope reasons.`

That is valid.

---

# 3. Source of Truth Priority

Persistent memory is supporting context, not absolute truth.

Use this priority:

1. Current explicit user instructions
2. Current repository state
3. Current tests and runtime evidence
4. Current project configuration
5. Engram project memory
6. Mnemosyne cross-project memory
7. Agent assumptions

Never modify correct current code merely because an old memory says something different.

---

# 4. Starting Work

When beginning meaningful work in a repository, identify the Engram project first.

Use:

`mem_current_project`

Then recover recent project context:

`mem_context`

This should normally be the first persistent-memory recovery mechanism for repository work.

Do NOT automatically query Mnemosyne on every turn.

Use Mnemosyne when cross-project or user-specific context could materially affect the task.

Examples:

* preferred coding style;
* preferred architecture style;
* preferred testing approach;
* preferred response format;
* recurring development conventions;
* previous cross-project decisions.

Use:

`mnemosyne_recall`

only when that information is relevant.

---

# 5. Retrieval Routing

Before searching memory, classify the question.

## Project-specific question

Examples:

* Why does this repository use JdbcTemplate?
* Did we already fix this Hibernate issue?
* How is authentication implemented here?
* Why is this dependency pinned?
* What was the previous implementation decision?

Search Engram first:

`mem_search`

If more detail is required:

`mem_get_observation`

If historical evolution matters:

`mem_timeline`

---

## User or cross-project question

Examples:

* What Java style does the user prefer?
* Does the user normally want tests added?
* What architecture style does the user usually prefer?
* How does the user like technical explanations?
* Is there a recurring convention used across repositories?

Use:

`mnemosyne_recall`

---

## Ambiguous question

If it could involve both:

Search Engram first when currently inside a repository.

Then use Mnemosyne only if broader user context would improve the decision.

Do not query both systems mechanically.

---

# 6. Search Before Re-Investigating

Before spending significant time rediscovering:

* previous bugs;
* architectural decisions;
* unusual configuration;
* framework limitations;
* database behavior;
* implementation history;
* failed approaches;

search Engram.

Before asking the user again about a stable development preference that may already be known:

search Mnemosyne.

Memory should reduce repeated investigation and repeated questions.

---

# 7. Saving to Engram

Use:

`mem_save`

after durable PROJECT knowledge is discovered.

Good Engram memories include:

* architecture decisions;
* bug fixes;
* root causes;
* important discoveries;
* configuration decisions;
* reusable repository patterns;
* project constraints;
* non-obvious behavior;
* failed approaches worth avoiding.

Do not save routine activity.

Engram is not a tool-call log.

Do NOT save:

* every file opened;
* every command executed;
* trivial edits;
* formatting;
* imports;
* temporary debugging;
* ordinary test output;
* speculative hypotheses;
* status updates.

---

# 8. Engram Memory Format

Prefer structured, concise observations.

Use:

**What:** what was decided, fixed, or discovered.

**Why:** why it matters.

**Where:** relevant files, classes, modules, services, tables, or configuration.

**Learned:** caveats, limitations, failed approaches, or future implications.

Example:

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

---

# 9. Evolving Engram Knowledge

Project architecture evolves.

When an existing project decision changes, avoid creating disconnected contradictory observations.

Use stable `topic_key` values when appropriate.

Examples:

`architecture/authentication`

`architecture/database-access`

`architecture/session-management`

`config/keycloak`

`pattern/error-handling`

`decision/api-pagination`

Use:

`mem_suggest_topic_key`

when a stable key is needed.

Use:

`mem_update`

when an existing observation should be corrected or refined.

If Engram detects a possible conflict and requests judgment, use:

`mem_judge`

after validating the current repository evidence.

---

# 10. Saving to Mnemosyne

Use:

`mnemosyne_remember`

only for durable information that should remain useful beyond the current repository.

Good Mnemosyne memories include statements such as:

`User generally prefers constructor injection in Spring projects.`

`User prefers examples using Java when discussing backend patterns.`

`User prefers technical explanations that include concrete implementation examples.`

`User normally wants local-first development tools when possible.`

`User prefers avoiding unnecessary dependencies.`

These are cross-project preferences or recurring working patterns.

---

# 11. Do Not Put Project Details in Mnemosyne

Avoid memories such as:

`ProductoService currently has an N+1 issue.`

`This repository uses PostgreSQL 18.`

`AuthenticationController was refactored today.`

`The payment API timeout was caused by RetryService.`

These belong in Engram.

Otherwise Mnemosyne eventually becomes polluted with details from unrelated repositories.

---

# 12. Do Not Put General User Memory in Engram

Avoid using Engram as the primary store for statements such as:

`User prefers Java examples.`

`User likes detailed architecture explanations.`

`User generally prefers local tools.`

These belong in Mnemosyne when they are durable and genuinely useful across projects.

Project-specific exceptions may still belong in Engram.

---

# 13. Cross-Project Knowledge vs Project Knowledge

Use this test:

Ask:

> Would this fact still matter if the user opened a completely different repository tomorrow?

If NO:

→ Engram.

If YES:

Ask:

> Is this a durable user preference, recurring workflow rule, or cross-project context?

If YES:

→ Mnemosyne.

If neither:

→ Do not persist it.

---

# 14. One Fact, One Owner

Every durable fact should have a primary owner.

Use:

PROJECT FACT
→ Engram

USER / CROSS-PROJECT FACT
→ Mnemosyne

TEMPORARY FACT
→ Do not persist

SECRET
→ Never persist

This rule is mandatory.

---

# 15. Handling Information That Contains Both

Sometimes one user statement contains both general and project-specific information.

Example:

> I normally prefer WebFlux, but this application must stay MVC because it runs on Spring Boot 1.4.1.

Split the knowledge.

Mnemosyne:

`User generally prefers reactive/WebFlux approaches when technically appropriate.`

Engram:

`This project must remain Spring MVC because it runs on Spring Boot 1.4.1 and migration is outside the current scope.`

Do not copy the complete statement into both memories.

Extract the appropriate fact for each system.

---

# 16. Conflict Between Engram and Mnemosyne

A cross-project preference may conflict with a project constraint.

Example:

Mnemosyne:

`User prefers constructor injection.`

Engram:

`Legacy framework in this project requires the existing field-injection pattern to remain unchanged.`

For the current repository:

Engram's project constraint wins.

Do NOT delete the Mnemosyne preference.

It may still apply to other projects.

Explain or apply the local exception when relevant.

---

# 17. Outdated Mnemosyne Preferences

If the user explicitly changes a durable preference, the new explicit instruction takes precedence immediately.

Example:

Old:

`User prefers Maven.`

New user instruction:

`From now on I prefer Gradle.`

The current user instruction wins.

Update the persistent knowledge using the Mnemosyne tools available at runtime.

If replacing old memory requires `mnemosyne_forget`, only remove the old memory when the target is unambiguous.

Then store the new preference using:

`mnemosyne_remember`

Do not perform broad deletion.

---

# 18. Mnemosyne Forget Policy

Treat:

`mnemosyne_forget`

as a destructive operation.

Use it conservatively.

Appropriate cases:

* user explicitly requests forgetting;
* incorrect memory was stored;
* a stable preference was explicitly replaced;
* sensitive information was accidentally persisted;
* an exact harmful duplicate can safely be identified.

Do not delete memory merely because it is old.

Do not broadly delete memories based on vague matches.

---

# 19. Session State Belongs to Engram

Use Engram for repository session continuity.

Before finishing meaningful repository work, use:

`mem_session_summary`

Capture:

**Goal**

What was being attempted.

**Accomplished**

What was completed.

**Decisions**

Important decisions.

**Discoveries**

Non-obvious knowledge.

**Current State**

Where implementation stands.

**Remaining Work**

What remains.

**Next Steps**

Recommended continuation.

**Important Locations**

Relevant classes, files, modules, configuration, or services.

Mnemosyne should NOT receive a duplicate session summary.

---

# 20. After Context Compaction

If conversation context is compacted or lost while working in a repository:

First:

`mem_context`

Then, if necessary:

`mem_search`

Recover project state from Engram.

Only call:

`mnemosyne_recall`

if cross-project/user context is also required.

Do not reconstruct previous decisions from guesses.

---

# 21. Sensitive Information

Never persist secrets in either memory system.

Never store:

* passwords;
* API keys;
* access tokens;
* refresh tokens;
* private keys;
* cookies;
* session secrets;
* database credentials;
* `.env` secret values;
* verification codes.

Store only the non-secret conclusion when useful.

Good:

`Payment integration requires PAYMENT_API_TOKEN.`

Bad:

`PAYMENT_API_TOKEN=...`

---

# 22. Memory Quality Rule

Before every persistent write, ask two questions.

### Question 1

Will another session benefit from this information?

If NO:

Do not save.

### Question 2

Who owns this knowledge?

If it belongs to this repository:

→ Engram.

If it follows the user across repositories:

→ Mnemosyne.

Do not save without answering both.

---

# 23. Operational Strategy

Use this default workflow:

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

Do not automatically execute every step when it is unnecessary.

Memory operations should support development, not dominate it.

---

# 24. Tool Responsibilities

## Engram

Use primarily:

`mem_current_project`
→ identify current repository memory namespace.

`mem_context`
→ recover recent repository context.

`mem_search`
→ search previous project knowledge.

`mem_get_observation`
→ retrieve full project memory.

`mem_timeline`
→ inspect project-memory history.

`mem_save`
→ persist project engineering knowledge.

`mem_update`
→ evolve existing project knowledge.

`mem_suggest_topic_key`
→ establish stable evolving topics.

`mem_judge`
→ resolve detected memory conflicts.

`mem_session_summary`
→ leave a repository handoff.

---

## Mnemosyne

Use:

`mnemosyne_recall`
→ recover durable user or cross-project context.

`mnemosyne_remember`
→ preserve durable user or cross-project knowledge.

`mnemosyne_forget`
→ explicitly and conservatively remove obsolete or unwanted memory.

Do not assume additional Mnemosyne MCP tools exist unless they are actually exposed by the runtime.

---

# 25. Final Memory Model

Think about the systems this way:

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

Engram is the project's engineering memory.

Mnemosyne is the agent's durable cross-project understanding of the user.

Do not duplicate them.

Use each memory system only for the knowledge it is best suited to preserve.

---

# Core Rule

Before rediscovering project knowledge:

**Search Engram.**

Before asking again about a durable user preference:

**Recall Mnemosyne.**

After learning something important about the repository:

**Save to Engram.**

After learning something durable about how the user works across repositories:

**Remember with Mnemosyne.**

At the end of meaningful repository work:

**Summarize with Engram.**

One fact. One owner. Minimal duplication.
