---
name: "tavily"
displayName: "Web Search with Tavily"
description: "Search the web, extract content from URLs, crawl websites, and conduct AI-powered research using Tavily's LLM-optimized APIs"
keywords: ["search", "web-search", "tavily", "web", "lookup", "find", "internet", "extract", "crawl", "scrape", "research", "rag", "realtime", "latest"]
author: "Tavily"
---

# Onboarding

Before using Tavily, you need a Tavily account. Sign up at [tavily.com](https://tavily.com) if you don't have one. Authentication is handled via OAuth — on first MCP connection, your browser will open for login. Tokens are cached in `~/.mcp-auth/`.

Alternatively, get an API key from your Tavily dashboard and set `TAVILY_API_KEY` in your environment.

# Overview

Tavily provides LLM-optimized APIs for web search, content extraction, site crawling, and AI-powered research. Results include relevance scores, content snippets, and metadata designed for agent consumption.

**Key capabilities:**

- **Search**: Web search with relevance scoring and domain/time filtering
- **Extract**: Pull content from specific URLs in markdown or text
- **Crawl**: Extract content from entire websites with configurable depth
- **Map**: Discover URLs on a site without extracting content
- **Research**: End-to-end AI-powered research from multiple sources

**Authentication**: OAuth via MCP server (auto-prompted on first use), or `TAVILY_API_KEY` environment variable.

## Available Steering Files

- **api-reference** — Full parameter and response reference for all 5 Tavily MCP tools

## Available MCP Servers

### tavily

**Connection:** HTTPS API endpoint at `https://mcp.tavily.com/mcp`
**Authorization:** OAuth (browser login on first use) or Bearer token via `TAVILY_API_KEY`

**Tools:**

1. **tavily_search** — Search the web for current information on any topic
   - Required: `query` (string)
   - Optional: `max_results` (default 5), `search_depth` ("basic"/"advanced"/"fast"/"ultra-fast"), `time_range`, `include_domains`, `exclude_domains`, `country`, `start_date`/`end_date`, `include_raw_content`, `include_images`, `include_image_descriptions`, `include_favicon`

2. **tavily_extract** — Extract content from URLs in markdown or text
   - Required: `urls` (array of strings)
   - Optional: `extract_depth` ("basic"/"advanced"), `format` ("markdown"/"text"), `query` (reranks chunks), `include_images`, `include_favicon`

3. **tavily_crawl** — Crawl a website starting from a URL
   - Required: `url` (string)
   - Optional: `max_depth` (default 1), `max_breadth` (default 20), `limit` (default 50), `instructions`, `select_paths`, `select_domains`, `allow_external`, `extract_depth`, `format`, `include_favicon`

4. **tavily_map** — Map a website's URL structure (no content extraction)
   - Required: `url` (string)
   - Optional: `max_depth`, `max_breadth`, `limit`, `instructions`, `select_paths`, `select_domains`, `allow_external`

5. **tavily_research** — Comprehensive multi-source research on a topic
   - Required: `input` (string) — description of the research task
   - Optional: `model` ("mini"/"pro"/"auto", default "auto")
   - Rate limit: 20 requests/minute

## Tool Usage Examples

### Searching the Web

**Basic search:**

```
tavily_search({ "query": "latest developments in quantum computing" })
```

**Search with time filter and domain restriction:**

```
tavily_search({
  "query": "machine learning best practices",
  "search_depth": "advanced",
  "time_range": "month",
  "include_domains": ["arxiv.org", "github.com"],
  "max_results": 10
})
```

**Search with date range:**

```
tavily_search({
  "query": "AI regulation updates",
  "start_date": "2026-01-01",
  "end_date": "2026-03-01"
})
```

### Extracting Content from URLs

**Basic extraction:**

```
tavily_extract({ "urls": ["https://docs.example.com/api-reference"] })
```

**Advanced extraction with relevance targeting:**

```
tavily_extract({
  "urls": ["https://example.com/long-article"],
  "extract_depth": "advanced",
  "query": "authentication setup steps"
})
```

### Crawling a Website

**Crawl documentation site:**

```
tavily_crawl({
  "url": "https://docs.example.com",
  "max_depth": 2,
  "instructions": "Find API documentation and code examples",
  "select_paths": ["/docs/.*", "/api/.*"]
})
```

### Mapping a Website

**Discover site structure before crawling:**

```
tavily_map({
  "url": "https://docs.example.com",
  "max_depth": 2,
  "instructions": "Find all API and guide pages"
})
```

### Researching a Topic

**Focused research:**

```
tavily_research({
  "input": "What are the top 5 competitors to Notion in the SMB market and how do they differentiate?",
  "model": "mini"
})
```

**Comprehensive research:**

```
tavily_research({
  "input": "Analyze the competitive landscape for AI code assistants, including key players, pricing models, market positioning, and recent product moves",
  "model": "pro"
})
```

## Combining Tools (Workflows)

### Workflow 1: Find and Read Specific Content

1. Search for the topic with `tavily_search`
2. Pick the top results by relevance score
3. Extract full content from those URLs with `tavily_extract`, using `query` to focus on the relevant parts

### Workflow 2: Build Knowledge from a Docs Site

1. Map the site structure with `tavily_map` to discover all pages
2. Filter the discovered URLs to relevant paths (e.g. only `/api/` or `/guides/`)
3. Extract content from filtered URLs with `tavily_extract`

### Workflow 3: Research Then Verify

1. Run `tavily_research` for a comprehensive report on the topic
2. Use `tavily_search` to verify specific claims or find more recent data
3. Use `tavily_extract` on the most relevant sources for full content

### Workflow 4: Monitor Recent Developments

1. Search with `tavily_search` using `time_range: "day"` or `"week"`
2. Filter results by score to get the most relevant ones
3. Extract full content from top results with `tavily_extract`

## Best Practices

### ✅ Do

- **Keep search queries concise** — think search query, not long prompt
- **Break complex queries into sub-queries** — multiple focused searches beat one giant query
- **Use `search_depth: "advanced"`** when precision matters
- **Use `include_domains`** to restrict to trusted sources
- **Use `time_range` or `start_date`/`end_date`** for recent information
- **Filter results by `score`** — discard below 0.5 for higher quality
- **Use `query` in extract** to rerank chunks by relevance
- **Start crawls conservative** — `max_depth=1`, `max_breadth=20`, scale up if needed
- **Always set `limit` on crawls** to prevent runaway operations
- **Use `instructions` in crawl/map** for semantic focus

### ❌ Don't

- **Don't write giant queries** — search queries over 400 chars perform poorly
- **Don't skip score filtering** — low-score results add noise
- **Don't crawl with high depth blindly** — depth 3+ is exponentially slower
- **Don't forget `limit` on crawls** — can lead to unexpected costs
- **Don't use `extract_depth: "basic"` for JS-heavy sites** — use `"advanced"` for LinkedIn, protected sites, dynamic content
- **Don't fire many `tavily_research` calls in parallel** — rate limited to 20/minute

## Troubleshooting

### Error: "Authentication failed" / "invalid_token"

**Cause:** Token expired or not yet obtained
**Solution:**

1. Clear cached tokens in `~/.mcp-auth/` (WARNING: This will also log you out of other MCP servers. Consider adding an environment variable instead.)
2. Reconnect — browser will open for OAuth login
3. Or set `TAVILY_API_KEY` environment variable directly

### Error: No results returned

**Cause:** Query too specific or restrictive filters
**Solution:**

1. Broaden the query
2. Remove `include_domains` or `time_range` restrictions
3. Try `search_depth: "basic"` instead of `"advanced"`

### Extract returns in `failed_results`

**Cause:** Site blocks basic extraction or is JS-rendered
**Solution:**

1. Retry with `extract_depth: "advanced"`
2. Check if the URL is accessible (not behind auth/paywall)

### Crawl returns few or no results

**Cause:** Depth too shallow or paths too restrictive
**Solution:**

1. Increase `max_depth` (try 2)
2. Increase `max_breadth`
3. Broaden or remove `select_paths` filters
4. Check `allow_external` if you need cross-domain links

## Configuration

**Authentication**: OAuth (recommended) or API key

**Setup Steps:**

1. Create a Tavily account at [tavily.com](https://tavily.com)
2. On first MCP connection, complete OAuth login in browser
3. Or: get API key from dashboard → set `TAVILY_API_KEY` in environment

**MCP Configuration:**

```json
{
  "mcpServers": {
    "tavily": {
      "url": "https://mcp.tavily.com/mcp"
    }
  }
}
```

## Resources

- [Tavily API Documentation](https://docs.tavily.com)
- [Search API Reference](https://docs.tavily.com/documentation/api-reference/endpoint/search)
- [Extract API Reference](https://docs.tavily.com/documentation/api-reference/endpoint/extract)
- [Crawl API Reference](https://docs.tavily.com/documentation/api-reference/endpoint/crawl)
- [Map API Reference](https://docs.tavily.com/documentation/api-reference/endpoint/map)
- [Research API Reference](https://docs.tavily.com/documentation/api-reference/endpoint/research)

## License and Support

This power integrates with [Tavily MCP Server](https://github.com/tavily-ai/tavily-mcp) (MIT).

- [Privacy Policy](https://tavily.com/privacy)
- [Support](https://docs.tavily.com)