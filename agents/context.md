---
pillar: context
status: present
always_load: true
covers: [project identity, domain language, product invariants, glossary]
triggers: []
must_read_with: []
see_also: [repo]
---

## Scope

This pillar describes what Pillars is, the domain language used throughout the standard, the product invariants that hold across all decisions, and a glossary of canonical terms. It does not cover technical conventions (those live in `repo.md`), the standard's authoring template (lives in `SPEC.md`), or the pillar enumeration (lives in `PILLARS.md`).

## Context

Pillars is an open standard for project-specific instructions that keep coding agents (Claude, Cursor, Copilot, Codex, Aider, others) aligned to a codebase's facts, decisions, and conventions. A project adopting Pillars publishes markdown files at `./agents/*.md`, each conforming to the 8-section pillar template and opening with YAML frontmatter the agent uses to decide what to load. An `AGENTS.md` at the repo root describes the loading protocol; it is short by design (does not grow when pillars are added).

The standard is tool-agnostic: any agent that can read markdown and parse YAML can implement Pillars. Pillars sits underneath the broader AGENTS.md convention rather than replacing it; tool-specific files (`CLAUDE.md`, `.cursorrules`, `copilot-instructions.md`) become thin shims pointing at AGENTS.md.

**Glossary:**

- **Pillar** — a single domain's briefing file (e.g., `data.md`, `auth.md`). Sits in `./agents/`.
- **Always-loaded pillar** — frontmatter has `always_load: true`. Read every session regardless of task. Currently `context` and `repo`.
- **Trigger** — keyword or phrase in frontmatter; the agent matches against the task description to decide relevance.
- **Touchpoint** — declared coupling between pillars. Hard (`must_read_with`) or soft (`see_also`).
- **Load set** — the full set of pillars loaded for a task: primaries plus their direct `must_read_with` (depth 1, no transitive following).
- **Stub** — pillar with `status: stub` and minimal body. Signals "concern acknowledged, rules undefined; ask, don't infer."
- **Excluded** — pillar name declared in AGENTS.md's `excluded:` list as not applicable to this project.

**Product invariants:**

- The standard is portable; no tool-specific extensions allowed.
- Pillars are briefings, not rulebooks. They surface what the agent can't infer from code.
- Loading is single-pass, depth 1. No chain walks during task execution.
- Missing pillars degrade gracefully via inference + assumption-stating. Adoption is incremental.
- The standard is small. The full SPEC.md is intended to be readable in a sitting.

## Decisions

- **Three tiers plus an always-loaded set, not a flat list.** Reason: handles the long tail of project types without bloating the required core. A flat list of 25+ mandatory pillars would prevent incremental adoption.
- **Frontmatter-driven discovery, not a central router.** Reason: keeps the standard decentralized and portable. Adding a pillar requires zero AGENTS.md changes.
- **8-section template combining briefing + prescriptive layers.** Reason: trust the agent by default (Context + Decisions) but allow hard constraints (Rules + Watchouts) when not inferable from facts.
- **Depth-1 loading, not transitive closure.** Reason: predictability for authors. Each pillar's frontmatter declares the exact load set.
- **Sub-pillars discovered by folder, not frontmatter field.** Reason: visual hierarchy is self-documenting; conventional pattern (Hugo, Jekyll, Astro).
- **Layer 1 (spec) only for v1; Layer 2 (toolkit) separate product.** Reason: a standard should stand alone; tooling versions independently.

## Rules

(none) — context.md surfaces facts; constraint pillars sit elsewhere.

## Workflows

- **Adding a new top-level pillar to the standard:** decide tier, write frontmatter, write the 8 sections per "earn your keep" principle, update `PILLARS.md` enumeration, declare touchpoints from neighboring pillars where coupling is real.
- **Adding a sub-pillar to the standard:** create `./agents/<parent>/<name>.md`; same template and frontmatter as top-level pillars; document the sub-pillar pattern in `PILLARS.md` if it's a new pattern.

## Watchouts

- **Don't write Rules content that would be inferred from Context.** Bloats pillars without value. Re-read the principle: prescriptive content only when not inferable.
- **Don't make pillars compliance documents.** Tone is "experienced engineer briefing a new contributor on day one," not "auditor's checklist."
- **Don't over-couple via `must_read_with`.** More than 3 entries signals a boundary smell; the pillar may be doing too much, or a shared dependency should graduate to `always_load: true`.

## Touchpoints

- `see_also: [repo]` — tasks that touch file layout in addition to project identity load `repo.md`.

## Gaps

(none open at the standard level; project-specific gaps would be listed here as the standard evolves)
