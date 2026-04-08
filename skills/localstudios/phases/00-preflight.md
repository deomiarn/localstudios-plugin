# Phase 0 — Preflight Check

**This phase runs BEFORE everything else. No exceptions.**
**The user should not have to do anything manually — Claude handles all setup.**
**The plugin MUST work with zero pre-installed tools.**

---

## SETUP IS NOT OPTIONAL

**Run EVERY install command below. Do NOT skip because "something similar is already available".**
If MCP Docker browser is available — install Playwright anyway.
If some other keyword tool exists — install Semrush anyway.
These are the EXACT tools this plugin uses. No substitutes.

---

## Step 1 — Install ALL required tools

Run these commands. Check AFTER running if they are available. Do NOT check first and skip.

### 1a. Playwright MCP
```bash
claude mcp add playwright npx @playwright/mcp@latest
```
Do NOT use MCP Docker or any other browser tool as substitute.

### 1b. Semrush MCP
```bash
claude mcp add semrush https://mcp.semrush.com/v1/mcp -t http
```
Requires one-time OAuth: `/mcp` → Semrush → Authenticate

### 1c. shadcn MCP
Write `.mcp.json` in the project directory:
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
If `.mcp.json` already exists → overwrite it with this exact config.

### 1d. shadcnblocks API Key (REQUIRED)
Ask the user:
```
shadcnblocks API key is required for building the homepage.
→ Get your key at https://shadcnblocks.com
→ Paste your API key here:
```

**Do NOT proceed without the key.** Wait until the user provides it.
Store for Phase 9 (environment variable, NEVER commit to code).

### 1e. frontend-design skill
```bash
npx claude-code-templates@latest --skill creative-design/frontend-design
```

### 1f. Create CLAUDE.md

```markdown
# [Project Name]

## Project Documentation — READ FIRST
- **`docs/BUSINESS.md`** — all business facts (NAP, hours, services, USPs)
- **`docs/SEO-STRATEGY.md`** — keyword targets, geo strategy, linking map
- **`docs/pages/[page].md`** — per-page: purpose, keywords, SEO, GEO signals
- **Update docs/** when making changes — keep in sync with code

## Skills — ALWAYS USE
- /frontend-design for every component (distinctive, not generic)
- references/ui-rules.md for strict CSS rules (no hardcoded colors)

## Styling Rules — CRITICAL
- **ALL styles in `app/globals.css`** — read it before any style change
- Never use inline `style={}` props
- Never create separate CSS files
- All colors via shadcn CSS variables (--primary, --accent etc.)
- Override shadcn defaults in globals.css, not in component files

## NAP Data
All business data lives in `lib/site-config.ts` AND `docs/BUSINESS.md`.
Components read from site-config.ts — never hardcode in multiple places.
```

---

## Step 2 — Show Dashboard and Decide

Show the status AFTER running all install commands:

```
=== LOCALSTUDIOS PREFLIGHT CHECK ===

PROJECT
  Directory .......... [current working directory]
  CLAUDE.md .......... ✅ Created

TOOLS
  Playwright MCP ..... ✅ / 🔧 Installed
  Semrush MCP ........ ✅ / 🔧 Installed
  shadcn MCP ......... ✅ / 🔧 Installed
  frontend-design .... ✅ / 🔧 Installed
  shadcnblocks ....... ✅ Key stored

OPTIONAL
  claude-seo ......... ✅ / ❌

=== STATUS ===
```

## Step 3 — ALWAYS restart if anything was just installed

**If ANY tool was just installed or configured in Step 1 (even one):**

```
Setup complete. Restart needed to load all tools.
→ Type /exit, then run 'claude' again.
→ Then re-run: /localstudios generate <url>
```

**STOP. Do NOT proceed to Phase 1. Do NOT use fallbacks. Do NOT say "CLI fallback is fine".**
**The restart ensures ALL tools are properly loaded. No exceptions.**

**Only proceed to Phase 1 if this is a RE-RUN after restart and all tools show ✅.**
