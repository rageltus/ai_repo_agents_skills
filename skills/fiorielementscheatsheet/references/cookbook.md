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

