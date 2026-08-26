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

