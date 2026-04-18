# Phase 2 — Interview + Design Source

**⚠ MANDATORY PAUSE — Do not proceed until user responds.**

## References
- Load `./references/interview-template.md` for business checklist
- Load `./references/design-brief-template.md` for design source rules
- Load `./references/docs-structure.md` for BUSINESS.md template

## Process

Present TWO checklists: Business Info + Design Source.
Prefill from Phase 1 scraping where possible.

---

## Part 1 — Business Information

```
=== BUSINESS INFO ===

BUSINESS
[ ] Company name: ___
[ ] Industry: ___
[ ] Main location (city + region): ___
[ ] Year founded: ___

SEO
[ ] Primary Keyword (e.g. "Zahnarzt Zürich"): ___ [REQUIRED]
[ ] 2-3 Secondary Keywords: ___
[ ] Target audience: ___

CONTACT (for schema + footer)
[ ] Full address: ___
[ ] Phone: ___
[ ] Email: ___
[ ] Opening hours: ___
[ ] Google Business Profile URL: ___
[ ] Google Maps Embed URL (für Karte auf der Website): ___

SERVICES (for overview section)
[ ] Main services (3-6): ___
[ ] Welche 2-3 Hauptdienstleistungen sollen prominent hervorgehoben werden? ___

TRUST
[ ] Years experience / clients / Google rating: ___
[ ] Certifications or awards: ___

CONVERSION
[ ] Main goal: Call / Form / Book / Purchase: ___
```

## Part 2 — Design Source (PFLICHT)

Das Design kommt aus **einer** von zwei Quellen. Der User muss genau eine liefern:

```
=== DESIGN SOURCE (PFLICHT) ===

Option A — getdesign CLI Command:
[ ] Command (z.B. "npx getdesign@latest add nike"): ___

Option B — Existierende design.md:
[ ] Pfad zur vorhandenen design.md: ___

(Entweder A ODER B ausfüllen. Beides leer → Phase bricht ab.)

=== FEINTUNING (optional) ===

[ ] 3-5 Adjektive für Gefühl (z.B. "modern, warm, vertrauenswürdig"): ___
[ ] Anti-Muster — was NICHT? (z.B. "kein Dark Mode", "nicht verspielt"): ___
[ ] Farbwechsel gegenüber Brand-Default? (NUR Farb-Tokens, z.B. "primary von gelb auf blau" — sonst nichts): ___

=== WARTE AUF ANTWORT ===
```

---

## After Response

### 1. Validate Design Source
- Wenn weder Option A noch B gefüllt → STOP. Freundlich nochmals fragen.
- Wenn A: den Command roh speichern (Ausführung erst in Phase 8).
- Wenn B: den Pfad prüfen (File existiert?). Wenn nicht → User fragen.
- **design.md ist READ-ONLY** — nach Phase 8 wird sie nicht mehr angefasst. Falls der User einen Farbwechsel genannt hat, wird NUR der entsprechende Farb-Token ersetzt. Alle anderen Inhalte (Gefühl, Typografie, Anti-Muster, Wording) bleiben 1:1.

### 2. Show PROJECT BRIEF
```
=== PROJECT BRIEF ===
Business: [name] — [industry] — [city]
Primary Keyword: [keyword]
Services: [list]
Goal: [call/form/booking]
Design Source: [A: getdesign command "___"  |  B: design.md at "___"]
Adjektive: [list, falls angegeben]
Anti-Muster: [list, falls angegeben]
Farbwechsel: [none | z.B. "primary: gelb → blau"]
=== END BRIEF ===
```

### 3. Create docs/BUSINESS.md
Nutze das Template aus `./references/docs-structure.md`.
Speichere die Design Source (Command ODER Pfad) im Abschnitt „Design" — Phase 8 liest sie dort.

### 4. Update CLAUDE.md with docs references
Stelle sicher, dass `design.md` im CLAUDE.md als Pflicht-Input-Referenz steht (wird in Phase 8 erzeugt/übernommen).
