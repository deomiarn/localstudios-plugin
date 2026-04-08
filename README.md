# LocalStudios Plugin for Claude Code

Generates a single-page homepage as a client pitch. Scrapes the existing site, researches keywords, creates SEO-optimized content, and builds a polished Next.js homepage with shadcnblocks + frontend-design.

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

1. **Preflight** — auto-configures MCPs, installs frontend-design skill
2. **Scrapes** the existing website (max 7 requests, fast)
3. **Interviews** you to fill gaps and confirm details
4. **Researches keywords** via Semrush MCP (max 5 API calls)
5. **Plans** the homepage structure (8 mandatory sections)
6. **Writes** extensive SEO-optimized content (1500-2500 words)
7. **Generates** schema markup (LocalBusiness, WebSite)
8. **Defines** design direction (colors, typography, atmosphere)
9. **Builds** homepage: shadcnblocks as foundation → /frontend-design refinement
10. **Checks** quality with /seo validation
11. **Reports** with next steps

### Design Philosophy
- **shadcnblocks** = starting point blocks (generic structure)
- **frontend-design** = makes it distinctive and production-grade
- **ui-rules** = enforces clean CSS via shadcn variables, no hardcoded colors

## Dependencies

### Auto-configured (no user action)
| Tool | Purpose |
|------|---------|
| Playwright MCP | Browser scraping |
| Semrush MCP | Keyword research |
| shadcn MCP | Base UI components |
| frontend-design | Distinctive frontend code |

### Requires API key (asked during build)
| Tool | Purpose | Get key at |
|------|---------|-----------|
| shadcnblocks | Pre-built section blocks | https://shadcnblocks.com |

### Optional
| Tool | Purpose | Install |
|------|---------|---------|
| claude-seo | SEO validation | `curl -fsSL https://raw.githubusercontent.com/AgriciDaniel/claude-seo/main/install.sh \| bash` |

## License

MIT
