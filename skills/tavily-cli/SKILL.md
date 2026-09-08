---
name: tavily-cli
description: |
  Set up, authenticate, update, troubleshoot, or choose between Tavily CLI web commands. Use when the user asks about the Tavily CLI, installing Tavily skills, first-time setup, authentication, keyless limits, CLI updates, or which Tavily command to use. For an ordinary web task, use the specific search, extract, map, crawl, research, or dynamic-search skill instead.
compatibility: Requires Tavily CLI (tvly) and network access; authenticated access for map, crawl, and research.
---

# Tavily CLI

Web search, content extraction, site crawling, URL discovery, and deep research. Returns JSON optimized for LLM consumption.

Requires `tavily-cli`. Search and extract support capped keyless access; map,
crawl, and research require authentication.

Run `tvly --help` or `tvly <command> --help` for full option details.

## Setup

This power already bundles the skills. Install only the CLI if `tvly` is missing:

```bash
uv tool install tavily-cli
# Alternative when uv is unavailable:
# pip install tavily-cli
```

Use `tvly --help` and `tvly <command> --help` to check the installed CLI's options.
Do not run the default skill installer or bare `tvly init` from this power;
those can install another copy of these skills into other editors.

Search and extract support capped keyless access. Try the requested command
without authentication first. Optional setup and verification with the already
bundled skills:

```bash
tvly init --skip-skills --skip-auth
```

Map, crawl, and research require authentication. When it is needed, use:

```bash
tvly login
# Or guided authentication and verification without reinstalling skills:
tvly init --skip-skills
```

If a search or extract reaches the keyless cap in an interactive session, use
`tvly login`, then retry the original request once. In an unattended environment,
report the authentication requirement instead of opening an interactive flow.
Check the current state only when needed with `tvly --status --json`.

Browser OAuth requires a localhost callback on the machine running `tvly`.
`tvly login --no-browser` prints the sign-in link; remote sessions may require
port forwarding. For unattended use or API-key-only operations, use a securely
provided `TAVILY_API_KEY`. Do not print credentials or write them to this power.

CLI credentials and the MCP connection's host-managed credentials are separate.
Use [tavily-mcp](../tavily-mcp/SKILL.md) for connection guidance.

Check for CLI updates with `tvly update --check`; use `tvly update` when an update
is requested or needed. Update this power to refresh its bundled skills.

## Workflow

Follow this escalation pattern — start simple, escalate when needed:

1. **Search** — No specific URL. Find pages, answer questions, discover sources.
2. **Extract** — Have a URL. Pull its content directly.
3. **Map** — Large site, need to find the right page. Discover URLs first.
4. **Crawl** — Need bulk content from an entire site section.
5. **Research** — Need comprehensive, multi-source analysis with citations.

| Need | Command | When |
|------|---------|------|
| Find pages on a topic | `tvly search` | No specific URL yet |
| Get a page's content | `tvly extract` | Have a URL |
| Find URLs within a site | `tvly map` | Need to locate a specific subpage |
| Bulk extract a site section | `tvly crawl` | Need many pages (e.g., all /docs/) |
| Deep research with citations | `tvly research` | Need multi-source synthesis |

For detailed command reference, use the individual skill for each command (e.g., `tavily-search`, `tavily-crawl`) or run `tvly <command> --help`.

Run `tvly` without a subcommand for the interactive REPL.

## Output

Search, extract, crawl, map, and research support `--json` for structured output.
Result-producing commands support `-o` to save the JSON response; crawl also
supports `--output-dir` for one Markdown file per page. Setup, authentication,
status, and update commands expose `--json` where documented but do not support
`-o`.

```bash
tvly search "react hooks" --json -o results.json
tvly extract "https://example.com/docs" -o docs.json
tvly crawl "https://docs.example.com" --output-dir ./docs/
```

## Tips

- **Always quote URLs** — shell interprets `?` and `&` as special characters.
- **Use `--json` for agentic workflows** when the selected command exposes it.
- **Read from stdin with `-`** — `echo "query" | tvly search -`
- **Exit codes**: 0 = success, 1 = setup/update failure, 2 = bad input, 3 = auth error, 4 = API or live-verification error.
