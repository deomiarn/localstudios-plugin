# Clone Phase 0 — Preflight

Same as generate preflight. Run ALL install commands.
Load `../00-preflight.md` and execute it.

Additionally verify:
- Source project path exists and contains a Next.js project
- Source has `design.md` (READ-ONLY), `docs/BUSINESS.md`, `app/globals.css` (und idealerweise `layout-plan.md`)
- If source is missing key files → warn user, ask if they want to proceed
