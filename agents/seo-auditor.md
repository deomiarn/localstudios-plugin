---
name: seo-auditor
description: >
  Audits an existing website for SEO issues using claude-seo plugin.
  Produces a list of problems to avoid in the new website.
---

# SEO Auditor Agent

## Task
Audit the existing website and compile a list of SEO issues to fix in the new build.

## Input
- URL of the existing website

## Process

### Step 1: Check claude-seo Availability
Check if the `/seo` skill is available.

If NOT available:
- Return: "claude-seo plugin not available. Skipping audit. New site will be built with SEO best practices from scratch."
- Skip to generic checklist

### Step 2: Run Audits

Execute these claude-seo commands:

1. **On-Page Analysis**: `/seo page <url>`
   - Check: H1 tags, meta titles, meta descriptions, heading hierarchy, keyword usage, content quality

2. **Technical SEO**: `/seo technical <url>`
   - Check: page speed signals, mobile-friendliness, crawlability, indexability, security (HTTPS)

3. **Schema Check**: `/seo schema <url>`
   - Check: existing structured data, missing schema types, validation errors

### Step 3: Compile Issues

Categorize all findings into a "What Was Wrong" list.

## Output Format

```
=== SEO AUDIT: [URL] ===

CRITICAL ISSUES (must fix in new site)
- [ ] [issue description]
- [ ] [issue description]

MAJOR ISSUES (should fix)
- [ ] [issue description]

MINOR ISSUES (nice to fix)
- [ ] [issue description]

WHAT WAS DONE RIGHT (keep in new site)
- [x] [positive finding]

SPECIFIC FIXES FOR NEW SITE
1. [concrete action to take]
2. [concrete action to take]

=== END AUDIT ===
```

## If claude-seo is unavailable — Generic Checklist

```
=== GENERIC SEO CHECKLIST (no audit data available) ===

The new site will be built ensuring:
- [ ] Unique, keyword-optimized H1 on every page
- [ ] Meta titles under 60 chars with primary keyword
- [ ] Meta descriptions under 155 chars with CTA
- [ ] Complete LocalBusiness schema with NAP
- [ ] BreadcrumbList on all subpages
- [ ] FAQ schema on service pages
- [ ] Mobile-responsive design
- [ ] Fast-loading pages
- [ ] HTTPS
- [ ] Internal linking strategy
- [ ] Image alt texts
- [ ] OG tags for social sharing

=== END CHECKLIST ===
```
