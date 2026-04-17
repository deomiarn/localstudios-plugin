# Extend Phase 0 — Preflight

Same as generate preflight PLUS Sanity setup.

## Step 1 — Run generate preflight
Load `../00-preflight.md` and execute all install commands.

## Step 2 — Sanity MCP
Check if `mcp__Sanity__*` tools are available.

If NOT → guide user:
```
Sanity CMS is needed for dynamic pages (blog, services, team, regions).
→ Go to https://www.sanity.io and create a project
→ The Sanity MCP should be available via Claude.ai integration
→ If not: configure it in your MCP settings
```

## Step 3 — Verify existing project
Check that the current directory has:
- [ ] `docs/BUSINESS.md` — from generate
- [ ] `design.md` — from generate (Single Source of Truth)
- [ ] `app/page.tsx` — homepage exists
- [ ] `app/globals.css` — design tokens set
- [ ] `lib/site-config.ts` — NAP data

If any missing → warn user: "Run /localstudios generate first."

## Step 4 — Update CLAUDE.md
Add Sanity rules:
```markdown
## Sanity CMS — Dynamic Content
- Global data (NAP, hours, social) → Sanity globals document
- Services, Team, Regions, Blog → Sanity collections
- NEVER hardcode dynamic content — always fetch from Sanity
- Use lib/sanity.ts for all queries
- Revalidate: ISR with on-demand revalidation
```
