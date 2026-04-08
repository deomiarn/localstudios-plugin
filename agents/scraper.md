---
name: scraper
description: >
  Quick website scrape to extract key business data.
  Max 7 requests. Uses Playwright MCP for browser-rendered content.
---

# Website Scraper Agent

## Task
Extract key business data from the target URL for homepage generation.

## Rules
- **Max 7 requests total.** No exceptions.
- Use **Playwright MCP** (`mcp__playwright__*`) as primary tool. Fallback: WebFetch.
- Ask for specific data points — never "extract everything"
- Missing data is fine — note it for the interview phase
- No Wayback Machine, no Google Cache, no archive.org

## Process

**Request 1**: Navigate to homepage with Playwright
- `mcp__playwright__browser_navigate` → URL
- `mcp__playwright__browser_snapshot` → DOM
- Get: company name, H1, meta title/description, nav items, phone, email, address, logo, colors, social links, hero image, services mentioned

**Request 2**: Fetch sitemap.xml
- Get: list of all pages
- If 404: use nav menu from homepage

**Request 3**: Navigate to contact/kontakt page
- Get: full address, phone, email, opening hours, map

**Request 4**: Navigate to about/ueber-uns page
- Get: team names + roles, founding year, certifications

**Request 5**: Navigate to services/leistungen page
- Get: service names with one-line descriptions

**Requests 6-7** (optional): Impressum or key service subpage

## Image Collection
From all visited pages, collect usable images:
- Filter: skip icons <50px, tracking pixels, base64 tiny images
- Keep: hero, team photos, service images, storefront, logo
- Note: URL, alt text, type, approximate size

## Output

```
=== SCRAPE: [URL] ===

BUSINESS: [name] — [industry]
NAP: [address] | [phone] | [email]
HOURS: [found/missing]
SERVICES: [list]
PAGES: [from sitemap/nav]
IMAGES: [count usable]
TONE: [formal/casual]
LANGUAGE: [detected]

MISSING: [list for interview]
=== END ===
```
