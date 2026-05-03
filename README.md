# web-search-toolkit

<!-- README-I18N:START -->

**English** | [简体中文](./README.zh.md)

<!-- README-I18N:END -->

Unified search toolkit for Claude Code — intelligent routing across multiple search tools for optimal results and token efficiency.

## Features

- **Natural Language** — Just ask naturally, no command needed. "React useEffect 怎么用" activates Context7 automatically
- **Intelligent Routing** — Automatically selects the best search tool based on query type
- **Token Efficient** — Uses lightweight tools for simple queries, reserves heavy tools for complex research
- **Graceful Fallback** — If a tool is unavailable, falls back to the next best option
- **Zero Config** — Works out of the box with built-in tools; optional tools add extra capabilities

## Usage

No command needed — just ask in natural language:

- "React useEffect 怎么清理" → Context7 (library docs)
- "搜索 GitHub 上的 Next.js middleware 示例" → gh_grep (code search)
- "深度调研 2025 AI agent 框架" → Exa deep research
- "今天天气怎么样" → WebSearch (quick facts)
- "帮我看看 https://example.com" → WebFetch (URL extraction)

## Commands

| Command | Description | Tool |
|---------|-------------|------|
| `/web-search-toolkit:search` | Smart search with auto routing | Context7 / gh_grep / WebSearch |
| `/web-search-toolkit:exa-search` | Exa AI semantic search | Exa `web_search_exa` |
| `/web-search-toolkit:deep-search` | Deep multi-agent research | Exa `exa:search` skill |
| `/web-search-toolkit:fetch` | Fetch URL content | WebFetch / Exa `web_fetch_exa` |

### Natural Language Routing

The router classifies your query and picks the optimal tool:

| Query Type | Tool | Example |
|------------|------|---------|
| Library/API docs | Context7 CLI | "How to use React useEffect" |
| GitHub code search | gh_grep MCP | "Find Next.js middleware examples" |
| Quick facts | WebSearch | "What's new in React 19" |
| Exa semantic search | Exa `web_search_exa` | "用 Exa 搜索最新 AI 工具" |
| Deep research | Exa `exa:search` skill | "Comprehensive comparison of AI agents" |
| Simple URL | WebFetch | "帮我看看这个博客" |
| Complex/multiple URLs | Exa `web_fetch_exa` | "抓取这两个 SPA 页面" |

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

### Optional (enhances capabilities)

| Tool | Purpose | Install | Auth |
|------|---------|---------|------|
| **Exa plugin** | Deep multi-angle research | `claude install exa@claude-plugins-official` | OAuth or API key |

Without Exa, deep research falls back to parallel WebSearch queries.

### Built-in (always available)

| Tool | Purpose |
|------|---------|
| **WebSearch** | General web search |
| **WebFetch** | URL content extraction |

## Installation

Add the marketplace, then install the plugin:

```
/plugin marketplace add claude-toolbox/marketplace
/plugin install web-search-toolkit@marketplace
```

### Optional: Install Exa for Deep Research

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

Every tool has a fallback path:

- **Context7 fails** → WebSearch with library name + question
- **gh_grep unavailable** → WebSearch with `site:github.com`
- **Exa not installed** → WebSearch with parallel subagents
- **WebFetch fails** → Exa `web_fetch_exa` (if available)

## License

[MIT](./LICENSE)
