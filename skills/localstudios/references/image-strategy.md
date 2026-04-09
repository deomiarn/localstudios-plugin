# Image Strategy

## Source Priority (cost-aware)

Images are sourced in this strict order. Only move to the next source if the previous one doesn't provide enough.

### 1. Scrape from Existing Website (FREE)
- Extract all `<img>` URLs from the old site via WebFetch
- Download/reference usable images (logo, team photos, service images, storefront)
- Keep original alt texts as reference (but rewrite for SEO)
- Skip: stock watermarked images, tiny icons, tracking pixels

### 2. Scrape from Google Business Profile (FREE)
- If GBP URL is known, fetch the profile page
- Extract business photos: exterior, interior, team, products/services
- GBP photos are often the best quality real photos of the business
- These are authentic and build trust — prioritize them

### 3. AI Generation via Banana (COSTS — last resort only)
- Only generate images that cannot be sourced from steps 1-2
- Typical need: hero backgrounds, abstract section dividers, service illustrations
- Generate at **1024x1024 minimum** resolution
- Use `/banana` skill with specific, descriptive prompts
- Never generate fake team photos or fake storefronts — that's deceptive

### Source Tracking
For each image, document in `docs/pages/[page].md`:
```
| Image | Source | Original URL | Status |
|-------|--------|-------------|--------|
| hero.webp | Old website | https://old.site/img/hero.jpg | ✅ Scraped |
| team.webp | GBP | https://lh3.google... | ✅ Scraped |
| service-bg.webp | Banana | Generated | ✅ Generated |
| office.webp | — | — | ❌ Placeholder |
```

---

## Format & Optimization

### Always WebP
- Convert ALL images to WebP format before use
- Use `next/image` component which handles format optimization automatically
- If source is JPG/PNG, convert to WebP (Next.js does this at build/serve time)
- Name files as `.webp`: `hero.webp`, `team-photo.webp`

### Resolution
- **Minimum**: 1024px on the longest side
- **Hero images**: 1920x1080 (full-width sections)
- **Card images**: 800x600 or 1:1 aspect ratio
- **Team photos**: 600x600 (square, for consistency)
- **Thumbnails**: 400x300 minimum
- Always provide `width` and `height` to prevent CLS

### File Naming Convention
```
[page]-[section]-[descriptor].webp

Examples:
home-hero-storefront.webp
home-services-teeth-whitening.webp
home-team-dr-mueller.webp
about-story-founding.webp
service-cleaning-process-step1.webp
contact-exterior-office.webp
```

---

## SEO Image Optimization

### Alt Text Rules

Alt text must be:
- **Descriptive**: What the image actually shows
- **Keyword-relevant**: Include primary or secondary keyword naturally
- **Location-aware**: Include city/neighborhood when contextually appropriate
- **Concise**: 5-15 words, one sentence

### Alt Text Templates per Section

| Section | Alt Text Pattern | Example |
|---------|-----------------|---------|
| Hero | [Service] in [City] — [what's shown] | "Dental practice in Zurich — modern treatment room" |
| Team | [Name], [Role] at [Company] in [City] | "Dr. Anna Müller, lead dentist at SmileCare Zurich" |
| Service Card | [Service] — [brief description] | "Professional teeth whitening treatment" |
| Process Step | [Service] process — step [n]: [action] | "Teeth cleaning process — step 2: polishing" |
| Testimonial | [Client name] — [Company] client | "Marco R. — satisfied SmileCare patient" |
| Storefront | [Company] office in [City/District] | "SmileCare dental practice in Zurich Oerlikon" |
| Certificate | [Certification name] — [Company] | "Swiss Dental Association certified — SmileCare" |
| Local/Map | [Neighborhood/District] area in [City] | "Oerlikon district in Zurich North" |

### Title Attribute
Every image also gets a `title` attribute:
- Slightly more detailed than alt text
- Include a call-to-action or benefit when relevant
- Example: `title="Visit our modern dental practice in Zurich Oerlikon — book your appointment today"`

### In Next.js Code
```tsx
import Image from "next/image"

<Image
  src="/images/home-hero-storefront.webp"
  alt="Dental practice in Zurich — modern treatment room"
  title="Visit our modern dental practice in Zurich Oerlikon"
  width={1920}
  height={1080}
  priority  // only for above-fold images
  className="..."
/>
```

---

## Image Count per Page Type

**Bilder machen Pages schöner. Lieber ein Bild mehr als zu wenig.**

| Page | Min Images | Max Images | Must-Have |
|------|-----------|-----------|-----------|
| Home | 8 | 14 | Hero, service cards, team teaser, testimonials, storefront |
| About | 6 | 12 | Team photo, owner portrait, certificates, local/community |
| Service | 4 | 8 | Service hero, process steps, result/before-after |
| Contact | 1 | 2 | Storefront/exterior + map embed |

### Image Inventory Check
Before Phase 10 (Build), verify:
- [ ] Every must-have image slot is filled (scraped, GBP, or generated)
- [ ] All images are WebP or will be converted by next/image
- [ ] All images have SEO alt text + title attribute
- [ ] All images meet minimum resolution (1024px)
- [ ] No placeholder images remain
- [ ] Image source tracked in docs/pages/[page].md

---

## Placeholder Strategy

If an image cannot be sourced (Cloudflare, auth, no old site, no GBP):
- **Skippen ist OK** — kein Problem
- Aber **NIEMALS leere Spaces** im Design lassen
- IMMER stilvolle Placeholder einsetzen die zum Design passen:

```tsx
/* Gradient Placeholder — passt sich dem Farbschema an */
<div className="aspect-video rounded-lg bg-gradient-to-br from-primary/20 to-accent/20 flex items-center justify-center">
  <span className="text-muted-foreground text-sm">Praxisfoto</span>
</div>

/* Icon Placeholder */
<div className="aspect-square rounded-lg bg-muted flex items-center justify-center">
  <ImageIcon className="h-8 w-8 text-muted-foreground/50" />
</div>
```

Placeholder Regeln:
- Gleiche Proportionen (aspect-ratio) wie das echte Bild
- Gleiche Rundungen und Schatten
- shadcn Farb-Variablen (nie hardcoded)
- Beschreibender Text oder passendes Icon
- Mark in docs als `❌ Placeholder — needs real image`
