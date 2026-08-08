# Web Reader MCP

Use Web Reader when a concrete URL is known.

Use `web-reader` to read and extract the complete content of that URL.

## Available Tool

- `webReader`: retrieves a web page's title, main content, metadata, and links.

## When to Use It

- When the user provides a URL.
- After locating a relevant source with `web-search-prime`.
- To consult documentation, release notes, technical articles, guides, public READMEs, or reference pages.
- When the actual page content must be verified rather than relying only on a search summary.

## Recommended Workflow

1. Confirm that the URL points to the relevant source.
2. Read the page with `webReader`.
3. Identify the main content, date, version, and applicable warnings.
4. Extract only the sections related to the task.
5. Follow internal links only when needed to complete the answer.
6. Summarize the findings without changing their meaning.
7. If the page does not contain the answer, use `web-search-prime` to locate another source.

## Rules

- Do not invent content when the page is empty, blocked, or inaccessible.
- Do not confuse metadata, menus, or navigation links with the main content.
- Do not follow every link on a page without a specific reason.
- Prefer official pages and documentation matching the version in use.
- Distinguish explicit source text from your own inferences.
- Do not put credentials, tokens, or private information in the URL.
- Do not use `web-reader` to explore repository structure or files; use `zread`.
- Do not use `web-reader` to discover sources when no URL is known; use `web-search-prime`.
