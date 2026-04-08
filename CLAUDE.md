# LocalStudios Plugin

Claude Code plugin for automated website generation with SEO optimization.
Generates **Next.js + shadcn/ui** websites.

## Structure
- `skills/localstudios/SKILL.md` — Main orchestrator, routes sub-commands
- `skills/localstudios/phases/` — One file per workflow phase (01-12)
- `skills/localstudios/references/` — Rules, templates, standards (lazy-loaded)
- `agents/` — Subagents for heavy tasks (content writing, schema generation)

## Development
- Keep SKILL.md under 100 lines — offload detail to phases/ and references/
- All external dependencies must have graceful fallbacks
- Test each phase independently before integration

---

## CRITICAL: Styling Rules

### globals.css is the single source of truth

**Before ANY style change:**
1. **READ `app/globals.css` first** — always
2. Check if a relevant style, token, or class already exists
3. Extend or modify existing styles in globals.css
4. **NEVER** create new CSS files
5. **NEVER** use inline `style={}` props
6. **NEVER** use CSS-in-JS or styled-components
7. **NEVER** use `.module.css` files

Tailwind utility classes in JSX are OK — they compile from globals.css config.
Custom styles go in globals.css as utility classes or component-level styles.

### shadcn/ui Component Overrides
Override shadcn defaults in globals.css, not in the component files under `components/ui/`.

### NAP Data
All business data (name, address, phone) lives in `lib/site-config.ts`.
Components read from there — never hardcode NAP in multiple places.
