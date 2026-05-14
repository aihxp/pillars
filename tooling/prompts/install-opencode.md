# Pillars in opencode

[opencode](https://opencode.ai/) reads `AGENTS.md` at the project root natively. Pillars works in opencode with zero installation beyond the standard itself.

## Runtime alignment (no install needed)

Once your project has `AGENTS.md` and `agents/` at the root, opencode will:

1. Read `AGENTS.md` on session start.
2. Follow its protocol to scan `agents/*.md` frontmatter.
3. Load the relevant pillars based on the task.
4. Comply with their content during code work.

No opencode-specific configuration required.

## Running the meta-operations (check / init / author / verify)

opencode supports custom commands and agents via `.opencode/` configuration. To install Pillars meta-operations as opencode commands:

### Option A: opencode custom commands

Check your opencode version's command format (it has evolved). If your version supports `.opencode/commands/` or similar:

```bash
mkdir -p .opencode/commands
cp tooling/prompts/pillars-init.md .opencode/commands/pillars-init.md
cp tooling/prompts/pillars-author.md .opencode/commands/pillars-author.md
cp tooling/prompts/pillars-verify.md .opencode/commands/pillars-verify.md
cp tooling/prompts/pillars-check.md .opencode/commands/pillars-check.md
```

Then invoke via `/pillars-check`, `/pillars-init`, etc. Adapt the file format (frontmatter, extension) to match your opencode version's expectations.

### Option B: paste into chat (always works)

1. Open the relevant prompt file.
2. Copy its content.
3. Paste into opencode chat.

The agent follows the procedure step by step.

## Removing

Delete `.opencode/commands/pillars-*.md` if you used Option A.

## Reference

- opencode: https://opencode.ai/
- Pillars standard: https://github.com/aihxp/pillars
