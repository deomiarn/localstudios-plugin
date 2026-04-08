# LocalStudios Plugin for Claude Code

Website generation plugin that scrapes existing websites, conducts SEO keyword research, creates optimized content, generates schema markup, and builds complete websites.

## Installation

Add the marketplace to your Claude Code settings:

```bash
# In Claude Code, run:
/install localstudios@localstudios-marketplace
```

Or manually add to `~/.claude/settings.json`:

```json
{
  "enabledPlugins": {
    "localstudios@localstudios-marketplace": true
  },
  "extraKnownMarketplaces": {
    "localstudios-marketplace": {
      "source": {
        "source": "github",
        "repo": "deomiarn/localstudios-plugin"
      }
    }
  }
}
```

## Usage

```
/localstudios generate <url>
```

### What it does

1. **Scrapes** the existing website for business info, services, NAP data, brand elements
2. **Interviews** you to fill gaps and confirm details
3. **Researches keywords** via Semrush MCP (max 5 API calls, optional)
4. **Maps keywords** to pages with geo-cluster strategy
5. **Audits** the existing site for SEO issues (via claude-seo, optional)
6. **Plans** the complete page structure and gets your approval
7. **Writes** SEO-optimized content for every page (E-E-A-T, internal linking)
8. **Generates** schema markup (LocalBusiness, Service, FAQ, Breadcrumb)
9. **Creates** a design system (via ui-ux-pro-max, optional)
10. **Builds** the website in Google Stitch (optional, falls back to markdown export)
11. **Checks** quality against 40+ criteria
12. **Reports** everything with next steps for the client

### Interactive Pauses

The workflow pauses for your input at two points:
- After Phase 2 (Interview) — confirm the Project Brief
- After Phase 6 (Outline) — approve the page structure before content writing

## Optional Dependencies

All external tools are optional. The plugin works without them but provides richer results when available.

| Tool | What it adds | Install |
|------|-------------|---------|
| Semrush MCP | Real keyword volume and difficulty data | Configure Semrush MCP server |
| claude-seo | SEO audit of existing site | `curl -fsSL https://raw.githubusercontent.com/AgriciDaniel/claude-seo/main/install.sh \| bash` |
| ui-ux-pro-max | Industry-specific design system | `npm install -g uipro-cli && uipro init --ai claude --global` |
| Google Stitch MCP | Visual website generation | Configure Stitch MCP server |

## License

MIT
