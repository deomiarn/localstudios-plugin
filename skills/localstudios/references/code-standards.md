# Code Standards for Generated Websites

The generated website code MUST follow clean code principles. This applies to both Stitch output and markdown export.

---

## File Structure

Every page and component gets its own file. No monolithic single-file websites.

```
website/
├── index.html                    # Home — imports shared components
├── about.html
├── services/
│   ├── [service-1].html
│   └── [service-2].html
├── contact.html
├── components/
│   ├── header.html               # Navigation + logo (shared)
│   ├── footer.html               # NAP + links (shared)
│   ├── hero.html                 # Hero section template
│   ├── trust-bar.html            # USP numbers/badges
│   ├── service-card.html         # Individual service card
│   ├── testimonial.html          # Single testimonial block
│   ├── cta-section.html          # Reusable CTA block
│   ├── faq-section.html          # FAQ accordion
│   ├── contact-form.html         # Contact form
│   └── map-embed.html            # Google Maps embed
├── css/
│   ├── variables.css             # Design tokens (colors, fonts, spacing)
│   ├── base.css                  # Reset + typography + base styles
│   ├── layout.css                # Grid, container, responsive
│   ├── components.css            # Component-specific styles
│   └── utilities.css             # Helper classes
├── js/
│   ├── main.js                   # Minimal JS (mobile menu, form validation)
│   └── analytics.js              # Tracking setup (placeholder)
├── assets/
│   ├── images/                   # Image placeholders with correct names
│   └── fonts/                    # Local font files if needed
├── schema/
│   ├── localbusiness.json        # LocalBusiness JSON-LD
│   ├── organization.json         # Organization schema
│   └── [page]-schema.json        # Per-page schema (FAQ, Service, Breadcrumb)
└── meta/
    └── sitemap.xml               # Sitemap structure
```

## CSS Architecture

### Design Tokens (variables.css)
All colors, fonts, spacing, and breakpoints as CSS custom properties:

```css
:root {
  /* Colors */
  --color-primary: #...;
  --color-secondary: #...;
  --color-accent: #...;
  --color-background: #...;
  --color-text: #...;
  --color-text-light: #...;
  --color-border: #...;

  /* Typography */
  --font-heading: '...', sans-serif;
  --font-body: '...', sans-serif;
  --font-size-base: 1rem;
  --font-size-lg: 1.25rem;
  --font-size-xl: 1.5rem;
  --font-size-2xl: 2rem;
  --font-size-3xl: 2.5rem;
  --line-height-tight: 1.2;
  --line-height-normal: 1.6;

  /* Spacing */
  --space-xs: 0.25rem;
  --space-sm: 0.5rem;
  --space-md: 1rem;
  --space-lg: 2rem;
  --space-xl: 4rem;
  --space-2xl: 6rem;

  /* Layout */
  --container-max: 1200px;
  --container-padding: 1rem;

  /* Breakpoints (for reference, use in media queries) */
  /* --bp-mobile: 375px */
  /* --bp-tablet: 768px */
  /* --bp-desktop: 1024px */
  /* --bp-wide: 1440px */
}
```

### No Inline Styles
- NEVER use inline `style=""` attributes
- All styling via classes referencing CSS custom properties
- Component styles scoped by component class name

### Responsive
- Mobile-first: base styles for 375px
- `@media (min-width: 768px)` for tablet
- `@media (min-width: 1024px)` for desktop
- `@media (min-width: 1440px)` for wide screens

## HTML Standards

### Semantic Elements
- `<header>` for site header
- `<nav>` for navigation
- `<main>` for page content
- `<section>` for content sections (with descriptive class)
- `<article>` for self-contained content blocks
- `<aside>` for sidebars
- `<footer>` for site footer
- NEVER use `<div>` when a semantic element fits

### Accessibility
- All images: `alt` attribute with descriptive, keyword-relevant text
- Form inputs: `<label>` with `for` attribute
- Buttons: clear text content, not just icons
- Skip-to-content link
- `aria-label` on navigation landmarks
- Color contrast ratio ≥ 4.5:1 for text
- Focus styles visible on all interactive elements

### Clean Markup
- No unnecessary wrapper divs
- No deprecated elements or attributes
- Consistent 2-space indentation
- Logical source order matching visual order
- Comments only for section boundaries: `<!-- Hero Section -->`

## JavaScript

### Minimal
- No frameworks unless absolutely needed
- No jQuery
- Vanilla JS only, modern syntax (ES6+)
- Only for: mobile menu toggle, form validation, smooth scroll
- `defer` attribute on all script tags

### No Render Blocking
- CSS in `<head>`, JS before `</body>` or with `defer`
- No inline `<script>` blocks in body content
- Critical CSS can be inlined in `<head>` for above-the-fold

## Performance

- Images: `loading="lazy"` on below-fold images
- Images: `width` and `height` attributes to prevent CLS
- Fonts: `font-display: swap` for web fonts
- Fonts: preload critical font files
- No unused CSS or JS shipped
- Minification-ready (no code that breaks when minified)

## Schema Embedding

Schema JSON-LD in `<head>` of each page, not inline in body:

```html
<head>
  ...
  <script type="application/ld+json">
    { /* loaded from schema/[page]-schema.json */ }
  </script>
</head>
```

## Naming Conventions

- Files: kebab-case (`service-card.html`, `trust-bar.html`)
- CSS classes: BEM-like (`hero__title`, `service-card__description`, `cta--primary`)
- CSS variables: kebab-case with category prefix (`--color-primary`, `--space-lg`)
- JS: camelCase for variables and functions
- IDs: kebab-case, only when needed for anchors or form labels
