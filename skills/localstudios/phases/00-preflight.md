# Phase 0 — Preflight Check

**This phase runs BEFORE everything else. No exceptions.**
**The user should not have to do anything manually — Claude handles all setup.**

---

## SETUP IS NOT OPTIONAL

**Run EVERY install command below. Do NOT skip because "something similar is already available".**

---

## Step 1 — Install ALL required tools

### 1a. Playwright MCP (Pflicht für Phase 10 Visual Validation)
```bash
claude mcp add playwright npx @playwright/mcp@latest
```

### 1b. Semrush MCP (für Keyword-Research in Phase 3)
```bash
claude mcp add semrush https://mcp.semrush.com/v1/mcp -t http
```
Requires one-time OAuth: `/mcp` → Semrush → Authenticate

### 1c. getdesign CLI (wird in Phase 8 aufgerufen)
Kein globaler Install nötig — wird via `npx getdesign@latest add <brand>` on-demand ausgeführt.
Nur sicherstellen, dass `npx` (Node.js) verfügbar ist:
```bash
node --version && npx --version
```

### 1d. Create CLAUDE.md

```markdown
# [Project Name]

## Project Documentation — READ FIRST
- **`docs/BUSINESS.md`** — all business facts + Design Source (getdesign command ODER design.md Pfad)
- **`docs/SEO-STRATEGY.md`** — keyword targets
- **`docs/pages/home.md`** — homepage: keywords, SEO, GEO
- **`design.md`** — Single Source of Truth für Farben, Fonts, Buttons, Spacing, Atmosphäre (aus Phase 8)

## Build-Regeln
- **ALLE Styles in `app/globals.css`** — einzige Source of Truth, abgeleitet aus `design.md`
- **Custom Components** in `components/sections/variant-N/*.tsx` — keine Block-Libraries, keine shadcn-Blocks
- **Keine hardcoded Farben** in Components (keine hex/rgb, kein `bg-blue-600`) — immer CSS Vars aus globals.css
- **Kein `style={}`** inline
- **Jede Section braucht ein Bild** (scraped oder `<ImagePlaceholder>`) — mindestens 5 Bilder pro Variante
- **Buttons:** via `<Button variant="primary|secondary|outline|outline-on-dark" />` — auf dunklen Sections NIE generische Tailwind-Outline

## NAP Data
`lib/site-config.ts` — single source of truth.
```

---

## Step 2 — Check if restart needed

**If ANY MCP was just installed:**
```
Setup complete. Restart needed to load all MCP tools.
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
  Node.js / npx ...... ✅ (getdesign läuft on-demand via npx)

OPTIONAL
  claude-seo ......... ✅ / ❌   (für Phase 10 SEO Audit)

=== READY ===
```
