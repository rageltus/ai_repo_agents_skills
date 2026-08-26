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

