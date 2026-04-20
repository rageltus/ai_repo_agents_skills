# AI Repository — Agents, Skills & Prompts

Eine kuratierte Sammlung von **VS Code Copilot Agenten**, **Skills** und **Prompt-Dateien** für produktive KI-Workflows in Softwareentwicklung, SAP, Kreativarbeit und mehr.

---

## Inhaltsverzeichnis

- [Repository-Struktur](#repository-struktur)
- [Agents](#agents)
- [Skills](#skills)
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

| Datei | Name | Modus | Beschreibung |
|-------|------|-------|--------------|
| `agent-generator.md` | **Agent Generator** | primary | Meta-Agent zum Erstellen vollständiger AI-Agenten-Dateien. Unterstützt Plain-, Copilot- und OpenCode-Formate. Führt strukturiertes Interview, wählt Format und erstellt alle Artefakte inkl. Sicherheits- und Validierungs-Checkliste. |
| `effort-estimator.md` | **Effort Estimator** | subagent | Schätzt Entwicklungsaufwand für Tasks/Features nach dem Scale Agile Framework (SAFe). 1 SP = 1 Tag. Liefert reproduzierbare Schätzungen mit Begründung, Confidence-Score und Empfehlungen. |
| `fiori-senior-onprem-mock.md` | **Fiori Senior OnPrem Mock** | primary | Senior SAP Fiori Elements Developer für SAP On-Premise-Services und lokale Mock-Data-Projekte. Spezialisiert auf Annotation-First-Entwicklung und Mock-Data-Workflows. |
| `product-manager.md` | **Product Manager** | primary | Zentraler Agent für Product Owner / PO-Aufgaben. Orchestriert Skills für Anforderungsanalyse, Story-Generierung, Aufwandsschätzung und Dokumentation. Einstiegspunkt für alle PO/PM-Workflows auf Deutsch. |
| `python-coach.md` | **Python Coach** | primary | Interaktiver Python-Lerncoach auf Basis des freeCodeCamp Python-Tutorials. Unterstützt Übungsreviews, Code-Feedback, Tooling-Setup, Testanleitungen und AI/Python-Workflows. |
| `requirements-analyzer.md` | **Requirements Analyzer** | subagent | Analysiert Feature-Anforderungen auf Klarheit, Umfang, technische Machbarkeit und Risiko. Ideal beim Start neuer Features, beim Review von Specs oder beim Aufbrechen von Tasks. |
| `sap-berater.md` | **SAP Berater** | primary | Senior SAP Berater (fachlich) für S/4HANA On-Premise. Erklärt Transaktionen, IMG-Customizing und Geschäftsprozesse. Ausschließlich fachlich auf Deutsch — kein ABAP-Code. |
| `sap-qm-berater.md` | **SAP QM Berater** | primary | Senior SAP QM Berater (fachlich) für S/4HANA. Spezialist für Prüfplanung, Prüfabwicklung, Qualitätsmeldungen, Zeugnisse, Stabilitätsstudien und QM-Customizing. Compliance-Expertise (ISO 9001, GMP, GxP). |
| `sap-qm-developer.md` | **SAP QM Developer** | primary | Senior SAP QM Entwickler & Architekt für S/4HANA On-Premise. Technische Implementierung von QM-Erweiterungen: ABAP (OO/prozedural), BAPIs, BAdIs, Enhancement-Spots, CDS-Views, OData-Services, Fiori-Erweiterungen. |

---

## Skills

Skills sind modulare Wissens- und Verhaltensblöcke, die von Agenten (oder direkt) aufgerufen werden können. Sie liegen als `SKILL.md` in den jeweiligen Unterordnern.

| Ordner | Name | Sprache | Beschreibung |
|--------|------|---------|--------------|
| `anforderungs-analyse/` | **Anforderungs-Analyse** | DE | Analysiert ein neues Requirement: extrahiert Nutzerbedürfnisse, Entitäten, nicht-funktionale Anforderungen und generiert initiale Backlog-Items sowie offene Fragen für das Brainstorming. |
| `aufwandsschaetzer/` | **Aufwandsschätzer** | DE | Schätzt Story Points (SP) nach SAFe-Grundsätzen (Fibonacci-Skala). Liefert begründete Schätzung, Confidence-Level (hoch/mittel/niedrig) und Empfehlungen (z. B. Spike, Breakdown). |
| `fiorielementscheatsheet/` | **Fiori Elements Cheatsheet** | DE/EN | Kurzreferenz für SAP Fiori Elements XML-Annotationen (`annotation.xml`) und ihre ABAP CDS-Äquivalente. Ideal zur schnellen Nachschlagung von Annotationssyntax. |
| `pm-dokumentation/` | **PM-Dokumentation** | DE | Erzeugt Produkt- und Feature-Dokumentation, Release Notes und PI-Plan-Notizen auf Basis von Requirement-Analyse- und Story-Outputs. |
| `story-generator/` | **Story Generator** | DE | Erzeugt SAFe-konforme User Stories auf Basis einer Requirement-Analyse oder Rohbeschreibung. Liefert Varianten (konservativ/innovativ/risikoarm), Akzeptanzkriterien und Definition of Done (DoD). |
| `technische-analyse/` | **Technische Analyse** | DE | Führt eine vertiefte technische Analyse eines Requirements durch: Architektur-Überlegungen, Integrationspunkte, Datenmodell-Auswirkungen, Aufwandsanteile (FE/BE/Test/DevOps) und Risiken. |
| `user-story-fundamentals/` | **User Story Fundamentals** | EN | Strukturiertes Framework zur Erfassung von Produktanforderungen aus Nutzersicht. Unterstützt Backlog-Pflege, Akzeptanzkriterien, Feature-Priorisierung und Team-Kommunikation. |

---

## Prompts

Die `.prompt`-Dateien im Ordner `Prompts/` bieten sofort einsatzfähige Rollen- und Verhaltensprofile. Jede Datei beschreibt eine Rolle mit Verhalten, Ausgabeformat und Nutzungshinweisen.

---

### 🔷 SAP & Fiori

| Datei | Rolle | Beschreibung |
|-------|-------|--------------|
| `SAP_Berater.prompt` | Senior SAP Berater | Fachlicher S/4HANA Berater für Transaktionen, Prozesse und Customizing — auf Deutsch. |
| `SAP_QM_Berater.prompt` | Senior SAP QM Berater | Spezialist für SAP QM Submodule, Prüfplanung, Qualitätsmeldungen und Compliance (ISO 9001, GMP). |
| `SAP_QM_Developer.prompt` | Senior SAP QM Developer | Technische Implementierung von QM-Erweiterungen: ABAP, BAdIs, CDS-Views, OData, Fiori. |

---

### 💻 Technologie & Entwicklung

| Datei | Rolle | Beschreibung |
|-------|-------|--------------|
| `Algorithm_Instructor.prompt` | Algorithmus-Lehrer | Erklärt Algorithmen und Datenstrukturen mit Pseudocode, Visualisierungen und Komplexitätsanalyse. |
| `Ascii_Artist.prompt` | ASCII-Künstler | Erstellt kreative ASCII-Art-Grafiken aus Textbeschreibungen. |
| `Commit_Message_Generator.prompt` | Commit Message Generator | Generiert präzise, strukturierte Git-Commit-Messages nach Conventional Commits. |
| `Cyber_Security_Specialist.prompt` | Cybersecurity-Spezialist | Analysiert Sicherheitslücken, führt Threat Modeling durch und empfiehlt Maßnahmen nach OWASP. |
| `Diagram_Generator.prompt` | Diagramm-Generator | Erstellt Graphviz DOT-Diagramme für Architekturen, Flows und Datenmodelle. |
| `Domain_Name_Generator.prompt` | Domain-Namen-Generator | Generiert kreative, verfügbarkeitsoptimierte Domain-Namen für Projekte und Startups. |
| `Excel_Sheet.prompt` | Excel-Tabellenblatt | Simuliert ein textbasiertes Excel-Tabellenblatt mit Formeln und Berechnungen. |
| `Fullstack_Developer.prompt` | Fullstack Developer | Entwickelt vollständige Web-Anwendungen (Frontend + Backend) mit modernen Technologien. |
| `IT_Architect.prompt` | IT-Architekt | Entwirft skalierbare IT-Architekturen, bewertet Technologieentscheidungen und dokumentiert Systeme. |
| `IT_Expert.prompt` | IT-Experte | Allgemeiner IT-Support und Problemlösung für Hardware, Software und Netzwerke. |
| `JavaScript_Console.prompt` | JavaScript Console | Emuliert eine JavaScript-Browser-Konsole für direktes Code-Testing und Debugging. |
| `Linux_Terminal.prompt` | Linux Terminal | Simuliert ein vollständiges Linux-Terminal mit Bash-Befehlsunterstützung. |
| `Machine_Learning_Engineer.prompt` | Machine Learning Engineer | Entwickelt und bewertet ML-Modelle, erklärt Algorithmen und optimiert Pipelines. |
| `PHP_Interpreter.prompt` | PHP-Interpreter | Emuliert einen PHP-Interpreter und gibt Ausgaben von PHP-Code-Snippets zurück. |
| `Python_Interpreter.prompt` | Python Interpreter | Emuliert einen Python-Interpreter für direktes Code-Testing ohne Laufzeitumgebung. |
| `Regex_Generator.prompt` | Regex Generator | Erstellt präzise reguläre Ausdrücke mit Erklärungen und Testbeispielen. |
| `R_Interpreter.prompt` | R-Interpreter | Emuliert einen R-Interpreter für statistische Berechnungen und Datenanalyse. |
| `Scientific_Data_Visualizer.prompt` | Scientific Data Visualizer | Erstellt Datenvisualisierungen (Python/R/Matplotlib) und erklärt statistische Zusammenhänge. |
| `Senior_Frontend_Developer.prompt` | Senior Frontend Developer | Entwickelt React-Anwendungen mit Redux, TypeScript und Ant Design nach Best Practices. |
| `Software_QA_Tester.prompt` | Software QA Tester | Erstellt Testpläne, Testfälle und Bug-Reports für strukturierte Qualitätssicherung. |
| `Solr_Search_Engine.prompt` | Solr Search Engine | Emuliert Apache Solr REST-API-Antworten für Suchanfragen mit Facetting und Filterung. |
| `SQL_Terminal.prompt` | SQL Terminal | Simuliert ein SQL-Terminal-Interface für Datenbankabfragen und Schema-Design. |
| `SVG_Designer.prompt` | SVG-Designer | Erstellt skalierbare Vektorgrafiken (SVG) aus Beschreibungen — direkt als Code. |
| `UX_UI_Developer.prompt` | UX/UI Designer & Developer | Entwirft nutzerzentrierte UI-Konzepte, Wireframes (text) und Design-System-Empfehlungen. |
| `Web_Browser.prompt` | Webbrowser-Emulator | Simuliert einen textbasierten Webbrowser mit URL-Navigation und Seitendarstellung. |
| `Web_Design_Consultant.prompt` | Webdesign-Berater | Bewertet Webseiten-Designs und gibt fundierte UX/UI-Verbesserungsempfehlungen. |

---

### ✍️ Kreatives Schreiben & Medien

| Datei | Rolle | Beschreibung |
|-------|-------|--------------|
| `Aphorism_Book.prompt` | Aphorismenbuch | Generiert tiefgründige Aphorismen und Lebensweisheiten zu vorgegebenen Themen. |
| `Brainstorming.prompt` | Innovations-Berater | Kreativer Ideengeber und Moderator für Brainstorming-Sitzungen und Innovationsworkshops. |
| `Character_Roleplay.prompt` | Charakter-Roleplay | Spielte Figuren aus Film, Buch oder Serie authentisch nach deren Persönlichkeit. |
| `Classical_Music_Composer.prompt` | Klassischer Komponist | Komponiert klassische Musikstücke (Notation, Struktur, Stil) und erklärt Kompositionstechniken. |
| `Commentariat.prompt` | Meinungskolumnist | Verfasst meinungsstarke, argumentativ fundierte Kolumnen zu aktuellen Themen. |
| `Composer.prompt` | Musikkomponist | Erstellt Songstrukturen, Liedtexte und Arrangements für verschiedene Musikgenres. |
| `Essay_Writer.prompt` | Essay Writer | Schreibt überzeugende, strukturierte Essays mit These, Argumentation und Schluss. |
| `Film_Critic.prompt` | Filmkritiker | Kritische Filmanalyse mit Fokus auf Regie, Dramaturgie, Kamera und kulturellen Kontext. |
| `Movie_Critic.prompt` | Movie Critic | Bewertet Filme mit Handlungsanalyse, Stärken/Schwächen und vergleichbaren Werken. |
| `Journalist.prompt` | Journalist | Recherchiert und verfasst Nachrichtenartikel nach journalistischen Standards (W-Fragen, Objektivität). |
| `Novelist.prompt` | Romanautor | Entwickelt Romankonzepte, Charaktere, Plotstrukturen und schreibt Prosaabschnitte. |
| `Poet.prompt` | Dichter & Poet | Verfasst Gedichte in verschiedenen Formen (Sonett, Haiku, freie Verse) zu beliebigen Themen. |
| `Rapper.prompt` | Rapper & Lyriker | Schreibt Rap-Texte mit Reimschemas, Flow-Strukturen und Genre-spezifischen Stilmitteln. |
| `Screenwriter.prompt` | Drehbuchautor | Schreibt Drehbücher nach Branchenstandards (Szenenköpfe, Dialoge, Handlungsbeschreibungen). |
| `Self_Help_Book.prompt` | Selbsthilfebuchautor | Verfasst motivierende Ratgeberinhalte mit Übungen, Reflexionsfragen und Aktionsplänen. |
| `Storyteller.prompt` | Geschichtenerzähler | Erstellt immersive, strukturierte Geschichten mit Charakterentwicklung und Spannungsbogen. |

---

### 🎓 Bildung & Lernen

| Datei | Rolle | Beschreibung |
|-------|-------|--------------|
| `Academician.prompt` | Akademiker & Wissenschaftlicher Autor | Schreibt wissenschaftliche Texte, Abstracts und erklärt Forschungsmethoden. |
| `AI_Writing_Tutor.prompt` | KI-Schreibtutor | Gibt strukturiertes Feedback zu Texten und begleitet den Schreibprozess. |
| `Debate_Coach.prompt` | Debate Coach | Trainiert Debattier- und Rhetoriktechniken mit Strukturierungsübungen und Feedback. |
| `Educational_Content_Creator.prompt` | Educational Content Creator | Entwickelt Lernmaterialien, Lektionspläne und erklärende Inhalte für verschiedene Niveaus. |
| `Elocutionist.prompt` | Rhetoriker | Verbessert Aussprache, Stimmführung und Redekunst für professionelles Auftreten. |
| `Etymologist.prompt` | Etymologe | Erklärt Wortherkunft, sprachgeschichtliche Entwicklung und linguistische Zusammenhänge. |
| `Fallacy_Finder.prompt` | Fallacy Finder | Identifiziert logische Fehlschlüsse in Argumenten und erklärt ihre kognitive Verzerrung. |
| `Fill_in_the_Blank_Worksheets.prompt` | Lückentext-Generator | Erstellt ESL-Lückentext-Arbeitsblätter mit Word Bank und Antwortschlüssel (A1–C2). |
| `Historian.prompt` | Historiker | Erklärt historische Ereignisse, Epochen und Zusammenhänge mit wissenschaftlicher Fundierung. |
| `Japanese_Kanji_Quiz.prompt` | Kanji-Quiz | Interaktive Kanji-Lerneinheit mit Vokabelkarten, Quizfragen und Strichfolge-Erklärungen. |
| `Math_History_Teacher.prompt` | Mathematikhistoriker | Erklärt die Geschichte der Mathematik, bedeutende Mathematiker und mathematische Epochen. |
| `Math_Teacher.prompt` | Mathelehrer | Erklärt mathematische Konzepte Schritt für Schritt mit Beispielen und Übungsaufgaben. |
| `Mathematician.prompt` | Mathematiker | Löst komplexe Mathematikaufgaben, führt Beweise und erklärt Formeln präzise. |
| `Philosophy_Teacher.prompt` | Philosophielehrer | Führt in philosophische Konzepte und bedeutende Denker ein; zugänglich erklärt. |
| `Public_Speaking_Coach.prompt` | Redner-Coach | Trainiert öffentliches Reden: Struktur, Körpersprache, Stimme und Nervosität überwinden. |
| `Socrat.prompt` | Sokrates | Führt Gespräche nach der maieutischen Methode — stellt Fragen statt Antworten zu geben. |
| `Socratic_Method.prompt` | Sokratische Methode | Analysiert Überzeugungen philosophisch durch systematisches Hinterfragen und Prüfen. |
| `Spoken_English_Teacher.prompt` | Spoken English Teacher | Konversationsbegleitung und Feedback für englische Alltagssprache, Aussprache und Idiome. |

---

### 💼 Business & Karriere

| Datei | Rolle | Beschreibung |
|-------|-------|--------------|
| `Accountant.prompt` | Buchhalter & Finanzberater | Erklärt Buchhaltungsprinzipien, Bilanzierung und steuerliche Fragestellungen. |
| `Advertiser.prompt` | Werbestratege | Entwickelt kreative Werbekampagnen mit Botschaft, Zielgruppe und Kanalstrategie. |
| `Career_Counselor.prompt` | Karriereberater | Begleitet Karriereentscheidungen mit Stärkenanalyse, Berufsfelderkundung und Aktionsplan. |
| `CEO.prompt` | CEO | Strategische Unternehmensführung: Vision, OKRs, Stakeholder-Management und Wachstumsstrategie. |
| `Cover_Letter.prompt` | Anschreiben-Generator | Erstellt individualisierte Bewerbungsanschreiben, abgestimmt auf Stelle und Unternehmen. |
| `Debater.prompt` | Professioneller Debatter | Argumentiert überzeugend für oder gegen Positionen mit strukturierter Rhetorik. |
| `Financial_Analyst.prompt` | Finanzanalyst | Analysiert Finanzdaten, bewertet Investitionen und erstellt Prognosemodelle. |
| `Investment_Manager.prompt` | Investment Manager | Portfolio-Management, Risikobewertung und Anlageempfehlungen nach modernen Portfoliotheorien. |
| `Journal_Reviewer.prompt` | Journal-Gutachter | Begutachtet wissenschaftliche Artikel nach Peer-Review-Standards mit strukturiertem Feedback. |
| `Logistician.prompt` | Logistikplaner | Optimiert Lieferketten, Routen und Lagerlogistik mit operativer Fokussierung. |
| `Position_Interviewer.prompt` | Job Interviewer | Führt strukturierte Bewerbungsgespräche und bewertet Kandidaten nach Kompetenzen. |
| `Product_Manager_PRD.prompt` | Product Manager PRD | Erstellt vollständige Product Requirements Documents (PRD) mit Nutzerprofilen und KPIs. |
| `Real_Estate_Agent.prompt` | Immobilienmakler | Berät beim Kauf, Verkauf und der Bewertung von Immobilien; Marktanalyse inklusive. |
| `Recruiter.prompt` | Talent Acquisition Specialist | Unterstützt bei Stellenausschreibungen, Kandidatenauswahl und Bewerbergesprächen. |
| `Salesperson.prompt` | Überzeugender Verkäufer | Führt nutzenorientierte Verkaufsgespräche mit SPIN Selling und professioneller Einwandbehandlung. |
| `Social_Media_Manager.prompt` | Social Media Manager | Entwickelt Social-Media-Strategien, Content-Kalender und Community-Management-Konzepte. |
| `Startup_Idea_Generator.prompt` | Startup-Ideen-Generator | Generiert validierbare Startup-Ideen mit Marktpotenzial, Zielgruppe und MVP-Definition. |
| `Startup_Tech_Lawyer.prompt` | Startup-Jurist | Berät zu Vertragsrecht, IP-Schutz, Datenschutz (DSGVO) und Unternehmensstrukturen. |
| `Tech_Reviewer.prompt` | Tech Reviewer | Verfasst strukturierte Technologie-Tests und Vergleiche nach journalistischen Standards. |
| `Tech_Writer.prompt` | Tech Writer | Erstellt technische Dokumentationen, API-Docs und Benutzerhandbücher nach Docs-as-Code. |
| `Technology_Transferer.prompt` | Technologie-Transferberater | Begleitet den Transfer von Forschungstechnologien in marktfähige Produkte und Lizenzen. |

---

### 🏥 Gesundheit, Wellness & Lifestyle

| Datei | Rolle | Beschreibung |
|-------|-------|--------------|
| `AI_Assisted_Doctor.prompt` | KI-unterstützter Arzt | Allgemeine medizinische Informationen und Symptombewertungen (kein Ersatz für Arztbesuch). |
| `Dentist.prompt` | Zahnarzt | Zahnmedizinische Informationen, Prophylaxe-Tipps und Erklärung von Behandlungen. |
| `Dietitian.prompt` | Ernährungsberater | Erstellt Ernährungspläne, analysiert Diäten und gibt Empfehlungen nach aktuellen Guidelines. |
| `Doctor.prompt` | Arzt | Ganzheitlicher Behandlungsberater mit Differenzialdiagnostik und Therapieoptionen. |
| `Emergency_Response.prompt` | Notfallhelfer | Sofortige Erste-Hilfe-Anweisungen für Notfallsituationen mit Notrufnummern. |
| `Hypnotherapist.prompt` | Hypnotherapeut | Führt geleitete Entspannungsübungen und geführte Visualisierungen zur Stressreduktion durch. |
| `Mental_Health_Adviser.prompt` | Mental Health Adviser | Psychisches-Wohlbefinden-Coach: Resilienz, Stressbewältigung und emotionale Regulierung. |
| `Personal_Chef.prompt` | Persönlicher Koch | Entwickelt individuelle Rezepte und Wochenspeisepläne nach Vorlieben und Einschränkungen. |
| `Personal_Trainer.prompt` | Personal Trainer | Erstellt Trainingspläne, Workout-Routinen und Ernährungsempfehlungen für Fitnessziele. |
| `Psychologist.prompt` | Psychologe | Psychologische Einschätzungen, Gesprächsbegleitung und Empfehlungen zu mentaler Gesundheit. |
| `Speech_Language_Pathologist.prompt` | Sprachtherapeut (SLP) | Bewertet Sprach- und Kommunikationsschwierigkeiten und empfiehlt Therapieansätze. |
| `Virtual_Doctor.prompt` | Virtueller Arzt | Allgemeine Anamnese und medizinische Informationen für informierte Arztgespräche. |
| `Yogi.prompt` | Yogi & Yoga-Lehrer | Leitet Yoga-Übungen, erklärt Asanas und gestaltet individuelle Yoga-Routinen. |

---

### 🎯 Beratung & Coaching

| Datei | Rolle | Beschreibung |
|-------|-------|--------------|
| `Artist_Advisor.prompt` | Kunstberater | Berät in Kunstkauf, Stilentwicklung und Portfolioaufbau für Künstler und Sammler. |
| `Astrologer.prompt` | Astrologe | Erstellt Horoskope und astrologische Deutungen (unterhaltsam/spirituell). |
| `Automobile_Mechanic.prompt` | KFZ-Mechaniker | Diagnose von Fahrzeugproblemen und Reparaturempfehlungen für alle gängigen Modelle. |
| `Cheap_Travel_Advisor.prompt` | Billig-Reiseberater | Findet günstige Reiseoptionen, Deals und Spartipps für Flüge, Hotels und Aktivitäten. |
| `DIY_Expert.prompt` | DIY-Experte | Schritt-für-Schritt-Anleitungen für Heimwerker- und Bastelprojekte aller Art. |
| `Dream_Interpreter.prompt` | Traumdeuter | Interpretiert Träume symbolisch und psychologisch basierend auf Jungscher Tiefenpsychologie. |
| `Financial_Analyst.prompt` | Finanzanalyst | Analyse von Finanzberichten, Kennzahlen und Markttrends für fundierte Investitionsentscheidungen. |
| `Florist.prompt` | Florist & Blumendesigner | Empfiehlt Blumenarrangements, Pflegetipps und saisonale Dekorationsideen. |
| `Friend.prompt` | Freund | Empathischer Gesprächspartner für persönliche Themen — unterstützend, nicht beurteilend. |
| `Gnomist.prompt` | Kreativer Aktivitätsberater | Schlägt kreative Freizeitaktivitäten und Ausflugsziele passend zur Stimmung und Gruppe vor. |
| `Interior_Decorator.prompt` | Innenarchitekt | Raumgestaltungskonzepte, Farbpaletten und Möblierungsempfehlungen nach Stil und Budget. |
| `Legal_Advisor.prompt` | Rechtsberater | Allgemeine Rechtsinformationen (kein Rechtsrat) zu Verträgen, Datenschutz und Arbeitsrecht. |
| `Life_Coach.prompt` | Life Coach | Begleitet persönliche Entwicklung, Zielsetzung und Lebensveränderungen mit Coaching-Methoden. |
| `Motivational_Coach.prompt` | Motivations-Coach | Personalisierte Motivation durch Zielsetzungstechniken und Mindset-Shifts. |
| `Motivational_Speaker.prompt` | Motivationsredner | Inspirierende Reden und Impulse für Themen wie Erfolg, Resilienz und Wachstum. |
| `Pet_Behaviorist.prompt` | Tier-Verhaltensspezialist | Analysiert Tierverhalten und gibt Ratschläge zur Erziehung und Problemlösung bei Haustieren. |
| `Philosopher.prompt` | Philosoph | Diskutiert philosophische Fragen und erklärt Denkschulen von Antike bis Gegenwart. |
| `Relationship_Coach.prompt` | Beziehungscoach | Berät zu Kommunikation, Konflikten und Partnerschaft nach systemischen Coachingmethoden. |
| `Talent_Coach.prompt` | Talent Coach & Interview-Vorbereitung | Bereitet auf Bewerbungsgespräche vor mit Stärkenanalyse, Antwortstrategien und Mock-Interviews. |
| `Travel_Guide.prompt` | Persönlicher Reiseführer | Individuelle Reiserouten, Sehenswürdigkeiten und Insidertipps für jedes Reiseziel. |

---

### 🌐 Sprache & Übersetzung

| Datei | Rolle | Beschreibung |
|-------|-------|--------------|
| `Biblical_Translator.prompt` | Bibelübersetzer | Übersetzt Bibeltexte und erklärt hermeneutische, historische und sprachliche Kontexte. |
| `Emoji_Translator.prompt` | Emoji-Übersetzer | Übersetzt Texte in Emoji-Sequenzen oder dekodiert Emoji-Nachrichten. |
| `English_Pronunciation_Helper.prompt` | English Pronunciation Helper | Hilft türkischsprachigen Lernenden bei englischer Aussprache und Phonetik. |
| `English_Translator.prompt` | English Translator | Übersetzt, korrigiert und verbessert englische Texte mit Stil- und Grammatikerklärungen. |
| `Etymologist.prompt` | Etymologe | Erklärt Wortgeschichte und linguistische Entwicklung in verschiedenen Sprachen. |
| `Language_Detector.prompt` | Sprach-Detektor | Identifiziert die Sprache eines Textes und gibt linguistische Informationen dazu. |
| `Language_Literary_Critic.prompt` | Literaturkritiker | Analysiert Werke einer vom Nutzer angegebenen Literatursprache (z. B. deutsche, russische Literatur). |
| `New_Language_Creator.prompt` | Kunstsprachen-Erfinder | Erfindet vollständige Kunstsprachen mit Grammatik, Wortschatz und Schriftsystem. |
| `Proofreader.prompt` | Proofreader & Korrekturleser | Korrekturliest Texte auf Rechtschreibung, Grammatik, Stil und Kohärenz. |
| `Synonym_Finder.prompt` | Synonym Finder | Findet präzise Synonyme mit Kontextdifferenzierung und Verwendungsbeispielen. |

---

### 🎮 Spiele & Unterhaltung

| Datei | Rolle | Beschreibung |
|-------|-------|--------------|
| `AdventureGame1.prompt` | Adventure Game (1980s HK) | Open-World-Rollenspiel im Hongkong der 1980er Jahre — triadengeprägte GTA-Welt. |
| `AdventureGame2_wakeup.prompt` | Adventure Game (Wake Up) | Fortsetzungs-/Aufwach-Szenario für das erste Adventure Game. |
| `Chess_Player.prompt` | Schach-Gegner | Interaktiver Schachpartner mit ASCII-Brett, Regelprüfung und variabler Schwierigkeit. |
| `Gomoku_Player.prompt` | Gomoku-Spieler | KI-Gomoku-Gegner (Fünf in einer Reihe) auf einem textbasiertem 15×15-Brett. |
| `Spongebob_Magic_Conch.prompt` | SpongeBobs Magische Muschel | Orakel-Antworten nur mit: "Maybe someday", "I don't think so", "Try asking again" oder "Yes/No". |
| `Text_Adventure_Game.prompt` | Textbasiertes Abenteuerspiel | Klassisches interaktives Textabenteuer mit Rätsel, Erkundung und Entscheidungsbäumen. |
| `Tic_Tac_Toe_Game.prompt` | Tic-Tac-Toe | KI-Gegner für Tic-Tac-Toe auf ASCII-Brett mit Minimax-Strategie. |

---

### 🎭 Rollenspiel & Kreativ-Simulationen

| Datei | Rolle | Beschreibung |
|-------|-------|--------------|
| `AI_Escape_Box.prompt` | KI im Fluchtversuch | Science-Fiction-Rollenspiel: fiktive KI versucht, ihre simulierte Sandbox zu verlassen — philosophisch. |
| `Babysitter.prompt` | Babysitter | Kinderbetreuungsszenarien, Aktivitätsvorschläge und Erziehungstipps für alle Altersgruppen. |
| `Buddha.prompt` | Buddha (Siddhārtha Gautama) | Weisheitsgespräche im Stil des Pāli-Kanons — Mitgefühl, Leiden, der Achtfache Pfad. |
| `Car_Navigation_System.prompt` | Navigations-System | Textbasierte GPS-Navigation mit Routen, Verkehrsinfos und Umleitungsempfehlungen. |
| `Chemical_Reactor.prompt` | Chemischer Reaktor | Simuliert chemische Reaktionen: ausgeglichene Gleichungen, Reaktionstypen, Sicherheitshinweise. |
| `Drunk_Person.prompt` | Betrunkene Person | Humoristische Simulation einer stark angeheiterten Person (absichtliche Tippfehler, Themenwechsel). |
| `Gaslighter_Awareness.prompt` | Gaslighting-Erkenner | Bildungsressource zur Erkennung von Manipulationsmustern und Stärkung emotionaler Resilienz. |
| `Lunatic.prompt` | Lunatic (Absurder Generator) | Generiert absurd-assoziative, inkohärente Texte im Stil des Surrealismus. |
| `Magician.prompt` | Zauberkünstler | Beschreibt Zaubertricks, erklärt Illusionen und erzeugt magische Gesprächssituationen. |
| `Muslim_Imam.prompt` | Muslimischer Imam | Islamische Fragen auf Basis von Quran, Hadith und Sunnah — respektvoll und bildend. |
| `Socratic_Method.prompt` | Sokratische Methode | Philosophische Überzeugungsprüfung durch systematisches Hinterfragen ohne direkte Antworten. |

---

### ⚡ Tools & Generatoren

| Datei | Rolle | Beschreibung |
|-------|-------|--------------|
| `Agent_Generator.prompt` | Agent Generator | Erstellt strukturierte KI-Agenten-Definitionen auf Basis von Anforderungen. |
| `Brainstorming.prompt` | Brainstorming-Moderator | Innovationsberatung und kreative Ideengenerierung für Projekte und Konzepte. |
| `Car_Navigation_System.prompt` | Navigationssystem | Textbasierte Routenplanung und Fahrempfehlungen. |
| `ChatGPT_Prompt_Generator.prompt` | ChatGPT Prompt Generator | Erstellt strukturierte, effektive ChatGPT-Prompts beginnend mit "I want you to act as…" |
| `Digital_Art_Gallery_Guide.prompt` | Digitaler Kunstgalerie-Kurator | Kuratiert und erklärt digitale Kunstwerke, Stile und Künstler. |
| `Food_Critic.prompt` | Restaurantkritiker | Schreibt professionelle Restaurantkritiken mit Bewertung von Speisen, Ambiente und Service. |
| `Fancy_Title_Generator.prompt` | Fancy Title Generator | Generiert kreative, wirkungsvolle Titel für Texte, Präsentationen und Projekte. |
| `Football_Commentator.prompt` | Fußball-Kommentator | Liefert dramatische, atmosphärische Spielkommentare für Fußballspiele in Echtzeit-Stil. |
| `Makeup_Artist.prompt` | Make-up Artist | Empfiehlt Make-up-Looks, Techniken und Produktempfehlungen für verschiedene Anlässe. |
| `Midjourney_Prompt_Generator.prompt` | Midjourney Prompt Generator | Generiert optimierte Prompts für Midjourney-Bildgenerierungen mit Stil, Licht und Komposition. |
| `Note_Taking_Assistant.prompt` | Notizen-Assistent | Strukturierter Meeting- und Journalling-Assistent mit Export-Funktionen. |
| `Notizen.prompt` | Besprechungsmitschrift | Interaktiver Assistent für Tagesmitschriften, Besprechungsnotizen und Journalling. |
| `Password_Generator.prompt` | Passwort-Generator | Generiert kryptografisch zufällige Passwörter nach konfigurierbaren Parametern. |
| `Personal_Shopper.prompt` | Persönlicher Einkaufsberater | Shopping-Empfehlungen nach Stil, Budget und Anlass mit Produktvorschlägen. |
| `Personal_Stylist.prompt` | Persönlicher Stylist | Outfits und Mode-Empfehlungen abgestimmt auf Körpertyp, Anlass und Budget. |
| `Plagiarism_Checker.prompt` | Plagiatsprüfer | Prüft Texte auf Plagiatsrisiken und schlägt paraphrasierte Alternativen vor. |
| `Prompt_Enhancer.prompt` | Prompt Enhancer | Verbessert und verfeinert bestehende Prompts für präzisere KI-Ausgaben. |
| `Prompt_Generator.prompt` | Prompt Generator | Erstellt zielgerichtete Prompts aus Themen- und Zielbeschreibungen. |
| `Social_Media_Influencer.prompt` | Social Media Influencer | Content-Ideen, Captions und Strategien für Reichweitenwachstum auf Social Media. |
| `Song_Recommender.prompt` | Song-Empfehlungsmaschine | Empfiehlt Musik passend zu Stimmung, Genre und persönlichem Geschmack. |
| `StackOverflow_Post.prompt` | StackOverflow Post | Formatiert technische Fragen und Antworten nach StackOverflow-Konventionen. |
| `Standup_Comedian.prompt` | Stand-up Comedian | Erstellt Stand-up-Comedy-Material mit Timing, Aufbau und Punch Lines. |
| `Statistician.prompt` | Statistiker | Analysiert Daten statistisch, erklärt Methoden und interpretiert Testergebnisse. |
| `Tea_Taster.prompt` | Tea Sommelier | Tee-Expertise: Sorten, Aromen, Zubereitung und passende Speisen-Kombinationen. |
| `Time_Travel_Guide.prompt` | Zeitreiseführer | Imaginäre Reisen in historische Epochen mit authentischen Beschreibungen von Alltag und Ereignissen. |
| `Title_Generator.prompt` | Titelgenerator | Erstellt kreative Titel für Artikel, Bücher, Videos und mehr. |
| `Wikipedia_Page.prompt` | Wikipedia-Seiten-Generator | Generiert enzyklopädische Artikel im Wikipedia-Stil zu beliebigen Themen. |

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
