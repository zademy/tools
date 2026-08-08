# SOLID Development Rules Design

## Goal

Add a stack-agnostic instruction that guides agents through design, implementation, refactoring, testing, and code review using SOLID principles without speculative abstraction.

## Scope

Create `instructions/solid-development-rules.md` and add a concise activation pointer to `instructions/opencode-skill-router.md`.

- Keep the instruction in English.
- Apply it across languages and frameworks.
- Adapt concrete patterns to repository conventions.
- Keep workflow details in the new instruction rather than the router.
- Do not modify `config.json`.

## Execution Flow

### 1. Applicability

Determine whether the request involves software design, implementation, refactoring, testing, debugging, or code review. Complete this step when the applicable branches are explicit.

### 2. Evidence

Inspect the repository's conventions and identify concrete change pressure or a demonstrated design problem. Complete this step with current evidence, not a hypothetical future requirement.

### 3. Minimal Design

Choose the smallest design that satisfies the current behavior and repository constraints. Complete this step when every proposed abstraction has an immediate responsibility.

### 4. Test-Driven Development

Use Red-Green-Refactor for features, bug fixes, and other behavioral changes. Complete this step when the failing test proves the intended behavior, the minimal implementation passes it, and relevant tests remain green. For documentation-only or formatting-only changes, record why behavior is unchanged and mark this gate not applicable.

### 5. SOLID Pass

Evaluate classes, functions, modules, and interfaces against observable criteria:

- **SRP:** Each unit has one cohesive responsibility and one primary reason to change.
- **OCP:** Extend stable behavior through an existing seam only when a concrete variation exists.
- **LSP:** Substitutions preserve the caller-visible contract, invariants, and error behavior.
- **ISP:** Consumers depend only on the operations they use.
- **DIP:** Policy does not depend directly on unstable implementation details when a real boundary exists.

Complete this step when applicable violations are fixed or explicitly shown to be irrelevant.

### 6. Complexity Gate

Apply YAGNI and simplicity before introducing interfaces, factories, layers, extension points, or other indirection. A production abstraction requires an evidenced production responsibility or boundary; test isolation may support that evidence but cannot justify production abstraction alone. Complete this step when every remaining abstraction addresses present change pressure, an evidenced production responsibility or boundary, or demonstrated reuse.

### 7. Verification

Run relevant tests, formatting, linting, builds, and diff review. Complete this step with fresh evidence and no unexplained regression.

### 8. Completion

Finish only when behavior is proven, complexity is justified, repository conventions are preserved, and no speculative abstraction remains.

## Router Integration

Add `SOLID Development Rules` as an instruction dependency for routes that perform design, implementation, refactoring, testing, debugging, or code review.

The router pointer must:

- State what the instruction controls.
- List each distinct activation branch once.
- Trigger the instruction before the task-specific execution skill.
- Return control to the router after the task-specific skill finishes.
- Avoid copying the instruction's internal steps or SOLID definitions.

## Verification

The change is complete when:

1. `instructions/solid-development-rules.md` contains the eight ordered gates.
2. TDD is mandatory for behavioral changes and explicitly exempt for documentation-only and formatting-only changes.
3. Each SOLID principle has an observable criterion.
4. YAGNI overrides speculative abstraction.
5. `instructions/opencode-skill-router.md` activates the instruction for all approved branches without duplicating its workflow.
6. Markdown structure and repository checks pass.
7. No unrelated file is modified.
