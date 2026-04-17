# LocalStudios Plugin for Claude Code

Generates a single-page homepage as a client pitch. Scrapes the existing site, researches keywords, creates SEO-optimized content, and builds a polished Next.js homepage with **custom components** styled from a **`design.md`** (via `getdesign` CLI) and validated visually via Playwright MCP.

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

1. **Preflight** — auto-configures MCPs (Playwright, Semrush)
2. **Scrapes** the existing website (max 7 requests, fast)
3. **Interviews** you for business data + **Design Source** (Pflicht: `getdesign` Command ODER Pfad zu einer `design.md`)
4. **Researches keywords** via Semrush MCP
5. **Plans** the homepage structure (10 fixed sections per `references/page-sections.md`)
6. **Writes** extensive SEO-optimized content (1500-2500 words)
7. **Generates** schema markup (LocalBusiness, industry-specific `@type`)
8. **Builds `design.md`** (via `npx getdesign@latest add <brand>` oder aus vorhandener Datei) + `variant-blueprints.md`
9. **Builds** N homepage variants as **custom components** — `globals.css` wird 1:1 aus `design.md` abgeleitet
10. **Validates** visually via Playwright MCP (Screenshots pro Variante, Abgleich vs. `design.md`, iteratives Fixen) + `/seo` Audit
11. **Reports** with next steps

### Design Philosophy
- **`design.md`** ist Single Source of Truth (Farben, Fonts, Buttons, Spacing, Atmosphäre, Anti-Muster)
- **`globals.css`** ist 1:1 Ableitung aus `design.md` — alle Styles zentralisiert
- **Custom Components** — pro Section ein eigenes File, semantisches HTML, clean code, keine fremden Block-Libraries
- **Playwright MCP** validiert am Ende visuell, dass die Page tatsächlich `design.md` entspricht

## Dependencies

### Auto-configured (no user action)
| Tool | Purpose |
|------|---------|
| Playwright MCP | Browser scraping + visuelle QA in Phase 10 |
| Semrush MCP | Keyword research |

### Per invocation
| Tool | Purpose |
|------|---------|
| `npx getdesign@latest add <brand>` | Design-Quelle (wird in Phase 8 aus dem vom User gelieferten Command ausgeführt) |

### Optional
| Tool | Purpose | Install |
|------|---------|---------|
| claude-seo | SEO validation | `curl -fsSL https://raw.githubusercontent.com/AgriciDaniel/claude-seo/main/install.sh \| bash` |

## License

MIT
