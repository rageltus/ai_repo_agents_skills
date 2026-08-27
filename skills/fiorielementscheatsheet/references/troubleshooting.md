## 15. Häufige Fehler & Troubleshooting

### Chart bleibt leer / zeigt keine Daten

```
  Problem:  Chart wird gerendert, aber keine Balken/Segmente
  Ursache:  Aggregation.ApplySupported fehlt am EntitySet
  Lösung:   In metadata.xml hinzufügen:

  <Annotations Target="MyService.MainContainer/SalesOrders">
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

### DataPoint (Progress / Rating) wird nicht als Micro-Chart angezeigt

```
  Problem:  Spalte zeigt nur den Rohwert (Zahl), nicht den Balken/Sterne
  Ursache 1: UI.DataFieldForAnnotation fehlt im UI.LineItem — statt dessen
             wurde UI.DataField mit Path auf das Feld verwendet
  Ursache 2: Target-Qualifier stimmt nicht überein

  Falsch:
  <Record Type="UI.DataField">
    <PropertyValue Property="Value" Path="Stock" />    ← zeigt nur Zahl
  </Record>

  Richtig:
  <Record Type="UI.DataFieldForAnnotation">
    <PropertyValue Property="Target" AnnotationPath="@UI.DataPoint#StockDataPoint" />
  </Record>
```

### F4-Hilfe (Lupe) erscheint nicht im Filterfeld

```
  Problem:  Filterfeld hat kein F4-Icon
  Ursache:  Common.ValueList ist nicht am Property-Target annotiert,
            sondern fälschlicherweise am EntityType-Target

  Falsch:
  <Annotations Target="MyService.Product">        ← EntityType
    <Annotation Term="Common.ValueList"> ... </Annotation>
  </Annotations>

  Richtig:
  <Annotations Target="MyService.Product/Category">  ← Property
    <Annotation Term="Common.ValueList"> ... </Annotation>
  </Annotations>
```

### ObjectPage zeigt keine Sections / ist leer

```
  Problem:  ObjectPage lädt, aber keine Abschnitte sichtbar
  Ursache:  UI.Facets fehlt oder zeigt auf falsche AnnotationPath

  Checkliste:
  ✅ UI.Facets ist an MyService.Product annotiert (EntityType)
  ✅ ReferenceFacet.Target zeigt auf @UI.FieldGroup#GeneralData
  ✅ UI.FieldGroup Qualifier="GeneralData" existiert am gleichen EntityType
  ✅ manifest.json hat eine ObjectPage-Route mit dem richtigen EntitySet
```

### Tab erscheint nicht im ListReport

```
  Problem:  Nur ein Tab sichtbar (oder keine Tabs)
  Ursache:  manifest.json views.paths nicht konfiguriert

  Checkliste:
  ✅ manifest.json: "views" mit "paths"-Array vorhanden
  ✅ annotationPath zeigt auf den korrekten Qualifier:
     "com.sap.vocabularies.UI.v1.SelectionPresentationVariant#VAR1"
  ✅ UI.SelectionPresentationVariant#VAR1 in annotation.xml vorhanden
  ✅ Die referenzierte @UI.LineItem#TAB1 Annotation existiert
```

### i18n-Binding funktioniert nicht (OVP)

```
  Problem:  String "{@i18n>myKey}" wird literal angezeigt statt übersetzt
  Ursache:  i18n-Modell nicht registriert in manifest.json

  Lösung: In manifest.json unter sap.app/i18n oder sap.ui5/models:
  "i18n": {
    "type": "sap.ui.model.resource.ResourceModel",
    "settings": { "bundleName": "com.example.fioriovp.i18n.i18n" }
  }

  Hinweis: Das @-Zeichen vor i18n ("{@i18n>key}") ist OVP-spezifisch.
  In Standard Fiori Elements annotation.xml ist i18n-Binding NICHT möglich.
  Dort: Common.Label String="Fester Text" oder Übersetzungskeys in der
  i18n.properties der App.
```

### Common.Text zeigt nichts / Navigation-Pfad schlägt fehl

```
  Problem:  Feld zeigt weiterhin den Code statt des Textes
  Ursache:  NavigationProperty "to_Status" fehlt in metadata.xml

  Checkliste:
  ✅ <NavigationProperty Name="to_Status" Type="MyService.StatusText" />
     muss im EntityType definiert sein
  ✅ <Association .../> und <AssociationSet .../> vorhanden (V2)
     ODER <NavigationPropertyBinding .../> im EntitySet vorhanden (V4)
  ✅ Im Mock-Betrieb: $expand wird vom Mockserver unterstützt
```

### Annotation wird ignoriert / hat keine Wirkung

```
  Häufige Ursachen:

  ① Falscher Namespace-Alias:
     Falsch:  Term="SAP.UI.LineItem"  (großgeschrieben)
     Richtig: Term="UI.LineItem"

  ② Qualifier-Tippfehler:
     Annotation: Qualifier="GeneralData"
     Referenz:   AnnotationPath="@UI.FieldGroup#GeneralDate"  ← Tippfehler!

  ③ Aggregation-Annotation in annotation.xml statt metadata.xml:
     Aggregation.ApplySupported MUSS in metadata.xml stehen

  ④ EntityType-Namespace fehlt:
     Falsch:  Target="Product"           ← kein Namespace
     Richtig: Target="MainService.Product"
```

### manifest.json – Häufige Konfigurationsfehler

```
  Problem:  App startet nicht / zeigt "Component could not be loaded"
  Ursache:  sap.app/id stimmt nicht mit Dateipfad überein
  Lösung:   sap.app/id muss dem Namespace-Pfad entsprechen:
            id: "com.example.myapp.myapp" → Dateien unter webapp/

  Problem:  OData-Service wird nicht geladen (503 Service Unavailable)
  Ursache:  dataSource.uri zeigt auf falsche URL
  Lösung:   Bei Mock-Betrieb: uri muss mit urlPath in ui5-mock.yaml übereinstimmen

  Problem:  Annotationen werden nicht angewendet
  Ursache:  ODataAnnotation-DataSource nicht in der Service-dataSource referenziert
  Lösung:   In mainService.settings.annotations: ["annotation"] eintragen

  Problem:  i18n-Texte werden nicht aufgelöst
  Ursache:  bundleName falsch oder Datei nicht vorhanden
  Lösung:   "bundleName": "<appId>.i18n.i18n" → webapp/i18n/i18n.properties
```

### Fiori Elements Page Map / Joule – Annotation-Konflikte

```
  Problem:  Page Map erstellt Annotationen, die mit bestehenden kollidieren
  Ursache:  Page Map schreibt in annotation.xml — doppelte Terme ohne Qualifier
  Lösung:   Page Map nur für neue Annotationen nutzen; bestehende manuell pflegen

  Problem:  OVP-App zeigt Karte nicht obwohl manifest.json korrekt
  Ursache:  OVP-App benötigt sap.ovp Dependency
  Lösung:   In sap.ui5.dependencies.libs:
            "sap.ovp": {}  hinzufügen
```

### SelectionVariant / Filter-Chips erscheinen nicht

```
  Problem:  Filter-Chips werden nicht über der Tabelle angezeigt
  Ursache:  UI.SelectionVariant vorhanden, aber kein "All"-Chip (Standard-Chip fehlt)
  Lösung:   Mindestens einen UI.SelectionVariant ohne SelectOptions als "All"-Chip definieren

  Problem:  Chip-Klick hat keine Wirkung (Daten bleiben gleich)
  Ursache:  FilterRestrictions verhindern den Filter auf diesem Feld
  Lösung:   In metadata.xml FilterRestrictions prüfen oder entfernen
```

### Fehler nach Umstieg von Mock auf echten Service

```
  Problem:  Tabelle zeigt "No Data" obwohl im Backend Daten vorhanden
  Ursache:  manifest.json uri zeigt noch auf Mock-URL (z.B. relativer Pfad)
  Lösung:   dataSources.mainService.uri auf echte Backend-URL setzen
            ui5-local.yaml mit fiori-tools-proxy konfigurieren

  Problem:  CORS-Fehler in der Browser-Konsole (blocked by CORS policy)
  Ursache:  Browser blockiert direkte Anfragen von localhost an SAP-Host
  Lösung:   fiori-tools-proxy in ui5-local.yaml einrichten — Anfragen laufen
            dann über den lokalen Node.js-Server, kein direkter Browser-Zugriff

  Problem:  401 Unauthorized
  Ursache:  Authentifizierung nicht konfiguriert
  Lösung:   Credentials in .env setzen (HTTP_PROXY_USER / HTTP_PROXY_PASSWORD)
            oder SAP-SSO / SAML korrekt konfigurieren

  Problem:  Alle Annotationen werden ignoriert (App zeigt nur rohe Felder)
  Ursache:  Target-Namespace in annotation.xml stimmt nicht mit realem Namespace
            aus metadata.xml überein
  Lösung:   Schema Namespace="..." aus realer metadata.xml ablesen und alle
            Target="<Namespace>.<EntityType>" in annotation.xml anpassen

  Problem:  Chart bleibt leer obwohl im Mock funktioniert
  Ursache:  Aggregation.ApplySupported fehlt im echten Backend-Service
  Lösung:   In CDS-View @Aggregation.default oder @Analytics.query: true setzen
            Prüfen: GET /EntitySet?$apply=... → muss 200 zurückgeben

  Problem:  Common.Text zeigt Code statt Text
  Ursache:  NavigationProperty existiert nicht im echten Backend-Schema
  Lösung:   In realer metadata.xml prüfen ob NavigationProperty vorhanden ist;
            ggf. Backend-Team bitten Association zu deklarieren

  Problem:  F4-ValueList öffnet sich, aber ist leer
  Ursache:  CollectionPath-EntitySet existiert nicht im echten Service
  Lösung:   Backend-Team bitten ValueList-Entity im Service zu exponieren
            (z.B. als @ObjectModel.representativeKey annotiertes CDS-View)

  Problem:  OData-Action gibt 404 zurück
  Ursache:  Action ist nicht im echten Service implementiert
  Lösung:   In realer metadata.xml prüfen ob <Action Name="..."> vorhanden;
            im Mock-Betrieb hat mockRequests-Handler den Fehler verdeckt
```

### OVP Global Filter funktioniert nicht

```
  Problem:  Änderung im Global Filter hat keinen Effekt auf OVP-Karten
  Ursache:  Karte verwendet eigenes Model (nicht "mainService")
  Lösung:   In manifest.json: "model": "mainService" in der Karten-Konfiguration setzen

  Problem:  SelectionFields für Global Filter werden nicht angezeigt
  Ursache:  UI.SelectionFields ist am falschen EntityType (nicht dem Global Filter Entity)
  Lösung:   globalFilterEntityType in manifest.json prüfen und SelectionFields
            am gleichen EntityType annotieren
```

---

