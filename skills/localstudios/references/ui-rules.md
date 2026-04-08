# UI Rules — Strikte CSS/Code Regeln

Diese Regeln gelten IMMER. Kein Code wird geschrieben ohne sie einzuhalten.
Sie ergänzen den `/frontend-design` Skill:
- **frontend-design** = WAS das Design sein soll (Richtung, Komposition, Atmosphäre)
- **ui-rules** = WIE es technisch umgesetzt wird (CSS, Variablen, Code-Struktur)

---

## Farben

### ALLE Farben über shadcn CSS Variablen
```css
/* Diese Variablen in globals.css definieren */
--background, --foreground
--primary, --primary-foreground
--secondary, --secondary-foreground
--accent, --accent-foreground
--muted, --muted-foreground
--destructive, --destructive-foreground
--card, --card-foreground
--border, --input, --ring
```

### VERBOTEN in Komponenten-Code
```tsx
/* NIEMALS: */
className="bg-[#1E3A8A]"        /* hardcoded hex */
className="text-blue-600"        /* Tailwind named color statt Variable */
style={{ color: '#333' }}         /* inline style */
className="bg-sky-500"           /* direkte Tailwind Farbe */

/* IMMER: */
className="bg-primary"           /* shadcn Variable */
className="text-muted-foreground" /* shadcn Variable */
className="bg-accent"            /* shadcn Variable */
```

### Einzige Ausnahme
Tailwind Opacity-Varianten von shadcn Variablen sind OK:
```tsx
className="bg-primary/10"  /* primary mit 10% opacity — OK */
```

---

## Typografie

### Fonts nur in globals.css definieren
```css
:root {
  --font-heading: 'FontName', sans-serif;
  --font-body: 'FontName', sans-serif;
}
```

### In Komponenten nur referenzieren
```tsx
className="font-heading"  /* referenziert --font-heading */
className="font-body"     /* referenziert --font-body */
```

### Schriftgrössen über Tailwind
```tsx
className="text-sm"    /* 0.875rem */
className="text-base"  /* 1rem */
className="text-lg"    /* 1.125rem */
className="text-xl"    /* 1.25rem */
className="text-2xl"   /* 1.5rem */
className="text-4xl"   /* 2.25rem */
```

---

## Spacing & Layout

### Über Tailwind Klassen
```tsx
className="p-4 md:p-8"      /* responsive padding */
className="gap-6"            /* grid/flex gap */
className="space-y-8"        /* vertikaler Abstand */
className="max-w-7xl mx-auto" /* Container */
```

### Section Spacing konsistent
```css
/* In globals.css definieren: */
.section {
  @apply py-16 md:py-24 lg:py-32;
}
```

---

## Border Radius

### Über shadcn --radius Variable
```css
:root {
  --radius: 0.625rem;
}
```

### In Komponenten
```tsx
className="rounded-lg"   /* nutzt --radius */
className="rounded-full"  /* für Pills/Avatare */
```
Kein `rounded-[12px]` — immer Tailwind Klassen.

---

## Schatten & Tiefe

### Über Tailwind Shadow Klassen
```tsx
className="shadow-sm"
className="shadow-md"
className="shadow-lg"
```
Kein `shadow-[0_4px_12px_rgba(0,0,0,0.1)]` — zu spezifisch.

---

## Hover & Focus States

### Konsistent über alle Komponenten
```tsx
className="hover:bg-primary/90 transition-colors"       /* Buttons */
className="hover:bg-accent transition-colors"            /* Cards */
className="focus-visible:ring-2 focus-visible:ring-ring" /* Focus */
```

### Transitions
```tsx
className="transition-colors duration-200"  /* Farb-Übergänge */
className="transition-all duration-300"     /* Komplexe Animationen */
```
Immer `transition-*` Klassen, nie CSS `transition` Property direkt.

---

## Dark Mode

### Vorbereitet über CSS Variablen
```css
:root {
  --background: 0 0% 100%;
  --foreground: 0 0% 3.9%;
}
.dark {
  --background: 0 0% 3.9%;
  --foreground: 0 0% 98%;
}
```
Komponenten brauchen keine `dark:` Prefix-Klassen wenn alles über Variablen läuft.

---

## shadcnblocks Anpassung

### Wie man Blocks korrekt anpasst

1. **Block installieren**: `npx shadcn add @shadcnblocks/hero2`
2. **Hardcoded Farben ersetzen**: Alle `bg-blue-*`, `text-gray-*` etc. → shadcn Variablen
3. **Fonts anpassen**: `font-heading`, `font-body` statt hardcoded Font-Namen
4. **Spacing angleichen**: Section-Padding auf `.section` Klasse umstellen
5. **Border Radius**: Auf `--radius` umstellen
6. **Content ersetzen**: Platzhalter-Text mit echtem Content aus Phase 6
7. **Mit /frontend-design verfeinern**: Layout, Komposition, Atmosphäre verbessern

### Was man NICHT ändern soll
- Responsive Grid-Struktur (die ist meistens gut)
- Component-Komposition (Import-Struktur beibehalten)
- Accessibility-Attribute (aria-*, role, etc.)

---

## Image Placeholder

### Wenn Bilder nicht verfügbar sind (Cloudflare, Auth etc.)

NIEMALS leere Spaces oder fehlende Bilder im Design.
IMMER stilvolle Placeholder einsetzen:

```tsx
/* Gradient Placeholder */
<div className="aspect-video rounded-lg bg-gradient-to-br from-primary/20 to-accent/20 flex items-center justify-center">
  <span className="text-muted-foreground text-sm">Praxisfoto</span>
</div>

/* Icon Placeholder */
<div className="aspect-square rounded-lg bg-muted flex items-center justify-center">
  <ImageIcon className="h-8 w-8 text-muted-foreground/50" />
</div>
```

Placeholder MÜSSEN:
- Gleiche Proportionen (aspect-ratio) wie das echte Bild
- Gleiche Rundungen (rounded-lg etc.)
- Zum Farbschema passen (shadcn Variablen)
- Einen beschreibenden Text oder Icon haben

---

## Verboten

- `style={}` Props
- `.module.css` Dateien
- `styled-components` oder CSS-in-JS
- `!important`
- Hardcoded Farb-Werte (hex, rgb, hsl)
- Hardcoded Font-Namen in Komponenten
- Leere Spaces wo Bilder sein sollten
- `className="bg-white"` oder `className="text-black"` (→ `bg-background`, `text-foreground`)
