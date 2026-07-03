# Palate — Claude Code plugin

Connects Claude Code (and Claude Desktop's Code tab) to Palate's hosted MCP server with a one-command install: the plugin registers `https://palate.inc/mcp` and Claude Code itself asks the user for their API key (masked, stored client-side, injected as the `palate-api-key` header). No installer document, no key in chat, no agent-written config.

It also bundles the thin Palate skill, which routes the agent to the canonical behavior contract at `https://palate.inc/SKILL.md` (fetched at runtime — the plugin never freezes behavior).

This repo is both the plugin and its marketplace: `.claude-plugin/marketplace.json` points at `./`, so adding the repo as a marketplace exposes the plugin directly.

## Install

```
/plugin marketplace add abbey-titcomb/palate-claude-plugin
/plugin install palate@palate
```

Or from the terminal:

```
claude plugin marketplace add abbey-titcomb/palate-claude-plugin && claude plugin install palate@palate
```

Enter your Palate API key (from https://palate.inc/settings/keys) when prompted.

**While this repo is private**, install works only for org members whose git auth has access. Going public is a repo-visibility flip — no file changes needed.

## Test locally

```
claude --plugin-dir /path/to/palate-claude-plugin
```

Confirm the `palate` MCP server connects (masked key prompt) and the Palate tools appear.

## Roadmap

1. Team-test while private.
2. Flip the repo public → external users install with the commands above; palate.inc gains a Claude Code one-liner and `install.md` gains a router line pointing here.
3. Submit to Anthropic's community marketplace (https://platform.claude.com/plugins/submit) for browsable discovery across Claude Code, Desktop, and claude.ai.
