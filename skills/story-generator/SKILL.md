---
name: story-generator
description: Erzeugt SAFe-konforme User Stories auf Basis einer Requirement-Analyse oder Rohbeschreibung; liefert Varianten, Akzeptanzkriterien und DoD.
compatibility: opencode
---

Name: Story-Generator
Sprache: DE
Kurzbeschreibung: Erzeugt SAFe-konforme User Stories auf Basis einer Requirement-Analyse oder Rohbeschreibung. Liefert mehrere Varianten, Akzeptanzkriterien und eine Definition of Done.

Zweck:
- Generiert Story-Titel, Persona, Ziel, Akzeptanzkriterien, Definition of Done (DoD).
- Variantenbildung: konservativ / innovativ / risikoarm.

Technische Detaillierung:
- Für jede Story generiere zusätzlich technische Unteraufgaben (Implementation Tasks) wie API-Endpunkte, Datenmodell-Änderungen, Background-Jobs, Validierungslogik und Prüfaufwand.
- Erzeuge Testfälle pro Akzeptanzkriterium (Unit/Integration/E2E) und füge Hinweise zur Testautomatisierung hinzu.
- Kennzeichne Abhängigkeiten (externe Services, Datenmigrationen, Auth/Authorisierung).

Eingabe:
- Kurzbeschreibung / Analysis-Output (z.B. aus `anforderungs-analyse`).
- Optional: gewünschte Variante (konservativ/innovativ/risikoarm).

Ausgabe pro Story-Variante:
- Titel
- Persona
- Nutzerstory (Als <Persona> möchte ich <Funktion>, damit <Nutzen>)
- Akzeptanzkriterien (nummeriert, klar testbar, Gherkin-ähnlich)
- Definition of Done (Checkliste)
- Vorschlag für Priorität (MoSCoW) und grobe Abhängigkeiten
 - Technische Unteraufgaben (Liste)
 - Testfälle / Test-Typen
 - Abhängigkeiten / Integrationspunkte

Beispiel-Template (Output):
1) Titel: "PDF-Upload für Kundenbestellungen"
2) Persona: "Sachbearbeiter"
3) Story: "Als Sachbearbeiter möchte ich PDFs mit Bestellungen hochladen, damit Bestellungen automatisch im System angelegt werden."
4) Akzeptanzkriterien:
   - GIVEN ein korrekt formatiertes PDF, WHEN hochgeladen, THEN wird die Bestellung angelegt.
   - GIVEN ein fehlerhaftes PDF, WHEN hochgeladen, THEN wird eine Fehlermeldung mit Korrekturanweisungen angezeigt.
5) DoD:
   - Unit-Tests vorhanden
   - End-to-End Upload getestet
   - Dokumentation aktualisiert

Technische-DoD (Beispiel):
- API-Spezifikation aktualisiert (OpenAPI)
- DB-Migration mit Rollback-Plan
- Performance-Tests vorhanden
- Monitoring/Alerts konfiguriert

Konfiguration:
- `variants`: Anzahl Varianten (default: 3)
- `useSAFeFormat`: true/false (default true) — wenn true, Story-Felder nach SAFe-Struktur anreichern.

Regeln (SAFe):
- Stories sollen Business Value klar darstellen.
- Acceptance Criteria sollen testbar & atomar sein.
- Bei größeren Umfang prüfen: in Feature/Epic hochstufen.