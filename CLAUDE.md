# LocalStudios Plugin

Claude Code plugin for automated website generation with SEO optimization.

## Structure
- `skills/localstudios/SKILL.md` — Main orchestrator, routes sub-commands
- `skills/localstudios/references/` — Workflow details, templates, rules (lazy-loaded)
- `agents/` — Subagents for heavy tasks (content writing, schema generation)

## Development
- Keep SKILL.md under 400 lines — offload detail to references/
- All external dependencies must have graceful fallbacks
- Test each phase independently before integration
