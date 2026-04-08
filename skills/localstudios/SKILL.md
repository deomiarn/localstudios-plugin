---
name: localstudios
description: >
  Website generation and SEO toolkit by LocalStudios. Use /localstudios generate <url>
  to scrape an existing website, conduct keyword research, create SEO-optimized content,
  generate schema markup, design the UI, and build the complete website via Google Stitch.
user-invokable: true
argument-hint: "[command] [url]"
license: MIT
metadata:
  author: LocalStudios
  version: "1.0.0"
  category: website-generation
---

# LocalStudios — Website Generation Plugin

## Available Commands

| Command | Description |
|---------|-------------|
| `generate <url>` | Generate a complete SEO-optimized website from an existing URL |

## Command Routing

Parse the first argument to determine the command:

- **`generate`**: Execute the website generation workflow below. Second argument = target URL.
- **No argument**: Show the commands table above.
- **Unknown command**: Show "Unknown command. Available commands:" + table.

---

## Workflow: generate

### Execution Rules
1. Execute phases **sequentially** — never skip ahead
2. **Lazy load** references — only read a reference file when its phase begins
3. **Pause for user input** at Phase 2 (interview) and Phase 6 (outline approval)
4. Check external dependencies at runtime — graceful skip if unavailable
5. Use **subagents** for heavy work (content writing, schema generation)

### Reference Files (load on demand, NOT at startup)
- `./references/generate-workflow.md` — Complete phase-by-phase instructions
- `./references/scraping-checklist.md` — Phase 1: what to extract
- `./references/interview-template.md` — Phase 2: user interview format
- `./references/seo-rules.md` — Phase 3-4: keyword strategy rules
- `./references/content-guidelines.md` — Phase 7: writing rules
- `./references/schema-templates.md` — Phase 8: JSON-LD templates
- `./references/quality-checklist.md` — Phase 11: QA checklist

### Subagents (spawn when needed)
- `scraper` — Phase 1: scrape and analyze the URL
- `keyword-researcher` — Phase 3: Semrush keyword research
- `seo-auditor` — Phase 5: audit existing site with claude-seo
- `content-writer` — Phase 7: write content per page (spawn one per page in parallel)
- `schema-generator` — Phase 8: generate all JSON-LD markup
- `quality-checker` — Phase 11: final quality check

---

### Phase 1 — Scrape & Analyze

Load `./references/scraping-checklist.md`.

Use WebFetch on the provided URL. If URL is unreachable, mark everything as NOT FOUND and proceed to Phase 2.

Extract: company name, industry, services, NAP data, brand elements, existing SEO signals, page structure, tone.

Also fetch common subpages: /about, /contact, /services, /impressum (and localized variants).

---

### Phase 2 — User Interview [MANDATORY PAUSE]

Load `./references/interview-template.md`.

Show the structured checklist. Prefill values found in Phase 1 with `[FOUND — please confirm]`. Mark missing required items with `[MISSING — required]`.

**DO NOT proceed until the user has responded.**

After response, compile and display a **PROJECT BRIEF** summary. Ask user to confirm before continuing.

---

### Phase 3 — Keyword Research (max 5 Semrush calls)

**Check:** Are `mcp__semrush__*` tools available?

- **Yes**: Load `./references/seo-rules.md`. Execute keyword research using Semrush MCP. Max 5 API calls total. Priority: Overview → Variations → Geo combos → Service KWs → Competitor KWs.
- **No**: Use manual keywords from interview. Inform user.

Output: Keyword table with volume, difficulty, and page assignment.

---

### Phase 4 — Geo-Strategy per Page

Using `./references/seo-rules.md` (already loaded from Phase 3).

Map keyword clusters to pages. Each page targets a semantic cluster, not a single keyword. One strong page beats five thin pages.

Output: Complete keyword-to-page assignment table. Show to user.

---

### Phase 5 — SEO Audit (optional)

**Check:** Is `/seo` skill available AND was a URL provided?

- **Yes**: Run `/seo page`, `/seo technical`, `/seo schema` on the URL. Compile "What Was Wrong" list.
- **No**: Skip. Build new site with best practices from scratch.

---

### Phase 6 — Structure Outline [MANDATORY PAUSE]

Create complete page outline with:
- Per page: H1, section structure, keyword targets, meta title, meta description
- Internal linking map with anchor texts
- Show the complete outline to the user

**DO NOT proceed until user approves the outline.**

---

### Phase 7 — Content Writing

Load `./references/content-guidelines.md`.

Write SEO-optimized content for each page following:
- H1 with primary keyword + location
- H2s with secondary keywords and geo-terms
- Geo-term in first paragraph
- 1-2% keyword density
- E-E-A-T signals throughout
- FAQ sections on service pages (voice-search friendly)
- CTAs: above the fold + after social proof + end of page
- Internal links with keyword-rich anchor texts
- Meta Title (max 60 chars), Meta Description (max 155 chars)
- OG Title + OG Description for social sharing

Content can be written per page in parallel using the content-writer agent.

---

### Phase 8 — Schema Markup

Load `./references/schema-templates.md`.

Generate JSON-LD for all pages:
- **Home + Contact**: LocalBusiness (with sameAs for Google Business Profile), BreadcrumbList
- **Service pages**: Service schema, FAQPage schema, BreadcrumbList
- **About**: Organization/Person schema, BreadcrumbList
- Industry-specific @type (Dentist, Restaurant, etc.)

All NAP data must match exactly across all schema blocks. Use consistent @id references.

---

### Phase 9 — Design System

**Check:** Is `/ui-ux-pro-max` skill available?

- **Yes**: Use ui-ux-pro-max to generate industry-specific design system (colors, typography, layout pattern, anti-patterns).
- **No**: Create generic professional design system based on user preferences from interview.

---

### Phase 10 — Website Generation

**Check:** Are `mcp__stitch__*` tools available?

- **Yes**: Create project in Stitch. Generate each page with content from Phase 7, design from Phase 9, structure from Phase 6, and schema reference from Phase 8.
- **No**: Export everything as structured markdown files with all content, meta tags, schema blocks, and design specifications.

---

### Phase 11 — Quality Check

Load `./references/quality-checklist.md`.

Verify every item: SEO on-page, schema validation, content quality, internal linking, NAP consistency, conversion elements, technical requirements.

Report score as percentage. Fix critical issues before proceeding.

---

### Phase 12 — Final Report

Present complete summary:
- Keyword strategy (primary, secondary, geo-cluster count)
- Pages created with keyword targets
- SEO technical status (schema, meta, links, mobile)
- Design system applied
- Next steps for the client:
  1. Google Business Profile setup/update (NAP must match)
  2. Google Search Console + sitemap
  3. Collect 5-10 Google Reviews in first 30 days
  4. Expect rankings in 3-6 months
  5. Monthly SEO monitoring recommended

---

### Error Handling

| Scenario | Action |
|----------|--------|
| URL unreachable | Skip scraping, gather everything in interview |
| Semrush unavailable | Use manual keywords, inform user |
| claude-seo unavailable | Skip audit, use best practices |
| ui-ux-pro-max unavailable | Generic design system |
| Stitch unavailable | Export as markdown files |
| User skips optional interview items | Use sensible defaults |
| User skips required interview items | Ask again specifically |
| Semrush API call fails | Note error, continue with remaining calls |
| Schema field unknown | Mark as `[PLACEHOLDER — add value]` |
