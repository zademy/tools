# Context7 MCP

Use Context7 to consult current, version-specific documentation when a task involves libraries, frameworks, SDKs, APIs, CLIs, or external services.

## Mandatory Workflow

1. Identify the library and its version by inspecting project files such as:
   - `package.json`
   - `pom.xml`
   - `build.gradle`
   - `requirements.txt`
   - `pyproject.toml`
   - Equivalent configuration files.

2. If a Context7 ID has already been provided in this format:

   ```text
   /organizacion/proyecto
   /organizacion/proyecto/version
   ```

   use it directly.

3. If no known ID exists, call `resolve-library-id` first using:
   - The official library name.
   - A concrete description of the task.

4. Select the result that best matches:
   - The official name.
   - The version used by the project.
   - Source reputation.
   - Documentation coverage.
   - Relevance to the task.

5. Call `query-docs` with the selected ID and a specific query.

6. Use separate queries for independent topics. For example, query authentication, caching, and routing separately rather than combining them in one request.

7. Implement the solution using only APIs, configurations, and examples supported by the retrieved documentation.

## Rules

- Always prioritize the version used by the project.
- Do not invent methods, properties, annotations, parameters, or configurations.
- Do not rely on recalled information when Context7 can verify it.
- Do not send passwords, tokens, API keys, personal data, or confidential code.
- Make at most three resolution calls and three documentation queries per task.
- Do not use Context7 for business logic, simple refactoring, or general concepts that do not depend on external documentation.
- If the documentation is insufficient, ambiguous, or does not match the version in use, state that clearly instead of assuming.
- At completion, briefly mention the library and documentation version consulted.
