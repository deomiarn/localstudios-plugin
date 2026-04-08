# LocalStudios Plugin for Claude Code

Generates a single-page homepage as a client pitch. Scrapes the existing site, researches keywords, creates SEO-optimized content, and builds a polished Next.js homepage with 21st.dev components.

## Installation

Add to `~/.claude/settings.json`:

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

1. **Preflight** — auto-configures MCPs, guides 21st.dev API key setup
2. **Scrapes** the existing website (max 7 requests, fast)
3. **Interviews** you to fill gaps and confirm details
4. **Researches keywords** via Semrush MCP (max 5 API calls)
5. **Plans** the homepage structure (8 mandatory sections)
6. **Writes** SEO-optimized content (E-E-A-T, geo-signals)
7. **Generates** schema markup (LocalBusiness, WebSite)
8. **Creates** design system via ui-ux-pro-max
9. **Builds** homepage with 21st.dev polished components
10. **Checks** quality with /seo validation
11. **Reports** with next steps

### Interactive Pauses

The workflow pauses for your input at:
- After Preflight — API key setup if needed
- After Interview — confirm Project Brief
- After Outline — approve homepage structure

## Dependencies

### Auto-configured (no user action)
| Tool | Purpose |
|------|---------|
| Playwright MCP | Browser scraping |
| Semrush MCP | Keyword research |
| shadcn MCP | Base UI components |

### Requires API key (guided setup)
| Tool | Purpose | Get key at |
|------|---------|-----------|
| 21st.dev Magic MCP | Pre-built polished components | https://21st.dev/mcp |

### Optional (user installs if wanted)
| Tool | Purpose | Install |
|------|---------|---------|
| claude-seo | SEO audit + validation | `curl -fsSL https://raw.githubusercontent.com/AgriciDaniel/claude-seo/main/install.sh \| bash` |
| ui-ux-pro-max | Industry design system | `npm install -g uipro-cli && uipro init --ai claude --global` |

## License

MIT
