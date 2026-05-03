---
description: Semantic search with Exa AI (higher quality results)
allowed-tools: mcp__plugin_exa_exa__web_search_exa, mcp__plugin_exa_exa__web_fetch_exa, WebSearch
---

Search with Exa AI for high-quality semantic results.

**If Exa plugin is available (preferred):**
1. Use `mcp__plugin_exa_exa__web_search_exa` with the user's query
2. For deeper content extraction, follow up with `mcp__plugin_exa_exa__web_fetch_exa` on the best URLs
3. Present results with sources

**If Exa plugin is NOT available (fallback):**
1. Use `WebSearch` with a refined query
2. Present results with sources
3. Note: results are from WebSearch; suggest installing the Exa plugin for higher quality

$ARGUMENTS
