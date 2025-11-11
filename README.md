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
  skills/
    oxlint/
      SKILL.md
      plugin.json
```

- `.claude-plugin/marketplace.json`: Marketplace manifest listing plugins in `plugins/`.
- `plugins/`: Distributable plugins with their own `.claude-plugin` metadata.

## Current components

- `oxlint` skill and plugin: Use [oxlint](https://oxlint.com/) to lint/fix JS/TS/Vue.



## License

MIT
