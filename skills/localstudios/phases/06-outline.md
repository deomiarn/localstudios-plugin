# Phase 6 — Website Structure Outline

**⚠ MANDATORY PAUSE — Do not proceed until user approves.**

## References
- Load `./references/page-sections.md` for mandatory sections per page type
- Load `./references/image-strategy.md` for image placement rules

## Process

Create the complete outline BEFORE any content is written.
Use the mandatory sections from `page-sections.md` — never skip or reorder them.

## Per-Page Outline Format

```
PAGE: [Name]
H1: [Primary Keyword + Location]
Keywords: [cluster from Phase 4]
Meta Title: [max 60 chars]
Meta Description: [max 155 chars]
Sections:
  1. [Section name] — [what it contains]
  2. [Section name] — [what it contains]
  ...
```

## Standard Page Templates

**HOME** (8 sections): Hero → Trust Bar → Services Overview → About Teaser → Social Proof → Local Area → CTA → Footer
**ABOUT** (6 sections): Hero/Intro → Our Story → Team → Credentials → Local Connection → CTA
**SERVICE** (7 sections): Hero → What & Why → Process → Benefits → FAQ → Testimonial → CTA
**CONTACT** (5 sections): Hero/NAP → Contact Form → Map → Opening Hours → Alternative Contact

See `./references/page-sections.md` for exact content, images, and SEO requirements per section.

### Image Placement in Outline
For each section, note which image goes there and its source:
- `[SCRAPED]` — from old website (Phase 1)
- `[GBP]` — from Google Business Profile
- `[GENERATE]` — needs Banana AI generation
- `[PLACEHOLDER]` — no image available yet

## Internal Linking Map

Show which pages link where and with what anchor texts:

```
[Source Page] → [Target Page] (anchor: "[text]")
```

Rules:
- Home → all service pages
- All service pages → Home + Contact
- Related services → each other
- Keyword-rich anchors, varied, never "click here"

## Present to User

Show complete outline + linking map. Ask: "Approve this structure? Any changes?"

Do not proceed to Phase 7 until approved.

## Create docs/pages/ Documentation

After user approves the outline, create a documentation file per page in `docs/pages/`:

```
docs/pages/
├── home.md
├── about.md
├── service-[name].md    (one per service)
└── contact.md
```

Use the per-page template from `./references/docs-structure.md`. Fill in:
- **Purpose**: page type (Money Page / Informational / Trust / Conversion / Navigational)
- **Goal**: what this page should achieve
- **Keywords**: primary, secondary, geo — with density targets
- **Meta**: title, description, OG tags (with char counts)
- **Headings**: H1 + all H2s with keyword intent
- **GEO Signals**: where city/district/region are mentioned
- **Schema**: which schema types go on this page
- **Internal Links**: outgoing + incoming with anchor texts
- **Notes**: anything specific

These docs are the **page-level source of truth**. Claude Code reads them before editing any page.
