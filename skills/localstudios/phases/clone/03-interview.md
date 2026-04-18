# Clone Phase 3 — Interview (kurz, fokussiert)

**⚠ MANDATORY PAUSE — Do not proceed until user responds.**

## Context
Das Source-Projekt ist bekannt. Fokus auf: was ist ANDERS beim neuen Kunden?

## Checklist

```
=== CLONE: NEUER KUNDE ===

BUSINESS (wie bei generate)
[ ] Company name: ___
[ ] Industry: ___
[ ] Main location: ___
[ ] Year founded: ___

CONTACT
[ ] Full address: ___
[ ] Phone: ___
[ ] Email: ___
[ ] Opening hours: ___
[ ] Google Business Profile URL: ___
[ ] Google Maps Embed URL: ___

SEO
[ ] Primary Keyword: ___ [REQUIRED]
[ ] 2-3 Secondary Keywords: ___
[ ] Target audience: ___

SERVICES
[ ] Main services (3-6): ___
[ ] Welche 2-3 sollen prominent hervorgehoben werden? ___

TRUST
[ ] Years / clients / rating: ___
[ ] Certifications: ___

CONVERSION
[ ] Main goal: Call / Form / Book / Purchase: ___

TONALITÄT
[ ] Ton der Website: formal / persönlich / freundlich / fachlich: ___

=== DESIGN ===

STYLE
[ ] Style vom Source-Projekt beibehalten? Ja / Anpassen / Komplett neu: ___

Falls "Ja" → design.md + globals.css des Source-Projekts werden übernommen. Weiter zu WARTE AUF ANTWORT.

Falls "Anpassen" oder "Komplett neu" → NEUE DESIGN SOURCE erforderlich (genau eine Option):

  Option A — getdesign CLI Command
  [ ] Command (z.B. "npx getdesign@latest add nike"): ___

  Option B — Existierende design.md
  [ ] Absoluter Pfad zur Datei: ___

  FEINTUNING (optional)
  [ ] 3-5 Adjektive für Gefühl: ___
  [ ] Anti-Muster — was NICHT?: ___
  [ ] Farbwechsel gegenüber Brand-Default? (NUR Farb-Tokens): ___

Falls "Ja, aber nur Farbwechsel" → Design bleibt, NUR Farb-Tokens in der übernommenen design.md werden ersetzt:
  [ ] Farbwechsel (z.B. "primary: gelb → blau"): ___

=== WARTE AUF ANTWORT ===
```

## After Response

### 1. Validate Design Source (nur falls Style geändert werden soll)
- Wenn „Anpassen/Neu" gewählt aber weder Command noch Pfad angegeben → STOP. Nochmals fragen.
- Wenn Command → roh speichern (Ausführung in Phase 5 Adapt).
- Wenn Pfad → File-Existenz prüfen.

### 2. Compile Brief
```
=== CLONE BRIEF ===
New Client: [name] — [industry] — [city]
Source: [source project name]
Ton: [formal / persönlich / freundlich / fachlich]
Style: [keep / adapt / new]
Design Source (falls adapt/new): [A: getdesign command "___"  |  B: design.md at "___"]
Adjektive: [list falls angegeben]
Anti-Muster: [list falls angegeben]
=== END ===
```

### 3. Create `docs/BUSINESS.md` with new client data
Speichere die Design Source (Command ODER Pfad) im Abschnitt „Design" — Phase 5 Adapt liest sie dort.

### 4. Keyword-to-Section Mapping (Geo-Strategy)
Map die keywords aus Phase 4 zu Homepage-Sections:
```
PRIMARY: [keyword] → H1 + first paragraph
SECONDARY: [kw1] → H2 Services, [kw2] → H2 About, [kw3] → H2 Local
GEO: [city] → H1 + first paragraph, [district] → one H2, [region] → about/local section
```
Speichern in `docs/SEO-STRATEGY.md`. Diese Mapping ist erforderlich vor Content-Writing in Phase 5.

### 5. Adaptation Preview (MANDATORY PAUSE)
Zeige dem User was die adaptierte Homepage enthalten wird:
```
=== ADAPTATION PREVIEW ===
Sections: [kept from source — list all 10]
Design: [keep / changes: neue design.md aus "___"]
Keywords: [primary] → Hero, [secondary] → Sections
Images: [x from scrape, x placeholders needed]
Content: Fresh — 1500-2000 words (+ optional FAQ 200-300)
=== Bestätigen? ===
```
