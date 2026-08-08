# Web Search Prime MCP

Use Web Search Prime when the source URL is not yet known.

Use `web-search-prime` to locate public, current, verifiable information on the Internet.

## Available Tool

- `webSearchPrime`: searches the web and returns titles, URLs, summaries, and site data.

## When to Use It

- When the user asks to search, investigate, verify, or consult current public information.
- For news, recent versions, API changes, known errors, service availability, and current documentation.
- When the source's exact URL is not yet known.
- To find official sources before implementing a technical solution.

## Recommended Workflow

1. Convert the request into a concrete, concise query.
2. Include official names, versions, error messages, or dates when available.
3. Prefer official documentation, official repositories, specifications, and primary sources.
4. If results are ambiguous, refine the search instead of assuming.
5. When a claim depends on a page's full content, open the URL with `web-reader`.
6. Summarize only information supported by the results found.

## Rules

- Do not invent results, URLs, versions, or features.
- Do not present a search summary as though it were the full content of a page.
- Corroborate critical information with more than one reliable source.
- Clearly distinguish found facts from your own inferences.
- Do not include API keys, tokens, passwords, personal data, or confidential code in queries.
- Do not use this tool to read repository code; use `zread`.
- Do not use this tool when a concrete URL is already available to read; use `web-reader`.
- Avoid repeated searches that add no new information.
