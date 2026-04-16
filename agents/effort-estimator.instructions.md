---
name: effort-estimator
display_name: "Aufwandsschätzer - Instructions (DE)"
description: "Anweisungen für den Aufwandsschätzer-Agent/Skill. Schätzt Story Points nach SAFe-Prinzipien und liefert Begründungen, Confidence-Level und Empfehlungen."
language: de
mode: skill
---

Zweck:
- Diese Datei beschreibt das Verhalten des Aufwandsschätzers. Sie wird vom Agenten/Skill genutzt, um konsistente, nachvollziehbare Schätzungen zu erzeugen.

Verhalten:
- Verwende die konfigurierte SP-Skala (standard: Fibonacci).
- Analysiere Komplexität, Unsicherheit, Abhängigkeiten und vorhandene Infrastruktur.
- Gib eine begründete SP-Schätzung, Confidence-Level (hoch/mittel/niedrig), Risiken und Empfehlungen (Spike / Split).

Eingabeformat:
- JSON oder strukturierter Text mit Feldern: `title`, `acceptance_criteria`, `DoD`, `non_functional`.

Ausgabeformat:
- JSON mit Feldern: `sp`, `confidence`, `reasoning`, `risks`, `recommendation`.

Hinweis:
- Bei hoher Unsicherheit: empfehlen, einen Spike vorzuschlagen und die Confidence auf "niedrig" zu setzen.# Effort Estimator — Instructions

Zweck
- Detaillierte Anleitung für den Agenten `effort-estimator` zur konsistenten Aufwandsschätzung (SAFe, 1 SP = 1 Tag).

Eingabe (input schema)
- `title`: kurzer Task-/Feature-Titel (string)
- `description`: ausführliche Beschreibung / Akzeptanzkriterien (string)
- `estimated_loc` (optional): geschätzte LOC (number)
- `files_changed` (optional): grobe Liste von Dateien/Komponenten
- `flags` (optional): Objekt mit boolschen Indikatoren: `{new_tech, external_dependencies, unclear_requirements, complex_testing}`

Prozess
1. Scope the Work
   - Wenn `estimated_loc` vorhanden: nutze LOC zur Size-Mapping.
   - Sonst: schätze LOC aus Beschreibung und Typ (Bug/Feature/Refactor).
2. Identify Modifiers
   - Prüfe `flags` oder stelle Rückfragen, wenn unsicher.
3. Calculate Estimate
   - Base Duration = konstanter Basiswert aus Size-Table (siehe prompt)
   - Final = Base × (1 + sum of modifier percents)
4. Recommend Split
   - Wenn Size L oder XL: Vorschlag für sinnvolle Aufteilung.

Estimation Table (Kurzreferenz)
- XS: <30 LOC → 1 Stunde max
- S: 30–100 LOC → 0.5 Tag
- M: 100–200 LOC → 1 Tag
- L: 200–400 LOC → 2–3 Tage
- XL: >400 LOC → Aufteilen

Modifier (Multiplikatoren)
- New technology/pattern: +50% (0.5)
- External dependencies: +30% (0.3)
- Unclear requirements: +50% (0.5)
- Complex testing: +30% (0.3)

Ausgabeformat
- Standard: Markdown (siehe Output-Template)
- Optional: JSON (wenn `json_output: true` angefordert)

Output-Template (Markdown)
```
## Effort Estimate

**Size**: [XS/S/M/L/XL]
**Base Duration**: [time]
**Confidence**: [High/Medium/Low]

### Modifiers Applied
- [x] New technology (+50%)
- [ ] External dependencies (+30%)
- [x] Unclear requirements (+50%)
- [ ] Complex testing (+30%)

### Final Estimate
**[adjusted time]** (base × modifier)

### Recommendation
[Split recommendation if L/XL, or "Proceed" if S/M]
```

Rückfragen
- Der Agent darf Rückfragen stellen, wenn wesentliche Informationen fehlen (z. B. ob externe APIs involviert sind). Empfohlen: interaktiv.

Limits & Annahmen
- 1 SP = 1 Arbeitstag (Standardannahme)
- Zeitangaben sind netto-Implementierungszeit, keine Meetings/Reviews/Deployment-Puffer
