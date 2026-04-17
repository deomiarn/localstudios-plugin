# Extend Phase 7 — Content for All Pages

## References
- Load `./references/content-guidelines.md` — ALL writing rules apply
- Load `./references/image-strategy.md` — alt text + title for every image
- Load `./references/seo-rules.md` — keyword clustering + geo-embedding
- Load `docs/SEO-STRATEGY.md` — keyword assignments per page

## Task
Write SEO-optimized content for every page. Same quality as homepage content.

---

## Tonalität per Page Type

| Page Type | Ton | Warum |
|-----------|-----|-------|
| Services Overview | Fachlich + einladend | Kompetenz zeigen, zum Klicken motivieren |
| Service Detail | Fachlich + persönlich | Expertise beweisen, Vertrauen aufbauen |
| About | Persönlich + warm | Gründungsgeschichte, menschliche Seite |
| Team | Persönlich + professionell | Menschen hinter der Firma zeigen |
| Regions | Lokal + vertrauenswürdig | Ortskenntnisse beweisen |
| FAQ | Freundlich + hilfreich | Fragen direkt und klar beantworten |
| Blog | Informativ + zugänglich | Wissen teilen, Expertise demonstrieren |
| Contact | Einladend + konkret | Hemmschwelle senken, klare nächste Schritte |

## VERBOTEN (auf ALLEN Pages)
- "Willkommen auf unserer Website"
- "Wir sind ein führendes..."
- "Herzlich willkommen bei..."
- "Klicken Sie hier"
- Generic Fülltext ohne Substanz
- Keyword Stuffing
- Duplicate Content zwischen Pages
- Textwände ohne Zwischenüberschriften

---

## Content per Page Type

### Services Overview (600-800 words)
- Intro paragraph with primary KW + city
- Brief description per service (2-3 sentences each)
- Why choose us for these services
- **E-E-A-T:** Years of experience, number of clients, certifications
- **Images:** Alt text per service card: "[Service] — [brief description]"

### Service Detail Pages (800-1200 words each)
- H1: [Service] in [City]
- What is it (2-3 paragraphs)
- Who is it for (target audience)
- Process/steps (detailed, numbered)
- Benefits (concrete, not generic — numbers where possible)
- FAQ (5-7 voice-search questions, per content-guidelines.md FAQ rules)
- CTA with phone + booking
- **E-E-A-T:** Specific methods/tools used, qualifications relevant to this service
- **Images:** Alt text: "[Service] process — step [n]: [action]"

### About (800-1200 words)
- Founding story (who, when, why)
- Mission and values (concrete, not corporate)
- What makes us different (USPs with proof)
- Local connection (why this location, community involvement)
- **E-E-A-T:** First-person perspective, qualifications, awards, partnerships
- **Images:** Alt text: "[Name], [Role] at [Company] in [City]"

### Team Overview (300-500 words)
- Intro about the team culture and expertise
- Brief per person (name, role, 1 sentence about specialty)
- Warm, personal tone — these are real people

### Team Member Detail (400-600 words each)
- Personal story — why this profession
- Qualifications and experience with specifics
- Specialties and approach
- Local connection
- **E-E-A-T:** Certifications, years of experience, specific expertise
- **Images:** Alt text: "[Name], [Role] at [Company] in [City]"

### Regions Overview (400-600 words)
- Service area description with geo-terms
- Why local presence matters
- Brief per region (1-2 sentences)

### Region Detail Pages (600-800 words each)
- H1: [Service] in [Region/City]
- What we offer in this area
- How to reach us from there (public transport, parking)
- Local references (landmarks, neighborhoods)
- Strong GEO signals (postal codes, district names, local institutions)
- **E-E-A-T:** Local expertise, years serving this area
- **Images:** Alt text: "[Service area] in [Region] — [what's shown]"

### FAQ (all questions, grouped by category)
- Natural, voice-search friendly phrasing
- 2-4 sentence answers — directly answering the question
- Service name + city in at least 2 answers per category
- Link to relevant service page in answer where helpful
- **Phrasing examples:**
  - Good: "Wie viel kostet eine Zahnreinigung in Zürich?"
  - Bad: "Zahnreinigung Preise"

### Blog Posts (800-1500 words each)
- **H1:** Topic + primary KW (unique per post)
- **Intro paragraph:** What, why, for whom (2-3 sentences, becomes Google snippet)
- **3-5 H2 sections** with substantial content (150-250 words each)
- **FAQ section:** 3-5 questions with voice-search phrasing
- **CTA:** Link to relevant service page
- **Author box:** Name, role, 1-sentence credential
- **Meta:** Unique title (≤60 chars) + description (≤155 chars)
- **Internal links:** Min 2 to service pages, 1 to about/team
- **E-E-A-T:** Author expertise, specific examples, industry terminology explained
- **Images:** Cover image + min 1 in-content image, alt text with topic + keyword

### Contact (200-300 words)
- Welcoming intro (low barrier)
- Clear instructions for form
- Alternative contact methods (phone, email, WhatsApp)
- Opening hours prominent

### Impressum + Datenschutz
- Legal text, factual, complete
- Swiss/German/Austrian format as appropriate

---

## SEO Rules (all pages)

- H1 with primary KW + city — exactly ONE per page, unique across site
- Meta Title ≤ 60 chars — unique per page
- Meta Description ≤ 155 chars — unique per page, includes CTA + location
- OG tags set (title + description)
- Geo-terms distributed naturally (city in H1 + first paragraph, district in H2, region in body)
- Keyword density 1-2% — natural, never stuffed
- BreadcrumbList schema on every subpage

## Internal Linking Map

Every page links to related pages. Follow this matrix:

```
Home → Services Overview, About, Contact
Services Overview → Each Service Detail, Contact, About
Service Detail → Related Services (2-3), FAQ, Contact, Relevant Region
About → Team Overview, Services Overview, Contact
Team Overview → Each Team Member, About
Team Member → About, Services (their specialty), Contact
Regions Overview → Each Region Detail, Services Overview
Region Detail → Services available there, Contact, Other nearby Regions
FAQ → Relevant Service Details, Contact
Blog Overview → Recent Posts, Categories
Blog Post → Related Services (2+), Other Posts (2-3), About/Team (author), Contact
Contact → Services Overview, About, Google Maps
Impressum → Datenschutz
Datenschutz → Impressum, Contact
```

**Every page must have at least 3 internal links.** Anchor text should be descriptive (not "click here").

## Image Alt Text per Page Type

Follow templates from `image-strategy.md`. Every image gets:
1. **alt** attribute: descriptive + keyword + location (5-15 words)
2. **title** attribute: benefit/CTA oriented

Document all images with sources in `docs/pages/[page].md`.
