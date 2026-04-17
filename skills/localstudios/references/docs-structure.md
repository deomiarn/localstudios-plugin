# Documentation Structure

Every generated project gets a `docs/` directory as the single source of truth.
Claude Code reads these files via CLAUDE.md references to stay in context.

---

## File Structure

```
docs/
├── BUSINESS.md              # Business bible — all key facts
├── SEO-STRATEGY.md          # Overall keyword strategy + geo targeting
└── pages/
    ├── home.md              # Per-page: purpose, keywords, SEO, GEO
    ├── about.md
    ├── service-[name].md
    └── contact.md
```

---

## BUSINESS.md — Business Bible

The central reference for everything about the client. Scraped from website + GBP + interview. Keep it concise and structured.

```markdown
# [Company Name] — Business Profile

## Identity
- **Legal Name**: [name]
- **Brand Name**: [name if different]
- **Industry**: [category]
- **Founded**: [year]
- **Owner/Founder**: [name]

## NAP (Name, Address, Phone)
- **Address**: [street], [postal code] [city], [region], [country]
- **Phone**: [number]
- **Email**: [email]
- **Website**: [domain]

## Opening Hours
| Day | Hours |
|-----|-------|
| Monday | [HH:MM – HH:MM] |
| Tuesday | [HH:MM – HH:MM] |
| Wednesday | [HH:MM – HH:MM] |
| Thursday | [HH:MM – HH:MM] |
| Friday | [HH:MM – HH:MM] |
| Saturday | [closed / HH:MM – HH:MM] |
| Sunday | [closed / HH:MM – HH:MM] |

## Services
| Service | Description | Primary? |
|---------|-------------|----------|
| [name] | [one-line description] | ✅ / — |

## Unique Selling Points
1. [USP 1]
2. [USP 2]
3. [USP 3]

## Target Audience
- [description]

## Geographic Coverage
- **Primary City**: [city]
- **Districts/Neighborhoods**: [list]
- **Region/State**: [region]
- **Service Radius**: [description]

## Social & Profiles
- **Google Business Profile**: [URL or "not set up"]
- **Instagram**: [URL]
- **Facebook**: [URL]
- **LinkedIn**: [URL]
- **Other**: [URLs]

## Brand
- **Logo**: [available / not available]
- **Primary Colors**: [hex codes]
- **Tone of Voice**: [formal / casual / professional / friendly]
- **Language(s)**: [list]

## Design
- **Source**: [A: getdesign command  |  B: existierende design.md]
- **Command**: [z.B. `npx getdesign@latest add nike`]  _(nur wenn Option A)_
- **design.md Path**: [absoluter Pfad]                _(nur wenn Option B)_
- **Adjektive**: [3-5 Adjektive aus Phase 2, falls angegeben]
- **Anti-Muster**: [Liste aus Phase 2, falls angegeben]
- **Variants**: [N] (default: 3)

## Certifications & Trust Signals
- [certification/award/membership]

## Notes
[Anything special — promotions, seasonal changes, upcoming events]
```

---

## SEO-STRATEGY.md — Keyword & Geo Strategy

```markdown
# SEO Strategy — [Company Name]

## Primary Keyword
**[keyword]** — Volume: [x]/mo — Difficulty: [x] — Intent: [informational/transactional]

## Keyword Map

| Keyword | Volume | Difficulty | Target Page | Type | Intent |
|---------|--------|------------|-------------|------|--------|
| [kw] | [vol] | [diff] | Home | Primary | Transactional |
| [kw] | [vol] | [diff] | Service X | Secondary | Transactional |
| [kw] | [vol] | [diff] | About | Supporting | Informational |
| [kw] | [vol] | [diff] | Contact | Geo | Navigational |

## Geo Targeting
- **Primary Geo**: [city]
- **Secondary Geos**: [districts, region]
- **Geo-Cluster Strategy**: [brief description of how geo terms are distributed]

## Internal Linking Map
```
Home → Service A ("[anchor text]")
Home → Service B ("[anchor text]")
Home → Contact ("[anchor text]")
Service A → Contact ("[anchor text]")
Service A ↔ Service B ("[anchor text]")
```

## Competitors
| Competitor | URL | Primary KW | Strength |
|-----------|-----|-----------|----------|
| [name] | [url] | [kw] | [what they do well] |

## Source
- Data from: [Semrush / Manual / Interview]
- Date: [YYYY-MM-DD]
```

---

## Per-Page Documentation — docs/pages/[page].md

```markdown
# [Page Name]

## Purpose
**Type**: [Money Page / Informational / Trust / Conversion / Navigational]
**Goal**: [What this page should achieve — e.g. "Convert visitors to appointment bookings"]

## SEO

### Keywords
| Keyword | Role | Density Target |
|---------|------|---------------|
| [kw] | Primary | 1-2% |
| [kw] | Secondary | 0.5-1% |
| [kw] | Geo | natural mentions |

### Meta
- **Title**: [title] ([char count])
- **Description**: [description] ([char count])
- **OG Title**: [og title]
- **OG Description**: [og description]

### Headings
- **H1**: [heading text]
- **H2s**: [list of H2 headings with keyword intent]

## GEO Signals
- **City mentioned in**: [first paragraph, H2, footer]
- **District/Neighborhood**: [where mentioned]
- **Region/State**: [where mentioned]
- **Local references**: [landmarks, local names, community refs]

## Schema
- [list of schema types on this page: LocalBusiness, Service, FAQ, Breadcrumb]

## Internal Links
| From This Page | To | Anchor Text |
|---------------|-----|-------------|
| [section] | [target page] | "[anchor]" |

## Incoming Links
| From | Anchor Text |
|------|-------------|
| [source page] | "[anchor]" |

## Content Stats
- **Word Count**: [x]
- **Sections**: [count]
- **FAQ Questions**: [count or n/a]
- **CTAs**: [count + positions]

## Notes
[Anything specific — seasonal content, A/B test ideas, future improvements]
```

---

## CLAUDE.md Integration

The project CLAUDE.md MUST reference docs/ so Claude always has context:

```markdown
## Project Documentation
- **Read `docs/BUSINESS.md`** for all business facts (NAP, hours, services)
- **Read `docs/SEO-STRATEGY.md`** for keyword targets and geo strategy
- **Read `docs/pages/[page].md`** before editing any page — contains keywords, purpose, SEO rules
- **Update docs/** when making changes — keep documentation in sync with code
```
