# Changelog

All notable changes to the Pillars standard are documented here. Format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/). Versions follow [Semantic Versioning](https://semver.org/spec/v2.0.0.html):

- **Major** — frontmatter schema or loading-mechanism changes that break existing pillars.
- **Minor** — backward-compatible additions to schema, protocol, or pillar catalog.
- **Patch** — clarifications and corrections without behavior changes.

## [0.1.3] - 2026-05-14

Standard itself unchanged.

### Added

- **`tooling/prompts/pillars-check.md`**: paste-in structural conformance check for frontmatter, required sections, floor pillars, references, and exclusions. This gives adopters a no-CLI validation path.
- **`examples/saas-dashboard/`**: compact end-to-end adoption example with `AGENTS.md`, always-loaded pillars, task-routed pillars, and example task loading behavior.

### Changed

- Dogfooded `agents/context.md` and `agents/repo.md` now reflect the current repository shape, including optional tooling.
- README, FAQ, and contributing docs now clarify that standard compatibility is defined by `SPEC.md`; tooling-only releases do not change compatibility.

[0.1.3]: https://github.com/aihxp/pillars/releases/tag/v0.1.3

## [0.1.2] - 2026-05-13

Multi-tool tooling support. Standard itself unchanged.

### Added

- **`tooling/prompts/`** — universal paste-in prompts that drive the same three operations as the Claude Code skill, but work in any AI coding tool:
  - `pillars-init.md` — tool-agnostic version of the init procedure.
  - `pillars-author.md` — tool-agnostic version of the author procedure. Pillar name supplied at the bottom of the prompt.
  - `pillars-verify.md` — tool-agnostic version of the verify procedure.
- **Per-tool install guides** under `tooling/prompts/install-*.md` for: Cursor, Codex CLI, Gemini CLI, opencode, Aider, Windsurf, Cline, Continue. Each covers runtime alignment (how the tool reads AGENTS.md) and meta-operation invocation (native commands where supported, paste-in where not).
- **`tooling/README.md`** — umbrella intro to all tooling forms with a tool-to-form matrix.
- **`tooling/prompts/README.md`** — how to use the universal prompts.
- README updated to highlight multi-tool support.

### Notes

- Universal prompts and the Claude Code skill drive the same three procedures. Both forms should be updated together when procedures change.
- For tools beyond the eight with install docs (e.g., Pieces, Pi Coder, future tools), the universal prompts work via paste-in. No per-tool wrapper is required for the operations to function.
- CLI still deliberately not included. Roadmap item for v0.2+ when CI demand surfaces.

[0.1.2]: https://github.com/aihxp/pillars/releases/tag/v0.1.2

## [0.1.1] - 2026-05-13

First tooling form ships. Standard itself unchanged.

### Added

- **`tooling/claude-skill/`** — Claude Code skill bundle with three skills:
  - `pillars-init` — bootstrap Pillars in a project. Detects archetype (CLI / SaaS / ML / marketing / mobile / OSS lib / greenfield), drops AGENTS.md, scaffolds `./agents/`, writes stubs, sets archetype-appropriate exclusions with reasons.
  - `pillars-author` — draft a specific pillar from the codebase. Scans relevant code, presents 8-section draft for user approval before writing.
  - `pillars-verify` — audit existing pillars against the current codebase. Walks Context-section claims, flags drift with evidence, suggests fixes. Does not auto-fix.
- **`tooling/claude-skill/README.md`** — install and usage instructions for the skill bundle.
- README updated to reference the tooling directory and explain the standard-vs-tooling split.

### Notes

- Tooling is optional; the standard works unmodified in every major AI coding tool without it.
- CLI form is deliberately not included; deferred until clear demand for CI/scripted use.
- Other tooling forms (Cursor commands, Codex prompts, neutral CLI) may follow when adoption signal warrants.

[0.1.1]: https://github.com/aihxp/pillars/releases/tag/v0.1.1

## [0.1.0] - 2026-05-13

Initial release. Foundations of the Pillars standard.

### Added

- **Specification** (`SPEC.md`): formal definition of the standard, including:
  - Repository layout (`AGENTS.md` at root, `./agents/` directory).
  - 8-section pillar body template (Scope, Context, Decisions, Rules, Workflows, Watchouts, Touchpoints, Gaps).
  - YAML frontmatter schema (`pillar`, `status`, `always_load`, `covers`, `triggers`, `must_read_with`, `see_also`).
  - 6-step loading protocol (depth-1, no transitive closure).
  - Four-state missing-pillar protocol (Present / Stub / Excluded / Absent).
  - Maturity gradient (stub -> partially populated -> fully populated).
- **Pillar enumeration** (`PILLARS.md`): tiered catalog including:
  - 2 always-loaded pillars (`context`, `repo`).
  - 9 Core pillars (`stack`, `arch`, `data`, `api`, `ui`, `auth`, `quality`, `deploy`, `observe`).
  - 10 Common pillars (`config`, `security`, `compliance`, `i18n`, `a11y`, `analytics`, `integrations`, `async`, `cache`, `notifications`).
  - Open-ended Domain tier with worked examples.
  - Boundary call tiebreakers between commonly-confused pillars.
  - Sub-pillar conventions and patterns.
  - Archetype starter exclusion lists (CLI, internal API, ML pipeline, marketing site, mobile app, open-source library).
- **Reference AGENTS.md**: ~50-line protocol description, dogfooded.
- **Dogfooded pillars** (`agents/`):
  - `agents/context.md` — Pillars project identity, domain language, glossary.
  - `agents/repo.md` — Pillars repository layout and naming conventions.
- **Worked examples for adopters** (`examples/`):
  - `examples/data.md` — typical Drizzle + Postgres data pillar.
  - `examples/auth.md` — typical Better-Auth + multi-tenant auth pillar.
- **Design notes** (`DESIGN-NOTES.md`): the full design conversation that produced v0.1, preserved for context.
- **Project documents**: `README.md`, `CONTRIBUTING.md`, `CODE_OF_CONDUCT.md`, `SECURITY.md`, `FAQ.md`, `LICENSE`.

### Validated

- Standard structure pressure-tested against six hypothetical project archetypes (SaaS dashboard, CLI tool, ML pipeline, marketing site, real-time collaborative app, e-commerce platform). All friction surfaced was guidance-level, not structural.

[0.1.0]: https://github.com/aihxp/pillars/releases/tag/v0.1.0
