# Phase 9 — Design System

## Dependency Check
Is `/ui-ux-pro-max` skill available?

### If available — USE IT:

Invoke the ui-ux-pro-max skill directly:

```
/ui-ux-pro-max plan [industry] website for [company name] in [city]
```

This returns:
- Recommended UI style from 50+ options
- Industry-specific color palette
- Typography pairing (Google Fonts)
- Layout pattern (e.g. "Hero-Centric + Social Proof")
- Key effects and hover states
- Anti-patterns to avoid for this industry

Then apply the design system to the homepage by using:

```
/ui-ux-pro-max build homepage with [style] for [industry]
```

Take the output and write all design tokens into `globals.css` in Phase 10.

### If NOT available:

Create a generic professional design system based on the interview:

```
DESIGN SYSTEM
Layout: Clean, card-based with clear hierarchy
Colors: User preference from interview, or neutral professional palette
Typography: System fonts or popular Google Fonts pairing
CTA: High-contrast button, consistent across sections
Mobile: Responsive, touch-friendly targets (min 44px)
```

## Output

Design system brief with:
- **Style**: which of the 50+ ui-ux-pro-max styles was chosen (or "generic")
- **Colors**: primary, secondary, accent, background, text (hex codes)
- **Typography**: heading font, body font, sizes
- **Component styles**: buttons, cards, navigation
- **Anti-patterns**: what to avoid for this industry
- **Breakpoints**: 375px, 768px, 1024px, 1440px

Pass to Phase 10 to write into globals.css.
