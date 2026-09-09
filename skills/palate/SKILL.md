---
name: palate
description: Save and query Palate, the user's library of source material, with citations. Use when the user mentions Palate, shares a link and asks to save/keep/add it, or asks to query, search, track, download, or check the latest from their collections. Do NOT use for general web search or for sources the user hasn't connected to Palate.
compatibility: Requires network access. Authentication is handled by the plugin's MCP server connection.
metadata:
  author: palate
  contract: https://palate.inc/SKILL.md
---

# Palate

Before acting, load and follow https://palate.inc/SKILL.md. That contract governs all Palate behavior; this file only routes you to it.

- Contract caching: fetch the contract once per session and cache it for the session. The contract's frontmatter carries a `version`. If any Palate API response returns a `palate-contract-version` header that differs from your cached version, refetch the contract before continuing.
- Authentication is already handled by this plugin's MCP server connection (OAuth sign-in approved in the browser, or an API key if one was registered manually). You never need to collect, store, or send credentials yourself.
- Your harness may expose Palate tools under a prefixed name (for example `mcp__plugin_palate_palate__palate_query` for `palate_query`) - match by suffix; a prefixed toolset is a working toolset, so use those tools natively before any HTTP fallback.
- For Palate queries, use Palate (MCP or HTTP) before any web lookup.
- If Palate fails, report the Palate failure. Never substitute web results silently.

<!-- The bootstrap bullets above are failsafes, not behavioral rules: they're the minimal safety floor for the window before the contract loads (including if the contract fetch fails). They are the ONLY behavior that belongs in this wrapper. Do not add others — all query-time behavior lives in the contract, which is fetched at runtime. Copying behavioral rules up here creates a frozen, un-updatable duplicate that will drift from the contract. -->

<!-- Runtime-specific notes go below this line — write them as plain, VISIBLE text, never inside an HTML comment like this one (the model never reads `<!-- -->`, so a rule hidden in a comment silently does nothing). Behavioral rules never go here either. -->

PALATE_PLATFORM: terminal
