# SOLID Development Rules

Use these rules for software design, implementation, refactoring, testing, debugging, and code review.

## 1. Applicability

Identify which task branches apply before acting: design, implementation,
refactoring, testing, debugging, or review. Apply concrete patterns only when
they match repository conventions.

Complete this gate when every applicable branch is explicit and no unrelated
branch is included.

## 2. Evidence

Inspect current code, tests, runtime behavior, configuration, and repository
conventions. State the concrete change pressure or demonstrated design problem.
Do not treat a hypothetical future requirement as evidence.

Complete this gate when current repository or runtime evidence identifies the
problem, constraint, and affected boundary.

## 3. Minimal Design

Choose the smallest design that satisfies current behavior and constraints.
Prefer existing seams and direct code over new indirection. Every proposed
abstraction must have an immediate responsibility.

Complete this gate when the design addresses only evidenced needs and every
abstraction can be tied to work required by the current change.

## 4. Test-Driven Development

Red-Green-Refactor is mandatory for features, bug fixes, and other behavior
changes:

1. Add or change a test that fails for the intended reason.
2. Write the smallest implementation that makes it pass.
3. Refactor only while relevant tests remain green.

TDD is not required for documentation-only or formatting-only changes that do
not alter behavior.

Complete this gate when either:

- the failing test proves the intended behavior, the smallest implementation
  passes it, and relevant regression tests remain green; or
- TDD is marked not applicable for a documentation-only or formatting-only
  change, with a concise reason why behavior is unchanged.

## 5. SOLID Pass

Evaluate applicable classes, functions, modules, and interfaces. Fix evidenced
violations; do not create abstractions only to make the design appear SOLID.

### Single Responsibility

Apply SRP when a unit combines distinct responsibilities that change for
different reasons. Keep one cohesive responsibility and one primary reason to
change.

Complete when each affected unit has one stated responsibility, and each
changed operation supports that responsibility.

### Open/Closed

Apply OCP when a concrete variation must extend stable behavior. Use an
existing seam when one fits; introduce a seam only when present variation
requires it.

Complete when the evidenced variation is added without unnecessary changes to
stable behavior, and tests prove both existing and new cases.

### Liskov Substitution

Apply LSP when callers can use multiple implementations through the same
contract. Preserve caller-visible behavior, invariants, accepted inputs,
outputs, side effects, and error behavior.

Complete when contract tests or equivalent caller evidence show every affected
implementation can substitute without caller changes or weakened guarantees.

### Interface Segregation

Apply ISP when consumers depend on operations they do not use or cannot
support. Keep consumer-facing contracts limited to required operations.

Complete when each affected consumer depends only on operations it uses, with
no placeholder, unsupported, or irrelevant implementation requirements.

### Dependency Inversion

Apply DIP when policy crosses a real boundary to an unstable detail, such as an
external service, storage mechanism, platform API, or replaceable adapter. Do
not invert stable local dependencies without concrete pressure.

Complete when policy can be tested independently of the unstable detail, the
boundary contract reflects policy needs, and dependency direction is visible
in code.

Complete the SOLID pass when every applicable principle meets its criterion.
Mark an unaffected principle not applicable when the changed unit does not involve that principle's boundary, with a concise reason.

## 6. Complexity Gate

YAGNI overrides speculative abstraction. Reject interfaces, factories, layers,
extension points, and indirection created only for hypothetical future needs.
Permit a production abstraction only when present change pressure identifies a
production responsibility or boundary, or demonstrated reuse requires it. Test isolation may support that evidence but cannot justify production abstraction alone.

Complete this gate when every remaining abstraction has a cited current use and
removing any remaining indirection would break an evidenced requirement,
production responsibility or boundary, or existing reuse.

## 7. Verification

Run relevant tests, formatting, linting, builds, and final diff review. Match
the repository's established commands. Document every skipped check with its
reason and residual risk.

Complete this gate when every executed check passes, each skipped check has a
documented reason and residual risk, the diff contains only intended changes,
and no regression is unexplained.

## 8. Completion

Finish only when current evidence proves behavior, relevant tests pass,
complexity is justified, repository conventions are preserved, and no
speculative abstraction remains.

Complete this gate when every prior gate has its observable evidence recorded
and the current change has no unresolved failure, unsupported assumption, or
unintended modification.
