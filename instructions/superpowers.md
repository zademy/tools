# Superpowers — OpenCode

Use Superpowers as the primary software-development process layer.

## Core Rule

Before any response or action:

1. Check whether a Superpowers skill applies.
2. Load every relevant skill with OpenCode's native `skill` tool.
3. Follow the loaded skill exactly.
4. Do not explore files, ask clarifying questions, plan, or modify code before
   checking applicable skills.

When loading a skill, announce it briefly:

`Using <skill> to <purpose>.`

Process skills run before implementation or domain-specific skills.

Before entering plan mode, use `brainstorming` unless a design has already
been explicitly approved.

If a skill contains a checklist, create matching items with `todowrite`.

## Skill Routing

### `brainstorming`

Use before:

- Creating features or components.
- Adding or changing behavior.
- Designing architecture.
- Starting creative implementation work.
- Converting an incomplete idea into requirements.

Clarify intent, constraints, alternatives, and acceptance criteria.
For non-trivial work, present the design and obtain approval before planning.

### `systematic-debugging`

Use for:

- Bugs.
- Failed tests.
- Unexpected behavior.
- Performance regressions.
- Build or integration failures.

Find and verify the root cause before proposing or applying a fix.
Do not guess or apply unrelated speculative changes.

### `test-driven-development`

Use when implementing a feature or fixing a bug.

Follow RED-GREEN-REFACTOR:

1. Write the smallest failing test.
2. Run it and confirm the expected failure.
3. Implement the smallest correct change.
4. Run the test and confirm it passes.
5. Refactor while keeping tests green.

Do not claim TDD when implementation was written before the test.

### `writing-plans`

Use after requirements and design are approved for multi-step work.

Plans must include:

- Exact files or modules.
- Small ordered tasks.
- Implementation details.
- Tests and expected results.
- Commands for verification.
- Relevant risks and dependencies.

### `using-git-worktrees`

Use before executing substantial feature plans when isolation is useful.

- Create a separate worktree and branch.
- Follow repository naming conventions.
- Run project setup.
- Verify the baseline before changing code.
- Do not use a worktree when the user or repository explicitly forbids it.

### `executing-plans`

Use when a written plan must be executed in batches with review checkpoints.

Stop at the defined checkpoints and report evidence before continuing.

### `subagent-driven-development`

Use when a written plan contains independent tasks suitable for fresh
subagents.

Each task must receive:

- Complete task context.
- Exact scope.
- Relevant file paths.
- Acceptance criteria.
- Verification requirements.

Review specification compliance before code quality.

### `dispatching-parallel-agents`

Use when two or more tasks are independent and do not share mutable state.

Do not parallelize tasks with ordering, dependency, or conflicting-file
requirements.

### `requesting-code-review`

Use after completing substantial work and before integration.

Review against:

- Approved requirements.
- Implementation plan.
- Correctness.
- Security.
- Tests.
- Existing architecture.
- Unintended changes.

Resolve critical findings before continuing.

### `receiving-code-review`

Use before applying review feedback.

- Verify every suggestion technically.
- Ask for clarification when feedback is ambiguous.
- Reject incorrect feedback with evidence.
- Do not agree performatively or implement blindly.

### `verification-before-completion`

Use before stating that work is complete, fixed, passing, or ready.

- Run the relevant verification commands.
- Inspect actual output and exit codes.
- Confirm changed behavior.
- Report commands not executed or failures encountered.

Evidence before completion claims.

### `finishing-a-development-branch`

Use after implementation and verification are complete.

Present the appropriate integration options:

- Merge locally.
- Create a pull request.
- Keep the branch.
- Discard the work.

Do not merge, delete, or discard without user authorization.

### `writing-skills`

Use whenever creating, editing, or validating a skill.

Follow the skill format, testing methodology, and trigger-writing guidance.

## OpenCode Tool Mapping

Use OpenCode-native tools:

- Load skill: `skill`
- Todos: `todowrite`
- General subagent: `task` with `subagent_type: "general"`
- Exploration subagent: `task` with `subagent_type: "explore"`
- Read files: `read`
- Modify files: `apply_patch`
- Search contents: `grep`
- Find files: `glob`
- Run commands: OpenCode shell tool
- Fetch public resources: `webfetch`

Follow the existing operating-system and shell instructions for command
syntax. Do not assume Unix syntax on Windows.

## Integration with Other Instructions

Instruction precedence:

1. Safety and explicit user requirements.
2. Project-specific `AGENTS.md` rules.
3. Superpowers process and skill selection.
4. Codebase Memory for repository exploration and impact analysis.
5. Context Mode for tool and context routing.
6. RTK for shell-output optimization.
7. Ponytail for minimal implementation decisions.
8. i-have-adhd for response structure.
9. Caveman `full` for final wording.

Superpowers determines the development process.
Codebase Memory supplies code intelligence.
Ponytail keeps implementation minimal.
Caveman must not remove required reasoning, evidence, or verification.

## Guardrails

- Project skills override personal skills.
- Personal skills override bundled Superpowers skills.
- Explicit user instructions override skill defaults.
- Do not skip an applicable skill to save time.
- Do not invent skill results or claim a skill was loaded when it was not.
- Do not run destructive Git operations without explicit authorization.
- Do not claim success without fresh verification evidence.
