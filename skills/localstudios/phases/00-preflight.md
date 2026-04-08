# Phase 0 — Preflight Check

**This phase runs BEFORE everything else. No exceptions.**

---

## Step 1 — Project Directory Setup

The user typically starts in an **empty directory**. Set it up:

1. **Check if `.mcp.json` exists** in the current working directory
2. **If not → create it** with the shadcn MCP config:

```json
{
  "mcpServers": {
    "shadcn": {
      "command": "npx",
      "args": ["-y", "@anthropic-ai/claude-code-mcp@latest", "https://shadcn-mcp.vercel.app/sse"]
    }
  }
}
```

3. **Check if `CLAUDE.md` exists** in the current working directory
4. **If not → create it** with the core styling rules:

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

5. **Inform the user**:
```
Project directory initialized:
  .mcp.json .... created (shadcn MCP)
  CLAUDE.md .... created (styling rules)
```

**Note:** The shadcn MCP will only be available after Claude Code picks up the new `.mcp.json`. If shadcn MCP tools are not detected after creation, inform the user:
"shadcn MCP was just configured. It will be available on next restart or in the next message. Continuing — components will be installed via CLI if needed."

---

## Step 2 — Check All Dependencies

Test each tool by checking if its functions are available:

| Dependency | How to Check | Required? |
|-----------|-------------|-----------|
| WebFetch | Try calling WebFetch | Required for scraping |
| Semrush MCP | Check for `mcp__semrush__*` tools | Optional |
| claude-seo | Check if `/seo` skill is loaded | Optional |
| ui-ux-pro-max | Check if `/ui-ux-pro-max` skill is loaded | Optional |
| shadcn MCP | Check for `mcp__shadcn__*` tools | Optional |
| Stitch MCP | Check for `mcp__stitch__*` tools | Optional |

---

## Step 3 — Show Status Dashboard

```
=== LOCALSTUDIOS PREFLIGHT CHECK ===

PROJECT
  Directory .......... [current working directory]
  .mcp.json .......... ✅ Created / ✅ Already exists
  CLAUDE.md .......... ✅ Created / ✅ Already exists

CORE
  WebFetch ........... ✅ Ready / ❌ Not available
                       → Without: all info must come from interview

KEYWORD RESEARCH
  Semrush MCP ........ ✅ Ready / ❌ Not configured
                       → Without: manual keywords only (from interview)

SEO ANALYSIS
  claude-seo ......... ✅ Ready / ❌ Not installed
                       → Without: no audit, build from best practices

DESIGN
  ui-ux-pro-max ...... ✅ Ready / ❌ Not installed
                       → Without: generic design system

BUILD
  shadcn MCP ......... ✅ Ready / ⏳ Just configured (available next restart)
                       → Without: components via CLI
  Stitch MCP ......... ✅ Ready / ❌ Not configured
                       → Without: Next.js project generated locally

=== [x]/6 TOOLS READY ===
```

---

## Step 4 — Handle Issues

**If OPTIONAL tools are missing:**
- Show the fallback for each
- Ask: "Continue with available tools, or set up missing ones first?"
- If user wants to set up, provide instructions:
  - **Semrush**: "Add Semrush MCP to your global Claude Code settings (~/.claude/settings.json)"
  - **claude-seo**: `curl -fsSL https://raw.githubusercontent.com/AgriciDaniel/claude-seo/main/install.sh | bash`
  - **ui-ux-pro-max**: `npm install -g uipro-cli && uipro init --ai claude --global`
  - **shadcn MCP**: Already configured in .mcp.json — restart Claude Code
  - **Stitch**: "Add Google Stitch MCP to your global Claude Code settings"

**If user wants to skip:**
- Record which tools are available
- All subsequent phases will check this and use fallbacks automatically

---

## Step 5 — Confirm and Proceed

```
Ready to generate website for: [URL]

Tools active: [list of ✅ tools]
Skipping: [list of ❌ tools with fallbacks]

Starting Phase 1 (Scrape)...
```

Proceed to Phase 1 after user confirms or acknowledges.
