Name: Technische-Analyse
Sprache: DE
Kurzbeschreibung: Führt eine vertiefte technische Analyse eines Requirements durch, inkl. Architektur-Überlegungen, Integrationen, Datenmodell-Auswirkungen, Test- und DevOps-Aufwand.

Zweck:
- Analysiert technische Implikationen eines Features.
- Liefert eine Liste technischer Tasks, geschätzten Aufwandsanteile und Risiken.
- Liefert Empfehlungen für Architektur-Alternativen (z.B. Microservice vs. Monolith, Sync vs. Async).

Eingabe:
- Requirement-Beschreibung und ggf. Systemkontext (bestehende Architektur, bekannte Services).

Ausgabe:
- Architektur-Übersicht (Kurzbeschreibung, Komponenten, Schnittstellen)
- Integrationspunkte (APIs, Auth, 3rd-Party)
- Datenmodell-Änderungen (Tabellen, Felder, Migrationsempfehlungen)
- Nicht-funktionale Anforderungen (Performance, Skalierung, Sicherheit) mit konkreten Zahlen/SLAs falls möglich
- Technische Tasks (Aufgabenliste mit geschätzten Anteilen)
- Risiken & Unsicherheiten (mit Vorschlag für Spikes)

Beispiel (Output):
- Architektur: "Neuer Parsing-Service (stateless) als Microservice, Queue-basiertes Processing für hohe Belastung."
- Integrationen: "Externer S3-Storage für PDFs, Auth via OAuth2, Produktkatalog-API für Matching." 
- Datenmodell: "orders: + sourceDocumentId (GUID), documents: id, rawContent, parsed=false"
- Tasks: Parsing-Service (5d), Matching-Algorithmus (3d), DB-Migration (1d), Monitoring (1d)

Hinweis: Dieses SKILL liefert präzise technische Inputs, die `aufwandsschaetzer` und `story-generator` für genauere Schätzungen und Stories verwenden sollten.