# Palate plugin

Connects Claude Code (and Claude Desktop's Code tab) to Palate's hosted MCP server with a keyless install: the plugin registers `https://palate.inc/mcp` URL-only, and Claude Code's native OAuth flow signs the user in with a one-time browser approval. No API key, no installer document, no agent-written config.

It also bundles the thin Palate skill, which routes the agent to the canonical behavior contract at `https://palate.inc/SKILL.md` (fetched at runtime - the plugin never freezes behavior).

This repo is both the plugin and its marketplace: `.claude-plugin/marketplace.json` points at `./`, so adding the repo as a marketplace exposes the plugin directly.

## Two packaging formats, one plugin

The repo ships the same skill and the same MCP server in two manifests.
Both read `skills/`, so there is one copy of the skill and no drift between formats.

| Format | Manifest | MCP config | Used by |
| --- | --- | --- | --- |
| Claude plugin | `.claude-plugin/plugin.json` | `.mcp.json` | Claude Code, Claude Desktop |
| Agent Plugins 1.0.0 | `plugin.json` | `mcp.json` | Cursor and other clients that support the open standard |

Agent Plugins is a vendor-neutral standard for packaging skills and MCP servers.
The specification is at https://agent-plugins.org.

The skill frontmatter is limited to the fields the Agent Skills specification allows.
A client that follows the standard skips a skill with any other field, so do not add one.

## Install

```
/plugin marketplace add Copiapoa-Inc/palate-claude-plugin
/plugin install palate@palate
```

Or from the terminal:

```
claude plugin marketplace add Copiapoa-Inc/palate-claude-plugin && claude plugin install palate@palate
```

Then sign in: Claude Code flags the `palate` server as needing authentication (you'll see a startup notice, or run `/mcp`). Select the server, choose Authenticate, and approve access in the browser. Done - no key ever touches your config.

### API key alternative (headless or scripted use)

The browser approval needs an interactive session. For CI, remote boxes, or scripted `claude -p` runs, register the server manually with an API key from https://palate.inc/#settings/api-keys instead:

```
claude mcp add --transport http palate https://palate.inc/mcp \
  --header "palate-api-key: YOUR_KEY" \
  --header "palate-client: claude-plugin"
```

The plugin ships without a key field on purpose: plugin manifests can't include a header conditionally, and a `${user_config.*}` reference with no value breaks the server registration. The manual command above is the supported key path.

## Test locally

```
claude --plugin-dir /path/to/palate-claude-plugin
```

Confirm the `palate` MCP server appears, complete the OAuth approval via `/mcp`, and check that the Palate tools load.

Before submitting or releasing, validate the plugin and marketplace manifests:

```
claude plugin validate /path/to/palate-claude-plugin
```

### Test the Agent Plugins format

Cursor loads a plugin from a local folder.
Copy `plugin.json`, `mcp.json`, `LICENSE`, and `skills/` into `~/.cursor/plugins/local/palate`, then restart Cursor.

Validate both manifests against the published schemas at https://agent-plugins.org/schemas/1.0.0/, and validate the skill with the Agent Skills reference validator.

## Roadmap

1. Submit to Anthropic's community marketplace (https://platform.claude.com/plugins/submit) for discovery across Claude Code, Desktop, and claude.ai.
2. Submit to the Cursor marketplace, which accepts the Agent Plugins format.
3. Keep the xAI catalog entry pinned to a current commit.
