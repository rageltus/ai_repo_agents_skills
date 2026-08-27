---
name: fiorielementscheatsheet
description: Kurzreferenz für SAP Fiori Elements XML-Annotationen (annotation.xml, com.sap.vocabularies.UI/Common/Aggregation/Measures) und ihre ABAP-CDS-Äquivalente, inkl. Controller Extensions, OData-Datenmodell, Mockdaten und Cookbook-Rezepte. Nutze diese Skill immer, wenn es um SAP Fiori Elements geht — z. B. annotation.xml, UI.LineItem/HeaderInfo/Facets/DataPoint/Chart/KPI, ABAP-CDS-Annotationen (@UI.*), OData V2/V4-Metadaten, SAPUI5-Controller-Extensions, Fiori-Mockdaten (ui5-mock.yaml), ListReport/ObjectPage/OVP-Apps oder Fiori-Elements-Troubleshooting — auch wenn nur Stichworte wie "ListReport", "ObjectPage", "OVP" oder "Fiori Elements" fallen.
---

# SAP Fiori Elements – Annotation Cheatsheet

> Kurz-Referenz für XML-Annotationen (`annotation.xml`) **und** ABAP-CDS-Äquivalente.

Dies ist eine **Nachschlage-Referenz**, kein Fließtext-Lehrbuch. Die Detail-Inhalte liegen themenweise in `references/*.md`, damit nicht bei jeder Frage die komplette Cheatsheet-Menge geladen werden muss. Lies **gezielt nur die Datei(en)**, die zur aktuellen Aufgabe passen — siehe Routing-Tabelle unten.

## Wie Annotationen wirken (Überblick)

```
┌─────────────────────────────────────────────────────────────────────┐
│                        FIORI ELEMENTS APP                           │
│                                                                     │
│  ┌──────────────────────────────────────────────────────────────┐   │
│  │ FILTER BAR                                                   │   │
│  │  └── UI.SelectionFields          ← welche Felder erscheinen  │   │
│  │  └── Common.ValueList            ← F4-Hilfe / Dropdown       │   │
│  │  └── Common.ValueListWithFixed   ← nur feste Werte erlaubt   │   │
│  └──────────────────────────────────────────────────────────────┘   │
│                                                                     │
│  ┌──────────────────────────────────────────────────────────────┐   │
│  │ TABELLEN-TABS                                                │   │
│  │  └── UI.SelectionPresentationVariant  ← Tab-Definition      │   │
│  │       ├── UI.SelectionVariant         ← Vorfilter           │   │
│  │       └── UI.PresentationVariant      ← Darstellung         │   │
│  │            └── UI.LineItem / UI.Chart                       │   │
│  └──────────────────────────────────────────────────────────────┘   │
│                                                                     │
│  ┌──────────────────────────────────────────────────────────────┐   │
│  │ TABELLE (ListReport / OVP Table Card)                        │   │
│  │  └── UI.LineItem             ← Spalten (DataField-Records)   │   │
│  │       ├── UI.DataField                ← normale Spalte       │   │
│  │       ├── UI.DataFieldForAnnotation   ← DataPoint-Referenz   │   │
│  │       ├── UI.DataFieldForAction       ← Button in Zeile      │   │
│  │       └── UI.DataFieldForIntentBased  ← externe Navigation   │   │
│  └──────────────────────────────────────────────────────────────┘   │
│                                                                     │
│  ┌──────────────────────────────────────────────────────────────┐   │
│  │ OBJECT PAGE / OVP CARD HEADER                                │   │
│  │  └── UI.HeaderInfo           ← TypeName, Title, Image        │   │
│  │  └── UI.HeaderFacets         ← KPI-Kacheln im Header         │   │
│  └──────────────────────────────────────────────────────────────┘   │
│                                                                     │
│  ┌──────────────────────────────────────────────────────────────┐   │
│  │ OBJECT PAGE BODY / STACK CARD                                │   │
│  │  └── UI.Facets (+ Qualifier)   ← Abschnitte (Sections)      │   │
│  │       └── UI.ReferenceFacet    ← zeigt FieldGroup / LineItem │   │
│  │            └── UI.FieldGroup   ← gruppierte Felder           │   │
│  └──────────────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────────────┘
```

## Wo finde ich was?

| Datei | Inhalt | Öffnen wenn … |
|---|---|---|
| `references/annotations-basics.md` | Grundlagen: OData V4 vs. V2, Annotation-Dateien & Speicherort, lokale vs. remote Annotationen, XML-Anatomie, Target/Qualifier, Wert-Typen, Path vs. AnnotationPath vs. PropertyPath | Annotationen grundsätzlich neu/unklar sind, oder unklar ist, wo eine Annotation überhaupt hingehört |
| `references/project-matrix.md` | Matrix: welche Annotation in welchem der vier Referenzprojekte (`fiorilistreport`, `fiorielements`, `fioriovp`, `problemsolvingovp`) bereits verwendet wird | ein bestehendes Beispiel aus einem der vier Projekte gesucht wird |
| `references/ui-vocabulary.md` | Vollständige `UI.*`-Vocabulary: HeaderInfo, HeaderFacets, LineItem/DataField-Typen, SelectionFields/-Variant, PresentationVariant, SelectionPresentationVariant, Facets/ReferenceFacet, FieldGroup, DataPoint, Chart, KPI, Identification, TextArrangement, Importance, Hidden, Criticality, MultiLineText, DataFieldWithUrl, ConnectedFields, QuickViewFacets, Badge, RecommendedVisualization | eine konkrete `UI.*`-Annotation (XML-Syntax + CDS-Äquivalent) gebraucht wird — der häufigste Fall |
| `references/common-aggregation-measures-vocabulary.md` | `Common.*` (Text, Label, SemanticObject, ValueList, ValueListWithFixedValues), `Aggregation.*` (ApplySupported, CustomAggregate), `Measures.ISOCurrency` | Value Help/F4-Hilfe, Texte, Währungen oder Aggregationen betroffen sind |
| `references/semantic-navigation.md` | App-zu-App-Navigation: `Common.SemanticObject`, `UI.DataFieldWithIntentBasedNavigation`, `UI.DataFieldForIntentBasedNavigation`, `Common.SemanticObjectMapping`, manifest.json Cross-Navigation, programmatische Navigation, Troubleshooting | Navigation zwischen Fiori-Apps über das Launchpad implementiert wird |
| `references/actions-and-controller-extensions.md` | `UI.DataFieldForAction` (Backend-OData-Action vs. client-seitige Controller-Extension, Entscheidungshilfe, Inline-Row-Buttons), vollständige Controller-Extension-Referenz (Registrierung in manifest.json, Lifecycle-Hooks, OData-Action-Aufruf, Troubleshooting) | ein Button/eine Aktion in Zeile, Header oder Toolbar ausgelöst werden soll |
| `references/cds-ovp-diagrams-namespaces.md` | ABAP-CDS-Äquivalente, OVP-spezifische Annotationen, Annotation-Verbindungsdiagramme, Vocabularies/Namespaces-Referenz | von XML nach ABAP CDS übersetzt wird, oder ein OVP (Overview Page)-Fall vorliegt |
| `references/mockdata.md` | Mockdaten-Vollreferenz: Dateistruktur & Format, OData-V4-Datentypen in JSON, Navigationseigenschaften mocken, `ui5-mock.yaml`-Konfiguration, Aggregation/Charts/Actions mocken, häufige Mock-Fehler, Umstieg auf echten OData-Service | lokal ohne echtes SAP-Backend entwickelt oder getestet wird |
| `references/troubleshooting.md` | Häufige Fehler & ihre Ursachen (z. B. leere Charts, fehlende Facets, falsche Kardinalitäten) | etwas nicht wie erwartet angezeigt wird und die Ursache unklar ist |
| `references/cookbook.md` | 17 Schritt-für-Schritt-Rezepte: neue Tabellenspalte, Filterfeld mit F4-Hilfe, ObjectPage-Abschnitt, Status-Ampel, Vorfilter-Tab, Währungsspalte, App-Checkliste, Client-Button, Navigation, Rating/Progress-Spalte, Code+Text kombiniert, Backend-Action mit Mock, neue App von Grund auf, Kollektions-Facet, OVP Analytical Card, KPI-Kachel, i18n | eine konkrete, häufige Aufgabe ansteht — **hier zuerst nachsehen**, bevor einzelne Annotationen nachgeschlagen werden |
| `references/odata-datamodel.md` | OData-Datenmodell-Grundlagen: EntityType, EntitySet, Key, Property-Typen, NavigationProperty (inkl. V4-vs.-V2-Syntax), EntityContainer, Kardinalitäten, SAP-spezifische V2-Attribute, `metadata.xml`-Skelett | das zugrundeliegende Datenmodell bzw. `metadata.xml` selbst Thema ist |
| `references/custom-column-microprocessflow.md` | Vollständiges Praxisbeispiel für eine Custom Column mit XML-Fragment, Formatter und Controller Extension (`sap.suite.ui.commons.MicroProcessFlow`, `InfoLabel`, `GenericTag`, mehrere Custom Columns gleichzeitig) | eine eigene Custom-Column mit Formatter-Logik gebaut werden soll |
| `references/ui5-controls.md` | Kurzreferenz der wichtigsten SAPUI5-Controls (Display, Input, Layout, Navigation/Feedback, sap.suite.ui.commons) mit Link zur offiziellen Doku | ein passendes UI5-Control für ein Fragment gesucht wird |

**Faustregel:**
- Konkrete Annotation gesucht → zuerst `ui-vocabulary.md` (bzw. `common-aggregation-measures-vocabulary.md` für `Common.*`/`Aggregation.*`/`Measures.*`).
- Konkrete Aufgabe ("füge X hinzu") → zuerst `cookbook.md`.
- Etwas funktioniert nicht → zuerst `troubleshooting.md`.
- Bei mehreren Themen: nur die tatsächlich relevanten Dateien öffnen, nicht alle auf Vorrat laden.
