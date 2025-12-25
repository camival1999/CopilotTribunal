# Reusable Prompts

<!-- Owner: [YOUR TEAM] | Last updated: [DATE] -->

Most prompts for this repository are sourced from the **awesome-copilot MCP server**. A few discovery prompts are stored locally for users without MCP access.

## Local Prompts

| Prompt | Description |
|--------|-------------|
| \`setup-wizard.prompt.md\` | Interactive setup wizard for new repositories |
| \`suggest-awesome-github-copilot-agents.prompt.md\` | Discover and install community agents |
| \`suggest-awesome-github-copilot-instructions.prompt.md\` | Discover and install community instructions |
| \`suggest-awesome-github-copilot-prompts.prompt.md\` | Discover and install community prompts |

## Accessing More Prompts via MCP

Use the MCP server tools to discover and load additional prompts:

\`\`\`
# Search for prompts by keyword
@mcp_awesome-copilot search_instructions "implementation plan"

# Load a specific prompt
@mcp_awesome-copilot load_instruction mode="prompts" filename="create-implementation-plan.prompt.md"

# Browse available collections
@mcp_awesome-copilot list_collections
\`\`\`

## Recommended Prompts (via MCP)

| Prompt | Description |
|--------|-------------|
| \`conventional-commit.prompt.md\` | Generate conventional commit messages |
| \`create-implementation-plan.prompt.md\` | Generate detailed implementation plans |
| \`create-specification.prompt.md\` | Create AI-ready specification documents |
| \`documentation-writer.prompt.md\` | Technical documentation authoring |
| \`review-and-refactor.prompt.md\` | Code review and refactoring |
| \`readme-blueprint-generator.prompt.md\` | Generate README templates |
| \`prompt-builder.prompt.md\` | Create new prompt files |

## Why MCP Instead of Local Files?

1. **Always up-to-date** -- MCP server has the latest versions
2. **No duplication** -- Single source of truth
3. **Broader selection** -- Access to 100+ community prompts
4. **Less maintenance** -- No need to sync local copies

## Related

- [Custom agents](../agents/README.md)
- [Instruction files](../instructions/)
- [Core agent rules](../copilot-instructions.md)
