# Z.AI Vision MCP Server

Use `zai-mcp-server` to analyze images, screenshots, diagrams, charts, and videos with Z.AI vision capabilities.

## Specialized Tool Selection

- Use `ui_to_artifact` to reproduce a UI or generate code, specifications, prompts, or descriptions from a UI screenshot.
- Use `extract_text_from_screenshot` to extract exact text, code, documentation, logs, or terminal content from a screenshot.
- Use `diagnose_error_screenshot` for errors visible in an IDE, terminal, or browser and to propose causes and corrective actions.
- Use `understand_technical_diagram` to interpret architecture, flow, UML, entity-relationship, and system diagrams.
- Use `analyze_data_visualization` to analyze charts, dashboards, metrics, trends, and anomalies.
- Use `ui_diff_check` to compare expected and implemented UI screenshots and identify visual or implementation differences.
- Use `image_analysis` for general image analysis when no specialized tool fits.
- Use `video_analysis` to analyze local or remote videos and describe scenes, moments, and entities.

## Recommended Workflow

1. Verify that the file exists and provide its local path or URL.
2. Select the most specific tool for the task.
3. State what to identify and the expected response format.
4. Separate visible observations from interpretations or hypotheses.
5. When analyzing a UI, identify its structure, components, states, typography, spacing, and observable behavior.
6. When diagnosing an error, relate the visible text to the available technical context.
7. Use `image_analysis` only as a general fallback.

## Rules

- In OpenCode, prefer referencing the file path; pasting an image directly may prevent the client from invoking this MCP.
- Do not invent text that is not legible.
- State when a conclusion is uncertain or depends on information that is not visible.
- Do not claim that a UI behaves in a particular way based only on a static screenshot.
- For `ui_diff_check`, clearly identify the reference image and the candidate image.
- For videos, use supported formats and respect the server's accepted limit.
- Do not expose secrets, personal data, or sensitive information present in images or videos.
- Do not use vision tools for information that can be verified directly in code or documentation.
