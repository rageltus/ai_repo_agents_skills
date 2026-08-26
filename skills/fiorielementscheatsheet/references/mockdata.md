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

