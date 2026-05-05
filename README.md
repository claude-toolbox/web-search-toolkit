# web-search-toolkit

<!-- README-I18N:START -->

**English** | [简体中文](./README.zh.md)

<!-- README-I18N:END -->

Unified search toolkit for Claude Code — intelligent routing with Exa-first strategy for highest quality results.

## Features

- **Natural Language** — Just ask naturally, no command needed. "React useEffect 怎么用" activates Context7 automatically
- **Exa-First** — When Exa plugin is installed, uses Exa for all web search and content extraction for best quality
- **Intelligent Routing** — Automatically selects the best search tool based on query type
- **Graceful Fallback** — Without Exa, falls back to WebSearch and WebFetch
- **Zero Config** — Works out of the box with built-in tools; Exa plugin recommended for best experience

## Usage

No command needed — just ask in natural language:

- "React useEffect 怎么清理" → Context7 (library docs)
- "搜索 GitHub 上的 Next.js middleware 示例" → gh_grep (code search)
- "深度调研 2025 AI agent 框架" → Exa deep research
- "今天天气怎么样" → Exa search (or WebSearch if Exa not installed)
- "帮我看看 https://example.com" → Exa fetch (or WebFetch if Exa not installed)

## Commands

| Command | Description | Primary Tool | Fallback |
|---------|-------------|-------------|----------|
| `/web-search-toolkit:search` | Smart search with auto routing | Context7 / gh_grep / Exa | WebSearch / WebFetch |
| `/web-search-toolkit:exa-search` | Semantic search | Exa `web_search_exa` | WebSearch |
| `/web-search-toolkit:deep-search` | Multi-agent deep research | Exa `exa:search` skill | WebSearch parallel subagents |
| `/web-search-toolkit:fetch` | Fetch URL content | Exa `web_fetch_exa` | WebFetch |

## Dependencies

### Auto-configured (no setup needed)

| Tool | Purpose | Auth |
|------|---------|------|
| **gh_grep MCP** | GitHub code search across 1M+ repos | None (anonymous) |
| **Context7 CLI** | Library documentation lookup | None (optional `CONTEXT7_API_KEY` for higher limits) |

Context7 runs via `npx ctx7@latest` — no global installation required.

### Recommended (best experience)

| Tool | Purpose | Install |
|------|---------|---------|
| **Exa plugin** | All web search and content extraction | `/plugin install exa@claude-plugins-official` |

## Installation

```
/plugin marketplace add claude-toolbox/marketplace
/plugin install web-search-toolkit@marketplace
```

## Project Structure

```
web-search-toolkit/
├── agents/
│   └── web-search.md              # Search routing agent (auto-loaded)
├── commands/
│   ├── search.md                  # /search — smart routing
│   ├── exa-search.md              # /exa-search — Exa semantic search
│   ├── deep-search.md             # /deep-search — multi-agent research
│   └── fetch.md                   # /fetch — URL content extraction
├── skills/
│   └── search/
│       ├── SKILL.md               # Core routing skill
│       └── references/
│           └── routing-guide.md   # Detailed decision tree
├── .claude-plugin/plugin.json
├── .mcp.json
├── LICENSE
├── README.md
└── README.zh.md
```

## License

[MIT](./LICENSE)
