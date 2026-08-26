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

