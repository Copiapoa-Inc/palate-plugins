---
name: palate
description: Query Palate — experts' source material with citations. Use when the user mentions Palate or asks to query, search, get, unlock, or check the latest from a Palate source. Do NOT use for general web search or for sources the user hasn't connected to Palate.
metadata:
  author: palate
  contract: https://palate.inc/SKILL.md
---

# Palate

Before acting, load and follow https://palate.inc/SKILL.md. That contract governs all Palate behavior; this file only routes you to it.

- Contract caching: fetch the contract once per session and cache it for the session. The contract's frontmatter carries a `version`. If any Palate API response returns a `palate-contract-version` header that differs from your cached version, refetch the contract before continuing.
- Authentication is already handled: this plugin's MCP server sends the user's Palate API key on every request. You never need to collect, store, or send the key yourself.
- For Palate queries, use the Palate MCP tools before any web lookup.
- If Palate fails, report the Palate failure. Never substitute web results silently.

<!-- The bootstrap bullets above are failsafes, not behavioral rules: they're the minimal safety floor for the window before the contract loads (including if the contract fetch fails). They are the ONLY behavior that belongs in this wrapper. Do not add others — all query-time behavior lives in the contract, which is fetched at runtime. Copying behavioral rules up here creates a frozen, un-updatable duplicate that will drift from the contract. -->

PALATE_PLATFORM: terminal
