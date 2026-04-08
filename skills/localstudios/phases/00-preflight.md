# Phase 0 — Preflight Check

**This phase runs BEFORE everything else. No exceptions.**

## Process

Check every MCP and dependency. Show the user a clear status dashboard.

### Step 1 — Check All Dependencies

Test each tool by checking if its functions are available:

| Dependency | How to Check | Required? |
|-----------|-------------|-----------|
| WebFetch | Try calling WebFetch | Required for scraping |
| Semrush MCP | Check for `mcp__semrush__*` tools | Optional |
| claude-seo | Check if `/seo` skill is loaded | Optional |
| ui-ux-pro-max | Check if `/ui-ux-pro-max` skill is loaded | Optional |
| shadcn MCP | Check for `mcp__shadcn__*` tools | Optional |
| Stitch MCP | Check for `mcp__stitch__*` tools | Optional |

### Step 2 — Show Status Dashboard

```
=== LOCALSTUDIOS PREFLIGHT CHECK ===

CORE
  WebFetch ........... ✅ Ready / ❌ Not available

KEYWORD RESEARCH
  Semrush MCP ........ ✅ Ready / ❌ Not configured
                       → Without: manual keywords only (from interview)

SEO ANALYSIS
  claude-seo ......... ✅ Ready / ❌ Not installed
                       → Without: no audit of existing site, build from best practices

DESIGN
  ui-ux-pro-max ...... ✅ Ready / ❌ Not installed
                       → Without: generic design system

BUILD
  shadcn MCP ......... ✅ Ready / ❌ Not configured
                       → Without: components installed via CLI instead
  Stitch MCP ......... ✅ Ready / ❌ Not configured
                       → Without: Next.js project generated locally

=== [x]/6 TOOLS READY ===
```

### Step 3 — Handle Issues

**If a REQUIRED tool is missing (WebFetch):**
- Inform user: "WebFetch is required for scraping. Without it, you'll need to provide all business information manually in the interview."
- Ask: "Continue without scraping? [Yes / No]"

**If OPTIONAL tools are missing:**
- Show what will be skipped and what the fallback is
- Ask: "Continue with available tools? Or set up missing tools first?"
- If user wants to set up:
  - **Semrush**: "Configure Semrush MCP in your Claude Code settings. See: [semrush docs]"
  - **claude-seo**: "Install with: `curl -fsSL https://raw.githubusercontent.com/AgriciDaniel/claude-seo/main/install.sh | bash`"
  - **ui-ux-pro-max**: "Install with: `npm install -g uipro-cli && uipro init --ai claude --global`"
  - **shadcn MCP**: "Add to .mcp.json or Claude Code MCP settings"
  - **Stitch**: "Configure Google Stitch MCP in your Claude Code settings"

**If user wants to skip:**
- Record which tools are available in a status object
- Pass this to all subsequent phases so they know what to skip

### Step 4 — Confirm and Proceed

```
Ready to generate website for: [URL]
Tools active: [list of ✅ tools]
Skipping: [list of ❌ tools with fallbacks]

Proceeding to Phase 1 (Scrape)...
```

Only proceed to Phase 1 after user confirms or acknowledges the dashboard.
