---
name: tavily-mcp
description: Use the Tavily MCP server connected by this power for web search, URL extraction, site mapping, crawling, and research. Use when Tavily MCP tools are available or the user needs help connecting the power. Use tavily-cli for CLI setup and commands, and tavily-best-practices for application SDK integrations.
compatibility: Requires a Tavily MCP connection managed by the host client and network access.
---

# Tavily MCP in Kiro

The power's root `mcp.json` connects to `https://mcp.tavily.com/mcp` using
Streamable HTTP. Complete the host's OAuth flow when prompted. Authentication
for this MCP connection is separate from Tavily CLI authentication.

Use the connected tools for web tasks when available. Inspect their actual
names and schemas before calling them; Kiro may namespace server and tool names.
Do not translate a CLI command into an MCP call by copying its flags directly.
For parameter details, read [the MCP API reference](references/api-reference.md)
only as needed. The live tool schema takes precedence over the bundled reference.

| Need | Tavily tool | Required input |
| --- | --- | --- |
| Find pages on a topic | `tavily_search` | `query` |
| Read known pages | `tavily_extract` | `urls` |
| Find URLs on a known site | `tavily_map` | `url` |
| Collect a site's relevant section | `tavily_crawl` | `url` |
| Produce a multi-source report | `tavily_research` | `input` |

## Workflow

1. Search with short, focused queries. Use source and date filters when relevant.
2. Select the strongest results, then extract the pages needed to verify claims.
3. Map a known domain when useful pages are hard to locate. Crawl only a scoped
   section, with an explicit page limit and conservative depth.
4. Reserve research for requests that need substantial cited synthesis.
5. Cite supporting URLs and describe material evidence gaps or failed sources.

For outcome-specific planning, load the relevant neighboring skill, such as
[product competitor intelligence](../product-competitor-intelligence/SKILL.md)
or [academic research](../academic-scientific-research/SKILL.md).

## CLI-specific workflows

The bundled endpoint skills use `tvly`. Follow
[CLI setup](../tavily-cli/SKILL.md) when the requested workflow needs its local
JSON processing, saved files, or research status commands. Do not require a CLI
installation for a task that the connected MCP tools can complete.

Monitoring needs an external scheduler; installing this power does not create
one. Past research lookup uses a local log, not a hosted history service.

## Connection problems

- If authorization fails, reconnect the Tavily server through the host's
  authentication controls. Avoid deleting shared MCP credential caches.
- Setting `TAVILY_API_KEY` alone does not configure the remote server in this
  package. Keep credentials in the host's credential settings or use the CLI;
  do not place API keys in the distributed manifest or MCP configuration.
- If an extraction fails, check the failed URL and try advanced extraction
  when supported. Report inaccessible sources rather than inventing content.
