---
name: localstudios
description: >
  Website generation toolkit by LocalStudios. Use /localstudios generate <url>
  to create a single-page homepage as a client pitch. SEO-optimized, ready to show.
user-invokable: true
argument-hint: "[generate] [url]"
license: MIT
metadata:
  author: LocalStudios
  version: "1.0.0"
  category: website-generation
---

# LocalStudios Plugin

## Commands

| Command | Description |
|---------|-------------|
| `generate <url>` | Generate a single-page homepage as client pitch |

## Routing

Parse the first argument:

- **`generate`** → Execute generate workflow (second arg = URL)
- **No argument** → Show commands table
- **Unknown** → Show "Unknown command." + commands table

---

## Generate Workflow

Creates a **single homepage** to pitch the client. Not a full website — just one strong page that shows what's possible.

Load each phase file **only when that phase begins**. Run phases in parallel where possible.

| Phase | File | Pause? | Group |
|-------|------|--------|-------|
| 0. Preflight | `./phases/00-preflight.md` | WAIT | — |
| 1. Scrape | `./phases/01-scrape.md` | | — |
| 2. Interview | `./phases/02-interview.md` | WAIT | — |
| 3. Keywords | `./phases/03-keywords.md` | | ⬇ parallel |
| 4. Geo-Strategy | `./phases/04-geo-strategy.md` | | ⬇ after 3 |
| 5. SEO Audit | `./phases/05-seo-audit.md` | | ⬇ parallel with 3 |
| 6. Outline | `./phases/06-outline.md` | WAIT | — |
| 7. Content | `./phases/07-content.md` | | ⬇ parallel |
| 8. Schema | `./phases/08-schema.md` | | ⬇ parallel with 7 |
| 9. Design | `./phases/09-design.md` | | ⬇ parallel with 7+8 |
| 10. Build | `./phases/10-build.md` | | — (needs 7+8+9) |
| 11. QA | `./phases/11-quality.md` | | — |
| 12. Report | `./phases/12-report.md` | | — |

### Parallelization Rules

**Always run phases in parallel when they don't depend on each other:**

- **Phases 3+5 parallel**: Keyword research and SEO audit can run at the same time (both only need interview data)
- **Phase 4 after 3**: Geo-strategy needs keyword results
- **Phases 7+8+9 parallel**: Content writing, schema generation, and design system are independent. Run all three at the same time.
- **Phase 10 waits for 7+8+9**: Build needs content, schema, and design to be done.

**Use subagents for parallel work.** Spawn multiple agents in a single message.

### MANDATORY Skill Usage

**If a skill or MCP is available, you MUST use it. Never skip an available tool.**

| Tool | When Available | Action | NEVER do instead |
|------|---------------|--------|------------------|
| `/ui-ux-pro-max` | Phase 9 | MUST call `/ui-ux-pro-max plan` + `/ui-ux-pro-max build` | Do NOT invent your own design |
| `/seo page` | Phase 11 | MUST call `/seo page` on the built homepage | Do NOT skip SEO validation |
| `/seo schema` | Phase 11 | MUST call `/seo schema` to validate markup | Do NOT assume schema is correct |
| Stitch MCP | Phase 10 | MUST use Stitch to generate the page | Do NOT build manually if Stitch is there |
| Semrush MCP | Phase 3 | MUST use for keyword research | Do NOT guess keywords |
| shadcn MCP | Phase 10 | MUST use to discover/install components | Do NOT manually pick components |

**Fallbacks are ONLY for when the tool is genuinely unavailable (not detected in preflight).**
If preflight shows ✅ for a tool, that tool MUST be used in its phase. No exceptions.

### Other Rules
- WAIT = do not proceed until user responds
- Load reference files on demand (see each phase)
- This generates ONE page only — the homepage
- No screenshots needed — user can check localhost themselves
- No `npm run dev` or dev server — just build, user starts it themselves

### Dependencies

| Tool | Check | Fallback (ONLY if not available) |
|------|-------|----------|
| Playwright MCP | `mcp__playwright__*` | WebFetch |
| Semrush MCP | `mcp__semrush__*` | Manual keywords from interview |
| shadcn MCP | `mcp__shadcn__*` | Install components via CLI |
| claude-seo | `/seo` skill loaded | Skip audit |
| ui-ux-pro-max | `/ui-ux-pro-max` loaded | Generic design system |
| Stitch MCP | `mcp__stitch__*` | Generate Next.js project directly |

### Tech Stack
- **Next.js** App Router (initialized via `npx shadcn@latest init --preset b0 --template next`)
- **shadcn/ui** for all UI components
- **globals.css** — single source of truth for ALL styles (never inline)
- See `./references/code-standards.md` for full standards
