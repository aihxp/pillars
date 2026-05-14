# Pillars: Agent Protocol

This fictional project follows the Pillars standard. Coding agents read `./agents/*.md` to stay aligned with Northstar CRM's facts, decisions, and conventions.

## At the start of any task

1. Load every file in `./agents/` whose frontmatter has `always_load: true`.
2. Scan frontmatter in the remaining pillar files.
3. Identify primary pillars whose `triggers` match the task.
4. Load the primaries and every pillar listed in their `must_read_with`, depth 1 only.
5. Consult `see_also` only if the task explicitly touches that area.
6. Follow `Rules`, apply `Workflows`, heed `Watchouts`, and ask about `Gaps`.

## Handling missing pillars

| State | Action |
|---|---|
| `status: present` | Load and comply. |
| `status: stub` | Ask before making decisions in this area. |
| Name in `excluded:` | Treat as intentionally not applicable. |
| Trigger matches a known absent pillar | Infer from code, state the assumption, and recommend creating the pillar. |

If `context.md` or `repo.md` is missing, pause and ask the human to create stubs or declare them excluded.

## Excluded pillars

```yaml
excluded:
  - name: mobile
    reason: Web-only product
  - name: ml
    reason: No model training or inference surface
  - name: payments
    reason: Billing is handled by the parent company platform
```
