---
name: localstudios
description: >
  Website generation and SEO toolkit by LocalStudios. Use /localstudios generate <url>
  to scrape an existing website, conduct keyword research, create SEO-optimized content,
  generate schema markup, design the UI, and build the complete website via Google Stitch.
user-invokable: true
argument-hint: "[command] [url]"
license: MIT
metadata:
  author: LocalStudios
  version: "1.0.0"
  category: website-generation
---

# LocalStudios — Website Generation Plugin

## Available Commands

| Command | Description |
|---------|-------------|
| `generate <url>` | Generate a complete SEO-optimized website from an existing URL |

## Command Routing

Parse the first argument to determine the command:

- **`generate`**: Load `./references/generate-workflow.md` and execute the 12-phase website generation workflow. The second argument is the target URL to scrape and rebuild.

If no command is provided, show the available commands table above.

If an unknown command is provided, show: "Unknown command. Available commands:" followed by the table.

## Workflow: generate

When `generate` is invoked:

1. Read `./references/generate-workflow.md` for the complete workflow
2. Execute phases sequentially, pausing for user input where specified
3. Use lazy loading — only read reference files when their phase is reached

### Phase Overview
- **Phase 1** — Scrape and analyze the provided URL
- **Phase 2** — Interactive user interview (MANDATORY before proceeding)
- **Phase 3** — Keyword research via Semrush MCP (max 5 calls, graceful skip if unavailable)
- **Phase 4** — Multi-keyword geo-strategy per page
- **Phase 5** — SEO audit of existing site via claude-seo (skip if unavailable)
- **Phase 6** — Website structure outline (PAUSE for user approval)
- **Phase 7** — SEO-optimized content per page
- **Phase 8** — Schema markup generation (LocalBusiness, Service, Breadcrumb)
- **Phase 9** — Design system via ui-ux-pro-max (fallback to generic if unavailable)
- **Phase 10** — Website generation in Google Stitch (fallback to markdown export)
- **Phase 11** — Quality check
- **Phase 12** — Final report

### External Dependencies (all optional)

| Dependency | Check | Fallback |
|-----------|-------|----------|
| Semrush MCP | Check if `mcp__semrush__*` tools available | Use manual keywords from interview |
| claude-seo plugin | Check if `/seo` skill available | Skip audit phase |
| ui-ux-pro-max plugin | Check if `/ui-ux-pro-max` skill available | Use generic design system |
| Google Stitch MCP | Check if `mcp__stitch__*` tools available | Export as structured markdown |
| WebFetch | Check tool availability | User provides all info manually |
