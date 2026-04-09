# LocalStudios Plugin

## HARD RULES — read before writing ANY code

### 1. CENTERING — every section, always
```tsx
// EVERY section component MUST have this wrapper:
<section className="py-16 md:py-24 lg:py-32">
  <div className="mx-auto max-w-7xl px-4 sm:px-6 lg:px-8">
    {/* content here */}
  </div>
</section>
```
If a shadcnblocks block is not centered → add this wrapper.
If content appears left-aligned → the wrapper is missing. Fix it.

### 2. BUTTONS — visible, contrasty, generous
```tsx
// On DARK backgrounds: light button with dark text
<Button className="bg-white text-primary hover:bg-white/90 px-8 py-3 text-lg font-semibold">
  044 853 22 74
</Button>

// On LIGHT backgrounds: dark button with light text  
<Button className="bg-primary text-primary-foreground hover:bg-primary/90 px-8 py-3 text-lg font-semibold">
  Termin vereinbaren
</Button>

// Secondary on LIGHT bg: visible border + readable text
<Button variant="outline" className="border-2 border-primary text-primary hover:bg-primary hover:text-white px-6 py-2.5">
  Mehr erfahren
</Button>

// Secondary on DARK bg: transparent with white border — NEVER use variant="outline" as-is
<Button className="bg-transparent border-2 border-white text-white hover:bg-white hover:text-primary px-6 py-2.5">
  Öffnungszeiten
</Button>
```

**CRITICAL: shadcn Button variant="outline" sets its own bg/text colors that BREAK on dark backgrounds.**
NEVER use `variant="outline"` on dark sections. Instead use custom classes: `bg-transparent border-2 border-white text-white`.

**For EVERY button: check text is readable against BOTH the button bg AND the section bg.**

### 3. NO PILL BADGES / LABELS
```tsx
// VERBOTEN:
<span className="rounded-full border px-3 py-1">Regensdorf</span>

// STATTDESSEN — clean text:
<p className="text-muted-foreground">Regensdorf · Steinmaur · Oberglatt</p>
```

### 4. COLORS — only CSS variables
```tsx
// NEVER: bg-blue-600, text-gray-500, bg-[#1E3A8A], style={{color:'#333'}}
// ALWAYS: bg-primary, text-muted-foreground, bg-accent
```

### 5. FONTS — from design-system.md, not generic
Choose distinctive fonts that match the industry. Never default Inter/Sans.

### 6. IMAGES — minimum 5, never empty spaces
Styled placeholders where images aren't available.

### 7. SHADCNBLOCKS — vary your choices
Do NOT always pick hero1, feature1, testimonial1.
Search with SPECIFIC queries. Try different blocks each time.
Read the block descriptions before choosing.

---

## Project Documentation
- `docs/BUSINESS.md` — business facts
- `docs/SEO-STRATEGY.md` — keywords
- `docs/pages/home.md` — homepage SEO
- `design-system.md` — colors, fonts, atmosphere

## NAP Data
`lib/site-config.ts` — single source of truth.
