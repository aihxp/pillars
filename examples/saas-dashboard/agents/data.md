---
pillar: data
status: present
always_load: false
covers: [database schema, migrations, tenant-scoped queries, Drizzle usage]
triggers: [database, schema, migration, query, table, column, drizzle, postgres, sql, model, tenant, workspace]
must_read_with: [auth]
see_also: [api, observe]
---

## Scope

This pillar covers Northstar CRM's data layer: schema, migrations, query boundaries, and tenant scoping. It does not cover HTTP contracts (`api.md`) or runtime telemetry (`observe.md`).

## Context

Northstar CRM uses Drizzle with Postgres. Schema lives in `src/db/schema.ts`. Reusable query functions live in `src/db/queries/`. The active workspace is represented as `tenant_id` in the database and `workspaceId` in TypeScript.

Core tables:

- `users`: global identity.
- `workspaces`: customer organizations.
- `memberships`: users in workspaces with role.
- `accounts`: workspace-scoped companies.
- `contacts`: workspace-scoped people associated with accounts.
- `invites`: workspace-scoped pending invitations.

Every workspace-scoped table has `tenant_id` as the first non-id column.

## Decisions

- **Drizzle over Prisma.** Reason: schema definitions stay close to TypeScript, while raw SQL escape hatches remain available for reporting queries.
- **`tenant_id` in the database, `workspaceId` in app code.** Reason: the database convention is compact; the product language is clearer in application code.
- **Reusable query functions for shared reads.** Reason: account lists appear on multiple pages and need consistent tenant filtering.

## Rules

- Every query against a workspace-scoped table must filter by `tenant_id`.
- Do not accept `tenant_id` from the client. Derive it from the active session.
- Do not write raw SQL outside the approved reporting helper.

## Workflows

- **Adding a workspace-scoped table:**
  1. Add the table to `src/db/schema.ts`.
  2. Include `tenant_id` as the first non-id column.
  3. Add a foreign key to `workspaces.id`.
  4. Add or update query helpers under `src/db/queries/`.
  5. Add tests for same-workspace and cross-workspace access.

## Watchouts

- A missing `tenant_id` filter is a security bug, not a normal data bug.
- List endpoints need limits, even when the current workspace is small in development.

## Touchpoints

- `must_read_with: [auth]`: the active workspace comes from the session.
- `see_also: [api, observe]`: endpoint shape and query metrics live outside this example.

## Gaps

- Soft delete policy is undecided. Ask before adding `deleted_at` to a new table.
