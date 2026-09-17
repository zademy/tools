# RTK — Token-Efficient Terminal Output

## Role
RTK is the preferred terminal-output compression layer for supported shell commands. It reduces noisy command output; it does **not** replace the command, compiler, test runner, or build system itself.

## Use RTK For
- Git status/diff/log output.
- Build, package-manager, container, Kubernetes, and similar CLI output when RTK supports it.
- Routine validation commands where concise output is sufficient.

If an installed hook transparently rewrites commands through RTK, do not manually double-wrap them.

## Direct RTK Commands
Use RTK directly for its own analytics/diagnostic commands such as `rtk gain`, `rtk discover`, version checks, and proxy/debug behavior.

Use `rtk proxy <cmd>` only when exact unfiltered command output is required for diagnosis.

## Routing Boundaries
- Code navigation/architecture → CodeGraph.
- Durable memory → Engram.
- Docs → Context7.
- Web → Web Search Prime / Web Reader.
- Public GitHub repositories → Zread.
- Vision → Z.AI Vision.

If terminal output is still very large, requires correlation across many commands, or needs custom parsing/filtering, use **context-mode** instead of forcing RTK to solve a bulk-analysis problem.
