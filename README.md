# Bitrise Agent Skills

A collection of [Agent Skills](https://agentskills.io) that bring Bitrise expertise directly into your AI-assisted development workflow.

## Skills

### `using-bitrise-ci`

Gives agents deep knowledge of Bitrise CI so it can help you plan, create, edit, and troubleshoot your CI/CD setup.

**What it covers:**
- Creating Bitrise CI projects
- Writing, refactoring or explaining `bitrise.yml` files (workflows, pipelines, step bundles, triggers)
- Finding and configuring steps from the Step Library
- Managing workspaces, apps, groups, and roles via the API
- Triggering and troubleshooting builds

Agents will automatically load this skill when the conversation is about Bitrise CI. You can also invoke it in Cluade directly with `/using-bitrise-ci`.

## Recommended companion: Bitrise MCP server

For the best experience, pair these skills with the [Bitrise MCP server](https://github.com/bitrise-io/bitrise-mcp). The MCP server gives agents direct access to the Bitrise API — it can manage your projects, releases or builds, and more without leaving the conversation.

When the MCP server is connected, agents will prefer it over manual API calls or CLI commands.

## Installation

### Via Skills CLI (recommended)

[Skills CLI](https://github.com/vercel-labs/skills) is a universal skill installer that works with Claude Code and other AI tools.

To install all skills from this repo:

```bash
npx skills add bitrise-io/agent-skills
```

By default this installs skills for your current project (`.claude/skills/`). Add `--global` to make them available across all your projects:

```bash
npx skills add bitrise-io/agent-skills --global
```

To list available skills without installing:

```bash
npx skills add bitrise-io/agent-skills --list
```

To install a specific skill by name:

```bash
npx skills add bitrise-io/agent-skills --skill using-bitrise-ci
```

### Manual installation

Copy the skill directory into your agent's skills folder.

**Personal (available in all projects):**

```bash
mkdir -p ~/.claude/skills/using-bitrise-ci
cp using-bitrise-ci/SKILL.md ~/.claude/skills/using-bitrise-ci/SKILL.md
```

**Project-level (current project only):**

```bash
mkdir -p .claude/skills/using-bitrise-ci
cp using-bitrise-ci/SKILL.md .claude/skills/using-bitrise-ci/SKILL.md
```
