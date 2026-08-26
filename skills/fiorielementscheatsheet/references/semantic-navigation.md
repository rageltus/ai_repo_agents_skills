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

