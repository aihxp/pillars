---
pillar: repo
status: present
always_load: true
covers: [file layout, naming conventions, where things go]
triggers: []
must_read_with: []
see_also: [context]
---

## Scope

This pillar covers Northstar CRM's file layout and naming conventions. It does not cover database rules (`data.md`) or permission rules (`auth.md`).

## Context

Northstar CRM uses a Next.js app router layout with route groups by product area.

```text
src/
├── app/
│   ├── (dashboard)/
│   │   ├── accounts/
│   │   ├── contacts/
│   │   └── settings/
│   └── api/
├── auth/
│   ├── server.ts
│   └── permissions.ts
├── db/
│   ├── schema.ts
│   ├── client.ts
│   └── queries/
└── ui/
    ├── components/
    └── forms/
```

Route handlers live under `src/app/api/`. Shared server-side auth code lives under `src/auth/`. Database schema and reusable queries live under `src/db/`. Reusable visual components live under `src/ui/components/`.

## Decisions

- **Route groups mirror product areas.** Reason: account teams think in accounts, contacts, and settings, so feature files stay easy to find.
- **Reusable queries under `src/db/queries/`.** Reason: cross-page account and contact queries otherwise drift into subtly different versions.

## Rules

- Do not put database access in client components.
- Do not put route handlers under `src/ui/`.

## Workflows

- **Adding a new dashboard area:** create a route group under `src/app/(dashboard)/`, place server actions with the route, and move shared UI into `src/ui/components/` only after a second caller appears.

## Watchouts

- Avoid creating top-level folders for one feature. Start colocated under the route group; promote only when reuse is real.

## Touchpoints

- `see_also: [context]`: product language explains why routes are grouped by customer workflow.

## Gaps

- Background job layout is undecided. Ask before adding workers or scheduled tasks.
