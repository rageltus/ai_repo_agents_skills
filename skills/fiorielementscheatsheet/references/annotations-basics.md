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

