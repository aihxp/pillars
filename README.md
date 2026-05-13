# Pillars

[![License: CC0-1.0](https://img.shields.io/badge/license-CC0--1.0-blue.svg)](LICENSE)
[![Status: pre-1.0](https://img.shields.io/badge/status-pre--1.0-orange.svg)](CHANGELOG.md)
[![Spec: v0.1.0](https://img.shields.io/badge/spec-v0.1.0-green.svg)](SPEC.md)

**An open standard for project-specific instructions that keep coding agents aligned.**

Pillars is a decomposed, frontmatter-driven convention for documenting what a coding agent (Claude Code, Cursor, Copilot, Codex, Gemini, opencode, Aider, others) needs to know about a project before it writes a single line of code. Inspired by [design.md](https://github.com/google-labs-code/design.md), generalized into a tiered set of pillar files plus a thin `AGENTS.md` loader.

## Quick start

In your project root:

```bash
curl -O https://raw.githubusercontent.com/aihxp/pillars/main/AGENTS.md
mkdir agents
```

Then open your coding agent and say:

> *Adopt the Pillars standard. Read SPEC.md and PILLARS.md from https://github.com/aihxp/pillars. Scaffold stubs for the always-loaded pillars (`context.md`, `repo.md`) and any Core pillars that apply to this project.*

That's the install. Five minutes, two commands, one prompt. See [Adoption](#adopting-pillars) below for details.

## What Pillars solves

Coding agents drift. Without project-specific context they:

- Reach for the wrong library (Drizzle when you use Prisma).
- Place files where they don't belong (components in `src/lib/` when the convention is `src/components/`).
- Re-invent decisions you already made (auth strategy, error handling, naming).
- "Fix" intentional behavior because they don't know why it exists.

Linters and type systems catch some of this. They don't catch *intent*. Pillars is the layer between "what the code looks like" and "what an experienced engineer would tell a new contributor on day one."

## What Pillars is not

- Not a workflow tool. It produces no work; it surfaces context.
- Not a compliance document. Pillars are briefings, not rulebooks. The agent is trusted by default.
- Not tool-specific. Any agent that reads markdown and parses YAML can implement it.
- Not tooling. This repo defines the standard. Tooling (CLI, skills, IDE wrappers) lives separately, when it exists. See [the tooling philosophy](#standard-vs-tooling).

## How it works (one minute)

1. A project adopts Pillars by adding an `AGENTS.md` at the repo root and a folder of pillar files at `./agents/`.
2. Each pillar covers one domain (`data.md`, `auth.md`, `ui.md`, etc.) and follows an 8-section template with YAML frontmatter.
3. When an agent starts a task, it scans the frontmatter, picks the relevant pillars based on the task description, loads them, and follows their content.
4. Pillars come in tiers: 2 always-loaded, 9 Core, 10 Common, plus open-ended Domain pillars. Most projects use ~10-20 pillars total. Excluded pillars are first-class.

Full spec: [SPEC.md](SPEC.md). Pillar catalog with tiers, boundaries, and sub-pillar patterns: [PILLARS.md](PILLARS.md). FAQ: [FAQ.md](FAQ.md). This project dogfoods itself; the [AGENTS.md](AGENTS.md) and [agents/](agents/) folder demonstrate the standard in use.

## Adopting Pillars

### For a new project

1. Copy [AGENTS.md](AGENTS.md) from this repo into your repo root.
2. Create an `agents/` folder.
3. Ask your coding agent to write stubs for the two always-loaded pillars (`context.md`, `repo.md`) and any Core pillars that apply. Reference [examples/](examples/) for shape.
4. Fill sections as you make decisions. A pillar exists once it has frontmatter and a Scope statement; the rest fills in over time.

### For an existing project

1. Copy `AGENTS.md` into your repo root.
2. Don't create pillars upfront. The agent will degrade gracefully on absent pillars: it infers from existing code and states the assumption in its response.
3. As inferences hit edge cases, ask the agent to author the relevant pillar from the codebase.

## Standard vs. tooling

The repo defines the **standard** at the root (the spec, template, catalog, protocol). It's portable: any agent in any tool can read it.

**Tooling** lives in [`tooling/`](tooling/). It automates the meta-operations (bootstrap, authoring, drift detection) but isn't required. The standard works in every major AI coding tool with zero tooling installed.

### Multi-tool support

Tooling ships in two forms, with broad coverage:

| Form | Tools | Where |
|---|---|---|
| Native skill bundle | Claude Code | [`tooling/claude-skill/`](tooling/claude-skill/) |
| Universal prompts + per-tool install guides | Cursor, Codex CLI, Gemini CLI, opencode, Aider, Windsurf, Cline, Continue, and any other AI tool | [`tooling/prompts/`](tooling/prompts/) |

Both forms drive the same three operations: **init** (bootstrap Pillars in a project), **author** (draft a pillar from the codebase), **verify** (audit pillars against current code).

Pick your tool's install doc in [`tooling/prompts/`](tooling/prompts/) for one-paragraph setup instructions, or paste a prompt file directly into your tool's chat to run the operation.

CLI, deeper per-tool wrappers, and CI integrations are roadmap items for when adoption signal warrants them.

## Repository layout

```
pillars/
├── README.md           # this file
├── SPEC.md             # the formal standard
├── PILLARS.md          # the pillar enumeration: tiers, boundaries, sub-pillar patterns
├── AGENTS.md           # this project's own AGENTS.md (dogfooded protocol)
├── FAQ.md              # adoption and usage questions
├── CONTRIBUTING.md     # how to contribute
├── CODE_OF_CONDUCT.md  # community standards
├── SECURITY.md         # security policy
├── CHANGELOG.md        # version history
├── LICENSE             # CC0 1.0 Universal
├── agents/             # this project's own pillar files (dogfooded)
│   ├── context.md
│   └── repo.md
├── examples/           # worked example pillars for adopters
│   ├── data.md
│   └── auth.md
├── tooling/            # tooling forms (skills, universal prompts)
│   ├── claude-skill/   # Claude Code native skill bundle
│   │   ├── pillars-init/
│   │   ├── pillars-author/
│   │   └── pillars-verify/
│   └── prompts/        # Universal paste-in prompts + per-tool install guides
│       ├── pillars-init.md
│       ├── pillars-author.md
│       ├── pillars-verify.md
│       └── install-{cursor,codex-cli,gemini-cli,opencode,aider,windsurf,cline,continue}.md
└── DESIGN-NOTES.md     # design conversation log
```

## Status

Pre-1.0. The standard's structure is stable, validated against six hypothetical project archetypes (SaaS dashboard, CLI tool, ML pipeline, marketing site, real-time collab, e-commerce). Guidance and catalog will refine as real adoption surfaces edge cases. See [CHANGELOG.md](CHANGELOG.md) for version history.

## Contributing

Pull requests and proposals are welcome. See [CONTRIBUTING.md](CONTRIBUTING.md) for the contribution model (RFC-style for spec changes, direct PR for typos and examples).

## License

[CC0 1.0 Universal](LICENSE). Use Pillars freely, in any project, public or private, with or without attribution. The standard wins by being adopted.
