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

