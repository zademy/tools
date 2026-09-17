# Z.AI Vision — Visual Analysis

## Role
Use Z.AI Vision only when the task depends on visual input: screenshots, images, UI references, charts, diagrams, terminal/IDE captures, or video.

Choose the most specific vision capability available: UI reproduction, exact screenshot text extraction, error diagnosis, technical-diagram interpretation, data-visualization analysis, UI comparison, general image analysis, or video analysis.

## Routing Boundaries
Do not use vision when the same information is directly available from source code, configuration, logs, or documentation.

After visual extraction, hand off by intent when useful:
- Local code/error path → CodeGraph.
- External API/library behavior → Context7.
- Current/public error research → Web Search Prime, then Web Reader.
- Public GitHub implementation → Zread.

## Rules
Use file paths/URLs supported by the server. Separate visible facts from interpretation. Do not invent illegible text or behavior that cannot be observed from a static image.

For UI comparison, clearly identify reference and candidate. For errors, extract visible evidence before proposing causes.

Do not expose secrets or sensitive information visible in screenshots or videos.
