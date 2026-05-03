---
description: Smart search with automatic tool routing (Exa, Context7, gh_grep, WebSearch)
allowed-tools: Bash, WebSearch, WebFetch, mcp__gh_grep__searchGitHub, mcp__plugin_exa_exa__web_search_exa, mcp__plugin_exa_exa__web_fetch_exa
---

Use the web-search-toolkit:search skill to analyze the query and route to the optimal search tool.

Routes to: Context7 (library docs), gh_grep (code search), Exa (search/fetch when available), WebSearch/WebFetch (fallback when Exa not installed).

$ARGUMENTS
