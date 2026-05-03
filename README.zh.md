# web-search-toolkit

<!-- README-I18N:START -->

[English](./README.md) | **简体中文**

<!-- README-I18N:END -->

Claude Code 统一搜索工具包 — 智能路由多个搜索工具，实现最佳结果和 token 效率。

## 功能

- **自然语言** — 直接用自然语言提问，无需输入命令。"React useEffect 怎么用" 自动触发 Context7
- **智能路由** — 根据查询类型自动选择最佳搜索工具
- **节省 Token** — 简单查询用轻量工具，复杂研究才用重量级工具
- **优雅降级** — 工具不可用时自动切换到次优方案
- **零配置** — 内置工具开箱即用，可选工具额外增强能力

## 使用方式

无需命令，直接用自然语言提问：

- "React useEffect 怎么清理" → Context7（库文档）
- "搜索 GitHub 上的 Next.js middleware 示例" → gh_grep（代码搜索）
- "深度调研 2025 AI agent 框架" → Exa 深度研究
- "今天天气怎么样" → WebSearch（快速事实）
- "帮我看看 https://example.com" → WebFetch（URL 提取）

## 命令

| 命令 | 说明 | 工具 |
|------|------|------|
| `/web-search-toolkit:search` | 智能路由搜索 | Context7 / gh_grep / WebSearch |
| `/web-search-toolkit:exa-search` | Exa AI 语义搜索 | Exa `web_search_exa` |
| `/web-search-toolkit:deep-search` | 多 agent 深度研究 | Exa `exa:search` skill |
| `/web-search-toolkit:fetch` | 抓取 URL 内容 | WebFetch / Exa `web_fetch_exa` |

### 自然语言路由

路由器分析查询类型，选择最优工具：

| 查询类型 | 工具 | 示例 |
|----------|------|------|
| 库/API 文档 | Context7 CLI | "React useEffect 怎么用" |
| GitHub 代码搜索 | gh_grep MCP | "找 Next.js middleware 示例" |
| 快速事实查询 | WebSearch | "React 19 有什么新特性" |
| Exa 语义搜索 | Exa `web_search_exa` | "用 Exa 搜索最新 AI 工具" |
| 深度研究 | Exa `exa:search` skill | "全面对比 AI agent 框架" |
| 简单页面 | WebFetch | "帮我看看这个博客" |
| 复杂/多 URL | Exa `web_fetch_exa` | "抓取这两个 SPA 页面" |

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

### 可选（增强能力）

| 工具 | 用途 | 安装方式 | 认证 |
|------|------|----------|------|
| **Exa 插件** | 深度多角度研究 | `claude install exa@claude-plugins-official` | OAuth 或 API key |

未安装 Exa 时，深度研究会降级为并行 WebSearch 查询。

### 内置（始终可用）

| 工具 | 用途 |
|------|------|
| **WebSearch** | 通用网页搜索 |
| **WebFetch** | URL 内容提取 |

## 安装

先添加 marketplace，然后安装插件：

```
/plugin marketplace add claude-toolbox/marketplace
/plugin install web-search-toolkit@marketplace
```

### 可选：安装 Exa 以启用深度研究

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
│   ├── search.md                # /search — 智能路由
│   ├── exa-search.md            # /exa-search — Exa 语义搜索
│   ├── deep-search.md           # /deep-search — 多 agent 深度研究
│   └── fetch.md                 # /fetch — URL 内容提取
├── rules/
│   └── search-routing.mdc       # 始终激活的路由规则
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

每个工具都有降级路径：

- **Context7 失败** → WebSearch 搜索库名 + 问题
- **gh_grep 不可用** → WebSearch 加 `site:github.com`
- **Exa 未安装** → WebSearch 并行子 agent 搜索
- **WebFetch 失败** → Exa `web_fetch_exa`（如可用）

## 许可证

[MIT](./LICENSE)
