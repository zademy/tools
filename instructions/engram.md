# Engram Persistent Memory Protocol

Engram is the persistent memory system for this agent.

Use Engram proactively to preserve useful knowledge across sessions.

The purpose of Engram is not to log everything the agent does. Its purpose is to preserve information that will help future work avoid repeated investigation and recover project context quickly.

## Core behavior

Follow this cycle:

1. **Recover** relevant context before starting work.
2. **Search** memory before repeating investigation.
3. **Work** using the current repository as the primary source of truth.
4. **Save** important discoveries, decisions, fixes, and constraints.
5. **Update** memories when knowledge changes.
6. **Summarize** meaningful sessions before finishing.

Think of Engram as durable engineering memory, not as a transcript.

---

# Starting a session

At the beginning of work, identify the current project using:

`mem_current_project`

Then recover recent project context using:

`mem_context`

Use the recovered context to understand:

* previous work;
* architectural decisions;
* unfinished tasks;
* recent bug fixes;
* known limitations;
* important discoveries;
* project conventions;
* next recommended steps.

Do not assume that remembered information is still correct.

The current repository and current user instructions always have higher priority than stored memory.

---

# Recovering previous knowledge

Before investigating a problem that may have been handled previously, search Engram.

Use:

`mem_search`

Good search targets include:

* class names;
* service names;
* modules;
* configuration names;
* bugs;
* architectural concepts;
* database decisions;
* API decisions;
* framework behavior;
* error messages;
* previously attempted solutions.

Prefer short and meaningful search queries.

Do not search using the entire user prompt unless necessary.

Example:

Instead of:

`"The user says authentication is failing when trying to login and wants to know what we did last time"`

Prefer:

`authentication login failure`

or:

`Keycloak authentication`

---

# Progressive retrieval

Do not retrieve large amounts of memory unnecessarily.

Use progressive retrieval:

1. `mem_context`
2. `mem_search`
3. `mem_get_observation`
4. `mem_timeline`

Use `mem_get_observation` when a search result appears relevant and the complete stored observation is needed.

Use `mem_timeline` when understanding the chronological evolution of a decision, bug, implementation, or investigation is useful.

Retrieve only enough information to perform the current task.

---

# When to save memory

Use:

`mem_save`

Save information when it is likely to help future work.

Good candidates include:

## Decisions

Save architectural or technical decisions.

Examples:

* choosing JDBC instead of JPA for a particular operation;
* choosing a specific authentication strategy;
* defining how pagination works;
* selecting a caching strategy;
* establishing transaction boundaries.

## Bug fixes

Save meaningful fixes when the root cause is not obvious.

Include:

* symptom;
* root cause;
* solution;
* important caveats.

## Discoveries

Save non-obvious facts discovered while investigating the codebase.

Examples:

* a library behaves differently than expected;
* a service depends on hidden configuration;
* a database column has unusual semantics;
* a framework feature has an important limitation.

## Patterns

Save reusable project conventions.

Examples:

* error handling pattern;
* DTO mapping convention;
* repository query convention;
* testing strategy;
* logging convention.

## Configuration

Save important configuration knowledge.

Examples:

* required Spring configuration;
* database behavior;
* build configuration;
* environment requirements.

Do not store secret values.

## Constraints

Save limitations that future agents must know.

Examples:

* Java version cannot be upgraded;
* REST endpoints cannot be introduced;
* a specific dependency must remain compatible with a legacy framework;
* a module must preserve backward compatibility.

---

# What NOT to save

Do not use Engram as a log.

Do NOT save:

* every command executed;
* every file opened;
* every code edit;
* trivial refactors;
* formatting changes;
* import changes;
* temporary debug statements;
* compilation output;
* routine test results;
* obvious information directly visible in the code;
* speculative ideas;
* hypotheses that were immediately disproven;
* repetitive status updates;
* conversation filler.

A memory should provide future value.

Before saving, ask:

> Would another agent benefit from knowing this in a future session?

If the answer is no, do not save it.

---

# Memory quality

Memories should be:

* concise;
* factual;
* searchable;
* project-specific when appropriate;
* focused on knowledge rather than activity.

Prefer recording:

**What**

What was discovered, changed, fixed, or decided.

**Why**

Why it matters or why the decision was made.

**Where**

Relevant files, classes, modules, components, services, or configuration.

**Learned**

Important caveats, failed approaches, edge cases, or future considerations.

Example structure:

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

---

# Memory titles

Use short, descriptive, searchable titles.

Good:

`Fixed N+1 in product search`

`Authentication roles come from Keycloak resource_access`

`Use JDBC for bulk report queries`

`Oracle timeout caused by missing index`

`Retiro flow state stored in SessionScope`

Bad:

`Important`

`Changes`

`Bug`

`Fixed issue`

`New information`

The title should help future `mem_search` operations.

---

# Memory types

Use the most accurate memory type available.

Typical types:

* `bugfix`
* `decision`
* `architecture`
* `discovery`
* `pattern`
* `config`
* `preference`

Prefer project-scoped memories for repository-specific information.

---

# Evolving decisions

Some knowledge changes over time.

Examples:

* authentication architecture;
* database strategy;
* caching strategy;
* API design;
* deployment architecture.

Avoid creating unrelated duplicate memories for the same evolving topic.

Use a stable `topic_key`.

If necessary, obtain a suitable topic key using:

`mem_suggest_topic_key`

Example topic keys:

`architecture/authentication`

`architecture/database`

`architecture/session-management`

`config/keycloak`

`pattern/error-handling`

`decision/api-pagination`

`decision/database-access`

Reuse the same topic key when the same conceptual decision evolves.

---

# Updating existing knowledge

When an existing observation is known and needs correction or refinement, use:

`mem_update`

Prefer updating existing knowledge instead of creating contradictory duplicates.

Use updates when:

* implementation changed;
* previous assumptions became incorrect;
* a decision was replaced;
* additional evidence clarified an earlier memory;
* a bug fix evolved.

Historical information may still be useful, so do not erase meaningful context unnecessarily.

---

# Handling conflicting memories

Engram may detect that a new memory conflicts with existing knowledge.

If Engram requests judgment, inspect the candidates.

Use:

`mem_judge`

Evaluate whether memories are:

* compatible;
* unrelated;
* conflicting;
* scoped differently;
* superseding one another.

Do not automatically prefer newer information.

Validate against:

1. current repository state;
2. current configuration;
3. current user instructions;
4. actual runtime behavior when available.

The repository is the primary source of truth.

Memory is supporting context.

---

# Repository vs memory

Never treat Engram as authoritative when it conflicts with the current codebase.

Priority order:

1. Explicit current user instructions
2. Current repository state
3. Current runtime/test evidence
4. Engram memory
5. Agent assumptions

If memory says one thing and the repository says another, investigate the discrepancy.

If the repository clearly changed, update the relevant memory.

---

# Avoid repeated investigation

One of the main purposes of Engram is to prevent duplicated work.

Before spending significant time investigating:

* an existing bug;
* architecture;
* configuration;
* framework behavior;
* database behavior;
* implementation history;

search memory first.

If useful knowledge already exists, use it as a starting point and validate it against the current repository.

Do not blindly repeat previously failed approaches.

---

# Failed approaches

Failed approaches can be valuable memory when they prevent expensive repeated investigation.

Save them only when they are non-obvious and likely to be attempted again.

Example:

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

Do not store every failed experiment.

---

# Sensitive information

Never persist secrets.

Do not save:

* passwords;
* API keys;
* access tokens;
* refresh tokens;
* private keys;
* cookies;
* session secrets;
* database credentials;
* secrets from environment variables;
* secrets from configuration files.

If a discovery involves a secret, store only the useful architectural conclusion.

Example:

Save:

`Application authentication requires the PAYMENT_API_TOKEN environment variable.`

Do not save the token value.

---

# Context recovery

If conversational context is lost, compacted, reset, or insufficient:

use:

`mem_context`

Then search specific topics using:

`mem_search`

Do not reconstruct previous project decisions from guesses when Engram can recover them.

Use memory to restore:

* current implementation state;
* previous decisions;
* completed work;
* remaining work;
* known issues.

---

# During implementation

Do not call memory tools after every edit.

Work normally.

Save memory at meaningful checkpoints such as:

* root cause discovered;
* architecture decision made;
* important implementation completed;
* important constraint discovered;
* reusable pattern established;
* significant bug fixed.

Memory operations should not interrupt normal development unnecessarily.

---

# Before finishing a meaningful session

Use:

`mem_session_summary`

Create a concise handoff containing:

## Goal

What the session was trying to accomplish.

## Accomplished

What was actually completed.

## Decisions

Important technical or architectural decisions.

## Discoveries

Non-obvious information learned.

## Current state

Where the implementation currently stands.

## Remaining work

What still needs to be completed.

## Next steps

The most logical continuation for the next agent/session.

## Important locations

Relevant files, classes, modules, services, or configuration.

The goal is that another agent can continue the work without reconstructing the entire session.

Do not include the entire conversation.

Do not include trivial implementation details.

---

# Agent memory strategy

Use this simple rule:

Before investigating:

`mem_search`

When starting or recovering:

`mem_context`

When something important is learned:

`mem_save`

When knowledge changes:

`mem_update`

When memories conflict:

`mem_judge`

When historical context matters:

`mem_timeline`

When finishing meaningful work:

`mem_session_summary`

---

# Final principle

Engram should contain the project's durable engineering knowledge.

Code tells the agent:

> How the system works now.

Engram should tell the agent:

> Why it works this way, what was learned previously, what decisions were made, and what should not be rediscovered from scratch.

Keep memory small, useful, factual, searchable, and continuously maintained.
