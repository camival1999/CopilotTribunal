# Tier 2 Instruction Files

<!-- Owner: [YOUR TEAM] | Last updated: [DATE] -->

Auto-scoped instruction files that are loaded based on file path patterns.

## How Tier 2 Works

VS Code Copilot automatically loads these instructions when working with matching files. Each file uses \`applyTo:\` frontmatter to specify glob patterns.

## Available Instructions

| File | Scope | Description |
|------|-------|-------------|
| \`markdown.instructions.md\` | \`**/*.md\` | Markdown conventions |

## Adding New Instructions

1. Create \`<name>.instructions.md\`
2. Add \`applyTo:\` frontmatter with glob pattern
3. Add entry to this README
4. Update \`AGENTS.md\` if significant

## Bracket Escaping

If you have folders with brackets (e.g., \`[FolderName]/\`), escape them in glob patterns:

| Folder | Pattern |
|--------|---------|
| \`[FolderName]/\` | \`[[]FolderName[]]/**\` |

## Related

- [AGENTS.md](../AGENTS.md) -- Tier system overview
- [copilot-instructions.md](../copilot-instructions.md) -- Tier 1 core rules
