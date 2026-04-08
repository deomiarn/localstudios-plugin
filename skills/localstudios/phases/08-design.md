# Phase 9 — Design System

**ui-ux-pro-max = Design System (colors, typography, layout rules)**
**21st.dev Magic = Components (pre-built, polished UI sections)**

These two work together: ui-ux-pro-max decides HOW it looks, 21st.dev provides WHAT is built.

---

## MANDATORY: Use /ui-ux-pro-max if available

Check if `/ui-ux-pro-max` skill is loaded.

### If available — YOU MUST USE IT:

**Step 1 — Generate design system:**
```
/ui-ux-pro-max plan [industry] website for [company name] in [city]
```

**Step 2 — Get build instructions:**
```
/ui-ux-pro-max build homepage with [recommended style] for [industry]
```

Take the output: colors (hex), typography (font names), layout pattern, effects, anti-patterns.

**DO NOT:**
- Invent your own colors
- Pick fonts yourself
- Use generic blue/white defaults

### If NOT available:

Create design system from interview preferences:
```
Colors: [from interview or brand scraping]
Typography: Google Fonts pairing for [industry]
Layout: Card-based, clean hierarchy
CTA: High-contrast
```

## Output

Design system brief with:
- Hex color codes (primary, secondary, accent, background, text)
- Font names (heading, body)
- Component styles
- Anti-patterns for this industry

Pass to Phase 10 — design tokens go into globals.css, component style guides to 21st.dev prompts.
