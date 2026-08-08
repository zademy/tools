# SOLID Development Rules Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Add stack-agnostic SOLID development rules with an evidence-driven, YAGNI-safe workflow and activate them through the OpenCode skill router.

**Architecture:** Create one focused instruction that owns the complete SOLID process. Add only selection and sequencing rules to the existing router so it remains an orchestrator and does not duplicate implementation details.

**Tech Stack:** Markdown, Git, Python 3 standard library.

## Global Constraints

- Work on branch `docs/solid-development-rules`.
- Write all new prose in English.
- Create `instructions/solid-development-rules.md`.
- Modify `instructions/opencode-skill-router.md` only for routing and sequencing.
- Do not modify `config.json`.
- Keep the rules stack-agnostic and compatible with repository-local conventions.
- Require TDD for behavioral changes; mark it not applicable for documentation-only and formatting-only changes with a concise reason why behavior is unchanged.
- Prefer YAGNI and the smallest correct design over speculative SOLID abstractions.
- Require an evidenced production responsibility or boundary for production abstractions; test isolation is supporting evidence only.
- Do not duplicate task-specific skill internals in the router.

---

### Task 1: Add The SOLID Development Instruction

**Files:**
- Create: `instructions/solid-development-rules.md`
- Reference: `docs/superpowers/specs/2026-08-08-solid-development-rules-design.md`

**Interfaces:**
- Consumes: A software design, implementation, refactoring, testing, debugging, or review task plus current repository evidence.
- Produces: A minimal, verified change whose SOLID decisions address current pressure rather than hypothetical needs.

- [ ] **Step 1: Write the applicability and evidence gates**

Start with a direct pointer:

```markdown
Use these rules for software design, implementation, refactoring, testing, debugging, and code review.
```

Require the agent to identify applicable branches, inspect repository conventions, and state the concrete change pressure or demonstrated design problem. Complete the evidence gate only with current repository or runtime evidence.

- [ ] **Step 2: Write the minimal-design and TDD gates**

Require the smallest design that satisfies current behavior and constraints. Define Red-Green-Refactor as mandatory for features, bug fixes, and other behavioral changes. For documentation-only and formatting-only changes, require an explicit not-applicable result with a concise reason why behavior is unchanged.

Completion criteria:

- The failing test proves the intended behavior.
- The smallest implementation passes it.
- Relevant regression tests remain green.
- Every proposed abstraction has an immediate responsibility.
- Exempt changes record why TDD is not applicable and behavior is unchanged.

- [ ] **Step 3: Write observable SOLID checks**

Cover classes, functions, modules, and interfaces:

- **SRP:** One cohesive responsibility and one primary reason to change.
- **OCP:** Extend through an existing seam only when a concrete variation exists.
- **LSP:** Substitution preserves caller-visible contracts, invariants, and error behavior.
- **ISP:** Consumers depend only on operations they use.
- **DIP:** Policy avoids direct dependency on unstable details when a real boundary exists.

Each principle must state when it applies and what evidence proves completion. An unaffected principle may be marked not applicable when the changed unit does not involve its boundary, with a concise reason rather than repository evidence.

- [ ] **Step 4: Write the complexity and verification gates**

Reject interfaces, factories, layers, extension points, and indirection created only for hypothetical future needs. Permit a production abstraction only when present change pressure identifies a production responsibility or boundary, or demonstrated reuse requires it. Test isolation may support that evidence but cannot justify production abstraction alone.

Require relevant tests, formatting, linting, builds, and final diff review. Every executed check must pass; every skipped check must record its reason and residual risk. Finish only with fresh evidence, justified complexity, preserved repository conventions, and no speculative abstraction.

- [ ] **Step 5: Validate the new instruction**

Run:

```bash
python3 - <<'PY'
from pathlib import Path

path = Path("instructions/solid-development-rules.md")
text = path.read_text()

required = [
    "Applicability",
    "Evidence",
    "Minimal Design",
    "Test-Driven Development",
    "Single Responsibility",
    "Open/Closed",
    "Liskov Substitution",
    "Interface Segregation",
    "Dependency Inversion",
    "Complexity Gate",
    "Verification",
    "Completion",
    "Red-Green-Refactor",
    "documentation-only",
    "formatting-only",
    "YAGNI",
    "not applicable",
    "behavior is unchanged",
    "residual risk",
    "production responsibility or boundary",
    "cannot justify production abstraction alone",
    "concise reason",
]

missing = [item for item in required if item.lower() not in text.lower()]
assert not missing, missing
assert len([line for line in text.splitlines() if line.startswith("```")]) % 2 == 0
print("SOLID instruction valid")
PY
```

Expected: `SOLID instruction valid`.

### Task 2: Integrate The Instruction With The Router

**Files:**
- Modify: `instructions/opencode-skill-router.md`
- Verify: `instructions/solid-development-rules.md`

**Interfaces:**
- Consumes: Router branches for design, implementation, refactoring, testing, debugging, and review.
- Produces: A concise dependency that activates SOLID rules before the task-specific execution skill and returns control to the normal routing loop afterward.

- [ ] **Step 1: Add the instruction dependency**

Add a compact router concept for `SOLID Development Rules`. State that it controls engineering-quality constraints for design, implementation, refactoring, testing, debugging, and review. Point to `instructions/solid-development-rules.md` using the repository's existing pointer style.

- [ ] **Step 2: Add selection and sequencing rules**

For each approved branch, sequence the instruction before the selected execution skill:

```text
ROUTER
-> SOLID Development Rules
-> task-specific skill
-> ROUTER
```

Apply it to `improve-codebase-architecture`, `domain-modeling`, `codebase-design`, `implement`, `tdd`, `diagnosing-bugs`, and `code-review`.

Use observable direct-refactoring branches:

- Seam or interface refactoring: `SOLID Development Rules`, then `codebase-design`.
- Behavior-changing refactoring: `SOLID Development Rules`, then `tdd`.
- Behavior-preserving direct refactoring: apply `SOLID Development Rules`, perform the refactoring under those rules, then return to `ROUTER` without inventing a task skill.

Do not activate it for unrelated writing, teaching, research-only, setup, handoff, or questionnaire routes.

- [ ] **Step 3: Preserve router boundaries**

Keep the router limited to selection, sequencing, dependency resolution, and termination. Do not copy SOLID definitions, TDD steps, review checks, or complexity rules into the router.

- [ ] **Step 4: Validate router integration**

Run:

```bash
python3 - <<'PY'
from pathlib import Path
import re

router = Path("instructions/opencode-skill-router.md").read_text()
solid = Path("instructions/solid-development-rules.md").read_text()
router_lower = router.lower()
solid_lower = solid.lower()
dependency_match = re.search(
    r"^## Instruction Dependencies\s*$\n(.*?)(?=^## |\Z)",
    router,
    re.M | re.S,
)
assert dependency_match, "missing Instruction Dependencies section"
dependencies = dependency_match.group(1)
dependencies_lower = dependencies.lower()

required_router = [
    "SOLID Development Rules",
    "instructions/solid-development-rules.md",
    "improve-codebase-architecture",
    "domain-modeling",
    "codebase-design",
    "implement",
    "tdd",
    "diagnosing-bugs",
    "code-review",
]
missing = [item for item in required_router if item not in dependencies]
assert not missing, missing

assert all(item in dependencies for item in [
    "ROUTER",
    "-> SOLID Development Rules",
    "-> task-specific skill",
    "-> ROUTER",
]), "missing SOLID dependency sequence"

required_refactoring = [
    "Seam or interface refactoring",
    "Behavior-changing refactoring",
    "Behavior-preserving direct refactoring",
    "without inventing a task skill",
]
missing_refactoring = [
    item for item in required_refactoring if item not in dependencies
]
assert not missing_refactoring, missing_refactoring

for detail in [
    "one cohesive responsibility",
    "existing seam",
    "caller-visible behavior",
    "consumer depends only",
]:
    assert detail not in dependencies_lower, detail
    assert detail in solid_lower, detail

print("SOLID router integration valid")
PY
```

Expected: `SOLID router integration valid`.

### Task 3: Verify The Complete Change

**Files:**
- Verify: `instructions/solid-development-rules.md`
- Verify: `instructions/opencode-skill-router.md`
- Verify: `docs/superpowers/specs/2026-08-08-solid-development-rules-design.md`
- Verify: `docs/superpowers/plans/2026-08-08-solid-development-rules.md`
- Verify: `.superpowers/sdd/2026-08-08-solid-development-rules/final-fix-report.md`

**Interfaces:**
- Consumes: The new instruction, router integration, design, and plan.
- Produces: Fresh evidence that the approved scope and behavior are complete without unrelated changes.

- [ ] **Step 1: Run structural and language checks**

Run:

```bash
python3 - <<'PY'
from pathlib import Path
import re
import re

paths = [
    Path("instructions/solid-development-rules.md"),
    Path("instructions/opencode-skill-router.md"),
    Path("docs/superpowers/specs/2026-08-08-solid-development-rules-design.md"),
    Path("docs/superpowers/plans/2026-08-08-solid-development-rules.md"),
    Path(".superpowers/sdd/2026-08-08-solid-development-rules/final-fix-report.md"),
]
spanish = re.compile(
    r"\b(?:reglas|desarrollo|cuando|debe|usar|pruebas|revisión|código|diseño|cambios)\b",
    re.I,
)

for path in paths:
    text = path.read_text()
    prose_lines = []
    fence = None
    for line in text.splitlines():
        marker = re.match(r"^\s*(`{3,}|~{3,})", line)
        if fence is None:
            if marker:
                fence = marker.group(1)
            else:
                prose_lines.append(line)
        elif re.match(rf"^\s*{re.escape(fence[0])}{{{len(fence)},}}\s*$", line):
            fence = None
    assert fence is None, path
    prose = re.sub(r"`[^`\n]+`", "", "\n".join(prose_lines))
    assert not spanish.search(prose), path

print("SOLID documentation structure valid")
PY
```

Expected: `SOLID documentation structure valid`.

- [ ] **Step 2: Run all Task 1 and Task 2 validators**

Expected:

```text
SOLID instruction valid
SOLID router integration valid
```

- [ ] **Step 3: Run the six final-review semantic probes**

Run:

```bash
python3 - <<'PY'
from pathlib import Path
import re

solid = Path("instructions/solid-development-rules.md").read_text().lower()
router = Path("instructions/opencode-skill-router.md").read_text().lower()
design = Path(
    "docs/superpowers/specs/2026-08-08-solid-development-rules-design.md"
).read_text().lower()
dependency_match = re.search(
    r"^## instruction dependencies\s*$\n(.*?)(?=^## |\Z)",
    router,
    re.M | re.S,
)
assert dependency_match, "missing Instruction Dependencies section"
dependencies = dependency_match.group(1)

probes = {
    "tdd-n-a": all(item in solid and item in design for item in [
        "not applicable", "documentation-only", "formatting-only",
        "behavior is unchanged",
    ]),
    "design-routes": all(item in dependencies for item in [
        "improve-codebase-architecture", "domain-modeling",
    ]) and all(item in dependencies for item in [
        "router", "-> solid development rules",
        "-> task-specific skill", "-> router",
    ]),
    "refactoring-branches": all(item in dependencies for item in [
        "seam or interface refactoring", "codebase-design",
        "behavior-changing refactoring", "tdd",
        "behavior-preserving direct refactoring",
        "without inventing a task skill",
    ]),
    "verification-completion": all(item in solid for item in [
        "every executed check passes", "each skipped check",
        "reason and residual risk",
    ]),
    "production-abstraction": all(item in solid and item in design for item in [
        "production responsibility or boundary",
        "test isolation may support",
        "cannot justify production abstraction alone",
    ]),
    "principle-n-a": all(item in solid for item in [
        "unaffected principle", "not applicable",
        "does not involve that principle's boundary", "concise reason",
    ]) and "repository evidence showing why it does not apply" not in solid,
}

failed = [name for name, passed in probes.items() if not passed]
assert not failed, failed
print("SOLID six semantic probes valid")
PY
```

Expected: `SOLID six semantic probes valid`.

- [ ] **Step 4: Review the scoped diff**

Run:

```bash
git diff --check
python3 - <<'PY'
from pathlib import Path
import re
import subprocess

approved_source = {
    "instructions/solid-development-rules.md",
    "instructions/opencode-skill-router.md",
    "docs/superpowers/specs/2026-08-08-solid-development-rules-design.md",
    "docs/superpowers/plans/2026-08-08-solid-development-rules.md",
}
report = ".superpowers/sdd/2026-08-08-solid-development-rules/final-fix-report.md"
approved = approved_source | {report}
allowed_unrelated = {
    "docs/superpowers/plans/2026-08-07-instruction-audit.md",
    "docs/superpowers/specs/2026-08-07-instruction-audit-design.md",
}

for name in sorted(approved_source):
    data = Path(name).read_bytes()
    assert data.endswith(b"\n"), f"missing final newline: {name}"
    bad_lines = [
        number
        for number, line in enumerate(data.splitlines(), 1)
        if re.search(rb"[ \t]+$", line)
    ]
    assert not bad_lines, f"trailing whitespace: {name}:{bad_lines}"

status = subprocess.run(
    ["git", "status", "--short", "--untracked-files=all"],
    check=True,
    capture_output=True,
    text=True,
).stdout.splitlines()
changed = {line[3:] for line in status if line}
assert approved_source <= changed, (
    f"missing scoped evidence: {sorted(approved_source - changed)}"
)
assert Path(report).is_file(), f"missing report: {report}"
unexpected = changed - approved - allowed_unrelated
assert not unexpected, f"unexpected changed files: {sorted(unexpected)}"

print("SOLID four approved-file whitespace, newline, and scope valid")
PY
```

Generate the full four-source-file diff evidence package with repository-relative labels:

```bash
python3 - <<'PY'
from pathlib import Path
import subprocess

paths = [
    "instructions/solid-development-rules.md",
    "instructions/opencode-skill-router.md",
    "docs/superpowers/specs/2026-08-08-solid-development-rules-design.md",
    "docs/superpowers/plans/2026-08-08-solid-development-rules.md",
]
chunks = []

for path in paths:
    tracked = subprocess.run(
        ["git", "ls-files", "--error-unmatch", path],
        capture_output=True,
    ).returncode == 0
    command = (
        ["git", "diff", "--no-ext-diff", "--binary", "--", path]
        if tracked
        else ["git", "diff", "--no-index", "--binary", "--", "/dev/null", path]
    )
    result = subprocess.run(command, capture_output=True, text=True)
    expected_codes = {0} if tracked else {0, 1}
    assert result.returncode in expected_codes, (path, result.stderr)
    assert result.stdout, f"missing diff evidence: {path}"
    assert path in result.stdout, f"non-relative or missing diff label: {path}"
    chunks.append(result.stdout.rstrip())

evidence = Path(
    ".superpowers/sdd/2026-08-08-solid-development-rules/task-3-four-file.diff"
)
evidence.write_text("\n".join(chunks) + "\n")
print(f"SOLID four-file diff package written: {evidence}")
PY
```

Expected:

```text
SOLID four approved-file whitespace, newline, and scope valid
SOLID four-file diff package written: .superpowers/sdd/2026-08-08-solid-development-rules/task-3-four-file.diff
```

Confirm that the package contains all four approved files, all four pass whitespace and final-newline checks, tracked-change whitespace checks pass, and existing unrelated untracked documents remain untouched.

- [ ] **Step 5: Run impact analysis**

Use `codebase-memory-mcp_detect_changes` with project `Volumes-HIKSEMI-GitHub-tools`, scope `all`, depth `3`, base branch `master`, and `since` set to the branch point. Confirm documentation-only impact and investigate unexpected symbols.

- [ ] **Step 6: Request final review**

Review for missing activation branches, duplicated instruction internals, speculative-abstraction incentives, untestable completion criteria, and contradictions with repository conventions. Complete the task only after all Critical and Important findings are resolved and fresh validation passes.
