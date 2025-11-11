# Agents: Claude Code Skills & Plugin Marketplace

A cohesive setup to develop, test, and distribute Claude Code Skills and Plugins.

- **Skills**: Reusable expertise bundles discoverable by Claude Code.
- **Plugin marketplace**: Private registry to install your plugins via `/plugin install`.

## Quick Start

- Add this marketplace (local path):
  ```
  /plugin marketplace add /Users/kunl/code/github/agents
  /plugin install oxlint@agents-plugin-marketplace
  ```
- Add this marketplace (from GitHub once pushed):
  ```
  /plugin marketplace add kunl/agents
  /plugin install oxlint@agents-plugin-marketplace
  ```

## What’s inside

```
.claude-plugin/
  marketplace.json
plugins/
  oxlint/
    .claude-plugin/
      plugin.json
    skills/
      oxlint/
        SKILL.md
```

- `.claude/skills/`: Skills auto-discovered by Claude Code in this repo.
- `.claude-plugin/marketplace.json`: Marketplace manifest listing plugins in `plugins/`.
- `plugins/`: Distributable plugins with their own `.claude-plugin` metadata.

## Current components

- `oxlint` skill and plugin: Use [oxlint](https://oxlint.com/) to lint/fix JS/TS/Vue.

## Create a new plugin

1) Scaffold a new plugin under `plugins/<name>/.claude-plugin/plugin.json`
2) Add skills or commands under `plugins/<name>/skills` or `plugins/<name>/commands`
3) Register it in `.claude-plugin/marketplace.json` under `plugins` array

See template and guidelines:
- Claude Code Plugin Template (docs & structure): https://github.com/ivan-magda/claude-code-plugin-template/tree/main

## Create a new skill (local-only)

Copy an existing skill in `.claude/skills/<your-skill>/SKILL.md` with YAML frontmatter:

```markdown
---
name: your-skill-name
description: Short description
allowed-tools: Bash
---
# Usage
Explain scenarios, instructions, and examples.
```

## License

MIT
