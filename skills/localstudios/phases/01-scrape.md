# Phase 1 — Scrape & Analyze

## Reference
Load `./references/scraping-checklist.md` for extraction targets.

## Agent
Spawn `scraper` agent with the provided URL.

## Process

1. Use WebFetch on the provided URL
2. If URL unreachable → mark all fields as `[NOT FOUND]`, proceed to Phase 2
3. If reachable → also fetch common subpages:
   - /about, /about-us, /ueber-uns, /chi-siamo, /a-propos
   - /contact, /kontakt, /contatto
   - /services, /leistungen, /dienstleistungen, /servizi
   - /impressum, /imprint, /legal
   - /team

## Extract

- **Business**: name, industry, tagline, founding year
- **Services**: list, primary service, descriptions, pricing
- **NAP**: legal name, address, postal code, city, region, country, phone, email
- **Geo**: primary city, service areas, districts, neighborhoods
- **Content**: H1s, tone, language, USPs, testimonials, team, certifications
- **Brand**: logo, colors, fonts, social links
- **Technical**: page count, nav structure, meta tags, schema, mobile viewport
- **SEO**: heading keywords, alt texts, internal links, sitemap, robots.txt

## Output

Structured results with FOUND/NOT FOUND status per field.
Pass results to Phase 2 for interview prefill.
