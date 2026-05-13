# Pillars

**An open standard for project-specific instructions that keep coding agents aligned.**

Pillars is a decomposed, frontmatter-driven convention for documenting what a coding agent (Claude, Cursor, Copilot, Codex, Aider, others) needs to know about a project before it writes a single line of code. Inspired by [design.md](https://github.com/google-labs-code/design.md), generalized into a tiered set of pillar files plus a thin `AGENTS.md` loader.

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
- Not a toolkit (yet). This repo defines the standard. Tooling for bootstrap, generation, and drift detection is a separate future product.

## How it works (one minute)

1. A project adopts Pillars by adding an `AGENTS.md` at the repo root and a folder of pillar files at `./agents/`.
2. Each pillar covers one domain (`data.md`, `auth.md`, `ui.md`, etc.) and follows an 8-section template with YAML frontmatter.
3. When an agent starts a task, it scans the frontmatter, picks the relevant pillars based on the task description, loads them, and follows their content.
4. Pillars come in tiers: 2 always-loaded, 9 Core, 10 Common, plus open-ended Domain pillars. Most projects use ~10-20 pillars total. Excluded pillars are first-class.

The full spec is in [SPEC.md](SPEC.md). The pillar enumeration with tiers, boundaries, and sub-pillar patterns is in [PILLARS.md](PILLARS.md). This project dogfoods itself: its own [AGENTS.md](AGENTS.md) and [agents/](agents/) demonstrate the standard in use.

## Adopting Pillars

For a new project:

1. Copy `AGENTS.md` from this repo into your repo root.
2. Create an `agents/` folder.
3. Start with stubs for the two always-loaded pillars (`context.md`, `repo.md`) and any Core pillars that apply. Reference [examples/](examples/) for shape.
4. Fill sections as you make decisions. A pillar exists once it has frontmatter and a Scope statement; the rest fills in over time.

For an existing project:

1. Copy `AGENTS.md` into your repo root.
2. The agent will degrade gracefully when pillars are missing: it infers from existing code and states the assumption in its response.
3. Author pillars from the agent's inferences over time, or generate them with a toolkit (separate future product).

## Repository layout

```
pillars/
├── README.md          # this file
├── SPEC.md            # the formal standard
├── PILLARS.md         # the pillar enumeration, tiers, boundaries
├── AGENTS.md          # this project's own AGENTS.md (dogfooded)
├── agents/            # this project's own pillar files (dogfooded)
│   ├── context.md
│   └── repo.md
├── examples/          # worked example pillars for adopters
│   ├── data.md
│   └── auth.md
├── DESIGN-NOTES.md    # design conversation log
└── LICENSE            # CC0 1.0 Universal
```

## License

[CC0 1.0 Universal](LICENSE). Use Pillars freely, in any project, public or private, with or without attribution. The standard wins by being adopted.

## Status

Pre-1.0. The standard's structure is stable (validated against six hypothetical project archetypes); guidance refinements are expected as real adoption surfaces edge cases. Iterations land in [DESIGN-NOTES.md](DESIGN-NOTES.md).
