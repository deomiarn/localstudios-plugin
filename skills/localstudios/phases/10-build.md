# Phase 10 — Website Build

## Reference
Load `./references/code-standards.md` for clean code requirements.

## Dependency Check
Are `mcp__stitch__*` tools available?

### If Stitch available:

1. Create project: `mcp__stitch__create_project` with company name
2. Per page, generate screen: `mcp__stitch__generate_screen_from_text`
   - Pass: content (Phase 7), design system (Phase 9), section structure (Phase 6)
   - Instruct Stitch to use semantic HTML, component-based structure, CSS custom properties
3. Apply consistent design across all pages
4. Optionally generate variants: `mcp__stitch__generate_variants`

### If Stitch NOT available:

Generate clean, production-ready HTML/CSS following `./references/code-standards.md`:

```
output/
├── index.html                    # Home
├── about.html
├── services/
│   └── [service].html
├── contact.html
├── components/
│   ├── header.html               # Shared navigation
│   ├── footer.html               # Shared footer with NAP
│   ├── hero.html
│   ├── trust-bar.html
│   ├── service-card.html
│   ├── testimonial.html
│   ├── cta-section.html
│   ├── faq-section.html
│   ├── contact-form.html
│   └── map-embed.html
├── css/
│   ├── variables.css             # Design tokens
│   ├── base.css                  # Reset + typography
│   ├── layout.css                # Grid + responsive
│   ├── components.css            # Component styles
│   └── utilities.css             # Helpers
├── js/
│   └── main.js                   # Minimal (menu, form validation)
├── schema/
│   └── [page]-schema.json        # Per-page JSON-LD
└── meta/
    └── sitemap.xml
```

### Code Quality Rules
- Semantic HTML (`<header>`, `<nav>`, `<main>`, `<section>`, `<footer>`)
- No inline styles — all via CSS classes + custom properties
- Mobile-first responsive (375px → 768px → 1024px → 1440px)
- BEM-like class naming (`hero__title`, `cta--primary`)
- Images: `loading="lazy"`, `width`/`height` for CLS prevention
- Accessibility: `alt` texts, `<label>` for inputs, focus styles, contrast ≥ 4.5:1
- JS deferred, no render blocking
- Each component = own file, reusable across pages
