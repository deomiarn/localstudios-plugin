# Phase 6 — Website Structure Outline

**⚠ MANDATORY PAUSE — Do not proceed until user approves.**

## Process

Create the complete outline BEFORE any content is written.

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

**HOME**: Hero (H1 + CTA) → Trust Bar (3 USPs) → Services Overview (cards + links) → Social Proof → Local Trust → CTA repeat → Footer (NAP)

**ABOUT**: Team Story → Credentials/Certifications (E-E-A-T) → Photo placeholders → Local connection

**SERVICE** (per service): What is it → Who is it for → Process/Steps → Benefits → FAQ (5-7 questions) → CTA → Internal links

**CONTACT**: NAP prominent → Contact form → Maps embed → Opening hours → Additional contact methods

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
