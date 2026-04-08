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
  version: "1.1.0"
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

Creates a **single homepage** to pitch the client. Run phases in parallel where possible.

| Phase | File | Pause? | Group |
|-------|------|--------|-------|
| 0. Preflight | `./phases/00-preflight.md` | WAIT | — |
| 1. Scrape | `./phases/01-scrape.md` | | — |
| 2. Interview | `./phases/02-interview.md` | WAIT | — |
| 3. Keywords | `./phases/03-keywords.md` | | — |
| 4. Geo-Strategy | `./phases/04-geo-strategy.md` | | after 3 |
| 5. Outline | `./phases/05-outline.md` | WAIT | — |
| 6. Content | `./phases/06-content.md` | | ⬇ parallel |
| 7. Schema | `./phases/07-schema.md` | | ⬇ parallel with 6 |
| 8. Design | `./phases/08-design.md` | | ⬇ parallel with 6+7 |
| 9. Build | `./phases/09-build.md` | | — (needs 6+7+8) |
| 10. QA | `./phases/10-quality.md` | | — |
| 11. Report | `./phases/11-report.md` | | — |

### Parallelization Rules

- **Phases 6+7+8 parallel**: Content, schema, and design are independent — run all three simultaneously
- **Phase 9 waits for 6+7+8**: Build needs content, schema, and design

**Use subagents for parallel work.** Spawn multiple agents in a single message.

### MANDATORY Skill & Tool Usage

**If a skill or MCP is available, you MUST use it. Never skip an available tool.**

| Tool | When Available | Action | NEVER do instead |
|------|---------------|--------|------------------|
| `/ui-ux-pro-max` | Phase 8 | MUST call `/ui-ux-pro-max plan` + `/ui-ux-pro-max build` | Do NOT invent your own design |
| `/frontend-design` | Phase 9 | MUST use for high-quality, distinctive frontend code | Do NOT write generic-looking components |
| 21st.dev Inspiration | Phase 9 | MUST use `mcp__magic__21st_magic_component_inspiration` (FREE) to find proven layouts before building | Do NOT skip inspiration and build from scratch |
| `/seo page` | Phase 10 | MUST call `/seo page` to validate the NEW homepage | Do NOT skip SEO validation |
| `/seo schema` | Phase 10 | MUST call `/seo schema` to validate markup | Do NOT assume schema is correct |
| Semrush MCP | Phase 3 | MUST use for keyword research | Do NOT guess keywords |
| shadcn MCP | Phase 9 | MUST use to install base UI components | Do NOT manually pick components |

**Fallbacks are ONLY for when the tool is genuinely unavailable (not detected in preflight).**

### Other Rules
- WAIT = do not proceed until user responds
- Load reference files on demand
- ONE page only — the homepage
- No screenshots — user checks localhost themselves
- No `npm run dev` — just build, user starts it

### Dependencies

| Tool | Check | Fallback (ONLY if not available) |
|------|-------|----------|
| Playwright MCP | `mcp__playwright__*` | WebFetch |
| Semrush MCP | `mcp__semrush__*` | Manual keywords from interview |
| shadcn MCP | `mcp__shadcn__*` | Install components via CLI |
| 21st.dev Magic MCP | `mcp__magic__*` | Build components manually with shadcn |
| frontend-design | `/frontend-design` skill loaded | Auto-install via npx |
| claude-seo | `/seo` skill loaded | Skip QA validation |
| ui-ux-pro-max | `/ui-ux-pro-max` loaded | Generic design system |

### Tech Stack
- **Next.js** App Router (`npx shadcn@latest init --preset b0 --template next`)
- **shadcn/ui** for base UI components
- **21st.dev** for polished, pre-built section components
- **ui-ux-pro-max** for industry-specific design system
- **globals.css** — single source of truth for ALL styles (never inline)
