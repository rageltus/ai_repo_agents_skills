# UI Vocabulary – SAP Fiori Elements Annotationen

> Teil des [Fiori Elements Cheatsheets](../SKILL.md). Vollständige Referenz für alle `com.sap.vocabularies.UI.v1`-Annotationen: XML-Syntax, CDS-Äquivalent, Properties und typische Fehler je Annotation.

## Inhalt

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

