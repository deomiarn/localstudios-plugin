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
| **Playwright MCP** | `claude mcp add playwright -- npx @anthropic-ai/mcp-server-playwright` |
| **Semrush MCP** | `claude mcp add semrush https://mcp.semrush.com/v1/mcp -t http` |
| **shadcn MCP** | Write `.mcp.json` with `{"mcpServers":{"shadcn":{"command":"npx","args":["shadcn@latest","mcp"]}}}` |

### Requires User API Key (guided setup)

| Tool | Setup |
|------|-------|
| **21st.dev Magic MCP** | User provides API key from https://21st.dev/mcp → Claude runs install command |

### Optional Enhancements (user must install separately)

| Tool | What it adds | Install | Fallback |
|------|-------------|---------|----------|
| claude-seo | SEO audit + validation | `curl -fsSL https://raw.githubusercontent.com/AgriciDaniel/claude-seo/main/install.sh \| bash` | Best practices |
| ui-ux-pro-max | Industry-specific design | `npm install -g uipro-cli && uipro init --ai claude --global` | Generic design |
| banana-claude | AI image generation | Plugin install | Scraped images + placeholders |

---

## Step 1 — Project Directory Setup

### 1a. Auto-configure MCP servers

**Playwright**: Check if `mcp__playwright__*` tools are available.
- If NOT → run: `claude mcp add playwright -- npx @anthropic-ai/mcp-server-playwright`

**Semrush**: Check if `mcp__semrush__*` tools are available.
- If NOT → run: `claude mcp add semrush https://mcp.semrush.com/v1/mcp -t http`
- Requires one-time OAuth via `/mcp` → Semrush → Authenticate

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

### 1b. 21st.dev Magic MCP Setup (guided)

Check if `mcp__magic__*` tools are available.

If NOT → guide the user through setup:

```
21st.dev Magic MCP is not configured yet. It provides beautiful pre-built components.

To set it up:
1. Go to https://21st.dev/mcp and get your API key
2. Paste your API key here:
```

Wait for the user to provide their API key.

When user provides the key, run:
```bash
claude mcp add magic --scope user --env API_KEY="[USER_PROVIDED_KEY]" -- npx -y @21st-dev/magic@latest
```

**NEVER hardcode or log the API key. Use the user's input directly in the command.**

If user says "skip" → proceed without 21st.dev, components will be built manually with shadcn.

### 1c. Create CLAUDE.md

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

### 1d. Check if restart needed

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

USER KEY REQUIRED
  21st.dev Magic ..... ✅ Ready / 🔧 Just configured / ❌ Skipped

OPTIONAL
  claude-seo ......... ✅ / ❌ → fallback: best practices
  ui-ux-pro-max ...... ✅ / ❌ → fallback: generic design
  banana-claude ...... ✅ / ❌ → fallback: scraped images + placeholders

=== READY ===
```

---

## Step 3 — Handle Results

**If any MCP was just installed (🔧):**
→ Tell user to restart and re-run. Stop here.

**If everything is ✅:**
→ Proceed directly to Phase 1. No confirmation needed — just go.
