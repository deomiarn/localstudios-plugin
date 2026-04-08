# Phase 0 — Preflight Check

**This phase runs BEFORE everything else. No exceptions.**
**The user should not have to do anything manually — Claude handles all setup.**

---

## Step 1 — Project Directory Setup

The user typically starts in an **empty directory**. Set it up automatically:

### 1a. Set up MCP servers

**Check if Semrush MCP is available** (`mcp__semrush__*` tools):
- If NOT → run: `claude mcp add semrush https://mcp.semrush.com/v1/mcp -t http`

**Check if shadcn MCP is available** (`mcp__shadcn__*` tools):
- If NOT → run: `npx shadcn@latest mcp init --client claude` in the project directory
- This creates/updates the `.mcp.json` with the correct shadcn config

### 1b. Create CLAUDE.md

**Check if `CLAUDE.md` exists** in the current working directory.
**If not → create it:**

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

**If any MCP was just added and is still not available:**
```
⚠ MCP servers were just configured. They load at startup.
→ Restarting Claude Code is needed. Type /exit, then run 'claude' again.
→ Then re-run: /localstudios generate <url>
```
**Stop the workflow here.** The user only needs to restart and re-run the command — everything else is already set up.

---

## Step 2 — Check All Dependencies

Test each tool by checking if its functions are available:

| Dependency | How to Check | Auto-Setup? |
|-----------|-------------|-------------|
| WebFetch | Try calling WebFetch | Built-in, always available |
| Semrush MCP | Check for `mcp__semrush__*` tools | Yes → `claude mcp add semrush ...` |
| claude-seo | Check if `/seo` skill is loaded | No → plugin, user must install |
| ui-ux-pro-max | Check if `/ui-ux-pro-max` skill is loaded | No → plugin, user must install |
| shadcn MCP | Check for `mcp__shadcn__*` tools | Yes → `npx shadcn@latest mcp init --client claude` |
| Stitch MCP | Check for `mcp__stitch__*` tools | No → user must configure |

---

## Step 3 — Show Status Dashboard

```
=== LOCALSTUDIOS PREFLIGHT CHECK ===

PROJECT
  Directory .......... [current working directory]
  CLAUDE.md .......... ✅ Ready

MCP SERVERS
  Semrush ............ ✅ Ready / 🔧 Just installed (restart needed)
  shadcn ............. ✅ Ready / 🔧 Just installed (restart needed)

PLUGINS
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
- These cannot be auto-installed — they are Claude Code plugins
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
