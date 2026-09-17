# Context7 — Version-Specific Technical Documentation

## Role
Use Context7 for authoritative, version-aware documentation about external libraries, frameworks, SDKs, APIs, CLIs, and services.

## Priority
For an API/configuration question tied to a dependency version, prefer Context7 before general web search when Context7 covers the technology.

Do not use Context7 to understand the application's own business logic or local architecture; use CodeGraph for that.

## Workflow
1. Determine the dependency/version from the local project configuration when available.
2. Reuse a known Context7 library ID when reliable; otherwise resolve the library ID.
3. Select the result matching the official project and relevant version.
4. Query documentation with a focused technical question.
5. Split unrelated documentation topics into separate queries.
6. Implement only APIs/options supported by the retrieved documentation.

## Fallbacks
- If Context7 coverage is missing, ambiguous, or outdated → use Web Search Prime to find official sources, then Web Reader to inspect them.
- If the required fact is specifically about implementation, issues, commits, or files in a public GitHub repository → use Zread.

Never send secrets or confidential source code. Prefer the version actually used by the project and explicitly note uncertainty when documentation does not match it.
