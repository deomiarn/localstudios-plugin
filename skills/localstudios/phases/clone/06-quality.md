# Clone Phase 6 — Quality Check + Playwright + SEO

Same structure as generate Phase 10 (QA → Playwright → SEO). Load `../10-quality.md` und execute den vollen Ablauf. Danach die Clone-spezifischen Checks unten ausführen.

---

## Clone-Specific Checks (zusätzlich zur generate Phase 10)

### Old Client Purge
- [ ] NO old client name/text remaining anywhere
- [ ] ALL NAP data ist neuer Client
- [ ] Schema referenziert neuen Client (name, @type, sameAs, description)
- [ ] `design.md` reflektiert Style-Änderungen (oder ist unverändert bei „Keep")
- [ ] docs/ verweisen alle auf neuen Client

Grep-Check:
```bash
grep -rn "<alter-client-name>" . --exclude-dir=node_modules --exclude-dir=.next
```
Erwartetes Ergebnis: keine Hits.

### Content Quality
- [ ] Total content: 1500-2500 Wörter
- [ ] Tonalität matcht Interview
- [ ] Keywords in den richtigen Sections laut SEO-STRATEGY.md
- [ ] Keine generischen Filler-Phrasen
- [ ] E-E-A-T Signale vorhanden

### Image Inventory
- [ ] Min. 5 Bilder pro Variante
- [ ] Alle Bilder mit alt-Text (keyword + location)
- [ ] Alle Bilder mit title
- [ ] Keine leeren Spaces — `<ImagePlaceholder>` wo Bilder fehlen
- [ ] Image Sources in `docs/pages/home.md` dokumentiert
- [ ] Above-fold Images mit `priority`

### Design Compliance
- [ ] `globals.css` matcht `design.md` (bei Style-Change regeneriert)
- [ ] Keine hardcoded Farben in Components
- [ ] Anti-Muster aus `design.md` nicht verletzt
- [ ] Page sieht custom aus, nicht wie Template-Recolor

---

## Playwright Visual Validation (MANDATORY)

Identisch zu generate Phase 10 Part 2:
1. Dev/Build starten
2. Pro Variante: Desktop 1440×900 + Mobile 375×812 Screenshots
3. Console-Errors + Network-404/5xx checken
4. Visueller Abgleich gegen `design.md` (Farben, Typo-Hierarchie, Spacing, Atmosphäre, Bilder, Buttons)
5. Abweichungen sofort im Code fixen → re-screenshot → bis Match

---

## /seo Audit (MANDATORY if available)

```
/seo page [localhost URL]
/seo schema [localhost URL]
/seo content [localhost URL]
```

**FIX all issues found. Re-run after fixes.**
