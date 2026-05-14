---
pillar: auth
status: present
always_load: false
covers: [identity, sessions, workspace membership, roles, permissions]
triggers: [auth, login, logout, session, invite, member, membership, role, permission, access, user, workspace]
must_read_with: [data]
see_also: [security, api]
---

## Scope

This pillar covers authentication, workspace membership, and authorization rules. It does not cover secret handling (`config.md`) or general adversarial concerns (`security.md`).

## Context

Northstar CRM uses Better-Auth with server-side sessions. A session contains `userId`, `workspaceId`, `role`, and `expiresAt`.

Memberships join users to workspaces:

- `owner`: manages invites, workspace settings, and destructive actions.
- `admin`: manages accounts, contacts, notes, and regular members.
- `member`: manages accounts, contacts, notes, and tasks.
- `viewer`: read-only access to workspace records.

Permission checks live at the route handler or server action boundary. Database helpers assume the caller has already authorized the action.

## Decisions

- **Server-side sessions.** Reason: workspace deactivation and role changes must revoke access immediately.
- **Role on membership, not user.** Reason: one person can be an owner in one workspace and a viewer in another.
- **Permission checks at boundaries.** Reason: it keeps the data layer focused on tenant-scoped persistence and makes authorization tests easier to read.

## Rules

- Check permissions before calling a mutation that changes workspace-scoped data.
- Rotate the session when a member's role changes.
- Do not perform permission checks inside `src/db/queries/`.

## Workflows

- **Adding an owner-only action:**
  1. Add the permission to `src/auth/permissions.ts`.
  2. Check the permission in the route handler or server action.
  3. Call the data-layer mutation with the active `workspaceId`.
  4. Add tests for allowed, denied, and cross-workspace cases.

## Watchouts

- Invite links must be scoped to a workspace. A globally valid invite token can become a cross-tenant access bug.

## Touchpoints

- `must_read_with: [data]`: workspace identity in auth must match tenant scoping in queries.
- `see_also: [security, api]`: rate limiting and request contracts live outside this example.

## Gaps

- SSO and SCIM behavior is undecided. Ask before adding enterprise identity flows.
