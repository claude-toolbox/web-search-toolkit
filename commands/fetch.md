---
description: Fetch URL content (WebFetch for known URLs, Exa for complex/multiple pages)
allowed-tools: WebFetch, mcp__plugin_exa_exa__web_fetch_exa
---

Fetch the URL(s) provided by the user. For known URLs, use WebFetch first; for complex or multiple pages, prefer Exa.

**Single URL:** Use `WebFetch` directly. If the result is poor (heavy JS, SPA, incomplete), retry with `mcp__plugin_exa_exa__web_fetch_exa` if Exa is available.

**Multiple URLs:**
- If Exa plugin is available: use `mcp__plugin_exa_exa__web_fetch_exa(urls=[url1, url2, ...], maxCharacters=3000)` for batch fetching
- If Exa plugin is NOT available: run parallel `WebFetch` calls

$ARGUMENTS
