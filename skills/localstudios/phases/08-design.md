# Phase 8 — Design System

## MANDATORY: Use /ui-ux-pro-max

This skill has 67 UI styles, 97 color palettes, 100 industry reasoning rules.
It MUST lead the entire design process. No manual design decisions.

---

### Step 1 — Generate Design System

```
/ui-ux-pro-max plan [industry] [service] website for [company] in [city]
```

This runs 5 parallel searches (product type, style, color, landing page pattern, typography) and applies industry-specific reasoning rules.

Output includes:
- **Pattern**: landing page structure (hero-centric, social-proof-focused, etc.)
- **Style**: from 67 options (e.g. Soft UI Evolution, Swiss Modernism 2.0, etc.)
- **Colors**: full palette with hex codes (primary, secondary, CTA, background, text)
- **Typography**: font pairing with Google Fonts links
- **Effects**: transitions, shadows, hover states, animations
- **Anti-patterns**: what NOT to do for this industry

### Step 2 — Persist the Design System

```
/ui-ux-pro-max design-system --persist
```

This creates `design-system/MASTER.md` as the reference for all subsequent work.

### Step 3 — Get Stack-Specific Guidelines

```
/ui-ux-pro-max stack nextjs shadcn
```

Get Next.js + shadcn/ui specific implementation rules.

### Step 4 — Get Landing Page Specifics

```
/ui-ux-pro-max landing [recommended pattern from Step 1]
```

Get detailed section-by-section layout guidance for the recommended pattern.

---

## Output for Phase 9

The complete design system to be written into globals.css:

```
DESIGN SYSTEM (from ui-ux-pro-max)

PATTERN: [e.g. Hero-Centric + Social Proof]
STYLE: [e.g. Soft UI Evolution]

COLORS (exact hex → convert to HSL for shadcn)
--primary: [hex → hsl]
--secondary: [hex → hsl]
--accent/CTA: [hex → hsl]
--background: [hex → hsl]
--foreground: [hex → hsl]
--muted: [hex → hsl]

TYPOGRAPHY
Heading: [Font Name] (Google Fonts)
Body: [Font Name] (Google Fonts)

EFFECTS
Shadows: [specific values]
Transitions: [duration, easing]
Hover states: [specific behavior]
Border radius: [value]

ANTI-PATTERNS
- [what to avoid for this industry]

LANDING PAGE STRUCTURE
- [section order and layout per pattern]
```

**Phase 9 MUST use these exact values. No deviations.**
**The design-system/MASTER.md file is the single source of truth.**
