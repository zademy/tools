# context-mode — Context Budget and Bulk Processing

## Role
Use context-mode to keep **large, noisy, or repetitive raw data** out of the conversation. It is a temporary processing/indexing layer, not the authority for code, memory, documentation, or web discovery.

## Route by Intent First
- Local indexed code → CodeGraph.
- Durable project state → Engram.
- Version-specific external docs → Context7.
- Unknown public web source → Web Search Prime.
- Known normal web page → Web Reader.
- Public GitHub repository → Zread.
- Visual input → Z.AI Vision.
- Ordinary supported CLI output → RTK.

## Use context-mode When
- Commands may emit large output.
- Multiple outputs require batching/correlation.
- Data needs counting, filtering, parsing, comparison, or transformation.
- Large files/resources should be analyzed without loading raw content into context.

Prefer batch execution for gathering and execute/execute-file for programmatic reduction. Return only derived results.

## RTK Coexistence
Use RTK for normal compact CLI output. Switch to context-mode when output remains large or custom processing is required. Do not wrap context-mode operations in RTK.

## Web Coexistence
Avoid uncontrolled `curl`/`wget` where context-mode interception applies. Use dedicated web tools normally; use context-mode fetch/index for bulk resources that need repeated targeted analysis.

## Continuity
After compaction, recover durable decisions/state from Engram first. Use context-mode search only for temporary indexed artifacts or bulky working outputs.
