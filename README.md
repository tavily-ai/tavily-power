# Tavily Power for Kiro

Search the web, extract content from URLs, crawl websites, and conduct AI-powered research — all from within Kiro. This power gives Kiro AI agents access to Tavily's LLM-optimized APIs for real-time web data.

## About Tavily

[Tavily](https://tavily.com) is a search engine and API designed specifically for AI agents and LLMs. It delivers real-time web data — search results, page content, full-site crawls, and multi-source research reports — in formats optimized for agent consumption, with relevance scoring and metadata built in.

Tavily powers the retrieval layer for thousands of AI applications, from RAG pipelines to autonomous research agents.

## Features

- **Web Search**: Find current information with relevance scoring, domain filtering, and time-range controls
- **Content Extraction**: Pull clean markdown or text from any URL, including JavaScript-rendered pages
- **Site Crawling**: Extract content from entire websites with configurable depth, breadth, and semantic instructions
- **URL Mapping**: Discover a site's URL structure without extracting content — fast recon before targeted extraction
- **AI-Powered Research**: Comprehensive multi-source research reports with citations, powered by Tavily's research models

## Installation

### Prerequisites

1. **Tavily Account**: Sign up at [tavily.com](https://tavily.com)

### Kiro Power Setup

1. Open the Powers panel in Kiro
2. Click "Add power from GitHub"
3. Enter: `https://github.com/tavily-ai/tavily-power`
4. On first use, your browser will open for OAuth login — tokens are cached automatically

Alternatively, get an API key from your [Tavily dashboard](https://app.tavily.com) and set `TAVILY_API_KEY` in your environment.

## What Can You Do with Tavily and Kiro?

### 1 — Search the Web

Use prompts like:
- Search for the latest developments in quantum computing
- Find recent articles about Rust async patterns on arxiv.org and github.com
- What happened in AI regulation this month?

### 2 — Extract Content from URLs

Use prompts like:
- Extract the content from https://docs.example.com/api-reference
- Grab the authentication setup steps from this page
- Pull the text from these three URLs and summarize them

### 3 — Crawl Documentation Sites

Use prompts like:
- Crawl the docs at https://docs.example.com and find all API references
- Get all the content under /guides/ on this documentation site
- Map the URL structure of this site so I can decide what to extract

### 4 — Research a Topic

Use prompts like:
- Research the competitive landscape for AI code assistants
- What are the top 5 competitors to Notion in the SMB market?
- Investigate the pros and cons of server-side rendering vs. static generation

## Resources

- **Documentation**: [docs.tavily.com](https://docs.tavily.com)
- **API Dashboard**: [app.tavily.com](https://app.tavily.com)
- **Website**: [tavily.com](https://tavily.com)
