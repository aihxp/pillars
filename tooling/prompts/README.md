# Universal Prompts

Tool-agnostic prompts that drive the Pillars meta-operations (init, author, verify) in any AI coding tool. Paste the relevant prompt into your tool's chat to run the procedure.

These work in: Claude Code, Cursor, Codex CLI, Gemini CLI, opencode, Aider, Windsurf, Cline, Continue, Pieces, and anything else that can read a project and write files based on instructions.

If your tool has a native command/skill format, see the `tooling/<tool>/` directory (when available) for tighter integration. For now, the universal prompts are the broadest-compatibility option.

## The three prompts

| File | When to use |
|---|---|
| [`pillars-init.md`](pillars-init.md) | Bootstrap Pillars in a new or existing project. Detects archetype, drops AGENTS.md, scaffolds stubs, sets exclusions. |
| [`pillars-author.md`](pillars-author.md) | Draft or revise a specific pillar from the codebase. Scans relevant code, presents 8-section draft for approval. |
| [`pillars-verify.md`](pillars-verify.md) | Audit pillars against current code. Flags drift with evidence, suggests fixes. Manual lightweight check. |

## How to use

### Generic flow (works in any tool)

1. Open the relevant prompt file in this folder.
2. Copy its full content to your clipboard.
3. Paste it as a single message into your AI coding tool's chat.
4. For `pillars-author.md`, add the pillar name at the bottom (the prompt's last line asks you to fill it in).
5. The AI will follow the procedure step by step.

### Per-tool quick install

For tools where the universal prompts are the recommended integration:

| Tool | Install doc |
|---|---|
| Cursor | [install-cursor.md](install-cursor.md) |
| Codex CLI | [install-codex-cli.md](install-codex-cli.md) |
| Gemini CLI | [install-gemini-cli.md](install-gemini-cli.md) |
| opencode | [install-opencode.md](install-opencode.md) |
| Aider | [install-aider.md](install-aider.md) |
| Windsurf | [install-windsurf.md](install-windsurf.md) |
| Cline | [install-cline.md](install-cline.md) |
| Continue | [install-continue.md](install-continue.md) |

For Claude Code, prefer the native skill bundle in [../claude-skill/](../claude-skill/) over the universal prompts.

## Why these are separate from the Claude Code skill

The Claude Code skill (`tooling/claude-skill/`) packages the same procedures as Claude Code skill files with proper frontmatter, triggers, and slash-command invocation. That packaging is tool-specific.

The universal prompts here are the same procedures formatted as paste-in instructions, with no tool-specific framing. They're slightly more verbose (because they include orientation that a native skill takes for granted) but they work everywhere.

Both forms drive the same underlying procedure. The Claude Code skill is sugar; the universal prompts are the substance.

## Drift between forms

If a procedural update happens, both forms should be updated together. The repository's CI (or a future Layer 2 toolkit check) may eventually enforce parity. For now, treat the procedures as canonically defined here in the prompts; the Claude Code skill mirrors them.

## Reference

- Pillars standard: https://github.com/aihxp/pillars
- SPEC.md: https://github.com/aihxp/pillars/blob/main/SPEC.md
- PILLARS.md: https://github.com/aihxp/pillars/blob/main/PILLARS.md
