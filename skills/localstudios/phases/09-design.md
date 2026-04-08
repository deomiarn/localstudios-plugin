# Phase 9 — Design System

## MANDATORY: Use /ui-ux-pro-max if available

Check if `/ui-ux-pro-max` skill is loaded.

### If available — YOU MUST USE IT. No exceptions.

**Step 1 — Generate design system:**
```
/ui-ux-pro-max plan [industry] website for [company name] in [city]
```

**Step 2 — Apply to homepage:**
```
/ui-ux-pro-max build homepage with [style] for [industry]
```

Take the complete output (colors, typography, layout, effects, anti-patterns) and use it as the design foundation. Write all design tokens into `globals.css` in Phase 10.

**DO NOT:**
- Invent your own colors
- Pick fonts yourself
- Skip this skill because "you can do it faster"
- Use generic blue/white defaults

### If NOT available (skill not loaded):

Only then create a generic design system:

```
DESIGN SYSTEM (generic — ui-ux-pro-max not available)
Layout: Clean, card-based with clear hierarchy
Colors: From user interview preference
Typography: System fonts or Google Fonts pairing
CTA: High-contrast button
Mobile: Responsive, min 44px touch targets
```

## Output

Design system brief with hex codes, font names, component styles.
Pass to Phase 10 for globals.css.
