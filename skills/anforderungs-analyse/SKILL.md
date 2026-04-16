---
name: anforderungs-analyse
description: Analysiert ein neues Requirement; extrahiert Nutzerbedürfnisse, Entitäten, nicht-funktionale Anforderungen und generiert initiale Backlog-Items und Fragen für Brainstorming.
compatibility: opencode
---

Name: Anforderungs-Analyse
Sprache: DE
Kurzbeschreibung: Analysiert ein neues Requirement, extrahiert Nutzerbedürfnisse, Entitäten, nicht-funktionale Anforderungen und generiert initiale Backlog-Items und Fragen für das Brainstorming.

Zweck:
- Strukturiert eingehende Anforderungen.
- Liefert Vorschläge für Personas, Goals, Akzeptanzkriterien und erste Tasks.
- Markiert offene Fragen und Risiken.

Erweiterte technische Analyse:
- Identifiziere notwendige Integrationen (APIs, Datenquellen, Drittanbieter).
- Schätze Auswirkungen auf Datenmodell (neue Entities/Attribute, Migrationen).
- Frage nach nicht-funktionalen Anforderungen: Performance (Latenz/TP), Skalierung, Sicherheit, Datenschutz, SLAs.
- Identifiziere Testanforderungen: Unit, Integration, E2E, Performance-Tests.
- Liefere eine initiale technische Aufgabenliste (z.B. API-Änderung, DB-Migration, Parsing-Service, Monitoring).

Eingabe:
- Freitext-Beschreibung des Requirements (Deutsch).

Ausgabe:
- Kurz-Zusammenfassung (1-2 Sätze).
- Liste erkannter Entitäten / Datenfelder.
- Vorschläge für Personas (wenn nicht angegeben).
- Initiale Akzeptanzkriterien in Gherkin-ähnlichem Stil.
- Vorschläge für 3 initiale Backlog-Items (EPIC/Feature/Story-Zuordnung).
- Offene Fragen (für das Brainstorming).

Beispiel (Input):
"Kunden sollen Bestellungen per PDF-Upload hinzufügen und diese automatisch den richtigen Produkten zuordnen können."

Beispiel (Output):
- Zusammenfassung: "PDF-Upload für Bestellungen; automatische Produktzuordnung via Parsing/Matching."
- Entitäten: Bestellung, Kunde, Produkt, PDF-Datei, Bestellpositionen
- Personas: Sachbearbeiter, Kunde
- Akzeptanzkriterien:
  - GIVEN ein gültiges PDF mit Bestelldaten, WHEN der Nutzer uploadet, THEN werden Bestellungen im System angelegt.
- Backlog-Items:
  1) Feature: PDF-Upload UI + Validierung
  2) Story: Parsing-Service für Bestellpositionen
  3) Story: Matching-Algorithmus Produktzuordnung
- Offene Fragen: Unterstützte PDF-Formate? Wie mit fehlerhaften PDFs umgehen?

Konfiguration:
- `storyDepth`: Anzahl generierter Story-Varianten (default: 3)
- `includeNonFunctional`: true/false (default true)

Beispiel erweitertes Output (technisch):
- Technische Tasks:
  - Implementiere Parsing-Service (Microservice) mit Schnittstelle `/parse`.
  - Erweiterung DB-Tabelle `orders` um Feld `sourceDocumentId`.
  - Hintergrundjob zur Zuordnung (`matching-service`).
  - Monitoring: Erstelle Alerts bei Parsing-Fehlern > 5%.
- Nicht-funktional: Support für bis zu 1000 Uploads/Tag, 95% der Uploads innerhalb 5s verarbeiten.

Hinweis: Die technischen Vorschläge dienen der Verfeinerung von Story-Points und können vom Team validiert/angepasst werden.

Hinweise für das Agenten-Calling:
- Wenn Unsicherheiten erkannt werden, setze `requiresBrainstorm=true` in der Response, damit der Agent zusätzliche Fragen stellt.