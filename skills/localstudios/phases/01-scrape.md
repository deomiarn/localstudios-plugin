# Phase 1 — Scrape & Analyze

## References
- Load `./references/scraping-checklist.md` for extraction targets
- Load `./references/image-strategy.md` for image scraping rules

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

## Image Scraping

Extract ALL usable images from the old website:

1. **Find all `<img>` tags** across all fetched pages
2. **Filter out**: tracking pixels (<5px), icons (<50px), stock watermarked images, base64 tiny images
3. **For each usable image, capture**:
   - Source URL
   - Original alt text
   - Original filename
   - Approximate dimensions (from width/height attributes or CSS)
   - Context: which page and section it was in
   - Type: logo / team photo / service image / storefront / background / other

4. **Also check for**:
   - `<picture>` elements with WebP sources
   - CSS background images on hero/banner sections
   - OpenGraph images (`og:image` meta tag)
   - Favicon / apple-touch-icon (for logo fallback)

5. **GBP Image Scraping**:
   - If a Google Business Profile URL was found (in social links or schema `sameAs`):
     - Fetch the GBP page via WebFetch
     - Extract business photos (exterior, interior, team, products)
   - If no GBP URL but business name + city are known:
     - Note for Phase 2: ask user for GBP URL

## Output

Structured results with FOUND/NOT FOUND status per field.

### Image Inventory:
```
| # | Image | Source | URL | Type | Size | Usable? |
|---|-------|--------|-----|------|------|---------|
| 1 | logo.png | Website | https://... | Logo | 200x80 | ✅ |
| 2 | hero.jpg | Website | https://... | Hero | 1920x800 | ✅ |
| 3 | team.jpg | GBP | https://lh3... | Team | 1200x800 | ✅ |
| 4 | icon.svg | Website | https://... | Icon | 24x24 | ❌ Too small |
```

Pass results to Phase 2 for interview prefill.
