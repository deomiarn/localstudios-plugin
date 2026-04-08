# Phase 0 — Preflight Check

**This phase runs BEFORE everything else. No exceptions.**
**The user should not have to do anything manually — Claude handles all setup.**
**The plugin MUST work with zero pre-installed tools.**

---

## Tool Architecture

### Always Available (built into Claude Code)

| Tool | What it does |
|------|-------------|
| **WebFetch** | Scrape websites, fetch URLs |
| **Read/Write/Edit** | Create project files |
| **Bash** | Run CLI commands (npm, npx, git) |
| **Agent** | Spawn subagents for parallel work |

### Auto-Configured by Plugin (no user action needed)

| Tool | How Plugin Sets It Up |
|------|----------------------|
| **Playwright MCP** | `claude mcp add playwright npx @playwright/mcp@latest` |
| **Semrush MCP** | `claude mcp add semrush https://mcp.semrush.com/v1/mcp -t http` |
| **shadcn MCP** | Write `.mcp.json` with `{"mcpServers":{"shadcn":{"command":"npx","args":["shadcn@latest","mcp"]}}}` |
| **frontend-design skill** | `npx claude-code-templates@latest --skill creative-design/frontend-design` |

### Requires User API Key (guided setup in Phase 9)

| Tool | Setup |
|------|-------|
| **shadcnblocks** | User provides API key from https://shadcnblocks.com → stored as ENV variable, added to components.json registry |

### Optional Enhancements (user must install separately)

| Tool | What it adds | Install | Fallback |
|------|-------------|---------|----------|
| claude-seo | SEO audit + validation | `curl -fsSL https://raw.githubusercontent.com/AgriciDaniel/claude-seo/main/install.sh \| bash` | Best practices |
| banana-claude | AI image generation | Plugin install | Scraped images + placeholders |

---

## Step 1 — Project Directory Setup

### 1a. Auto-configure MCP servers

**Playwright**: Check if `mcp__playwright__*` tools are available.
- If NOT → run: `claude mcp add playwright npx @playwright/mcp@latest`

**Semrush**: Check if `mcp__semrush__*` tools are available.
- If NOT → run: `claude mcp add semrush https://mcp.semrush.com/v1/mcp -t http`
- Requires one-time OAuth via `/mcp` → Semrush → Authenticate

**frontend-design skill**: Check if `/frontend-design` skill is loaded.
- If NOT → run: `npx claude-code-templates@latest --skill creative-design/frontend-design`
- This installs the creative design skill for high-quality frontend output

**shadcn**: Check if `mcp__shadcn__*` tools are available.
- If NOT → write `.mcp.json` in the project directory:
```json
{
  "mcpServers": {
    "shadcn": {
      "command": "npx",
      "args": ["shadcn@latest", "mcp"]
    }
  }
}
```
- If `.mcp.json` exists but shadcn is failing → overwrite with this exact config.

### 1b. Create CLAUDE.md

Check if `CLAUDE.md` exists. If not, create it:

```markdown
# [Project Name]

## Project Documentation — READ FIRST
- **`docs/BUSINESS.md`** — all business facts (NAP, hours, services, USPs)
- **`docs/SEO-STRATEGY.md`** — keyword targets, geo strategy, linking map
- **`docs/pages/[page].md`** — per-page: purpose, keywords, SEO, GEO signals
- **Update docs/** when making changes — keep in sync with code

## Styling Rules — CRITICAL
- **ALL styles in `app/globals.css`** — read it before any style change
- Never use inline `style={}` props
- Never create separate CSS files
- Override shadcn defaults in globals.css, not in component files
- Tailwind utility classes in JSX are OK

## NAP Data
All business data lives in `lib/site-config.ts` AND `docs/BUSINESS.md`.
Components read from site-config.ts — never hardcode in multiple places.
```

### 1c. Check if restart needed

If any MCP was just added and is still not connecting:
```
MCP servers were just configured. They load at startup.
→ Type /exit, then run 'claude' again.
→ Then re-run: /localstudios generate <url>
```
**Stop here.** User restarts once, re-runs, everything works.

---

## Step 2 — Show Status Dashboard

```
=== LOCALSTUDIOS PREFLIGHT CHECK ===

PROJECT
  Directory .......... [current working directory]
  CLAUDE.md .......... ✅ Ready

AUTO-CONFIGURED
  Playwright MCP ..... ✅ Ready / 🔧 Just installed
  Semrush MCP ........ ✅ Ready / 🔧 Just installed
  shadcn MCP ......... ✅ Ready / 🔧 Just installed
  frontend-design .... ✅ Ready / 🔧 Just installed

OPTIONAL
  shadcnblocks ....... API key needed in Phase 9 (from shadcnblocks.com)
  claude-seo ......... ✅ / ❌ → fallback: best practices
  banana-claude ...... ✅ / ❌ → fallback: scraped images + placeholders

=== READY ===
```

---

## Step 3 — Handle Results

**If any MCP was just installed (🔧):**
→ Tell user to restart and re-run. Stop here.

**If everything is ✅:**
→ Proceed directly to Phase 1. No confirmation needed — just go.
