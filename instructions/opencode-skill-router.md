# OpenCode Skill Router

## Scope

This router selects and sequences installed `mattpocock/skills`. It owns WHICH SKILL, WHEN, IN WHAT ORDER, WHAT DEPENDS ON WHAT, WHEN CONTROL RETURNS, and WHEN TO STOP. Each selected skill owns its `HOW`.

```text
WHICH SKILL
WHEN
IN WHAT ORDER
WHAT DEPENDS ON WHAT
WHEN CONTROL RETURNS
WHEN TO STOP
```

```text
HOW
```

Never duplicate a `SKILL.md`, copy skill steps, preload unrelated skills, or select by name alone.

## Skill Classes

User-invoked/orchestration skills:

- `ask-matt`, `grill-with-docs`, `triage`, `improve-codebase-architecture`
- `setup-matt-pocock-skills`, `to-spec`, `to-tickets`, `implement`, `wayfinder`
- `grill-me`, `handoff`, `teach`, `to-questionnaire`, `wait-what`

The router may select these automatically when user intent clearly matches. A user-invoked skill returns to the router before another user-invoked skill starts:

Invalid direct chain:

```text
user-invoked skill
-> another user-invoked skill
```

```text
user-invoked skill
-> ROUTER
-> next user-invoked skill
```

Model-invoked skills:

- `prototype`, `diagnosing-bugs`, `research`, `tdd`, `domain-modeling`
- `codebase-design`, `code-review`, `resolving-merge-conflicts`, `wizard`
- `grilling`, `writing-for-agents`

The router may select these directly. An active orchestration skill may use one as a dependency when its `SKILL.md` requires it, then regain control. Never duplicate a dependency already delegated by the active skill.

```text
user-invoked skill
    -> model-invoked skill
    -> same user-invoked skill
```

Never delegate one user-invoked skill from another:

```text
user-invoked skill
    -> user-invoked skill
```

## Instruction Dependencies

`SOLID Development Rules` controls engineering-quality constraints for design,
implementation, refactoring, testing, debugging, and review. Read and apply
`instructions/solid-development-rules.md` before invoking
`improve-codebase-architecture`, `domain-modeling`, `codebase-design`,
`implement`, `tdd`, `diagnosing-bugs`, or `code-review`.

For direct refactoring, use these observable branches:

- Seam or interface refactoring: apply `SOLID Development Rules`, then invoke
  `codebase-design`.
- Behavior-changing refactoring: apply `SOLID Development Rules`, then invoke
  `tdd`.
- Behavior-preserving direct refactoring: apply `SOLID Development Rules`,
  perform the refactoring under those rules, then return to the router without inventing a task skill.

```text
ROUTER
-> SOLID Development Rules
-> task-specific skill
-> ROUTER
```

This dependency wraps the selected execution step; it does not replace its
route or own its `HOW`. Do not activate it for unrelated writing, teaching,
research-only, setup, handoff, or questionnaire routes.

## Routing Loop

For every request:

1. Determine the user's objective and current work state.
2. Inspect skills actually available in OpenCode.
3. Apply priority and choose the shortest valid route.
4. Invoke only the next required skill and let its `SKILL.md` execute.
5. On completion, return control to the router and re-evaluate the remaining objective.
6. Stop when the objective is complete; otherwise invoke only the next missing phase.

```text
USER REQUEST
     |
     v
CLASSIFY INTENT
     |
     v
DETECT CURRENT STATE
     |
     v
SELECT MINIMAL ROUTE
     |
     v
INVOKE NEXT SKILL
     |
     v
SKILL EXECUTES ITS OWN SKILL.md
     |
     v
SKILL FINISHES
     |
     v
RETURN TO ROUTER
     |
     v
OBJECTIVE COMPLETE?
   /       \
 YES        NO
  |          |
 STOP        v
        SELECT NEXT SKILL
```

Never restart completed phases. Continue from current state across follow-up messages unless the objective changes, a skill completes, its instructions require a transition, or new evidence changes the task class.

## Priority

Apply top to bottom:

```text
1. explicit skill requested by user
2. resolving-merge-conflicts
3. diagnosing-bugs
4. code-review
5. wizard
6. triage
7. implement
8. tdd
9. wayfinder
10. grill-with-docs / grill-me
11. architecture/domain/design skills
12. research / prototype
13. productivity/documentation skills
14. no skill
```

A specific active state beats a broad workflow. Example:

```text
"Implement this ticket, but my rebase has conflicts"
-> resolving-merge-conflicts
-> ROUTER
-> implement
```

## Route Table

| Intent or state | Route |
|---|---|
| User asks which skill or flow fits | `ask-matt` |
| Repository change has unresolved decisions | `grill-with-docs` |
| Non-repository plan or idea needs grilling | `grill-me` |
| Incoming issue or external PR needs classification or verification | `triage` |
| Broad architecture/codebase assessment | `improve-codebase-architecture` |
| Specific module, interface, seam, or testability design | `codebase-design` |
| Domain terminology or model is the problem | `domain-modeling` |
| Required Matt repository setup is missing | `setup-matt-pocock-skills` |
| Existing discussion should become a spec | `to-spec` |
| Existing plan or spec should become implementation tickets | `to-tickets` |
| Existing spec or ticket should be implemented | `implement` |
| Work is both huge and unclear beyond one session | `wayfinder` |
| Concrete feature or proven fix should be implemented test-first | `tdd` |
| Bug, failure, regression, or slowness has unknown cause | `diagnosing-bugs` |
| Authoritative external facts are required | `research` |
| Runnable or visual evidence is required | `prototype` |
| User requests code, diff, PR, or branch review | `code-review` |
| Git has unresolved merge or rebase conflicts | `resolving-merge-conflicts` |
| A human-only operation blocks progress | `wizard` |
| Another person owns missing information | `to-questionnaire` |
| Context must move to another agent, session, person, or directory | `handoff` |
| User wants persistent multi-session learning | `teach` |
| Previous explanation did not land | `wait-what` |
| Agent-facing instructions or documents are requested | `writing-for-agents` |
| Another workflow requires the reusable interview primitive | `grilling` |
| No route matches | no skill |

## Main Routes

Enter the first missing phase, never every phase by default:

```text
ROUTER
-> grill-with-docs
-> ROUTER
-> to-spec
-> ROUTER
-> to-tickets
-> ROUTER
-> implement
```

Skills never chain this route directly:

```text
grill-with-docs
-> to-spec
-> to-tickets
-> implement
```

The complete review route retains router boundaries:

```text
grill-with-docs
-> ROUTER
-> to-spec
-> ROUTER
-> to-tickets
-> ROUTER
-> implement
-> ROUTER
-> code-review
```

Direct entries:

```text
idea unclear
-> grill-with-docs
```

```text
decisions already settled
-> to-spec
```

```text
spec already exists
-> to-tickets
```

```text
tickets already exist
-> implement
```

```text
implementation already exists
-> code-review
```

```text
small concrete change
-> tdd
```

`wayfinder` route:

```text
wayfinder
```

Use only when both conditions hold:

```text
work is too large for one normal session
AND
the route is still unclear
```

It returns before subsequent routing:

```text
wayfinder
-> ROUTER
```

Then select the next missing phases through router boundaries:

```text
to-spec
-> ROUTER
-> to-tickets
-> ROUTER
-> implement
```

Bug routing:

```text
cause unknown
-> diagnosing-bugs
```

```text
cause known + direct fix
-> tdd
```

```text
cause known + existing spec/ticket
-> implement
```

After `diagnosing-bugs`, return to the router. Continue to a fix only when the user's objective includes fixing it.

```text
diagnosing-bugs
-> ROUTER
```

Architecture routing:

```text
improve-codebase-architecture
```

```text
codebase-design
```

A broad scan may progress through the router into focused design:

```text
improve-codebase-architecture
-> ROUTER
-> codebase-design
```

After design, use `grill-with-docs` if material decisions remain or `tdd` if implementation is concrete. Use `domain-modeling` only when domain terminology/model decisions are themselves the task.

```text
codebase-design
-> ROUTER
-> grill-with-docs
```

```text
codebase-design
-> ROUTER
-> tdd
```

Domain modeling can stand alone or return to its parent:

```text
domain-modeling
```

```text
parent workflow
-> domain-modeling
-> parent workflow
```

Interview routing:

```text
grill-with-docs
```

```text
grill-me
```

```text
grilling
```

Never chain `grill-me` and `grill-with-docs`.

Triage routes incoming work back to the router:

```text
incoming issue/PR
-> triage
-> ROUTER
```

Continue to implementation only if requested:

```text
triage
-> ROUTER
-> implement
```

Never send tickets generated by `to-tickets` through `triage`.

## Dependencies And Interrupts

Standalone research terminates:

```text
research
-> STOP
```

Dependency research, prototype, or domain modeling returns to its parent workflow:

```text
parent workflow
-> research
-> parent workflow
```

Standalone prototype and dependency prototype routes:

```text
prototype
-> ROUTER
```

```text
parent workflow
-> prototype
-> parent workflow
```

A prototype is not production implementation.

Before a flow requiring Matt repository configuration, check setup. If missing, run `setup-matt-pocock-skills`, return to the router, and resume the original route. Run setup once per repository, not per task.

```text
IF setup is missing
    -> setup-matt-pocock-skills
    -> ROUTER
    -> resume original route
ELSE
    -> continue original route
```

When selecting `implement`, let it own its internal dependencies. Do not automatically wrap it with `tdd` or `code-review`; invoke either later only as a separate phase still required after `implement` returns.

```text
implement
```

```text
implement
-> ROUTER
```

Do not impose this chain on `implement`:

```text
tdd
-> implement
-> code-review
```

Merge/rebase conflict interrupt:

```text
current workflow
-> resolving-merge-conflicts
-> ROUTER
```

Resume from current state, never restart the original workflow.

Human-only interrupt:

```text
current workflow
-> wizard
```

Return to the router if execution can continue; otherwise stop.

If another human owns required information:

```text
to-questionnaire
-> STOP
```

When answers arrive, re-enter the router. Never invent missing information.

```text
ROUTER
```

Use `handoff` only across a real context boundary:

```text
handoff
-> STOP
```

Never make handoff an automatic final phase.

Persistent learning:

```text
teach
```

One-off explanation:

```text
NO SKILL
```

Failed prior explanation:

```text
wait-what
```

Agent-facing document targets include:

```text
AGENTS.md
CLAUDE.md
SKILL.md
agent routing instructions
agent workflow instructions
```

Route those targets to:

```text
writing-for-agents
```

## State And Termination

Do not repeat setup, alignment, spec creation, ticket creation, diagnosis, research, or review unless new information invalidates the result or the user explicitly requests repetition.

An explicitly requested installed skill runs first and returns to the router. Continue only if the original objective still needs another phase.

```text
requested skill
-> ROUTER
```

Use no skill for simple factual or syntax questions, short or one-off explanations, ordinary conversation, trivial code questions, and tasks outside the installed skill set. Persistent learning uses `teach`; a one-off explanation uses NO SKILL; a failed prior explanation uses `wait-what`.

Minimal routes:

```text
idea only
-> grill-with-docs

alignment complete
-> to-spec

spec exists
-> to-tickets

tickets exist
-> implement

implementation exists
-> code-review

bug cause unknown
-> diagnosing-bugs

bug cause known
-> tdd or implement
```

```text
"Review this PR"
-> code-review
```

Avoid direct user-skill chaining:

```text
grill-with-docs
-> to-spec
-> to-tickets
-> implement
-> code-review
```

```text
"Implement this ticket"
-> implement
```

```text
grill-with-docs
-> to-spec
-> to-tickets
-> implement
```

```text
"Research this API behavior"
-> research
```

```text
grill-with-docs
-> research
-> to-spec
```

Stop when the objective is satisfied. The router decides routing only; never copy a skill's `HOW` into this router.
