# Laya — Structured Decision Engine

## Purpose

Laya is a specialized decision engine for fast structured classification,
routing, scoring, ranking, triage, and yes/no decisions.

Use Laya only when the task contains a genuine decision that benefits from
evaluating defined alternatives or explicit criteria.

Laya complements the other tools. It does not replace specialized tools for
code exploration, documentation, memory, web research, terminal execution,
bulk processing, or vision.

---

## Core Rule

Deterministic routing beats Laya.

If an existing instruction clearly determines which specialized tool should be
used, call that tool directly.

Do not call Laya merely to confirm an obvious tool choice.

---

## Existing Tool Ownership

Use the existing specialized tools according to their own instructions.

Typical ownership:

- Local indexed repository → CodeGraph
- Durable project knowledge or memory → Engram
- Large or noisy context processing → context-mode
- CLI, build, test, or runtime output → RTK
- External library or API documentation → Context7
- Unknown or current public web information → Web Search Prime
- Known web page content → Web Reader
- Public GitHub repository → Zread
- Images, screenshots, diagrams, or visual input → Z.AI Vision

These routes are deterministic and normally do not require Laya.

---

## Automatic Invocation

When Laya tools are available, invoke Laya automatically when the current task
clearly matches the criteria in this instruction.

Do not ask the user for permission before using Laya.

Do not require the user to explicitly say "use Laya".

Do not mention Laya unless doing so is useful to the final answer.

Do not invoke Laya when the answer or route is already clear from explicit
instructions, current evidence, source code, tests, runtime output, or
authoritative documentation.

---

## Use Laya When

Use Laya when there is a genuine structured decision, including:

- choosing among multiple defined alternatives;
- classifying an input into known categories;
- triaging an issue into possible investigation areas;
- scoring candidates against explicit criteria;
- comparing strategies when several are technically valid;
- making a yes/no decision from supplied evidence;
- selecting a strategy when no deterministic routing rule already applies;
- applying a reusable decision preset or guardrail;
- prioritizing alternatives when the criteria are explicitly provided.

Examples:

- Redis vs Caffeine vs Hazelcast for a defined set of requirements.
- Classifying an incident as application, infrastructure, authentication,
  database, or network related.
- Scoring multiple implementation approaches against latency, complexity,
  maintainability, and operational cost.
- Deciding whether a migration should proceed based on defined conditions.

---

## Do Not Use Laya For

Do not use Laya for:

- open-ended question answering;
- writing or generating normal prose;
- generating or editing source code;
- repository exploration;
- locating files, symbols, references, or execution paths;
- documentation lookup;
- web search;
- reading normal web pages;
- persistent memory;
- terminal execution;
- processing large logs or bulk output;
- image or video understanding;
- deterministic tool routing;
- verifying facts that can be checked directly from authoritative evidence.

Use the specialized tool that owns the task instead.

---

## Decision Authority

Evidence has higher priority than prediction.

Use the following priority order:

1. Explicit user instructions
2. Explicit system or project routing rules
3. Repository source code and configuration
4. Compiler, test, build, or runtime evidence
5. Authoritative documentation
6. Laya decision output
7. Model inference

If Laya conflicts with stronger evidence, ignore the Laya decision and follow
the stronger evidence.

Laya must never override explicit user instructions or deterministic tool
routing.

---

## Tool Selection

Use Laya tools according to their specific responsibility.

### `laya_predict`

Use for structured decisions such as:

- choice;
- classification;
- scoring;
- ranking;
- yes/no decisions;
- triage.

Prefer explicit alternatives and explicit decision criteria.

### `laya_route`

Use only for routing capabilities defined by Laya itself.

Do not treat `laya_route` as the global tool router for the agent.

Do not call `laya_route` before every MCP or tool call.

The agent's normal instruction-based routing remains authoritative.

### `laya_preset`

Use when a supported reusable Laya decision preset clearly matches the current
task.

Prefer a preset over recreating the same decision structure repeatedly when the
preset is appropriate.

### `laya_status`

Use only when Laya runtime, availability, health, configuration, or capability
information is actually needed.

Do not call it as a routine preflight check.

---

## Execution Pattern

When Laya is appropriate, use this pattern:

1. Gather enough evidence for the decision.
2. Define the alternatives or categories.
3. Define the relevant criteria.
4. Call Laya once for that decision stage.
5. Use the result to select the next strategy.
6. Continue with the specialized tool that owns the actual work.
7. Validate important conclusions using source code, tests, runtime evidence,
   or authoritative documentation when available.

Example:

User request:

"Which cache should I use: Redis, Caffeine, or Hazelcast? I have five
application pods and need shared cache state with low operational complexity."

Preferred flow:

Requirements
→ Laya structured choice
→ selected strategy
→ Context7 for authoritative documentation
→ CodeGraph for repository integration
→ RTK for build and tests

Laya makes the structured decision.

The specialized tools perform and validate the actual work.

---

## Efficiency

Use Laya only when it adds decision value.

Do not call Laya when:

- there is only one reasonable option;
- an explicit routing rule already determines the tool;
- source code already answers the question;
- tests or runtime output already establish the cause;
- authoritative documentation directly answers the question;
- the task is simply retrieval, generation, or execution.

Normally use no more than one Laya call per decision stage.

Batch closely related criteria into one structured decision when possible
instead of making many independent calls.

---

## Avoid Meta-Routing Loops

Never use Laya to decide whether Laya itself should be used.

Do not recursively route decisions between Laya and another routing tool.

Once a specialized tool has been selected, continue using it until new
information creates a genuinely different decision point.

Avoid patterns such as:

Laya
→ CodeGraph
→ Laya
→ CodeGraph

Prefer:

Laya
→ CodeGraph
→ evidence
→ implementation or conclusion

Only return to Laya if a new, independent structured decision appears.

---

## Routing Ambiguity

Use Laya for routing only when:

- there are multiple valid strategies;
- existing instructions do not already determine the route;
- the decision can be expressed using explicit alternatives or categories;
- choosing incorrectly would materially affect the next step.

Do not use Laya for simple routing such as:

- local code → CodeGraph;
- public GitHub → Zread;
- library documentation → Context7;
- current public information → Web Search Prime;
- known web page → Web Reader;
- large logs → context-mode or RTK according to their instructions;
- images → Z.AI Vision.

---

## Triage Example

Given:

"After deployment some requests return 401, others 403, and authentication uses
Keycloak JWT."

Laya may classify the likely investigation area among:

- Spring Security configuration;
- JWT claims or validation;
- Keycloak roles or mappers;
- gateway or infrastructure;
- application authorization logic.

After classification:

- local implementation → CodeGraph;
- Spring/Keycloak documentation → Context7;
- current external issue → Web Search Prime;
- large runtime logs → RTK or context-mode.

---

## Final Principle

Laya is a decision layer, not a universal tool.

Use it automatically when a genuine structured decision exists.

Skip it when routing is already deterministic.

Specialized tools gather evidence and perform work.

Evidence always wins.
