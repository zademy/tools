# Web Reader — Read a Known Web Source

## Role
Use Web Reader when a **specific normal web URL is already known** and its actual page content must be inspected.

## Routing Boundaries
- No source URL yet → Web Search Prime.
- Public GitHub repository code, structure, issues, commits, or repository docs → Zread.
- Version-specific library/API docs available through Context7 → Context7 first, unless the exact web page itself must be verified.
- Very large page requiring repeated targeted analysis without flooding context → context-mode fetch/index may be preferable.

## Workflow
1. Read the known relevant URL.
2. Identify the page's main content, date/version, and important warnings.
3. Extract only sections needed for the task.
4. Follow links only when they answer a concrete missing question.
5. Distinguish source statements from technical inference.

Prefer official documentation and primary sources. Do not invent content when access is blocked or incomplete, and do not treat menus/metadata as main content.

Never place credentials, tokens, private identifiers, or confidential information in URLs.
