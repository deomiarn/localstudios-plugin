# Phase 0 — Preflight Check

**This phase runs BEFORE everything else. No exceptions.**
**The user should not have to do anything manually — Claude handles all setup.**

---

## Required MCP Configurations

These are the exact configs. Use them verbatim — no alternatives.

### Semrush MCP
```
claude mcp add semrush https://mcp.semrush.com/v1/mcp -t http
```
- Requires one-time OAuth authentication via `/mcp` → select Semrush → Authenticate
- Provides: keyword volume, difficulty, CPC, intent, variations

### shadcn MCP
Write this exact `.mcp.json` in the project directory:
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
- Provides: component discovery, search, installation via natural language

### Stitch MCP
- Pre-configured globally by user as `stitch` in `~/.claude.json`
- Provides: visual website generation via Google Stitch

---

## Step 1 — Project Directory Setup

The user typically starts in an **empty directory**. Set it up automatically:

### 1a. Set up MCP servers

**Semrush**: Check if `mcp__semrush__*` tools are available.
- If NOT → run: `claude mcp add semrush https://mcp.semrush.com/v1/mcp -t http`

**shadcn**: Check if `mcp__shadcn__*` tools are available.
- If NOT → write the `.mcp.json` file above into the project directory.
- If `.mcp.json` already exists but shadcn is failing → overwrite it with the exact config above.

### 1b. Create CLAUDE.md

Check if `CLAUDE.md` exists in the current working directory. If not, create it:

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

After running MCP setup commands, check if the MCPs are now available.

If any MCP was just added and is still not connecting:
```
MCP servers were just configured. They load at startup.
→ Type /exit, then run 'claude' again.
→ Then re-run: /localstudios generate <url>
```
**Stop the workflow here.** The user only needs to restart and re-run — everything else is already set up.

---

## Step 2 — Check All Dependencies

| Dependency | Check | Auto-Setup | Fallback |
|-----------|-------|------------|----------|
| WebFetch | Built-in | — | User provides info manually |
| Semrush MCP | `mcp__semrush__*` | `claude mcp add ...` | Manual keywords from interview |
| shadcn MCP | `mcp__shadcn__*` | Write `.mcp.json` | Components via `npx shadcn@latest add` |
| claude-seo | `/seo` skill loaded | Cannot auto-install | Skip audit, use best practices |
| ui-ux-pro-max | `/ui-ux-pro-max` loaded | Cannot auto-install | Generic design system |
| Stitch MCP | `mcp__stitch__*` | Cannot auto-install | Next.js project generated locally |

---

## Step 3 — Show Status Dashboard

```
=== LOCALSTUDIOS PREFLIGHT CHECK ===

PROJECT
  Directory .......... [current working directory]
  CLAUDE.md .......... ✅ Ready

MCP SERVERS (auto-configured)
  Semrush ............ ✅ Ready / 🔧 Just installed (restart needed)
  shadcn ............. ✅ Ready / 🔧 Just installed (restart needed)

PLUGINS (require manual install)
  claude-seo ......... ✅ Ready / ❌ Not installed
                       → Without: no audit of existing site
  ui-ux-pro-max ...... ✅ Ready / ❌ Not installed
                       → Without: generic design system

OTHER
  WebFetch ........... ✅ Ready
  Stitch MCP ......... ✅ Ready / ❌ Not configured
                       → Without: Next.js project generated locally

=== [x]/6 TOOLS READY ===
```

---

## Step 4 — Handle Results

**If any MCP was just installed (🔧):**
→ Tell user to restart and re-run. Stop here.

**If plugins are missing (claude-seo, ui-ux-pro-max):**
- These are Claude Code plugins that cannot be auto-installed
- Show what will be skipped and the fallback
- Offer setup instructions if user wants:
  - **claude-seo**: `curl -fsSL https://raw.githubusercontent.com/AgriciDaniel/claude-seo/main/install.sh | bash`
  - **ui-ux-pro-max**: `npm install -g uipro-cli && uipro init --ai claude --global`
- Ask: "Continue without these, or install them first?"

**If everything is ✅:**
→ Proceed directly to Phase 1

---

## Step 5 — Confirm and Proceed

```
Ready to generate website for: [URL]

Tools active: [list]
Skipping: [list with fallbacks]

Starting Phase 1 (Scrape)...
```

Proceed to Phase 1 after user confirms or acknowledges.
