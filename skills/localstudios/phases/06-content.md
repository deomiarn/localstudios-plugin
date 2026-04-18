# Phase 6 — Homepage Content

## References
- Load `./references/content-guidelines.md` for writing rules
- Load `./references/page-sections.md` for section structure (10 Sections)
- Load `./references/image-strategy.md` for image alt text rules

## Task
Write **hochwertigen, kompakten** Content für die Homepage. Substanz über Fülle — keine Hülsen, aber auch nicht zu ausführlich.

## Content Length
**1500-2000 Wörter** über alle 10 Sections (+ optional FAQ 200-300 Wörter). Wenn du merkst eine Section wird zu lang → kürzen. Lieber ein scharfer Absatz als drei schwammige.

## Section-by-Section Content

### 1. Hero (100-180 Wörter)
- H1 mit primary keyword + city
- Subheadline mit secondary keyword (1 Satz)
- Kurzer Absatz (2-3 Sätze) der sofort Vertrauen aufbaut
- Primary CTA Button Text
- Zweit-CTA (z.B. Telefonnummer oder "Mehr erfahren")

### 2. Trust Bar (50-100 Wörter)
- 3-4 Trust Signals mit konkreten Zahlen (Jahre, Kunden, Rating, Zertifikate)
- Optional: 1 Satz Begleittext

### 3. Featured Service 1 (100-150 Wörter)
- Service-Name + 3-4 Sätze Beschreibung
- Konkreter Nutzen für den Kunden
- Eigener CTA

### 4. Featured Service 2 (100-150 Wörter)
- Wie Featured Service 1 für den zweiten Haupt-Service
- Eigener CTA

### 5. Services Grid (150-250 Wörter gesamt)
- 3-6 weitere Services als kompakte Cards
- Pro Service: Name + 1-2 Sätze
- Kein langer Einleitungs-Absatz

### 6. About Teaser (120-200 Wörter)
- Gründungsgeschichte in 1-2 Absätzen
- Persönliche Note + Qualifikationen
- Lokale Verbindung zum Ort

### 7. Social Proof (120-180 Wörter)
- 3-4 Testimonials mit echten Details
- Jedes Testimonial: 2 Sätze mit konkretem Ergebnis + Sterne (wenn vorhanden)
- Keine lange Einleitung

### 8. Local Area (100-150 Wörter)
- Warum dieser Standort
- Welche Stadtteile/Gemeinden
- Erreichbarkeit (1 Satz)

### 9. CTA Section (60-100 Wörter)
- Werteversprechen-Zusammenfassung (1-2 Sätze)
- Konkreter nächster Schritt
- Telefonnummer prominent
- Öffnungszeiten-Hinweis

### 10. Footer (80-120 Wörter)
- Vollständiger NAP
- Öffnungszeiten
- Kurzlinks
- Copyright + Social Links

### Optional: FAQ (200-300 Wörter, 6 Fragen)
- 6 häufige Fragen mit je 1-3 Sätzen Antwort
- Natürliche Formulierung (voice-search-freundlich)
- Keyword/Geo-Term in mindestens 2 Antworten

## SEO Rules
- H1: primary keyword + city, **genau einmal**
- Geo-term in erstem Absatz + mindestens einer H2
- Keyword density 1-2%

## Meta Tags
- Meta Title: Primary KW — Company — City (max 60 chars)
- Meta Description: Benefit + CTA + Location (max 155 chars)
- OG Title + OG Description

## Tonalität
- Professionell aber persönlich
- Aus Erfahrung schreiben, nicht generisch
- Konkrete Zahlen und Details
- Lokale Referenzen die echt wirken
- KEIN „Willkommen auf unserer Website"

## Image-Metadaten — wichtige Regel

**Der Content-Text von Phase 6 wird in Phase 9 1:1 ins JSX eingesetzt.** Deswegen dürfen **niemals** Image-Platzhalter, Alt-Text-Hinweise oder Source-Notizen im Content-Text stehen.

VERBOTEN im Content-Text:
- `[Image #1]`, `[Image #2]`, `[Photo]`, `[IMG]`
- `<!-- bitte Foto hier -->`
- `Bild: Praxisfoto`

Bild-Beschreibungen, Alt-Texte, Source-Tracking (scraped/GBP/generate/placeholder) gehören **ausschliesslich** in `docs/pages/home.md` — dort dokumentiert Claude pro Section welches Bild aus welcher Quelle kommt.

## Output
Kompletter Content für alle 10 Sections (plus optional FAQ), Meta Tags, und separat in `docs/pages/home.md` die Image-Zuordnung pro Section.
