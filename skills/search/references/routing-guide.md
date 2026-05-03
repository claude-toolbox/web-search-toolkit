# Routing Guide

Detailed decision tree for the search router.

## Decision Tree

```
START: User query received
  │
  ├─ Contains a URL?
  │   └─ YES → WebFetch
  │
  ├─ Mentions a specific library/framework/SDK name?
  │   ├─ YES + usage/API/config question?
  │   │   └─ Context7 CLI
  │   └─ YES + general info (not usage)?
  │       └─ WebSearch
  │
  ├─ Looking for code patterns/implementations?
  │   ├─ YES + specific repo mentioned?
  │   │   └─ gh_grep (with repoFilter)
  │   └─ YES + open search?
  │       └─ gh_grep
  │
  ├─ Deep/multi-angle/research query?
  │   ├─ Exa available?
  │   │   └─ YES → exa:search skill
  │   └─ Exa not available?
  │       └─ WebSearch + parallel subagents
  │
  └─ Default → WebSearch
```

## Tool Comparison

| Dimension | Context7 | gh_grep | Exa | WebSearch | WebFetch |
|-----------|----------|---------|-----|-----------|----------|
| Best for | Library docs | Code search | Deep research | Quick facts | URL content |
| Auth required | No | No | Optional | No | No |
| Token cost | Low | Low | Medium-High | Low | Low |
| Accuracy for docs | High (version-specific) | N/A | Medium | Medium | N/A |
| Code coverage | N/A | High (1M+ repos) | Low | Low | N/A |
| Freshness | High (indexed) | High | High | High | Real-time |

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
- Solution: Fall back to WebSearch with parallel subagents for multi-angle coverage

### User asks in Chinese but library docs are English
- Solution: Search in English (library docs are English), present results in user's language
