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

[ ] 3-5 Adjektive für Gefühl (wird im Phase-8-Verstehens-Protokoll erwähnt, NICHT in design.md eingetragen): ___
[ ] Anti-Muster — was NICHT? (Claude berücksichtigt es beim Layout-Plan, schreibt es NICHT in design.md): ___
[ ] Farbwechsel gegenüber Brand-Default? (NUR Farb-Tokens, z.B. "primary von gelb auf blau"): ___

=== ENDE ===
```

## Wie der Brief verarbeitet wird

1. **Phase 2 Interview** speichert die Design Source (Command ODER Pfad) in `docs/BUSINESS.md` unter „Design".
2. **Phase 8 Design System** liest den Eintrag:
   - Fall A: führt `npx getdesign@latest add <brand>` im Projekt-Root aus **EINMAL**, legt die ausgegebene `design.md` im Projekt-Root ab — **unverändert**.
   - Fall B: kopiert die existierende `design.md` ins Projekt-Root — **unverändert**.
3. **`design.md` wird NICHT normalisiert.** Das File ist ab jetzt READ-ONLY. Claude liest und versteht es, ergänzt aber keine Sektionen und formuliert nichts um. Fehlt eine Pflicht-Angabe (z.B. Farb-Tokens in HSL) → User fragen, NICHT selbst schreiben.
4. **Sonderfall Farbwechsel aus Phase 2**: NUR die betroffenen Farb-Tokens in `design.md` ersetzen. Alles andere bleibt 1:1.
5. **Phase 9 Build** leitet `globals.css` aus `design.md` ab (CSS-Vars + Tailwind v4 `@theme inline` Mapping).
6. **Phase 10 QA** validiert via Playwright MCP dass die gerenderte Seite `design.md` entspricht. Abweichungen werden IMMER im Code gefixt, NIE in `design.md`.

## Regeln
- **Entweder** getdesign Command **oder** design.md Pfad. Beides leer → Phase 2 bricht ab und fragt erneut.
- Der Command wird exakt wie vom User angegeben ausgeführt. Wenn er fehlschlägt, Claude informiert freundlich und fragt nach einer Alternative.
- `design.md` ist die einzige Design-Wahrheit im Projekt und **READ-ONLY** ab Phase 8. Keine Farben/Fonts/Spacing irgendwo hardcoden.
