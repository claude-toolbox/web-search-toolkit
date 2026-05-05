# web-search-toolkit

<!-- README-I18N:START -->

[English](./README.md) | **简体中文**

<!-- README-I18N:END -->

Claude Code 统一搜索工具包 — 智能路由，Exa 优先策略，实现最高质量搜索结果。

## 功能

- **自然语言** — 直接用自然语言提问，无需输入命令。"React useEffect 怎么用" 自动触发 Context7
- **Exa 优先** — 安装 Exa 插件后，所有网页搜索和内容提取均使用 Exa，确保最高质量
- **智能路由** — 根据查询类型自动选择最佳搜索工具
- **优雅降级** — 未安装 Exa 时，自动降级到 WebSearch 和 WebFetch
- **零配置** — 内置工具开箱即用，推荐安装 Exa 插件获得最佳体验

## 使用方式

无需命令，直接用自然语言提问：

- "React useEffect 怎么清理" → Context7（库文档）
- "搜索 GitHub 上的 Next.js middleware 示例" → gh_grep（代码搜索）
- "深度调研 2025 AI agent 框架" → Exa 深度研究
- "今天天气怎么样" → Exa 搜索（未安装 Exa 时降级为 WebSearch）
- "帮我看看 https://example.com" → Exa 抓取（未安装 Exa 时降级为 WebFetch）

## 命令

| 命令 | 说明 | 主要工具 | 降级方案 |
|------|------|----------|----------|
| `/web-search-toolkit:search` | 智能路由搜索 | Context7 / gh_grep / Exa | WebSearch / WebFetch |
| `/web-search-toolkit:exa-search` | 语义搜索 | Exa `web_search_exa` | WebSearch |
| `/web-search-toolkit:deep-search` | 多 agent 深度研究 | Exa `exa:search` skill | WebSearch 并行子 agent |
| `/web-search-toolkit:fetch` | 抓取 URL 内容 | Exa `web_fetch_exa` | WebFetch |

## 依赖

### 自动配置（无需手动安装）

| 工具 | 用途 | 认证 |
|------|------|------|
| **gh_grep MCP** | 搜索百万级 GitHub 仓库代码 | 无需认证（匿名访问） |
| **Context7 CLI** | 库文档查询 | 无需认证（可选 `CONTEXT7_API_KEY` 提升配额） |

Context7 通过 `npx ctx7@latest` 运行，无需全局安装。

### 推荐安装（最佳体验）

| 工具 | 用途 | 安装方式 |
|------|------|----------|
| **Exa 插件** | 所有网页搜索和内容提取 | `/plugin install exa@claude-plugins-official` |

## 安装

```
/plugin marketplace add claude-toolbox/marketplace
/plugin install web-search-toolkit@marketplace
```

## 项目结构

```
web-search-toolkit/
├── agents/
│   └── web-search.md              # 搜索路由 agent（自动加载）
├── commands/
│   ├── search.md                  # /search — 智能路由
│   ├── exa-search.md              # /exa-search — Exa 语义搜索
│   ├── deep-search.md             # /deep-search — 多 agent 深度研究
│   └── fetch.md                   # /fetch — URL 内容提取
├── skills/
│   └── search/
│       ├── SKILL.md               # 核心路由 skill
│       └── references/
│           └── routing-guide.md   # 详细决策树
├── .claude-plugin/plugin.json
├── .mcp.json
├── LICENSE
├── README.md
└── README.zh.md
```

## 许可证

[MIT](./LICENSE)
