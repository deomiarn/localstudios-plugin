# Phase 8 — Design System (design.md READ-ONLY + layout-plan.md)

## Outputs
1. **`design.md`** im Projekt-Root — **UNVERÄNDERT** aus getdesign-Output ODER User-Quelle übernommen. Read-only.
2. **`layout-plan.md`** im Projekt-Root — EIN Layout-Plan für EINE Homepage (section-für-section Komposition + Bild-Plan), abgeleitet aus design.md.

---

## CRITICAL RULE — design.md ist READ-ONLY

**Nach Step 1 wird `design.md` NICHT mehr angefasst.**

- Keine „Normalisierung"
- Keine Ergänzung fehlender Sektionen
- Keine Umformulierung
- Kein Umsortieren
- Kein „Konsolidieren"

Die einzige Ausnahme: wenn der User in Phase 2 einen Farbwechsel genannt hat (z.B. „primary von gelb auf blau"), ersetze **nur** die betroffenen Farb-Tokens in `design.md` — Werte, sonst nichts. Gefühl, Typografie, Button-Stil, Atmosphäre, Anti-Muster, Bild-Regeln, Wording: alles 1:1 drin lassen.

Wenn Pflicht-Angaben fehlen → **User fragen**, nicht selbst erfinden/ergänzen.

Wenn das Design-Ergebnis später nicht passt (Phase 10) → der **Code** wird angepasst, niemals `design.md`.

---

## Step 1 — design.md beschaffen

Lies die **Design Source** aus `docs/BUSINESS.md` (in Phase 2 gespeichert).

### Fall A — getdesign CLI Command vorhanden

Beispiel: `npx getdesign@latest add nike`

```bash
# Aus Projekt-Root ausführen — EINMAL
<exakter command aus Phase 2>
```

- Den Command **exakt** ausführen wie vom User angegeben.
- Output prüfen: legt der Command eine `design.md` / `DESIGN.md` / Tokens-File ab?
  - Ja → Datei als `design.md` im Projekt-Root verwenden (ggf. umbenennen/verschieben — **Inhalt unverändert**).
  - Unterverzeichnis / anderes Format → User fragen wie weiter, nicht selbst mergen.
  - Command fehlgeschlagen → User fragen (Command falsch? Brand existiert nicht?).

### Fall B — Pfad zu existierender design.md vorhanden

```bash
cp "<Pfad aus Phase 2>" ./design.md
```

Falls die Datei bereits im Projekt-Root liegt → belassen.

### Farbwechsel (Sonderfall aus Phase 2)

Wenn der User in Phase 2 einen Farbwechsel genannt hat:
- **Nur** die explizit genannten Farb-Tokens in `design.md` ersetzen (z.B. `--primary` Wert)
- Restliche Tokens, Text, Beschreibungen → unverändert

Keinen Farbwechsel? Dann design.md bleibt **1:1** so wie sie beschafft wurde.

---

## Step 2 — design.md LESEN und VERSTEHEN

**Bevor Claude irgendeine Design-Entscheidung trifft, wird `design.md` komplett gelesen und verstanden.** Nur mit dem Inhalt dieser Datei als Basis wird der Layout-Plan geschrieben und später im Build umgesetzt.

Claude liest `design.md` komplett und fasst dem User gegenüber kurz zusammen:

```
=== design.md verstanden ===

Gefühl / Atmosphäre: [aus design.md — z.B. "editorial, warm, viel Weissraum, dezente Schatten"]
Farben-Philosophie: [z.B. "warmes Beige/Creme Background, kontrastiger Dark-Primary, Orange-Accent für CTAs"]
Typografie: [Heading-Font + Body-Font aus design.md, Scale-Richtung]
Button-Stil: [pill / rounded / sharp, Padding-Gefühl]
Bild-Regeln: [z.B. "warme Fotografie, natürliches Licht, Menschen im Fokus"]
Anti-Muster: [was laut design.md NICHT gemacht werden soll]

=== Auf dieser Basis wird layout-plan.md gebaut. ===
```

Dieser Block ist ein **Antwort-Text an den User** und wird optional auch am Anfang von `docs/pages/home.md` als „Design-Context"-Abschnitt abgelegt. Er landet **nicht** in `design.md`.

### Fehlende Pflicht-Angaben

Wenn `design.md` eine Pflicht-Angabe nicht enthält (z.B. keine Farb-Tokens in HSL, keine Fonts, kein Radius-Wert), **User fragen**. Beispiel:

```
design.md enthält keine expliziten HSL-Farb-Tokens. Was soll ich tun?
(a) Du fügst die Tokens selbst in design.md hinzu und wir laufen weiter
(b) Du gibst sie mir hier im Chat — ich schreibe sie einmalig in globals.css ohne design.md anzufassen
```

Claude ergänzt **nie** selbst Werte in `design.md`.

---

## Step 3 — `layout-plan.md` schreiben

EIN Layout-Plan für EINE Homepage. Abgeleitet aus `design.md` (Gefühl/Atmosphäre/Bild-Regeln).

```markdown
# Layout Plan — [Company Name]

## Herleitung aus design.md
- Gefühl: [aus design.md]
- Visueller Schwerpunkt: [bild-schwer / typografie-schwer / grid-schwer — basierend auf design.md]
- Atmosphäre-Akzent: [aus design.md — Schatten, Rundungen, Texturen]
- Layout-Rhythmus: [z.B. "alternierend breit/schmal, grosszügiger Weissraum"]

## Section Layout Plan (10 Sections, fixe Reihenfolge per `references/page-sections.md`)

| # | Section | Layout-Ansatz | Komposition | Bild-Plan |
|---|---------|---------------|-------------|-----------|
| 1 | Hero | **PFLICHT: above-the-fold Bild** — Split (Text links 55–60%, Bild rechts 40–45%) ODER Full-Bleed-Image mit Overlay-Text | H1 + Subheadline + 2 CTAs inline + sichtbares Bild | 1× Hero-Bild (scraped oder ImagePlaceholder `aspect-[4/5]` / `aspect-[3/4]`) |
| 2 | Trust Bar | Inline Zahlen, minimal, volle Breite | 3–4 Zahlen + kurzer Begleittext | — (reine Zahlen + Icons) |
| 3 | Featured Service 1 | Alternierend: Bild links, Text rechts | Headline + 3–4 Sätze + eigener CTA | 1× Service-Bild |
| 4 | Featured Service 2 | Alternierend: Text links, Bild rechts | Headline + 3–4 Sätze + eigener CTA | 1× Service-Bild |
| 5 | Services Grid | 3-Spalten Cards | Icon + Name + 1–2 Sätze pro Service | 1× Icon/Bild pro Card (optional) |
| 6 | About Teaser | Split: Text + Owner-/Team-Foto | 2–3 kompakte Absätze, Gründungsjahr + Stadt | 1× Portrait/Team-Foto |
| 7 | Social Proof | Zentrierte grosse Zitate ODER Card-Grid | 3–4 Testimonials, Sterne, Ort-Nennung | Optional: Profilbilder |
| 8 | Local Area + Map | Split: Text links, Google-Maps-Embed rechts | Stadt/Stadtteile/„seit [Jahr]" | 1× Maps-Embed |
| 9 | CTA Section | Full-Width Accent-Background | Grosser CTA + Telefon clickable | Optional: Gradient oder Foto |
| 10 | Footer | Mehrspaltig, NAP + Social | Identisch mit Header-Links | Logo |

## Hero-Spezifikation (aus design.md ableiten)

- Layout: **Split** (Text 55–60% / Bild 40–45%) ODER **Full-Bleed Background** mit Overlay-Text
- Bild-Proportion im Split: `aspect-[4/5]` oder `aspect-[3/4]` — NICHT `aspect-video` (zu flach)
- Wenn kein scraped Bild verfügbar: `<ImagePlaceholder label="<kurzer Kontext>" className="aspect-[4/5]" />`
- Oben im Viewport (above-the-fold) muss das Bild sichtbar sein — KEIN text-only Hero

## FAQ (optional, 6 Fragen — siehe Phase 6 Content)
Keine eigene Layout-Zeile in der Tabelle oben, wird als eigene Section unter 10 angehängt falls im Content-Outline vorhanden.
```

---

## Step 4 — Validierung

Bevor Phase 9 startet, prüfen:
- [ ] `design.md` im Projekt-Root — UNVERÄNDERT ggü. Quelle (ausser explizitem Farbwechsel aus Phase 2)
- [ ] `design.md` wurde komplett gelesen und verstanden (Verstehens-Block wurde dem User gezeigt)
- [ ] `layout-plan.md` im Projekt-Root
- [ ] layout-plan.md hat alle 10 Sections mit Layout + Komposition + Bild-Plan
- [ ] Hero-Eintrag spezifiziert explizit ein above-the-fold Bild (Split oder Full-Bleed)
- [ ] Fehlende Pflicht-Angaben in design.md wurden nicht eigenmächtig ergänzt — stattdessen beim User nachgefragt

## Output

- `design.md` — im Projekt-Root (read-only ab jetzt)
- `layout-plan.md` — im Projekt-Root (einziger Layout-Plan für Phase 9)
