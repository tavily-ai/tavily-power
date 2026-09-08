---
inclusion: manual
---

# Tavily API Reference

Full parameter and response reference for all 5 Tavily MCP tools.

## tavily_search

Search the web for current information on any topic. Returns snippets and source URLs.

### Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `query` | string | **Required** | Search query |
| `max_results` | integer | 5 | Maximum number of results to return |
| `search_depth` | string | `"basic"` | `"basic"`, `"advanced"`, `"fast"`, `"ultra-fast"` |
| `topic` | string | `"general"` | Search category (only `"general"` supported) |
| `time_range` | string | null | `"day"`, `"week"`, `"month"`, `"year"` |
| `start_date` | string | `""` | Results after this date (`YYYY-MM-DD`) |
| `end_date` | string | `""` | Results before this date (`YYYY-MM-DD`) |
| `include_domains` | array | [] | Domains to include in results |
| `exclude_domains` | array | [] | Domains to exclude from results |
| `country` | string | `""` | Boost results from a country (full name, e.g. "United States") |
| `include_raw_content` | boolean | false | Include cleaned/parsed HTML of each result |
| `include_images` | boolean | false | Include query-related images |
| `include_image_descriptions` | boolean | false | Include images with AI descriptions |
| `include_favicon` | boolean | false | Include favicon URL per result |

### Search Depth

| Depth | Latency | Relevance | Best For |
|-------|---------|-----------|----------|
| `ultra-fast` | Lowest | Lower | Real-time chat, autocomplete |
| `fast` | Low | Good | Low latency with good relevance |
| `basic` | Medium | High | General-purpose |
| `advanced` | Higher | Highest | Precision queries |

### Response

```json
{
  "query": "quantum computing",
  "results": [
    {
      "title": "Page Title",
      "url": "https://example.com/page",
      "content": "Extracted text snippet...",
      "score": 0.85
    }
  ],
  "response_time": 1.2
}
```

---

## tavily_extract

Extract content from URLs. Returns raw page content in markdown or text format.

### Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `urls` | array | **Required** | List of URLs to extract content from |
| `extract_depth` | string | `"basic"` | `"basic"` or `"advanced"` (LinkedIn, protected sites, tables) |
| `format` | string | `"markdown"` | `"markdown"` or `"text"` |
| `query` | string | `""` | Query to rerank content chunks by relevance |
| `include_images` | boolean | false | Include images from pages |
| `include_favicon` | boolean | false | Include favicon URLs |

### Response

```json
{
  "results": [
    { "url": "https://...", "raw_content": "# Page content in markdown..." }
  ],
  "failed_results": [
    { "url": "https://...", "error": "Extraction failed" }
  ]
}
```

---

## tavily_crawl

Crawl a website starting from a URL. Extracts content from pages with configurable depth and breadth.

### Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `url` | string | **Required** | Root URL to begin crawl |
| `max_depth` | integer | 1 | How far from the base URL to explore (min 1) |
| `max_breadth` | integer | 20 | Links to follow per page (min 1) |
| `limit` | integer | 50 | Total pages to process before stopping (min 1) |
| `instructions` | string | `""` | Natural language instructions for which pages to return |
| `select_paths` | array | [] | Regex patterns to include (e.g. `["/docs/.*"]`) |
| `select_domains` | array | [] | Regex for domains to include (e.g. `["^docs\\.example\\.com$"]`) |
| `allow_external` | boolean | true | Include external domain links |
| `extract_depth` | string | `"basic"` | `"basic"` or `"advanced"` (tables, embedded content) |
| `format` | string | `"markdown"` | `"markdown"` or `"text"` |
| `include_favicon` | boolean | false | Include favicon URL per result |

### Response

```json
{
  "base_url": "https://docs.example.com",
  "results": [
    { "url": "https://docs.example.com/page", "raw_content": "..." }
  ]
}
```

---

## tavily_map

Map a website's structure. Returns a list of URLs found starting from the base URL. No content extraction.

### Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `url` | string | **Required** | Root URL to begin mapping |
| `max_depth` | integer | 1 | How far from the base URL to explore (min 1) |
| `max_breadth` | integer | 20 | Links to follow per page (min 1) |
| `limit` | integer | 50 | Total links to process before stopping (min 1) |
| `instructions` | string | `""` | Natural language instructions for the crawler |
| `select_paths` | array | [] | Regex patterns for URL paths to include |
| `select_domains` | array | [] | Regex for domains to include |
| `allow_external` | boolean | true | Include external domain links |

### Response

```json
{
  "base_url": "https://docs.example.com",
  "results": [
    "https://docs.example.com/api/auth",
    "https://docs.example.com/api/users"
  ]
}
```

---

## tavily_research

Perform comprehensive research on a given topic or question. Gathers information from multiple sources and returns a detailed report. Rate limit: 20 requests per minute.

### Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `input` | string | **Required** | Comprehensive description of the research task |
| `model` | string | `"auto"` | `"mini"` (narrow tasks), `"pro"` (broad tasks), `"auto"` |

### Response

Returns a detailed research report based on findings from multiple web sources.
