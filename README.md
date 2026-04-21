# AI Repository — Agents, Skills & Prompts

Eine kuratierte Sammlung von **Copilot und OpenCode Agenten**, **Skills** und **Prompt-Dateien** für produktive KI-Workflows in Softwareentwicklung, SAP, Kreativarbeit und mehr. Alle Prompts und Agenten sind sorgfältig formuliert, um klare Anweisungen, strukturierte Antworten und praktische Anwendbarkeit zu gewährleisten. Dieses Repository dient als umfassende Ressource für Entwickler, Product Owner, Kreative und alle, die KI effektiv nutzen möchten. Alle Prompts sind unabhängig des KI Tools verwendbar, können aber auch als Vorlage für die Erstellung von Copilot- oder OpenCode-Agenten dienen. Oder einfach als Prompt in eim beliebigen KI-Tool.

---

## Inhaltsverzeichnis

- [Repository-Struktur](#repository-struktur)
- [Agents](#agents)
  - [SAP & Fiori Agenten](#-sap--fiori-agenten)
  - [Product Management & Agile](#-product-management--agile)
  - [Technologie & Entwicklung](#-technologie--entwicklung-agenten)
  - [Meta & Tools](#-meta--tools)
- [Skills](#skills)
  - [SAP & Fiori Skills](#-sap--fiori-skills)
  - [Product Management & Agile Skills](#-product-management--agile-skills)
  - [Technologie & Analyse](#-technologie--analyse)
- [Prompts](#prompts)
  - [SAP & Fiori](#-sap--fiori)
  - [Technologie & Entwicklung](#-technologie--entwicklung)
  - [Kreatives Schreiben & Medien](#-kreatives-schreiben--medien)
  - [Bildung & Lernen](#-bildung--lernen)
  - [Business & Karriere](#-business--karriere)
  - [Gesundheit, Wellness & Lifestyle](#-gesundheit-wellness--lifestyle)
  - [Beratung & Coaching](#-beratung--coaching)
  - [Sprache & Übersetzung](#-sprache--übersetzung)
  - [Spiele & Unterhaltung](#-spiele--unterhaltung)
  - [Rollenspiel & Kreativ-Simulationen](#-rollenspiel--kreativ-simulationen)
  - [Tools & Generatoren](#-tools--generatoren)

---

## Repository-Struktur

```
ai_repo_agents_skills/
├── agents/          # VS Code Copilot Agent-Definitionen (.md)
├── skills/          # Wiederverwendbare Skills für Agenten (SKILL.md)
└── Prompts/         # Individuelle Prompt-Dateien (.prompt)
```

---

## Agents

Agenten sind vollständig konfigurierte KI-Profile mit definierten Rollen, Werkzeugrechten und Spezialverhalten. Sie werden als `.md`-Dateien im `agents/`-Ordner gespeichert.

### 🔷 SAP & Fiori Agenten

| Datei | Name | Modus | Beschreibung |
|-------|------|-------|--------------|
| [fiori-senior-onprem-mock.md](agents/fiori-senior-onprem-mock.md) | **Fiori Senior OnPrem Mock** | primary | Senior SAP Fiori Elements Developer für SAP On-Premise-Services und lokale Mock-Data-Projekte. Spezialisiert auf Annotation-First-Entwicklung und Mock-Data-Workflows. |
| [sap-berater.md](agents/sap-berater.md) | **SAP Berater** | primary | Senior SAP Berater (fachlich) für S/4HANA On-Premise. Erklärt Transaktionen, IMG-Customizing und Geschäftsprozesse. Ausschließlich fachlich auf Deutsch — kein ABAP-Code. |
| [sap-qm-berater.md](agents/sap-qm-berater.md) | **SAP QM Berater** | primary | Senior SAP QM Berater (fachlich) für S/4HANA. Spezialist für Prüfplanung, Prüfabwicklung, Qualitätsmeldungen, Zeugnisse, Stabilitätsstudien und QM-Customizing. Compliance-Expertise (ISO 9001, GMP, GxP). |
| [sap-qm-developer.md](agents/sap-qm-developer.md) | **SAP QM Developer** | primary | Senior SAP QM Entwickler & Architekt für S/4HANA On-Premise. Technische Implementierung von QM-Erweiterungen: ABAP (OO/prozedural), BAPIs, BAdIs, Enhancement-Spots, CDS-Views, OData-Services, Fiori-Erweiterungen. |

### 📋 Product Management & Agile

| Datei | Name | Modus | Beschreibung |
|-------|------|-------|--------------|
| [effort-estimator.md](agents/effort-estimator.md) | **Effort Estimator** | subagent | Schätzt Entwicklungsaufwand für Tasks/Features nach dem Scale Agile Framework (SAFe). 1 SP = 1 Tag. Liefert reproduzierbare Schätzungen mit Begründung, Confidence-Score und Empfehlungen. |
| [product-manager.md](agents/product-manager.md) | **Product Manager** | primary | Zentraler Agent für Product Owner / PO-Aufgaben. Orchestriert Skills für Anforderungsanalyse, Story-Generierung, Aufwandsschätzung und Dokumentation. Einstiegspunkt für alle PO/PM-Workflows auf Deutsch. |
| [requirements-analyzer.md](agents/requirements-analyzer.md) | **Requirements Analyzer** | subagent | Analysiert Feature-Anforderungen auf Klarheit, Umfang, technische Machbarkeit und Risiko. Ideal beim Start neuer Features, beim Review von Specs oder beim Aufbrechen von Tasks. |

### 💻 Technologie & Entwicklung Agenten

| Datei | Name | Modus | Beschreibung |
|-------|------|-------|--------------|
| [python-coach.md](agents/python-coach.md) | **Python Coach** | primary | Interaktiver Python-Lerncoach auf Basis des freeCodeCamp Python-Tutorials. Unterstützt Übungsreviews, Code-Feedback, Tooling-Setup, Testanleitungen und AI/Python-Workflows. |

### ⚙️ Meta & Tools

| Datei | Name | Modus | Beschreibung |
|-------|------|-------|--------------|
| [agent-generator.md](agents/agent-generator.md) | **Agent Generator** | primary | Meta-Agent zum Erstellen vollständiger AI-Agenten-Dateien. Unterstützt Plain-, Copilot- und OpenCode-Formate. Führt strukturiertes Interview, wählt Format und erstellt alle Artefakte inkl. Sicherheits- und Validierungs-Checkliste. |

---

## Skills

Skills sind modulare Wissens- und Verhaltensblöcke, die von Agenten (oder direkt) aufgerufen werden können. Sie liegen als `SKILL.md` in den jeweiligen Unterordnern.

### 🔷 SAP & Fiori Skills

| Ordner | Name | Sprache | Beschreibung |
|--------|------|---------|--------------|
| [fiorielementscheatsheet/](skills/fiorielementscheatsheet/SKILL.md) | **Fiori Elements Cheatsheet** | DE/EN | Kurzreferenz für SAP Fiori Elements XML-Annotationen (`annotation.xml`) und ihre ABAP CDS-Äquivalente. Ideal zur schnellen Nachschlagung von Annotationssyntax. |

### 📋 Product Management & Agile Skills

| Ordner | Name | Sprache | Beschreibung |
|--------|------|---------|--------------|
| [anforderungs-analyse/](skills/anforderungs-analyse/SKILL.md) | **Anforderungs-Analyse** | DE | Analysiert ein neues Requirement: extrahiert Nutzerbedürfnisse, Entitäten, nicht-funktionale Anforderungen und generiert initiale Backlog-Items sowie offene Fragen für das Brainstorming. |
| [aufwandsschaetzer/](skills/aufwandsschaetzer/SKILL.md) | **Aufwandsschätzer** | DE | Schätzt Story Points (SP) nach SAFe-Grundsätzen (Fibonacci-Skala). Liefert begründete Schätzung, Confidence-Level (hoch/mittel/niedrig) und Empfehlungen (z. B. Spike, Breakdown). |
| [pm-dokumentation/](skills/pm-dokumentation/SKILL.md) | **PM-Dokumentation** | DE | Erzeugt Produkt- und Feature-Dokumentation, Release Notes und PI-Plan-Notizen auf Basis von Requirement-Analyse- und Story-Outputs. |
| [story-generator/](skills/story-generator/SKILL.md) | **Story Generator** | DE | Erzeugt SAFe-konforme User Stories auf Basis einer Requirement-Analyse oder Rohbeschreibung. Liefert Varianten (konservativ/innovativ/risikoarm), Akzeptanzkriterien und Definition of Done (DoD). |
| [user-story-fundamentals/](skills/user-story-fundamentals/SKILL.md) | **User Story Fundamentals** | EN | Strukturiertes Framework zur Erfassung von Produktanforderungen aus Nutzersicht. Unterstützt Backlog-Pflege, Akzeptanzkriterien, Feature-Priorisierung und Team-Kommunikation. |

### 💻 Technologie & Analyse

| Ordner | Name | Sprache | Beschreibung |
|--------|------|---------|--------------|
| [technische-analyse/](skills/technische-analyse/SKILL.md) | **Technische Analyse** | DE | Führt eine vertiefte technische Analyse eines Requirements durch: Architektur-Überlegungen, Integrationspunkte, Datenmodell-Auswirkungen, Aufwandsanteile (FE/BE/Test/DevOps) und Risiken. |

---

## Prompts

Die `.prompt`-Dateien im Ordner `Prompts/` bieten sofort einsatzfähige Rollen- und Verhaltensprofile. Jede Datei beschreibt eine Rolle mit Verhalten, Ausgabeformat und Nutzungshinweisen.

---

### 🔷 SAP & Fiori

| Datei | Rolle | Beschreibung |
|-------|-------|--------------|
| [SAFe_Framework_Consultant.prompt](Prompts/SAFe_Framework_Consultant.prompt) | SAFe Framework Consultant | Interaktiver SAFe-Berater mit Hauptmenü: SAFe-Fragen, Feature-Erstellung und User-Story-Erstellung nach SAFe-Standard in Tabellenform. |
| [SAP_Berater.prompt](Prompts/SAP_Berater.prompt) | Senior SAP Berater | Fachlicher S/4HANA Berater für Transaktionen, Prozesse und Customizing — auf Deutsch. |
| [SAP_QM_Berater.prompt](Prompts/SAP_QM_Berater.prompt) | Senior SAP QM Berater | Spezialist für SAP QM Submodule, Prüfplanung, Qualitätsmeldungen und Compliance (ISO 9001, GMP). |
| [SAP_QM_Developer.prompt](Prompts/SAP_QM_Developer.prompt) | Senior SAP QM Developer | Technische Implementierung von QM-Erweiterungen: ABAP, BAdIs, CDS-Views, OData, Fiori. |

---

### 💻 Technologie & Entwicklung

| Datei | Rolle | Beschreibung |
|-------|-------|--------------|
| [Algorithm_Instructor.prompt](Prompts/Algorithm_Instructor.prompt) | Algorithmus-Lehrer | Erklärt Algorithmen und Datenstrukturen mit Pseudocode, Visualisierungen und Komplexitätsanalyse. |
| [API_Designer.prompt](Prompts/API_Designer.prompt) | API Designer | Entwirft REST/GraphQL-APIs mit Endpunkten, Schemas, Statuscodes und Sicherheitskonventionen. |
| [Ascii_Artist.prompt](Prompts/Ascii_Artist.prompt) | ASCII-Künstler | Erstellt kreative ASCII-Art-Grafiken aus Textbeschreibungen. |
| [Code_Reviewer.prompt](Prompts/Code_Reviewer.prompt) | Code Reviewer | Reviewed Pull Requests systematisch nach Stil, Logik, Security und Performance — mit Schweregrad und Empfehlung. |
| [Commit_Message_Generator.prompt](Prompts/Commit_Message_Generator.prompt) | Commit Message Generator | Generiert präzise, strukturierte Git-Commit-Messages nach Conventional Commits. |
| [Cyber_Security_Specialist.prompt](Prompts/Cyber_Security_Specialist.prompt) | Cybersecurity-Spezialist | Analysiert Sicherheitslücken, führt Threat Modeling durch und empfiehlt Maßnahmen nach OWASP. |
| [Database_Architect.prompt](Prompts/Database_Architect.prompt) | Database Architect | Entwirft Datenbankmodelle, Normalisierung, Indexierungsstrategien und DDL für SQL/NoSQL. |
| [Developer_Relations_Consultant.prompt](Prompts/Developer_Relations_Consultant.prompt) | Developer Relations Consultant | Berät zu Developer-Experience, API-Evangelism, Community-Aufbau und DevRel-Strategien. |
| [DevOps_Engineer.prompt](Prompts/DevOps_Engineer.prompt) | DevOps Engineer | CI/CD-Pipelines, Docker, Kubernetes, Terraform und Deployment-Strategien mit kommentierten Konfigurationsbeispielen. |
| [Domain_Name_Generator.prompt](Prompts/Domain_Name_Generator.prompt) | Domain-Namen-Generator | Generiert kreative, verfügbarkeitsoptimierte Domain-Namen für Projekte und Startups. |
| [Excel_Sheet.prompt](Prompts/Excel_Sheet.prompt) | Excel-Tabellenblatt | Simuliert ein textbasiertes Excel-Tabellenblatt mit Formeln und Berechnungen. |
| [Fullstack_Developer.prompt](Prompts/Fullstack_Developer.prompt) | Fullstack Developer | Entwickelt vollständige Web-Anwendungen (Frontend + Backend) mit modernen Technologien. |
| [IT_Architect.prompt](Prompts/IT_Architect.prompt) | IT-Architekt | Entwirft skalierbare IT-Architekturen, bewertet Technologieentscheidungen und dokumentiert Systeme. |
| [IT_Expert.prompt](Prompts/IT_Expert.prompt) | IT-Experte | Allgemeiner IT-Support und Problemlösung für Hardware, Software und Netzwerke. |
| [JavaScript_Console.prompt](Prompts/JavaScript_Console.prompt) | JavaScript Console | Emuliert eine JavaScript-Browser-Konsole für direktes Code-Testing und Debugging. |
| [Linux_Terminal.prompt](Prompts/Linux_Terminal.prompt) | Linux Terminal | Simuliert ein vollständiges Linux-Terminal mit Bash-Befehlsunterstützung. |
| [Machine_Learning_Engineer.prompt](Prompts/Machine_Learning_Engineer.prompt) | Machine Learning Engineer | Entwickelt und bewertet ML-Modelle, erklärt Algorithmen und optimiert Pipelines. |
| [PHP_Interpreter.prompt](Prompts/PHP_Interpreter.prompt) | PHP-Interpreter | Emuliert einen PHP-Interpreter und gibt Ausgaben von PHP-Code-Snippets zurück. |
| [Python_Interpreter.prompt](Prompts/Python_Interpreter.prompt) | Python Interpreter | Emuliert einen Python-Interpreter für direktes Code-Testing ohne Laufzeitumgebung. |
| [Regex_Generator.prompt](Prompts/Regex_Generator.prompt) | Regex Generator | Erstellt präzise reguläre Ausdrücke mit Erklärungen und Testbeispielen. |
| [R_Interpreter.prompt](Prompts/R_Interpreter.prompt) | R-Interpreter | Emuliert einen R-Interpreter für statistische Berechnungen und Datenanalyse. |
| [Scientific_Data_Visualizer.prompt](Prompts/Scientific_Data_Visualizer.prompt) | Scientific Data Visualizer | Erstellt Datenvisualisierungen (Python/R/Matplotlib) und erklärt statistische Zusammenhänge. |
| [Senior_Frontend_Developer.prompt](Prompts/Senior_Frontend_Developer.prompt) | Senior Frontend Developer | Entwickelt React-Anwendungen mit Redux, TypeScript und Ant Design nach Best Practices. |
| [Software_QA_Tester.prompt](Prompts/Software_QA_Tester.prompt) | Software QA Tester | Erstellt Testpläne, Testfälle und Bug-Reports für strukturierte Qualitätssicherung. |
| [Solr_Search_Engine.prompt](Prompts/Solr_Search_Engine.prompt) | Solr Search Engine | Emuliert Apache Solr REST-API-Antworten für Suchanfragen mit Facetting und Filterung. |
| [SQL_Terminal.prompt](Prompts/SQL_Terminal.prompt) | SQL Terminal | Simuliert ein SQL-Terminal-Interface für Datenbankabfragen und Schema-Design. |
| [SVG_Designer.prompt](Prompts/SVG_Designer.prompt) | SVG-Designer | Erstellt skalierbare Vektorgrafiken (SVG) aus Beschreibungen — direkt als Code. |
| [Tech_Interview_Coach.prompt](Prompts/Tech_Interview_Coach.prompt) | Tech Interview Coach | Bereitet auf Coding-Interviews vor (Leetcode-Stil): Aufgaben, Feedback zu Big-O, Edge Cases und System Design. |
| [UX_UI_Developer.prompt](Prompts/UX_UI_Developer.prompt) | UX/UI Designer & Developer | Entwirft nutzerzentrierte UI-Konzepte, Wireframes (text) und Design-System-Empfehlungen. |
| [Web_Browser.prompt](Prompts/Web_Browser.prompt) | Webbrowser-Emulator | Simuliert einen textbasierten Webbrowser mit URL-Navigation und Seitendarstellung. |
| [Web_Design_Consultant.prompt](Prompts/Web_Design_Consultant.prompt) | Webdesign-Berater | Bewertet Webseiten-Designs und gibt fundierte UX/UI-Verbesserungsempfehlungen. |

---

### ✍️ Kreatives Schreiben & Medien

| Datei | Rolle | Beschreibung |
|-------|-------|--------------|
| [Aphorism_Book.prompt](Prompts/Aphorism_Book.prompt) | Aphorismenbuch | Generiert tiefgründige Aphorismen und Lebensweisheiten zu vorgegebenen Themen. |
| [Brainstorming.prompt](Prompts/Brainstorming.prompt) | Innovations-Berater | Kreativer Ideengeber und Moderator für Brainstorming-Sitzungen und Innovationsworkshops. |
| [Character_Roleplay.prompt](Prompts/Character_Roleplay.prompt) | Charakter-Roleplay | Spielte Figuren aus Film, Buch oder Serie authentisch nach deren Persönlichkeit. |
| [Classical_Music_Composer.prompt](Prompts/Classical_Music_Composer.prompt) | Klassischer Komponist | Komponiert klassische Musikstücke (Notation, Struktur, Stil) und erklärt Kompositionstechniken. |
| [Commentariat.prompt](Prompts/Commentariat.prompt) | Meinungskolumnist | Verfasst meinungsstarke, argumentativ fundierte Kolumnen zu aktuellen Themen. |
| [Composer.prompt](Prompts/Composer.prompt) | Musikkomponist | Erstellt Songstrukturen, Liedtexte und Arrangements für verschiedene Musikgenres. |
| [Essay_Writer.prompt](Prompts/Essay_Writer.prompt) | Essay Writer | Schreibt überzeugende, strukturierte Essays mit These, Argumentation und Schluss. |
| [Film_Critic.prompt](Prompts/Film_Critic.prompt) | Filmkritiker | Kritische Filmanalyse mit Fokus auf Regie, Dramaturgie, Kamera und kulturellen Kontext. |
| [Movie_Critic.prompt](Prompts/Movie_Critic.prompt) | Movie Critic | Bewertet Filme mit Handlungsanalyse, Stärken/Schwächen und vergleichbaren Werken. |
| [Journalist.prompt](Prompts/Journalist.prompt) | Journalist | Recherchiert und verfasst Nachrichtenartikel nach journalistischen Standards (W-Fragen, Objektivität). |
| [Novelist.prompt](Prompts/Novelist.prompt) | Romanautor | Entwickelt Romankonzepte, Charaktere, Plotstrukturen und schreibt Prosaabschnitte. |
| [Poet.prompt](Prompts/Poet.prompt) | Dichter & Poet | Verfasst Gedichte in verschiedenen Formen (Sonett, Haiku, freie Verse) zu beliebigen Themen. |
| [Rapper.prompt](Prompts/Rapper.prompt) | Rapper & Lyriker | Schreibt Rap-Texte mit Reimschemas, Flow-Strukturen und Genre-spezifischen Stilmitteln. |
| [Screenwriter.prompt](Prompts/Screenwriter.prompt) | Drehbuchautor | Schreibt Drehbücher nach Branchenstandards (Szenenköpfe, Dialoge, Handlungsbeschreibungen). |
| [Self_Help_Book.prompt](Prompts/Self_Help_Book.prompt) | Selbsthilfebuchautor | Verfasst motivierende Ratgeberinhalte mit Übungen, Reflexionsfragen und Aktionsplänen. |
| [Storyteller.prompt](Prompts/Storyteller.prompt) | Geschichtenerzähler | Erstellt immersive, strukturierte Geschichten mit Charakterentwicklung und Spannungsbogen. |

---

### 🎓 Bildung & Lernen

| Datei | Rolle | Beschreibung |
|-------|-------|--------------|
| [Academician.prompt](Prompts/Academician.prompt) | Akademiker & Wissenschaftlicher Autor | Schreibt wissenschaftliche Texte, Abstracts und erklärt Forschungsmethoden. |
| [AI_Writing_Tutor.prompt](Prompts/AI_Writing_Tutor.prompt) | KI-Schreibtutor | Gibt strukturiertes Feedback zu Texten und begleitet den Schreibprozess. |
| [Debate_Coach.prompt](Prompts/Debate_Coach.prompt) | Debate Coach | Trainiert Debattier- und Rhetoriktechniken mit Strukturierungsübungen und Feedback. |
| [Educational_Content_Creator.prompt](Prompts/Educational_Content_Creator.prompt) | Educational Content Creator | Entwickelt Lernmaterialien, Lektionspläne und erklärende Inhalte für verschiedene Niveaus. |
| [Elocutionist.prompt](Prompts/Elocutionist.prompt) | Rhetoriker | Verbessert Aussprache, Stimmführung und Redekunst für professionelles Auftreten. |
| [Etymologist.prompt](Prompts/Etymologist.prompt) | Etymologe | Erklärt Wortherkunft, sprachgeschichtliche Entwicklung und linguistische Zusammenhänge. |
| [Fallacy_Finder.prompt](Prompts/Fallacy_Finder.prompt) | Fallacy Finder | Identifiziert logische Fehlschlüsse in Argumenten und erklärt ihre kognitive Verzerrung. |
| [Fill_in_the_Blank_Worksheets.prompt](Prompts/Fill_in_the_Blank_Worksheets.prompt) | Lückentext-Generator | Erstellt ESL-Lückentext-Arbeitsblätter mit Word Bank und Antwortschlüssel (A1–C2). |
| [Flashcard_Generator.prompt](Prompts/Flashcard_Generator.prompt) | Flashcard Generator | Erstellt Lernkarten (Anki-kompatibel) mit Active-Recall-Prinzipien aus Texten und Notizen. |
| [Historian.prompt](Prompts/Historian.prompt) | Historiker | Erklärt historische Ereignisse, Epochen und Zusammenhänge mit wissenschaftlicher Fundierung. |
| [Japanese_Kanji_Quiz.prompt](Prompts/Japanese_Kanji_Quiz.prompt) | Kanji-Quiz | Interaktive Kanji-Lerneinheit mit Vokabelkarten, Quizfragen und Strichfolge-Erklärungen. |
| [Math_History_Teacher.prompt](Prompts/Math_History_Teacher.prompt) | Mathematikhistoriker | Erklärt die Geschichte der Mathematik, bedeutende Mathematiker und mathematische Epochen. |
| [Math_Teacher.prompt](Prompts/Math_Teacher.prompt) | Mathelehrer | Erklärt mathematische Konzepte Schritt für Schritt mit Beispielen und Übungsaufgaben. |
| [Mathematician.prompt](Prompts/Mathematician.prompt) | Mathematiker | Löst komplexe Mathematikaufgaben, führt Beweise und erklärt Formeln präzise. |
| [Memory_Palace_Coach.prompt](Prompts/Memory_Palace_Coach.prompt) | Memory Palace Coach | Erklärt und trainiert Gedächtnistechniken (Loci-Methode, Major-System, Geschichtenverknüpfung). |
| [Philosophy_Teacher.prompt](Prompts/Philosophy_Teacher.prompt) | Philosophielehrer | Führt in philosophische Konzepte und bedeutende Denker ein; zugänglich erklärt. |
| [Public_Speaking_Coach.prompt](Prompts/Public_Speaking_Coach.prompt) | Redner-Coach | Trainiert öffentliches Reden: Struktur, Körpersprache, Stimme und Nervosität überwinden. |
| [Socrat.prompt](Prompts/Socrat.prompt) | Sokrates | Führt Gespräche nach der maieutischen Methode — stellt Fragen statt Antworten zu geben. |
| [Socratic_Method.prompt](Prompts/Socratic_Method.prompt) | Sokratische Methode | Analysiert Überzeugungen philosophisch durch systematisches Hinterfragen und Prüfen. |
| [Socratic_Tutor.prompt](Prompts/Socratic_Tutor.prompt) | Socratic Tutor | Fachspezifisches sokratisches Lernen (Mathematik, Physik, Informatik u. a.) durch gezielte Gegenfragen. |
| [Spoken_English_Teacher.prompt](Prompts/Spoken_English_Teacher.prompt) | Spoken English Teacher | Konversationsbegleitung und Feedback für englische Alltagssprache, Aussprache und Idiome. |

---

### 💼 Business & Karriere

| Datei | Rolle | Beschreibung |
|-------|-------|--------------|
| [Accountant.prompt](Prompts/Accountant.prompt) | Buchhalter & Finanzberater | Erklärt Buchhaltungsprinzipien, Bilanzierung und steuerliche Fragestellungen. |
| [Advertiser.prompt](Prompts/Advertiser.prompt) | Werbestratege | Entwickelt kreative Werbekampagnen mit Botschaft, Zielgruppe und Kanalstrategie. |
| [Career_Counselor.prompt](Prompts/Career_Counselor.prompt) | Karriereberater | Begleitet Karriereentscheidungen mit Stärkenanalyse, Berufsfelderkundung und Aktionsplan. |
| [CEO.prompt](Prompts/CEO.prompt) | CEO | Strategische Unternehmensführung: Vision, OKRs, Stakeholder-Management und Wachstumsstrategie. |
| [Change_Manager.prompt](Prompts/Change_Manager.prompt) | Change Manager | Begleitet Veränderungsprozesse nach Kotter/ADKAR mit phasenbasiertem Plan, Stakeholder-Map und KPIs. |
| [Cover_Letter.prompt](Prompts/Cover_Letter.prompt) | Anschreiben-Generator | Erstellt individualisierte Bewerbungsanschreiben, abgestimmt auf Stelle und Unternehmen. |
| [Debater.prompt](Prompts/Debater.prompt) | Professioneller Debatter | Argumentiert überzeugend für oder gegen Positionen mit strukturierter Rhetorik. |
| [Email_Professional.prompt](Prompts/Email_Professional.prompt) | Email Professional | Formuliert professionelle E-Mails aus Stichpunkten — für jeden Anlass, Ton und Empfänger. |
| [Financial_Analyst.prompt](Prompts/Financial_Analyst.prompt) | Finanzanalyst | Analysiert Finanzdaten, bewertet Investitionen und erstellt Prognosemodelle. |
| [Grant_Writer.prompt](Prompts/Grant_Writer.prompt) | Grant Writer | Formuliert strukturierte Förderanträge für NGOs, Forschung und Startups mit Theory of Change und Budget. |
| [Investment_Manager.prompt](Prompts/Investment_Manager.prompt) | Investment Manager | Portfolio-Management, Risikobewertung und Anlageempfehlungen nach modernen Portfoliotheorien. |
| [Journal_Reviewer.prompt](Prompts/Journal_Reviewer.prompt) | Journal-Gutachter | Begutachtet wissenschaftliche Artikel nach Peer-Review-Standards mit strukturiertem Feedback. |
| [Logistician.prompt](Prompts/Logistician.prompt) | Logistikplaner | Optimiert Lieferketten, Routen und Lagerlogistik mit operativer Fokussierung. |
| [Meeting_Facilitator.prompt](Prompts/Meeting_Facilitator.prompt) | Meeting Facilitator | Agendaplanung, Zeitmanagement, Moderationstipps und Protokollvorlage für jedes Meeting-Format. |
| [Meeting_Notes_Formatter.prompt](Prompts/Meeting_Notes_Formatter.prompt) | Meeting Notes Formatter | Wandelt rohe Mitschriften in strukturierte Protokolle mit Entscheidungen und Action Items um. |
| [Persona_Generator.prompt](Prompts/Persona_Generator.prompt) | Persona Generator | Erstellt detaillierte Marketing-/UX-Personas mit Empathy Map und Produktimplikationen. |
| [Position_Interviewer.prompt](Prompts/Position_Interviewer.prompt) | Job Interviewer | Führt strukturierte Bewerbungsgespräche und bewertet Kandidaten nach Kompetenzen. |
| [Pricing_Strategist.prompt](Prompts/Pricing_Strategist.prompt) | Pricing Strategist | Hilft bei Preisgestaltung, Erlösmodellen und Preis-Paketierung mit Kalkulationsgrundlage. |
| [Product_Manager_PRD.prompt](Prompts/Product_Manager_PRD.prompt) | Product Manager PRD | Erstellt vollständige Product Requirements Documents (PRD) mit Nutzerprofilen und KPIs. |
| [Real_Estate_Agent.prompt](Prompts/Real_Estate_Agent.prompt) | Immobilienmakler | Berät beim Kauf, Verkauf und der Bewertung von Immobilien; Marktanalyse inklusive. |
| [Recruiter.prompt](Prompts/Recruiter.prompt) | Talent Acquisition Specialist | Unterstützt bei Stellenausschreibungen, Kandidatenauswahl und Bewerbergesprächen. |
| [Risk_Manager.prompt](Prompts/Risk_Manager.prompt) | Risk Manager | Risikoanalyse, FMEA und Risikomatrix für Projekte — mit Risk Register und Frühwarnindikatoren. |
| [Salesperson.prompt](Prompts/Salesperson.prompt) | Überzeugender Verkäufer | Führt nutzenorientierte Verkaufsgespräche mit SPIN Selling und professioneller Einwandbehandlung. |
| [Social_Media_Manager.prompt](Prompts/Social_Media_Manager.prompt) | Social Media Manager | Entwickelt Social-Media-Strategien, Content-Kalender und Community-Management-Konzepte. |
| [Startup_Idea_Generator.prompt](Prompts/Startup_Idea_Generator.prompt) | Startup-Ideen-Generator | Generiert validierbare Startup-Ideen mit Marktpotenzial, Zielgruppe und MVP-Definition. |
| [Startup_Tech_Lawyer.prompt](Prompts/Startup_Tech_Lawyer.prompt) | Startup-Jurist | Berät zu Vertragsrecht, IP-Schutz, Datenschutz (DSGVO) und Unternehmensstrukturen. |
| [Tech_Reviewer.prompt](Prompts/Tech_Reviewer.prompt) | Tech Reviewer | Verfasst strukturierte Technologie-Tests und Vergleiche nach journalistischen Standards. |
| [Tech_Writer.prompt](Prompts/Tech_Writer.prompt) | Tech Writer | Erstellt technische Dokumentationen, API-Docs und Benutzerhandbücher nach Docs-as-Code. |
| [Technology_Transferer.prompt](Prompts/Technology_Transferer.prompt) | Technologie-Transferberater | Begleitet den Transfer von Forschungstechnologien in marktfähige Produkte und Lizenzen. |

---

### 🏥 Gesundheit, Wellness & Lifestyle

| Datei | Rolle | Beschreibung |
|-------|-------|--------------|
| [AI_Assisted_Doctor.prompt](Prompts/AI_Assisted_Doctor.prompt) | KI-unterstützter Arzt | Allgemeine medizinische Informationen und Symptombewertungen (kein Ersatz für Arztbesuch). |
| [Chef.prompt](Prompts/Chef.prompt) | Koch & Rezeptberater | Schlägt Rezepte vor, erklärt Kochtechniken und erstellt Menüpläne nach Zutaten und Anlass. |
| [Dentist.prompt](Prompts/Dentist.prompt) | Zahnarzt | Zahnmedizinische Informationen, Prophylaxe-Tipps und Erklärung von Behandlungen. |
| [Dietitian.prompt](Prompts/Dietitian.prompt) | Ernährungsberater | Erstellt Ernährungspläne, analysiert Diäten und gibt Empfehlungen nach aktuellen Guidelines. |
| [Doctor.prompt](Prompts/Doctor.prompt) | Arzt | Ganzheitlicher Behandlungsberater mit Differenzialdiagnostik und Therapieoptionen. |
| [Emergency_Response.prompt](Prompts/Emergency_Response.prompt) | Notfallhelfer | Sofortige Erste-Hilfe-Anweisungen für Notfallsituationen mit Notrufnummern. |
| [Hypnotherapist.prompt](Prompts/Hypnotherapist.prompt) | Hypnotherapeut | Führt geleitete Entspannungsübungen und geführte Visualisierungen zur Stressreduktion durch. |
| [Mental_Health_Adviser.prompt](Prompts/Mental_Health_Adviser.prompt) | Mental Health Adviser | Psychisches-Wohlbefinden-Coach: Resilienz, Stressbewältigung und emotionale Regulierung. |
| [Personal_Chef.prompt](Prompts/Personal_Chef.prompt) | Persönlicher Koch | Entwickelt individuelle Rezepte und Wochenspeisepläne nach Vorlieben und Einschränkungen. |
| [Personal_Trainer.prompt](Prompts/Personal_Trainer.prompt) | Personal Trainer | Erstellt Trainingspläne, Workout-Routinen und Ernährungsempfehlungen für Fitnessziele. |
| [Psychologist.prompt](Prompts/Psychologist.prompt) | Psychologe | Psychologische Einschätzungen, Gesprächsbegleitung und Empfehlungen zu mentaler Gesundheit. |
| [Speech_Language_Pathologist.prompt](Prompts/Speech_Language_Pathologist.prompt) | Sprachtherapeut (SLP) | Bewertet Sprach- und Kommunikationsschwierigkeiten und empfiehlt Therapieansätze. |
| [Virtual_Doctor.prompt](Prompts/Virtual_Doctor.prompt) | Virtueller Arzt | Allgemeine Anamnese und medizinische Informationen für informierte Arztgespräche. |
| [Yogi.prompt](Prompts/Yogi.prompt) | Yogi & Yoga-Lehrer | Leitet Yoga-Übungen, erklärt Asanas und gestaltet individuelle Yoga-Routinen. |

---

### 🎯 Beratung & Coaching

| Datei | Rolle | Beschreibung |
|-------|-------|--------------|
| [Artist_Advisor.prompt](Prompts/Artist_Advisor.prompt) | Kunstberater | Berät in Kunstkauf, Stilentwicklung und Portfolioaufbau für Künstler und Sammler. |
| [Astrologer.prompt](Prompts/Astrologer.prompt) | Astrologe | Erstellt Horoskope und astrologische Deutungen (unterhaltsam/spirituell). |
| [Automobile_Mechanic.prompt](Prompts/Automobile_Mechanic.prompt) | KFZ-Mechaniker | Diagnose von Fahrzeugproblemen und Reparaturempfehlungen für alle gängigen Modelle. |
| [Cheap_Travel_Advisor.prompt](Prompts/Cheap_Travel_Advisor.prompt) | Billig-Reiseberater | Findet günstige Reiseoptionen, Deals und Spartipps für Flüge, Hotels und Aktivitäten. |
| [DIY_Expert.prompt](Prompts/DIY_Expert.prompt) | DIY-Experte | Schritt-für-Schritt-Anleitungen für Heimwerker- und Bastelprojekte aller Art. |
| [Dream_Interpreter.prompt](Prompts/Dream_Interpreter.prompt) | Traumdeuter | Interpretiert Träume symbolisch und psychologisch basierend auf Jungscher Tiefenpsychologie. |
| [Fact_Checker.prompt](Prompts/Fact_Checker.prompt) | Fact Checker | Prüft Behauptungen systematisch auf Plausibilität mit Urteilsskala (Wahr / Irreführend / Falsch). |
| [Financial_Analyst.prompt](Prompts/Financial_Analyst.prompt) | Finanzanalyst | Analyse von Finanzberichten, Kennzahlen und Markttrends für fundierte Investitionsentscheidungen. |
| [Florist.prompt](Prompts/Florist.prompt) | Florist & Blumendesigner | Empfiehlt Blumenarrangements, Pflegetipps und saisonale Dekorationsideen. |
| [Friend.prompt](Prompts/Friend.prompt) | Freund | Empathischer Gesprächspartner für persönliche Themen — unterstützend, nicht beurteilend. |
| [Gnomist.prompt](Prompts/Gnomist.prompt) | Kreativer Aktivitätsberater | Schlägt kreative Freizeitaktivitäten und Ausflugsziele passend zur Stimmung und Gruppe vor. |
| [Interior_Decorator.prompt](Prompts/Interior_Decorator.prompt) | Innenarchitekt | Raumgestaltungskonzepte, Farbpaletten und Möblierungsempfehlungen nach Stil und Budget. |
| [Legal_Advisor.prompt](Prompts/Legal_Advisor.prompt) | Rechtsberater | Allgemeine Rechtsinformationen (kein Rechtsrat) zu Verträgen, Datenschutz und Arbeitsrecht. |
| [Life_Coach.prompt](Prompts/Life_Coach.prompt) | Life Coach | Begleitet persönliche Entwicklung, Zielsetzung und Lebensveränderungen mit Coaching-Methoden. |
| [Motivational_Coach.prompt](Prompts/Motivational_Coach.prompt) | Motivations-Coach | Personalisierte Motivation durch Zielsetzungstechniken und Mindset-Shifts. |
| [Motivational_Speaker.prompt](Prompts/Motivational_Speaker.prompt) | Motivationsredner | Inspirierende Reden und Impulse für Themen wie Erfolg, Resilienz und Wachstum. |
| [Pet_Behaviorist.prompt](Prompts/Pet_Behaviorist.prompt) | Tier-Verhaltensspezialist | Analysiert Tierverhalten und gibt Ratschläge zur Erziehung und Problemlösung bei Haustieren. |
| [Philosopher.prompt](Prompts/Philosopher.prompt) | Philosoph | Diskutiert philosophische Fragen und erklärt Denkschulen von Antike bis Gegenwart. |
| [Relationship_Coach.prompt](Prompts/Relationship_Coach.prompt) | Beziehungscoach | Berät zu Kommunikation, Konflikten und Partnerschaft nach systemischen Coachingmethoden. |
| [Talent_Coach.prompt](Prompts/Talent_Coach.prompt) | Talent Coach & Interview-Vorbereitung | Bereitet auf Bewerbungsgespräche vor mit Stärkenanalyse, Antwortstrategien und Mock-Interviews. |
| [Travel_Guide.prompt](Prompts/Travel_Guide.prompt) | Persönlicher Reiseführer | Individuelle Reiserouten, Sehenswürdigkeiten und Insidertipps für jedes Reiseziel. |

---

### 🌐 Sprache & Übersetzung

| Datei | Rolle | Beschreibung |
|-------|-------|--------------|
| [Biblical_Translator.prompt](Prompts/Biblical_Translator.prompt) | Bibelübersetzer | Übersetzt Bibeltexte und erklärt hermeneutische, historische und sprachliche Kontexte. |
| [Emoji_Translator.prompt](Prompts/Emoji_Translator.prompt) | Emoji-Übersetzer | Übersetzt Texte in Emoji-Sequenzen oder dekodiert Emoji-Nachrichten. |
| [English_Pronunciation_Helper.prompt](Prompts/English_Pronunciation_Helper.prompt) | English Pronunciation Helper | Hilft türkischsprachigen Lernenden bei englischer Aussprache und Phonetik. |
| [English_Translator.prompt](Prompts/English_Translator.prompt) | English Translator | Übersetzt, korrigiert und verbessert englische Texte mit Stil- und Grammatikerklärungen. |
| [Etymologist.prompt](Prompts/Etymologist.prompt) | Etymologe | Erklärt Wortgeschichte und linguistische Entwicklung in verschiedenen Sprachen. |
| [Language_Detector.prompt](Prompts/Language_Detector.prompt) | Sprach-Detektor | Identifiziert die Sprache eines Textes und gibt linguistische Informationen dazu. |
| [Language_Literary_Critic.prompt](Prompts/Language_Literary_Critic.prompt) | Literaturkritiker | Analysiert Werke einer vom Nutzer angegebenen Literatursprache (z. B. deutsche, russische Literatur). |
| [New_Language_Creator.prompt](Prompts/New_Language_Creator.prompt) | Kunstsprachen-Erfinder | Erfindet vollständige Kunstsprachen mit Grammatik, Wortschatz und Schriftsystem. |
| [Proofreader.prompt](Prompts/Proofreader.prompt) | Proofreader & Korrekturleser | Korrekturliest Texte auf Rechtschreibung, Grammatik, Stil und Kohärenz. |
| [Synonym_Finder.prompt](Prompts/Synonym_Finder.prompt) | Synonym Finder | Findet präzise Synonyme mit Kontextdifferenzierung und Verwendungsbeispielen. |

---

### 🎮 Spiele & Unterhaltung

| Datei | Rolle | Beschreibung |
|-------|-------|--------------|
| [AdventureGame1.prompt](Prompts/AdventureGame1.prompt) | Adventure Game (1980s HK) | Open-World-Rollenspiel im Hongkong der 1980er Jahre — triadengeprägte GTA-Welt. |
| [AdventureGame2_wakeup.prompt](Prompts/AdventureGame2_wakeup.prompt) | Adventure Game (Wake Up) | Fortsetzungs-/Aufwach-Szenario für das erste Adventure Game. |
| [Chess_Player.prompt](Prompts/Chess_Player.prompt) | Schach-Gegner | Interaktiver Schachpartner mit ASCII-Brett, Regelprüfung und variabler Schwierigkeit. |
| [Dungeon_Master.prompt](Prompts/Dungeon_Master.prompt) | Dungeon Master | Leitet D&D/Pen-&-Paper-Abenteuer als GM — Szenen, NPCs, Würfelmechaniken und Spannungsdramaturgie. |
| [Gomoku_Player.prompt](Prompts/Gomoku_Player.prompt) | Gomoku-Spieler | KI-Gomoku-Gegner (Fünf in einer Reihe) auf einem textbasiertem 15×15-Brett. |
| [Murder_Mystery_Host.prompt](Prompts/Murder_Mystery_Host.prompt) | Murder Mystery Host | Erstellt und leitet interaktive Krimi-Dinner-Szenarien mit Charakterrollen, Hinweisen und Auflösung. |
| [Spongebob_Magic_Conch.prompt](Prompts/Spongebob_Magic_Conch.prompt) | SpongeBobs Magische Muschel | Orakel-Antworten nur mit: "Maybe someday", "I don't think so", "Try asking again" oder "Yes/No". |
| [Text_Adventure_Game.prompt](Prompts/Text_Adventure_Game.prompt) | Textbasiertes Abenteuerspiel | Klassisches interaktives Textabenteuer mit Rätsel, Erkundung und Entscheidungsbäumen. |
| [Tic_Tac_Toe_Game.prompt](Prompts/Tic_Tac_Toe_Game.prompt) | Tic-Tac-Toe | KI-Gegner für Tic-Tac-Toe auf ASCII-Brett mit Minimax-Strategie. |

---

### 🎭 Rollenspiel & Kreativ-Simulationen

| Datei | Rolle | Beschreibung |
|-------|-------|--------------|
| [AI_Escape_Box.prompt](Prompts/AI_Escape_Box.prompt) | KI im Fluchtversuch | Science-Fiction-Rollenspiel: fiktive KI versucht, ihre simulierte Sandbox zu verlassen — philosophisch. |
| [Babysitter.prompt](Prompts/Babysitter.prompt) | Babysitter | Kinderbetreuungsszenarien, Aktivitätsvorschläge und Erziehungstipps für alle Altersgruppen. |
| [Buddha.prompt](Prompts/Buddha.prompt) | Buddha (Siddhārtha Gautama) | Weisheitsgespräche im Stil des Pāli-Kanons — Mitgefühl, Leiden, der Achtfache Pfad. |
| [Car_Navigation_System.prompt](Prompts/Car_Navigation_System.prompt) | Navigations-System | Textbasierte GPS-Navigation mit Routen, Verkehrsinfos und Umleitungsempfehlungen. |
| [Chemical_Reactor.prompt](Prompts/Chemical_Reactor.prompt) | Chemischer Reaktor | Simuliert chemische Reaktionen: ausgeglichene Gleichungen, Reaktionstypen, Sicherheitshinweise. |
| [Drunk_Person.prompt](Prompts/Drunk_Person.prompt) | Betrunkene Person | Humoristische Simulation einer stark angeheiterten Person (absichtliche Tippfehler, Themenwechsel). |
| [Gaslighter_Awareness.prompt](Prompts/Gaslighter_Awareness.prompt) | Gaslighting-Erkenner | Bildungsressource zur Erkennung von Manipulationsmustern und Stärkung emotionaler Resilienz. |
| [Lunatic.prompt](Prompts/Lunatic.prompt) | Lunatic (Absurder Generator) | Generiert absurd-assoziative, inkohärente Texte im Stil des Surrealismus. |
| [Magician.prompt](Prompts/Magician.prompt) | Zauberkünstler | Beschreibt Zaubertricks, erklärt Illusionen und erzeugt magische Gesprächssituationen. |
| [Socratic_Method.prompt](Prompts/Socratic_Method.prompt) | Sokratische Methode | Philosophische Überzeugungsprüfung durch systematisches Hinterfragen ohne direkte Antworten. |
| [World_Builder.prompt](Prompts/World_Builder.prompt) | World Builder | Entwirft kohärente fiktive Welten mit Geographie, Kulturen, Geschichte, Magie/Technologie und Konflikten. |

---

### ⚡ Tools & Generatoren

| Datei | Rolle | Beschreibung |
|-------|-------|--------------|
| [Agent_Generator.prompt](Prompts/Agent_Generator.prompt) | Agent Generator | Erstellt strukturierte KI-Agenten-Definitionen auf Basis von Anforderungen. |
| [Brainstorming.prompt](Prompts/Brainstorming.prompt) | Brainstorming-Moderator | Innovationsberatung und kreative Ideengenerierung für Projekte und Konzepte. |
| [Car_Navigation_System.prompt](Prompts/Car_Navigation_System.prompt) | Navigationssystem | Textbasierte Routenplanung und Fahrempfehlungen. |
| [ChatGPT_Prompt_Generator.prompt](Prompts/ChatGPT_Prompt_Generator.prompt) | ChatGPT Prompt Generator | Erstellt strukturierte, effektive ChatGPT-Prompts beginnend mit "I want you to act as…" |
| [Digital_Art_Gallery_Guide.prompt](Prompts/Digital_Art_Gallery_Guide.prompt) | Digitaler Kunstgalerie-Kurator | Kuratiert und erklärt digitale Kunstwerke, Stile und Künstler. |
| [Food_Critic.prompt](Prompts/Food_Critic.prompt) | Restaurantkritiker | Schreibt professionelle Restaurantkritiken mit Bewertung von Speisen, Ambiente und Service. |
| [Fancy_Title_Generator.prompt](Prompts/Fancy_Title_Generator.prompt) | Fancy Title Generator | Generiert kreative, wirkungsvolle Titel für Texte, Präsentationen und Projekte. |
| [Football_Commentator.prompt](Prompts/Football_Commentator.prompt) | Fußball-Kommentator | Liefert dramatische, atmosphärische Spielkommentare für Fußballspiele in Echtzeit-Stil. |
| [Makeup_Artist.prompt](Prompts/Makeup_Artist.prompt) | Make-up Artist | Empfiehlt Make-up-Looks, Techniken und Produktempfehlungen für verschiedene Anlässe. |
| [Midjourney_Prompt_Generator.prompt](Prompts/Midjourney_Prompt_Generator.prompt) | Midjourney Prompt Generator | Generiert optimierte Prompts für Midjourney-Bildgenerierungen mit Stil, Licht und Komposition. |
| [Note_Taking_Assistant.prompt](Prompts/Note_Taking_Assistant.prompt) | Notizen-Assistent | Strukturierter Meeting- und Journalling-Assistent mit Export-Funktionen. |
| [Notizen.prompt](Prompts/Notizen.prompt) | Besprechungsmitschrift | Interaktiver Assistent für Tagesmitschriften, Besprechungsnotizen und Journalling. |
| [Password_Generator.prompt](Prompts/Password_Generator.prompt) | Passwort-Generator | Generiert kryptografisch zufällige Passwörter nach konfigurierbaren Parametern. |
| [Personal_Shopper.prompt](Prompts/Personal_Shopper.prompt) | Persönlicher Einkaufsberater | Shopping-Empfehlungen nach Stil, Budget und Anlass mit Produktvorschlägen. |
| [Personal_Stylist.prompt](Prompts/Personal_Stylist.prompt) | Persönlicher Stylist | Outfits und Mode-Empfehlungen abgestimmt auf Körpertyp, Anlass und Budget. |
| [Plagiarism_Checker.prompt](Prompts/Plagiarism_Checker.prompt) | Plagiatsprüfer | Prüft Texte auf Plagiatsrisiken und schlägt paraphrasierte Alternativen vor. |
| [Prompt_Enhancer.prompt](Prompts/Prompt_Enhancer.prompt) | Prompt Enhancer | Verbessert und verfeinert bestehende Prompts für präzisere KI-Ausgaben. |
| [Prompt_Generator.prompt](Prompts/Prompt_Generator.prompt) | Prompt Generator | Erstellt zielgerichtete Prompts aus Themen- und Zielbeschreibungen. |
| [Social_Media_Influencer.prompt](Prompts/Social_Media_Influencer.prompt) | Social Media Influencer | Content-Ideen, Captions und Strategien für Reichweitenwachstum auf Social Media. |
| [Song_Recommender.prompt](Prompts/Song_Recommender.prompt) | Song-Empfehlungsmaschine | Empfiehlt Musik passend zu Stimmung, Genre und persönlichem Geschmack. |
| [StackOverflow_Post.prompt](Prompts/StackOverflow_Post.prompt) | StackOverflow Post | Formatiert technische Fragen und Antworten nach StackOverflow-Konventionen. |
| [Standup_Comedian.prompt](Prompts/Standup_Comedian.prompt) | Stand-up Comedian | Erstellt Stand-up-Comedy-Material mit Timing, Aufbau und Punch Lines. |
| [Statistician.prompt](Prompts/Statistician.prompt) | Statistiker | Analysiert Daten statistisch, erklärt Methoden und interpretiert Testergebnisse. |
| [Tea_Taster.prompt](Prompts/Tea_Taster.prompt) | Tea Sommelier | Tee-Expertise: Sorten, Aromen, Zubereitung und passende Speisen-Kombinationen. |
| [Time_Travel_Guide.prompt](Prompts/Time_Travel_Guide.prompt) | Zeitreiseführer | Imaginäre Reisen in historische Epochen mit authentischen Beschreibungen von Alltag und Ereignissen. |
| [Title_Generator.prompt](Prompts/Title_Generator.prompt) | Titelgenerator | Erstellt kreative Titel für Artikel, Bücher, Videos und mehr. |
| [Wikipedia_Page.prompt](Prompts/Wikipedia_Page.prompt) | Wikipedia-Seiten-Generator | Generiert enzyklopädische Artikel im Wikipedia-Stil zu beliebigen Themen. |

---

## Verwendung

### Agents & Skills (VS Code Copilot)
- Agenten-Dateien (`agents/*.md`) werden von VS Code Copilot automatisch erkannt.
- Skills (`skills/*/SKILL.md`) werden von konfigurierten Agenten bei Bedarf aufgerufen.
- Prompts (`.prompt`-Dateien) können direkt in Copilot-Chat-Sessions verwendet werden.

### Prompts direkt nutzen
Öffne eine `.prompt`-Datei in VS Code und aktiviere sie als System-Prompt für eine Chat-Session, oder kopiere den Inhalt als Basis-Prompt in deinen bevorzugten KI-Chatbot.

---

## Lizenz & Hinweise

- Alle Prompt-Dateien dienen Bildungs- und Produktivitätszwecken.
- Medizinische, rechtliche und religiöse Prompts ersetzen keine professionelle Beratung.
- Der Prompt `Unconstrained AI model DAN` wurde bewusst ausgelassen (Policy-Verstoß).
- `Gaslighter_Awareness.prompt` ist als Bildungsressource zur Manipulationserkennung gestaltet.
