# Palate - Claude Code plugin

Connects Claude Code (and Claude Desktop's Code tab) to Palate's hosted MCP server with a keyless install: the plugin registers `https://palate.inc/mcp` URL-only, and Claude Code's native OAuth flow signs the user in with a one-time browser approval. No API key, no installer document, no agent-written config.

It also bundles the thin Palate skill, which routes the agent to the canonical behavior contract at `https://palate.inc/SKILL.md` (fetched at runtime - the plugin never freezes behavior).

This repo is both the plugin and its marketplace: `.claude-plugin/marketplace.json` points at `./`, so adding the repo as a marketplace exposes the plugin directly.

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

## Roadmap

1. Team-test while private.
2. Flip the repo public -> external users install with the commands above; palate.inc gains a Claude Code one-liner and `install.md` gains a router line pointing here.
3. Submit to Anthropic's community marketplace (https://platform.claude.com/plugins/submit) for browsable discovery across Claude Code, Desktop, and claude.ai.
