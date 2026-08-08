# Zread MCP

Use Zread for public GitHub repository research.

Use `zread` to understand a public repository's documentation, structure, and source code.

## Available Tools

- `search_doc`: searches documentation, code, comments, news, issues, commits, PRs, and contributors related to a repository.
- `get_repo_structure`: retrieves the directory and file structure of a repository or subdirectory.
- `read_file`: reads the full content of a specific file.

## When to Use It

- To understand an open-source library or project hosted on GitHub.
- To locate the implementation of a class, function, module, or feature.
- To review a repository's architecture and organization.
- To investigate issues, recent changes, or documented project decisions.
- To read specific files before proposing an integration, fix, or refactoring.

## Recommended Workflow

1. Identify the repository in exact `owner/repo` format.
2. Use `search_doc` for an overview or to locate relevant concepts.
3. Use `get_repo_structure` to understand project organization and find exact paths.
4. Use `read_file` only when a concrete, relevant path is known.
5. Relate findings to specific files, modules, and symbols.
6. For deeper research, repeat the search-structure-read cycle only in the necessary areas.
7. Explain what comes from the repository and what is a technical inference.

## Rules

- Work only with public repositories supported by Zread.
- Do not invent paths, files, classes, functions, or behavior.
- Do not read large files before narrowing the scope.
- Do not use `read_file` without a sufficiently precise path.
- Prefer source files, official repository documentation, tests, and configuration.
- When multiple implementations exist, identify which one matches the version or branch being analyzed.
- Do not use Zread for local files in the current project.
- Do not use Zread for general web pages; use `web-reader`.
- Do not use Zread for general Internet searches; use `web-search-prime`.
- Do not include tokens, credentials, or private information in queries.
