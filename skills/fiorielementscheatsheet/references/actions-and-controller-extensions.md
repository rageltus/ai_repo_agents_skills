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

