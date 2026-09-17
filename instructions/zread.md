# Zread — Public GitHub Repository Research

## Role
Use Zread for **remote public GitHub repository** research: documentation, repository structure, source files, implementation details, issues, commits, pull requests, and related project history.

## Routing Boundaries
- Current local repository → CodeGraph, not Zread.
- General Internet discovery → Web Search Prime.
- Normal known web page → Web Reader.
- Version-specific consumer documentation → Context7 when available.
- Durable local project memory → Engram.

## Workflow
1. Identify the exact `owner/repo`.
2. Search repository content/concepts before reading files broadly.
3. Inspect repository structure when paths or architecture matter.
4. Read a file only after a relevant path is known.
5. Match the branch/version/tag relevant to the task whenever possible.
6. Distinguish repository evidence from inference.

Do not invent paths, symbols, files, or behavior. Narrow scope before reading large files.

When remote findings are used to modify the current project, return to the local toolchain: inspect the local implementation with CodeGraph/current files and validate with the project's real tests/build. RTK may compress terminal validation output; context-mode may process unusually large results.
