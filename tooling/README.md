# Pillars Tooling

Tooling forms that automate the Pillars meta-operations (bootstrap, authoring, verification) across AI coding tools.

The **standard** itself ships separately as portable markdown ([SPEC.md](../SPEC.md), [PILLARS.md](../PILLARS.md), [AGENTS.md](../AGENTS.md)). It works in every major AI coding tool with **zero tooling installed**: the runtime alignment loop is just the tool reading `AGENTS.md` and following its protocol.

Tooling is optional. Use it when the meta-operations (creating pillars, drafting from code, checking for drift) get tedious to do by hand.

## What's here

```
tooling/
├── claude-skill/           # Native Claude Code skill bundle
│   ├── pillars-init/
│   ├── pillars-author/
│   └── pillars-verify/
└── prompts/                # Universal paste-in prompts + per-tool install guides
    ├── pillars-init.md
    ├── pillars-author.md
    ├── pillars-verify.md
    ├── install-cursor.md
    ├── install-codex-cli.md
    ├── install-gemini-cli.md
    ├── install-opencode.md
    ├── install-aider.md
    ├── install-windsurf.md
    ├── install-cline.md
    └── install-continue.md
```

## Which form for which tool

| Tool | Recommended form | Notes |
|---|---|---|
| Claude Code | `claude-skill/` (native skill bundle) | Drop into `~/.claude/skills/`; invoke via `/pillars-init` etc. |
| Cursor | `prompts/install-cursor.md` | Rule file + optional commands; or paste-in |
| Codex CLI | `prompts/install-codex-cli.md` | Native AGENTS.md support; paste-in for meta-ops |
| Gemini CLI | `prompts/install-gemini-cli.md` | Native AGENTS.md or GEMINI.md redirect |
| opencode | `prompts/install-opencode.md` | Native AGENTS.md support; optional commands |
| Aider | `prompts/install-aider.md` | `--read AGENTS.md` flag or conventions file |
| Windsurf | `prompts/install-windsurf.md` | `.windsurfrules` redirect or native AGENTS.md |
| Cline | `prompts/install-cline.md` | `.clinerules` redirect or native AGENTS.md |
| Continue | `prompts/install-continue.md` | `systemMessage` + `slashCommands` config |
| Any other AI coding tool | `prompts/` (paste-in) | The prompts are tool-agnostic; works anywhere |

## Quick start (any tool)

1. Adopt the standard: copy `AGENTS.md` to your project root, create `agents/`.
2. Read the install doc for your tool.
3. Use the meta-operation prompts (or skill, in Claude Code) to bootstrap and maintain pillars.

## Two forms, same procedure

The Claude Code skill bundle and the universal prompts implement the same three procedures:

| Operation | Job |
|---|---|
| **init** | Bootstrap Pillars: detect archetype, drop AGENTS.md, scaffold `agents/`, write stubs, set exclusions |
| **author** | Draft a specific pillar from the codebase via targeted archaeology, present 8-section draft for approval |
| **verify** | Audit pillars against current code, flag drift with evidence, suggest fixes (no auto-fix) |

The Claude Code skill is sugar (proper skill format, slash command invocation). The universal prompts are the substance (procedures that work in any AI coding tool by paste).

## Why no CLI

A CLI would be the right shape for CI/CD checks (lint, drift detection in PR gates). It's deliberately not in this release because:

1. The runtime alignment loop doesn't need a CLI; every AI tool already reads AGENTS.md.
2. Meta-operations work cleanly through the AI tool itself (skill or paste-in prompt).
3. CI integration is a separate use case with its own design (deterministic checks, no LLM calls). Belongs in v0.2 or later when there's clear demand.

If you want to use Pillars in CI today, write a workflow that pipes the verify prompt to your AI CLI of choice. A native CLI will come when warranted.

## Why no full per-tool wrappers

For most tools listed above, the universal prompts plus a thin install doc covers the integration. Building full per-tool wrappers (a Cursor command bundle, a Gemini commands TOML set, etc.) is multiplicative work that drifts as each tool's command format evolves.

Claude Code gets a full skill bundle because it has a stable native skill format and is the most common tooling target. Other tools get install docs that show how to use the universal prompts inside their existing command/rules surfaces.

When a particular tool develops a stable native command format and Pillars adoption signal warrants a tighter integration, a native bundle for that tool can ship as `tooling/<tool>/`. PRs welcome.

## License

[CC0 1.0 Universal](../LICENSE). Same as the standard. Use freely.
