<div align="center">

# OpenCode Tools

**Curated instruction files for the [OpenCode](https://opencode.ai) AI coding agent.**

Skill routing, MCP tool policies, communication modes, token optimization, and persistent-memory protocols — ready to drop into any project.

[![Repo size](https://img.shields.io/github/repo-size/zademy/tools?style=flat-square)](https://github.com/zademy/tools)
[![Last commit](https://img.shields.io/github/last-commit/zademy/tools?style=flat-square)](https://github.com/zademy/tools/commits/master)
[![Commit activity](https://img.shields.io/github/commit-activity/m/zademy/tools?style=flat-square)](https://github.com/zademy/tools/graphs/commit-activity)
[![Stars](https://img.shields.io/github/stars/zademy/tools?style=flat-square)](https://github.com/zademy/tools/stargazers)
[![Built for OpenCode](https://img.shields.io/badge/Built%20for-OpenCode-4A90D9?style=flat-square)](https://opencode.ai)
[![Made with Markdown](https://img.shields.io/badge/Markdown-000000?style=flat-square&logo=markdown&logoColor=white)](https://www.markdownguide.org/)

</div>

## About

This repository is a collection of standalone, composable **instruction files** that shape how OpenCode behaves. Each file in [`instructions/`](instructions/) encodes one concern — a routing policy, a tool-usage protocol, a communication style — so you can mix and match only what a given project needs.

## Contents

| File | Purpose |
|---|---|
| [`opencode-skill-router.md`](instructions/opencode-skill-router.md) | Selects and sequences installed skills: which skill, when, in what order, and when control returns |
| [`superpowers.md`](instructions/superpowers.md) | Superpowers as the primary software-development process layer |
| [`ponytail.md`](instructions/ponytail.md) | "Lazy senior dev" mode: efficient, minimal code; stop at the first rung that holds |
| [`engram.md`](instructions/engram.md) | Engram persistent-memory protocol for durable project knowledge |
| [`persistent-memory-engram-mnemosyne.md`](instructions/persistent-memory-engram-mnemosyne.md) | Split persistent memory between Engram (project facts) and Mnemosyne (user context) |
| [`codebase-memory-mcp.md`](instructions/codebase-memory-mcp.md) | Codebase Memory MCP as the primary code-intelligence tool |
| [`context-mode.md`](instructions/context-mode.md) | Mandatory routing rules to protect the context window from flooding |
| [`RTK.md`](instructions/RTK.md) | Rust Token Killer: CLI proxy that cuts up to 90% of bash output tokens |
| [`context7.md`](instructions/context7.md) | Consult current, version-specific documentation via Context7 |
| [`zread.md`](instructions/zread.md) | Research public GitHub repositories with Zread |
| [`web-reader.md`](instructions/web-reader.md) | Read and extract full content from a known URL |
| [`web-search-prime.md`](instructions/web-search-prime.md) | Locate public, current, verifiable information when no URL is known |
| [`zai-mcp-server.md`](instructions/zai-mcp-server.md) | Analyze images, screenshots, diagrams, charts, and videos with Z.AI vision |
| [`caveman.md`](instructions/caveman.md) | Ultra-compressed caveman communication mode to save tokens |
| [`i-have-adhd.md`](instructions/i-have-adhd.md) | Output style tuned for ADHD readers: next action first, numbered steps |
| [`iso-8859-1.md`](instructions/iso-8859-1.md) | Enforce strict ISO-8859-1 handling for legacy files, never convert to UTF-8 |

## Getting started

OpenCode loads these files through the `instructions` field of [`opencode.json`](https://opencode.ai/docs/config/). The files are combined with your `AGENTS.md` rules — no copy-pasting needed.

### Without downloading anything (recommended)

OpenCode can fetch instructions straight from this repository, so you never have to clone or copy files locally. Just add the raw GitHub URLs to your `opencode.json` (project or global at `~/.config/opencode/opencode.json`):

```json
{
  "$schema": "https://opencode.ai/config.json",
  "instructions": [
    "https://raw.githubusercontent.com/zademy/tools/master/instructions/opencode-skill-router.md",
    "https://raw.githubusercontent.com/zademy/tools/master/instructions/engram.md"
  ]
}
```

The pattern for any file is:

```text
https://raw.githubusercontent.com/zademy/tools/master/instructions/<FILE>.md
```

> [!NOTE]
> Remote instructions are fetched with a 5 second timeout.

### Per project (local copy)

Alternatively, clone (or copy) this repository into your project and reference the files in your project's `opencode.json`:

```json
{
  "$schema": "https://opencode.ai/config.json",
  "instructions": [
    "tools/instructions/opencode-skill-router.md",
    "tools/instructions/engram.md"
  ]
}
```

Paths and glob patterns are both supported, so you can load everything at once:

```json
{
  "$schema": "https://opencode.ai/config.json",
  "instructions": ["tools/instructions/*.md"]
}
```

> [!TIP]
> Every file is self-contained. Start with one or two (e.g. `opencode-skill-router.md` + `engram.md`) and add more as you need them.

## Requirements

Some instructions are pure prompts and work out of the box. Others depend on MCP servers, CLI tools, or skill packs that you must install and configure in OpenCode first:

| Instruction | Requires | Where to get it |
|---|---|---|
| [`opencode-skill-router.md`](instructions/opencode-skill-router.md) | Matt Pocock's skills installed in OpenCode | [mattpocock/skills](https://github.com/mattpocock/skills) |
| [`superpowers.md`](instructions/superpowers.md) | Superpowers skills installed in OpenCode | [obra/superpowers](https://github.com/obra/superpowers) |
| [`engram.md`](instructions/engram.md) | Engram MCP server | [Gentleman-Programming/engram](https://github.com/Gentleman-Programming/engram) |
| [`persistent-memory-engram-mnemosyne.md`](instructions/persistent-memory-engram-mnemosyne.md) | Engram + Mnemosyne MCP servers | [engram](https://github.com/Gentleman-Programming/engram) · [mnemosyne](https://github.com/mnemosyne-oss/mnemosyne) |
| [`codebase-memory-mcp.md`](instructions/codebase-memory-mcp.md) | Codebase Memory MCP server | [DeusData/codebase-memory-mcp](https://github.com/DeusData/codebase-memory-mcp) |
| [`context-mode.md`](instructions/context-mode.md) | context-mode MCP server | [mksglu/context-mode](https://github.com/mksglu/context-mode) |
| [`RTK.md`](instructions/RTK.md) | `rtk` CLI installed and on your `$PATH` | [rtk-ai/rtk](https://github.com/rtk-ai/rtk) |
| [`context7.md`](instructions/context7.md) | Context7 MCP server | [upstash/context7](https://github.com/upstash/context7) |
| [`zread.md`](instructions/zread.md) | Zread MCP server (Z.AI) | [zread.ai/mcp](https://zread.ai/mcp) |
| [`web-reader.md`](instructions/web-reader.md) | Web Reader MCP server (Z.AI) | [docs.z.ai](https://docs.z.ai/devpack/mcp/reader-mcp-server) |
| [`web-search-prime.md`](instructions/web-search-prime.md) | Web Search Prime MCP server (Z.AI) | [docs.z.ai](https://docs.z.ai/devpack/mcp/search-mcp-server) |
| [`zai-mcp-server.md`](instructions/zai-mcp-server.md) | Z.AI Vision MCP server | [npm `@z_ai/mcp-server`](https://www.npmjs.com/package/@z_ai/mcp-server) |
| [`caveman.md`](instructions/caveman.md) | Caveman skills installed in OpenCode | [juliusbrussee/caveman](https://github.com/juliusbrussee/caveman) |
| [`ponytail.md`](instructions/ponytail.md) | Ponytail skill installed in OpenCode | [DietrichGebert/ponytail](https://github.com/DietrichGebert/ponytail) |
| [`i-have-adhd.md`](instructions/i-have-adhd.md) | i-have-adhd skill installed in OpenCode | [ayghri/i-have-adhd](https://github.com/ayghri/i-have-adhd) |
| [`iso-8859-1.md`](instructions/iso-8859-1.md) | None | — |

> [!IMPORTANT]
> The Z.AI servers (Zread, Web Reader, Web Search Prime, Vision) need a Z.AI API key. The files marked with `—` are custom prompts with no external project behind them.

## Repository layout

```text
.
├── instructions/          # The instruction files (the actual content)
├── docs/superpowers/      # SDD plans and specs from instruction audits
└── .superpowers/sdd/      # Per-task working artifacts of those audits
```

## Acknowledgements

Built around the [OpenCode](https://opencode.ai) agent and workflows inspired by [mattpocock/skills](https://github.com/mattpocock/skills) and [Superpowers](https://github.com/obra/superpowers).
