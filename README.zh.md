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
| `/web-search-toolkit:init` | 将插件规则安装到 `.claude/rules/` | — | — |
| `/web-search-toolkit:search` | 智能路由搜索 | Context7 / gh_grep / Exa | WebSearch / WebFetch |
| `/web-search-toolkit:exa-search` | 语义搜索 | Exa `web_search_exa` | WebSearch |
| `/web-search-toolkit:deep-search` | 多 agent 深度研究 | Exa `exa:search` skill | WebSearch 并行子 agent |
| `/web-search-toolkit:fetch` | 抓取 URL 内容 | Exa `web_fetch_exa` | WebFetch |

### 自然语言路由

路由器分析查询类型，选择最优工具：

| 查询类型 | 工具 | 降级方案 | 示例 |
|----------|------|----------|------|
| 库/API 文档 | Context7 CLI | WebSearch | "React useEffect 怎么用" |
| GitHub 代码搜索 | gh_grep MCP | WebSearch | "找 Next.js middleware 示例" |
| 快速事实查询 | Exa `web_search_exa` | WebSearch | "React 19 有什么新特性" |
| 深度研究 | Exa `exa:search` skill | WebSearch 并行子 agent | "全面对比 AI agent 框架" |
| 简单 URL | WebFetch | Exa `web_fetch_exa` | "帮我看看这个博客" |
| 复杂/多 URL | Exa `web_fetch_exa` | WebFetch | "抓取这两个 SPA 页面" |

**策略：** 模糊搜索（发现信息、事实查询、深度研究）优先使用 Exa 以确保质量。已知 URL 优先使用 WebFetch（直接、轻量），内容质量不佳时再用 Exa 重试。

## 依赖

### 自动配置（无需手动安装）

| 工具 | 用途 | 认证 |
|------|------|------|
| **gh_grep MCP** | 搜索百万级 GitHub 仓库代码 | 无需认证（匿名访问） |
| **Context7 CLI** | 库文档查询 | 无需认证（可选 `CONTEXT7_API_KEY` 提升配额） |

Context7 通过 `npx ctx7@latest` 运行，无需全局安装。

**Context7 API Key（可选）：** 如需更高配额，启动 Claude Code 前设置环境变量：
```bash
export CONTEXT7_API_KEY=your_key
```
或交互式登录：`npx ctx7@latest login`

### 推荐安装（最佳体验）

| 工具 | 用途 | 安装方式 | 认证 |
|------|------|----------|------|
| **Exa 插件** | 所有网页搜索和内容提取 | `/plugin install exa@claude-plugins-official` | OAuth 或 API key |

安装 Exa 后，所有搜索和抓取任务均使用 Exa 以获得更高质量的结果。未安装时降级到 WebSearch 和 WebFetch。

### 内置（始终可用）

| 工具 | 用途 |
|------|------|
| **WebSearch** | 通用网页搜索（降级方案） |
| **WebFetch** | URL 内容提取（降级方案） |

## 安装

先添加 marketplace，然后安装插件：

```
/plugin marketplace add claude-toolbox/marketplace
/plugin install web-search-toolkit@marketplace
```

### 推荐：安装 Exa 以获得最佳体验

```
/plugin install exa@claude-plugins-official
```

## 项目结构

```
web-search-toolkit/
├── .claude-plugin/
│   └── plugin.json              # 插件清单
├── .mcp.json                    # gh_grep MCP 服务器声明
├── commands/
│   ├── init.md                  # /init — 安装规则
│   ├── search.md                # /search — 智能路由
│   ├── exa-search.md            # /exa-search — Exa 语义搜索
│   ├── deep-search.md           # /deep-search — 多 agent 深度研究
│   └── fetch.md                 # /fetch — URL 内容提取
├── rules-templates/
│   └── search-routing.md        # 路由规则（通过 /init 安装）
├── skills/
│   └── search/
│       ├── SKILL.md             # 核心路由 skill
│       └── references/
│           └── routing-guide.md # 详细决策树
├── LICENSE
├── README.md
└── README.zh.md
```

## 降级行为

| 任务 | 主要工具 | 降级方案 |
|------|----------|----------|
| 快速事实查询 | Exa `web_search_exa` | WebSearch |
| 简单 URL | WebFetch | Exa `web_fetch_exa` |
| 复杂/多 URL | Exa `web_fetch_exa` | WebFetch |
| 深度研究 | Exa `exa:search` skill | WebSearch 并行子 agent |
| 库文档查询 | Context7 CLI | WebSearch |
| 代码搜索 | gh_grep MCP | WebSearch |

## 许可证

[MIT](./LICENSE)
