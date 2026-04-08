# Phase 7 — Content Writing

## References
- Load `./references/content-guidelines.md` for writing rules
- Load `./references/page-sections.md` for mandatory sections per page
- Load `./references/image-strategy.md` for image alt text + title rules

## Agent
Spawn `content-writer` agent per page. Pages can be written in parallel.

## Per-Page Requirements

### Structure
- ONE H1 with primary keyword + location, at the top
- 3-6 H2s with secondary keywords and geo-terms
- Short paragraphs (2-4 sentences)

### First Paragraph
Must contain: primary keyword + city + what the business does. 2-3 sentences.

### SEO
- Keyword density: 1-2% primary, 0.5-1% secondary
- Geo-term in first paragraph + at least one H2
- No keyword stuffing — every mention reads naturally

### E-E-A-T
- Experience perspective ("We have been serving...")
- Expertise (qualifications, methods, terminology)
- Authority (years, client count, certifications)
- Trust (NAP, reviews, local references)

### CTAs
- Above the fold (hero section)
- After social proof section
- End of page (before footer)
- Action verbs: "Book", "Call", "Get", "Schedule"

### Internal Links
- Follow linking map from Phase 6
- Keyword-rich anchor texts, varied
- Never "click here"

### Meta Tags (every page)
- Meta Title: Primary KW — Company — City (max 60 chars)
- Meta Description: Benefit + CTA + Location (max 155 chars)
- OG Title + OG Description for social sharing

### FAQ (service pages only)
- 5-7 questions, voice-search phrasing
- 2-4 sentence answers
- Service name + location in 2+ answers

## Content Length
- Home: 600-1000 words
- About: 400-700 words
- Service: 800-1200 words
- Contact: 200-400 words

### Images per Section
- Follow `page-sections.md` for which sections need images
- For each image, write:
  - **alt text**: descriptive + keyword + location (see templates in `image-strategy.md`)
  - **title attribute**: slightly more detailed, include benefit or CTA
  - **filename**: `[page]-[section]-[descriptor].webp`
- Source: use scraped images from Phase 1 first, GBP second, Banana last
- All images must be WebP (Next.js handles conversion via next/image)
- Minimum 1024px resolution

## Output per Page
Complete content + meta tags + internal links list + keyword density report + image assignments with alt/title texts.
