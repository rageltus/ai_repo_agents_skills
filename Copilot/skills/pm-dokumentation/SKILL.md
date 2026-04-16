---
name: pm-dokumentation
description: Erzeugt Produkt- und Feature-Dokumentation, Release Notes und PI-Plan-Notizen basierend auf Requirement-Analyse und Story-Outputs.
---

Name: PM-Dokumentation
Sprache: DE
Kurzbeschreibung: Erzeugt Produkt- und Feature-Dokumentation, Release Notes und PI-Plan-Notizen basierend auf Requirement-Analyse und Story-Outputs.

Funktionen:
- Erstelle eine prägnante Feature-Beschreibung für Stakeholder.
- Generiere Release Notes mit Auswirkungen und Rollout-Hinweisen.
- Erstelle PI-Notes (Ziele, Metriken, Risiken) für PI-Planning.

Technische Dokumentations-Erweiterung:
- Erzeuge eine technische Design-Übersicht: Architekturdiagramm-Beschreibung, benötigte Services, Datenfluss, Schnittstellen.
- Dokumentiere DB-Änderungen, Migrationsschritte und Backout-Strategien.
- Liste erforderliche Tests und Monitoring/Logging-Anforderungen auf.

Eingabe:
- Zusammenfassung des Features (Aus `anforderungs-analyse` oder Freitext)
- Optionale Liste der Stories

Ausgabe:
- Feature-Overview (Zweck, Zielgruppe, KPIs)
- Release Notes (Was ist neu, Auswirkungen, Migrationshinweise)
- PI-Plan-Notiz (Business Outcome, Metrik, Risiken, Dependencies)
- Exportierbares JSON/Markdown

Technische Ausgabe:
- Technical Overview (Architektur-Highlights, Komponenten)
- Migrationsplan (Schritte, Datenbackups, Rollback)
- Testmatrix (Unit/Integration/E2E/Performance) und vorgeschlagene Metriken
- Monitoring/Alerting-Vorschläge

Beispiel (Output):
- Feature-Overview: "PDF-Upload ermöglicht Sachbearbeitern, Bestellungen effizienter aufzunehmen..."
- Release Notes: Kurzbeschreibung, Aktivierungsschritte, bekannte Einschränkungen
- PI-Notes: Ziel (z.B. "Reduktion manueller Erfassungszeit um 30%"), Metrik-Vorschlag, Risiken

Konfiguration:
- `includeKPIs`: true/false
- `exportFormat`: markdown|json (default: markdown)

Hinweis: Dieses SKILL legt Wert auf klare Kommunikation für Business-Stakeholder und PO-Reports.