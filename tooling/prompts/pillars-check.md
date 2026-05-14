# Pillars Check Prompt

Paste this entire file into your AI coding tool's chat to check whether a project's Pillars files are structurally valid. This is a lightweight conformance check, not a CLI and not a drift audit.

For drift between pillar claims and the current codebase, use `pillars-verify.md` instead.

---

## Task: check Pillars structure

You are going to inspect the current project for structural conformance with the [Pillars](https://github.com/aihxp/pillars) standard. Report findings only. Do not write, edit, move, or delete files.

### Step 1: Locate the required entry points

Check for:

```bash
ls AGENTS.md agents/
```

If `AGENTS.md` is missing, report:

> Missing `AGENTS.md`. This project is not currently Pillars-compatible.

If `agents/` is missing, report:

> Missing `agents/`. This project is not currently Pillars-compatible.

If either is missing, stop after reporting the missing entry points.

### Step 2: Inventory pillar files

List every markdown file under `agents/` recursively:

```bash
find agents -name "*.md" -type f | sort
```

For each file, parse the YAML frontmatter and record:

- Path
- `pillar`
- `status`
- `always_load`
- `covers`
- `triggers`
- `must_read_with`
- `see_also`

### Step 3: Validate frontmatter

For each pillar file, check:

- Frontmatter starts and ends with `---`.
- `pillar` exists.
- `covers` exists and is a list.
- `triggers` exists and is a list, unless `always_load: true`.
- `status`, when present, is either `present` or `stub`.
- `always_load`, when present, is a boolean.
- `must_read_with`, when present, is a list.
- `see_also`, when present, is a list.

### Step 4: Validate naming and paths

For each top-level pillar at `agents/<name>.md`:

- The filename without `.md` must match the `pillar` value.

For each sub-pillar at `agents/<parent>/<name>.md`:

- The filename without `.md` must match the `pillar` value.
- Do not require a `parent` frontmatter field. Pillars derives parentage from the folder path.

Flag deeper nesting under `agents/<parent>/<child>/<name>.md` as a warning unless the project has explicitly documented that pattern.

### Step 5: Validate body sections

Each pillar body must include these 8 headings, in this order:

1. `## Scope`
2. `## Context`
3. `## Decisions`
4. `## Rules`
5. `## Workflows`
6. `## Watchouts`
7. `## Touchpoints`
8. `## Gaps`

Empty sections should contain `(none)` or a short explanation of why they are empty. For `status: stub`, accept placeholder text as long as the 8 headings exist.

### Step 6: Validate required floor pillars

Check that:

- `agents/context.md` exists.
- `agents/repo.md` exists.
- Both have `always_load: true`.

If either floor pillar is missing, report it as a blocking conformance issue.

### Step 7: Validate references

For every name listed in `must_read_with` or `see_also`:

- Check whether a matching pillar file exists under `agents/`.
- Check whether the name appears in the `excluded:` block in `AGENTS.md`.

If the referenced pillar is neither present nor excluded, flag it as an unresolved reference.

Do not follow `must_read_with` transitively. Pillars loading is depth 1.

### Step 8: Validate exclusions

Read the `excluded:` block in `AGENTS.md`.

Accept either:

```yaml
excluded: []
```

or:

```yaml
excluded:
  - name: ui
    reason: No visual UI surface
```

For each exclusion:

- `name` should be a pillar name.
- `reason` is recommended. Flag a missing reason as a warning, not an error.

### Step 9: Produce the report

Use this format:

```markdown
# Pillars Structure Check

Project: <best available project name>
Pillar files scanned: <N>
Blocking issues: <N>
Warnings: <N>

## Blocking Issues

- <issue with file path and reason>

## Warnings

- <issue with file path and reason>

## Confirmed

- <short list of things that passed>

## Suggested Next Actions

1. <highest-value fix>
2. <next fix>
```

If there are no blocking issues, say:

> No blocking Pillars structure issues found.

---

## Constraints

- Do not write files.
- Do not modify files.
- Do not audit pillar claims against application code. That belongs to `pillars-verify.md`.
- Do not treat absent non-floor pillars as errors when they are not referenced.
- Do not require a CLI, package install, schema library, or external network access.

---

Now: check this project and produce the structure report.
