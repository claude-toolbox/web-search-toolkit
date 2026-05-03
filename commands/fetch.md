---
description: Fetch URL content (simple pages use WebFetch, complex pages use Exa for better quality)
allowed-tools: WebFetch, mcp__plugin_exa_exa__web_fetch_exa
---

Fetch the URL(s) provided by the user. Choose the tool based on complexity:

**Simple pages** (blog posts, docs, articles): Use the built-in `WebFetch` tool directly.

**Complex pages** (SPAs, heavy JavaScript, multi-section pages, or when user requests high quality): Use `mcp__plugin_exa_exa__web_fetch_exa` with `maxCharacters` parameter (default: 3000).

**Multiple URLs**: Always use `mcp__plugin_exa_exa__web_fetch_exa` with the `urls` array parameter for batch fetching.

If unsure, default to WebFetch. If the result is poor, retry with Exa.

$ARGUMENTS
