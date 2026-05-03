# Routing Guide

Detailed decision tree for the search router. **Exa-first strategy:** when Exa is available, prefer it for all web search and content extraction.

## Decision Tree

```
START: User query received
  │
  ├─ Contains a URL?
  │   ├─ Single URL → WebFetch first (direct, lightweight)
  │   │   └─ Result poor + Exa available? → retry with mcp__plugin_exa_exa__web_fetch_exa
  │   └─ Multiple URLs?
  │       ├─ Exa available? → mcp__plugin_exa_exa__web_fetch_exa (batch)
  │       └─ No Exa? → parallel WebFetch
  │
  ├─ Mentions a specific library/framework/SDK name?
  │   ├─ YES + usage/API/config question?
  │   │   └─ Context7 CLI (always, even with Exa)
  │   └─ YES + general info (not usage)?
  │       ├─ Exa available? → mcp__plugin_exa_exa__web_search_exa
  │       └─ No Exa? → WebSearch
  │
  ├─ Looking for code patterns/implementations?
  │   ├─ YES + specific repo mentioned?
  │   │   └─ gh_grep (with repoFilter) (always, even with Exa)
  │   └─ YES + open search?
  │       └─ gh_grep (always, even with Exa)
  │
  ├─ Deep/multi-angle/research query?
  │   ├─ Exa available? → exa:search skill
  │   └─ No Exa? → WebSearch + parallel subagents
  │
  └─ Default (quick facts, general questions)
      ├─ Exa available? → mcp__plugin_exa_exa__web_search_exa
      └─ No Exa? → WebSearch
```

## Strategy

- **Fuzzy searches** (discovery, facts, research) → Exa first for quality
- **Known URLs** → WebFetch first (direct, lightweight), Exa retry if content is poor
- **Specialized domains** → Context7 (docs) and gh_grep (code) always, even with Exa

## Tool Comparison

| Dimension | Context7 | gh_grep | Exa | WebSearch | WebFetch |
|-----------|----------|---------|-----|-----------|----------|
| Best for | Library docs | Code search | Web search & content | Quick facts (fallback) | URL content (fallback) |
| Auth required | No | No | Optional | No | No |
| Token cost | Low | Low | Medium-High | Low | Low |
| Accuracy for docs | High (version-specific) | N/A | Medium | Medium | N/A |
| Code coverage | N/A | High (1M+ repos) | Low | Low | N/A |
| Freshness | High (indexed) | High | High | High | Real-time |
| Content quality | N/A | N/A | High (clean extraction) | Medium | Medium |

## When to use what

- **Context7**: Always for library/framework/API documentation (specialized, version-specific)
- **gh_grep**: Always for GitHub code search (specialized, 1M+ repos)
- **Exa**: For everything else — web search, content extraction, deep research (when available)
- **WebSearch/WebFetch**: Only when Exa is not installed

## Edge Cases

### Library name is ambiguous
- "react" → could be React.js or React Native
- Solution: Context7 `library` command returns multiple matches; pick based on query context

### Query spans multiple types
- "How does Next.js middleware work + show me examples from GitHub"
- Solution: Run Context7 for docs AND gh_grep for examples, combine results

### Context7 quota exhausted
- Error: "Monthly quota reached" or "quota exceeded"
- Solution: Inform user, suggest `npx ctx7@latest login`, fall back to WebSearch

### gh_grep returns too many results
- Solution: Add `language`, `repo`, or `path` filter to narrow down

### Exa not installed
- Solution: Fall back to WebSearch (search) or WebFetch (URL extraction)

### User asks in Chinese but library docs are English
- Solution: Search in English (library docs are English), present results in user's language
