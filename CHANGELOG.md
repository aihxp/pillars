# Changelog

All notable changes to the Pillars standard are documented here. Format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/). Versions follow [Semantic Versioning](https://semver.org/spec/v2.0.0.html):

- **Major** — frontmatter schema or loading-mechanism changes that break existing pillars.
- **Minor** — backward-compatible additions to schema, protocol, or pillar catalog.
- **Patch** — clarifications and corrections without behavior changes.

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
