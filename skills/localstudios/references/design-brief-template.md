# Design Brief Template

Dieser Brief wird im Interview (Phase 2) vom User ausgefüllt.
Er ist der WICHTIGSTE Input für das Design — ein Bild sagt mehr als 1000 Worte.

---

```
=== DESIGN BRIEF ===

REFERENZEN (der grösste Hebel)
[ ] 1-3 Screenshots oder URLs von Websites die dir gefallen: ___
[ ] Was genau gefällt daran? (Farben? Layout? Typografie? Bilder?): ___

GEFÜHL
[ ] 3-5 Adjektive die beschreiben wie die Website wirken soll: ___
    Beispiele: modern, warm, vertrauenswürdig, premium, clean,
    elegant, bodenständig, mutig, freundlich, professionell

FARBEN
[ ] Primary Farbe (Hex-Code oder "wie Referenz X"): ___
[ ] Accent/CTA Farbe (Hex-Code oder "wie Referenz X"): ___
[ ] Oder: "soll zur Branche passen — AI entscheidet"

TYPOGRAFIE
[ ] Serif (elegant, traditionell) / Sans-Serif (modern, clean) / Egal: ___
[ ] Oder: "wie auf Referenz X"

BESONDERES
[ ] Was soll auffallen? (z.B. "grosse Bilder", "viel Weissraum", "Karten-Layout"): ___

ANTI-MUSTER
[ ] Was NICHT? (z.B. "kein Dark Mode", "nicht verspielt", "keine Animationen"): ___

=== ENDE BRIEF ===
```

## Wie der Brief verarbeitet wird

1. **Referenz-Screenshots**: Claude analysiert die URLs/Bilder und extrahiert:
   - Farbpalette (Primary, Accent, Background)
   - Layout-Muster (Hero-Stil, Grid-Nutzung, Whitespace)
   - Typografie-Stil (Serif/Sans, Gewicht, Grössen)
   - Atmosphäre (Schatten, Rundungen, Gradients)

2. **Adjektive** steuern die Gesamtrichtung:
   - "warm + vertrauenswürdig" → warme Farben, weiche Rundungen, Testimonials prominent
   - "modern + clean" → viel Whitespace, Sans-Serif, minimale Schatten
   - "premium + elegant" → Serif Headings, dezente Animationen, dunkle Akzente

3. **Farben + Typografie** werden direkt in globals.css übernommen

4. **Anti-Muster** werden als Verbotsliste in design-system.md dokumentiert
