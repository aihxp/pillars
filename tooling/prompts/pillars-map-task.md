# Pillars Map Task Prompt

Paste this entire file into your AI coding tool's chat to explain which Pillars files should load for a specific task and why. This is a report-only workflow. It does not write files.

Use this to debug routing, teach the loading model, or check whether a task is missing a relevant pillar.

---

## Task: map a task to its Pillars load set

You are going to map a user task to the Pillars files that should be loaded under the project's `AGENTS.md` protocol. Do not write, edit, move, or delete files.

### Step 1: Get the task

Use the task text the user provided with this prompt. If no task text is provided, ask:

> What task should I map to Pillars?

Then stop until the user answers.

### Step 2: Locate Pillars

Check for:

```bash
ls AGENTS.md agents/
find agents -name "*.md" -type f | sort
```

If `AGENTS.md` or `agents/` is missing, report:

> This project does not appear to have Pillars set up. Use `pillars-init.md` first.

Then stop.

### Step 3: Read routing metadata

For each pillar file, parse frontmatter:

- `pillar`
- `status`
- `always_load`
- `covers`
- `triggers`
- `must_read_with`
- `see_also`

Read `AGENTS.md` for the `excluded:` block.

### Step 4: Compute the load set

Apply the Pillars loading protocol:

1. Load every pillar with `always_load: true`.
2. Identify primary pillars whose `triggers` or `covers` semantically match the task.
3. Add every pillar listed in each primary's `must_read_with`, depth 1 only.
4. Add `see_also` only if the task explicitly touches that area.
5. Treat excluded pillars as intentionally not applicable.
6. For missing but relevant known pillars, state the assumption and recommend authoring the pillar.

Deduplicate the final load set. If a pillar qualifies for multiple reasons, list it once and include all reasons in the "Why it loads" cell.

Do not follow `must_read_with` transitively.

### Step 5: Produce the report

Use this format:

```markdown
# Pillars Task Map

Task: <task>

## Load Set

| Pillar | Why it loads |
|---|---|
| `context` | always-loaded |
| `repo` | always-loaded |
| `<pillar>` | trigger match: <trigger or semantic reason> |
| `<pillar>` | must_read_with from `<primary>` |

## Soft References

| Pillar | Why it is only `see_also` |
|---|---|
| `<pillar>` | task touches this only if <condition> |

## Missing Or Excluded

- `<pillar>`: <missing, stub, or excluded explanation>

## Routing Notes

- <any useful observation about overmatching, weak triggers, or missing pillars>
```

If no task-routed pillars match, say:

> Only always-loaded pillars match this task.

---

## Constraints

- Do not write files.
- Do not modify files.
- Do not execute the mapped task.
- Do not treat absent non-floor pillars as errors.
- Do not follow `must_read_with` transitively.

---

## Now: map this task

**Task to map:** _<replace this line with the task, e.g., `Add workspace invite links`>_
