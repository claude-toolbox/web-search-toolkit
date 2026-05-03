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

### Natural Language Routing

The router classifies your query and picks the optimal tool:

| Query Type | Tool | Fallback | Example |
|------------|------|----------|---------|
| Library/API docs | Context7 CLI | WebSearch | "How to use React useEffect" |
| GitHub code search | gh_grep MCP | WebSearch | "Find Next.js middleware examples" |
| Quick facts | Exa `web_search_exa` | WebSearch | "What's new in React 19" |
| Deep research | Exa `exa:search` skill | WebSearch parallel subagents | "Comprehensive comparison of AI agents" |
| Simple URL | WebFetch | Exa `web_fetch_exa` | "帮我看看这个博客" |
| Complex/multiple URLs | Exa `web_fetch_exa` | WebFetch | "抓取这两个 SPA 页面" |

**Strategy:** Fuzzy searches (discovery, facts, research) prefer Exa for quality. Known URLs use WebFetch first (direct, lightweight), retry with Exa if content quality is poor.

## Dependencies

### Auto-configured (no setup needed)

| Tool | Purpose | Auth |
|------|---------|------|
| **gh_grep MCP** | GitHub code search across 1M+ repos | None (anonymous) |
| **Context7 CLI** | Library documentation lookup | None (optional `CONTEXT7_API_KEY` for higher limits) |

Context7 runs via `npx ctx7@latest` — no global installation required.

**Context7 API Key (optional):** For higher rate limits, set the environment variable before starting Claude Code:
```bash
export CONTEXT7_API_KEY=your_key
```
Or login interactively: `npx ctx7@latest login`

### Recommended (best experience)

| Tool | Purpose | Install | Auth |
|------|---------|---------|------|
| **Exa plugin** | All web search and content extraction | `/plugin install exa@claude-plugins-official` | OAuth or API key |

With Exa, all search and fetch tasks use Exa for higher quality results. Without Exa, the toolkit falls back to WebSearch and WebFetch.

### Built-in (always available)

| Tool | Purpose |
|------|---------|
| **WebSearch** | General web search (fallback) |
| **WebFetch** | URL content extraction (fallback) |

## Installation

Add the marketplace, then install the plugin:

```
/plugin marketplace add claude-toolbox/marketplace
/plugin install web-search-toolkit@marketplace
```

### Recommended: Install Exa for Best Experience

```
/plugin install exa@claude-plugins-official
```

## Project Structure

```
web-search-toolkit/
├── .claude-plugin/
│   └── plugin.json              # Plugin manifest
├── .mcp.json                    # gh_grep MCP server declaration
├── commands/
│   ├── search.md                # /search — smart routing
│   ├── exa-search.md            # /exa-search — Exa semantic search
│   ├── deep-search.md           # /deep-search — multi-agent research
│   └── fetch.md                 # /fetch — URL content extraction
├── rules/
│   └── search-routing.mdc       # Always-active routing rules
├── skills/
│   └── search/
│       ├── SKILL.md             # Core routing skill
│       └── references/
│           └── routing-guide.md # Detailed decision tree
├── LICENSE
├── README.md
└── README.zh.md
```

## Fallback Behavior

| Task | Primary | Fallback |
|------|---------|----------|
| Quick facts | Exa `web_search_exa` | WebSearch |
| Simple URL | WebFetch | Exa `web_fetch_exa` |
| Complex/multiple URLs | Exa `web_fetch_exa` | WebFetch |
| Deep research | Exa `exa:search` skill | WebSearch parallel subagents |
| Library docs | Context7 CLI | WebSearch |
| Code search | gh_grep MCP | WebSearch |

## License

[MIT](./LICENSE)
