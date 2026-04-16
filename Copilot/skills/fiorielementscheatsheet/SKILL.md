---
name: fiorielementscheatsheet
description: Cheatsheet für SAP Fiori Elements XML-Annotationen und ABAP CDS-Äquivalente
---

# SAP Fiori Elements – Annotation Cheatsheet


> Kurz-Referenz für XML-Annotationen (annotation.xml) **und** ABAP CDS-Äquivalente.

---

## Inhaltsverzeichnis

0. [Wie Annotationen funktionieren](#0-wie-annotationen-funktionieren)
   - [OData V4 vs. V2](#odata-v4-vs-v2--unterschiede-für-annotationen)
   - [Annotation-Dateien und Speicherort](#annotation-dateien-und-ihr-speicherort)
   - [Lokale vs. Remote-Annotationen](#lokale-vs-remote-annotationen)
   - [Anatomie einer Annotation](#anatomie-einer-annotation-xml)
   - [Target: Woran kann annotiert werden?](#target-woran-kann-annotiert-werden)
   - [Qualifier: Mehrere Annotationen desselben Terms](#qualifier-mehrere-annotationen-desselben-terms)
   - [Wert-Typen in Annotationen](#wert-typen-in-annotationen)
   - [Wie Fiori Elements Annotationen auswertet](#wie-fiori-elements-annotationen-auswertet)
   - [Annotation-Datei vs. metadata.xml](#annotation-datei-vs-metadataxml--was-gehört-wohin)
   - [Path vs. AnnotationPath vs. PropertyPath](#path-vs-annotationpath-vs-propertypath)
1. [Überblick: Wo welche Annotation wirkt](#1-überblick-wo-welche-annotation-wirkt)
2. [Projekt-Matrix: Welches Projekt nutzt welche Annotation](#2-projekt-matrix)
3. [UI Vocabulary](#3-ui-vocabulary)
   - [UI.HeaderInfo](#uiheaderinfo)
   - [UI.HeaderFacets](#uiheaderfacets)
   - [UI.LineItem & DataField-Typen](#uilineitem--datafield-typen)
   - [UI.SelectionFields](#uiselectionfields)
   - [UI.SelectionVariant](#uiselectionvariant)
   - [UI.PresentationVariant](#uipresentationvariant)
   - [UI.SelectionPresentationVariant](#uiselectionpresentationvariant)
   - [UI.Facets & ReferenceFacet](#uifacets--referencefacet)
   - [UI.FieldGroup](#uifieldgroup)
   - [UI.DataPoint](#uidatapoint)
   - [UI.Chart](#uichart)
   - [UI.KPI](#uikpi)
   - [UI.Identification](#uiidentification)
   - [UI.TextArrangement](#uitextarrangement)
   - [UI.Importance](#uiimportance)
   - [UI.Hidden & UI.HiddenFilter](#uihidden--uihiddenfilter)
   - [UI.Criticality](#uicriticality)
   - [UI.CriticalityCalculation](#uicriticalitycalculation)
   - [UI.MultiLineText](#uilinetext)
   - [UI.DataFieldWithUrl](#uidatafieldwithurl)
   - [UI.ConnectedFields](#uiconnectedfields)
   - [UI.QuickViewFacets](#uiquickviewfacets)
   - [UI.Badge](#uibadge)
   - [UI.RecommendedVisualization](#uirecommendedvisualization)
4. [Common Vocabulary](#4-common-vocabulary)
   - [Common.Text](#commontext)
   - [Common.Label](#commonlabel)
   - [Common.SemanticObject](#commonsemantcobject)
   - [Common.ValueList](#commonvaluelist)
   - [Common.ValueListWithFixedValues](#commonvaluelistwithfixedvalues)
5. [Aggregation Vocabulary](#5-aggregation-vocabulary)
   - [Aggregation.ApplySupported](#aggregationapplysupported)
   - [Aggregation.CustomAggregate](#aggregationcustomaggregate)
6. [Measures Vocabulary](#6-measures-vocabulary)
   - [Measures.ISOCurrency](#measuresissocurrency)
 7. [Semantic Object Navigation (App-zu-App)](#7-semantic-object-navigation-app-zu-app-navigation)
    - [7.1 Common.SemanticObject — Globale Property-Annotation](#71-commonsemanticobject--globale-property-annotation)
    - [7.2 UI.DataFieldWithIntentBasedNavigation — Semantic Link als Hyperlink](#72-uidatafieldwithintentbasednavigation--semantic-link-als-hyperlink)
    - [7.3 UI.DataFieldForIntentBasedNavigation — Semantic Button](#73-uidatafieldforintentbasednavigation--semantic-button)
    - [7.4 Common.SemanticObjectMapping — Parameter-Mapping](#74-commonsemanticobjectmapping--parameter-mapping)
    - [7.5 manifest.json — Cross Navigation Konfiguration](#75-manifestjson--cross-navigation-konfiguration)
    - [7.6 Programmatic Navigation per Controller Extension](#76-programmatic-navigation-per-controller-extension)
    - [7.7 Vergleich: Alle Navigations-Varianten](#77-vergleich-alle-navigations-varianten)
    - [7.8 Typische Fehler & Troubleshooting](#78-typische-fehler--troubleshooting)
8. [UI.DataFieldForAction (eigenständig)](#8-uidatafieldfraction-eigenständig)
   - [Variante A: Backend (OData-Action)](#variante-a-backend-odata-action)
   - [Variante B: Client-seitig (Controller-Extension)](#variante-b-client-seitig-controller-extension)
   - [Entscheidungshilfe A vs. B](#variante-a-vs-b--entscheidungshilfe)
   - [Echte Inline-Row-Buttons (Custom Column)](#echte-inline-row-buttons-alternative-zu-datafieldfaction)
9. [Controller Extensions – Vollreferenz](#9-controller-extensions--vollreferenz)
   - [Was ist eine Controller Extension?](#was-ist-eine-controller-extension)
   - [Registrierung in manifest.json](#registrierung-in-manifestjson)
   - [Die Extension-Datei](#die-extension-datei)
   - [override-Block: Lifecycle-Hooks](#override-block-lifecycle-hooks)
   - [OData Backend-Action aufrufen](#odata-backend-action-aus-der-extension-aufrufen)
   - [Fehlerquellen & Troubleshooting](#fehlerquellen--troubleshooting-controller-extensions)
10. [ABAP CDS – Äquivalente Syntax](#10-abap-cds--äquivalente-syntax)
11. [OVP-spezifische Annotationen](#11-ovp-spezifische-annotationen)
12. [Annotation-Verbindungsdiagramme](#12-annotation-verbindungsdiagramme)
13. [Vocabularies & Namespaces – Referenz](#13-vocabularies--namespaces--referenz)
14. [Mockdaten – Vollreferenz](#14-mockdaten--vollreferenz)
    - [Dateistruktur & Format](#dateistruktur--format)
    - [OData V4 Datentypen in JSON](#odata-v4-datentypen-in-json)
    - [Navigationseigenschaften mocken](#navigationseigenschaften-mocken)
    - [ui5-mock.yaml Konfiguration](#ui5-mockyaml-konfiguration)
    - [Aggregation & Charts mocken](#aggregation--charts-mocken)
    - [OData Actions mocken](#odata-actions-mocken)
    - [Häufige Mock-Fehler](#häufige-mock-fehler)
    - [Umstieg Mock → echter OData-Service](#umstieg-mock--echter-odata-service)
15. [Häufige Fehler & Troubleshooting](#15-häufige-fehler--troubleshooting)
16. [Schnell-Rezepte (Cookbook)](#16-schnell-rezepte-cookbook)
    - [Rezept 1: Neue Tabellenspalte](#rezept-1-neue-tabellenspalte-hinzuf%C3%BCgen)
    - [Rezept 2: Filterfeld mit F4-Hilfe](#rezept-2-neues-filterfeld-mit-f4-hilfe)
    - [Rezept 3: ObjectPage-Abschnitt](#rezept-3-neuen-objectpage-abschnitt-hinzuf%C3%BCgen)
    - [Rezept 4: Status-Ampel](#rezept-4-status-ampel-in-tabellenspalte)
    - [Rezept 5: Vorfilter-Tab](#rezept-5-vorfilter-tab-mit-selectionpresentationvariant)
    - [Rezept 6: Währungsspalte](#rezept-6-w%C3%A4hrungsspalte-mit-automatischer-einheit)
    - [Rezept 7: Checkliste neue App](#rezept-7-annotations-schnell-checkliste-f%C3%BCr-neue-app)
    - [Rezept 8: Client-Button (Controller Extension)](#rezept-8-client-button-controller-extension)
    - [Rezept 9: Navigation ListReport → ObjectPage](#rezept-9-navigation-listreport--objectpage)
    - [Rezept 10: Rating-Sterne und Progress-Bar](#rezept-10-rating-sterne-und-progress-bar-als-spalte)
    - [Rezept 11: Code + Text kombiniert](#rezept-11-code--text-kombiniert-anzeigen)
    - [Rezept 12: Backend OData-Action mit Mock](#rezept-12-backend-odata-action-mit-mock-handler)
    - [Rezept 13: Neue App komplett aufsetzen](#rezept-13-neue-app-von-grund-auf--schritt-f%C3%BCr-schritt)
    - [Rezept 14: Kollektions-Facet (zwei FieldGroups nebeneinander)](#rezept-14-kollektions-facet-zwei-fieldgroups-nebeneinander)
    - [Rezept 15: OVP Analytical Card komplett aufsetzen](#rezept-15-ovp-analytical-card-komplett-aufsetzen)
    - [Rezept 16: KPI-Kachel im ObjectPage-Header](#rezept-16-kpi-kachel-im-objectpage-header)
    - [Rezept 17: Mehrsprachige App (i18n)](#rezept-17-mehrsprachige-app-i18n-für-annotations-labels)
17. [Custom Column mit XML-Fragment & Formatter (MicroProcessFlow)](#17-custom-column-mit-xml-fragment--formatter-microprocessflow)
    - [17.1 Übersicht: Alle beteiligten Dateien](#171-übersicht-alle-beteiligten-dateien)
    - [17.2 metadata.xml – Navigation Property definieren](#172-metadataxml--navigation-property-definieren)
    - [17.3 Mock-Daten – SalesOrderSteps.json](#173-mock-daten--salesorderstepsjson)
    - [17.4 Das XML-Fragment](#174-das-xml-fragment)
    - [17.5 Formatter-Syntax im Fragment – detaillierte Erklärung](#175-formatter-syntax-im-fragment--detaillierte-erklärung)
    - [17.6 Controller Extension – vollständige Datei](#176-controller-extension--vollständige-datei)
    - [17.7 manifest.json – Registrierung (3 Pflicht-Einträge)](#177-manifestjson--registrierung-3-pflicht-einträge)
    - [17.8 Vollständiger Datenfluss](#178-vollständiger-datenfluss)
    - [17.9 Häufige Fehler](#179-häufige-fehler)
    - [17.10 Rezept: MicroProcessFlow Custom Column in 10 Schritten](#1710-rezept-microprocessflow-custom-column-in-10-schritten)
    - [17.11 Custom Column: InfoLabel (sap.tnt)](#1711-custom-column-infolabel-sapttnt)
    - [17.12 Custom Column: GenericTag (sap.m)](#1712-custom-column-generictag-sapm)
    - [17.13 Mehrere Custom Columns gleichzeitig (controlConfiguration)](#1713-mehrere-custom-columns-gleichzeitig-controlconfiguration)
18. [OData Datenmodell – Grundlagen Cheatsheet](#18-odata-datenmodell--grundlagen-cheatsheet)
    - [18.1 EntityType – die Datentabelle](#181-entitytype--die-datentabelle)
    - [18.2 EntitySet – der Endpunkt](#182-entityset--der-endpunkt)
    - [18.3 Key & Schlüsseleigenschaften](#183-key--schlüsseleigenschaften)
    - [18.4 Property-Typen Referenz](#184-property-typen-referenz)
    - [18.5 NavigationProperty – Beziehungen zwischen Entities](#185-navigationproperty--beziehungen-zwischen-entities)
    - [18.6 V4 vs. V2: NavigationProperty Syntax](#186-v4-vs-v2-navigationproperty-syntax)
    - [18.7 EntityContainer – Alles zusammen](#187-entitycontainer--alles-zusammen)
    - [18.8 Kardinalitäten (Multiplicity)](#188-kardinalitäten-multiplicity)
    - [18.9 SAP-spezifische Attribute (V2)](#189-sap-spezifische-attribute-v2)
    - [18.10 Schnell-Referenz: metadata.xml Skelett](#1810-schnell-referenz-metadataxml-skelett)
19. [Must-Have UI5 Controls – Referenz](#19-must-have-ui5-controls--referenz)
    - [19.1 Display Controls](#191-display-controls)
    - [19.2 Input Controls](#192-input-controls)
    - [19.3 Layout Controls](#193-layout-controls)
    - [19.4 Navigation & Feedback Controls](#194-navigation--feedback-controls)
    - [19.5 sap.suite.ui.commons Controls](#195-sapsuiteui-commons-controls)

---

## 0. Wie Annotationen funktionieren

### Was ist eine Annotation?

Eine **Annotation** ist Metadaten, die an ein OData-Element (EntityType, EntitySet oder Property) angehängt werden. Sie enthalten **keine Geschäftslogik** — sie beschreiben nur, *wie* ein Feld oder eine Entität in der UI dargestellt werden soll. Das Fiori Elements Framework liest diese Annotationen zur Laufzeit und generiert daraus automatisch die UI.

```
  Daten (OData)         Annotationen              Fiori Elements UI
  ─────────────         ────────────              ─────────────────
  EntityType            UI.LineItem          →    Tabelle mit Spalten
  Product ────────────► UI.HeaderInfo        →    ObjectPage-Titel
    ProductName         UI.DataPoint         →    Fortschrittsbalken
    Stock               Common.ValueList     →    F4-Hilfe Dropdown
    Rating              UI.Chart             →    Diagramm
```

> **Kernprinzip:** Man schreibt keine Views, keine Controller, kein HTML. Man annotiert die Daten — das Framework baut die UI.

---

### OData V4 vs. V2 – Unterschiede für Annotationen

| Merkmal | OData V2 | OData V4 |
|---|---|---|
| Annotation-Datei | getrennte `annotation.xml` | getrennte `annotation.xml` |
| `$apply` (Aggregation) | ❌ nicht unterstützt | ✅ unterstützt |
| `$batch` | ✅ | ✅ |
| Bound Actions | ❌ (nur Unbound über FunctionImport) | ✅ |
| Fiori Elements Version | V2 Templates (veraltet) | V4 Templates (aktuell) |
| manifest.json dataSource `type` | `"OData"` + `"settings": {"odataVersion": "2.0"}` | `"OData"` + `"settings": {"odataVersion": "4.0"}` |
| Chart-Unterstützung | Eingeschränkt (SmartChart) | Vollständig ($apply) |
| Controller Extensions | `sap.ui.core.mvc.ControllerExtension` | `sap.ui.core.mvc.ControllerExtension` |

> Alle Projekte in diesem Workspace nutzen **OData V4**. Annotationen sind in V4 und V2 identisch — der Unterschied liegt im Runtime-Verhalten und den OData-Abfragen.

---

### Annotation-Dateien und ihr Speicherort

```
webapp/
  localService/
    <ServiceName>/
      metadata.xml          ← OData-Schema (EntityType, EntitySet, Actions)
                               Im echten SAP-System: automatisch vom Backend generiert
      annotation.xml        ← UI-Annotationen (vom Entwickler gepflegt)
  annotations/
    annotation.xml          ← Alternative: App-interne Annotationen (Projekt-abhängig)
```

**Registrierung in manifest.json:**
```json
"sap.app": {
  "dataSources": {
    "mainService": {
      "uri": "/sap/opu/odata4/sap/my_service/srvd/sap/my_service/0001/",
      "type": "OData",
      "settings": {
        "odataVersion": "4.0",
        "annotations": ["annotation"]   // ← Verweis auf die annotation-Datei
      }
    },
    "annotation": {
      "uri": "annotations/annotation.xml",
      "type": "ODataAnnotation"         // ← Typ muss ODataAnnotation sein
    }
  }
}
```

> **Mehrere Annotation-Dateien:** Man kann mehrere `ODataAnnotation`-Dateien registrieren (z.B. eine für UI, eine für Common-Annotations). Bei Konflikt gewinnt die zuletzt geladene Datei.

---

### Lokale vs. Remote-Annotationen

| Art | Speicherort | Wann |
|---|---|---|
| **App-interne Annotation** | `webapp/annotations/annotation.xml` | Lokale Entwicklung, Mock-Betrieb |
| **Backend-Annotation** | Wird vom OData-Service über `$metadata` geliefert | Produktivsystem (Inline Annotations) |
| **SAP Gateway-Annotation** | SAP Gateway Annotation Editor | ABAP-Backend mit Gateway |
| **CDS-MDE-Datei** | `.ddlx`-Datei im ABAP-System | RAP / ABAP CDS (moderner Ansatz) |

---

### Anatomie einer Annotation (XML)

```xml
<!-- ══════════════════════════════════════════════════════════════
     AUFBAU EINER ANNOTATION
     ══════════════════════════════════════════════════════════════ -->

<Annotations Target="MainService.Product">          <!-- ① Ziel -->
  <Annotation Term="UI.LineItem" Qualifier="TAB1">  <!-- ② Term + Qualifier -->
    <Collection>                                    <!-- ③ Wert -->
      <Record Type="UI.DataField">
        <PropertyValue Property="Value" Path="ProductName" />
      </Record>
    </Collection>
  </Annotation>
</Annotations>
```

| Teil | Bedeutung |
|---|---|
| **Target** | Woran wird annotiert? Entity, EntitySet oder eine Property |
| **Term** | Was wird ausgesagt? Der Vocabulary-Begriff (z.B. `UI.LineItem`) |
| **Qualifier** | Unterscheidet mehrere Annotationen desselben Terms am gleichen Ziel |
| **Wert** | Der eigentliche Inhalt: String, Bool, Int, Path, Record, Collection |

---

### Target: Woran kann annotiert werden?

```
  OData-Modell                         Target-Syntax
  ────────────                         ─────────────

  EntityType  "Product"           →    "MainService.Product"
  Property    "Price" on Product   →    "MainService.Product/Price"
  EntitySet   "Products" im       →    "MainService.MainContainer/Products"
              EntityContainer
```

**Beispiel – alle drei Targets im Vergleich:**
```xml
<!-- EntityType: betrifft die ganze Entität (Tabelle, ObjectPage) -->
<Annotations Target="MainService.Product">
  <Annotation Term="UI.HeaderInfo"> ... </Annotation>
  <Annotation Term="UI.LineItem">   ... </Annotation>
  <Annotation Term="UI.Facets">     ... </Annotation>
</Annotations>

<!-- Property: betrifft ein einzelnes Feld -->
<Annotations Target="MainService.Product/Price">
  <Annotation Term="Common.Label"       String="Verkaufspreis" />
  <Annotation Term="Measures.ISOCurrency" Path="Currency" />
</Annotations>

<!-- EntitySet: betrifft den OData-Endpunkt (z.B. für Aggregation/Charts) -->
<Annotations Target="MainService.MainContainer/SalesOrders">
  <Annotation Term="Aggregation.ApplySupported"> ... </Annotation>
</Annotations>
```

---

### Qualifier: Mehrere Annotationen desselben Terms

Ohne Qualifier kann ein Term nur **einmal** an einem Ziel vorkommen. Mit Qualifier beliebig oft:

```xml
<Annotations Target="MainService.Order">

  <!-- Default LineItem (kein Qualifier) → OVP Table Card -->
  <Annotation Term="UI.LineItem">
    <Collection> ... </Collection>
  </Annotation>

  <!-- Zweite LineItem-Variante für List Card -->
  <Annotation Term="UI.LineItem" Qualifier="Priority">
    <Collection> ... </Collection>
  </Annotation>

  <!-- Dritte Variante für eine spezifische Ansicht -->
  <Annotation Term="UI.LineItem" Qualifier="AllCases">
    <Collection> ... </Collection>
  </Annotation>

</Annotations>
```

Referenz im Code: `@UI.LineItem#Priority` (Raute = Qualifier-Trennzeichen)

---

### Wert-Typen in Annotationen

```xml
<!-- String-Wert -->
<Annotation Term="Common.Label" String="Verkaufspreis" />

<!-- Boolean-Wert -->
<Annotation Term="Common.ValueListWithFixedValues" Bool="true" />

<!-- Integer-Wert -->
<PropertyValue Property="TargetValue" Int="1000" />

<!-- Decimal-Wert -->
<PropertyValue Property="TargetValue" Decimal="4.5" />

<!-- Path: dynamischer Verweis auf eine Property des gleichen EntityType -->
<PropertyValue Property="Value"    Path="ProductName" />
<PropertyValue Property="Criticality" Path="StockLevel" />   <!-- Feld liefert 1/2/3 -->

<!-- AnnotationPath: Verweis auf eine andere Annotation -->
<PropertyValue Property="Target" AnnotationPath="@UI.DataPoint#StockDataPoint" />

<!-- PropertyPath: Verweis auf eine Property (für Listen) -->
<PropertyPath>ProductName</PropertyPath>

<!-- EnumMember: ein Wert aus einer Aufzählung -->
<PropertyValue Property="ChartType" EnumMember="UI.ChartType/Donut" />

<!-- Collection: Liste von Werten/Records -->
<Collection>
  <PropertyPath>Name</PropertyPath>
  <PropertyPath>Category</PropertyPath>
</Collection>

<!-- Record: strukturierter Wert mit benannten Properties -->
<Record Type="UI.DataField">
  <PropertyValue Property="Label" String="Name" />
  <PropertyValue Property="Value" Path="ProductName" />
</Record>

<!-- i18n-Binding (OVP-spezifisch, kein Standard-OData) -->
<PropertyValue Property="Label" String="{@i18n>myKey}" />
```

---

### Wie Fiori Elements Annotationen auswertet

```
  Browser lädt App
       │
       ▼
  manifest.json         ← Welches EntitySet? Welche Annotation(Qualifier)?
       │                   Welche Routen / Views?
       ▼
  metadata.xml          ← Was sind die Datentypen? (Edm.String, Edm.Decimal...)
  + annotation.xml      ← Was soll wie dargestellt werden?
       │
       ▼
  Fiori Elements        ← Generiert zur Laufzeit:
  Framework                • Filter-Bar aus UI.SelectionFields
                           • Tabellen-Spalten aus UI.LineItem
                           • Charts aus UI.Chart + ApplySupported
                           • ObjectPage-Abschnitte aus UI.Facets
                           • F4-Dialoge aus Common.ValueList
       │
       ▼
  OData-Service         ← Anfragen: GET /Products?$select=...&$apply=...
```

---

### Annotation-Datei vs. metadata.xml — was gehört wohin?

| Was | Datei | Grund |
|---|---|---|  
| `UI.*`, `Common.*`, `Measures.*` | `annotation.xml` | UI-Metadaten, vom Entwickler gepflegt |
| `Aggregation.ApplySupported` | `metadata.xml` | Muss vom OData-Service deklariert werden |
| `Aggregation.CustomAggregate` | `metadata.xml` | Technisches Capability-Flag des Services |
| Entitätsstruktur, Schlüssel, Datentypen | `metadata.xml` | OData-Schema — wird vom Backend geliefert |

> In einem **echten SAP-System** liefert der Backend-Service die metadata.xml automatisch. Die annotation.xml wird vom Entwickler in der App oder in einer separaten Annotation-App gepflegt. Im Mock-Betrieb (lokalem Entwickeln) liegen beide Dateien im `localService/`-Ordner.

---

### Path vs. AnnotationPath vs. PropertyPath

| Wert-Typ | Verwendet in | Zeigt auf |
|---|---|---|
| `Path="PropertyName"` | `PropertyValue` | Eine **Property** des gleichen EntityType |
| `Path="Nav/Property"` | `PropertyValue` | Property über **Navigation** (z.B. `to_Status/Text`) |
| `AnnotationPath="@UI.X#Q"` | `PropertyValue`, `Target` | Eine andere **Annotation** am gleichen Entity |
| `PropertyPath>Name</PropertyPath>` | Collection-Einträge | Eine **Property** (ohne Wert, nur Verweis) |

```xml
<!-- Path: Feld-Wert lesen -->
<PropertyValue Property="Value" Path="ProductName" />

<!-- Path über Navigation: Text aus verknüpfter Entity -->
<Annotation Term="Common.Text" Path="to_Status/StatusText" />

<!-- AnnotationPath: andere Annotation referenzieren -->
<PropertyValue Property="Target" AnnotationPath="@UI.DataPoint#StockDataPoint" />

<!-- PropertyPath: Feld in einer Liste benennen -->
<PropertyValue Property="Dimensions">
  <Collection>
    <PropertyPath>Category</PropertyPath>
  </Collection>
</PropertyValue>
```

---

## 1. Überblick: Wo welche Annotation wirkt

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

---

## 2. Projekt-Matrix

| Annotation | `fiorilistreport` | `fiorielements` | `fioriovp` | `problemsolvingovp` |
|---|:---:|:---:|:---:|:---:|
| `UI.HeaderInfo` | ✅ | ✅ | ✅ | – |
| `UI.HeaderFacets` | – | ✅ | – | – |
| `UI.LineItem` | ✅ | ✅ | ✅ | ✅ |
| `UI.SelectionFields` | ✅ | ✅ | ✅ | ✅ |
| `UI.SelectionVariant` | ✅ (via SPV) | ✅ | ✅ | ✅ |
| `UI.PresentationVariant` | ✅ (via SPV) | ✅ | ✅ | ✅ |
| `UI.SelectionPresentationVariant` | ✅ | – | – | – |
| `UI.Facets` | ✅ | ✅ | ✅ (mit Qualifier) | – |
| `UI.FieldGroup` | ✅ | ✅ | ✅ | – |
| `UI.DataPoint` | ✅ (Progress+Rating) | ✅ (Progress+Rating+Number) | – | ✅ (Number) |
| `UI.Chart` | ✅ (Bar) | ✅ (Donut) | ✅ (Donut) | ✅ (Bar+Donut) |
| `UI.KPI` | – | ✅ | – | – |
| `UI.Identification` | – | ✅ | ✅ (mit Qualifier) | ✅ |
| `UI.TextArrangement` | – | – | – | ✅ |
| `UI.Importance` | – | – | – | ✅ |
| `Common.Text` | – | – | – | ✅ |
| `Common.Label` | – | ✅ | ✅ | – |
 | `Common.SemanticObject` | ✅ (Referenz) | – | – | ✅ |
| `Common.ValueList` | – | ✅ | ✅ | ✅ |
| `Common.ValueListWithFixedValues` | – | – | ✅ | ✅ |
| `Aggregation.ApplySupported` | ✅ | – | – | – |
| `Aggregation.CustomAggregate` | ✅ | – | – | – |
| `Measures.ISOCurrency` | – | ✅ | – | – |
| `DataFieldForIntentBasedNavigation` | – | – | – | ✅ |
| `UI.Hidden` | – | ✅ | – | – |
| `UI.HiddenFilter` | – | – | – | – |
| `UI.Criticality` | ✅ | ✅ | – | ✅ |
| `UI.CriticalityCalculation` | ✅ | – | – | – |
| `UI.MultiLineText` | – | ✅ | – | – |
 | `UI.DataFieldWithUrl` | ✅ | – | – | – |
 | `UI.DataFieldWithIntentBasedNavigation` | – | – | – | – |
| `UI.ConnectedFields` | – | – | – | – |
| `UI.QuickViewFacets` | – | – | – | – |
| `UI.Badge` | – | – | ✅ | – |
| `UI.RecommendedVisualization` | – | – | – | – |

---

## 3. UI Vocabulary

> Namespace: `com.sap.vocabularies.UI.v1` | Alias: `UI`

---

### UI.HeaderInfo

**Zweck:** Definiert den Kopfbereich einer ObjectPage oder OVP-Karte: Typ-Name, Haupttitel, Beschreibung, optionales Bild.

**Target:** `EntityType` (z.B. `MainService.Product`) — wirkt auf die gesamte Entität, nicht auf eine einzelne Property.

**Projekte:** `fiorilistreport`, `fiorielements`, `fioriovp`

#### Properties von UI.HeaderInfoType

| Property | Typ | Pflicht | Bedeutung |
|---|---|:---:|---|
| `TypeName` | String | ✅ | Singular-Name der Entität (z.B. "Produkt") |
| `TypeNamePlural` | String | ✅ | Plural-Name (z.B. "Produkte") |
| `Title` | DataField | ✅ | Hauptüberschrift (Path auf Property) |
| `Description` | DataField | – | Untertitel / Zusatzinfo |
| `ImageUrl` | Path/String | – | URL für Produktbild (Path auf String-Property) |
| `Initials` | String | – | Kürzel wenn kein Bild vorhanden (z.B. "PR") |

#### XML-Syntax
```xml
<Annotations Target="MyService.Product">
  <Annotation Term="UI.HeaderInfo">
    <Record Type="UI.HeaderInfoType">
      <!-- Anzeigename für Entitätstyp (Singular) -->
      <PropertyValue Property="TypeName"       String="Product" />
      <!-- Anzeigename (Plural) -->
      <PropertyValue Property="TypeNamePlural" String="Products" />
      <!-- Haupttitel der Karte / Seite -->
      <PropertyValue Property="Title">
        <Record Type="UI.DataField">
          <PropertyValue Property="Value" Path="ProductName" />
        </Record>
      </PropertyValue>
      <!-- Untertitel / Beschreibung -->
      <PropertyValue Property="Description">
        <Record Type="UI.DataField">
          <PropertyValue Property="Value" Path="ProductID" />
        </Record>
      </PropertyValue>
      <!-- Bild-URL (optional) -->
      <PropertyValue Property="ImageUrl" Path="ImageURL" />
    </Record>
  </Annotation>
</Annotations>
```

#### Mit i18n-Binding (OVP – fioriovp)
```xml
<PropertyValue Property="TypeName" String="{@i18n>entityOrder}" />
```

#### CDS-Äquivalent
```abap
@UI.headerInfo: {
  typeName:        'Product',
  typeNamePlural:  'Products',
  title.value:     'ProductName',
  description.value: 'ProductID'
}
define view ZC_Product as select from ...
```

#### Visuelle Wirkung
```
┌────────────────────────────────────────────┐
│ [Bild]  ProductName  (TypeName: Product)   │
│         ProductID (Description)            │
└────────────────────────────────────────────┘
```

#### Initials – Fallback wenn kein Bild vorhanden

```xml
<!-- Wenn ImageUrl leer/null ist, wird stattdessen Initials angezeigt (Avatar-Kürzel) -->
<PropertyValue Property="ImageUrl"  Path="ImageURL" />
<PropertyValue Property="Initials"  String="PR" />
```

```
  Mit Bild:          Ohne Bild (ImageURL = null):
  ┌─────────────┐    ┌─────────────┐
  │  [Foto]  PR │    │    [PR]  PR  │
  │  Produkt    │    │  Produkt    │
  └─────────────┘    └─────────────┘
```

#### OVP-Karte vs. ObjectPage — Unterschied

| Kontext | Wirkung |
|---|---|
| **ObjectPage** | TypeName erscheint im Breadcrumb-Pfad oben links. Title = Seitentitel (H1). Description = Untertitel. |
| **OVP Table/List Card** | TypeName erscheint als Karten-Typ-Label. Title = Hauptzeile jeder Karte (fett). Description = Unterzeile. |
| **OVP Stack Card** | Title = Kartenkopf-Hauptzeile. Description = zweite Zeile. |

#### Typische Fehler bei UI.HeaderInfo

| Fehler | Ursache | Lösung |
|---|---|---|
| ObjectPage zeigt "undefined" als Titel | `Path="ProductName"` zeigt auf Property die nicht geladen wird | Property zu `$select` hinzufügen oder `$expand` prüfen |
| TypeName fehlt im Breadcrumb | `TypeName` oder `TypeNamePlural` leer | Pflichtfelder befüllen |
| Bild wird nicht angezeigt | `ImageUrl` = relativer Pfad statt absoluter URL | Absoluter URL oder Base64-Data-URL verwenden |
| Initials werden nie angezeigt | `Initials` wird ignoriert wenn `ImageUrl` einen Wert hat (auch wenn Bild nicht lädt) | Im Mock: ImageURL-Feld leer lassen |

---

### UI.HeaderFacets

**Zweck:** KPI-Kacheln im Kopfbereich einer ObjectPage. Jede Kachel zeigt einen `UI.DataPoint` oder `UI.KPI`.

**Target:** `EntityType` — wirkt auf den Header-Bereich der ObjectPage dieser Entität.

**Projekt:** `fiorielements`

> **Unterschied zu `UI.Facets`:** HeaderFacets erscheinen **oberhalb** der Section-Tabs im schmalen Streifen unter dem Titel. Sie können keine FieldGroups anzeigen, nur DataPoints und KPIs.

#### XML-Syntax
```xml
<Annotations Target="ProductService.Product">
  <Annotation Term="UI.HeaderFacets">
    <Collection>
      <!-- Kachel 1: Stock-Fortschrittsbalken -->
      <Record Type="UI.ReferenceFacet">
        <PropertyValue Property="Label"  String="Stock" />
        <PropertyValue Property="Target" AnnotationPath="@UI.DataPoint#StockDataPoint" />
      </Record>
      <!-- Kachel 2: Preis-Bewertungssterne -->
      <Record Type="UI.ReferenceFacet">
        <PropertyValue Property="Label"  String="Price Rating" />
        <PropertyValue Property="Target" AnnotationPath="@UI.DataPoint#PriceRatingDataPoint" />
      </Record>
    </Collection>
  </Annotation>
</Annotations>
```

#### Visuelle Wirkung
```
┌───────────────────────────────────────────────────────────┐
│ Header                                                    │
│ ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌──────────┐  │
│ │  Stock   │  │  Price   │  │  Rating  │  │  Orders  │  │
│ │ [=====  ]│  │ ★★★★☆   │  │  KPI #   │  │  KPI #   │  │
│ └──────────┘  └──────────┘  └──────────┘  └──────────┘  │
└───────────────────────────────────────────────────────────┘
```

> **Hinweis:** HeaderFacets zeigen **keine** FieldGroups, sondern nur DataPoints oder KPIs.

---

### UI.LineItem & DataField-Typen

**Zweck:** Definiert die Spalten einer Tabelle (ListReport, OVP Table-/List-Karte).

**Target:** `EntityType` (z.B. `MainService.Product`) — alle Records in der Collection werden als Tabellenspalten gerendert. Mit Qualifier können mehrere Tabellenvarianten definiert werden.

**Projekte:** Alle 4 Projekte

> **Reihenfolge der Spalten:** Die Reihenfolge der `<Record>`-Einträge in der `<Collection>` bestimmt die Spaltenreihenfolge von links nach rechts. In CDS steuert das `position`-Attribut die Reihenfolge.

#### XML-Syntax (Überblick aller DataField-Typen)
```xml
<Annotations Target="MyService.Product">
  <Annotation Term="UI.LineItem" Qualifier="TAB1">
    <Collection>

      <!-- ① Standard-Textspalte -->
      <Record Type="UI.DataField">
        <PropertyValue Property="Label" String="Name" />
        <PropertyValue Property="Value" Path="ProductName" />
      </Record>

      <!-- ② Verweis auf einen DataPoint (Progress / Rating / Number) -->
      <Record Type="UI.DataFieldForAnnotation">
        <PropertyValue Property="Label"  String="Stock Level" />
        <PropertyValue Property="Target" AnnotationPath="@UI.DataPoint#StockDataPoint" />
      </Record>

      <!-- ③ Button, der eine OData-Action auslöst -->
      <Record Type="UI.DataFieldForAction">
        <PropertyValue Property="Label"  String="Approve" />
        <PropertyValue Property="Action" String="ProductService.approve" />
      </Record>

      <!-- ④ Intent-basierte Navigation (externe App) -->
      <Record Type="UI.DataFieldForIntentBasedNavigation">
        <PropertyValue Property="Label"         String="In App öffnen" />
        <PropertyValue Property="SemanticObject" String="QM_ProbSolvingProcess" />
        <PropertyValue Property="Action"         String="display" />
        <PropertyValue Property="Mapping">
          <Collection>
            <Record Type="Common.SemanticObjectMappingType">
              <PropertyValue Property="LocalProperty"         PropertyPath="ProbSolvingProc" />
              <PropertyValue Property="SemanticObjectProperty" String="ProbSolvingProc" />
            </Record>
          </Collection>
        </PropertyValue>
        <Annotation Term="UI.Importance" EnumMember="UI.ImportanceType/High"/>
      </Record>

    </Collection>
  </Annotation>
</Annotations>
```

#### CDS-Äquivalent
```abap
-- Standard-Spalte:
@UI.lineItem: [{ qualifier: 'TAB1', position: 10, label: 'Product Name' }]
ProductName,

-- DataPoint-Verweis (Progress-Bar):
@UI.lineItem: [{ qualifier: 'TAB1', position: 40,
                 type: #AS_DATAPOINT, valueQualifier: 'StockDataPoint', label: 'Stock' }]
Stock,
```

#### DataField-Typen auf einen Blick
| Record Type | Zweck | Schlüsselfelder |
|---|---|---|
| `UI.DataField` | Normaler Text-/Zahlenwert | `Value` (Path) |
| `UI.DataFieldForAnnotation` | Verweis auf DataPoint/Chart | `Target` (AnnotationPath) |
| `UI.DataFieldForAction` | OData-Action-Button | `Action` (String) |
| `UI.DataFieldForIntentBasedNavigation` | Ext. Navigation per SemanticObject | `SemanticObject`, `Action`, `Mapping` |

#### Navigation-Pfad in UI.DataField (über Association)

Der `Value`-Path kann über eine Navigation-Property gehen, um Felder **verknüpfter Entitäten** direkt anzuzeigen:

```xml
<!-- fiorielements: Produktname aus der assoziierten Product-Entity -->
<Record Type="UI.DataField">
  <PropertyValue Property="Label" String="Produkt" />
  <!-- "Product" ist eine NavigationProperty, "Name" das Zielfeld -->
  <PropertyValue Property="Value" Path="Product/Name" />
</Record>
```

> **Voraussetzung:** Der EntityType muss eine `NavigationProperty` mit diesem Namen in `metadata.xml` haben. Im Mock-Betrieb muss auch das $expand funktionieren.

---

### UI.SelectionFields

**Zweck:** Legt fest, welche Felder in der Filter-Bar der ListReport-Seite oder im OVP Global Filter sichtbar sind.

**Target:** `EntityType` — die aufgelisteten `PropertyPath`-Einträge müssen Properties dieses EntityType sein.

**Projekte:** Alle 4 Projekte

> **Wichtig:** Nur in `UI.SelectionFields` gelistete Felder erscheinen **standardmäßig** in der Filter-Bar. Der Benutzer kann weitere Felder manuell hinzufügen (über "Anpassen"). Mit `Common.ValueList` auf einer Property bekommt das Filterfeld automatisch einen F4-Hilfe-Button.

#### XML-Syntax
```xml
<Annotations Target="MyService.Product">
  <Annotation Term="UI.SelectionFields">
    <Collection>
      <PropertyPath>ProductName</PropertyPath>
      <PropertyPath>Category</PropertyPath>
      <PropertyPath>Price</PropertyPath>
    </Collection>
  </Annotation>
</Annotations>
```

#### CDS-Äquivalent
```abap
@UI.selectionField: [{ position: 10 }]
ProductName,

@UI.selectionField: [{ position: 20 }]
Category,
```

---

### UI.SelectionVariant

**Zweck:** Vorfilter-Definition mit fixen Filterwerten (SelectOptions). Wird für Tab-Vorfilter oder OVP-Karten-Filter genutzt.

**Target:** `EntityType` — definiert einen benannten Filter-Zustand ("Preset"). Wirkt **nicht** allein, sondern wird von einer anderen Annotation referenziert (z.B. `SelectionPresentationVariant` oder OVP-Karte in `manifest.json`).

**Projekte:** `fiorilistreport`, `fiorielements`, `fioriovp`, `problemsolvingovp`

> **Wie es funktioniert:** Das Fiori Elements Framework setzt beim Laden der Seite / Karte die in `SelectOptions` definierten Filter automatisch in die Filter-Bar ein. Der Benutzer sieht die Daten bereits vorgefiltert.

#### SelectionVariant vs. SelectionFields — der Unterschied
```
  UI.SelectionFields    → WELCHE Felder erscheinen in der Filter-Bar
  UI.SelectionVariant   → MIT WELCHEN WERTEN die Filter vorbelegt sind
```

#### XML-Syntax
```xml
<Annotation Term="UI.SelectionVariant" Qualifier="LowStock">
  <Record>
    <PropertyValue Property="Text" String="Wenig Vorrat" />
    <PropertyValue Property="SelectOptions">
      <Collection>
        <Record Type="UI.SelectOptionType">
          <PropertyValue Property="PropertyName" PropertyPath="Stock" />
          <PropertyValue Property="Ranges">
            <Collection>
              <Record Type="UI.SelectionRangeType">
                <!-- Sign: I = Include, E = Exclude -->
                <PropertyValue Property="Sign"   EnumMember="UI.SelectionRangeSignType/I" />
                <!-- Option: EQ, NE, LT, LE, GT, GE, BT (Between), CP (Contains Pattern) -->
                <PropertyValue Property="Option" EnumMember="UI.SelectionRangeOptionType/LE" />
                <!-- Vergleichswert -->
                <PropertyValue Property="Low"    Int="10" />
              </Record>
            </Collection>
          </PropertyValue>
        </Record>
      </Collection>
    </PropertyValue>
  </Record>
</Annotation>
```

#### SelectionRangeOptionType – Alle Operatoren
| Enum | Bedeutung |
|---|---|
| `EQ` | Equal (=) |
| `NE` | Not Equal (≠) |
| `LT` | Less Than (<) |
| `LE` | Less or Equal (≤) |
| `GT` | Greater Than (>) |
| `GE` | Greater or Equal (≥) |
| `BT` | Between (Low – High) |
| `CP` | Contains Pattern (Wildcard) |
| `NB` | Not Between |

#### Die 4 Verwendungskontexte von UI.SelectionVariant

```
  SelectionVariant wird in 4 verschiedenen Kontexten eingesetzt:

  ┌──────────────────────────────────────────────────────────────────────┐
  │  ① FILTER CHIPS (ListReport – fiorielements)                        │
  │     Mehrere SelectionVariants am gleichen EntityType →              │
  │     erscheinen als klickbare Schnellfilter-Chips über der Tabelle   │
  │     [ All ] [ Low Stock ] [ Electronics ] [ High Price ]            │
  ├──────────────────────────────────────────────────────────────────────┤
  │  ② TAB-VORFILTER (ListReport – fiorilistreport)                     │
  │     Eingebettet in SelectionPresentationVariant →                   │
  │     jeder Tab hat seinen eigenen Vorfilter-Zustand                  │
  ├──────────────────────────────────────────────────────────────────────┤
  │  ③ OVP-KARTEN-FILTER (fioriovp, problemsolvingovp)                  │
  │     Referenziert in manifest.json per selectionAnnotationPath →     │
  │     filtert die Daten einer einzelnen OVP-Karte                     │
  ├──────────────────────────────────────────────────────────────────────┤
  │  ④ KPI-VORFILTER (fiorielements)                                    │
  │     In UI.KPI.SelectionVariant →                                    │
  │     bestimmt welche Datensätze für den KPI-Wert gezählt werden      │
  └──────────────────────────────────────────────────────────────────────┘
```

---

#### ① Filter Chips im ListReport (fiorielements)

Filter Chips sind klickbare Schnellfilter-Buttons direkt über der Tabelle. Sie entstehen automatisch, wenn **mehrere `UI.SelectionVariant`-Annotationen** (mit verschiedenen Qualifiern) am gleichen `EntityType` vorhanden sind **und** der ListReport in `manifest.json` so konfiguriert ist, dass er sie anzeigt.

**Visuelle Darstellung:**
```
┌──────────────────────────────────────────────────────────────────┐
│ Filter Bar: [ Name ▼ ] [ Category ▼ ] [ Price ▼ ]   [Suchen]    │
├──────────────────────────────────────────────────────────────────┤
│ [ All ]  [ Low Stock ]  [ Electronics ]  [ Accessories ]         │  ← Filter Chips
│          ↑ aktiv                                                 │
├──────────────────────────────────────────────────────────────────┤
│  ID  │  Name         │  Price  │  Stock  │  Category            │
│ ...  │  ...          │  ...    │  ...    │  ...                 │
└──────────────────────────────────────────────────────────────────┘
```

**Vollständiges Beispiel aus fiorielements (alle 5 Chips):**
```xml
<!-- Chip 1: Alle Produkte (leere SelectOptions = kein Filter) -->
<Annotation Term="UI.SelectionVariant" Qualifier="All">
  <Record Type="UI.SelectionVariantType">
    <PropertyValue Property="Text" String="All Products" />
    <!-- Keine SelectOptions → zeigt alle Datensätze -->
  </Record>
</Annotation>

<!-- Chip 2: Niedriger Lagerbestand (Stock ≤ 10) -->
<Annotation Term="UI.SelectionVariant" Qualifier="LowStock">
  <Record Type="UI.SelectionVariantType">
    <PropertyValue Property="Text" String="Low Stock" />
    <PropertyValue Property="SelectOptions">
      <Collection>
        <Record Type="UI.SelectOptionType">
          <PropertyValue Property="PropertyName" PropertyPath="Stock" />
          <PropertyValue Property="Ranges">
            <Collection>
              <Record Type="UI.SelectionRangeType">
                <PropertyValue Property="Sign"   EnumMember="UI.SelectionRangeSignType/I" />
                <PropertyValue Property="Option" EnumMember="UI.SelectionRangeOptionType/LE" />
                <PropertyValue Property="Low"    Int="10" />
              </Record>
            </Collection>
          </PropertyValue>
        </Record>
      </Collection>
    </PropertyValue>
  </Record>
</Annotation>

<!-- Chip 3: Nur Electronics (Category = "Electronics") -->
<Annotation Term="UI.SelectionVariant" Qualifier="Electronics">
  <Record Type="UI.SelectionVariantType">
    <PropertyValue Property="Text" String="Electronics" />
    <PropertyValue Property="SelectOptions">
      <Collection>
        <Record Type="UI.SelectOptionType">
          <PropertyValue Property="PropertyName" PropertyPath="Category" />
          <PropertyValue Property="Ranges">
            <Collection>
              <Record Type="UI.SelectionRangeType">
                <PropertyValue Property="Sign"   EnumMember="UI.SelectionRangeSignType/I" />
                <PropertyValue Property="Option" EnumMember="UI.SelectionRangeOptionType/EQ" />
                <PropertyValue Property="Low"    String="Electronics" />
              </Record>
            </Collection>
          </PropertyValue>
        </Record>
      </Collection>
    </PropertyValue>
  </Record>
</Annotation>

<!-- Chip 4: Hoher Preis (Price ≥ 100) -->
<Annotation Term="UI.SelectionVariant" Qualifier="HighPrice">
  <Record Type="UI.SelectionVariantType">
    <PropertyValue Property="Text" String="High Price" />
    <PropertyValue Property="SelectOptions">
      <Collection>
        <Record Type="UI.SelectOptionType">
          <PropertyValue Property="PropertyName" PropertyPath="Price" />
          <PropertyValue Property="Ranges">
            <Collection>
              <Record Type="UI.SelectionRangeType">
                <PropertyValue Property="Sign"   EnumMember="UI.SelectionRangeSignType/I" />
                <PropertyValue Property="Option" EnumMember="UI.SelectionRangeOptionType/GE" />
                <PropertyValue Property="Low"    Decimal="100" />
              </Record>
            </Collection>
          </PropertyValue>
        </Record>
      </Collection>
    </PropertyValue>
  </Record>
</Annotation>
```

> **Wie Chips aktiviert werden:** Fiori Elements erkennt automatisch alle `UI.SelectionVariant`-Annotationen am EntityType und rendert sie als Chips. Die **Reihenfolge** richtet sich nach der Reihenfolge in der `annotation.xml`. Ein Klick auf einen Chip ersetzt den aktuellen Filter mit den `SelectOptions` des gewählten Variants.

**Mehrere Filter in einem Chip kombinieren:**
```xml
<!-- Chip: Electronics UND niedriger Preis (zwei SelectOptions = UND-Verknüpfung) -->
<Annotation Term="UI.SelectionVariant" Qualifier="CheapElectronics">
  <Record Type="UI.SelectionVariantType">
    <PropertyValue Property="Text" String="Günstige Elektronik" />
    <PropertyValue Property="SelectOptions">
      <Collection>
        <!-- Bedingung 1: Category = Electronics -->
        <Record Type="UI.SelectOptionType">
          <PropertyValue Property="PropertyName" PropertyPath="Category" />
          <PropertyValue Property="Ranges">
            <Collection>
              <Record Type="UI.SelectionRangeType">
                <PropertyValue Property="Sign"   EnumMember="UI.SelectionRangeSignType/I" />
                <PropertyValue Property="Option" EnumMember="UI.SelectionRangeOptionType/EQ" />
                <PropertyValue Property="Low"    String="Electronics" />
              </Record>
            </Collection>
          </PropertyValue>
        </Record>
        <!-- Bedingung 2: Price ≤ 50 (beide Bedingungen = UND) -->
        <Record Type="UI.SelectOptionType">
          <PropertyValue Property="PropertyName" PropertyPath="Price" />
          <PropertyValue Property="Ranges">
            <Collection>
              <Record Type="UI.SelectionRangeType">
                <PropertyValue Property="Sign"   EnumMember="UI.SelectionRangeSignType/I" />
                <PropertyValue Property="Option" EnumMember="UI.SelectionRangeOptionType/LE" />
                <PropertyValue Property="Low"    Decimal="50" />
              </Record>
            </Collection>
          </PropertyValue>
        </Record>
      </Collection>
    </PropertyValue>
  </Record>
</Annotation>
```

**Wertebereiche (Between) in einem Chip:**
```xml
<!-- Chip: Mittlerer Preisbereich 20–100 EUR -->
<Annotation Term="UI.SelectionVariant" Qualifier="MidRange">
  <Record Type="UI.SelectionVariantType">
    <PropertyValue Property="Text" String="Mittelklasse" />
    <PropertyValue Property="SelectOptions">
      <Collection>
        <Record Type="UI.SelectOptionType">
          <PropertyValue Property="PropertyName" PropertyPath="Price" />
          <PropertyValue Property="Ranges">
            <Collection>
              <Record Type="UI.SelectionRangeType">
                <PropertyValue Property="Sign"   EnumMember="UI.SelectionRangeSignType/I" />
                <!-- BT = Between: Low bis High -->
                <PropertyValue Property="Option" EnumMember="UI.SelectionRangeOptionType/BT" />
                <PropertyValue Property="Low"    Decimal="20" />
                <PropertyValue Property="High"   Decimal="100" />
              </Record>
            </Collection>
          </PropertyValue>
        </Record>
      </Collection>
    </PropertyValue>
  </Record>
</Annotation>
```

**Ausschluss-Filter (EXCLUDE) mit Sign="E":**
```xml
<!-- Chip: Alle außer Cancelled -->
<Annotation Term="UI.SelectionVariant" Qualifier="NotCancelled">
  <Record Type="UI.SelectionVariantType">
    <PropertyValue Property="Text" String="Nicht Storniert" />
    <PropertyValue Property="SelectOptions">
      <Collection>
        <Record Type="UI.SelectOptionType">
          <PropertyValue Property="PropertyName" PropertyPath="Status" />
          <PropertyValue Property="Ranges">
            <Collection>
              <Record Type="UI.SelectionRangeType">
                <!-- E = Exclude: dieser Wert wird AUSGESCHLOSSEN -->
                <PropertyValue Property="Sign"   EnumMember="UI.SelectionRangeSignType/E" />
                <PropertyValue Property="Option" EnumMember="UI.SelectionRangeOptionType/EQ" />
                <PropertyValue Property="Low"    String="CANCELLED" />
              </Record>
            </Collection>
          </PropertyValue>
        </Record>
      </Collection>
    </PropertyValue>
  </Record>
</Annotation>
```

**ODER-Verknüpfung (mehrere Ranges im gleichen SelectOptionType):**
```xml
<!-- Chip: Electronics ODER Accessories (zwei Ranges = ODER innerhalb desselben Feldes) -->
<Annotation Term="UI.SelectionVariant" Qualifier="TechProducts">
  <Record Type="UI.SelectionVariantType">
    <PropertyValue Property="Text" String="Tech Produkte" />
    <PropertyValue Property="SelectOptions">
      <Collection>
        <Record Type="UI.SelectOptionType">
          <PropertyValue Property="PropertyName" PropertyPath="Category" />
          <PropertyValue Property="Ranges">
            <Collection>
              <!-- Range 1 ODER Range 2 (mehrere Ranges = ODER) -->
              <Record Type="UI.SelectionRangeType">
                <PropertyValue Property="Sign"   EnumMember="UI.SelectionRangeSignType/I" />
                <PropertyValue Property="Option" EnumMember="UI.SelectionRangeOptionType/EQ" />
                <PropertyValue Property="Low"    String="Electronics" />
              </Record>
              <Record Type="UI.SelectionRangeType">
                <PropertyValue Property="Sign"   EnumMember="UI.SelectionRangeSignType/I" />
                <PropertyValue Property="Option" EnumMember="UI.SelectionRangeOptionType/EQ" />
                <PropertyValue Property="Low"    String="Accessories" />
              </Record>
            </Collection>
          </PropertyValue>
        </Record>
      </Collection>
    </PropertyValue>
  </Record>
</Annotation>
```

#### Logik-Zusammenfassung: AND vs. OR

```
  Mehrere SelectOptionType-Records (verschiedene Felder)  →  UND-Verknüpfung
  ─────────────────────────────────────────────────────────────────────
  SelectOption: Category = "Electronics"
  SelectOption: Price ≤ 50
  → Ergebnis: Category="Electronics" AND Price≤50

  Mehrere SelectionRangeType-Records (gleiche Felder)  →  ODER-Verknüpfung
  ─────────────────────────────────────────────────────────────────────
  SelectOption: Category, Ranges:
    Range 1: Category = "Electronics"
    Range 2: Category = "Accessories"
  → Ergebnis: Category="Electronics" OR Category="Accessories"
```

---

#### ② Tab-Vorfilter (in SelectionPresentationVariant eingebettet)

Siehe [UI.SelectionPresentationVariant](#uiselectionpresentationvariant) — der Filter ist inline im `SelectionVariant`-Block des SPV definiert.

---

#### ③ OVP-Karten-Filter

```json
"card00": {
  "settings": {
    "selectionAnnotationPath": "com.sap.vocabularies.UI.v1.SelectionVariant#Open"
  }
}
```

Filtert die Daten einer spezifischen OVP-Karte. Nur diese Karte ist betroffen, nicht der globale Filter.

---

#### ④ KPI-Vorfilter

```xml
<Annotation Term="UI.KPI" Qualifier="OpenCount">
  <!-- Zählt nur Datensätze, die dem SelectionVariant#OnlyOpen entsprechen -->
  <PropertyValue Property="SelectionVariant" AnnotationPath="@UI.SelectionVariant#OnlyOpen" />
  ...
</Annotation>
```

---

#### Reale Beispiele aus den Projekten
```xml
<!-- problemsolvingovp: Nur offene Prozesse -->
<Annotation Term="UI.SelectionVariant" Qualifier="OnlyOpen">
  <Record>
    <PropertyValue Property="Text" String="Nur offene Prozesse"/>
    <PropertyValue Property="SelectOptions">
      <Collection>
        <Record Type="UI.SelectOptionType">
          <PropertyValue Property="PropertyName" PropertyPath="ProbSolvingProcLifecycleStatus"/>
          <PropertyValue Property="Ranges">
            <Collection>
              <Record Type="UI.SelectionRangeType">
                <PropertyValue Property="Sign"   EnumMember="UI.SelectionRangeSignType/I"/>
                <PropertyValue Property="Option" EnumMember="UI.SelectionRangeOptionType/EQ"/>
                <PropertyValue Property="Low"    String="01"/>
              </Record>
            </Collection>
          </PropertyValue>
        </Record>
      </Collection>
    </PropertyValue>
  </Record>
</Annotation>
```

---

### UI.PresentationVariant

**Zweck:** Definiert **wie** Daten dargestellt werden — Sortierung + Visualisierungstyp (LineItem oder Chart).

**Target:** `EntityType` — referenziert per `AnnotationPath` andere Annotationen am gleichen Ziel (`@UI.LineItem#X` oder `@UI.Chart#Y`).

**Projekte:** Alle 4 Projekte

> **Zusammenspiel:** Ein PresentationVariant allein hat keine direkte Wirkung. Es wird immer von einem `SelectionPresentationVariant` (Tabs), einem `KPI` oder einer OVP-Karte in `manifest.json` referenziert.

#### Alle Properties von UI.PresentationVariant

| Property | Typ | Bedeutung |
|---|---|---|
| `Text` | String | Beschriftung (für UI, z.B. Tab-Name) |
| `SortOrder` | Collection\<SortOrderType\> | Initiale Sortierung der Daten |
| `Visualizations` | Collection\<AnnotationPath\> | Welche LineItem / Chart Annotation genutzt wird |
| `RequestAtLeast` | Collection\<PropertyPath\> | Felder die immer geladen werden (auch wenn nicht angezeigt) |
| `MaxItems` | Int | Maximale Anzahl dargestellter Datensätze |

#### XML-Syntax
```xml
<Annotation Term="UI.PresentationVariant" Qualifier="Default">
  <Record>
    <PropertyValue Property="Text" String="Nach Datum" />
    <!-- Sortierung -->
    <PropertyValue Property="SortOrder">
      <Collection>
        <Record Type="Common.SortOrderType">
          <PropertyValue Property="Property"   PropertyPath="OrderDate" />
          <PropertyValue Property="Descending" Bool="true" />
        </Record>
      </Collection>
    </PropertyValue>
    <!-- Welche Annotation wird als Darstellung genutzt -->
    <PropertyValue Property="Visualizations">
      <Collection>
        <!-- Tabellen-Darstellung: -->
        <AnnotationPath>@UI.LineItem#TAB2</AnnotationPath>
        <!-- ODER Chart-Darstellung: -->
        <!-- <AnnotationPath>@UI.Chart#CHART3</AnnotationPath> -->
      </Collection>
    </PropertyValue>
  </Record>
</Annotation>
```

---

### UI.SelectionPresentationVariant

**Zweck:** Kombiniert `SelectionVariant` + `PresentationVariant` für einen **Tab** im Multi-Tab ListReport.

**Target:** `EntityType` — jeder Tab im ListReport verweist über seinen `annotationPath` in `manifest.json` auf eine Instanz dieser Annotation. Mehrere SPV-Annotationen (mit verschiedenen Qualifiern) am gleichen EntityType ergeben mehrere Tabs.

**Projekt:** `fiorilistreport`

> **Wie Tabs entstehen:** Die Reihenfolge der Einträge in `views.paths` (manifest.json) bestimmt die Tab-Reihenfolge. Jeder Eintrag zeigt per `annotationPath` auf ein SPV. Ein SPV kann auf einen anderen EntitySet zeigen als der ListReport selbst (daher `entitySet`-Angabe pro Tab möglich).

#### XML-Syntax
```xml
<Annotation Term="UI.SelectionPresentationVariant" Qualifier="VAR1">
  <Record>
    <PropertyValue Property="Text" String="All Products" />
    <!-- Referenz auf SelectionVariant (Vorfilter) -->
    <PropertyValue Property="SelectionVariant">
      <Record>
        <PropertyValue Property="SelectOptions">
          <Collection />   <!-- leer = kein Vorfilter -->
        </PropertyValue>
      </Record>
    </PropertyValue>
    <!-- Referenz auf PresentationVariant (Darstellung) -->
    <PropertyValue Property="PresentationVariant">
      <Record>
        <PropertyValue Property="Visualizations">
          <Collection>
            <AnnotationPath>@UI.LineItem#TAB1</AnnotationPath>
          </Collection>
        </PropertyValue>
      </Record>
    </PropertyValue>
  </Record>
</Annotation>
```

#### Verbindung zu manifest.json (Tab-Konfiguration)
```json
"views": {
  "paths": [
    { "key": "tab0", "annotationPath": "com.sap.vocabularies.UI.v1.SelectionPresentationVariant#VAR1",
      "entitySet": "Products" },
    { "key": "tab1", "annotationPath": "com.sap.vocabularies.UI.v1.SelectionPresentationVariant#VAR2",
      "entitySet": "SalesOrders" },
    { "key": "tab2", "annotationPath": "com.sap.vocabularies.UI.v1.SelectionPresentationVariant#VAR3",
      "entitySet": "SalesOrders" }
  ]
}
```

#### Visueller Aufbau (3-Tab-Setup)
```
┌──────────────────────────────────────────────────────────┐
│ [Tab: Products] [Tab: SalesOrders] [Tab: Chart]          │
│──────────────────────────────────────────────────────────│
│  ▲SPV#VAR1          ▲SPV#VAR2          ▲SPV#VAR3        │
│  ├─ SV: (leer)       ├─ SV: (leer)      ├─ SV: (leer)   │
│  └─ PV → LineItem#1  └─ PV → LineItem#2  └─ PV → Chart  │
└──────────────────────────────────────────────────────────┘
```

---

### UI.Facets & ReferenceFacet

**Zweck:** Definiert die **Abschnitte (Sections)** einer ObjectPage oder eines OVP Stack-Card-Inhalts.

**Target:** `EntityType` — die Facets erscheinen als Tab-Bereiche (Sections) auf der ObjectPage dieser Entität. Mit einem Qualifier werden sie zu einer benannten Variante (z.B. für OVP Stack Cards).

**Projekte:** Alle 4 Projekte

#### Facet-Typen

| Record Type | Zeigt | Typischer Target-Pfad |
|---|---|---|
| `UI.ReferenceFacet` | Einen einzelnen Abschnitt | `@UI.FieldGroup#X`, `@UI.DataPoint#X`, `Orders/@UI.LineItem` |
| `UI.CollectionFacet` | Mehrere Abschnitte nebeneinander (Gruppe) | enthält selbst `UI.ReferenceFacet`-Records |

#### Properties von UI.ReferenceFacet

| Property | Typ | Pflicht | Bedeutung |
|---|---|:---:|---|
| `ID` | String | – | Technische ID des Abschnitts (für Stable IDs, Extensibility) |
| `Label` | String | – | Angezeigte Abschnitts-Überschrift |
| `Target` | AnnotationPath | ✅ | Verweis auf FieldGroup, DataPoint oder eingebettete LineItem |

> **Wann `ID` angeben?** Immer wenn die App später per SAP Fiori Launchpad Adaptation erweitert werden soll (Key User Extensibility). Ohne `ID` kann der Abschnitt nicht stabil referenziert werden. Konvention: `"GeneralDataSection"`, `"OrdersSection"` etc.

#### Navigation-Pfad in Target (eingebettete Tabelle)

Ein `ReferenceFacet` kann per `AnnotationPath` auch auf eine **assoziierte Entität** zeigen und deren `UI.LineItem` einbetten:

```xml
<!-- Zeigt die Bestellungen des Produkts als eingebettete Tabelle -->
<Record Type="UI.ReferenceFacet">
  <PropertyValue Property="Label"  String="Bestellungen" />
  <PropertyValue Property="ID"     String="OrdersSection" />
  <!-- "Orders" ist eine NavigationProperty auf dem EntityType -->
  <PropertyValue Property="Target" AnnotationPath="Orders/@UI.LineItem" />
</Record>
```

Das entspricht in CDS einer Composition/Association:
```abap
-- In ZC_PRODUCT muss eine Association _Orders existieren:
_Orders : redirected to composition child ZC_ORDER
```

#### CollectionFacet – Abschnitte nebeneinander
```xml
<!-- Zwei FieldGroups in einer Zeile (side by side) -->
<Record Type="UI.CollectionFacet">
  <PropertyValue Property="Label" String="Adresse und Kontakt" />
  <PropertyValue Property="Facets">
    <Collection>
      <Record Type="UI.ReferenceFacet">
        <PropertyValue Property="Label"  String="Adresse" />
        <PropertyValue Property="Target" AnnotationPath="@UI.FieldGroup#Address" />
      </Record>
      <Record Type="UI.ReferenceFacet">
        <PropertyValue Property="Label"  String="Kontakt" />
        <PropertyValue Property="Target" AnnotationPath="@UI.FieldGroup#Contact" />
      </Record>
    </Collection>
  </PropertyValue>
</Record>
```

#### XML-Syntax
```xml
<Annotations Target="MyService.Product">
  <Annotation Term="UI.Facets">
    <Collection>

      <!-- ① Einfacher Abschnitt → FieldGroup -->
      <Record Type="UI.ReferenceFacet">
        <PropertyValue Property="Label"  String="Allgemeine Daten" />
        <PropertyValue Property="Target" AnnotationPath="@UI.FieldGroup#GeneralData" />
      </Record>

      <!-- ② Abschnitt → eingebettete Tabelle (LineItem einer anderen Entity) -->
      <Record Type="UI.ReferenceFacet">
        <PropertyValue Property="Label"  String="Bestellungen" />
        <PropertyValue Property="Target" AnnotationPath="Orders/@UI.LineItem" />
      </Record>

      <!-- ③ OVP Stack Card: Qualifier="stack" markiert Karten-Inhalt -->
      <Record Type="UI.ReferenceFacet">
        <PropertyValue Property="ID"     String="OrderDetailsSection" />
        <PropertyValue Property="Label"  String="Details" />
        <PropertyValue Property="Target" AnnotationPath="@UI.FieldGroup#OrderDetails" />
        <!-- Qualifier steuert OVP Stack Card: -->
        <!-- <PropertyValue Property="Qualifier" String="stack" /> -->
      </Record>

    </Collection>
  </Annotation>
</Annotations>
```

#### OVP-spezifisch: Facets mit Qualifier="stack"
```xml
<!-- fioriovp: Stack Card verwendet Qualifier="stack" -->
<Annotation Term="UI.Facets" Qualifier="stack">
  <Collection>
    <Record Type="UI.ReferenceFacet">
      <PropertyValue Property="Label"  String="{@i18n>labelOrderDetails}" />
      <PropertyValue Property="Target" AnnotationPath="@UI.FieldGroup#OrderDetails" />
    </Record>
  </Collection>
</Annotation>
```

#### CDS-Äquivalent
```abap
annotate view ZC_Product with @UI.facets: [
  {
    $Type:           'UI.ReferenceFacet',
    label:           'General Data',
    targetQualifier: 'GeneralData',
    targetElement:   #FIELDGROUP_REFERENCE
  }
];
```

---

### UI.FieldGroup

**Zweck:** Gruppiert mehrere Felder in einem benannten Abschnitt (wird per `UI.Facets` eingebunden).

**Target:** `EntityType` — ein FieldGroup ist **kein eigenständiges UI-Element**. Es wird immer von einem `UI.Facets`-Eintrag (ReferenceFacet) referenziert. Mehrere FieldGroups am gleichen EntityType sind möglich (durch Qualifier unterschieden).

**Projekte:** Alle 4 Projekte

> **FieldGroup vs. LineItem:** Beide sind `Collection`-Annotationen mit DataField-Records. Der Unterschied: LineItem = Tabelle (eine Zeile pro Datensatz), FieldGroup = Formular (alle Felder eines Datensatzes untereinander).

#### XML-Syntax
```xml
<Annotation Term="UI.FieldGroup" Qualifier="GeneralData">
  <Record Type="UI.FieldGroupType">
    <PropertyValue Property="Label" String="Allgemeine Daten" />
    <PropertyValue Property="Data">
      <Collection>

        <!-- Normales Feld -->
        <Record Type="UI.DataField">
          <PropertyValue Property="Label" String="Name" />
          <PropertyValue Property="Value" Path="ProductName" />
        </Record>

        <!-- DataPoint-Verweis (z.B. Progress-Bar) -->
        <Record Type="UI.DataFieldForAnnotation">
          <PropertyValue Property="Label"  String="Stock" />
          <PropertyValue Property="Target" AnnotationPath="@UI.DataPoint#StockDataPoint" />
        </Record>

        <!-- Button für OData-Action -->
        <Record Type="UI.DataFieldForAction">
          <PropertyValue Property="Label"  String="Update Stock" />
          <PropertyValue Property="Action" String="ProductService.updateStock" />
        </Record>

        <!-- i18n-Label (OVP) -->
        <Record Type="UI.DataField">
          <PropertyValue Property="Label" String="{@i18n>labelStatus}" />
          <PropertyValue Property="Value" Path="Status" />
        </Record>

      </Collection>
    </PropertyValue>
  </Record>
</Annotation>
```

#### CDS-Äquivalent
```abap
@UI.fieldGroup: [{ qualifier: 'GeneralData', position: 20, label: 'Product Name' }]
ProductName,

@UI.fieldGroup: [{ qualifier: 'GeneralData', position: 30, label: 'Category' }]
Category,
```

---

### UI.DataPoint

**Zweck:** Definiert eine **Wert-Visualisierung**: Zahl, Fortschrittsbalken oder Bewertungssterne. Wird in HeaderFacets oder per `DataFieldForAnnotation` in LineItem / FieldGroup eingebunden.

**Target:** `EntityType` — ein DataPoint ist **kein eigenständiges UI-Element**, er wird immer von einer anderen Annotation referenziert:
- `UI.DataFieldForAnnotation` im `UI.LineItem` → Spalte in der Tabelle
- `UI.DataFieldForAnnotation` im `UI.FieldGroup` → Feld in einem Abschnitt
- `UI.HeaderFacets` → KPI-Kachel im ObjectPage-Header
- `UI.KPI` → KPI-Kachel mit Trend-Chart

**Projekte:** `fiorilistreport`, `fiorielements`, `problemsolvingovp`

#### Alle Properties von UI.DataPointType

| Property | Typ | Bedeutung |
|---|---|---|
| `Value` | Path | Pflicht. Das Feld, dessen Wert dargestellt wird |
| `Title` | String | Bezeichnung der Kachel / Tooltip |
| `TargetValue` | Int/Decimal/Path | 100%-Referenzwert (für Progress) oder Max-Wert (Rating) |
| `MinimumValue` | Int/Decimal | Minimalwert (z.B. 1 für Rating-Sterne) |
| `Visualization` | EnumMember | `Progress` oder `Rating` (ohne = Zahl) |
| `Criticality` | Path/EnumMember | Farbe: 1=Rot, 2=Gelb, 3=Grün (Path auf Int-Feld oder fest) |
| `CriticalityCalculation` | Record | Automatische Ampel-Berechnung aus Schwellwerten |

#### Criticality – Ampelfarben
```xml
<!-- Variante A: Pfad auf ein Integer-Feld im EntityType (1/2/3) -->
<PropertyValue Property="Criticality" Path="StockCriticality" />
<!-- Das Feld StockCriticality muss im Mock-Datensatz 1, 2 oder 3 enthalten -->

<!-- Variante B: Fest kodierter Wert -->
<PropertyValue Property="Criticality" EnumMember="UI.CriticalityType/Negative" />

<!-- Variante C: Automatische Berechnung aus Schwellwerten -->
<PropertyValue Property="CriticalityCalculation">
  <Record Type="UI.CriticalityCalculationType">
    <PropertyValue Property="ImprovementDirection" EnumMember="UI.ImprovementDirectionType/Maximize" />
    <PropertyValue Property="ToleranceRangeLowValue"   Int="300" />  <!-- unter 300 = Gelb -->
    <PropertyValue Property="DeviationRangeLowValue"   Int="100" />  <!-- unter 100 = Rot  -->
  </Record>
</PropertyValue>
```

| CriticalityType Enum | Farbe | Bedeutung |
|---|---|---|
| `Neutral` (0) | Grau | Kein Status |
| `Negative` (1) | Rot | Kritisch / Fehler |
| `Critical` (2) | Gelb | Warnung |
| `Positive` (3) | Grün | OK |

#### Variante 1: Einfache Zahl (Number)
```xml
<Annotation Term="UI.DataPoint" Qualifier="PriceDataPoint">
  <Record>
    <PropertyValue Property="Value" Path="Price" />
    <PropertyValue Property="Title" String="Preis" />
  </Record>
</Annotation>
```

#### Variante 2: Fortschrittsbalken (Progress)
```xml
<Annotation Term="UI.DataPoint" Qualifier="StockDataPoint">
  <Record>
    <PropertyValue Property="Value"       Path="Stock" />
    <PropertyValue Property="Title"       String="Lagerbestand" />
    <!-- Zielwert für 100% -->
    <PropertyValue Property="TargetValue" Int="1000" />
    <!-- Visualisierungstyp -->
    <PropertyValue Property="Visualization" EnumMember="UI.VisualizationType/Progress" />
    <!-- Kritikalitätsstufe: Pfad auf ein Integer-Feld (1=Red, 2=Yellow, 3=Green) -->
    <PropertyValue Property="Criticality"   Path="Stock" />
  </Record>
</Annotation>
```

#### Variante 3: Sterne-Bewertung (Rating)
```xml
<Annotation Term="UI.DataPoint" Qualifier="RatingDataPoint">
  <Record>
    <PropertyValue Property="Value"         Path="Rating" />
    <PropertyValue Property="Title"         String="Bewertung" />
    <!-- Max. Anzahl Sterne -->
    <PropertyValue Property="TargetValue"   Int="5" />
    <!-- Min. Anzahl Sterne -->
    <PropertyValue Property="MinimumValue"  Int="1" />
    <PropertyValue Property="Visualization" EnumMember="UI.VisualizationType/Rating" />
  </Record>
</Annotation>
```

#### Visualisierungstypen
| Enum | Darstellung |
|---|---|
| `UI.VisualizationType/Progress` | Fortschrittsbalken `[======   ] 60%` |
| `UI.VisualizationType/Rating` | Sterne ★★★☆☆ |
| `UI.VisualizationType/Number` | Formatierte Zahl (explizit) |
| _(kein)_ | Zahlenformat (implizit, gleiche Wirkung wie Number) |

> **Hinweis:** `UI.VisualizationType/Number` kann explizit gesetzt werden wenn man sicherstellen will, dass keine automatische Erkennung als Progress oder Rating stattfindet.

#### TargetValue als dynamischer Pfad

Statt eines festen Wertes kann `TargetValue` auch auf ein **anderes Feld** zeigen — z.B. um den Progress relativ zur Gesamtmenge darzustellen:

```xml
<!-- fiorielements: DeliveredOrders relativ zu TotalOrders -->
<Annotation Term="UI.DataPoint" Qualifier="DeliveredOrdersDP">
  <Record Type="UI.DataPointType">
    <PropertyValue Property="Value"        Path="DeliveredOrders" />
    <PropertyValue Property="Title"        String="Delivered" />
    <!-- TargetValue zeigt auf ein anderes Property des gleichen EntityType -->
    <PropertyValue Property="TargetValue"  Path="TotalOrders" />
    <PropertyValue Property="Criticality"  EnumMember="UI.CriticalityType/Neutral" />
    <PropertyValue Property="Visualization" EnumMember="UI.VisualizationType/Progress" />
  </Record>
</Annotation>
```

Damit ergibt sich: `Fortschritt = DeliveredOrders / TotalOrders × 100%`

#### Einbindung in LineItem
```xml
<!-- In UI.LineItem: DataFieldForAnnotation zeigt auf DataPoint -->
<Record Type="UI.DataFieldForAnnotation">
  <PropertyValue Property="Label"  String="Rating" />
  <PropertyValue Property="Target" AnnotationPath="@UI.DataPoint#RatingDataPoint" />
</Record>
```

#### CDS-Äquivalent
```abap
annotate view ZC_Product with {
  @UI.dataPoint: {
    qualifier:     'StockDataPoint',
    title:         'Lagerbestand',
    targetValue:   1000,
    visualization: #PROGRESS
  }
  Stock;

  @UI.dataPoint: {
    qualifier:     'RatingDataPoint',
    title:         'Bewertung',
    visualization: #RATING,
    maximumValue:  5,
    minimumValue:  1
  }
  Rating;
}
```

#### Häufige Fehler bei UI.DataPoint

| Fehler | Ursache | Lösung |
|---|---|---|
| Progress-Bar zeigt immer 100% | `TargetValue` fehlt oder ist 0 | `TargetValue` auf sinnvollen Maximalwert setzen |
| Sterne werden nicht angezeigt | `UI.DataField` statt `UI.DataFieldForAnnotation` in `UI.LineItem` | Record-Typ auf `UI.DataFieldForAnnotation` ändern |
| Farbe der Progress-Bar ändert sich nicht | `Criticality`-Feld enthält String statt Integer | Feld muss `Edm.Byte` oder `Edm.Int16` sein (1, 2, 3) |
| `TargetValue` als Path funktioniert nicht | `TargetValue` zeigt auf Property mit falschen Datentyp | Property muss `Edm.Decimal` oder `Edm.Int32` sein |
| KPI-Wert im Header leer | DataPoint wird von `UI.KPI` referenziert, aber `SelectionVariant` filtert alle Daten weg | `SelectionVariant` prüfen oder entfernen |

---

### UI.Chart

**Zweck:** Definiert ein Diagramm mit Dimensionen, Kennzahlen und Chart-Typ. Wird per `PresentationVariant.Visualizations` oder direkt in OVP-Karten referenziert.

**Target:** `EntityType` — ein Chart ist **kein eigenständiges UI-Element**. Er wird immer referenziert von:
- `UI.PresentationVariant.Visualizations` → Chart-Tab im ListReport
- `UI.KPI.Detail.DefaultPresentationVariant` → Trend-Chart in KPI-Kachel
- `manifest.json` Karten-Einstellung `chartAnnotationPath` → OVP Analytical Card

**Wichtig:** Charts benötigen immer `Aggregation.ApplySupported` am EntitySet in `metadata.xml`. Ohne dieses Flag sendet Fiori Elements keinen `$apply`-Parameter und der Chart bleibt leer.

**Projekte:** `fiorilistreport`, `fiorielements`, `fioriovp`, `problemsolvingovp`

#### XML-Syntax (vollständig)
```xml
<Annotation Term="UI.Chart" Qualifier="OrdersByStatus">
  <Record>
    <PropertyValue Property="Title"       String="Bestellungen nach Status" />
    <PropertyValue Property="Description" String="Verteilung aller Bestellungen" />
    <!-- Chart-Typ -->
    <PropertyValue Property="ChartType"   EnumMember="UI.ChartType/Donut" />

    <!-- Dimensionen (X-Achse / Segmente) -->
    <PropertyValue Property="Dimensions">
      <Collection>
        <PropertyPath>Status</PropertyPath>
      </Collection>
    </PropertyValue>
    <PropertyValue Property="DimensionAttributes">
      <Collection>
        <Record Type="UI.ChartDimensionAttributeType">
          <PropertyValue Property="Dimension" PropertyPath="Status" />
          <!-- Role: Category = X-Achse / Segment -->
          <PropertyValue Property="Role" EnumMember="UI.ChartDimensionRoleType/Category" />
        </Record>
      </Collection>
    </PropertyValue>

    <!-- Kennzahlen (Y-Achse / Größe) -->
    <PropertyValue Property="Measures">
      <Collection>
        <PropertyPath>NetAmount</PropertyPath>
      </Collection>
    </PropertyValue>
    <PropertyValue Property="MeasureAttributes">
      <Collection>
        <Record Type="UI.ChartMeasureAttributeType">
          <PropertyValue Property="Measure" PropertyPath="NetAmount" />
          <!-- Role: Axis1 = primäre Y-Achse -->
          <PropertyValue Property="Role"    EnumMember="UI.ChartMeasureRoleType/Axis1" />
          <!-- DataPoint-Referenz für Tooltip/Formatierung -->
          <PropertyValue Property="DataPoint" AnnotationPath="@UI.DataPoint#NetAmountDP" />
        </Record>
      </Collection>
    </PropertyValue>
  </Record>
</Annotation>
```

#### Chart-Typen
| Enum | Diagrammtyp |
|---|---|
| `Bar` | Horizontales Balkendiagramm |
| `Column` | Vertikales Säulendiagramm |
| `Line` | Liniendiagramm |
| `Donut` | Kreisdiagramm (Ring) |
| `Pie` | Kreisdiagramm |
| `Bubble` | Blasendiagramm |
| `Scatter` | Streudiagramm |
| `HeatMap` | Wärmekarte |

#### Dimension-Rollen
| Enum | Bedeutung |
|---|---|
| `Category` | Kategorisierungsachse (X) |
| `Series` | Datenreihe (Farbe/Legende) |

#### Measure-Rollen
| Enum | Bedeutung |
|---|---|
| `Axis1` | Primäre Werteachse |
| `Axis2` | Sekundäre Werteachse |
| `Axis3` | Tertiäre Werteachse |

#### Voraussetzung für Charts (OData V4)
Charts benötigen `Aggregation.ApplySupported` auf dem EntitySet in der metadata.xml:
```xml
<Annotations Target="MainService.MainContainer/SalesOrders">
  <Annotation Term="Aggregation.ApplySupported">
    <Record Type="Aggregation.ApplySupportedType">
      <PropertyValue Property="AggregatableProperties">
        <Collection>
          <Record Type="Aggregation.AggregatablePropertyType">
            <PropertyValue Property="Property" PropertyPath="NetAmount" />
          </Record>
        </Collection>
      </PropertyValue>
    </Record>
  </Annotation>
</Annotations>
```

---

### UI.KPI

**Zweck:** KPI-Kachel im ObjectPage-Header. Kombiniert einen `DataPoint` (Hauptzahl) mit einem Chart (Trendanzeige beim Klick auf die Kachel).

**Target:** `EntityType` — genau wie `UI.DataPoint` ist ein KPI kein eigenständiges Element. Er wird von `UI.HeaderFacets` referenziert: `AnnotationPath="@UI.KPI#MyKPI"`.

**Projekt:** `fiorielements`

> **Unterschied DataPoint vs. KPI:** Ein DataPoint zeigt nur einen Wert (Zahl, Progress, Rating). Ein KPI zeigt einen Wert + hat eine Drill-Down-Funktion mit Chart.

#### XML-Syntax
```xml
<Annotation Term="UI.KPI" Qualifier="ProductCount">
  <Record Type="UI.KPIType">
    <!-- Verweis auf den DataPoint der den Hauptwert liefert -->
    <PropertyValue Property="DataPoint" AnnotationPath="@UI.DataPoint#ProductCount" />
    <!-- Verweis auf PresentationVariant mit Chart für den Trend -->
    <PropertyValue Property="Detail">
      <Record Type="UI.KPIDetailType">
        <PropertyValue Property="DefaultPresentationVariant"
                       AnnotationPath="@UI.PresentationVariant#ProductCountPV" />
      </Record>
    </PropertyValue>
  </Record>
</Annotation>

<!-- Der zugehörige DataPoint -->
<Annotation Term="UI.DataPoint" Qualifier="ProductCount">
  <Record>
    <PropertyValue Property="Value" Path="TotalOrders" />
    <PropertyValue Property="Title" String="Bestellungen gesamt" />
  </Record>
</Annotation>

<!-- Der zugehörige PresentationVariant mit einem Donut-Chart -->
<Annotation Term="UI.PresentationVariant" Qualifier="ProductCountPV">
  <Record>
    <PropertyValue Property="Visualizations">
      <Collection>
        <AnnotationPath>@UI.Chart#ProductCountChart</AnnotationPath>
      </Collection>
    </PropertyValue>
  </Record>
</Annotation>
```

#### Verbindungsdiagramm
```
UI.KPI#ProductCount
  ├─ DataPoint        → UI.DataPoint#ProductCount  (Zahlenwert)
  ├─ SelectionVariant → UI.SelectionVariant#All     (optionaler Vorfilter)
  └─ Detail           → UI.PresentationVariant#PV
                          └─ Visualizations → UI.Chart#Chart (Donut)
```

#### Vollständige KPI-Syntax mit SelectionVariant

```xml
<!-- fiorielements: KPI mit Vorfilter und AlternativePresentationVariants -->
<Annotation Term="UI.KPI" Qualifier="ProductCount">
  <Record Type="UI.KPIType">
    <!-- Hauptwert -->
    <PropertyValue Property="DataPoint" AnnotationPath="@UI.DataPoint#ProductCount" />
    <!-- Optionaler Vorfilter (welche Datensätze gezählt werden) -->
    <PropertyValue Property="SelectionVariant" AnnotationPath="@UI.SelectionVariant#All" />
    <!-- Detail-Konfiguration -->
    <PropertyValue Property="Detail">
      <Record Type="UI.KPIDetailType">
        <!-- Haupt-PresentationVariant (primäre Chart-Ansicht) -->
        <PropertyValue Property="DefaultPresentationVariant"
                       AnnotationPath="@UI.PresentationVariant#ProductCountPV" />
        <!-- Alternative Ansichten (z.B. mehrere Chart-Typen wählbar) -->
        <PropertyValue Property="AlternativePresentationVariants">
          <Collection />
        </PropertyValue>
        <!-- Navigation bei Klick auf den KPI-Wert (optional) -->
        <PropertyValue Property="SemanticObject" String="" />
        <PropertyValue Property="Action"         String="" />
      </Record>
    </PropertyValue>
  </Record>
</Annotation>
```

---

### UI.Identification

**Zweck:** Definiert Aktionen / Buttons in der Toolbar oder für Zeilen-Navigation. Im OVP-Kontext: Quick-View- oder Stack-Header-Inhalt.

**Target:** `EntityType` — die Records erscheinen als Toolbar-Buttons auf der ObjectPage. Im OVP-Kontext: mit Qualifier `stackHeader` = Header-Felder der Stack-Karte, mit Qualifier `quickView` = Felder im Aufklapp-Fenster.

**Projekte:** `fiorielements`, `fioriovp`, `problemsolvingovp`

#### XML-Syntax
```xml
<Annotation Term="UI.Identification">
  <Collection>
    <!-- Felder anzeigen (z.B. im Quick View) -->
    <Record Type="UI.DataField">
      <PropertyValue Property="Label" String="Bestell-ID" />
      <PropertyValue Property="Value" Path="OrderID" />
    </Record>

    <!-- Action-Button -->
    <Record Type="UI.DataFieldForAction">
      <PropertyValue Property="Label"  String="Genehmigen" />
      <PropertyValue Property="Action" String="ProductService.approve" />
    </Record>

    <!-- Intent-Navigation (real. Beispiel problemsolvingovp) -->
    <Record Type="UI.DataFieldForIntentBasedNavigation">
      <PropertyValue Property="Label"          String="In App öffnen" />
      <PropertyValue Property="SemanticObject" String="QM_ProbSolvingProcess" />
      <PropertyValue Property="Action"         String="display" />
      <Annotation Term="UI.Importance" EnumMember="UI.ImportanceType/High"/>
    </Record>
  </Collection>
</Annotation>

<!-- OVP: mit Qualifier für Stack-Header -->
<Annotation Term="UI.Identification" Qualifier="stackHeader">
  <Collection>
    <Record Type="UI.DataField">
      <PropertyValue Property="Value" Path="OrderID" />
    </Record>
  </Collection>
</Annotation>

<!-- OVP: mit Qualifier für Quick View -->
<Annotation Term="UI.Identification" Qualifier="quickView">
  <Collection>
    <Record Type="UI.DataField">
      <PropertyValue Property="Value" Path="OrderID" />
    </Record>
  </Collection>
</Annotation>
```

---

### UI.TextArrangement

**Zweck:** Steuert, ob bei einem Feld nur der Text, nur die ID oder beides angezeigt wird (in Kombination mit `Common.Text`).

**Target:** `Property` des EntityType — wirkt auf dieses eine Feld überall dort, wo es angezeigt wird (Tabelle, Filter, Formular).

**Projekt:** `problemsolvingovp`

#### XML-Syntax
```xml
<!-- Auf dem Feld, das den Text bezieht: -->
<Annotations Target="MyService.MyEntity/StatusCode">
  <!-- Common.Text liefert den Anzeigetext aus einer verwandten Entität -->
  <Annotation Term="Common.Text" Path="to_Status/StatusText" />
  <!-- TextArrangement steuert das Format -->
  <Annotation Term="UI.TextArrangement" EnumMember="UI.TextArrangementType/TextOnly" />
</Annotations>
```

#### TextArrangement-Typen
| Enum | Darstellung |
|---|---|
| `TextOnly` | Nur Text (kein Code) |
| `TextFirst` | Text (Code) — z.B. "Offen (01)" |
| `TextLast` | Code (Text) — z.B. "01 (Offen)" |
| `TextSeparate` | Code und Text getrennt |

---

### UI.Importance

**Zweck:** Gibt die Priorität eines Feldes in der Darstellung an — z.B. in ValueList-Suchergebnissen oder in responsiven Tabellen.

**Projekt:** `problemsolvingovp`

#### XML-Syntax
```xml
<!-- Auf einem Record in Common.ValueList: -->
<Record Type="Common.ValueListParameterDisplayOnly">
  <PropertyValue Property="ValueListProperty" String="PlantName" />
  <Annotation Term="UI.Importance" EnumMember="UI.ImportanceType/High"/>
</Record>

<!-- Auf einem DataFieldForIntentBasedNavigation: -->
<Record Type="UI.DataFieldForIntentBasedNavigation">
  ...
  <Annotation Term="UI.Importance" EnumMember="UI.ImportanceType/High"/>
</Record>
```

#### Wichtigkeitsstufen
| Enum | Bedeutung |
|---|---|
| `High` | Immer anzeigen |
| `Medium` | Bei mittlerer Breite anzeigen |
| `Low` | Nur bei voller Breite anzeigen |

---

### UI.Hidden & UI.HiddenFilter

**Zweck:** Blendet ein Feld in der UI aus. `UI.Hidden` versteckt es überall (Tabelle, Formular). `UI.HiddenFilter` nur in der Filter-Bar.

**Target:** `Property` oder `EntityType`

> **Best Practice:** Technische Schlüsselfelder (z.B. interne GUIDs) oder Felder die nur als Binding-Basis dienen (z.B. ein Criticality-Code-Feld) werden mit `UI.Hidden` ausgeblendet, aber im Binding weiterhin genutzt.

#### XML-Syntax
```xml
<!-- Feld komplett ausblenden (Tabelle + Formular + Filter): -->
<Annotations Target="MyService.Product/InternalGUID">
  <Annotation Term="UI.Hidden" Bool="true" />
</Annotations>

<!-- Nur aus der Filter-Bar ausblenden (bleibt in Tabelle/Formular sichtbar): -->
<Annotations Target="MyService.Product/TechnicalStatus">
  <Annotation Term="UI.HiddenFilter" Bool="true" />
</Annotations>

<!-- Dynamisch: Ausblenden abhängig von einem anderen Feld (Path auf Bool-Property): -->
<Annotations Target="MyService.Product/DiscountRate">
  <Annotation Term="UI.Hidden" Path="IsDiscountHidden" />
</Annotations>
```

#### Typische Use-Cases
| Feld-Typ | Empfehlung |
|---|---|
| Interne GUID / technischer Schlüssel | `UI.Hidden = true` |
| Criticality-Code (für Farbsteuerung) | `UI.Hidden = true` — Wert wird per `Path` referenziert, nicht angezeigt |
| Navigations-Fremdschlüssel | `UI.HiddenFilter = true` |
| Benutzerabhängig ein-/ausblenden | `UI.Hidden` mit `Path` auf Bool-Property |

---

### UI.Criticality

**Zweck:** Steuert die **Farbe** eines Feldes, einer Zeile oder eines Buttons. Wird über einen statischen EnumMember oder dynamisch per Path auf eine Integer-Property gesetzt.

**Target:** Als Annotation innerhalb von `UI.DataField`, `UI.DataPoint` oder direkt auf einer Property.

> **Faustregel:** Den Criticality-**Wert** (0–4) in einer eigenen Property speichern, die Property mit `UI.Hidden` ausblenden und per `Path` referenzieren.

#### Criticality-Werte
| Integer-Wert | EnumMember | Farbe / Bedeutung |
|:---:|---|---|
| `0` | `UI.CriticalityType/Neutral` | Grau – neutral |
| `1` | `UI.CriticalityType/Negative` | Rot – kritisch / Fehler |
| `2` | `UI.CriticalityType/Critical` | Orange / Gelb – Warnung |
| `3` | `UI.CriticalityType/Positive` | Grün – OK |
| `5` | `UI.CriticalityType/Information` | Blau – Information |

#### XML-Syntax
```xml
<!-- Statisch auf einem DataField (immer rot): -->
<Record Type="UI.DataField">
  <PropertyValue Property="Value"       Path="Status" />
  <PropertyValue Property="Criticality" EnumMember="UI.CriticalityType/Negative" />
</Record>

<!-- Dynamisch: Farbe kommt aus einem Integer-Feld im Datensatz: -->
<Record Type="UI.DataField">
  <PropertyValue Property="Value"       Path="Status" />
  <!-- StatusCriticality ist eine Edm.Byte/Int Property mit Werten 0-3 -->
  <PropertyValue Property="Criticality" Path="StatusCriticality" />
</Record>

<!-- In einem DataPoint (für Fortschrittsbalken): -->
<Annotation Term="UI.DataPoint" Qualifier="StockDP">
  <Record Type="UI.DataPointType">
    <PropertyValue Property="Value"       Path="Stock" />
    <PropertyValue Property="Criticality" Path="StockCriticality" />
  </Record>
</Annotation>

<!-- Ganze Tabellenzeile einfärben (auf EntityType-Ebene): -->
<Annotations Target="MyService.SalesOrder">
  <Annotation Term="UI.Criticality" Path="OverallCriticality" />
</Annotations>
```

#### Praxis-Muster: Backend liefert Integer
```xml
<!-- metadata.xml: Criticality-Feld definieren -->
<Property Name="StatusCriticality" Type="Edm.Byte" />

<!-- annotation.xml: Feld ausblenden, Farbe referenzieren -->
<Annotations Target="MyService.Order/StatusCriticality">
  <Annotation Term="UI.Hidden" Bool="true" />
</Annotations>

<Annotations Target="MyService.Order">
  <Annotation Term="UI.LineItem">
    <Collection>
      <Record Type="UI.DataField">
        <PropertyValue Property="Value"       Path="Status" />
        <PropertyValue Property="Criticality" Path="StatusCriticality" />
      </Record>
    </Collection>
  </Annotation>
</Annotations>
```

---

### UI.CriticalityCalculation

**Zweck:** Berechnet die Criticality automatisch aus einem numerischen Wert und definierten Schwellenwerten — ohne Backend-Feld. Fiori Elements berechnet die Farbe clientseitig.

**Target:** Innerhalb von `UI.DataPoint`

> **Vorteil gegenüber `UI.Criticality` mit Path:** Kein extra Backend-Feld nötig. Schwellenwerte werden in der Annotation definiert.

#### ImprovementDirection — Bedeutung
| EnumMember | Bedeutung | Grün wenn... |
|---|---|---|
| `Maximizing` | Höher = besser (z.B. Umsatz, Rating) | Wert ≥ ToleranceRangeLowValue |
| `Minimizing` | Niedriger = besser (z.B. Fehlerrate, Kosten) | Wert ≤ ToleranceRangeHighValue |
| `Target` | Zielwert ± Toleranz (z.B. Temperatur, Füllstand) | Wert im Toleranzbereich |

#### XML-Syntax
```xml
<Annotation Term="UI.DataPoint" Qualifier="StockDP">
  <Record Type="UI.DataPointType">
    <PropertyValue Property="Value"       Path="Stock" />
    <PropertyValue Property="Title"       String="Lagerbestand" />
    <PropertyValue Property="TargetValue" Int="1000" />

    <PropertyValue Property="CriticalityCalculation">
      <Record Type="UI.CriticalityCalculationType">
        <!-- Maximizing: mehr Lager = besser -->
        <PropertyValue Property="ImprovementDirection"
          EnumMember="UI.ImprovementDirectionType/Maximizing" />
        <!-- Grün ab 500: -->
        <PropertyValue Property="ToleranceRangeLowValue"  Int="500" />
        <!-- Gelb ab 200: -->
        <PropertyValue Property="DeviationRangeLowValue"  Int="200" />
        <!-- Unter 200 → Rot (kein weiterer Schwellenwert nötig) -->
      </Record>
    </PropertyValue>
  </Record>
</Annotation>
```

#### Schwellenwert-Schema (Maximizing)
```
  Wert:    0 ──────── 200 ──────── 500 ──────── ∞
  Farbe:       ROT         GELB         GRÜN
               ↑                        ↑
        DeviationRangeLow        ToleranceRangeLow
```

#### Schwellenwert-Schema (Minimizing)
```xml
<!-- Minimizing: Fehlerrate — niedriger ist besser -->
<PropertyValue Property="CriticalityCalculation">
  <Record Type="UI.CriticalityCalculationType">
    <PropertyValue Property="ImprovementDirection"
      EnumMember="UI.ImprovementDirectionType/Minimizing" />
    <!-- Grün bis 2% Fehlerrate: -->
    <PropertyValue Property="ToleranceRangeHighValue" Decimal="2" />
    <!-- Gelb bis 5%: -->
    <PropertyValue Property="DeviationRangeHighValue" Decimal="5" />
    <!-- Über 5% → Rot -->
  </Record>
</PropertyValue>
```

```
  Fehlerrate: 0 ─── 2% ─── 5% ─── ∞
  Farbe:        GRÜN   GELB   ROT
```

#### Schwellenwert-Schema (Target – Zielwert mit Toleranz)
```xml
<!-- Target: Temperatur soll bei 20°C sein (±3°C Toleranz, ±8°C Abweichung) -->
<PropertyValue Property="CriticalityCalculation">
  <Record Type="UI.CriticalityCalculationType">
    <PropertyValue Property="ImprovementDirection"
      EnumMember="UI.ImprovementDirectionType/Target" />
    <!-- Grüner Bereich: 17°C bis 23°C -->
    <PropertyValue Property="ToleranceRangeLowValue"  Decimal="17" />
    <PropertyValue Property="ToleranceRangeHighValue" Decimal="23" />
    <!-- Gelber Bereich: 12°C bis 28°C -->
    <PropertyValue Property="DeviationRangeLowValue"  Decimal="12" />
    <PropertyValue Property="DeviationRangeHighValue" Decimal="28" />
    <!-- Außerhalb → Rot -->
  </Record>
</PropertyValue>
```

```
  Temperatur: 12° ─── 17° ─── 23° ─── 28°
  Farbe:       GELB   GRÜN     GRÜN    GELB
  Außerhalb:  ROT                        ROT
```

#### Alle Schwellenwert-Properties im Überblick

| Property | Verwendet bei | Bedeutung |
|---|---|---|
| `ToleranceRangeLowValue` | Maximizing, Target | Untere Grenze des grünen Bereichs |
| `ToleranceRangeHighValue` | Minimizing, Target | Obere Grenze des grünen Bereichs |
| `DeviationRangeLowValue` | Maximizing, Target | Untere Grenze des gelben Bereichs (darunter: Rot) |
| `DeviationRangeHighValue` | Minimizing, Target | Obere Grenze des gelben Bereichs (darüber: Rot) |

---

### UI.MultiLineText

**Zweck:** Rendert ein String-Feld als mehrzeiliges Textfeld (`sap.m.TextArea`) statt als einzeiligen Input.

**Target:** `Property` (Edm.String)

#### XML-Syntax
```xml
<Annotations Target="MyService.Task/Description">
  <Annotation Term="UI.MultiLineText" Bool="true" />
</Annotations>
```

> Wirkt auf der **Object Page** im Edit-Modus. Im Display-Modus wird der Text als mehrzeiliger `sap.m.Text` gerendert. In der Tabelle hat es keinen Effekt — dort bleibt der Text einzeilig.

---

### UI.DataFieldWithUrl

**Zweck:** Rendert ein Feld als klickbaren **Link** (Hyperlink) zu einer internen oder externen URL. Die URL kann statisch oder dynamisch (Path auf eine URL-Property) sein.

**Target:** Als Record-Typ innerhalb von `UI.LineItem`, `UI.FieldGroup` oder `UI.Identification`

**Projekte:** `fiorilistreport`

#### XML-Syntax
```xml
<!-- Dynamische URL aus einer Property: -->
<Annotation Term="UI.LineItem">
  <Collection>
    <Record Type="UI.DataFieldWithUrl">
      <PropertyValue Property="Label"  String="Dokument" />
      <!-- Anzeigetext des Links: -->
      <PropertyValue Property="Value"  Path="DocumentName" />
      <!-- URL: kommt aus einer Property des Datensatzes: -->
      <PropertyValue Property="Url"    Path="DocumentUrl" />
    </Record>
  </Collection>
</Annotation>

<!-- Statische externe URL: -->
<Record Type="UI.DataFieldWithUrl">
  <PropertyValue Property="Label" String="SAP Help" />
  <PropertyValue Property="Value" String="Dokumentation öffnen" />
  <PropertyValue Property="Url"   String="https://help.sap.com" />
</Record>

<!-- Mit URL-Medien-Typ (z.B. Bild-Link): -->
<Record Type="UI.DataFieldWithUrl">
  <PropertyValue Property="Label"        String="Produktbild" />
  <PropertyValue Property="Value"        Path="ImageDescription" />
  <PropertyValue Property="Url"          Path="ImageUrl" />
  <PropertyValue Property="UrlContentType" Path="ImageMimeType" />
</Record>

<!-- Link öffnet in neuem Tab (HTML5.LinkTarget): -->
<edmx:Reference Uri="https://sap.github.io/odata-vocabularies/vocabularies/HTML5.xml">
    <edmx:Include Namespace="com.sap.vocabularies.HTML5.v1" Alias="HTML5"/>
</edmx:Reference>

<Record Type="UI.DataFieldWithUrl">
    <PropertyValue Property="Label" String="Google" />
    <PropertyValue Property="Value" Path="LinkLabel" />
    <PropertyValue Property="Url"   String="https://www.google.de" />
    <Annotation Term="HTML5.LinkTarget" String="_blank"/>
</Record>
```

> **Best Practice:** Das URL-Feld in `metadata.xml` als `Edm.String` definieren und mit `Core.IsURL Bool="true"` annotieren, damit die Validierung greift.

> **Wichtig:** `UI.DataFieldWithUrl` rendert einen klickbaren Hyperlink in der Tabelle. `UI.DataFieldForIntentBasedNavigation` hingegen rendert einen **Button** (keinen Link) und wird für App-zu-App Navigation via SemanticObject/Action verwendet.

#### Vollständiges Beispiel aus fiorilistreport: Link-Spalte in Orders-Tabelle

**annotation.xml:**
```xml
<!-- HTML5-Vocabulary für LinkTarget (_blank = neuer Tab) -->
<edmx:Reference Uri="https://sap.github.io/odata-vocabularies/vocabularies/HTML5.xml">
    <edmx:Include Namespace="com.sap.vocabularies.HTML5.v1" Alias="HTML5"/>
</edmx:Reference>

<Annotation Term="UI.LineItem" Qualifier="TAB2">
    <Collection>
        <!-- ... andere Spalten ... -->
        <Record Type="UI.DataFieldWithUrl">
            <PropertyValue Property="Label" String="Link" />
            <PropertyValue Property="Value" Path="Link" />
            <PropertyValue Property="Url" String="https://www.google.de" />
            <Annotation Term="HTML5.LinkTarget" String="_blank"/>
        </Record>
    </Collection>
</Annotation>
```

**metadata.xml:**
```xml
<Property Name="Link" Type="Edm.String" MaxLength="40"/>
```

**Mock-Daten (SalesOrders.json):**
```json
{
  "OrderID": "ORD001",
  "Link": "TestApp"
}
```

#### DataFieldWithUrl vs. DataFieldForIntentBasedNavigation

| Merkmal | `DataFieldWithUrl` | `DataFieldForIntentBasedNavigation` |
|---|---|---|
| UI-Element | Klickbarer **Hyperlink** | **Button** |
| URL-Typ | Direkt (http://..., https://..., #hash) | SemanticObject + Action |
| Öffnet in neuem Tab | ✅ via `HTML5.LinkTarget` | ❌ (immer im FLP-Kontext) |
| Parameter-Übergabe | Manuell in URL | Via `Mapping` (SemanticObjectMappingType) |
| FLP Target Mapping | Nicht erforderlich | Erforderlich (Ziel-App muss registriert sein) |
| Mock-Betrieb | ✅ Funktioniert sofort | ⚠️ Nur mit registrierter Ziel-App |
| Use Case | Externe Links, Docs, Google | App-zu-App Navigation im FLP |

---

### UI.ConnectedFields

**Zweck:** Kombiniert mehrere Properties zu einem einzigen angezeigten Feld mit Trenner — z.B. Vorname + Nachname → "Max Mustermann".

**Target:** `EntityType` — definiert eine benannte Gruppe. Wird per `AnnotationPath` in `UI.FieldGroup` referenziert.

#### XML-Syntax
```xml
<!-- Definition der ConnectedFields: -->
<Annotations Target="MyService.Contact">
  <Annotation Term="UI.ConnectedFields" Qualifier="FullName">
    <Record Type="UI.ConnectedFieldsType">
      <PropertyValue Property="Label" String="Vollständiger Name" />
      <!-- Trenner zwischen den Feldern: -->
      <PropertyValue Property="Template" String="{0} {1}" />
      <PropertyValue Property="Data">
        <Record>
          <!-- Felder in Reihenfolge der Template-Platzhalter: -->
          <PropertyValue Property="Field1">
            <Record Type="UI.DataField">
              <PropertyValue Property="Value" Path="FirstName" />
            </Record>
          </PropertyValue>
          <PropertyValue Property="Field2">
            <Record Type="UI.DataField">
              <PropertyValue Property="Value" Path="LastName" />
            </Record>
          </PropertyValue>
        </Record>
      </PropertyValue>
    </Record>
  </Annotation>
</Annotations>

<!-- Verwendung in einem FieldGroup: -->
<Annotation Term="UI.FieldGroup" Qualifier="PersonalData">
  <Record Type="UI.FieldGroupType">
    <PropertyValue Property="Data">
      <Collection>
        <Record Type="UI.DataFieldForAnnotation">
          <PropertyValue Property="Target" AnnotationPath="@UI.ConnectedFields#FullName" />
          <PropertyValue Property="Label"  String="Name" />
        </Record>
      </Collection>
    </PropertyValue>
  </Record>
</Annotation>
```

> **Template-Syntax:** `"{0} {1}"` — Platzhalter sind die Keys der `Data`-Record-Properties (`Field1`, `Field2` usw.)

---

### UI.QuickViewFacets

**Zweck:** Definiert den Inhalt eines **Quick View Popups** — erscheint wenn der Benutzer auf einen Link in einer Tabellenspalte klickt (ohne Navigation zur ObjectPage).

**Target:** `EntityType` — der EntityType dessen Daten im Popup erscheinen (typisch: eine verknüpfte Entität wie Lieferant, Kunde).

> **Voraussetzung:** Das auslösende Feld muss eine `NavigationProperty` haben und in `Common.SemanticObject` annotiert sein oder per `DataFieldWithNavigationPath` referenziert werden.

#### XML-Syntax
```xml
<!-- Auf der Ziel-Entität (z.B. Supplier), die im Popup erscheint: -->
<Annotations Target="MyService.Supplier">
  <Annotation Term="UI.QuickViewFacets">
    <Collection>
      <Record Type="UI.ReferenceFacet">
        <PropertyValue Property="Target" AnnotationPath="@UI.FieldGroup#SupplierInfo" />
      </Record>
    </Collection>
  </Annotation>

  <Annotation Term="UI.FieldGroup" Qualifier="SupplierInfo">
    <Record Type="UI.FieldGroupType">
      <PropertyValue Property="Data">
        <Collection>
          <Record Type="UI.DataField">
            <PropertyValue Property="Label" String="Name" />
            <PropertyValue Property="Value" Path="SupplierName" />
          </Record>
          <Record Type="UI.DataField">
            <PropertyValue Property="Label" String="Stadt" />
            <PropertyValue Property="Value" Path="City" />
          </Record>
          <Record Type="UI.DataField">
            <PropertyValue Property="Label" String="Land" />
            <PropertyValue Property="Value" Path="Country" />
          </Record>
        </Collection>
      </PropertyValue>
    </Record>
  </Annotation>
</Annotations>

<!-- In der aufrufenden Entität: Navigation-Feld als Link darstellen: -->
<Annotations Target="MyService.PurchaseOrder/SupplierID">
  <Annotation Term="Common.SemanticObject" String="Supplier" />
</Annotations>
```

#### Visuelle Wirkung
```
  Tabelle:
  │ PO-Nr │ Lieferant ──────────┐  │ Betrag │
  │ 4500  │ [SAP SE]  ↓Popup    │  │ 1.200  │
  └───────┴────────────────────┘  └────────┘
                  │
          ┌───────▼────────┐
          │ SAP SE         │  ← QuickViewFacets
          │ Stadt: Walldorf│
          │ Land:  DE      │
          └────────────────┘
```

---

### UI.Badge

**Zweck:** Definiert eine kompakte Karten-Ansicht (Badge-Card) für eine Entität — z.B. in einer OVP-Kachel oder als Kurzvorschau. Enthält Titel, Untertitel und einen Hauptwert.

**Target:** `EntityType`

#### XML-Syntax
```xml
<Annotations Target="MyService.Product">
  <Annotation Term="UI.Badge">
    <Record Type="UI.BadgeType">
      <!-- Typ-Label (oben links in der Kachel): -->
      <PropertyValue Property="TypeImageUrl" String="sap-icon://product" />
      <!-- Haupttitel: -->
      <PropertyValue Property="Title">
        <Record Type="UI.DataField">
          <PropertyValue Property="Value" Path="ProductName" />
        </Record>
      </PropertyValue>
      <!-- Untertitel: -->
      <PropertyValue Property="Description">
        <Record Type="UI.DataField">
          <PropertyValue Property="Value" Path="Category" />
        </Record>
      </PropertyValue>
      <!-- Hauptkennzahl (groß angezeigt): -->
      <PropertyValue Property="MainInfo">
        <Record Type="UI.DataField">
          <PropertyValue Property="Label" String="Preis" />
          <PropertyValue Property="Value" Path="Price" />
        </Record>
      </PropertyValue>
    </Record>
  </Annotation>
</Annotations>
```

---

### UI.RecommendedVisualization

**Zweck:** Gibt dem Framework einen Hinweis, welche Visualisierungsform bevorzugt werden soll — z.B. `LineChart` für Zeitreihen oder `BarChart` für Vergleiche. Wird von OVP und Smart Controls ausgewertet.

**Target:** `EntityType` oder `EntitySet`

#### Verfügbare Visualisierungstypen
| EnumMember | Bedeutung |
|---|---|
| `UI.VisualizationType/LineChart` | Liniendiagramm (Zeitreihe) |
| `UI.VisualizationType/BarChart` | Balkendiagramm (Vergleich) |
| `UI.VisualizationType/DonutChart` | Kreisdiagramm (Anteile) |
| `UI.VisualizationType/ScatterPlot` | Streudiagramm |
| `UI.VisualizationType/BubbleChart` | Blasendiagramm |

#### XML-Syntax
```xml
<Annotations Target="MyService.SalesData">
  <Annotation Term="UI.RecommendedVisualization">
    <Record>
      <PropertyValue Property="Visualization"
        EnumMember="UI.VisualizationType/LineChart" />
    </Record>
  </Annotation>
</Annotations>
```

> In der Praxis wird die Visualisierung meist explizit über `UI.Chart` gesteuert. `UI.RecommendedVisualization` ist ein Hinweis für Tools (z.B. Fiori Elements Page Map, SAP Analytics Cloud), nicht zwingend für das Runtime-Rendering.

---

## 4. Common Vocabulary

> Namespace: `com.sap.vocabularies.Common.v1` | Alias: `Common`

---

### Common.Text

**Zweck:** Verweist auf ein Textfeld in einer assoziierten Entität. Wird immer zusammen mit `UI.TextArrangement` genutzt.

**Target:** `Property` — das Feld bekommt einen "Text-Lieferanten". Der Pfad kann über eine Navigation-Property gehen (`to_Status/Text`) wenn der EntityType eine Association zur Text-Tabelle hat.

**Projekt:** `problemsolvingovp`

> **Voraussetzung:** Der EntityType muss eine **Navigation Property** zur Text-Entität haben (in metadata.xml definiert als `NavigationProperty`). Ohne Association funktioniert der Path nicht.

#### XML-Syntax
```xml
<Annotations Target="QM_PROBSOLVPROC_EXECT_SRV.C_ProbSolvingProcExectType/ProbSolvingProcLifecycleStatus">
  <!-- Pfad zum Textfeld über eine Navigation-Property -->
  <Annotation Term="Common.Text" Path="to_ProbSolvingProcLfcycSts/ProbSolvingProcLifecycleStatusText" />
  <Annotation Term="UI.TextArrangement" EnumMember="UI.TextArrangementType/TextOnly" />
</Annotations>
```

#### Alternative: Inline-Annotation (verschachtelter Stil)

Statt zwei getrennter `<Annotation>`-Elemente kann `UI.TextArrangement` **direkt in** `Common.Text` verschachtelt werden:

```xml
<!-- problemsolvingovp: TextArrangement als Kind-Annotation von Common.Text -->
<Annotations Target="QM_PROBSOLVPROC_EXECT_SRV.C_ProbSolvingProcExectType/ProbSolvingProcLifecycleStatus">
  <Annotation Term="Common.Text" Path="to_ProbSolvingProcLfcycSts/ProbSolvingProcLifecycleStatus_Text">
    <!-- Inline: TextArrangement als Annotation AN Common.Text -->
    <Annotation Term="UI.TextArrangement" EnumMember="UI.TextArrangementType/TextOnly"/>
  </Annotation>
</Annotations>
```

> Beide Schreibweisen sind semantisch identisch. Der Inline-Stil ist kompakter, der getrennte Stil ist leichter lesbar.

---

### Common.Label

**Zweck:** Überschreibt das Label eines Properties. Wird auch in OVP mit i18n-Bindings genutzt.

**Target:** `Property` — wirkt auf das Label dieses Feldes überall in der UI (Filter-Bar, Tabellen-Spalte, Formular-Label). Überschreibt den Standardnamen aus dem EntityType-Schema.

**Projekte:** `fiorielements`, `fioriovp`

#### XML-Syntax
```xml
<!-- Festes Label: -->
<Annotations Target="ProductService.Product/Price">
  <Annotation Term="Common.Label" String="Verkaufspreis" />
</Annotations>

<!-- i18n-Label (fioriovp): -->
<Annotations Target="OrderService.Order/Status">
  <Annotation Term="Common.Label" String="{@i18n>labelOrderStatus}" />
</Annotations>
```

---

### Common.SemanticObject

**Zweck:** Markiert ein Feld als navigierbar — Klick auf den Wert öffnet eine andere Fiori-App (Intent-basierte Navigation).

**Target:** `Property` — macht den Wert dieses Feldes zu einem anklickbaren Link. Das Fiori Launchpad sucht nach einer registrierten App mit dem entsprechenden SemanticObject und zeigt einen Navigations-Popover.

**Projekte:** `problemsolvingovp`, `fiorilistreport` (als Referenz)

> **Umfassende Dokumentation:** Siehe [Kapitel 7 — Semantic Object Navigation](#7-semantic-object-navigation-app-zu-app-navigation) für alle Varianten (`Common.SemanticObject`, `DataFieldWithIntentBasedNavigation`, `DataFieldForIntentBasedNavigation`), manifest.json Cross Navigation, Parameter-Mapping und Troubleshooting.

#### XML-Syntax
```xml
<Annotations Target="QM_PROBSOLVPROC_EXECT_SRV.C_ProbSolvingProcExectType/ProbSolvingProc">
  <!-- Name des Semantic Objects in der Fiori Launchpad Konfiguration -->
  <Annotation Term="Common.SemanticObject" String="QM_ProbSolvingProcess" />
</Annotations>
```

> Das Fiori Launchpad muss eine App-Kachel mit passendem SemanticObject registrieren. Im LocalLaunchpad (`flpSandbox.html`) muss die entsprechende Konfiguration vorhanden sein.

---

### Common.ValueList

**Zweck:** F4-Hilfe / Value-Help-Dropdown für ein Eingabefeld. Definiert, aus welcher Entity-Collection die Auswahl kommt und welche Felder angezeigt / übernommen werden.

**Target:** `Property` — aktiviert einen F4-Button (Lupe) neben dem Eingabefeld. Beim Klick oder Drücken von F4 öffnet sich ein Dialog mit den Daten aus dem angegebenen `CollectionPath`.

**Projekte:** `fiorielements`, `fioriovp`, `problemsolvingovp`

#### Wie der Value-Help-Dialog funktioniert

```
  Benutzer tippt in Feld "Status" → drückt F4
       │
       ▼
  Fiori Elements liest Common.ValueList an Property "Status"
       │
       ├─ CollectionPath="StatusValues"  → GET /StatusValues
       ├─ SearchSupported=true           → Suchfeld anzeigen
       │
       ▼
  Dialog zeigt Tabelle:
  ┌──────────┬──────────────────┐
  │ Status   │ StatusText       │  ← InOut-Spalte │ DisplayOnly-Spalte
  ├──────────┼──────────────────┤
  │ OPEN     │ Offen            │
  │ CLOSED   │ Geschlossen      │
  │ PENDING  │ In Bearbeitung   │
  └──────────┴──────────────────┘
       │
  Benutzer wählt "OPEN"
       │
       ▼
  LocalDataProperty "Status" wird auf "OPEN" gesetzt
```

#### XML-Syntax (vollständig)
```xml
<Annotations Target="MyService.Order/Status">
  <Annotation Term="Common.ValueList">
    <Record Type="Common.ValueListType">
      <!-- Entity-Set aus dem die Werte geladen werden -->
      <PropertyValue Property="CollectionPath"   String="StatusValues" />
      <!-- Label für die Value-Help-Dialog-Überschrift -->
      <PropertyValue Property="Label"            String="Bestellstatus" />
      <!-- Volltextsuche im Dialog erlauben -->
      <PropertyValue Property="SearchSupported"  Bool="true" />
      <PropertyValue Property="Parameters">
        <Collection>

          <!-- ① InOut: Wert wird in beide Richtungen übertragen -->
          <Record Type="Common.ValueListParameterInOut">
            <!-- Feld in der Haupt-Entity -->
            <PropertyValue Property="LocalDataProperty"  PropertyPath="Status" />
            <!-- Feld in der ValueList-Entity -->
            <PropertyValue Property="ValueListProperty"  String="Status" />
          </Record>

          <!-- ② DisplayOnly: Nur zur Anzeige, wird nicht übernommen -->
          <Record Type="Common.ValueListParameterDisplayOnly">
            <PropertyValue Property="ValueListProperty" String="StatusText" />
            <!-- Optional: Priorität in der Spaltenanzeige -->
            <Annotation Term="UI.Importance" EnumMember="UI.ImportanceType/High"/>
          </Record>

          <!-- ③ Out: Wird aus ValueList in Haupt-Entity übertragen (kein Suchfeld) -->
          <!-- <Record Type="Common.ValueListParameterOut"> -->

          <!-- ④ In: Nur als Suchparameter, kein Transfer -->
          <!-- <Record Type="Common.ValueListParameterIn"> -->

        </Collection>
      </PropertyValue>
    </Record>
  </Annotation>
</Annotations>
```

#### Parameter-Typen
| Record Type | Richtung | Zweck |
|---|---|---|
| `ValueListParameterInOut` | Hin & Her | Suchfeld + Rückgabe |
| `ValueListParameterDisplayOnly` | Nur Anzeige | Info-Spalte, keine Übernahme |
| `ValueListParameterOut` | ValueList → Haupt | Wert übernehmen, kein Suchfeld |
| `ValueListParameterIn` | Haupt → ValueList | Vorfilter für Value-Help |

#### Reale Beispiele

**fiorielements** – Kategorie-Auswahl aus einer Category-Entity:
```xml
<Annotations Target="ProductService.Product/Category">
  <Annotation Term="Common.ValueList">
    <Record Type="Common.ValueListType">
      <PropertyValue Property="CollectionPath" String="Categories" />
      <PropertyValue Property="Parameters">
        <Collection>
          <Record Type="Common.ValueListParameterInOut">
            <PropertyValue Property="LocalDataProperty" PropertyPath="Category" />
            <PropertyValue Property="ValueListProperty" String="Name" />
          </Record>
          <Record Type="Common.ValueListParameterDisplayOnly">
            <PropertyValue Property="ValueListProperty" String="Description" />
          </Record>
        </Collection>
      </PropertyValue>
    </Record>
  </Annotation>
</Annotations>
```

**problemsolvingovp** – Benutzer-Suche mit Volltextsuche + Abteilung:
```xml
<Annotations Target="...C_ProbSolvingProcExectType/CreatedByUser">
  <Annotation Term="Common.ValueList">
    <Record>
      <PropertyValue Property="CollectionPath"  String="I_UserContactCard" />
      <PropertyValue Property="SearchSupported" Bool="true" />
      <PropertyValue Property="Parameters">
        <Collection>
          <Record Type="Common.ValueListParameterInOut">
            <PropertyValue Property="LocalDataProperty" PropertyPath="CreatedByUser" />
            <PropertyValue Property="ValueListProperty" String="UserID" />
          </Record>
          <Record Type="Common.ValueListParameterDisplayOnly">
            <PropertyValue Property="ValueListProperty" String="FullName" />
          </Record>
          <Record Type="Common.ValueListParameterDisplayOnly">
            <PropertyValue Property="ValueListProperty" String="Department" />
          </Record>
        </Collection>
      </PropertyValue>
    </Record>
  </Annotation>
</Annotations>
```

#### CDS-Äquivalent (RAP)
```abap
-- Direktes Value-Help in CDS:
@Consumption.valueHelpDefinition: [{
  entity: { name: 'ZI_CATEGORY', element: 'Name' }
}]
Category,
```

---

### Common.ValueListWithFixedValues

**Zweck:** Schränkt die Eingabe auf die Werte der ValueList ein — kein Freitext. Kombiniert immer mit `Common.ValueList`.

**Target:** `Property` — bei `Bool="true"` rendert Fiori Elements das Filterfeld als **Dropdown-Combobox** statt als Freitexteingabe. Der Benutzer kann nur aus der ValueList auswählen.

**Projekte:** `fioriovp`, `problemsolvingovp`

| Bool-Wert | Verhalten |
|---|---|
| `true` | Dropdown-Modus: nur vorgegebene Werte auswählbar |
| `false` | F4-Dialog wird geöffnet, aber Freitext bleibt möglich |

#### XML-Syntax
```xml
<Annotations Target="MyService.Order/Status">
  <!-- Bool=true: NUR Werte aus der ValueList erlaubt (Dropdown) -->
  <Annotation Term="Common.ValueListWithFixedValues" Bool="true" />
  <!-- Bool=false: Eingabe erlaubt, aber ValueList wird angeboten -->
  <Annotation Term="Common.ValueListWithFixedValues" Bool="false" />

  <!-- Immer zusammen mit Common.ValueList: -->
  <Annotation Term="Common.ValueList">
    ...
  </Annotation>
</Annotations>
```

---

## 5. Aggregation Vocabulary

> Namespace: `Org.OData.Aggregation.V1` | Alias: `Aggregation`

> **Wichtig:** Diese Annotationen stehen in der **metadata.xml**, nicht in annotation.xml!

---

### Aggregation.ApplySupported

**Zweck:** Aktiviert OData `$apply`-Anfragen auf einem EntitySet — **Pflicht für Charts** in Fiori Elements V4.

**Target:** `EntitySet` im EntityContainer (z.B. `MainService.MainContainer/SalesOrders`) — **nicht** der EntityType! Dies ist eine technische Capability-Deklaration des OData-Services. Steht daher in `metadata.xml`, nicht in `annotation.xml`.

**Projekt:** `fiorilistreport`

> **Warum EntitySet statt EntityType?** Der EntityType beschreibt die *Struktur* der Daten. Das EntitySet ist der konkrete *Endpunkt* (URL), den der Service bereitstellt. `$apply` ist eine Anfrage an den Endpunkt — daher wird die Capability am Endpunkt annotiert.

#### Was $apply macht
```
  Ohne $apply (normale Tabelle):
  GET /SalesOrders?$select=OrderID,CustomerName,NetAmount
  → Alle Einzelzeilen werden zurückgegeben

  Mit $apply (Chart-Aggregation):
  GET /SalesOrders?$apply=groupby((CustomerName),aggregate(NetAmount with sum as NetAmount))
  → Aggregierte Zeilen: eine Zeile pro CustomerName mit SUM(NetAmount)
```

#### XML-Syntax (in metadata.xml)
```xml
<!-- Im Schema-Bereich der metadata.xml: -->
<Annotations Target="MainService.MainContainer/SalesOrders">
  <Annotation Term="Aggregation.ApplySupported">
    <Record Type="Aggregation.ApplySupportedType">
      <!-- Welche Properties können aggregiert werden -->
      <PropertyValue Property="AggregatableProperties">
        <Collection>
          <Record Type="Aggregation.AggregatablePropertyType">
            <PropertyValue Property="Property" PropertyPath="NetAmount" />
          </Record>
        </Collection>
      </PropertyValue>
    </Record>
  </Annotation>
</Annotations>
```

---

### Aggregation.CustomAggregate

**Zweck:** Deklariert eine benutzerdefinierte Aggregation (z.B. SUM eines Betragsfeldes).

**Target:** `EntitySet` im EntityContainer — gleiche Logik wie `ApplySupported`. Der Qualifier gibt den Namen der aggregierten Property an, der String-Wert den Rückgabetyp.

**Projekt:** `fiorilistreport`

#### XML-Syntax (in metadata.xml)
```xml
<Annotations Target="MainService.MainContainer/SalesOrders">
  <!-- Definiert NetAmount als SUM-Aggregation -->
  <Annotation Term="Aggregation.CustomAggregate"
              Qualifier="NetAmount" String="Edm.Decimal" />
</Annotations>
```

#### CDS-Äquivalent
```abap
@DefaultAggregation: #SUM
NetAmount,
```

---

## 6. Measures Vocabulary

> Namespace: `Org.OData.Measures.V1` | Alias: `Measures`

---

### Measures.ISOCurrency

**Zweck:** Verknüpft ein Dezimalfeld mit dem Feld, das den Währungscode enthält. Fiori Elements zeigt dann automatisch die Währungseinheit an.

**Target:** `Property` (das Dezimalfeld, z.B. `Price`) — der `Path`-Wert zeigt auf eine andere Property **desselben EntityType**, die den ISO-4217-Währungscode enthält (z.B. `Currency` mit Wert `"EUR"`).

**Projekt:** `fiorielements`

#### XML-Syntax
```xml
<Annotations Target="ProductService.Product/Price">
  <!-- Path auf das Feld, das den Währungscode enthält -->
  <Annotation Term="Measures.ISOCurrency" Path="Currency" />
</Annotations>
```

#### Wirkung
```
Price-Spalte zeigt: "99,95 EUR"  (statt nur "99.95")
```

---

## 7. Semantic Object Navigation (App-zu-App Navigation)

**Zweck:** Navigation von einer Fiori Elements App zu einer anderen App über das Fiori Launchpad. Es gibt **drei Varianten**, die unterschiedliche UI-Elemente erzeugen:

| Variante | UI-Element | Verwendung |
|---|---|---|
| `Common.SemanticObject` | Klickbarer **Feldwert** (implizit) | Property wird überall als Semantic Link gerendert |
| `UI.DataFieldWithIntentBasedNavigation` | Klickbarer **Hyperlink** | Expliziter Link in LineItem/FieldGroup |
| `UI.DataFieldForIntentBasedNavigation` | **Button** | Button in Toolbar, LineItem oder FieldGroup |

**Projekt:** `problemsolvingovp` (DataFieldForIntentBasedNavigation), `fiorilistreport` (DataFieldWithUrl als Alternative)

---

### 7.1 Common.SemanticObject — Globale Property-Annotation

**Zweck:** Annotiert eine Property mit einem Semantic Object. Überall wo diese Property angezeigt wird, erscheint sie automatisch als klickbarer Semantic Link.

**Target:** `Property` (z.B. `MainService.SalesOrder/OrderID`)

#### XML-Syntax
```xml
<!-- OrderID wird überall als Semantic Link "Order" gerendert -->
<Annotations Target="MainService.SalesOrder/OrderID">
    <Annotation Term="Common.SemanticObject" String="Order"/>
</Annotations>

<!-- Dynamisches Semantic Object (Name kommt aus einer Property) -->
<Annotations Target="MainService.SalesOrder/LinkTarget">
    <Annotation Term="Common.SemanticObject" Path="SemanticObjectName"/>
</Annotations>
```

#### Visuelle Wirkung
```
┌──────────────────────────────────────────────┐
│ Tabelle:                                     │
│ OrderID   │ Customer   │ Amount              │
│ ORD001    │ Acme Corp  │ 5.999,95            │
│   ↑                                         │
│ Klickbarer Semantic Link → zeigt Popover    │
│ mit allen verfügbaren Actions für "Order"   │
└──────────────────────────────────────────────┘
```

> **Wichtig:** Wenn es **nur eine Action** für das Semantic Object gibt, navigiert der Klick direkt. Bei **mehreren Actions** erscheint ein Popover zur Auswahl.

---

### 7.2 UI.DataFieldWithIntentBasedNavigation — Semantic Link als Hyperlink

**Zweck:** Erzeugt einen klickbaren **Hyperlink** (kein Button!) für die Intent-basierte Navigation. Im Gegensatz zu `DataFieldForIntentBasedNavigation` (Button) wird hier ein normaler Link gerendert.

**Target:** Als Record-Typ innerhalb von `UI.LineItem`, `UI.FieldGroup` oder `UI.Identification`

#### XML-Syntax
```xml
<!-- Hyperlink in der Tabelle: "In App öffnen" → Order display -->
<Record Type="UI.DataFieldWithIntentBasedNavigation">
    <PropertyValue Property="Label" String="In App öffnen" />
    <!-- Anzeigetext des Links (Property-Wert) -->
    <PropertyValue Property="Value" Path="OrderID" />
    <!-- Semantic Object = Ziel-App im Launchpad -->
    <PropertyValue Property="SemanticObject" String="Order" />
    <!-- Action = Launchpad-Aktion -->
    <PropertyValue Property="Action" String="display" />
</Record>

<!-- Mit Mapping: OrderID wird als Parameter übergeben -->
<Record Type="UI.DataFieldWithIntentBasedNavigation">
    <PropertyValue Property="Label" String="Order Details" />
    <PropertyValue Property="Value" Path="OrderID" />
    <PropertyValue Property="SemanticObject" String="Order" />
    <PropertyValue Property="Action" String="display" />
    <PropertyValue Property="Mapping">
        <Collection>
            <Record Type="Common.SemanticObjectMappingType">
                <PropertyValue Property="LocalProperty" PropertyPath="OrderID" />
                <PropertyValue Property="SemanticObjectProperty" String="OrderID" />
            </Record>
        </Collection>
    </PropertyValue>
</Record>
```

#### CDS-Äquivalent
```abap
@UI.lineItem: [ {
    type: #WITH_INTENT_BASED_NAVIGATION,
    label: 'In App öffnen',
    semanticObjectAction: 'display'
} ]
@Consumption.semanticObject: 'Order'
OrderID;
```

---

### 7.3 UI.DataFieldForIntentBasedNavigation — Semantic Button

**Zweck:** Erzeugt einen **Button** für die Intent-basierte Navigation. Wird in Tabellen-Toolbars, ObjectPage-Headern oder als Inline-Button verwendet.

**Target:** Als Record-Typ innerhalb von `UI.LineItem`, `UI.FieldGroup`, `UI.Identification` oder `UI.HeaderFacets`

#### XML-Syntax
```xml
<!-- Button in der Tabelle (OVP Table Card) -->
<Record Type="UI.DataFieldForIntentBasedNavigation">
    <PropertyValue Property="Label" String="In App öffnen"/>
    <PropertyValue Property="SemanticObject" String="QM_ProbSolvingProcess"/>
    <PropertyValue Property="Action" String="display"/>
    <PropertyValue Property="Mapping">
        <Collection>
            <Record Type="Common.SemanticObjectMappingType">
                <PropertyValue Property="LocalProperty" PropertyPath="ProbSolvingProc"/>
                <PropertyValue Property="SemanticObjectProperty" String="ProbSolvingProc"/>
            </Record>
        </Collection>
    </PropertyValue>
    <Annotation Term="UI.Importance" EnumMember="UI.ImportanceType/High"/>
</Record>

<!-- Button mit RequiresContext (nur aktiv wenn Zeile selektiert) -->
<Record Type="UI.DataFieldForIntentBasedNavigation">
    <PropertyValue Property="Label" String="Bearbeiten" />
    <PropertyValue Property="SemanticObject" String="SalesOrder" />
    <PropertyValue Property="Action" String="manage" />
    <PropertyValue Property="RequiresContext" Bool="true"/>
</Record>

<!-- Button bedingt anzeigen (NavigationAvailable) -->
<Record Type="UI.DataFieldForIntentBasedNavigation">
    <PropertyValue Property="Label" String="Freigeben" />
    <PropertyValue Property="SemanticObject" String="PurchaseOrder" />
    <PropertyValue Property="Action" String="approve" />
    <PropertyValue Property="NavigationAvailable" Path="IsEditable"/>
</Record>
```

#### Vollständiges Beispiel aus problemsolvingovp
```xml
<!-- In UI.Identification#TableActions für OVP Table Card -->
<Annotation Term="UI.Identification" Qualifier="TableActions">
    <Collection>
        <Record Type="UI.DataFieldForIntentBasedNavigation">
            <PropertyValue Property="Label" String="In App öffnen"/>
            <PropertyValue Property="SemanticObject" String="QM_ProbSolvingProcess"/>
            <PropertyValue Property="Action" String="display"/>
            <PropertyValue Property="Mapping">
                <Collection>
                    <Record Type="Common.SemanticObjectMappingType">
                        <PropertyValue Property="LocalProperty" PropertyPath="ProbSolvingProc"/>
                        <PropertyValue Property="SemanticObjectProperty" String="ProbSolvingProc"/>
                    </Record>
                </Collection>
            </PropertyValue>
            <Annotation Term="UI.Importance" EnumMember="UI.ImportanceType/High"/>
        </Record>
    </Collection>
</Annotation>
```

---

### 7.4 Common.SemanticObjectMapping — Parameter-Mapping

**Zweck:** Definiert wie lokale Properties an die Ziel-App übergeben werden. Die `Mapping`-Collection übersetzt lokale Feldnamen in Semantic-Object-Parameter.

#### XML-Syntax
```xml
<!-- Einfaches Mapping: OrderID → OrderID -->
<Annotation Term="Common.SemanticObjectMapping">
    <Collection>
        <Record Type="Common.SemanticObjectMappingType">
            <PropertyValue Property="LocalProperty" PropertyPath="OrderID"/>
            <PropertyValue Property="SemanticObjectProperty" String="OrderID"/>
        </Record>
    </Collection>
</Annotation>

<!-- Mapping mit Umbenennung: CustomerID → SoldToParty -->
<Annotation Term="Common.SemanticObjectMapping">
    <Collection>
        <Record Type="Common.SemanticObjectMappingType">
            <PropertyValue Property="LocalProperty" PropertyPath="CustomerID"/>
            <PropertyValue Property="SemanticObjectProperty" String="SoldToParty"/>
        </Record>
    </Collection>
</Annotation>

<!-- Mehrere Parameter gleichzeitig -->
<Annotation Term="Common.SemanticObjectMapping">
    <Collection>
        <Record Type="Common.SemanticObjectMappingType">
            <PropertyValue Property="LocalProperty" PropertyPath="OrderID"/>
            <PropertyValue Property="SemanticObjectProperty" String="OrderID"/>
        </Record>
        <Record Type="Common.SemanticObjectMappingType">
            <PropertyValue Property="LocalProperty" PropertyPath="OrderType"/>
            <PropertyValue Property="SemanticObjectProperty" String="Type"/>
        </Record>
    </Collection>
</Annotation>
```

> **Wo wird Mapping verwendet?**
> - `UI.DataFieldForIntentBasedNavigation` → `Mapping` Property
> - `UI.DataFieldWithIntentBasedNavigation` → `Mapping` Property
> - `Common.SemanticObject` auf Property → `Common.SemanticObjectMapping` am gleichen Target

---

### 7.5 manifest.json — Cross Navigation Konfiguration

**Zweck:** Definiert welche Semantic Objects diese App aufrufen kann (outbound) und welche Intents sie selbst bereitstellt (inbound).

#### Outbound Navigation (diese App ruft andere Apps auf)
```json
{
  "sap.app": {
    "crossNavigation": {
      "outbounds": {
        "Order-display": {
          "semanticObject": "Order",
          "action": "display",
          "title": "Order anzeigen",
          "signature": {
            "parameters": {
              "OrderID": {
                "required": true
              }
            }
          },
          "resolutionResult": {
            "applicationType": "URL",
            "url": "#Order-display?OrderID={OrderID}"
          }
        },
        "Product-manage": {
          "semanticObject": "Product",
          "action": "manage",
          "title": "Produkt verwalten",
          "signature": {
            "parameters": {
              "ProductID": {
                "required": true
              }
            }
          },
          "resolutionResult": {
            "applicationType": "SAPUI5",
            "additionalInformation": "SAPUI5.Component=com.example.productmanage",
            "url": ""
          }
        }
      }
    }
  }
}
```

#### Inbound Navigation (andere Apps rufen diese App auf)
```json
{
  "sap.app": {
    "crossNavigation": {
      "inbounds": {
        "Order-display": {
          "semanticObject": "Order",
          "action": "display",
          "title": "Order anzeigen",
          "signature": {
            "parameters": {
              "OrderID": {
                "required": true
              }
            }
          },
          "resolutionResult": {
            "applicationType": "SAPUI5",
            "additionalInformation": "SAPUI5.Component=com.example.orderdisplay",
            "url": ""
          }
        }
      }
    }
  }
}
```

#### Inbound-Parameter in der App verarbeiten
```javascript
// Component.js — Startup-Parameter auslesen
sap.ui.define(["sap/ui/core/UIComponent"], function(UIComponent) {
    return UIComponent.extend("com.example.orderdisplay.Component", {
        init: function() {
            UIComponent.prototype.init.apply(this, arguments);

            var oStartupParams = this.getComponentData() && this.getComponentData().startupParameters;
            if (oStartupParams && oStartupParams.OrderID) {
                var sOrderID = oStartupParams.OrderID[0]; // Werte sind immer Arrays!
                // Zur Detailseite navigieren
                this.getRouter().navTo("OrderObjectPage", { key: sOrderID });
            }
        }
    });
});
```

---

### 7.6 Programmatic Navigation per Controller Extension

**Zweck:** Intent-basierte Navigation aus JavaScript-Code (z.B. bei Button-Click in Controller Extension).

```javascript
sap.ui.define([
    "sap/ui/core/mvc/ControllerExtension",
    "sap/m/MessageBox"
], function(ControllerExtension, MessageBox) {
    'use strict';
    return ControllerExtension.extend(
        "com.example.fiorilistreport.ext.controller.OrderListExt",
        {
            // Navigation per Button-Click
            onNavigateToOrder: function(oEvent) {
                var oContext = oEvent.getSource().getBindingContext();
                var sOrderID = oContext.getProperty("OrderID");

                // CrossApplicationNavigation Service holen
                var oCrossAppNav = sap.ushell && sap.ushell.Container &&
                    sap.ushell.Container.getService("CrossApplicationNavigation");

                if (oCrossAppNav) {
                    oCrossAppNav.toExternal({
                        target: {
                            semanticObject: "Order",
                            action: "display"
                        },
                        params: {
                            OrderID: [sOrderID]  // Werte MÜSSEN Arrays sein!
                        }
                    });
                }
            }
        }
    );
});
```

---

### 7.7 Vergleich: Alle Navigations-Varianten

| Merkmal | `Common.SemanticObject` | `DataFieldWithIntentBasedNavigation` | `DataFieldForIntentBasedNavigation` | `DataFieldWithUrl` |
|---|---|---|---|---|
| UI-Element | Klickbarer **Feldwert** | **Hyperlink** | **Button** | **Hyperlink** |
| Ziel-Typ | SemanticObject | SemanticObject + Action | SemanticObject + Action | Direkte URL |
| Konfigurationsort | Property-Annotation | LineItem/FieldGroup Record | LineItem/FieldGroup/Identification Record | LineItem/FieldGroup Record |
| Parameter-Mapping | Via `Common.SemanticObjectMapping` | Via `Mapping` | Via `Mapping` | Manuell in URL |
| Neuer Tab | ❌ | ❌ (FLP-Kontext) | ❌ (FLP-Kontext) | ✅ via `HTML5.LinkTarget` |
| FLP Target Mapping | Erforderlich | Erforderlich | Erforderlich | Nicht erforderlich |
| Mock-Betrieb | ⚠️ Nur mit inbound config | ⚠️ Nur mit inbound config | ⚠️ Nur mit inbound config | ✅ Funktioniert sofort |
| Use Case | Feld wird implizit zum Link | Expliziter Semantic-Link | Button für Navigation | Externe URLs/Docs |

---

### 7.8 Typische Fehler & Troubleshooting

| Fehler | Ursache | Lösung |
|---|---|---|
| Link öffnet sich nicht | Kein inbound/outbound in manifest.json | `crossNavigation` Einträge hinzufügen |
| Popover statt direkter Navigation | Mehrere Actions für Semantic Object registriert | Auf eine Action reduzieren oder `DataFieldWithIntentBasedNavigation` mit expliziter Action nutzen |
| Parameter kommen nicht an | `Mapping` falsch konfiguriert | `LocalProperty` und `SemanticObjectProperty` prüfen |
| Button immer disabled | `RequiresContext="true"` aber keine Zeile selektiert | Zeile selektieren oder `RequiresContext` entfernen |
| Navigation im Mock-Betrieb fehlerhaft | FLP Sandbox hat keine Target Maps | `DataFieldWithUrl` mit Hash-URL als Fallback nutzen |
| Parameter-Werte als Array | `startupParameters` Werte sind immer Arrays | `oStartupParams.OrderID[0]` statt `oStartupParams.OrderID` |

---

## 8. UI.DataFieldForAction (eigenständig)

**Zweck:** Löst eine OData-Action (Backend) oder eine client-seitige Controller-Methode aus. Kann in `UI.LineItem`, `UI.FieldGroup` und `UI.Identification` vorkommen.

**Projekte:** `fiorielements`, `com.example.taskworklist`

#### Wo DataFieldForAction erscheint — Übersicht

```
  UI.LineItem                 UI.FieldGroup               UI.Identification
  ───────────                 ─────────────               ─────────────────
  → Button in Tabellen-       → Button im Formular-       → Button in ObjectPage-
    Toolbar (über der           Abschnitt (inline)          Header-Toolbar
    Tabelle)

  ⚠️ NICHT inline pro Zeile! Für echte Row-Buttons → XML-Fragment-Extension (Custom Column)
```

#### XML-Syntax

```xml
<!-- In UI.LineItem (erscheint in der Tabellen-Toolbar): -->
<Record Type="UI.DataFieldForAction">
  <PropertyValue Property="Label"              String="Genehmigen" />
  <!-- Vollständig qualifizierter Action-Name: Namespace.ActionName -->
  <PropertyValue Property="Action"             String="ProductService.approve" />
  <!-- Optional: Binding (immer Bound bei Entity-Actions) -->
  <PropertyValue Property="InvocationGrouping" EnumMember="UI.OperationGroupingType/Isolated" />
</Record>

<!-- In UI.Identification (erscheint in ObjectPage-Header-Toolbar): -->
<Record Type="UI.DataFieldForAction">
  <PropertyValue Property="Label"  String="Ablehnen" />
  <PropertyValue Property="Action" String="ProductService.reject" />
</Record>

<!-- In UI.FieldGroup (erscheint als Button im Abschnitt): -->
<Record Type="UI.DataFieldForAction">
  <PropertyValue Property="Label"  String="Update Stock" />
  <PropertyValue Property="Action" String="ProductService.updateStock" />
</Record>
```

#### Properties

| Property | Typ | Bedeutung |
|---|---|---|
| `Label` | String | Button-Beschriftung |
| `Action` | String | `Namespace.ActionName` — **nur für Backend OData-Actions**. Für client-seitige Logik → manifest.json `actions` (siehe Variante B) |
| `InvocationGrouping` | EnumMember | `Isolated` (Default) = jede Zeile einzeln, `ChangeSet` = alle in einer Transaktion |
| `Criticality` | EnumMember | Farbe: `Positive` = grün, `Negative` = rot, `Critical` = gelb |

---

### Variante A: Backend (OData-Action)

**Wann:** Wenn die Aktion serverseitige Zustandsänderungen auslöst (Status setzen, Workflow starten, persistieren).

**Ablauf:**
```
Benutzer klickt Button
      ↓
Fiori Elements sendet OData POST an Action-URL
      ↓
Backend verarbeitet Logik
      ↓
UI wird automatisch aktualisiert (Binding refresh)
```

#### Action-String Schema (Backend)
```
"TaskService.completeTask"
  ───────────  ────────────
  OData-       Action-Name
  Namespace    (in metadata.xml deklariert)
```

#### Annotation
```xml
<Record Type="UI.DataFieldForAction">
  <PropertyValue Property="Label"  String="Complete" />
  <!-- Format: ServiceNamespace.ActionName -->
  <PropertyValue Property="Action" String="TaskService.completeTask" />
</Record>
```

#### Voraussetzung: Action in metadata.xml deklarieren

```xml
<!-- Bound Action (an einen EntityType gebunden): -->
<Action Name="completeTask" IsBound="true">
  <!-- bindingParameter = die Entität, auf der die Action ausgeführt wird -->
  <Parameter Name="bindingParameter" Type="TaskService.Task" Nullable="false" />
  <!-- Optionaler Rückgabetyp -->
  <ReturnType Type="Edm.Boolean" />
</Action>

<!-- Im EntityContainer verknüpfen: -->
<ActionImport Name="completeTask" Action="TaskService.completeTask" />
```

> **Bound vs. Unbound:**
> - **Bound** (`IsBound="true"`): Wirkt auf eine konkrete Entität. Button ist nur aktiv wenn eine Zeile selektiert ist.
> - **Unbound**: Wirkt global. Button ist immer aktiv (kein Zeilen-Kontext nötig).

#### Mock-Handler (für lokalen Betrieb)

Im Mock-Betrieb muss ein Handler registriert werden, sonst schlägt der Aufruf fehl:

```javascript
// filepath: webapp/localService/mockserver.js
{
  method: "POST",
  path: new RegExp("completeTask"),
  response: function(xhr) {
    xhr.respondJSON(200, {}, { value: true });
  }
}
```

---

### Variante B: Client-seitig (Controller-Extension)

**Wann:** Wenn nur UI-Logik nötig ist (Dialog öffnen, Toast, lokale Modelländerung) oder kein Backend-Endpunkt verfügbar ist.

> ⚠️ **Wichtig:** `UI.DataFieldForAction` mit `AppID::MethodName` in der annotation.xml funktioniert **nicht** für client-seitige Methoden — das ist ausschließlich für Backend OData-Actions. Client-seitige Aktionen müssen über `manifest.json` + `.extension.`-Prefix definiert werden.

**Ablauf:**
```
Benutzer klickt Button
      ↓
manifest.json: actions → press: ".extension.<FQN>.<MethodName>"
      ↓  (kein OData-Aufruf)
Fiori Elements sucht registrierte ControllerExtension
      ↓
Ruft CompleteTask(oEvent) auf
      ↓
Client-seitige Logik wird ausgeführt
```

#### Schritt 1: Action in manifest.json unter `controlConfiguration` definieren

Die `actions`-Definition sitzt **innerhalb der Target-Settings** des jeweiligen Floorplans, unterhalb von `controlConfiguration`:

```
manifest.json
└── sap.ui5
    └── routing
        └── targets
            └── <TargetName>          ← z.B. "TaskList"
                └── options
                    └── settings
                        ├── controlConfiguration
                        │   └── @com.sap.vocabularies.UI.v1.LineItem
                        │       ├── tableSettings            ← Tabellentyp
                        │       └── actions                  ← ⭐ hier rein
                        │           └── <ActionKey>          ← frei wählbarer Key
                        │               ├── press            ← Handler-String
                        │               ├── text             ← Button-Label
                        │               └── enabled          ← true / false / Pfad
                        └── controllerExtensions             ← Extension registrieren
                            └── <ExtKey>: "<FQN>"
```

```json
"sap.ui5": {
  "routing": {
    "targets": {
      "TaskList": {
        "options": {
          "settings": {
            "controlConfiguration": {
              "@com.sap.vocabularies.UI.v1.LineItem": {
                "tableSettings": { "type": "ResponsiveTable" },
                "actions": {
                  "CustomCompleteAction": {           // ← frei wählbarer Key
                    "press": ".extension.com.example.taskworklist.com.example.taskworklist.ext.TaskListExtension.CompleteTask",
                    "enabled": true,                  // true | false | "{= ${status} === 'OPEN'}"
                    "text": "Complete"                // Button-Beschriftung
                  }
                }
              }
            },
            "controllerExtensions": {
              "TaskListExtension": "com.example.taskworklist.com.example.taskworklist.ext.TaskListExtension"
            }                         // ↑ Key frei wählbar   ↑ muss = controllerName
          }
        }
      }
    }
  }
}
```

#### Felder der `actions`-Definition im Detail

| Feld | Typ | Bedeutung |
|---|---|---|
| Key (`CustomCompleteAction`) | String | Frei wählbarer eindeutiger Bezeichner für die Action |
| `press` | String | Handler-Pfad: `.extension.<FQN>.<Methode>` |
| `text` | String | Beschriftung des Toolbar-Buttons |
| `enabled` | Boolean \| Expression | `true/false` oder Binding-Expression z.B. `"{= ${editable} === true}"` |
| `requiresSelection` | Boolean | `true` → Button nur aktiv wenn mind. 1 Zeile selektiert |
| `type` | String | `"ForAction"` (Standard) |

#### Press-Handler Schema
```
 ".extension. com.example.taskworklist.com.example.taskworklist.ext.TaskListExtension . CompleteTask"
  ──────────  ───────────────────────────────────────────────────────────────────────   ───────────
  Präfix      Vollqualifizierter Namespace                                               Methodenname
  (fix)       = ControllerExtension.extend("...") erster Parameter                      = JS-Methode
```

#### Schritt 2: ControllerExtension in `extends` registrieren

```json
"sap.ui5": {
  "extends": {
    "extensions": {
      "sap.ui.controllerExtensions": {
        "sap.fe.templates.ListReport.ListReportController#<AppId>::<TargetId>": {
          "controllerName": "com.example.taskworklist.com.example.taskworklist.ext.TaskListExtension"
        }
      }
    }
  }
}
```

> **Format des Keys:** `<TemplateController>#<sap.app/id>::<TargetId>`
>
> | Button erscheint in | Template Controller |
> |---|---|
> | ListReport / Worklist Toolbar | `sap.fe.templates.ListReport.ListReportController` |
> | ObjectPage Header-Toolbar | `sap.fe.templates.ObjectPage.ObjectPageController` |

> ⚠️ **`enableLazyLoading` muss `false` sein**, sonst wird der globale `extends`-Block ignoriert:
> ```json
> "sap.fe": { "app": { "enableLazyLoading": false } }
> ```

#### Schritt 3: Extension-Datei erstellen

```javascript
// filepath: webapp/ext/TaskListExtension.controller.js
sap.ui.define(["sap/ui/core/mvc/ControllerExtension", "sap/m/MessageToast"],
  function (ControllerExtension, MessageToast) {
  "use strict";

  return ControllerExtension.extend(
    // Muss identisch mit controllerName in manifest.json sein:
    "com.example.taskworklist.com.example.taskworklist.ext.TaskListExtension",
    {
      override: {
        onInit: function () {
          // wird nach Standard-Init aufgerufen
        }
      },

      // Methodenname muss identisch mit dem Teil nach dem letzten Punkt
      // im press-Handler sein:
      CompleteTask: function (oEvent) {
        MessageToast.show("Task abgeschlossen!");

        // Kontext der selektierten Zeile:
        // var oContext = oEvent.getSource().getBindingContext();
        // var oTask = oContext.getObject();
      }
    }
  );
});
```

#### Zusammenfassung — 3 Stellen müssen übereinstimmen

| Stelle | Wert |
|---|---|
| `controllerName` (manifest extends) | `...ext.TaskListExtension` |
| `controllerExtensions` Key-Value (manifest target) | `...ext.TaskListExtension` |
| `ControllerExtension.extend("...")` (JS-Datei) | `...ext.TaskListExtension` |
| `press`-Handler (manifest actions) | `".extension....ext.TaskListExtension.CompleteTask"` |
| Methodenname in JS | `CompleteTask` |

---

### Variante A vs. B — Entscheidungshilfe

| Kriterium | Backend (OData-Action) | Client-seitig (Controller-Extension) |
|---|---|---|
| **Datenpersistenz** | ✅ Serverseitig gespeichert | ❌ Nur im Browser |
| **Backend-Aufwand** | Action muss entwickelt werden | Nicht nötig |
| **Mock-Betrieb** | Mock-Handler nötig | Direkt lauffähig |
| **Standardkonformität** | ✅ SAP-Standard | ⚠️ Customization |
| **Typischer Use-Case** | Status setzen, Workflow, Persistenz | Dialog, Toast, Navigation, Vorschau |
| **Button aktiv ohne Selektion** | Nein (Bound Action) | Konfigurierbar |

---

### Echte Inline-Row-Buttons (Alternative zu DataFieldForAction)

`UI.DataFieldForAction` erzeugt **immer Toolbar-Buttons** (über der Tabelle), nie Buttons direkt in einer Zeile. Für echte Zeilen-Buttons ist eine **XML-Fragment-Extension (Custom Column)** nötig:

```
  Standard DataFieldForAction         Custom Column (XML-Fragment)
  ────────────────────────────        ──────────────────────────────
  [Complete] [Delete]                 │ Task  │ Status │ [✓]  │
  ↑ Toolbar (über Tabelle)            │ Task1 │ Open   │ [✓]  │  ← inline pro Zeile
                                      │ Task2 │ Closed │ [✓]  │
```

Custom Column-Extensions erfordern:
1. Konfiguration in `manifest.json` unter `sap.ui5/extends/extensions/sap.ui.viewExtensions`
2. Ein XML-Fragment mit `sap.m.Button` pro Zeile
3. Event-Handler in der Controller-Extension

---

## 9. Controller Extensions – Vollreferenz

Controller Extensions sind das offizielle SAP-Erweiterungskonzept für Fiori Elements-Apps. Sie erlauben es, Standard-Verhalten zu überschreiben oder eigene Methoden hinzuzufügen — **ohne den generierten Framework-Code zu verändern**.

### Was ist eine Controller Extension?

```
  Fiori Elements App
  ──────────────────
  sap.fe.templates.ListReport.ListReportController   ← Standard-Controller (unveränderlich)
           │
           │  extends / override
           ▼
  TaskListExtension.controller.js                    ← Deine Extension
           │
           ├── override.onInit()     → Lifecycle-Hook überschreiben
           ├── override.onAfterRendering()  → nach dem Rendern
           ├── CompleteTask()        → eigene Methode (per manifest-Button aufrufbar)
           └── openDialog()         → weitere eigene Methoden
```

**Einsatzbereiche:**
- Eigene Toolbar-Buttons mit Client-Logik (Toast, Dialog, Navigation)
- Standard-Lifecycle (onInit, onExit) überschreiben
- Binding-Kontexte lesen und verarbeiten
- Backend OData-Actions programmgesteuert (nicht per Annotation) aufrufen
- Custom Fragments in ObjectPage-Sections einbinden

---

### Registrierung in manifest.json

Eine Extension muss an **zwei Stellen** in der manifest.json eingetragen werden:

#### Stelle 1: `controllerExtensions` im Target (verbindet Extension mit dem Button-Routing)

```json
"targets": {
  "TaskList": {
    "options": {
      "settings": {
        "controllerExtensions": {
          "TaskListExtension": "com.example.taskworklist.com.example.taskworklist.ext.TaskListExtension"
          //  ↑                   ↑
          //  Kurzname (Key,       Vollqualifizierter Name — muss mit ControllerExtension.extend() übereinstimmen
          //  frei wählbar)        und dem Dateipfad unter webapp/ entsprechen
        }
      }
    }
  }
}
```

#### Stelle 2: `extends.extensions.sap.ui.controllerExtensions` (registriert die Extension am Framework)

```json
"sap.ui5": {
  "extends": {
    "extensions": {
      "sap.ui.controllerExtensions": {

        // Format: "<TemplateController>#<sap.app/id>::<TargetId>"
        "sap.fe.templates.ListReport.ListReportController#com.example.taskworklist.com.example.taskworklist::TaskList": {
          "controllerName": "com.example.taskworklist.com.example.taskworklist.ext.TaskListExtension"
        }

        // Für ObjectPage:
        // "sap.fe.templates.ObjectPage.ObjectPageController#com.example.myapp.myapp::ProductObjectPage": {
        //   "controllerName": "com.example.myapp.myapp.ext.ProductObjectPageExt"
        // }
      }
    }
  }
}
```

#### Template-Controller je nach Floorplan

| Floorplan | Template-Controller |
|---|---|
| List Report / Worklist | `sap.fe.templates.ListReport.ListReportController` |
| Object Page | `sap.fe.templates.ObjectPage.ObjectPageController` |
| Analytical List Page | `sap.fe.templates.AnalyticalListPage.AnalyticalListPageController` |

#### `#AppId::TargetId` — Scoped vs. Unscoped

```json
// Scoped (empfohlen): Extension gilt nur für dieses Target
"sap.fe.templates.ListReport.ListReportController#com.example.myapp::TaskList": { ... }

// Unscoped (veraltet): Extension gilt für ALLE ListReport-Controller der App
"sap.fe.templates.ListReport.ListReportController": { ... }
```

> ⚠️ **`enableLazyLoading` muss `false` sein**, damit der `extends`-Block verarbeitet wird:
> ```json
> "sap.fe": { "app": { "enableLazyLoading": false } }
> ```
> Mit `true` (Standard nach Generator) wird der globale `extends`-Block beim App-Start übersprungen.

---

### Die Extension-Datei

**Dateipfad:** `webapp/ext/<Name>.controller.js`  
Der Pfad muss dem Namespace entsprechen: `ext.TaskListExtension` → `webapp/ext/TaskListExtension.controller.js`

```javascript
// webapp/ext/TaskListExtension.controller.js
sap.ui.define([
    "sap/ui/core/mvc/ControllerExtension",
    "sap/m/MessageToast",
    "sap/m/MessageBox"
], function (ControllerExtension, MessageToast, MessageBox) {
    "use strict";

    return ControllerExtension.extend(
        // ① Vollqualifizierter Name — muss mit manifest.json controllerName übereinstimmen
        "com.example.taskworklist.com.example.taskworklist.ext.TaskListExtension",
        {
            // ② override-Block: Standard-Lifecycle-Hooks überschreiben
            override: {
                onInit: function () {
                    // Wird nach dem Standard-onInit des Framework-Controllers aufgerufen
                    console.log("Extension geladen");
                },
                onExit: function () {
                    // Aufräumen beim Verlassen der Seite
                }
            },

            // ③ Eigene Methoden — aufrufbar über manifest.json actions
            CompleteTask: function (oEvent) {
                // oEvent.getSource() = der Button
                // Binding-Kontext der selektierten Zeile:
                var oContext = oEvent.getSource().getBindingContext();
                if (oContext) {
                    var oTask = oContext.getObject();
                    MessageToast.show("Task '" + oTask.Task + "' abgeschlossen!");
                }
            },

            openInfoDialog: function (oEvent) {
                MessageBox.information("Info-Dialog");
            }
        }
    );
});
```

#### Namespace → Dateipfad — Mapping

```
Namespace:  com.example.taskworklist . com.example.taskworklist . ext . TaskListExtension
                                                                  ───   ──────────────────
                                                                  ↓     ↓
Dateipfad:  webapp/                                               ext/  TaskListExtension.controller.js
```

---

### override-Block: Lifecycle-Hooks

Mit `override` können Standard-Methoden des Framework-Controllers erweitert werden. Die Methode im Framework wird **vor** der Extension ausgeführt (außer bei `return`).

```javascript
override: {
    // Nach dem Standard-Init — View und Bindings sind bereits gesetzt
    onInit: function () {
        // View referenzieren:
        var oView = this.getView();
        // Eigenes Modell setzen:
        var oModel = new sap.ui.model.json.JSONModel({ editMode: false });
        oView.setModel(oModel, "viewModel");
    },

    // Vor dem Verlassen der Seite
    onExit: function () {
        // Event-Handler abmelden, Timeouts löschen
    },

    // Nach jedem Rendern
    onAfterRendering: function () {
        // DOM-Manipulation (selten nötig)
    },

    // Fiori Elements spezifisch — Routing-Parameter auslesen
    routing: {
        onBeforeBinding: function (oContext, mParameters) {
            // Wird aufgerufen bevor das Binding gesetzt wird
            // oContext = der neue Navigations-Kontext
        },
        onAfterBinding: function (oContext, mParameters) {
            // Wird aufgerufen nachdem das Binding gesetzt wurde
            // Hier sind die Daten bereits geladen
        }
    }
}
```

---

### OData Backend-Action aus der Extension aufrufen

Neben der Annotation-basierten Variante A (automatisch durch FE) kann man Backend-Actions auch **programmatisch** aus einer Extension aufrufen. Das ist nützlich wenn vor dem Aufruf ein Bestätigungs-Dialog erscheinen soll:

```javascript
CompleteTaskWithConfirm: function (oEvent) {
    var oContext = oEvent.getSource().getBindingContext();
    var oTask    = oContext.getObject();

    // ① Bestätigung einholen
    MessageBox.confirm(
        "Task '" + oTask.Task + "' wirklich abschließen?",
        {
            title: "Bestätigung",
            onClose: function (sAction) {
                if (sAction === MessageBox.Action.OK) {

                    // ② OData V4 Bound-Action aufrufen
                    var oModel   = oContext.getModel();
                    var oAction  = oModel.bindContext(
                        "TaskService.completeTask(...)",   // Action-Name aus metadata.xml
                        oContext                           // gebunden an diese Entität
                    );

                    oAction.execute().then(function () {
                        MessageToast.show("Task abgeschlossen!");
                        // ③ Binding refreshen damit die UI sich aktualisiert:
                        oContext.refresh();
                    }).catch(function (oError) {
                        MessageBox.error("Fehler: " + oError.message);
                    });
                }
            }
        }
    );
}
```

#### OData V4 Action-Aufruf — Parameter im Detail

| Schritt | Code | Bedeutung |
|---|---|---|
| Action binden | `oModel.bindContext("NS.Action(...)", oContext)` | `(...)` signalisiert OData V4 dass es eine Action ist |
| Ausführen | `.execute()` | Gibt Promise zurück |
| Refresh | `oContext.refresh()` | Aktualisiert nur diese eine Entität |
| Komplett-Refresh | `oModel.refresh()` | Aktualisiert alle Bindings |

#### Parametrisierte Action (mit Input)

```javascript
var oAction = oModel.bindContext("TaskService.assignUser(...)", oContext);
// Parameter setzen:
oAction.setParameter("UserID", "USR001");
oAction.setParameter("Comment", "Zugewiesen per UI");
// Ausführen:
oAction.execute();
```

---

### Fehlerquellen & Troubleshooting Controller Extensions

| Problem | Ursache | Lösung |
|---|---|---|
| Extension lädt nicht (404) | Dateipfad stimmt nicht mit Namespace überein | `ext.MyExt` → `webapp/ext/MyExt.controller.js` |
| `onInit` feuert nicht | `enableLazyLoading: true` | `"sap.fe": { "app": { "enableLazyLoading": false } }` |
| Button erscheint nicht | `actions`-Block fehlt in `controlConfiguration` | Actions nur in `controlConfiguration`, nicht in `extends` |
| Button klick — nichts passiert | `press`-Handler-String falsch | `.extension.<FQN>.<Methode>` — FQN muss mit `extend("..")` übereinstimmen |
| Methode nicht gefunden | Methode steht versehentlich im `override`-Block | Eigene Methoden gehören **außerhalb** von `override` |
| `overrides` statt `override` | Tippfehler — **Singular** | Immer `override` (ohne s) |
| Extension gilt für alle Targets | Unscoped Key | `...Controller#AppId::TargetId` verwenden |
| Kontext ist `undefined` | Kein Binding / Zeile nicht selektiert | `oEvent.getSource().getBindingContext()` prüfen |

---

## 10. ABAP CDS – Äquivalente Syntax

### Annotations-Positionen in CDS

```abap
-- ① View-Level Annotations (über define view):
@UI.headerInfo: { typeName: 'Product', ... }
@UI.selectionVariant: [{ qualifier: 'VAR1', ... }]
@UI.presentationVariant: [{ qualifier: 'VAR1', ... }]
@UI.selectionPresentationVariant: [{ qualifier: 'VAR1', ... }]
define view ZC_Product as select from ZI_Product { ... }

-- ② Feld-Level Annotations (im select-Block):
  @UI.selectionField: [{ position: 10 }]
  @UI.lineItem: [{ qualifier: 'TAB1', position: 10, label: 'ID' }]
  @UI.fieldGroup: [{ qualifier: 'GeneralData', position: 10 }]
  @UI.identification: [{ position: 10 }]
  @UI.dataPoint: 'StockDataPoint'          -- Verweis auf annotate-Block
key ProductID,

-- ③ Nachgelagerte annotate-Blöcke (für komplexe Strukturen):
annotate view ZC_Product with {
  @UI.dataPoint: {
    qualifier:     'StockDataPoint',
    title:         'Lagerbestand',
    targetValue:   1000,
    visualization: #PROGRESS
  }
  Stock;
}

annotate view ZC_Product with @UI.facets: [
  { $Type: 'UI.ReferenceFacet', label: 'Daten',
    targetQualifier: 'GeneralData', targetElement: #FIELDGROUP_REFERENCE }
];
```

### Vollständiges CDS-Beispiel (ZC_Product – fiorilistreport)

```abap
@AbapCatalog.sqlViewName: 'ZC_PRODUCT_V'
@AccessControl.authorizationCheck: #NOT_REQUIRED
@EndUserText.label: 'Product - Consumption View'

@UI.headerInfo: {
  typeName:       'Product',
  typeNamePlural: 'Products',
  title.value:    'ProductName',
  description.value: 'ProductID'
}
@UI.selectionPresentationVariant: [{
  qualifier: 'VAR1',
  text: 'All Products',
  selectionVariantQualifier:    'VAR1',
  presentationVariantQualifier: 'VAR1'
}]
@UI.presentationVariant: [{
  qualifier: 'VAR1',
  visualizations: [{ type: #AS_LINEITEM, qualifier: 'TAB1' }]
}]
define view ZC_Product as select from ZI_Product {
    @UI.selectionField: [{ position: 10 }]
    @UI.lineItem: [{ qualifier: 'TAB1', position: 10, label: 'Product ID' }]
  key ProductID,

    @UI.lineItem: [{ qualifier: 'TAB1', position: 20, label: 'Product Name' }]
    @UI.fieldGroup: [{ qualifier: 'GeneralData', position: 20, label: 'Product Name' }]
    ProductName,

    @UI.fieldGroup:  [{ qualifier: 'GeneralData', position: 60, label: 'Stock' }]
    @UI.dataPoint:   'StockDataPoint'
    @UI.lineItem:    [{ qualifier: 'TAB1', position: 40, type: #AS_DATAPOINT,
                        valueQualifier: 'StockDataPoint', label: 'Stock' }]
    Stock,

    @UI.dataPoint:   'RatingDataPoint'
    @UI.lineItem:    [{ qualifier: 'TAB1', position: 50, type: #AS_DATAPOINT,
                        valueQualifier: 'RatingDataPoint', label: 'Rating' }]
    Rating
}

annotate view ZC_Product with {
  @UI.dataPoint: { qualifier: 'StockDataPoint', title: 'Stock Level',
                   targetValue: 1000, visualization: #PROGRESS }
  Stock;

  @UI.dataPoint: { qualifier: 'RatingDataPoint', title: 'Rating',
                   visualization: #RATING, maximumValue: 5, minimumValue: 1 }
  Rating;
}

annotate view ZC_Product with @UI.facets: [
  { $Type: 'UI.ReferenceFacet', label: 'General Data',
    targetQualifier: 'GeneralData', targetElement: #FIELDGROUP_REFERENCE }
];
```

### RAP CDS (fiorielements – modernes Format)

```abap
@Metadata.allowExtensions: true   -- erlaubt separate MDE-Dateien
@Search.searchable: true
define root view entity ZC_PRODUCT
  provider contract transactional_query
  as projection on ZI_PRODUCT
{
  key ID,
      @Search.defaultSearchElement: true
      Name,
      Price,
      Currency,
      Stock,
      -- Value-Help über @Consumption:
      @Consumption.valueHelpDefinition: [{
        entity: { name: 'ZI_CATEGORY', element: 'Name' }
      }]
      Category,
      _Orders : redirected to composition child ZC_ORDER
}
```

---

## 11. OVP-spezifische Annotationen

Die OVP-Karten werden in `manifest.json` konfiguriert. Die Annotationen bestimmen **Inhalt und Verhalten** der Karten.

### OVP Karten-Typen und ihre Annotations

```
┌─────────────────────────────────────────────────────────────────┐
│  OVP KARTEN-TYPEN                                               │
│                                                                 │
│  ┌─────────────┐  Annotation:    UI.LineItem (kein Qualifier)   │
│  │ Table Card  │  manifest:      "sap.ovp.cards.table"          │
│  └─────────────┘                                                │
│                                                                 │
│  ┌─────────────┐  Annotation:    UI.LineItem#Priority           │
│  │ List Card   │  manifest:      "sap.ovp.cards.list"           │
│  └─────────────┘                                                │
│                                                                 │
│  ┌─────────────┐  Annotation:    UI.Chart + PresentationVariant │
│  │Analytical C.│  manifest:      "sap.ovp.cards.analytical"     │
│  └─────────────┘  optional:      UI.SelectionVariant (Vorfilter)│
│                                                                 │
│  ┌─────────────┐  Annotation:    UI.Facets Qualifier="stack"    │
│  │ Stack Card  │                 UI.Identification#stackHeader  │
│  └─────────────┘                 UI.Identification#quickView    │
└─────────────────────────────────────────────────────────────────┘
```

### manifest.json Karten-Konfiguration (fioriovp)

```json
"card00": {
  "model": "mainService",
  "template": "sap.ovp.cards.analytical",
  "settings": {
    "title"                   : "{{cardAnalyticalTitle}}",
    "entitySet"               : "Orders",
    "selectionAnnotationPath" : "com.sap.vocabularies.UI.v1.SelectionVariant#Open",
    "chartAnnotationPath"     : "com.sap.vocabularies.UI.v1.Chart#OrdersByStatus",
    "presentationAnnotationPath": "com.sap.vocabularies.UI.v1.PresentationVariant#Default"
  }
},
"card01": {
  "model": "mainService",
  "template": "sap.ovp.cards.table",
  "settings": {
    "title"      : "{{cardTableTitle}}",
    "entitySet"  : "Orders",
    "annotationPath": "com.sap.vocabularies.UI.v1.LineItem"
  }
},
"card02": {
  "model": "mainService",
  "template": "sap.ovp.cards.list",
  "settings": {
    "title"         : "{{cardListTitle}}",
    "entitySet"     : "Orders",
    "annotationPath": "com.sap.vocabularies.UI.v1.LineItem#Priority"
  }
}
```

### Stack Card (fioriovp) – Annotations-Verbindung

```
manifest.json (card03_stack):
  template: "sap.ovp.cards.stack"
  annotationPath: "UI.Facets#stack"  ← Qualifier="stack"
  identificationAnnotationPath: ["UI.Identification#stackHeader",
                                  "UI.Identification#quickView"]
       │
       ▼
annotation.xml:
  UI.Facets Qualifier="stack"
    └─ ReferenceFacet → UI.FieldGroup#OrderDetails
  UI.Identification Qualifier="stackHeader"
    └─ DataField: OrderID, CustomerName
  UI.Identification Qualifier="quickView"
    └─ DataField: OrderID, CustomerName, Status, NetAmount, ...
```

---

## 12. Annotation-Verbindungsdiagramme

### ListReport + ObjectPage (fiorilistreport)

```
manifest.json
  │
  ├─ ListReport-Route: entitySet="Products"
  │    └─ views.paths[0]: SelectionPresentationVariant#VAR1
  │         ├─ SelectionVariant  (kein Vorfilter)
  │         └─ PresentationVariant
  │              └─ UI.LineItem#TAB1
  │                   ├─ DataField: ProductID, ProductName, Currency
  │                   ├─ DataFieldForAnnotation → UI.DataPoint#RatingDataPoint
  │                   │    └─ visualization: #RATING, max=5
  │                   └─ DataFieldForAnnotation → UI.DataPoint#StockDataPoint
  │                        └─ visualization: #PROGRESS, target=1000
  │
  ├─ views.paths[2]: SelectionPresentationVariant#VAR3
  │    └─ PresentationVariant
  │         └─ UI.Chart#CHART3 (Bar)
  │              ├─ Dimension: CustomerName (Category-Rolle)
  │              └─ Measure:   NetAmount (Axis1)
  │                   └─ DataPoint → UI.DataPoint#NetAmountDP
  │
  └─ ObjectPage-Route: Products({key})
       ├─ UI.HeaderInfo: Title=ProductName, Description=ProductID
       ├─ UI.Facets
       │    └─ ReferenceFacet → UI.FieldGroup#GeneralData
       │         └─ DataField: ProductName, Category, Price, Stock, Rating
       └─ (Chart braucht Aggregation.ApplySupported in metadata.xml)
```

### OVP-App (fioriovp)

```
manifest.json  →  OVP Container
  │
  ├─ card00 (Analytical)
  │    ├─ SelectionVariant#Open  (Status NE "%")
  │    ├─ Chart#OrdersByStatus   (Donut: Status → NetAmount)
  │    └─ PresentationVariant#Default  (SortBy: NetAmount DESC)
  │
  ├─ card01 (Table)
  │    └─ LineItem  (OrderID, CustomerName, Status, NetAmount, Category)
  │
  ├─ card02 (List)
  │    └─ LineItem#Priority  (OrderID, CustomerName, NetAmount, Priority)
  │
  └─ card04 (Table – OrderItems)
       ├─ SelectionVariant#ByOrder  (OrderID-Filter)
       └─ LineItem  (ItemID, OrderID, ProductName, Qty, UnitPrice, TotalPrice)

Global Filter:
  └─ SelectionFields: Status, OrderID
       ├─ Status → Common.ValueListWithFixedValues + Common.ValueList → StatusValues
       └─ OrderID → Common.ValueList → Orders (SearchSupported=true)
```

### KPI + Chart Verknüpfung (fiorielements)

```
UI.KPI#ProductCount
  │
  ├─ DataPoint ──────► UI.DataPoint#ProductCount
  │                       Value: TotalOrders
  │                       Title: "Bestellungen gesamt"
  │
  └─ Detail.DefaultPV ─► UI.PresentationVariant#ProductCountPV
                            Visualizations ──────► UI.Chart#ProductCountChart
                                                      ChartType: Donut
                                                      Dimension: Category
                                                      Measure:   TotalOrders
```

---

## 13. Vocabularies & Namespaces – Referenz

### Benötigte Namespace-Deklarationen in annotation.xml

```xml
<edmx:Edmx Version="4.0"
  xmlns:edmx="http://docs.oasis-open.org/odata/ns/edmx">
  <edmx:Reference Uri="https://sap.github.io/odata-vocabularies/vocabularies/UI.xml">
    <edmx:Include Namespace="com.sap.vocabularies.UI.v1" Alias="UI"/>
  </edmx:Reference>
  <edmx:Reference Uri="https://sap.github.io/odata-vocabularies/vocabularies/Common.xml">
    <edmx:Include Namespace="com.sap.vocabularies.Common.v1" Alias="Common"/>
  </edmx:Reference>
  <edmx:Reference Uri="https://oasis-tcs.github.io/odata-vocabularies/vocabularies/Org.OData.Aggregation.V1.xml">
    <edmx:Include Namespace="Org.OData.Aggregation.V1" Alias="Aggregation"/>
  </edmx:Reference>
  <edmx:Reference Uri="https://oasis-tcs.github.io/odata-vocabularies/vocabularies/Org.OData.Measures.V1.xml">
    <edmx:Include Namespace="Org.OData.Measures.V1" Alias="Measures"/>
  </edmx:Reference>
  <edmx:Reference Uri="https://sap.github.io/odata-vocabularies/vocabularies/Communication.xml">
    <edmx:Include Namespace="com.sap.vocabularies.Communication.v1" Alias="Communication"/>
  </edmx:Reference>
  <edmx:DataServices>
    <Schema Namespace="MyAlias" xmlns="http://docs.oasis-open.org/odata/ns/edm">
      <!-- Annotations hier -->
    </Schema>
  </edmx:DataServices>
</edmx:Edmx>
```

### Schnell-Referenz: Annotation → Datei → Namespace

| Annotation-Präfix | Vollständiger Namespace | Datei |
|---|---|---|
| `UI.*` | `com.sap.vocabularies.UI.v1` | annotation.xml |
| `Common.*` | `com.sap.vocabularies.Common.v1` | annotation.xml |
| `Aggregation.*` | `Org.OData.Aggregation.V1` | **metadata.xml** |
| `Measures.*` | `Org.OData.Measures.V1` | annotation.xml |
| `Communication.*` | `com.sap.vocabularies.Communication.v1` | annotation.xml |

### Offizielle Dokumentation

| Vocabulary | Link |
|---|---|
| UI | https://sap.github.io/odata-vocabularies/vocabularies/UI.html |
| Common | https://sap.github.io/odata-vocabularies/vocabularies/Common.html |
| Aggregation | https://oasis-tcs.github.io/odata-vocabularies/vocabularies/Org.OData.Aggregation.html |
| Measures | https://oasis-tcs.github.io/odata-vocabularies/vocabularies/Org.OData.Measures.html |

---

---

## 14. Mockdaten – Vollreferenz

Mock-Daten ermöglichen die lokale Entwicklung ohne echtes SAP-Backend. Der `@sap-ux/ui5-middleware-fe-mockserver` simuliert OData-Anfragen auf Basis von JSON-Dateien.

---

### Dateistruktur & Format

```
webapp/
  localService/
    <ServiceName>/            ← Name muss mit urlPath in ui5-mock.yaml übereinstimmen
      metadata.xml            ← OData-Schema (EntityTypes, EntitySets, Actions)
      data/
        Products.json         ← Dateiname = EntitySet-Name (aus metadata.xml)
        Categories.json
        SalesOrders.json
        SalesOrderItems.json  ← Navigationseigenschaft: SalesOrder → Items
```

> **Namenskonvention:** Die JSON-Datei muss exakt so heißen wie der EntitySet-Name in `metadata.xml`. Groß-/Kleinschreibung beachten!

#### Grundformat einer Mock-Datendatei

```json
// Products.json
{
  "value": [
    {
      "ProductID": "P001",
      "ProductName": "Laptop Pro",
      "Category": "Electronics",
      "Price": 1299.99,
      "Currency": "EUR",
      "Stock": 45,
      "Rating": 4,
      "StockCriticality": 3,
      "ImageURL": "",
      "IsDiscountHidden": false,
      "CreatedAt": "2025-01-15T08:30:00Z"
    },
    {
      "ProductID": "P002",
      "ProductName": "Bluetooth Headset",
      "Category": "Accessories",
      "Price": 89.99,
      "Currency": "EUR",
      "Stock": 8,
      "Rating": 3,
      "StockCriticality": 2,
      "ImageURL": "",
      "IsDiscountHidden": false,
      "CreatedAt": "2025-01-20T10:00:00Z"
    }
  ]
}
```

> **`"value"`-Wrapper:** Das OData V4 JSON-Format erwartet das Array unter dem Schlüssel `"value"`. Ohne diesen Wrapper liefert der Mock-Server einen Fehler oder eine leere Antwort.

---

### OData V4 Datentypen in JSON

| OData-Typ in metadata.xml | JSON-Wert-Format | Beispiel |
|---|---|---|
| `Edm.String` | String | `"Electronics"` |
| `Edm.Int16`, `Edm.Int32`, `Edm.Int64` | Integer | `42` |
| `Edm.Byte` | Integer 0–255 | `3` |
| `Edm.Decimal` | Dezimalzahl | `1299.99` |
| `Edm.Boolean` | true/false | `true` |
| `Edm.DateTime` | ISO 8601 String | `"2025-01-15T08:30:00Z"` |
| `Edm.DateTimeOffset` | ISO 8601 mit Offset | `"2025-01-15T08:30:00+01:00"` |
| `Edm.Date` | Datum-String | `"2025-01-15"` |
| `Edm.Guid` | GUID-String | `"6F9619FF-8B86-D011-B42D-00C04FC964FF"` |
| `Edm.Binary` | Base64-String | `"AQID"` |

#### Kritikalitäts-Werte (Integer) in Mock-Daten

```json
{ "StockCriticality": 0 }   // Neutral  → Grau
{ "StockCriticality": 1 }   // Negative → Rot
{ "StockCriticality": 2 }   // Critical → Gelb/Orange
{ "StockCriticality": 3 }   // Positive → Grün
{ "StockCriticality": 5 }   // Information → Blau
```

---

### Navigationseigenschaften mocken

Um Navigation-Properties zu simulieren (z.B. `SalesOrder` → `SalesOrderItems`), müssen die verknüpften Datensätze **Schlüssel-Felder** verwenden die zur Navigation passen.

#### Beispiel: SalesOrder mit Items (1:n)

```json
// SalesOrders.json
{
  "value": [
    { "OrderID": "SO001", "CustomerName": "Müller GmbH", "Status": "OPEN",   "NetAmount": 2500.00, "Currency": "EUR" },
    { "OrderID": "SO002", "CustomerName": "Schmidt AG",  "Status": "CLOSED", "NetAmount": 1800.00, "Currency": "EUR" }
  ]
}

// SalesOrderItems.json — Verknüpfung über OrderID
{
  "value": [
    { "ItemID": "I001", "OrderID": "SO001", "ProductName": "Laptop",    "Qty": 2, "UnitPrice": 1250.00 },
    { "ItemID": "I002", "OrderID": "SO001", "ProductName": "Maus",      "Qty": 1, "UnitPrice":   25.00 },
    { "ItemID": "I003", "OrderID": "SO002", "ProductName": "Tastatur",  "Qty": 3, "UnitPrice":   45.00 }
  ]
}
```

> Der Mock-Server löst `$expand=SalesOrderItems` auf und filtert die Items automatisch nach dem passenden `OrderID`-Wert — sofern in `metadata.xml` eine `NavigationProperty` mit dem gleichen Namen deklariert ist.

#### NavigationProperty in metadata.xml (V4)

```xml
<!-- Im EntityType SalesOrder: -->
<NavigationProperty Name="SalesOrderItems"
  Type="Collection(MyService.SalesOrderItem)"
  Partner="SalesOrder" />

<!-- Im EntityContainer: NavigationPropertyBinding damit $expand funktioniert -->
<EntitySet Name="SalesOrders" EntityType="MyService.SalesOrder">
  <NavigationPropertyBinding Path="SalesOrderItems" Target="SalesOrderItems" />
</EntitySet>
```

---

### ui5-mock.yaml Konfiguration

```yaml
# ui5-mock.yaml — vollständige Konfiguration
specVersion: "3.0"
metadata:
  name: com.example.myapp

server:
  customMiddleware:
    - name: sap-fe-mockserver
      afterMiddleware: compression
      configuration:
        mountPath: /
        services:
          - urlPath: /sap/opu/odata4/sap/my_service/srvd/sap/my_service/0001/
            metadataXmlPath: ./webapp/localService/mainService/metadata.xml
            mockdataRootPath: ./webapp/localService/mainService/data
            generateMockData: false           # true = automatisch generieren
            odataVersion: 4                   # 2 oder 4
            debug: true                       # Mehr Logging im Terminal
```

#### Alle Konfigurationsoptionen

| Option | Typ | Bedeutung |
|---|---|---|
| `urlPath` | String | Muss mit `sap.app.dataSources.mainService.uri` in manifest.json übereinstimmen |
| `metadataXmlPath` | Pfad | Pfad zur metadata.xml (relativ vom Projekt-Root) |
| `mockdataRootPath` | Pfad | Ordner mit den JSON-Datendateien |
| `generateMockData` | Boolean | `true` = Mock-Server generiert Zufallsdaten wenn JSON fehlt |
| `odataVersion` | `2` oder `4` | OData-Version des Services |
| `debug` | Boolean | Gibt jede Mock-Anfrage im Terminal aus |
| `mockRequests` | Array | Eigene HTTP-Handler überschreiben Standard-Verhalten |

---

### Aggregation & Charts mocken

Charts benötigen OData `$apply`-Anfragen. Der Mock-Server der `@sap-ux/ui5-middleware-fe-mockserver`-Bibliothek **unterstützt `$apply` nativ** — die Aggregation wird clientseitig berechnet.

**Voraussetzung in metadata.xml:**
```xml
<Annotations Target="MainService.MainContainer/SalesOrders">
  <Annotation Term="Aggregation.ApplySupported">
    <Record Type="Aggregation.ApplySupportedType">
      <PropertyValue Property="AggregatableProperties">
        <Collection>
          <Record Type="Aggregation.AggregatablePropertyType">
            <PropertyValue Property="Property" PropertyPath="NetAmount" />
          </Record>
        </Collection>
      </PropertyValue>
    </Record>
  </Annotation>
  <!-- CustomAggregate für SUM -->
  <Annotation Term="Aggregation.CustomAggregate" Qualifier="NetAmount" String="Edm.Decimal" />
</Annotations>
```

**Mock-Datensätze für Charts (genug Datenpunkte für aussagekräftige Charts):**
```json
// SalesOrders.json — min. 3-5 verschiedene Kategorien für sinnvollen Donut-Chart
{
  "value": [
    { "OrderID": "001", "Status": "OPEN",      "CustomerName": "A GmbH", "NetAmount": 2500.00, "Currency": "EUR" },
    { "OrderID": "002", "Status": "OPEN",      "CustomerName": "B AG",   "NetAmount": 1800.00, "Currency": "EUR" },
    { "OrderID": "003", "Status": "CLOSED",    "CustomerName": "C KG",   "NetAmount": 3200.00, "Currency": "EUR" },
    { "OrderID": "004", "Status": "PENDING",   "CustomerName": "A GmbH", "NetAmount":  900.00, "Currency": "EUR" },
    { "OrderID": "005", "Status": "CANCELLED", "CustomerName": "D GmbH", "NetAmount":  450.00, "Currency": "EUR" }
  ]
}
```

> **Tipp:** Für einen Donut-Chart nach `Status` brauchen die Daten mindestens 2–3 verschiedene Statuswerte. Sonst zeigt der Chart nur ein einzelnes Segment.

---

### OData Actions mocken

Backend-OData-Actions werden per `POST`-Request ausgeführt. Der Mock-Server kennt keine deklarierten Actions — sie müssen als `mockRequest`-Handler registriert werden.

#### Variante 1: Statische Antwort (ui5-mock.yaml)

```yaml
services:
  - urlPath: /sap/opu/odata4/...
    metadataXmlPath: ...
    mockdataRootPath: ...
    mockRequests:
      # Bound Action: Pattern matcht die Entity-URL + Action-Name
      - method: POST
        path: /Tasks\(.*\)/TaskService\.completeTask
        response:
          statusCode: 200
          body: '{"value": true}'
          headers:
            Content-Type: application/json
      # Unbound Action (kein Entity-Schlüssel in der URL):
      - method: POST
        path: /TaskService\.sendNotification
        response:
          statusCode: 200
          body: '{"value": "Notification sent"}'
```

#### Variante 2: Dynamischer Handler (mockserver.js)

```javascript
// webapp/localService/mockserver.js
sap.ui.define(["sap/ui/core/util/MockServer"], function (MockServer) {
    "use strict";
    return {
        init: function () {
            var oMockServer = new MockServer({
                rootUri: "/sap/opu/odata4/..."
            });

            // Eigenen Request-Handler registrieren:
            oMockServer.attachBefore(MockServer.HTTPMETHOD.POST, function (oEvent) {
                var sUrl = oEvent.getParameter("oXhr").url;
                if (sUrl.includes("completeTask")) {
                    oEvent.getParameter("oXhr").respondJSON(200, {}, { value: true });
                }
            });

            oMockServer.start();
        }
    };
});
```

---

### Häufige Mock-Fehler

| Problem | Ursache | Lösung |
|---|---|---|
| `404 Not Found` beim Laden der Daten | JSON-Dateiname stimmt nicht mit EntitySet-Name überein | Dateiname muss exakt wie `EntitySet Name="..."` in metadata.xml sein |
| Tabelle zeigt "No Data" obwohl JSON vorhanden | JSON hat kein `"value"`-Array | `{ "value": [...] }` — Wrapper nicht vergessen |
| Chart zeigt keine Daten | `Aggregation.ApplySupported` fehlt in metadata.xml | In metadata.xml am EntitySet annotieren |
| Action-Button gibt 404 | Mock-Request-Handler fehlt | `mockRequests` in ui5-mock.yaml hinzufügen |
| `$expand` gibt leere Navigation | `NavigationPropertyBinding` fehlt in metadata.xml | Im EntitySet-Container `NavigationPropertyBinding` definieren |
| `generateMockData: true` generiert unsinnige Werte | Zufalls-Generator kennt keine Domäne | `generateMockData: false` + manuelle JSON-Dateien verwenden |
| Dezimalzahl wird als String gespeichert | JSON enthält `"1299.99"` (String) statt `1299.99` (Number) | Anführungszeichen bei Zahlen entfernen |
| Datum wird nicht korrekt angezeigt | Falsches Datumsformat | ISO 8601: `"2025-01-15T08:30:00Z"` verwenden |
| `debug: true` zeigt keine Logs | `debug`-Flag liegt an falscher Stelle in yaml | Muss unter `configuration:` der Middleware-Konfiguration stehen |

---

### Umstieg Mock → echter OData-Service

Nach der lokalen Entwicklung mit Mock-Daten muss die App auf den echten SAP-Backend-Service umgestellt werden. Die folgende Checkliste beschreibt alle Schritte.

#### Übersicht: Was ändert sich?

```
  MOCK-BETRIEB                          PRODUKTIV-BETRIEB
  ─────────────                         ─────────────────
  ui5-mock.yaml    → simuliert Backend  ui5.yaml / ui5-local.yaml → echter Service
  metadata.xml     → manuell gepflegt   metadata.xml → vom Backend geliefert ($metadata)
  data/*.json      → Testdaten          SAP-Datenbank → echte Daten
  urlPath (lokal)  → relativer Pfad     uri → absoluter SAP-Service-Pfad
  npm run start-mock                    npm run start (ui5-local.yaml)
```

---

#### Schritt 1 – Service-URL ermitteln und eintragen

Die wichtigste Änderung ist die **URI des OData-Services** in `manifest.json`. Im Mock-Betrieb kann eine beliebige URL verwendet werden, da der Mock-Server alle Anfragen abfängt. Im Produktivbetrieb muss die URL exakt mit dem SAP-Backend-Endpunkt übereinstimmen.

**Typische SAP OData V4 Service-URLs:**
```
/sap/opu/odata4/<Namespace>/<ServiceGroup>/<ServiceName>/<ServiceVersion>/
  z.B.:
  /sap/opu/odata4/sap/zc_products_cds/0001/
  /sap/opu/odata4/sap/my_service/srvd/sap/my_service/0001/
```

**In manifest.json anpassen:**
```json
"sap.app": {
  "dataSources": {
    "mainService": {
      "uri": "/sap/opu/odata4/sap/zc_products_cds/0001/",  // ← echte URL eintragen
      "type": "OData",
      "settings": {
        "odataVersion": "4.0",
        "annotations": ["annotation"]
      }
    },
    "annotation": {
      "uri": "annotations/annotation.xml",  // bleibt gleich
      "type": "ODataAnnotation"
    }
  }
}
```

---

#### Schritt 2 – metadata.xml gegen echte Metadaten ersetzen

Die lokale `metadata.xml` war nur ein Mock-Platzhalter. Im Produktivbetrieb liefert der Backend-Service die Metadaten automatisch über den `$metadata`-Endpunkt. Die lokale Datei wird nicht mehr benötigt — **aber** sie bleibt im `localService`-Ordner für den Mock-Betrieb erhalten.

**Echte metadata.xml herunterladen:**
```powershell
# URL im Browser aufrufen und als Datei speichern:
# https://<SAP-Host>/sap/opu/odata4/sap/zc_products_cds/0001/$metadata

# Oder per curl:
curl -u <user>:<pass> "https://<host>/sap/opu/odata4/.../0001/$metadata" -o metadata_real.xml
```

**Abweichungen prüfen:**
Die reale metadata.xml kann sich von der Mock-Version unterscheiden:

| Mögliche Abweichung | Konsequenz | Lösung |
|---|---|---|
| Property-Namen anders (z.B. `ProductID` → `Product`) | Alle `Path="ProductID"`-Annotationen brechen | Annotation-Paths anpassen |
| Andere Namespace/Service-Name | `Target="MainService.Product"` zeigt ins Leere | Namespace in allen `Target`-Attributen anpassen |
| EntitySet heißt anders | Tabelle lädt nicht | `contextPath` in manifest.json anpassen |
| Datentyp unterschiedlich (z.B. `Edm.String` statt `Edm.Int32`) | Sortierung, Filteroperatoren falsch | Annotation-Typen prüfen (z.B. `Low Int="10"` → `Low String="10"`) |
| Navigation-Property fehlt | Common.Text / $expand schlägt fehl | Navigation-Annotation entfernen oder anpassen |
| Aggregation.ApplySupported fehlt | Charts leer | Backend-Team bitten die Annotation in der CDS-View zu setzen |

---

#### Schritt 3 – ui5-local.yaml für echten Service konfigurieren

Für den Betrieb gegen das echte Backend wird `ui5-local.yaml` (oder `ui5.yaml`) verwendet. Diese Datei enthält **keinen Mock-Server**, sondern einen Proxy der Anfragen an den SAP-Host weiterleitet.

```yaml
# ui5-local.yaml — Konfiguration für echten Backend-Service
specVersion: "3.0"
metadata:
  name: com.example.myapp

server:
  customMiddleware:
    - name: fiori-tools-proxy
      afterMiddleware: compression
      configuration:
        backend:
          - path: /sap             # Alle /sap/-Anfragen werden weitergeleitet
            url: https://<SAP-Host>:<Port>  # z.B. https://my-sap-system.example.com:44300
            # Für SAP BTP / Cloud:
            # destination: MY_DESTINATION_NAME
```

**Für SAP BTP (Cloud Foundry) mit Destination:**
```yaml
    - name: fiori-tools-proxy
      configuration:
        backend:
          - path: /sap
            destination: MY_BTP_DESTINATION
            # scp: true  ← nur für ältere CF-Umgebungen
```

**App mit echtem Backend starten:**
```powershell
npm run start         # nutzt ui5-local.yaml
# oder explizit:
ui5 serve --config ui5-local.yaml
```

---

#### Schritt 4 – CORS und Authentifizierung

Beim Zugriff auf ein echtes SAP-Backend aus dem lokalen Browser entstehen **CORS-Fehler** (Cross-Origin Resource Sharing), da Browser Anfragen von `localhost` an fremde Hosts blockieren.

**Lösung: fiori-tools-proxy als Relay**
Der Proxy in `ui5-local.yaml` (s. Schritt 3) leitet Anfragen serverseitig weiter — kein CORS-Problem.

**Authentifizierung konfigurieren:**
```yaml
# ui5-local.yaml: Basic Auth für lokale Entwicklung
    - name: fiori-tools-proxy
      configuration:
        backend:
          - path: /sap
            url: https://<SAP-Host>
            # Option A: Credentials in Umgebungsvariablen (sicher)
            # HTTP_PROXY_USER und HTTP_PROXY_PASSWORD in .env setzen

            # Option B: Direkt (NUR lokal, NIE committen!)
            # client: "100"
```

```powershell
# .env Datei im Projekt-Root (in .gitignore aufnehmen!):
# HTTP_PROXY_USER=meinuser
# HTTP_PROXY_PASSWORD=meinpasswort
```

> ⚠️ **Niemals Passwörter in `ui5-local.yaml` oder `manifest.json` committen!** `.env` zur `.gitignore` hinzufügen.

---

#### Schritt 5 – Annotation-Namespace anpassen

Wenn sich der Service-Namespace zwischen Mock und echtem Backend unterscheidet, müssen alle `Target`-Attribute in `annotation.xml` aktualisiert werden.

```xml
<!-- Mock: selbst gewählter Namespace -->
<Annotations Target="MainService.Product">          <!-- "MainService" ist Mock-Namespace -->

<!-- Produktiv: Namespace aus realer metadata.xml -->
<Annotations Target="ZC_PRODUCTS_CDS.ZC_ProductType"> <!-- realer CDS-Namespace -->
```

**Namespace in metadata.xml finden:**
```xml
<!-- In der realen metadata.xml: -->
<Schema Namespace="ZC_PRODUCTS_CDS"      <!-- ← dieser Namespace gilt für Target= -->
        Alias="SAP__self" ...>
  <EntityType Name="ZC_ProductType">     <!-- ← Target = "ZC_PRODUCTS_CDS.ZC_ProductType" -->
```

**Schnelles Ersetzen per PowerShell (alle Targets auf einmal):**
```powershell
# Alle Vorkommen von "MainService." durch echten Namespace ersetzen:
(Get-Content .\webapp\annotations\annotation.xml) `
  -replace 'MainService\.', 'ZC_PRODUCTS_CDS.' `
  | Set-Content .\webapp\annotations\annotation.xml
```

---

#### Schritt 6 – ValueList-CollectionPaths prüfen

Im Mock-Betrieb wurden ValueList-Collections als einfache JSON-Dateien angelegt. Im echten Backend müssen diese EntitySets im OData-Service tatsächlich existieren.

```xml
<!-- annotation.xml: CollectionPath muss ein echtes EntitySet im Service sein -->
<PropertyValue Property="CollectionPath" String="I_BusinessPartner" />
<!--                                              ↑ muss in metadata.xml als EntitySet existieren -->
```

**Checkliste ValueList für Produktivbetrieb:**
```
✅ CollectionPath = EntitySet-Name aus realer metadata.xml
✅ ValueListProperty-Strings = echte Property-Namen der ValueList-Entity
✅ LocalDataProperty = Property-Name in der Haupt-Entity (stimmt mit echtem Schema überein)
✅ SearchSupported = Backend unterstützt $search auf diesem EntitySet
```

---

#### Schritt 7 – Aggregation.ApplySupported im echten Backend

Im Mock-Betrieb wurde `Aggregation.ApplySupported` lokal in `metadata.xml` eingetragen. Im echten Backend muss diese Annotation **vom Backend-Service selbst geliefert werden** — sie kann nicht aus `annotation.xml` kommen.

```
  MOCK: Aggregation.ApplySupported in lokaler metadata.xml → funktioniert
  PRODUKTIV: Aggregation.ApplySupported muss in CDS-View annotiert sein
             oder vom Gateway/CAP-Service geliefert werden
```

**In ABAP CDS (RAP):**
```abap
@Aggregation.default: #SUM
NetAmount,

-- oder auf der gesamten View:
@Analytics.query: true
define view entity ZC_SALESORDER_AGGR as ...
```

**In CAP (Node.js/Java):**
```cds
-- In schema.cds:
service MyService {
  @Aggregation.ApplySupported
  entity SalesOrders as projection on db.SalesOrders;
}
```

**Charts testen nach Umstieg:**
```
1. Browser DevTools → Network Tab öffnen
2. Seite mit Chart neu laden
3. Nach einer Anfrage mit "$apply" suchen:
   GET /SalesOrders?$apply=groupby((Status),aggregate(NetAmount with sum as NetAmount))
4. Wenn diese Anfrage 200 zurückgibt: ✅ Aggregation funktioniert
   Wenn 400/501: ❌ Backend unterstützt $apply nicht → CDS-Annotation fehlt
```

---

#### Vollständige Checkliste: Mock → Produktiv

```
  DATEI-ÄNDERUNGEN:
  ☐ manifest.json: dataSources.mainService.uri → echte Backend-URL
  ☐ ui5-local.yaml: fiori-tools-proxy mit SAP-Host konfigurieren
  ☐ annotation.xml: Target-Namespace an reale metadata.xml anpassen
  ☐ annotation.xml: ValueList CollectionPaths prüfen
  ☐ .gitignore: .env mit Credentials aufnehmen

  BACKEND-VORAUSSETZUNGEN:
  ☐ OData-Service aktiv und erreichbar ($metadata-Endpunkt antwortet)
  ☐ Alle genutzten EntitySets im Service vorhanden
  ☐ Navigation-Properties (für $expand / Common.Text) deklariert
  ☐ Aggregation.ApplySupported im Backend annotiert (für Charts)
  ☐ Alle OData-Actions in metadata.xml deklariert (für DataFieldForAction)
  ☐ F4-ValueList-EntitySets existieren im Service

  TESTS NACH DEM UMSTIEG:
  ☐ Tabelle lädt Daten (kein "No Data" / kein 404)
  ☐ Filter-Bar funktioniert (Felder werden angezeigt)
  ☐ F4-Hilfe öffnet sich und liefert Werte
  ☐ Navigation ListReport → ObjectPage funktioniert
  ☐ Charts zeigen aggregierte Daten
  ☐ OData-Actions lösen korrekte Backend-Logik aus
  ☐ Common.Text-Felder zeigen Text statt Code
  ☐ Browser-Konsole: keine 401/403/404/500-Fehler
```

---

## 15. Häufige Fehler & Troubleshooting

### Chart bleibt leer / zeigt keine Daten

```
  Problem:  Chart wird gerendert, aber keine Balken/Segmente
  Ursache:  Aggregation.ApplySupported fehlt am EntitySet
  Lösung:   In metadata.xml hinzufügen:

  <Annotations Target="MyService.MainContainer/SalesOrders">
    <Annotation Term="Aggregation.ApplySupported">
      <Record Type="Aggregation.ApplySupportedType">
        <PropertyValue Property="AggregatableProperties">
          <Collection>
            <Record Type="Aggregation.AggregatablePropertyType">
              <PropertyValue Property="Property" PropertyPath="NetAmount" />
            </Record>
          </Collection>
        </PropertyValue>
      </Record>
    </Annotation>
  </Annotations>
```

### DataPoint (Progress / Rating) wird nicht als Micro-Chart angezeigt

```
  Problem:  Spalte zeigt nur den Rohwert (Zahl), nicht den Balken/Sterne
  Ursache 1: UI.DataFieldForAnnotation fehlt im UI.LineItem — statt dessen
             wurde UI.DataField mit Path auf das Feld verwendet
  Ursache 2: Target-Qualifier stimmt nicht überein

  Falsch:
  <Record Type="UI.DataField">
    <PropertyValue Property="Value" Path="Stock" />    ← zeigt nur Zahl
  </Record>

  Richtig:
  <Record Type="UI.DataFieldForAnnotation">
    <PropertyValue Property="Target" AnnotationPath="@UI.DataPoint#StockDataPoint" />
  </Record>
```

### F4-Hilfe (Lupe) erscheint nicht im Filterfeld

```
  Problem:  Filterfeld hat kein F4-Icon
  Ursache:  Common.ValueList ist nicht am Property-Target annotiert,
            sondern fälschlicherweise am EntityType-Target

  Falsch:
  <Annotations Target="MyService.Product">        ← EntityType
    <Annotation Term="Common.ValueList"> ... </Annotation>
  </Annotations>

  Richtig:
  <Annotations Target="MyService.Product/Category">  ← Property
    <Annotation Term="Common.ValueList"> ... </Annotation>
  </Annotations>
```

### ObjectPage zeigt keine Sections / ist leer

```
  Problem:  ObjectPage lädt, aber keine Abschnitte sichtbar
  Ursache:  UI.Facets fehlt oder zeigt auf falsche AnnotationPath

  Checkliste:
  ✅ UI.Facets ist an MyService.Product annotiert (EntityType)
  ✅ ReferenceFacet.Target zeigt auf @UI.FieldGroup#GeneralData
  ✅ UI.FieldGroup Qualifier="GeneralData" existiert am gleichen EntityType
  ✅ manifest.json hat eine ObjectPage-Route mit dem richtigen EntitySet
```

### Tab erscheint nicht im ListReport

```
  Problem:  Nur ein Tab sichtbar (oder keine Tabs)
  Ursache:  manifest.json views.paths nicht konfiguriert

  Checkliste:
  ✅ manifest.json: "views" mit "paths"-Array vorhanden
  ✅ annotationPath zeigt auf den korrekten Qualifier:
     "com.sap.vocabularies.UI.v1.SelectionPresentationVariant#VAR1"
  ✅ UI.SelectionPresentationVariant#VAR1 in annotation.xml vorhanden
  ✅ Die referenzierte @UI.LineItem#TAB1 Annotation existiert
```

### i18n-Binding funktioniert nicht (OVP)

```
  Problem:  String "{@i18n>myKey}" wird literal angezeigt statt übersetzt
  Ursache:  i18n-Modell nicht registriert in manifest.json

  Lösung: In manifest.json unter sap.app/i18n oder sap.ui5/models:
  "i18n": {
    "type": "sap.ui.model.resource.ResourceModel",
    "settings": { "bundleName": "com.example.fioriovp.i18n.i18n" }
  }

  Hinweis: Das @-Zeichen vor i18n ("{@i18n>key}") ist OVP-spezifisch.
  In Standard Fiori Elements annotation.xml ist i18n-Binding NICHT möglich.
  Dort: Common.Label String="Fester Text" oder Übersetzungskeys in der
  i18n.properties der App.
```

### Common.Text zeigt nichts / Navigation-Pfad schlägt fehl

```
  Problem:  Feld zeigt weiterhin den Code statt des Textes
  Ursache:  NavigationProperty "to_Status" fehlt in metadata.xml

  Checkliste:
  ✅ <NavigationProperty Name="to_Status" Type="MyService.StatusText" />
     muss im EntityType definiert sein
  ✅ <Association .../> und <AssociationSet .../> vorhanden (V2)
     ODER <NavigationPropertyBinding .../> im EntitySet vorhanden (V4)
  ✅ Im Mock-Betrieb: $expand wird vom Mockserver unterstützt
```

### Annotation wird ignoriert / hat keine Wirkung

```
  Häufige Ursachen:

  ① Falscher Namespace-Alias:
     Falsch:  Term="SAP.UI.LineItem"  (großgeschrieben)
     Richtig: Term="UI.LineItem"

  ② Qualifier-Tippfehler:
     Annotation: Qualifier="GeneralData"
     Referenz:   AnnotationPath="@UI.FieldGroup#GeneralDate"  ← Tippfehler!

  ③ Aggregation-Annotation in annotation.xml statt metadata.xml:
     Aggregation.ApplySupported MUSS in metadata.xml stehen

  ④ EntityType-Namespace fehlt:
     Falsch:  Target="Product"           ← kein Namespace
     Richtig: Target="MainService.Product"
```

### manifest.json – Häufige Konfigurationsfehler

```
  Problem:  App startet nicht / zeigt "Component could not be loaded"
  Ursache:  sap.app/id stimmt nicht mit Dateipfad überein
  Lösung:   sap.app/id muss dem Namespace-Pfad entsprechen:
            id: "com.example.myapp.myapp" → Dateien unter webapp/

  Problem:  OData-Service wird nicht geladen (503 Service Unavailable)
  Ursache:  dataSource.uri zeigt auf falsche URL
  Lösung:   Bei Mock-Betrieb: uri muss mit urlPath in ui5-mock.yaml übereinstimmen

  Problem:  Annotationen werden nicht angewendet
  Ursache:  ODataAnnotation-DataSource nicht in der Service-dataSource referenziert
  Lösung:   In mainService.settings.annotations: ["annotation"] eintragen

  Problem:  i18n-Texte werden nicht aufgelöst
  Ursache:  bundleName falsch oder Datei nicht vorhanden
  Lösung:   "bundleName": "<appId>.i18n.i18n" → webapp/i18n/i18n.properties
```

### Fiori Elements Page Map / Joule – Annotation-Konflikte

```
  Problem:  Page Map erstellt Annotationen, die mit bestehenden kollidieren
  Ursache:  Page Map schreibt in annotation.xml — doppelte Terme ohne Qualifier
  Lösung:   Page Map nur für neue Annotationen nutzen; bestehende manuell pflegen

  Problem:  OVP-App zeigt Karte nicht obwohl manifest.json korrekt
  Ursache:  OVP-App benötigt sap.ovp Dependency
  Lösung:   In sap.ui5.dependencies.libs:
            "sap.ovp": {}  hinzufügen
```

### SelectionVariant / Filter-Chips erscheinen nicht

```
  Problem:  Filter-Chips werden nicht über der Tabelle angezeigt
  Ursache:  UI.SelectionVariant vorhanden, aber kein "All"-Chip (Standard-Chip fehlt)
  Lösung:   Mindestens einen UI.SelectionVariant ohne SelectOptions als "All"-Chip definieren

  Problem:  Chip-Klick hat keine Wirkung (Daten bleiben gleich)
  Ursache:  FilterRestrictions verhindern den Filter auf diesem Feld
  Lösung:   In metadata.xml FilterRestrictions prüfen oder entfernen
```

### Fehler nach Umstieg von Mock auf echten Service

```
  Problem:  Tabelle zeigt "No Data" obwohl im Backend Daten vorhanden
  Ursache:  manifest.json uri zeigt noch auf Mock-URL (z.B. relativer Pfad)
  Lösung:   dataSources.mainService.uri auf echte Backend-URL setzen
            ui5-local.yaml mit fiori-tools-proxy konfigurieren

  Problem:  CORS-Fehler in der Browser-Konsole (blocked by CORS policy)
  Ursache:  Browser blockiert direkte Anfragen von localhost an SAP-Host
  Lösung:   fiori-tools-proxy in ui5-local.yaml einrichten — Anfragen laufen
            dann über den lokalen Node.js-Server, kein direkter Browser-Zugriff

  Problem:  401 Unauthorized
  Ursache:  Authentifizierung nicht konfiguriert
  Lösung:   Credentials in .env setzen (HTTP_PROXY_USER / HTTP_PROXY_PASSWORD)
            oder SAP-SSO / SAML korrekt konfigurieren

  Problem:  Alle Annotationen werden ignoriert (App zeigt nur rohe Felder)
  Ursache:  Target-Namespace in annotation.xml stimmt nicht mit realem Namespace
            aus metadata.xml überein
  Lösung:   Schema Namespace="..." aus realer metadata.xml ablesen und alle
            Target="<Namespace>.<EntityType>" in annotation.xml anpassen

  Problem:  Chart bleibt leer obwohl im Mock funktioniert
  Ursache:  Aggregation.ApplySupported fehlt im echten Backend-Service
  Lösung:   In CDS-View @Aggregation.default oder @Analytics.query: true setzen
            Prüfen: GET /EntitySet?$apply=... → muss 200 zurückgeben

  Problem:  Common.Text zeigt Code statt Text
  Ursache:  NavigationProperty existiert nicht im echten Backend-Schema
  Lösung:   In realer metadata.xml prüfen ob NavigationProperty vorhanden ist;
            ggf. Backend-Team bitten Association zu deklarieren

  Problem:  F4-ValueList öffnet sich, aber ist leer
  Ursache:  CollectionPath-EntitySet existiert nicht im echten Service
  Lösung:   Backend-Team bitten ValueList-Entity im Service zu exponieren
            (z.B. als @ObjectModel.representativeKey annotiertes CDS-View)

  Problem:  OData-Action gibt 404 zurück
  Ursache:  Action ist nicht im echten Service implementiert
  Lösung:   In realer metadata.xml prüfen ob <Action Name="..."> vorhanden;
            im Mock-Betrieb hat mockRequests-Handler den Fehler verdeckt
```

### OVP Global Filter funktioniert nicht

```
  Problem:  Änderung im Global Filter hat keinen Effekt auf OVP-Karten
  Ursache:  Karte verwendet eigenes Model (nicht "mainService")
  Lösung:   In manifest.json: "model": "mainService" in der Karten-Konfiguration setzen

  Problem:  SelectionFields für Global Filter werden nicht angezeigt
  Ursache:  UI.SelectionFields ist am falschen EntityType (nicht dem Global Filter Entity)
  Lösung:   globalFilterEntityType in manifest.json prüfen und SelectionFields
            am gleichen EntityType annotieren
```

---

## 16. Schnell-Rezepte (Cookbook)

### Rezept 1: Neue Tabellenspalte hinzufügen

```xml
<!-- In annotation.xml: UI.LineItem des EntityType erweitern -->
<Annotation Term="UI.LineItem" Qualifier="TAB1">
  <Collection>
    <!-- ... bestehende Einträge ... -->

    <!-- NEU: einfache Text-Spalte -->
    <Record Type="UI.DataField">
      <PropertyValue Property="Label" String="Priorität" />
      <PropertyValue Property="Value" Path="Priority" />
    </Record>
  </Collection>
</Annotation>

<!-- Sicherstellen: Property "Priority" ist in metadata.xml vorhanden! -->
```

### Rezept 2: Neues Filterfeld mit F4-Hilfe

```xml
<!-- Schritt 1: Feld zu SelectionFields hinzufügen -->
<Annotation Term="UI.SelectionFields">
  <Collection>
    <PropertyPath>Category</PropertyPath>   <!-- NEU -->
    <PropertyPath>ProductName</PropertyPath>
  </Collection>
</Annotation>

<!-- Schritt 2: ValueList an der Property annotieren -->
<Annotations Target="MainService.Product/Category">
  <Annotation Term="Common.ValueList">
    <Record Type="Common.ValueListType">
      <PropertyValue Property="CollectionPath" String="Categories" />
      <PropertyValue Property="Parameters">
        <Collection>
          <Record Type="Common.ValueListParameterInOut">
            <PropertyValue Property="LocalDataProperty" PropertyPath="Category" />
            <PropertyValue Property="ValueListProperty" String="Name" />
          </Record>
        </Collection>
      </PropertyValue>
    </Record>
  </Annotation>
</Annotations>
```

### Rezept 3: Neuen ObjectPage-Abschnitt hinzufügen

```xml
<!-- Schritt 1: FieldGroup definieren -->
<Annotation Term="UI.FieldGroup" Qualifier="AddressData">
  <Record Type="UI.FieldGroupType">
    <PropertyValue Property="Label" String="Adresse" />
    <PropertyValue Property="Data">
      <Collection>
        <Record Type="UI.DataField">
          <PropertyValue Property="Label" String="Straße" />
          <PropertyValue Property="Value" Path="Street" />
        </Record>
        <Record Type="UI.DataField">
          <PropertyValue Property="Label" String="Stadt" />
          <PropertyValue Property="Value" Path="City" />
        </Record>
      </Collection>
    </PropertyValue>
  </Record>
</Annotation>

<!-- Schritt 2: ReferenceFacet in UI.Facets hinzufügen -->
<Annotation Term="UI.Facets">
  <Collection>
    <!-- ... bestehende Abschnitte ... -->
    <Record Type="UI.ReferenceFacet">
      <PropertyValue Property="ID"     String="AddressSection" />
      <PropertyValue Property="Label"  String="Adresse" />
      <PropertyValue Property="Target" AnnotationPath="@UI.FieldGroup#AddressData" />
    </Record>
  </Collection>
</Annotation>
```

### Rezept 4: Status-Ampel in Tabellenspalte

```xml
<!-- Schritt 1: DataPoint mit Criticality definieren -->
<Annotation Term="UI.DataPoint" Qualifier="StatusDP">
  <Record Type="UI.DataPointType">
    <PropertyValue Property="Value"      Path="Status" />
    <PropertyValue Property="Title"      String="Status" />
    <!-- CriticalityPath zeigt auf ein Int-Feld (1=Rot, 2=Gelb, 3=Grün) -->
    <PropertyValue Property="Criticality" Path="StatusCriticality" />
  </Record>
</Annotation>

<!-- Schritt 2: Als DataFieldForAnnotation in LineItem einbinden -->
<Record Type="UI.DataFieldForAnnotation">
  <PropertyValue Property="Label"  String="Status" />
  <PropertyValue Property="Target" AnnotationPath="@UI.DataPoint#StatusDP" />
</Record>

<!-- Schritt 3: Mock-Daten: StatusCriticality-Feld befüllen -->
<!-- 1 = Negative (Rot), 2 = Critical (Gelb), 3 = Positive (Grün) -->
```

### Rezept 5: Vorfilter-Tab mit SelectionPresentationVariant

```xml
<!-- Nur "offene" Bestellungen im Tab anzeigen -->
<Annotation Term="UI.SelectionPresentationVariant" Qualifier="OpenOrders">
  <Record>
    <PropertyValue Property="Text" String="Offene Bestellungen" />
    <PropertyValue Property="SelectionVariant">
      <Record>
        <PropertyValue Property="SelectOptions">
          <Collection>
            <Record Type="UI.SelectOptionType">
              <PropertyValue Property="PropertyName" PropertyPath="Status" />
              <PropertyValue Property="Ranges">
                <Collection>
                  <Record Type="UI.SelectionRangeType">
                    <PropertyValue Property="Sign"   EnumMember="UI.SelectionRangeSignType/I" />
                    <PropertyValue Property="Option" EnumMember="UI.SelectionRangeOptionType/EQ" />
                    <PropertyValue Property="Low"    String="OPEN" />
                  </Record>
                </Collection>
              </PropertyValue>
            </Record>
          </Collection>
        </PropertyValue>
      </Record>
    </PropertyValue>
    <PropertyValue Property="PresentationVariant">
      <Record>
        <PropertyValue Property="Visualizations">
          <Collection>
            <AnnotationPath>@UI.LineItem#OrdersTab</AnnotationPath>
          </Collection>
        </PropertyValue>
      </Record>
    </PropertyValue>
  </Record>
</Annotation>

<!-- manifest.json: Tab registrieren -->
<!--
{ "key": "tab_open",
  "annotationPath": "com.sap.vocabularies.UI.v1.SelectionPresentationVariant#OpenOrders",
  "entitySet": "Orders" }
-->
```

### Rezept 6: Währungsspalte mit automatischer Einheit

```xml
<!-- Property-Annotation: Dezimalfeld mit Währungsfeld verknüpfen -->
<Annotations Target="MyService.Order/NetAmount">
  <!-- Zeigt: "1.234,56 EUR" statt "1234.56" -->
  <Annotation Term="Measures.ISOCurrency" Path="Currency" />
</Annotations>

<!-- Mock-Daten: Beide Felder müssen vorhanden sein -->
<!-- { "NetAmount": 1234.56, "Currency": "EUR" } -->
```

### Rezept 7: Annotations-Schnell-Checkliste für neue App

```
EntityType-Level (für ObjectPage / ListReport):
  ☐ UI.HeaderInfo         → ObjectPage-Titel
  ☐ UI.LineItem           → Tabellen-Spalten
  ☐ UI.SelectionFields    → Filter-Bar
  ☐ UI.Facets             → ObjectPage-Abschnitte
  ☐ UI.FieldGroup(s)      → Formular-Inhalte

Property-Level (für einzelne Felder):
  ☐ Common.Label          → Feld-Beschriftung (wenn abweichend)
  ☐ Common.ValueList      → F4-Hilfe bei Filterfeldern
  ☐ Measures.ISOCurrency  → Währungsfelder
  ☐ Common.Text           → Code + Text-Kombination

EntitySet-Level (in metadata.xml, nur für Charts):
  ☐ Aggregation.ApplySupported  → Pflicht für alle Charts
  ☐ Aggregation.CustomAggregate → Für Summen/Aggregationen

Optional / erweitert:
  ☐ UI.DataPoint          → Progress-Bar / Rating / Ampel
  ☐ UI.Chart              → Diagramme
  ☐ UI.HeaderFacets       → KPI-Kacheln im Header
  ☐ UI.SelectionVariant   → Vorfilter / Tab-Filter
  ☐ UI.SelectionPresentationVariant → Mehrere Tabs

Controller Extension (für Client-Buttons / Custom-Logik):
  ☐ enableLazyLoading: false                     → in sap.fe.app (Pflicht!)
  ☐ extends.sap.ui.controllerExtensions          → scoped Key #AppId::TargetId
  ☐ target.settings.controllerExtensions         → Kurzname → FQN Mapping
  ☐ target.settings.controlConfiguration.actions → press: .extension.<FQN>.<Methode>
  ☐ webapp/ext/<Name>.controller.js              → ControllerExtension.extend(<FQN>)
```

---

### Rezept 8: Client-Button (Controller Extension)

Ein Toolbar-Button der eine JavaScript-Methode aufruft — ohne Backend.

**Schritt 1 — manifest.json: 3 Stellen befüllen**

```json
// sap.fe: LazyLoading deaktivieren (PFLICHT)
"sap.fe": { "app": { "enableLazyLoading": false } },

// sap.ui5.routing.targets.<TargetName>.options.settings:
"controlConfiguration": {
  "@com.sap.vocabularies.UI.v1.LineItem": {
    "actions": {
      "MyCustomAction": {
        "press": ".extension.<AppNS>.ext.MyExtension.myMethod",
        "text":  "Button-Label",
        "enabled": true          // oder: "{= ${status} === 'OPEN'}"
      }
    }
  }
},
"controllerExtensions": {
  "MyExtension": "<AppNS>.ext.MyExtension"
},

// sap.ui5.extends.extensions.sap.ui.controllerExtensions:
"sap.fe.templates.ListReport.ListReportController#<AppId>::<TargetId>": {
  "controllerName": "<AppNS>.ext.MyExtension"
}
```

**Schritt 2 — webapp/ext/MyExtension.controller.js erstellen:**

```javascript
sap.ui.define(["sap/ui/core/mvc/ControllerExtension", "sap/m/MessageToast"],
function (ControllerExtension, MessageToast) {
  "use strict";
  return ControllerExtension.extend("<AppNS>.ext.MyExtension", {
    override: {
      onInit: function () { /* optional */ }
    },
    myMethod: function (oEvent) {
      var oCtx  = oEvent.getSource().getBindingContext();
      var oData = oCtx ? oCtx.getObject() : null;
      MessageToast.show("Clicked: " + (oData ? oData.ID : "no context"));
    }
  });
});
```

> ⚠️ Eigene Methoden gehören **außerhalb** des `override`-Blocks.

---

### Rezept 9: Navigation ListReport → ObjectPage

```json
// manifest.json — Routing-Konfiguration
"routing": {
  "routes": [
    { "pattern": ":?query:",         "name": "ProductList",       "target": "ProductList" },
    { "pattern": "Product({key}):?query:", "name": "ProductDetail", "target": "ProductObjectPage" }
  ],
  "targets": {
    "ProductList": {
      "type": "Component",
      "name": "sap.fe.templates.ListReport",
      "options": {
        "settings": {
          "contextPath": "/Products",
          "navigation": {
            "Products": {               // ← EntitySet-Name
              "detail": {
                "route": "ProductDetail" // ← Route-Name der ObjectPage
              }
            }
          }
        }
      }
    },
    "ProductObjectPage": {
      "type": "Component",
      "name": "sap.fe.templates.ObjectPage",
      "options": {
        "settings": {
          "contextPath": "/Products",
          "editableHeaderContent": false
        }
      }
    }
  }
}
```

```xml
<!-- annotation.xml: HeaderInfo ist Pflicht für ObjectPage-Titel -->
<Annotation Term="UI.HeaderInfo">
  <Record Type="UI.HeaderInfoType">
    <PropertyValue Property="TypeName"       String="Produkt" />
    <PropertyValue Property="TypeNamePlural" String="Produkte" />
    <PropertyValue Property="Title">
      <Record Type="UI.DataField">
        <PropertyValue Property="Value" Path="ProductName" />
      </Record>
    </PropertyValue>
  </Record>
</Annotation>
```

> **Checkliste Navigation:**  
> ✅ `navigation`-Block im ListReport-Target vorhanden  
> ✅ Route-Pattern der ObjectPage enthält `({key})`  
> ✅ `UI.HeaderInfo` in annotation.xml definiert  
> ✅ `UI.Facets` in annotation.xml definiert (sonst leere ObjectPage)

---

### Rezept 10: Rating-Sterne und Progress-Bar als Spalte

```xml
<!-- Rating-Sterne (z.B. Priorität 1-5) -->
<Annotation Term="UI.DataPoint" Qualifier="PriorityStars">
  <Record Type="UI.DataPointType">
    <PropertyValue Property="Value"       Path="Priority" />
    <PropertyValue Property="TargetValue" Decimal="5" />
    <PropertyValue Property="Visualization" EnumMember="UI.VisualizationType/Rating" />
  </Record>
</Annotation>

<!-- Progress-Bar (z.B. Lagerbestand) -->
<Annotation Term="UI.DataPoint" Qualifier="StockProgress">
  <Record Type="UI.DataPointType">
    <PropertyValue Property="Value"       Path="Stock" />
    <PropertyValue Property="TargetValue" Int="100" />
    <PropertyValue Property="Criticality" Path="StockCriticality" /> <!-- optional: Farbe -->
    <PropertyValue Property="Visualization" EnumMember="UI.VisualizationType/Progress" />
  </Record>
</Annotation>

<!-- Beide als Spalte einbinden — NICHT UI.DataField, sondern UI.DataFieldForAnnotation! -->
<Annotation Term="UI.LineItem">
  <Collection>
    <Record Type="UI.DataFieldForAnnotation">
      <PropertyValue Property="Label"  String="Priorität" />
      <PropertyValue Property="Target" AnnotationPath="@UI.DataPoint#PriorityStars" />
    </Record>
    <Record Type="UI.DataFieldForAnnotation">
      <PropertyValue Property="Label"  String="Lagerbestand" />
      <PropertyValue Property="Target" AnnotationPath="@UI.DataPoint#StockProgress" />
    </Record>
  </Collection>
</Annotation>
```

```json
// Mock-Daten: Integer-Werte für Rating, optionales Criticality-Feld
{ "Priority": 4, "Stock": 73, "StockCriticality": 3 }
// StockCriticality: 1=Rot, 2=Gelb, 3=Grün
```

---

### Rezept 11: Code + Text kombiniert anzeigen

Zeigt z.B. `"01 – Offen"` statt nur `"01"` in Tabelle und Filter.

```xml
<!-- annotation.xml: Common.Text auf dem Code-Feld -->
<Annotations Target="MyService.Task/Status">
  <!-- TextArrangement: wie Code und Text kombiniert werden -->
  <Annotation Term="UI.TextArrangement"
    EnumMember="UI.TextArrangementType/TextFirst" />
  <!-- Text: Path auf das Text-Feld (kann auch Navigation sein) -->
  <Annotation Term="Common.Text" Path="Status_Text" />
</Annotations>
```

```xml
<!-- Alternativer Ansatz über NavigationProperty: -->
<Annotations Target="MyService.Task/QualityTaskLifecycleStatus">
  <Annotation Term="Common.Text" Path="to_StatusType/QualityTaskLifecycleStatus_Text" />
  <Annotation Term="UI.TextArrangement"
    EnumMember="UI.TextArrangementType/TextOnly" />
</Annotations>
```

#### TextArrangement-Varianten
| EnumMember | Anzeige |
|---|---|
| `TextFirst` | "Offen (01)" |
| `TextLast` | "01 (Offen)" |
| `TextOnly` | "Offen" |
| `TextSeparate` | Code und Text in separaten Spalten |

```json
// Mock-Daten: Beide Felder befüllen
{ "Status": "01", "Status_Text": "Offen" }
```

---

### Rezept 12: Backend OData-Action mit Mock-Handler

**Schritt 1 — metadata.xml: Action deklarieren**

```xml
<!-- Bound Action (wirkt auf eine konkrete Entität): -->
<Action Name="completeTask" IsBound="true">
  <Parameter Name="bindingParameter" Type="TaskService.Task" Nullable="false" />
  <ReturnType Type="Edm.Boolean" />
</Action>
<ActionImport Name="completeTask" Action="TaskService.completeTask" />
```

**Schritt 2 — annotation.xml: Button definieren**

```xml
<Record Type="UI.DataFieldForAction">
  <PropertyValue Property="Label"  String="Abschließen" />
  <!-- Format: ServiceNamespace.ActionName -->
  <PropertyValue Property="Action" String="TaskService.completeTask" />
  <PropertyValue Property="InvocationGrouping"
    EnumMember="UI.OperationGroupingType/Isolated" />
</Record>
```

**Schritt 3 — ui5-mock.yaml: Mock-Handler registrieren**

```yaml
services:
  - urlPath: /here/goes/your/serviceurl
    metadataXmlPath: ./webapp/localService/mainService/metadata.xml
    mockdataRootPath: ./webapp/localService/mainService/data
    odataVersion: 4
    mockRequests:
      - method: POST
        path: /Task\(.*\)/TaskService.completeTask
        response:
          statusCode: 200
          body: '{"value": true}'
```

> **Unterschied zur Client-Action:** Bei einer Backend-Action sendet FE automatisch einen OData POST-Request. Bei einer Client-Action (Controller Extension) wird direkt JavaScript ausgeführt — kein Netzwerkaufruf.

---

### Rezept 13: Neue App von Grund auf – Schritt für Schritt

```
┌─────────────────────────────────────────────────────────────────────────────────┐
│  PHASE 1: Grundgerüst                                           │
└─────────────────────────────────────────────────────────────────────────────────┘
① App generieren:
   yo @sap/fiori
   → Template: List Report Page / Worklist
   → Data Source: Upload a Metadata Document
   → Table Type: Responsive Table

② metadata.xml anpassen (EntityType, Properties, EntitySet)
③ Mock-Daten erstellen: webapp/localService/<service>/data/<EntitySet>.json
④ ui5-mock.yaml prüfen: urlPath, odataVersion: 4
⑤ App starten: npm run start-mock
   → Prüfen: Tabelle zeigt Daten? URL korrekt?

┌─────────────────────────────────────────────────────────────────────────────────┐
│  PHASE 2: ListReport ausbauen                                   │
└─────────────────────────────────────────────────────────────────────────────────┘
① UI.LineItem: Spalten definieren (annotation.xml)
② UI.SelectionFields: Filterfelder festlegen
③ UI.HeaderInfo: TypeName + Title definieren (für ObjectPage)
④ Optional: UI.SelectionVariant / SelectionPresentationVariant für Tabs
⑤ Optional: UI.DataPoint + DataFieldForAnnotation für Micro-Charts

┌─────────────────────────────────────────────────────────────────────────────────┐
│  PHASE 3: ObjectPage ausbauen                                   │
└─────────────────────────────────────────────────────────────────────────────────┘
① Navigation in manifest.json konfigurieren (Rezept 9)
② UI.Facets + UI.FieldGroup: Abschnitte und Formularfelder
③ Optional: UI.HeaderFacets für KPI-Kacheln im Header
④ Optional: Common.ValueList für F4-Hilfe in Feldern

┌─────────────────────────────────────────────────────────────────────────────────┐
│  PHASE 4: Erweiterungen                                         │
└─────────────────────────────────────────────────────────────────────────────────┘
① Client-Button: Controller Extension (Rezept 8)
② Backend-Action: metadata.xml + annotation.xml + Mock-Handler (Rezept 12)
③ Prüfen: flexEnabled Buttons haben id-Attribut
④ Prüfen: enableLazyLoading: false wenn Controller Extension verwendet
```

---

### Rezept 14: Kollektions-Facet (zwei FieldGroups nebeneinander)

Zwei Formularabschnitte sollen in einer Zeile **nebeneinander** statt untereinander angezeigt werden — z.B. "Allgemeine Daten" links und "Adresse" rechts.

```xml
<!-- Schritt 1: Zwei FieldGroups definieren -->
<Annotation Term="UI.FieldGroup" Qualifier="GeneralData">
  <Record Type="UI.FieldGroupType">
    <PropertyValue Property="Label" String="Allgemeine Daten" />
    <PropertyValue Property="Data">
      <Collection>
        <Record Type="UI.DataField">
          <PropertyValue Property="Label" String="Name" />
          <PropertyValue Property="Value" Path="ProductName" />
        </Record>
        <Record Type="UI.DataField">
          <PropertyValue Property="Label" String="Kategorie" />
          <PropertyValue Property="Value" Path="Category" />
        </Record>
      </Collection>
    </PropertyValue>
  </Record>
</Annotation>

<Annotation Term="UI.FieldGroup" Qualifier="PriceData">
  <Record Type="UI.FieldGroupType">
    <PropertyValue Property="Label" String="Preisdaten" />
    <PropertyValue Property="Data">
      <Collection>
        <Record Type="UI.DataField">
          <PropertyValue Property="Label" String="Preis" />
          <PropertyValue Property="Value" Path="Price" />
        </Record>
        <Record Type="UI.DataField">
          <PropertyValue Property="Label" String="Währung" />
          <PropertyValue Property="Value" Path="Currency" />
        </Record>
      </Collection>
    </PropertyValue>
  </Record>
</Annotation>

<!-- Schritt 2: CollectionFacet gruppiert beide nebeneinander -->
<Annotation Term="UI.Facets">
  <Collection>
    <Record Type="UI.CollectionFacet">
      <PropertyValue Property="Label" String="Produktdaten" />
      <PropertyValue Property="ID"    String="ProductDataSection" />
      <PropertyValue Property="Facets">
        <Collection>
          <Record Type="UI.ReferenceFacet">
            <PropertyValue Property="Label"  String="Allgemein" />
            <PropertyValue Property="ID"     String="GeneralDataFacet" />
            <PropertyValue Property="Target" AnnotationPath="@UI.FieldGroup#GeneralData" />
          </Record>
          <Record Type="UI.ReferenceFacet">
            <PropertyValue Property="Label"  String="Preise" />
            <PropertyValue Property="ID"     String="PriceDataFacet" />
            <PropertyValue Property="Target" AnnotationPath="@UI.FieldGroup#PriceData" />
          </Record>
        </Collection>
      </PropertyValue>
    </Record>
  </Collection>
</Annotation>
```

```
  Visuelle Wirkung (ObjectPage):
  ┌──────────────────────────────────────────────────────┐
  │ Produktdaten                                         │
  │ ┌───────────────────┐  ┌───────────────────┐         │
  │ │ Allgemein         │  │ Preise            │         │
  │ │ Name:    Laptop   │  │ Preis:  1299 EUR  │         │
  │ │ Kategorie: Electr.│  │ Währung: EUR      │         │
  │ └───────────────────┘  └───────────────────┘         │
  └──────────────────────────────────────────────────────┘
```

---

### Rezept 15: OVP Analytical Card komplett aufsetzen

Eine OVP-Karte die ein Donut-Diagramm (Bestellungen nach Status) zeigt — komplett mit Chart-Annotation, SelectionVariant und PresentationVariant.

**annotation.xml:**
```xml
<!-- 1. Chart-Annotation -->
<Annotation Term="UI.Chart" Qualifier="OrdersByStatus">
  <Record Type="UI.ChartType">
    <PropertyValue Property="Title"     String="Bestellungen nach Status" />
    <PropertyValue Property="ChartType" EnumMember="UI.ChartType/Donut" />
    <PropertyValue Property="Dimensions">
      <Collection>
        <PropertyPath>Status</PropertyPath>
      </Collection>
    </PropertyValue>
    <PropertyValue Property="DimensionAttributes">
      <Collection>
        <Record Type="UI.ChartDimensionAttributeType">
          <PropertyValue Property="Dimension" PropertyPath="Status" />
          <PropertyValue Property="Role" EnumMember="UI.ChartDimensionRoleType/Category" />
        </Record>
      </Collection>
    </PropertyValue>
    <PropertyValue Property="Measures">
      <Collection>
        <PropertyPath>NetAmount</PropertyPath>
      </Collection>
    </PropertyValue>
    <PropertyValue Property="MeasureAttributes">
      <Collection>
        <Record Type="UI.ChartMeasureAttributeType">
          <PropertyValue Property="Measure" PropertyPath="NetAmount" />
          <PropertyValue Property="Role"    EnumMember="UI.ChartMeasureRoleType/Axis1" />
        </Record>
      </Collection>
    </PropertyValue>
  </Record>
</Annotation>

<!-- 2. SelectionVariant: nur nicht-stornierte Bestellungen -->
<Annotation Term="UI.SelectionVariant" Qualifier="NotCancelled">
  <Record Type="UI.SelectionVariantType">
    <PropertyValue Property="Text" String="Nicht storniert" />
    <PropertyValue Property="SelectOptions">
      <Collection>
        <Record Type="UI.SelectOptionType">
          <PropertyValue Property="PropertyName" PropertyPath="Status" />
          <PropertyValue Property="Ranges">
            <Collection>
              <Record Type="UI.SelectionRangeType">
                <PropertyValue Property="Sign"   EnumMember="UI.SelectionRangeSignType/E" />
                <PropertyValue Property="Option" EnumMember="UI.SelectionRangeOptionType/EQ" />
                <PropertyValue Property="Low"    String="CANCELLED" />
              </Record>
            </Collection>
          </PropertyValue>
        </Record>
      </Collection>
    </PropertyValue>
  </Record>
</Annotation>

<!-- 3. PresentationVariant: Chart verwenden, nach NetAmount absteigend -->
<Annotation Term="UI.PresentationVariant" Qualifier="ChartPV">
  <Record Type="UI.PresentationVariantType">
    <PropertyValue Property="SortOrder">
      <Collection>
        <Record Type="Common.SortOrderType">
          <PropertyValue Property="Property"   PropertyPath="NetAmount" />
          <PropertyValue Property="Descending" Bool="true" />
        </Record>
      </Collection>
    </PropertyValue>
    <PropertyValue Property="Visualizations">
      <Collection>
        <AnnotationPath>@UI.Chart#OrdersByStatus</AnnotationPath>
      </Collection>
    </PropertyValue>
  </Record>
</Annotation>
```

**manifest.json Karten-Eintrag:**
```json
"cardAnalytical": {
  "model": "mainService",
  "template": "sap.ovp.cards.analytical",
  "settings": {
    "title"                     : "Bestellungen nach Status",
    "entitySet"                 : "SalesOrders",
    "selectionAnnotationPath"   : "com.sap.vocabularies.UI.v1.SelectionVariant#NotCancelled",
    "chartAnnotationPath"       : "com.sap.vocabularies.UI.v1.Chart#OrdersByStatus",
    "presentationAnnotationPath": "com.sap.vocabularies.UI.v1.PresentationVariant#ChartPV"
  }
}
```

**metadata.xml (Pflicht für Chart):**
```xml
<Annotations Target="MainService.MainContainer/SalesOrders">
  <Annotation Term="Aggregation.ApplySupported">
    <Record Type="Aggregation.ApplySupportedType">
      <PropertyValue Property="AggregatableProperties">
        <Collection>
          <Record Type="Aggregation.AggregatablePropertyType">
            <PropertyValue Property="Property" PropertyPath="NetAmount" />
          </Record>
        </Collection>
      </PropertyValue>
    </Record>
  </Annotation>
  <Annotation Term="Aggregation.CustomAggregate" Qualifier="NetAmount" String="Edm.Decimal" />
</Annotations>
```

---

### Rezept 16: KPI-Kachel im ObjectPage-Header

KPI mit Gesamtbestellanzahl + Donut-Chart beim Drill-Down.

```xml
<!-- 1. DataPoint: Der Zahlenwert für die KPI-Kachel -->
<Annotation Term="UI.DataPoint" Qualifier="TotalOrdersDP">
  <Record Type="UI.DataPointType">
    <PropertyValue Property="Value" Path="TotalOrders" />
    <PropertyValue Property="Title" String="Bestellungen gesamt" />
    <PropertyValue Property="Criticality" EnumMember="UI.CriticalityType/Positive" />
  </Record>
</Annotation>

<!-- 2. Chart für den Drill-Down -->
<Annotation Term="UI.Chart" Qualifier="OrdersChart">
  <Record Type="UI.ChartType">
    <PropertyValue Property="ChartType" EnumMember="UI.ChartType/Donut" />
    <PropertyValue Property="Dimensions">
      <Collection><PropertyPath>Category</PropertyPath></Collection>
    </PropertyValue>
    <PropertyValue Property="DimensionAttributes">
      <Collection>
        <Record Type="UI.ChartDimensionAttributeType">
          <PropertyValue Property="Dimension" PropertyPath="Category" />
          <PropertyValue Property="Role" EnumMember="UI.ChartDimensionRoleType/Category" />
        </Record>
      </Collection>
    </PropertyValue>
    <PropertyValue Property="Measures">
      <Collection><PropertyPath>TotalOrders</PropertyPath></Collection>
    </PropertyValue>
    <PropertyValue Property="MeasureAttributes">
      <Collection>
        <Record Type="UI.ChartMeasureAttributeType">
          <PropertyValue Property="Measure" PropertyPath="TotalOrders" />
          <PropertyValue Property="Role"    EnumMember="UI.ChartMeasureRoleType/Axis1" />
        </Record>
      </Collection>
    </PropertyValue>
  </Record>
</Annotation>

<!-- 3. PresentationVariant für den Chart -->
<Annotation Term="UI.PresentationVariant" Qualifier="OrdersPV">
  <Record Type="UI.PresentationVariantType">
    <PropertyValue Property="Visualizations">
      <Collection>
        <AnnotationPath>@UI.Chart#OrdersChart</AnnotationPath>
      </Collection>
    </PropertyValue>
  </Record>
</Annotation>

<!-- 4. KPI: Verknüpft DataPoint + PresentationVariant -->
<Annotation Term="UI.KPI" Qualifier="TotalOrdersKPI">
  <Record Type="UI.KPIType">
    <PropertyValue Property="DataPoint" AnnotationPath="@UI.DataPoint#TotalOrdersDP" />
    <PropertyValue Property="Detail">
      <Record Type="UI.KPIDetailType">
        <PropertyValue Property="DefaultPresentationVariant"
                       AnnotationPath="@UI.PresentationVariant#OrdersPV" />
      </Record>
    </PropertyValue>
  </Record>
</Annotation>

<!-- 5. HeaderFacets: KPI in den ObjectPage-Header einbinden -->
<Annotation Term="UI.HeaderFacets">
  <Collection>
    <Record Type="UI.ReferenceFacet">
      <PropertyValue Property="Label"  String="Gesamt" />
      <PropertyValue Property="Target" AnnotationPath="@UI.KPI#TotalOrdersKPI" />
    </Record>
  </Collection>
</Annotation>
```

---

### Rezept 17: Mehrsprachige App (i18n für Annotations-Labels)

In Standard Fiori Elements XML-Annotationen ist kein direktes i18n-Binding möglich. Labels werden entweder als feste Strings oder über die i18n-Datei der App verwaltet.

**Methode 1 — `Common.Label` mit Schlüssel (wird automatisch übersetzt):**
```xml
<!-- Kein i18n-Binding in annotation.xml — stattdessen: fester englischer Text -->
<!-- Übersetzung erfolgt über SAP Translation Tool oder Gateway-Übersetzung -->
<Annotations Target="MyService.Product/Price">
  <Annotation Term="Common.Label" String="Price" />
</Annotations>
```

**Methode 2 — i18n in OVP-Annotationen (OVP-spezifisch!):**
```xml
<!-- NUR in OVP-Annotationen (fioriovp, problemsolvingovp): -->
<PropertyValue Property="Label" String="{@i18n>labelPrice}" />
```

```properties
# webapp/i18n/i18n.properties
labelPrice=Preis

# webapp/i18n/i18n_de.properties
labelPrice=Preis

# webapp/i18n/i18n_en.properties
labelPrice=Price
```

**Methode 3 — Labels über i18n-Properties direkt in der App (Standard-Fiori):**
```json
// manifest.json: i18n-Modell registrieren
"models": {
  "i18n": {
    "type": "sap.ui.model.resource.ResourceModel",
    "settings": {
      "bundleName": "com.example.myapp.i18n.i18n"
    }
  }
}
```

---

*Generiert aus den Projekten: `fiorilistreport` · `fiorielements` · `fioriovp` · `problemsolvingovp` · `taskworklist`*

---

## 18. OData Datenmodell – Grundlagen Cheatsheet

> Kurz-Referenz für `metadata.xml`: EntityTypes, Properties, NavigationProperties, Associations.
> Gilt sowohl für **OData V4** (FE V4 Apps) als auch **OData V2** (Smart Templates).

---

### 18.1 EntityType – die Datentabelle

Ein `EntityType` entspricht konzeptionell einer **Datenbanktabelle** oder einem **CDS View**.
Er definiert welche Felder ein Datenobjekt hat.

```xml
<EntityType Name="SalesOrder">
    <Key>
        <PropertyRef Name="OrderID"/>   <!-- Primärschlüssel -->
    </Key>
    <Property Name="OrderID"      Type="Edm.String"  Nullable="false" MaxLength="10"/>
    <Property Name="CustomerName" Type="Edm.String"  MaxLength="80"/>
    <Property Name="NetAmount"    Type="Edm.Decimal" Precision="13" Scale="2"/>
    <Property Name="OrderDate"    Type="Edm.DateTimeOffset"/>
</EntityType>
```

**Regeln:**
- Jeder EntityType braucht mindestens einen `<Key>`
- `Nullable="false"` = Pflichtfeld (entspricht `NOT NULL` in SQL)
- Der Name endet in V2 oft auf `...Type` (z.B. `SalesOrderType`), in V4 oft ohne Suffix

---

### 18.2 EntitySet – der Endpunkt

Ein `EntitySet` ist die **URL-Ressource** — der eigentliche OData-Endpunkt für eine Liste von Objekten.
Jeder EntityType braucht genau einen EntitySet (kann aber mehrere haben).

```xml
<EntityContainer Name="MainContainer">
    <EntitySet Name="SalesOrders" EntityType="MainService.SalesOrder"/>
    <!--               ↑ URL-Pfad        ↑ Namespace.EntityType-Name -->
</EntityContainer>
```

**URL-Zugriff:**
```
GET /SalesOrders          → Liste aller SalesOrders
GET /SalesOrders('ORD001') → Einzelner Datensatz
GET /SalesOrders?$top=10  → Mit OData-Parametern
```

**EntityType vs. EntitySet:**

| | EntityType | EntitySet |
|---|---|---|
| Entspricht | Tabellendefinition (Schema) | Tabelle selbst (Instanz) |
| Definiert | Felder und Typen | URL-Endpunkt |
| Analogie | `CREATE TABLE` | `SELECT * FROM` |
| Ort | `<Schema>` | `<EntityContainer>` |

---

### 18.3 Key & Schlüsseleigenschaften

```xml
<!-- Einfacher Schlüssel: -->
<EntityType Name="Product">
    <Key>
        <PropertyRef Name="ProductID"/>
    </Key>
    <Property Name="ProductID" Type="Edm.String" Nullable="false"/>
</EntityType>

<!-- Zusammengesetzter Schlüssel (z.B. bei Navigation-Entities): -->
<EntityType Name="SalesOrderStep">
    <Key>
        <PropertyRef Name="OrderID"/>    <!-- Fremdschlüssel -->
        <PropertyRef Name="StepID"/>     <!-- Eigenständiger Schlüssel -->
    </Key>
    <Property Name="OrderID" Type="Edm.String" Nullable="false" MaxLength="10"/>
    <Property Name="StepID"  Type="Edm.String" Nullable="false" MaxLength="4"/>
</EntityType>
```

> **Faustregel:** Jede Tabelle, die über eine Navigation Property erreichbar ist (1:n),
> braucht den Fremdschlüssel der Elterntabelle **im Key**.

---

### 18.4 Property-Typen Referenz

| OData Typ | Entspricht | Beispiel-Wert (JSON) | Beispiel (XML) |
|---|---|---|---|
| `Edm.String` | VARCHAR | `"ORD001"` | `Type="Edm.String" MaxLength="10"` |
| `Edm.Int32` | INTEGER | `42` | `Type="Edm.Int32"` |
| `Edm.Int64` | BIGINT | `9999999999` | `Type="Edm.Int64"` |
| `Edm.Decimal` | DECIMAL | `19.99` | `Type="Edm.Decimal" Precision="13" Scale="2"` |
| `Edm.Boolean` | BOOLEAN | `true` | `Type="Edm.Boolean"` |
| `Edm.DateTime` | DATETIME (V2) | `"/Date(1711670400000)/"` | `Type="Edm.DateTime"` |
| `Edm.DateTimeOffset` | TIMESTAMP (V4) | `"2024-03-29T00:00:00Z"` | `Type="Edm.DateTimeOffset"` |
| `Edm.Date` | DATE (V4) | `"2024-03-29"` | `Type="Edm.Date"` |
| `Edm.TimeOfDay` | TIME (V4) | `"14:30:00"` | `Type="Edm.TimeOfDay"` |
| `Edm.Guid` | GUID/UUID | `"550e8400-e29b-41d4-a716..."` | `Type="Edm.Guid"` |
| `Edm.Byte` | TINYINT (0-255) | `7` | `Type="Edm.Byte"` |

> **V2 vs. V4 Datum:**
> - V2: `Edm.DateTime` → JSON-Wert: `/Date(1711670400000)/` (Millisekunden seit 1970)
> - V4: `Edm.DateTimeOffset` → JSON-Wert: `2024-03-29T00:00:00Z` (ISO 8601)

---

### 18.5 NavigationProperty – Beziehungen zwischen Entities

Eine `NavigationProperty` ist der **Fremdschlüssel-Zeiger** im OData-Modell.
Sie ermöglicht `$expand` und Navigation-Bindings in Fragmenten (`{path: 'to_Steps'}`).

**1:1 Beziehung** (z.B. SalesOrder → Kunde):
```xml
<NavigationProperty
    Name="to_Customer"
    Type="MainService.Customer"          <!-- kein Collection() -->
    Partner="to_Orders"/>               <!-- Rück-Navigation -->
```

**1:n Beziehung** (z.B. SalesOrder → Steps):
```xml
<NavigationProperty
    Name="to_SalesOrderSteps"
    Type="Collection(MainService.SalesOrderStep)"   <!-- Collection() = 1:n -->
    Partner="to_SalesOrder"/>                       <!-- Rück-Navigation -->
```

**Rück-Navigation** (auf der n-Seite):
```xml
<!-- Im SalesOrderStep EntityType: -->
<NavigationProperty
    Name="to_SalesOrder"
    Type="MainService.SalesOrder"        <!-- kein Collection() = zurück zur 1-Seite -->
    Partner="to_SalesOrderSteps"/>
```

**Naming-Konvention:**
- Präfix `to_` kennzeichnet Navigation Properties (SAP-Standard)
- Name beschreibt das Ziel, nicht die Quelle: `to_SalesOrderSteps`, `to_Customer`, `to_StatusVH`

---

### 18.6 V4 vs. V2: NavigationProperty Syntax

**OData V4** (FE V4 Apps — `edmx Version="4.0"`):

```xml
<!-- V4: NavigationProperty direkt im EntityType, kein Association nötig -->
<EntityType Name="SalesOrder">
    <NavigationProperty
        Name="to_SalesOrderSteps"
        Type="Collection(MainService.SalesOrderStep)"
        Partner="to_SalesOrder"/>
</EntityType>

<!-- V4: NavigationPropertyBinding im EntitySet -->
<EntitySet Name="SalesOrders" EntityType="MainService.SalesOrder">
    <NavigationPropertyBinding Path="to_SalesOrderSteps" Target="SalesOrderSteps"/>
</EntitySet>
```

**OData V2** (Smart Templates — `edmx Version="1.0"`, `m:DataServiceVersion="2.0"`):

```xml
<!-- V2: NavigationProperty referenziert eine Association -->
<EntityType Name="SalesOrderType">
    <NavigationProperty
        Name="to_SalesOrderSteps"
        Relationship="Z_SRV.assoc_Order_Steps"   <!-- Name der Association -->
        FromRole="FromRole_Order"
        ToRole="ToRole_Steps"/>
</EntityType>

<!-- V2: Association MUSS separat definiert werden (Pflicht!) -->
<Association Name="assoc_Order_Steps">
    <End Type="Z_SRV.SalesOrderType"     Multiplicity="1"  Role="FromRole_Order"/>
    <End Type="Z_SRV.SalesOrderStepType" Multiplicity="*"  Role="ToRole_Steps"/>
    <ReferentialConstraint>
        <Principal Role="FromRole_Order">
            <PropertyRef Name="OrderID"/>
        </Principal>
        <Dependent Role="ToRole_Steps">
            <PropertyRef Name="OrderID"/>
        </Dependent>
    </ReferentialConstraint>
</Association>

<!-- V2: AssociationSet im EntityContainer -->
<AssociationSet Name="assoc_Order_Steps_Set"
    Association="Z_SRV.assoc_Order_Steps">
    <End EntitySet="SalesOrderSet"      Role="FromRole_Order"/>
    <End EntitySet="SalesOrderStepSet"  Role="ToRole_Steps"/>
</AssociationSet>
```

**Vergleich V4 vs. V2:**

| | OData V4 | OData V2 |
|---|---|---|
| Navigation definiert in | `NavigationProperty` direkt | `NavigationProperty` + `Association` + `AssociationSet` |
| Pflicht-Elemente | 2 (NavProp + Binding) | 4 (NavProp + Assoc + AssocSet + Binding) |
| Binding-Element | `NavigationPropertyBinding` | `AssociationSet` (+ optional Binding) |
| Komplexität | Einfach | Aufwändig |

---

### 18.7 EntityContainer – Alles zusammen

Der `EntityContainer` ist der **Einstiegspunkt** des OData-Service.
Alle EntitySets, FunctionImports und AssociationSets leben hier.

```xml
<EntityContainer Name="MainContainer"
    m:IsDefaultEntityContainer="true">   <!-- V2: Pflicht! V4: optional -->

    <!-- EntitySets: die URL-Endpunkte -->
    <EntitySet Name="SalesOrders"     EntityType="MainService.SalesOrder">
        <NavigationPropertyBinding Path="to_SalesOrderSteps" Target="SalesOrderSteps"/>
    </EntitySet>
    <EntitySet Name="SalesOrderSteps" EntityType="MainService.SalesOrderStep">
        <NavigationPropertyBinding Path="to_SalesOrder" Target="SalesOrders"/>
    </EntitySet>

    <!-- FunctionImports (V2/V4): Backend-Aktionen -->
    <FunctionImport Name="ConfirmOrder"
        ReturnType="MainService.SalesOrder"
        m:HttpMethod="POST">
        <Parameter Name="OrderID" Type="Edm.String" Mode="In"/>
    </FunctionImport>

</EntityContainer>
```

---

### 18.8 Kardinalitäten (Multiplicity)

Kardinalitäten beschreiben **wie viele** Objekte auf jeder Seite einer Beziehung stehen können.

| Symbol | Bedeutung | Beispiel |
|---|---|---|
| `1` | Genau eines | Eine Bestellung hat genau einen Kunden |
| `0..1` | Null oder eines | Eine Bestellung kann (muss nicht) eine Lieferadresse haben |
| `*` | Null oder viele | Eine Bestellung hat null bis viele Positionen |

**Typische Muster:**

```
1 ──── * :  Eltern-Kind   (SalesOrder → SalesOrderStep)
1 ──── 0..1: Optional 1:1 (SalesOrder → DraftAdminData)
* ──── 1 :  Lookup/VH    (SalesOrder → StatusVH)
* ──── * :  Selten        (eher vermeiden in OData)
```

In V4 erkennt man die Kardinalität am `Type`:
- `Type="Collection(...)"` → n-Seite (viele)
- `Type="MainService.SalesOrder"` → 1-Seite (genau eines)

---

### 18.9 SAP-spezifische Attribute (V2)

In OData V2 verwendet SAP den Namespace `sap:` für UI-Metadaten direkt in der Metadata.

```xml
<EntityType Name="SalesOrderType"
    sap:label="Sales Order"          <!-- Anzeigename in der UI -->
    sap:content-version="1">         <!-- Interne SAP-Versionsangabe -->

    <Property Name="OrderID"
        Type="Edm.String"
        sap:label="Order ID"          <!-- Spaltenüberschrift -->
        sap:creatable="false"         <!-- Kann nicht erstellt werden -->
        sap:updatable="false"         <!-- Kann nicht geändert werden -->
        sap:sortable="true"           <!-- Sortierbar in der Tabelle -->
        sap:filterable="true"         <!-- Filterbar in der Filterbar -->
        sap:searchable="true"         <!-- In der Freitextsuche -->
        sap:display-format="NonNegative"  <!-- Nur positive Zahlen -->
        sap:text="OrderDescription"   <!-- Textwert aus anderem Feld -->
        sap:unit="CurrencyCode"       <!-- Einheit/Währung aus anderem Feld -->
        sap:value-list="standard"     <!-- F4-Hilfe vorhanden -->
        sap:semantics="currency-code"/><!-- Ist ein Währungscode -->

</EntityType>

<EntitySet Name="SalesOrderSet"
    sap:searchable="true"            <!-- Volltextsuche erlaubt -->
    sap:creatable="true"             <!-- POST erlaubt -->
    sap:updatable="true"             <!-- PATCH/PUT erlaubt -->
    sap:deletable="false"/>          <!-- DELETE nicht erlaubt -->
```

**Häufig genutzte `sap:` Attribute:**

| Attribut | Wo | Bedeutung |
|---|---|---|
| `sap:label` | Property, EntityType | Anzeigename |
| `sap:text` | Property | Anderes Feld liefert den Anzeigetext (z.B. `StatusText` für `Status`) |
| `sap:unit` | Property | Anderes Feld liefert die Einheit (z.B. `CurrencyCode`) |
| `sap:value-list` | Property | F4-Werthilfe vorhanden (`"standard"` oder `"fixed-values"`) |
| `sap:semantics` | Property | `"currency-code"`, `"unit-of-measure"`, `"email"`, `"tel"` |
| `sap:creatable` | Property, EntitySet | Kann beim POST mitgesendet werden |
| `sap:updatable` | Property, EntitySet | Kann geändert werden |
| `sap:filterable` | Property | Erscheint in der Filterleiste |
| `sap:sortable` | Property | Spalte sortierbar |
| `sap:display-format` | Property | `"Date"`, `"NonNegative"`, `"UpperCase"` |

---

### 18.10 Schnell-Referenz: metadata.xml Skelett

**OData V4 (FE V4 Apps):**

```xml
<?xml version="1.0" encoding="utf-8"?>
<edmx:Edmx Version="4.0" xmlns:edmx="http://docs.oasis-open.org/odata/ns/edmx">
    <edmx:DataServices>
        <Schema Namespace="MyService" xmlns="http://docs.oasis-open.org/odata/ns/edm">

            <!-- 1. EntityType(s) -->
            <EntityType Name="MyEntity">
                <Key><PropertyRef Name="ID"/></Key>
                <Property Name="ID"   Type="Edm.String" Nullable="false"/>
                <Property Name="Name" Type="Edm.String"/>
                <!-- Navigation zu Child: -->
                <NavigationProperty Name="to_Children"
                    Type="Collection(MyService.ChildEntity)" Partner="to_Parent"/>
            </EntityType>

            <EntityType Name="ChildEntity">
                <Key><PropertyRef Name="ParentID"/><PropertyRef Name="ChildID"/></Key>
                <Property Name="ParentID" Type="Edm.String" Nullable="false"/>
                <Property Name="ChildID"  Type="Edm.String" Nullable="false"/>
                <Property Name="Value"    Type="Edm.String"/>
                <!-- Rück-Navigation: -->
                <NavigationProperty Name="to_Parent"
                    Type="MyService.MyEntity" Partner="to_Children"/>
            </EntityType>

            <!-- 2. EntityContainer -->
            <EntityContainer Name="MyContainer">
                <EntitySet Name="MyEntities" EntityType="MyService.MyEntity">
                    <NavigationPropertyBinding Path="to_Children" Target="ChildEntities"/>
                </EntitySet>
                <EntitySet Name="ChildEntities" EntityType="MyService.ChildEntity">
                    <NavigationPropertyBinding Path="to_Parent" Target="MyEntities"/>
                </EntitySet>
            </EntityContainer>

        </Schema>
    </edmx:DataServices>
</edmx:Edmx>
```

**OData V2 (Smart Templates):**

```xml
<?xml version="1.0" encoding="utf-8"?>
<edmx:Edmx Version="1.0"
    xmlns:edmx="http://schemas.microsoft.com/ado/2007/06/edmx"
    xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata"
    xmlns:sap="http://www.sap.com/Protocols/SAPData">
    <edmx:DataServices m:DataServiceVersion="2.0">
        <Schema Namespace="Z_MY_SRV"
            xmlns="http://schemas.microsoft.com/ado/2008/09/edm">

            <!-- 1. EntityType(s) -->
            <EntityType Name="MyEntityType" sap:label="My Entity">
                <Key><PropertyRef Name="ID"/></Key>
                <Property Name="ID"   Type="Edm.String" Nullable="false" sap:label="ID"/>
                <Property Name="Name" Type="Edm.String" sap:label="Name"/>
                <!-- NavigationProperty referenziert Association: -->
                <NavigationProperty Name="to_Children"
                    Relationship="Z_MY_SRV.assoc_Parent_Child"
                    FromRole="FromRole_Parent" ToRole="ToRole_Child"/>
            </EntityType>

            <EntityType Name="ChildEntityType" sap:label="Child">
                <Key><PropertyRef Name="ParentID"/><PropertyRef Name="ChildID"/></Key>
                <Property Name="ParentID" Type="Edm.String" Nullable="false"/>
                <Property Name="ChildID"  Type="Edm.String" Nullable="false"/>
                <NavigationProperty Name="to_Parent"
                    Relationship="Z_MY_SRV.assoc_Parent_Child"
                    FromRole="ToRole_Child" ToRole="FromRole_Parent"/>
            </EntityType>

            <!-- 2. Association (Pflicht in V2!) -->
            <Association Name="assoc_Parent_Child">
                <End Type="Z_MY_SRV.MyEntityType"  Multiplicity="1" Role="FromRole_Parent"/>
                <End Type="Z_MY_SRV.ChildEntityType" Multiplicity="*" Role="ToRole_Child"/>
                <ReferentialConstraint>
                    <Principal Role="FromRole_Parent"><PropertyRef Name="ID"/></Principal>
                    <Dependent Role="ToRole_Child"><PropertyRef Name="ParentID"/></Dependent>
                </ReferentialConstraint>
            </Association>

            <!-- 3. EntityContainer -->
            <EntityContainer Name="Z_MY_SRV_Entities"
                m:IsDefaultEntityContainer="true">
                <EntitySet Name="MyEntitySet"   EntityType="Z_MY_SRV.MyEntityType"
                    sap:creatable="true" sap:updatable="true" sap:deletable="false"/>
                <EntitySet Name="ChildEntitySet" EntityType="Z_MY_SRV.ChildEntityType"
                    sap:creatable="true" sap:updatable="true" sap:deletable="false"/>
                <!-- AssociationSet (Pflicht in V2!) -->
                <AssociationSet Name="assoc_Parent_Child_Set"
                    Association="Z_MY_SRV.assoc_Parent_Child">
                    <End EntitySet="MyEntitySet"   Role="FromRole_Parent"/>
                    <End EntitySet="ChildEntitySet" Role="ToRole_Child"/>
                </AssociationSet>
            </EntityContainer>

        </Schema>
    </edmx:DataServices>
</edmx:Edmx>
```

**Checkliste für neue metadata.xml:**

```
☐ Jeder EntityType hat einen <Key>
☐ Key-Properties sind Nullable="false"
☐ Zusammengesetzte Keys: Fremdschlüssel-Felder sind im Key enthalten
☐ NavigationProperty Name beginnt mit to_
☐ 1:n Navigation: Type="Collection(...)"  /  1:1: kein Collection()
☐ Partner-Attribute zeigen aufeinander
☐ EntityContainer: NavigationPropertyBinding für beide Richtungen
☐ V2 zusätzlich: Association + AssociationSet vorhanden
☐ Mock-JSON: Schlüsselfelder stimmen mit EntityType Key überein
☐ Mock-JSON: Format plain Array [...] (kein {"value":[...]} Wrapper)
```

---

## 17. Custom Column mit XML-Fragment & Formatter (MicroProcessFlow)

> Praxisbeispiel aus `com.example.fiorilistreport`: Ein `sap.suite.ui.commons.MicroProcessFlow`
> wird als Custom Column in einer ListReport-Tabelle eingebunden.
> Daten kommen aus einer Navigation Property (`to_SalesOrderSteps`).
> Der State jedes Schritts wird über einen **Controller Extension Formatter** berechnet.

---

### 17.1 Übersicht: Alle beteiligten Dateien

```
webapp/
├── manifest.json                            ← (1) Library + controllerExtension + controlConfiguration
├── ext/
│   ├── fragment/                            ← Ordner: fragment (Singular!)
│   │   ├── MicroProcessFlow.fragment.xml    ← (2a) Prozessschritte als Kreisfolge
│   │   ├── InfoLabel.fragment.xml           ← (2b) Farbiges Label (sap.tnt.InfoLabel)
│   │   └── GenericTag.fragment.xml          ← (2c) Status-Tag mit Betrag (sap.m.GenericTag)
│   └── controller/
│       └── SalesOrderListExt.controller.js  ← (3) ControllerExtension mit allen Formatter-Funktionen
└── localService/mainService/
    ├── metadata.xml                          ← (4) NavigationProperty + EntityType
    └── data/
        ├── SalesOrders.json                  ← (5) Haupt-Mock-Daten (plain Array)
        └── SalesOrderSteps.json              ← (6) Navigations-Mock-Daten (plain Array)
```

> **Wichtig:** Der Ordner heißt `fragment` (**Singular**), nicht `fragments`. Der `template`-Wert in
> `manifest.json` wird als UI5-Modulname aufgelöst (Punkte = Verzeichnistrennzeichen) und dann
> `.fragment.xml` angehängt:
> `com.example.fiorilistreport.ext.fragment.MicroProcessFlow`
> → `webapp/ext/fragment/MicroProcessFlow.fragment.xml`

---

### 17.2 metadata.xml – Navigation Property definieren

Die Navigation Property verbindet `SalesOrder` 1:n mit `SalesOrderStep`.
Der Mock-Server löst `to_SalesOrderSteps` automatisch über `NavigationPropertyBinding` auf.

```xml
<!-- EntityType: Hauptentity — NavigationProperty hinzufügen -->
<EntityType Name="SalesOrder">
    <Key>
        <PropertyRef Name="OrderID"/>
    </Key>
    <Property Name="OrderID"      Type="Edm.String" Nullable="false" MaxLength="10"/>
    <Property Name="CustomerName" Type="Edm.String" MaxLength="80"/>
    <Property Name="Status"       Type="Edm.String" MaxLength="20"/>
    <Property Name="NetAmount"    Type="Edm.Decimal" Precision="13" Scale="2"/>
    <!-- ... weitere Properties ... -->

    <!-- ⬇ Navigation Property: 1 SalesOrder → n SalesOrderSteps -->
    <NavigationProperty
        Name="to_SalesOrderSteps"
        Type="Collection(MainService.SalesOrderStep)"
        Partner="to_SalesOrder"/>
</EntityType>

<!-- EntityType: Navigations-Entity -->
<EntityType Name="SalesOrderStep">
    <Key>
        <PropertyRef Name="OrderID"/>
        <PropertyRef Name="StepID"/>
    </Key>
    <Property Name="OrderID"        Type="Edm.String"  Nullable="false" MaxLength="10"/>
    <Property Name="StepID"         Type="Edm.String"  Nullable="false" MaxLength="4"/>
    <Property Name="StepName"       Type="Edm.String"  MaxLength="60"/>
    <Property Name="StepStatus"     Type="Edm.String"  MaxLength="2"/>
    <!-- "01"=Offen, "02"=InArbeit, "03"=Fertig, "04"=Fehler -->
    <Property Name="StepSortNumber" Type="Edm.Int32"/>
    <Property Name="DueDate"        Type="Edm.DateTimeOffset"/>
    <Property Name="Responsible"    Type="Edm.String"  MaxLength="12"/>
    <!-- Rück-Navigation (optional, aber für Mock-Server wichtig) -->
    <NavigationProperty
        Name="to_SalesOrder"
        Type="MainService.SalesOrder"
        Partner="to_SalesOrderSteps"/>
</EntityType>

<!-- EntityContainer: NavigationPropertyBinding MUSS hier deklariert werden -->
<EntityContainer Name="MainContainer">
    <EntitySet Name="SalesOrders" EntityType="MainService.SalesOrder">
        <!-- ⬇ Verbindet to_SalesOrderSteps mit dem EntitySet SalesOrderSteps -->
        <NavigationPropertyBinding Path="to_SalesOrderSteps" Target="SalesOrderSteps"/>
    </EntitySet>
    <EntitySet Name="SalesOrderSteps" EntityType="MainService.SalesOrderStep">
        <NavigationPropertyBinding Path="to_SalesOrder" Target="SalesOrders"/>
    </EntitySet>
</EntityContainer>
```

> **Warum `NavigationPropertyBinding`?**
> Der Mock-Server (`@sap-ux/ui5-middleware-fe-mockserver`) liest den `NavigationPropertyBinding`
> und verknüpft automatisch `SalesOrders('ORD001')/to_SalesOrderSteps` mit allen Einträgen
> aus `SalesOrderSteps.json` bei denen `OrderID = 'ORD001'` ist.
> Ohne diesen Eintrag → 404 oder leere Navigation.

---

### 17.3 Mock-Daten – SalesOrderSteps.json

**Wichtig:** Format ist ein **plain Array** `[...]` — KEIN `{ "value": [...] }` Wrapper.
Jede Order sollte dieselbe Anzahl Schritte haben (für konsistente Darstellung).

```json
[
  {
    "OrderID": "ORD001",
    "StepID": "0010",
    "StepName": "Order Received",
    "StepStatus": "03",
    "StepSortNumber": 10,
    "DueDate": "2024-04-05T00:00:00Z",
    "Responsible": "USER01"
  },
  {
    "OrderID": "ORD001",
    "StepID": "0020",
    "StepName": "Picking",
    "StepStatus": "03",
    "StepSortNumber": 20,
    "DueDate": "2024-04-07T00:00:00Z",
    "Responsible": "USER02"
  },
  {
    "OrderID": "ORD001",
    "StepID": "0030",
    "StepName": "Packing",
    "StepStatus": "02",
    "StepSortNumber": 30,
    "DueDate": "2024-04-08T00:00:00Z",
    "Responsible": "USER03"
  },
  {
    "OrderID": "ORD001",
    "StepID": "0040",
    "StepName": "Shipped",
    "StepStatus": "01",
    "StepSortNumber": 40,
    "DueDate": "2024-04-10T00:00:00Z",
    "Responsible": "USER04"
  }
  // weitere Orders: ORD002, ORD003, ORD004 mit je 4 Steps ...
]
```

**Status-Werte (Konvention):**

| StepStatus | Bedeutung  | MicroProcessFlow State | Visuell          |
|-----------|------------|------------------------|------------------|
| `"01"`    | Offen      | `Planned`              | Leerer Kreis ○   |
| `"02"`    | In Arbeit  | `Neutral`              | Grauer Kreis ●   |
| `"03"`    | Fertig     | `Positive`             | Grüner Kreis ✔   |
| `"04"`    | Fehler     | `Negative`             | Roter Kreis ✘    |

---

### 17.4 Das XML-Fragment

```xml
<!-- webapp/ext/fragments/MicroProcessFlow.fragment.xml -->
<core:FragmentDefinition
    xmlns:m="sap.m"
    xmlns:core="sap.ui.core"
    xmlns="sap.suite.ui.commons">

    <MicroProcessFlow
        id="salesOrderProcessFlow"
        renderType="NoWrap"
        content="{
            path: 'to_SalesOrderSteps',
            sorter: [{ path: 'StepSortNumber' }],
            templateShareable: false
        }">
        <content>
            <MicroProcessFlowItem
                id="salesOrderProcessFlowItem"
                visible="true"
                title="{StepName}"
                state="{
                    parts: [{ path: 'StepStatus' }],
                    formatter: '.extension.com.example.fiorilistreport.ext.controller.SalesOrderListExt.formatStepState'
                }"
                icon="{
                    parts: [{ path: 'StepStatus' }],
                    formatter: '.extension.com.example.fiorilistreport.ext.controller.SalesOrderListExt.formatStepIcon'
                }"
                press=".extension.com.example.fiorilistreport.ext.controller.SalesOrderListExt.onStepPress"
                showSeparator="true"
                stepWidth="14px" />
        </content>
    </MicroProcessFlow>

</core:FragmentDefinition>
```

**Erklärung der Binding-Ausdrücke:**

| Attribut | Wert | Erklärung |
|---|---|---|
| `content` | `{ path: 'to_SalesOrderSteps', ... }` | Bindet die Liste der Steps über die Navigation Property |
| `sorter` | `[{ path: 'StepSortNumber' }]` | Sortiert Steps aufsteigend nach `StepSortNumber` |
| `templateShareable: false` | — | Pflicht bei `content`-Binding in Aggregationen innerhalb von Fragmenten |
| `title` | `{StepName}` | Einfaches Property-Binding auf Step-Kontext |
| `state` | `parts + formatter` | Berechneter Wert → siehe Formatter-Erklärung unten |
| `icon` | `parts + formatter` | Berechneter Wert → SAP-Icon-URI aus Status |
| `press` | `.extension.<ns>.onStepPress` | Event-Handler in der Controller Extension |

---

### 17.5 Formatter-Syntax im Fragment – detaillierte Erklärung

#### Einfaches Binding vs. Formatter-Binding

```xml
<!-- Einfaches Binding (kein Formatter): -->
<MicroProcessFlowItem title="{StepName}" />

<!-- Formatter-Binding mit einem Property: -->
<MicroProcessFlowItem state="{
    path: 'StepStatus',
    formatter: '.extension.com.example.fiorilistreport.ext.controller.SalesOrderListExt.formatStepState'
}" />

<!-- Formatter-Binding mit parts[] (mehrere Properties kombinieren): -->
<MicroProcessFlowItem state="{
    parts: [{ path: 'StepStatus' }, { path: 'Priority' }],
    formatter: '.extension.com.example.fiorilistreport.ext.controller.SalesOrderListExt.formatStepState'
}" />
```

> **Wann `parts` statt `path`?**
> `parts` ist notwendig, wenn der Formatter **mehrere Felder** gleichzeitig auswerten soll.
> Der Formatter bekommt dann alle Werte als separate Argumente übergeben:
> ```javascript
> formatStepState: function(sStatus, sPriority) { ... }
> ```

#### Der Formatter-Pfad: `.extension.<namespace>.<methode>`

```
.extension
  └── com.example.fiorilistreport       ← App-Namespace
        └── ext.controller              ← Ordnerstruktur: ext/controller/
              └── SalesOrderListExt     ← Dateiname (ohne .controller.js)
                    └── .formatStepState ← Methoden-Name in der Extension
```

- Das führende `.` bedeutet: **relativ zum aktuellen Controller-Kontext** der View
- `extension` ist der feste FE V4 Prefix für Controller Extensions
- Ohne korrekte Registrierung in `manifest.json` → Formatter wird **nicht gefunden** → alle Werte bleiben leer

---

### 17.6 Controller Extension – vollständige Datei

Die Extension stellt Formatter-Funktionen für **alle Custom-Column-Fragmente** bereit.
Jede Methode ist einer bestimmten Custom Column zugeordnet:

| Methode | Fragment | Control-Attribut |
|---|---|---|
| `formatStepState` | `MicroProcessFlow` | `MicroProcessFlowItem.state` |
| `formatStepIcon` | `MicroProcessFlow` | `MicroProcessFlowItem.icon` |
| `onStepPress` | `MicroProcessFlow` | `MicroProcessFlowItem.press` (Event) |
| `formatPriority` | `InfoLabel` | `InfoLabel.colorScheme` (Integer!) |
| `formatCategoryStatus` | `GenericTag` | `GenericTag.status` |

```javascript
// webapp/ext/controller/SalesOrderListExt.controller.js
sap.ui.define([
    "sap/ui/core/mvc/ControllerExtension",
    "sap/m/MessageToast"
], function (ControllerExtension, MessageToast) {
    "use strict";

    return ControllerExtension.extend(
        // ⚠ Dieser Name MUSS identisch mit "controllerName" in manifest.json sein
        "com.example.fiorilistreport.ext.controller.SalesOrderListExt",
        {
            override: {
                onInit: function () {}
            },

            // ── MicroProcessFlow: Step-State ─────────────────────────────────
            // Rückgabe: ValueState-String (Success/Warning/Information/None)
            formatStepState: function (sStatus) {
                switch (sStatus) {
                    case "03": return "Success";      // Fertig    → grün
                    case "04": return "Warning";      // Fehler    → orange
                    case "02": return "Information";  // In Arbeit → blau
                    case "01": return "None";          // Offen     → leer
                    default:   return "None";
                }
            },

            // ── MicroProcessFlow: Step-Icon ──────────────────────────────────
            // Gibt einen SAP-Icon-URI zurück (oder "" für keinen Icon).
            formatStepIcon: function (sStatus) {
                switch (sStatus) {
                    case "03": return "sap-icon://accept";       // ✔ grün
                    case "04": return "sap-icon://in-progress";  // ⟳ orange
                    case "02": return "sap-icon://time-overtime";// Uhr blau
                    case "01": return "";                        // leer
                    default:   return "";
                }
            },

            // ── MicroProcessFlow: Press-Handler ─────────────────────────────
            onStepPress: function (oEvent) {
                var oContext = oEvent.getSource().getBindingContext();
                if (oContext) {
                    var oStep = oContext.getObject();
                    MessageToast.show(oStep.StepName + " – Status: " + oStep.StepStatus);
                }
            },

            // ── InfoLabel: Priority → colorScheme (Integer 1–10) ─────────────
            // sap.tnt.InfoLabel.colorScheme ist ein Integer (1–10), kein String!
            // → deshalb targetType: 'any' im Fragment-Binding notwendig.
            formatPriority: function (sPriority) {
                switch (sPriority) {
                    case "High":   return 2;   // blau
                    case "Medium": return 1;   // dunkelblau
                    case "Low":    return 8;   // grau
                    default:       return 0;
                }
            },

            // ── GenericTag: Category → ValueState ───────────────────────────
            // Rückgabe: "Success" | "None" | "Warning" | "Error" | "Information"
            formatCategoryStatus: function (sCategory) {
                switch (sCategory) {
                    case "Enterprise": return "Success";     // grün
                    case "Standard":   return "None";        // neutral
                    case "SMB":        return "Information"; // blau
                    default:           return "None";
                }
            }
        }
    );
});
```

---

### 17.7 manifest.json – Registrierung (3 Pflicht-Einträge)

Damit das Fragment und die Formatter funktionieren, müssen in `manifest.json` **drei Dinge** vorhanden sein:

#### 1. Library-Abhängigkeit

```json
"sap.ui5": {
  "dependencies": {
    "libs": {
      "sap.m": {},
      "sap.ui.core": {},
      "sap.fe.templates": {},
      "sap.suite.ui.commons": {}   ← ⚠ PFLICHT für MicroProcessFlow
    }
  }
}
```

#### 2. Controller Extension registrieren

```json
"sap.ui5": {
  "extends": {
    "extensions": {
      "sap.ui.controllerExtensions": {
        // Key-Format: "<FE-Controller>#<AppId>::<Target>"
        "sap.fe.templates.ListReport.ListReportController#com.example.fiorilistreport::ProductsList": {
          "controllerName": "com.example.fiorilistreport.ext.controller.SalesOrderListExt"
        }
      }
    }
  }
}
```

> **Key-Aufbau:**
> - `sap.fe.templates.ListReport.ListReportController` = FE V4 Standard-Controller der ListReport-Seite
> - `#com.example.fiorilistreport::ProductsList` = App-ID `::` Target-ID aus dem `targets`-Block
> - Ohne diese Registrierung → `.extension.<ns>.formatter` nicht auflösbar → **Werte bleiben leer**

#### 3. Library auch in ui5-local.yaml eintragen

```yaml
framework:
  name: SAPUI5
  version: 1.146.0
  libraries:
    - name: sap.m
    - name: sap.ui.core
    - name: sap.fe.templates
    - name: sap.suite.ui.commons    # ← PFLICHT für lokalen Dev-Server
```

---

### 17.8 Vollständiger Datenfluss

```
manifest.json
  ├── sap.ui.controllerExtensions
  │     └── registriert SalesOrderListExt als Extension des ListReportControllers
  └── sap.suite.ui.commons (Library)

                    ↓ FE V4 instanziiert die Extension

SalesOrderListExt.controller.js
  └── formatStepState(sStatus) → "Positive" | "Negative" | "Neutral" | "Planned"
  └── formatStepIcon(sStatus)  → "sap-icon://..." | ""

                    ↓ Fragment bindet Formatter via .extension.<ns>.method

MicroProcessFlow.fragment.xml
  └── content="{path: 'to_SalesOrderSteps'}"
        ↓ Navigation Property
        MicroProcessFlowItem
          state="{parts:[{path:'StepStatus'}], formatter:'.extension...'}"
          icon="{parts:[{path:'StepStatus'}],  formatter:'.extension...'}"

                    ↓ Navigation wird aufgelöst durch

metadata.xml
  └── SalesOrder.NavigationProperty → to_SalesOrderSteps → SalesOrderStepSet
  └── NavigationPropertyBinding     → Target="SalesOrderSteps"

                    ↓ Mock-Server liest

SalesOrderSteps.json   ← plain Array, Schlüssel: OrderID + StepID
```

---

### 17.9 Häufige Fehler

| Symptom | Ursache | Lösung |
|---|---|---|
| 4 leere Kreise, kein State/Icon | `controllerExtensions` nicht in `manifest.json` registriert | `sap.ui.controllerExtensions`-Block ergänzen |
| Formatter-Fehler in Konsole | Falscher Key in `controllerExtensions` (Target-ID stimmt nicht) | Target-ID mit `targets`-Block in manifest.json abgleichen |
| Navigation lädt keine Daten | `NavigationPropertyBinding` fehlt im EntityContainer | In `metadata.xml` ergänzen |
| `forEach is not a function` | Mock-JSON hat `{ "value": [...] }` Wrapper | Wrapper entfernen → plain Array `[...]` verwenden |
| `sap.suite.ui.commons` nicht gefunden | Library nicht in `manifest.json` und `ui5-local.yaml` | In beiden Dateien ergänzen |
| Schritte in falscher Reihenfolge | Kein Sorter oder falsches Feld | `sorter: [{ path: 'StepSortNumber' }]` im content-Binding |
| Leerer Kreis statt gefülltem | `state: "Planned"` korrekt — das ist der Soll-Zustand für offene Schritte | Kein Fehler — so ist es gewollt |

---

### 17.10 Rezept: MicroProcessFlow Custom Column in 10 Schritten

```
1. metadata.xml     → NavigationProperty + SalesOrderStep EntityType
2. metadata.xml     → NavigationPropertyBinding im EntityContainer
3. SalesOrderSteps.json → Mock-Daten mit StepStatus "01"-"04"
4. manifest.json    → sap.suite.ui.commons in libs
5. ui5-local.yaml   → sap.suite.ui.commons in libraries
6. manifest.json    → sap.ui.controllerExtensions registrieren
7. SalesOrderListExt.controller.js → ControllerExtension mit Formattern
8. MicroProcessFlow.fragment.xml   → Fragment mit Navigation Binding
9. manifest.json    → controlConfiguration → Custom Column eintragen
10. Dev-Server neu starten (Metadata-Änderungen werden gecacht!)
```

---

### 17.11 Custom Column: InfoLabel (`sap.tnt`)

> **Verwendung:** Zeigt einen kategorisierten Wert (z. B. Priorität) als farbiges Label.
> Die Farbe wird über `colorScheme` (Integer 1–10) gesteuert — **nicht über einen String**.

**Datei:** `webapp/ext/fragment/InfoLabel.fragment.xml`

```xml
<core:FragmentDefinition xmlns:tnt="sap.tnt" xmlns:core="sap.ui.core" xmlns="sap.m">
    <FlexBox id="flex1" direction="Row" wrap="Wrap"
             justifyContent="Start" alignItems="Start" fitContainer="true">
        <tnt:InfoLabel
            id="infoLabel1"
            text="{Priority}"
            visible="true"
            class="sapUiSmallMarginBottom sapUiSmallMarginEnd"
            colorScheme="{
                path: 'Priority',
                formatter: '.extension.com.example.fiorilistreport.ext.controller.SalesOrderListExt.formatPriority',
                targetType: 'any'
            }" />
    </FlexBox>
</core:FragmentDefinition>
```

**Kritischer Punkt: `targetType: 'any'`**

| Situation | Binding ohne `targetType` | Binding mit `targetType: 'any'` |
|---|---|---|
| Formatter gibt String zurück | ✅ funktioniert | ✅ funktioniert |
| Formatter gibt Integer zurück | ❌ wird zu String konvertiert → `colorScheme` zeigt Fehler | ✅ Integer bleibt Integer |
| Formatter gibt Object/Array zurück | ❌ Fehler | ✅ funktioniert |

> `targetType: 'any'` deaktiviert die automatische Typ-Konvertierung des Binding-Systems.
> **Immer verwenden**, wenn der Formatter einen nicht-String-Typ zurückgibt.

**Formatter in der Controller Extension:**
```javascript
// colorScheme: Integer 1–10
// Farb-Schema: 1=dunkelblau, 2=blau, 3=grün, 4=türkis, 5=indigo,
//              6=lila, 7=pink, 8=grau, 9=orange, 10=gelb
formatPriority: function (sPriority) {
    switch (sPriority) {
        case "High":   return 2;   // blau
        case "Medium": return 1;   // dunkelblau
        case "Low":    return 8;   // grau
        default:       return 0;   // kein Farbschema
    }
}
```

**`sap.tnt.InfoLabel` – wichtige Properties:**

| Property | Typ | Beschreibung |
|---|---|---|
| `text` | String | Anzeigetext |
| `colorScheme` | Integer (1–10) | Farbschema |
| `displayOnly` | Boolean | `true` = kein Hover/Click-Effekt |
| `renderMode` | `"Loose"` / `"Narrow"` | Kompakte vs. normale Darstellung |
| `icon` | sap-icon:// | Optionales Icon links vom Text |

---

### 17.12 Custom Column: GenericTag (`sap.m`)

> **Verwendung:** Zeigt einen Status-Tag mit optionalem Inhalt (z. B. Betrag).
> Kombiniert einen Text-Label mit einem `ValueState`-Indikator und einem Child-Control.

**Datei:** `webapp/ext/fragment/GenericTag.fragment.xml`

```xml
<core:FragmentDefinition xmlns:core="sap.ui.core" xmlns="sap.m">
    <FlexBox id="fb1" class="sapUiTinyMarginBottom" direction="Row"
             fitContainer="true" alignItems="Start" justifyContent="Start" wrap="Wrap">
        <GenericTag
            id="gnt1"
            text="{Category}"
            design="StatusIconHidden"
            status="{
                path: 'Category',
                formatter: '.extension.com.example.fiorilistreport.ext.controller.SalesOrderListExt.formatCategoryStatus'
            }"
            class="sapUiSmallMarginBottom sapUiSmallMarginEnd">
            <ObjectNumber
                id="ON1"
                state="Error"
                emphasized="true"
                number="{NetAmount}"
                unit="{Currency}"/>
        </GenericTag>
    </FlexBox>
</core:FragmentDefinition>
```

**Formatter in der Controller Extension:**
```javascript
// status: ValueState-String
formatCategoryStatus: function (sCategory) {
    switch (sCategory) {
        case "Enterprise": return "Success";     // grün
        case "Standard":   return "None";        // neutral
        case "SMB":        return "Information"; // blau
        default:           return "None";
    }
}
```

**`sap.m.GenericTag` – wichtige Properties:**

| Property | Typ | Beschreibung |
|---|---|---|
| `text` | String | Anzeigetext des Tags |
| `status` | `ValueState` | Statusfarbe (Success/Warning/Error/Information/None) |
| `design` | `"Full"` / `"StatusIconHidden"` | `StatusIconHidden` = kein Status-Icon, nur Farbe |
| Aggregation `value` | Control | Child-Control (z. B. `ObjectNumber`, `ObjectStatus`) |

> **Child-Control:** `sap.m.GenericTag` hat eine `value`-Aggregation (default aggregation).
> Dort kann ein einzelnes Control eingebettet werden — typisch `ObjectNumber` oder `ObjectStatus`.

---

### 17.13 Mehrere Custom Columns gleichzeitig (`controlConfiguration`)

Mehrere Custom Columns werden im `controlConfiguration`-Block der manifest.json eingetragen.
Jede Column bekommt einen **eindeutigen Key** (`"MicroProcessFlow"`, `"InfoLabel"`, `"GenericTag"`) und
referenziert ihr Fragment über den `template`-Modulnamen.

```json
"controlConfiguration": {
  "/SalesOrders/@com.sap.vocabularies.UI.v1.LineItem#TAB2": {
    "columns": {
      "MicroProcessFlow": {
        "header": "ProcessFlow",
        "position": { "anchor": "DataField::OrderDate", "placement": "After" },
        "template": "com.example.fiorilistreport.ext.fragment.MicroProcessFlow",
        "availability": "Default"
      },
      "InfoLabel": {
        "header": "Priority",
        "position": { "anchor": "MicroProcessFlow", "placement": "After" },
        "template": "com.example.fiorilistreport.ext.fragment.InfoLabel"
      },
      "GenericTag": {
        "header": "Category / Amount",
        "position": { "anchor": "InfoLabel", "placement": "After" },
        "template": "com.example.fiorilistreport.ext.fragment.GenericTag"
      }
    }
  }
}
```

**Erklärung der wichtigsten Felder:**

| Feld | Wert-Beispiel | Bedeutung |
|---|---|---|
| Outer key | `/SalesOrders/@...LineItem#TAB2` | EntitySet + Annotation-Pfad mit Qualifier — **führender `/` Pflicht!** |
| Column key | `"MicroProcessFlow"` | Eindeutiger interner Name — wird auch als `anchor` für andere Columns genutzt |
| `header` | `"ProcessFlow"` | Spaltenüberschrift |
| `position.anchor` | `"DataField::OrderDate"` | Spalte, an der relativ positioniert wird (`DataField::<PropertyName>` für Annotationsfelder) |
| `position.placement` | `"After"` / `"Before"` | Vor oder nach dem Anchor |
| `template` | `"...ext.fragment.MicroProcessFlow"` | UI5-Modulname → wird zu `webapp/ext/fragment/MicroProcessFlow.fragment.xml` aufgelöst |
| `availability` | `"Default"` / `"Hidden"` / `"Adaptation"` | `Default`=sichtbar, `Hidden`=ausgeblendet, `Adaptation`=nur per Personalisierung einblendbar |

**`template`-Auflösung (Punkte → Dateipfad):**
```
com.example.fiorilistreport . ext . fragment . MicroProcessFlow
↓ Namespace (app-root)       ↓   ↓           ↓ Dateiname
                            ext/ fragment/   MicroProcessFlow.fragment.xml
```

**Zusammenfassung: Welche Datei wurde wofür geändert**

| Datei | Änderung | Grund |
|---|---|---|
| `metadata.xml` | `NavigationProperty to_SalesOrderSteps` auf `SalesOrder` | Fragment-Binding `{path: 'to_SalesOrderSteps'}` braucht die NavProp |
| `metadata.xml` | `SalesOrderStep` EntityType neu | Zielobjekt der Navigation |
| `metadata.xml` | `NavigationPropertyBinding` im EntityContainer | Mock-Server braucht das für automatische Auflösung |
| `manifest.json` | `sap.suite.ui.commons` in `libs` | MicroProcessFlow, InfoLabel, GenericTag sind in dieser Library |
| `manifest.json` | `sap.ui.controllerExtensions` registriert | Ohne Registrierung → Formatter-Pfad `.extension.<ns>` nicht auflösbar |
| `manifest.json` | `controlConfiguration` mit 3 Custom Columns | Definiert welche Fragmente als Spalten eingebunden werden |
| `ui5-local.yaml` | `sap.suite.ui.commons` in `libraries` | Library muss auch dem lokalen Dev-Server bekannt sein |
| `SalesOrderListExt.controller.js` | Neu erstellt mit 5 Formatter-Methoden | Zentrale Logik für alle 3 Custom-Column-Fragmente |
| `MicroProcessFlow.fragment.xml` | Neu erstellt | Custom Column: Prozessschritte als Kreisfolge |
| `InfoLabel.fragment.xml` | Neu erstellt | Custom Column: Priority als farbiges Label |
| `GenericTag.fragment.xml` | Neu erstellt | Custom Column: Kategorie + Betrag als Tag |
| `SalesOrderSteps.json` | Neu erstellt | Mock-Daten für `to_SalesOrderSteps` Navigation |

---

## 19. Must-Have UI5 Controls – Referenz

> Kurz-Referenz der wichtigsten SAPUI5-Controls mit Link zur offiziellen Dokumentation.
> Alle Links führen auf [ui5.sap.com](https://ui5.sap.com/#/controls).

---

### 19.1 Display Controls

| Control | Library | Kurzbeschreibung | Link |
|---|---|---|---|
| **Text** | `sap.m` | Einfacher nicht-editierbarer Text. Unterstützt `wrapping`, `maxLines`. | [→](https://ui5.sap.com/#/api/sap.m.Text) |
| **Label** | `sap.m` | Beschriftung für Formularfelder. Verknüpfbar mit `labelFor`. | [→](https://ui5.sap.com/#/api/sap.m.Label) |
| **ObjectStatus** | `sap.m` | Text + Icon mit Ampelfarbe (`ValueState`). Ideal für Status-Anzeigen. | [→](https://ui5.sap.com/#/api/sap.m.ObjectStatus) |
| **ObjectNumber** | `sap.m` | Zahl mit Einheit und ValueState (z.B. Preis in Rot/Grün). | [→](https://ui5.sap.com/#/api/sap.m.ObjectNumber) |
| **ObjectIdentifier** | `sap.m` | Titel + Untertitel für Listeneinträge. Titelklick navigierbar. | [→](https://ui5.sap.com/#/api/sap.m.ObjectIdentifier) |
| **Avatar** | `sap.m` | Rundes Bild oder Initialen-Fallback für Personen/Entitäten. | [→](https://ui5.sap.com/#/api/sap.m.Avatar) |
| **Icon** | `sap.ui.core` | SAP-Icon aus dem Icon-Font (`sap-icon://...`). | [→](https://ui5.sap.com/#/api/sap.ui.core.Icon) |
| **FormattedText** | `sap.m` | HTML-Subset für formatierten Text (fett, Links, Listen). | [→](https://ui5.sap.com/#/api/sap.m.FormattedText) |
| **ProgressIndicator** | `sap.m` | Fortschrittsbalken 0–100% mit ValueState-Farbe. | [→](https://ui5.sap.com/#/api/sap.m.ProgressIndicator) |
| **RatingIndicator** | `sap.m` | Stern-Bewertung (read-only oder editierbar). | [→](https://ui5.sap.com/#/api/sap.m.RatingIndicator) |
| **GenericTag** | `sap.m` | Kompaktes Tag mit Text + optionalem Status/Icon + aggregiertem Control. | [→](https://ui5.sap.com/#/api/sap.m.GenericTag) |
| **InfoLabel** | `sap.tnt` | Farbiges Inline-Label (10 Farbschemas). Ideal für Kategorien/Status. | [→](https://ui5.sap.com/#/api/sap.tnt.InfoLabel) |
| **MicroProcessFlow** | `sap.suite.ui.commons` | Kompakte Kreisfolge für Prozessschritte. Bindet an Collection. | [→](https://ui5.sap.com/#/api/sap.suite.ui.commons.MicroProcessFlow) |
| **MicroProcessFlowItem** | `sap.suite.ui.commons` | Einzelner Schritt im MicroProcessFlow (state, icon, title, press). | [→](https://ui5.sap.com/#/api/sap.suite.ui.commons.MicroProcessFlowItem) |
| **ProcessFlow** | `sap.suite.ui.commons` | Vollständiges Prozessdiagramm (größer als MicroProcessFlow). | [→](https://ui5.sap.com/#/api/sap.suite.ui.commons.ProcessFlow) |

---

### 19.2 Input Controls

| Control | Library | Kurzbeschreibung | Link |
|---|---|---|---|
| **Input** | `sap.m` | Texteingabe mit optionalem Value Help (F4). | [→](https://ui5.sap.com/#/api/sap.m.Input) |
| **Select** | `sap.m` | Dropdown für feste Wertelisten. | [→](https://ui5.sap.com/#/api/sap.m.Select) |
| **ComboBox** | `sap.m` | Dropdown + freie Texteingabe kombiniert. | [→](https://ui5.sap.com/#/api/sap.m.ComboBox) |
| **MultiComboBox** | `sap.m` | Mehrfachauswahl aus Dropdown. | [→](https://ui5.sap.com/#/api/sap.m.MultiComboBox) |
| **DatePicker** | `sap.m` | Datumseingabe mit Kalender-Popup. | [→](https://ui5.sap.com/#/api/sap.m.DatePicker) |
| **DateRangePicker** | `sap.m` | Datumsbereich (Von–Bis) mit Kalender. | [→](https://ui5.sap.com/#/api/sap.m.DateRangePicker) |
| **CheckBox** | `sap.m` | Einzelne Checkbox mit optionalem Label. | [→](https://ui5.sap.com/#/api/sap.m.CheckBox) |
| **Switch** | `sap.m` | Toggle On/Off (wie iOS-Switch). | [→](https://ui5.sap.com/#/api/sap.m.Switch) |
| **Slider** | `sap.m` | Numerischer Wertschieber mit Min/Max. | [→](https://ui5.sap.com/#/api/sap.m.Slider) |
| **SearchField** | `sap.m` | Sucheingabe mit Lupe und Clear-Button. | [→](https://ui5.sap.com/#/api/sap.m.SearchField) |
| **TextArea** | `sap.m` | Mehrzeiliges Textfeld. | [→](https://ui5.sap.com/#/api/sap.m.TextArea) |
| **FileUploader** | `sap.ui.unified` | Datei-Upload-Button mit Dateinamen-Anzeige. | [→](https://ui5.sap.com/#/api/sap.ui.unified.FileUploader) |

---

### 19.3 Layout Controls

| Control | Library | Kurzbeschreibung | Link |
|---|---|---|---|
| **FlexBox** | `sap.m` | CSS-Flexbox-Container (direction, wrap, justify, align). | [→](https://ui5.sap.com/#/api/sap.m.FlexBox) |
| **VBox** | `sap.m` | Vertikaler FlexBox-Shortcut (`direction="Column"`). | [→](https://ui5.sap.com/#/api/sap.m.VBox) |
| **HBox** | `sap.m` | Horizontaler FlexBox-Shortcut (`direction="Row"`). | [→](https://ui5.sap.com/#/api/sap.m.HBox) |
| **Grid** | `sap.ui.layout` | 12-Spalten-Grid mit responsive Breakpoints. | [→](https://ui5.sap.com/#/api/sap.ui.layout.Grid) |
| **SimpleForm** | `sap.ui.layout.form` | Einfaches zweispaltiges Formular-Layout. | [→](https://ui5.sap.com/#/api/sap.ui.layout.form.SimpleForm) |
| **Panel** | `sap.m` | Zusammenklappbarer Inhaltsbereich mit Titelzeile. | [→](https://ui5.sap.com/#/api/sap.m.Panel) |
| **ScrollContainer** | `sap.m` | Scrollbarer Bereich mit fixer Höhe. | [→](https://ui5.sap.com/#/api/sap.m.ScrollContainer) |
| **Splitter** | `sap.ui.layout` | Verschiebbarer Teiler zwischen zwei Bereichen. | [→](https://ui5.sap.com/#/api/sap.ui.layout.Splitter) |
| **Toolbar** | `sap.m` | Horizontale Leiste für Buttons, Spacer, Texte. | [→](https://ui5.sap.com/#/api/sap.m.Toolbar) |
| **OverflowToolbar** | `sap.m` | Toolbar mit automatischem Overflow-Menü bei wenig Platz. | [→](https://ui5.sap.com/#/api/sap.m.OverflowToolbar) |

---

### 19.4 Navigation & Feedback Controls

| Control | Library | Kurzbeschreibung | Link |
|---|---|---|---|
| **Button** | `sap.m` | Schaltfläche (Emphasized, Default, Transparent, Destructive). | [→](https://ui5.sap.com/#/api/sap.m.Button) |
| **SegmentedButton** | `sap.m` | Gruppe von Buttons als Einfachauswahl (Radio-Style). | [→](https://ui5.sap.com/#/api/sap.m.SegmentedButton) |
| **Link** | `sap.m` | Anklickbarer Text-Link mit `href` oder `press`-Handler. | [→](https://ui5.sap.com/#/api/sap.m.Link) |
| **MessageToast** | `sap.m` | Kurze Benachrichtigung die automatisch verschwindet. | [→](https://ui5.sap.com/#/api/sap.m.MessageToast) |
| **MessageBox** | `sap.m` | Modaler Dialog für Bestätigungen, Fehler, Warnungen. | [→](https://ui5.sap.com/#/api/sap.m.MessageBox) |
| **Dialog** | `sap.m` | Vollständig anpassbarer modaler Dialog. | [→](https://ui5.sap.com/#/api/sap.m.Dialog) |
| **Popover** | `sap.m` | Kontextmenü/Overlay das an einem Control andockt. | [→](https://ui5.sap.com/#/api/sap.m.Popover) |
| **BusyIndicator** | `sap.m` | Lade-Spinner (global oder lokal). | [→](https://ui5.sap.com/#/api/sap.m.BusyIndicator) |
| **List** | `sap.m` | Einfache vertikale Liste mit verschiedenen Item-Typen. | [→](https://ui5.sap.com/#/api/sap.m.List) |
| **Table** | `sap.m` | Responsive Tabelle (für Custom Views, nicht FE-generiert). | [→](https://ui5.sap.com/#/api/sap.m.Table) |
| **IconTabBar** | `sap.m` | Tab-Leiste mit Icons (als Navigations- oder Filter-Tabs). | [→](https://ui5.sap.com/#/api/sap.m.IconTabBar) |
| **StepInput** | `sap.m` | Zahleneingabe mit +/- Buttons. | [→](https://ui5.sap.com/#/api/sap.m.StepInput) |

---

### 19.5 sap.suite.ui.commons Controls

> Zusätzliche Library — muss in `manifest.json` + `ui5-local.yaml` eingetragen werden.

| Control | Kurzbeschreibung | Link |
|---|---|---|
| **MicroProcessFlow** | Kompakte horizontale Prozessschritte als Kreisfolge. State: Success/Warning/Information/None. | [→](https://ui5.sap.com/#/api/sap.suite.ui.commons.MicroProcessFlow) |
| **MicroProcessFlowItem** | Einzelner Schritt: `state`, `icon`, `title`, `press`, `stepWidth`, `showSeparator`. | [→](https://ui5.sap.com/#/api/sap.suite.ui.commons.MicroProcessFlowItem) |
| **ProcessFlow** | Vollständiges mehrstufiges Prozessfluss-Diagramm mit Lanes und Nodes. | [→](https://ui5.sap.com/#/api/sap.suite.ui.commons.ProcessFlow) |
| **Timeline** | Zeitachse mit Events (Datum + Icon + Text). Ideal für Änderungshistorie. | [→](https://ui5.sap.com/#/api/sap.suite.ui.commons.Timeline) |
| **NetworkGraph** | Interaktiver Netzwerkgraph für Abhängigkeiten/Beziehungen. | [→](https://ui5.sap.com/#/api/sap.suite.ui.commons.networkgraph.Graph) |
| **BulletMicroChart** | Mini-Balkendiagramm (Zielwert vs. Istwert) für kompakte Darstellung. | [→](https://ui5.sap.com/#/api/sap.suite.ui.microchart.BulletMicroChart) |
| **RadialMicroChart** | Kleines Kreisdiagramm für Prozentwerte (z.B. in Tabellenspalten). | [→](https://ui5.sap.com/#/api/sap.suite.ui.microchart.RadialMicroChart) |
| **HarveyBallMicroChart** | Gefüllter Kreis für Auslastung/Kapazität (0–100%). | [→](https://ui5.sap.com/#/api/sap.suite.ui.microchart.HarveyBallMicroChart) |

**Library in manifest.json einbinden:**
```json
"sap.ui5": {
  "dependencies": {
    "libs": {
      "sap.suite.ui.commons": {}
    }
  }
}
```

**Library in ui5-local.yaml einbinden:**
```yaml
libraries:
  - name: sap.suite.ui.commons
```

**Namespace im Fragment:**
```xml
<!-- Option A: Default-Namespace -->
<core:FragmentDefinition xmlns="sap.suite.ui.commons" xmlns:core="sap.ui.core">
    <MicroProcessFlow ...>

<!-- Option B: Präfix -->
<core:FragmentDefinition xmlns:suite="sap.suite.ui.commons" xmlns:core="sap.ui.core">
    <suite:MicroProcessFlow ...>
```

