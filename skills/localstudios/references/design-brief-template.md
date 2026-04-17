# Design Source Template

Das Design kommt aus **einer** Pflicht-Quelle. Der User liefert sie in Phase 2.
Keine Referenz-Screenshots, keine Adjektiv-Karussells — die eigentlichen Tokens kommen aus `getdesign` oder einer vorhandenen `design.md`.

---

```
=== DESIGN SOURCE (PFLICHT — genau eine Option) ===

Option A — getdesign CLI Command
[ ] Command (z.B. "npx getdesign@latest add nike"): ___

Option B — Existierende design.md im Projekt
[ ] Absoluter Pfad zur Datei: ___

=== FEINTUNING (optional) ===

[ ] 3-5 Adjektive für Gefühl (fliesst in design.md Anti-Muster/Kommentare): ___
[ ] Anti-Muster — was NICHT? (z.B. "kein Dark Mode", "nicht verspielt"): ___
[ ] Anzahl Design-Varianten (Default: 3): ___

=== ENDE ===
```

## Wie der Brief verarbeitet wird

1. **Phase 2 Interview** speichert die Design Source (Command ODER Pfad) in `docs/BUSINESS.md` unter „Design".
2. **Phase 8 Design System** liest den Eintrag:
   - Fall A: führt `npx getdesign@latest add <brand>` im Projekt-Root aus, konsolidiert das Output in `design.md`.
   - Fall B: kopiert die existierende `design.md` ins Projekt-Root.
3. **`design.md` wird normalisiert** (alle Pflicht-Sektionen: Farben HSL, Typografie, Buttons, Section/Layout, Atmosphäre, Anti-Muster).
4. **Phase 9 Build** generiert `globals.css` 1:1 aus `design.md` — CSS-Vars, Utility-Klassen, Typografie-Scale.
5. **Phase 10 QA** validiert via Playwright MCP, dass die gerenderte Seite den `design.md`-Tokens tatsächlich entspricht.

## Regeln
- **Entweder** getdesign Command **oder** design.md Pfad. Beides leer → Phase 2 bricht ab und fragt erneut.
- Der Command wird exakt wie vom User angegeben ausgeführt. Wenn er fehlschlägt, Claude informiert freundlich und fragt nach einer Alternative.
- `design.md` ist die einzige Design-Wahrheit im Projekt. Keine Farben/Fonts/Spacing irgendwo hardcoden.
