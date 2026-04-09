# Phase 0 — Preflight Check

**This phase runs BEFORE everything else. No exceptions.**
**The user should not have to do anything manually — Claude handles all setup.**

---

## SETUP IS NOT OPTIONAL

**Run EVERY install command below. Do NOT skip because "something similar is already available".**

---

## Step 1 — Install ALL required tools

### 1a. Playwright MCP
```bash
claude mcp add playwright npx @playwright/mcp@latest
```

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

### 1d. frontend-design skill
```bash
npx claude-code-templates@latest --skill creative-design/frontend-design
```

### 1e. Vercel agent-skills (web-design-guidelines + react-best-practices)
```bash
npx skills add vercel-labs/agent-skills
```

### 1f. Liquid Glass skill
```bash
npx skills add haider-nawaz/liquid-glass-skill
```

### 1g. shadcnblocks API Key (REQUIRED)
Ask the user:
```
shadcnblocks API key is required for building the homepage.
→ Get your key at https://shadcnblocks.com
→ Paste your API key here:
```

**Do NOT proceed without the key.** Wait until the user provides it.

Save to `.env.local`:
```bash
echo 'SHADCNBLOCKS_API_KEY=[user key]' >> .env.local
```
Ensure `.env.local` is in `.gitignore`.
NEVER put the key directly in `components.json` or any committed file.

### 1h. Create CLAUDE.md

```markdown
# [Project Name]

## Project Documentation — READ FIRST
- **`docs/BUSINESS.md`** — all business facts
- **`docs/SEO-STRATEGY.md`** — keyword targets
- **`docs/pages/home.md`** — homepage: keywords, SEO, GEO
- **`design-system.md`** — colors, fonts, atmosphere (from Design Brief)

## Skills — ALWAYS USE
- /frontend-design — for every component (distinctive, not generic)
- shadcnblocks — Blocks as structural foundation (EDIT, don't replace)
- /liquid-glass — for glassmorphism/frosted effects where fitting

## UI Rules — CRITICAL
- ALL styles in `app/globals.css` — read before any change
- ALL colors via shadcn CSS variables (--primary, --accent etc.)
- NEVER hardcode hex/rgb in components
- NEVER inline style={} props
- NEVER leave empty spaces — use styled placeholders
- Minimum 5 images on homepage

## NAP Data
`lib/site-config.ts` — single source of truth.
```

---

## Step 2 — Check if restart needed

**If ANY tool was just installed:**
```
Setup complete. Restart needed to load all tools.
→ Type /exit, then run 'claude' again.
→ Then re-run: /localstudios generate <url>
```
**STOP. Do NOT proceed. Do NOT use fallbacks.**

**Only proceed to Phase 1 if this is a RE-RUN after restart and all tools show ✅.**

---

## Step 3 — Status Dashboard

```
=== LOCALSTUDIOS PREFLIGHT CHECK ===

PROJECT
  Directory .......... [cwd]
  CLAUDE.md .......... ✅

INSTALLED
  Playwright MCP ..... ✅ / 🔧 Restart needed
  Semrush MCP ........ ✅ / 🔧 Restart needed
  shadcn MCP ......... ✅ / 🔧 Restart needed
  frontend-design .... ✅ / 🔧 Installed
  vercel agent-skills  ✅ / 🔧 Installed
  liquid-glass ....... ✅ / 🔧 Installed

BLOCKS
  shadcnblocks ....... ✅ Key in .env.local

OPTIONAL
  claude-seo ......... ✅ / ❌

=== READY ===
```
