---
name: search
description: >-
  Unified search router. Activates automatically on any search, lookup, research,
  or documentation query — no command needed, just ask in natural language.
  Routes to the optimal tool: Context7 for library docs, gh_grep for GitHub code
  search, Exa for deep research, WebSearch for quick facts, WebFetch for URL
  extraction. Also callable via /web-search-toolkit:search.
---

# Unified Search Router

Analyze the user's query, select the optimal search tool, execute, and return formatted results.

## Step 1: Determine Search Type

Read the user's query and classify it:

| Type | Signals | Route To |
|------|---------|----------|
| **Library docs** | Mentions a library/framework/SDK + usage question ("how to", "怎么用", "API", "配置") | Context7 |
| **Code search** | "代码", "实现", "示例", "code", "implementation", "example", find code patterns | gh_grep |
| **Deep research** | "深度", "调研", "研究", "deep dive", "research", "comprehensive", multi-angle topics | Exa |
| **URL extraction** | User pasted a URL | WebFetch |
| **Quick fact** | General question, news, facts, "what is", "搜索", "查找" | WebSearch |

When ambiguous, prefer the lighter tool (WebSearch) over heavier ones.

## Step 2: Execute by Type

### Library Docs → Context7 CLI

Use the `ctx7` CLI to retrieve current documentation. Run via `npx` to avoid requiring global installation.

**Environment setup (optional):**
The CLI works without authentication. For higher rate limits, set `CONTEXT7_API_KEY` in your environment:
```bash
export CONTEXT7_API_KEY=your_key
```
Or login interactively: `npx ctx7@latest login`

**Workflow (max 3 calls total):**

```bash
# Step 1: Resolve library ID
npx ctx7@latest library <name> "<query about what user wants>"

# Step 2: Get docs
npx ctx7@latest docs <libraryId> "<detailed query>"
```

Rules:
- Always call `library` first to get a valid ID (format: `/org/project`)
- Use the user's full question as the query, not single words
- If result is insufficient, retry once with `--research` flag
- Do NOT exceed 3 total calls. If still unsatisfied after 3 calls, use the best result
- On quota error: inform user, suggest `npx ctx7@latest login`, then fall back to WebSearch

**Fallback:** If ctx7 fails or is unavailable, use WebSearch with query `"<library name> <user question> official documentation"`.

### Code Search → gh_grep MCP

Use the `mcp__gh_grep__searchGitHub` MCP tool to search GitHub code.

**Tool: `mcp__gh_grep__searchGitHub`**
- `query` (required): Literal code pattern that would appear in files (e.g., `useState(`, `import React from`), NOT natural language
- `language`: Filter by language, array format (e.g., `["TypeScript", "TSX"]`, `["Python"]`)
- `repo`: Filter by repository (e.g., `facebook/react`, `vercel/`)
- `path`: Filter by file path (e.g., `src/components/Button.tsx`)
- `useRegexp`: Enable regex mode. Use `(?s)` prefix for multi-line matching (e.g., `(?s)useEffect\\(\\(\\) => {.*removeEventListener`)
- `matchCase`: Case-sensitive search
- `matchWholeWords`: Match whole words only

**Tips:**
- Search for actual code patterns, not keywords or questions
- Use `language` array when the user mentions a language
- Use `repo` when searching within a specific project
- Use regex for complex patterns: `(?s)try {.*await` matches multi-line blocks

**Fallback:** If gh_grep MCP is unavailable, use WebSearch with `site:github.com <query>`.

### Deep Research → Exa

Check if the Exa plugin is available by looking for `exa:search` in the skill list.

**If available:** Invoke the `exa:search` skill, passing the user's full query. The skill handles subagent dispatch, multi-pass search, and result compilation internally.

**If not available:** Use WebSearch with subagents for parallel searches:
1. Break the topic into 2-3 sub-questions
2. Dispatch parallel Agent subagents, each running WebSearch on a sub-question
3. Compile and deduplicate results
4. Present with sources

### URL Extraction → WebFetch

**Single URL:** Use the built-in `WebFetch` tool directly.

```
WebFetch(url=<user's URL>, prompt="Extract and summarize the key content")
```

**Multiple URLs (2+):** If Exa plugin is available, use `mcp__plugin_exa_exa__web_fetch_exa` for batch fetching — one call handles all URLs, more efficient than parallel WebFetch calls.

```
mcp__plugin_exa_exa__web_fetch_exa(urls=[url1, url2, ...], maxCharacters=3000)
```

If Exa is not available, fall back to parallel WebFetch calls.

**Fallback:** If WebFetch fails for a single URL, try `mcp__plugin_exa_exa__web_fetch_exa` if Exa is available.

### Quick Fact → WebSearch

Use the built-in `WebSearch` tool directly.

```
WebSearch(query=<refined query>)
```

Tips:
- Refine the query for better results (add context, specificity)
- Use `allowed_domains` if the user specifies a site
- For recent events, the tool handles date filtering automatically

## Step 3: Format Results

Always present results with:
1. **Answer** — Direct, concise response to the question
2. **Sources** — Numbered list with URLs
3. **Notes** (optional) — Tool used, fallback triggered, caveats

Example format:
```
[Answer content]

Sources:
1. [Title](URL)
2. [Title](URL)

---
Searched via: Context7 | gh_grep | Exa | WebSearch | WebFetch
```
