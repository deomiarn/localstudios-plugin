# Phase 1 — Scrape & Analyze

**Keep it fast and simple. Max 5-7 requests total. Missing data is OK — Phase 2 interview catches it.**

## Agent
Spawn `scraper` agent with the URL.

## Process

### Step 1 — Fetch Homepage (1 request)
Navigate to the URL. Extract:
- Company name, tagline
- Navigation menu items (= list of pages that exist)
- H1, meta title, meta description
- Services mentioned
- Phone, email, address if visible
- Logo URL, brand colors
- Social media links
- Images: hero image, team photos, service images (URLs + alt texts)

### Step 2 — Check Sitemap (1 request)
Fetch `[domain]/sitemap.xml`. If it exists:
- Get the list of all pages
- Identify which are service pages, contact, about, etc.
If it doesn't exist → use nav menu items from Step 1.

### Step 3 — Fetch Key Subpages (max 3-4 requests)
Pick the most important pages from sitemap or nav. Priority:
1. **Contact/Kontakt** — for NAP, opening hours, map
2. **About/Über uns** — for team, founding year, certifications
3. **Services/Leistungen** — for service list with descriptions
4. **Impressum** — for legal name, address (if not found yet)

Skip pages that are clearly redundant or unimportant (blog posts, gallery, etc.).

### Step 4 — Extract Images
From all fetched pages, collect usable images:
- Filter out: icons (<50px), tracking pixels, base64 tiny images
- Keep: hero images, team photos, service images, storefront, logo
- Note: URL, alt text, type (hero/team/service/logo), approximate size

## What NOT to do
- No Wayback Machine / archive.org
- No Google Cache
- No CDX API queries
- No "extract EVERYTHING" prompts — ask for specific data points
- No more than 7 total requests
- Don't worry about missing data — interview handles gaps

## Output

Brief summary with FOUND / NOT FOUND per category:

```
=== SCRAPE RESULTS: [URL] ===

BUSINESS: [name] — [industry]
NAP: [found/partial/missing]
SERVICES: [count found]
PAGES: [list from sitemap/nav]
IMAGES: [count usable]
TONE: [formal/casual/friendly]
LANGUAGE: [detected]

MISSING (will ask in interview):
- [list of what wasn't found]

=== END SCRAPE ===
```

Pass to Phase 2.
