# Phase 5 — Homepage Outline

**⚠ MANDATORY PAUSE — Do not proceed until user approves.**

## References
- Load `./references/page-sections.md` for mandatory sections (10 Sections, fixe Reihenfolge)
- Load `./references/image-strategy.md` for image placement

## Process

Erstelle die Homepage-Outline BEVOR irgendein Content geschrieben wird.
Das ist **eine** Homepage — alle 10 mandatory Sections aus `references/page-sections.md`.

## Homepage Sections (fixe Reihenfolge)

```
HOMEPAGE: [Company Name]

H1: [Primary Keyword + City]
Meta Title: [Primary KW — Company — City] (max 60 chars)
Meta Description: [Benefit + CTA + Location] (max 155 chars)

Sections:
  1. Hero — H1, Subheadline mit secondary KW, Primary + Sekundär CTA
         → PFLICHT: sichtbares Bild above-the-fold (Split oder Full-Bleed Background)
  2. Trust Bar — 3-4 Zahlen (Jahre, Kunden, Rating, Zertifikate)
  3. Featured Service 1 — alternierend, Bild + Text + eigener CTA
  4. Featured Service 2 — alternierend, Text + Bild + eigener CTA
  5. Services Grid — 3-6 kompakte Service-Cards
  6. About Teaser — 2-3 kompakte Absätze, Owner/Team-Foto
  7. Social Proof — 3-4 Testimonials mit Sternen
  8. Local Area + Map — Stadt/Stadtteile, "seit [Jahr]", Google Maps Embed
  9. CTA Section — Werteversprechen + Telefon
  10. Footer — NAP, Öffnungszeiten, Links

Optional: FAQ (6 Fragen) — voice-search friendly

Keywords: [Cluster aus Phase 4]
Images: [welches Bild pro Section, Source: scraped/GBP/generate/placeholder]
```

## Hero-Regel (KRITISCH)

Der Hero MUSS ein Bild above-the-fold haben. Text-only Hero ist verboten.

Zwei Layouts sind zulässig:
- **Split** — Text 55–60%, Bild 40–45%, Bild rechts oder links
- **Full-Bleed Background** — Bild als `<Image fill>` im Hintergrund, Text als Overlay

Bild-Proportion: `aspect-[4/5]` oder `aspect-[3/4]`. Niemals `aspect-video` (zu flach).

Wenn kein scraped Bild verfügbar → `<ImagePlaceholder>` mit passendem Aspect + sprechendem Label (z.B. „Praxis-Aussenansicht"). Keinen leeren Platz, keine Text-Platzhalter wie `[Image #1]`.

## Image Placement
Pro Section das Bild und die Quelle notieren:
- `[SCRAPED]` — vom alten Website
- `[GBP]` — aus Google Business Profile
- `[GENERATE]` — Banana/AI nötig
- `[PLACEHOLDER]` — noch keins, `<ImagePlaceholder>` verwenden

## Present to User

Zeige die komplette Outline. Frage: „Approve this homepage structure? Any changes?"

Do not proceed to Phase 6 until approved.

## Create docs/pages/home.md

Nach Approval `docs/pages/home.md` anlegen (Template aus `./references/docs-structure.md`):
- **Purpose**: Money Page — Client mit starkem Pitch akquirieren
- **Keywords**: primary, secondary, geo
- **Sections**: alle 10 mit Content-Notizen
- **Images**: Pro Section Source + Alt-Text-Entwurf (separate Dokumentation — NICHT in den Content-Text von Phase 6 mischen)
- **Design-Context** (optional, aus Phase 8 Verstehens-Protokoll)
