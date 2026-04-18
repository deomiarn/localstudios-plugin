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
- **`docs/pages/home.md`** — homepage SEO + Image-Source-Tracking pro Section
- **`design.md`** — **READ-ONLY** Single Source of Truth für Farben, Fonts, Buttons, Spacing, Atmosphäre (aus Phase 8)
- **`layout-plan.md`** — Layout-Plan für die Homepage (Phase 8)

## Build-Regeln
- **`design.md` ist READ-ONLY** — NIE editieren. Nur lesen und in `globals.css` / Tailwind-Utilities umsetzen. Einzige Ausnahme: wenn der User am Anfang einen Farbwechsel angefragt hat, ersetze NUR die betroffenen Farb-Tokens. Alle anderen Inhalte (Gefühl, Typografie-Regeln, Button-Stil, Atmosphäre, Anti-Muster) bleiben 1:1.
- **`globals.css`** bleibt schlank — nur CSS-Vars aus `design.md` + Tailwind v4 `@theme inline` Mapping + minimale Base-Styles
- **Custom Components** flach in `components/sections/*.tsx` — keine Block-Libraries, keine shadcn-Blocks, keine Varianten
- **Keine hardcoded Farben** in Components (keine hex/rgb, kein `bg-blue-600`) — immer CSS Vars via Tailwind-Utilities
- **Kein `style={}`** inline
- **Hero MUSS ein Bild above-the-fold haben** — Split oder Full-Bleed Background; Proportion `aspect-[4/5]` oder `aspect-[3/4]`, nie `aspect-video`
- **Keine `[Image …]`-Text-Platzhalter im JSX** — Image-Metadaten gehören in `docs/pages/home.md`; im JSX nur `next/image` oder `<ImagePlaceholder>`
- **Jede Section braucht ein Bild** — min. 5 Bilder auf der Homepage
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
