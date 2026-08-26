## 18. OData Datenmodell – Grundlagen Cheatsheet

> Kurz-Referenz für `metadata.xml`: EntityTypes, Properties, NavigationProperties, Associations.
> Gilt sowohl für **OData V4** (FE V4 Apps) als auch **OData V2** (Smart Templates).

---

### 18.1 EntityType – die Datentabelle

Ein `EntityType` entspricht konzeptionell einer **Datenbanktabelle** oder einem **CDS View**.
Er definiert welche Felder ein Datenobjekt hat.

```xml
<EntityType Name="SalesOrder">
    <Key>
        <PropertyRef Name="OrderID"/>   <!-- Primärschlüssel -->
    </Key>
    <Property Name="OrderID"      Type="Edm.String"  Nullable="false" MaxLength="10"/>
    <Property Name="CustomerName" Type="Edm.String"  MaxLength="80"/>
    <Property Name="NetAmount"    Type="Edm.Decimal" Precision="13" Scale="2"/>
    <Property Name="OrderDate"    Type="Edm.DateTimeOffset"/>
</EntityType>
```

**Regeln:**
- Jeder EntityType braucht mindestens einen `<Key>`
- `Nullable="false"` = Pflichtfeld (entspricht `NOT NULL` in SQL)
- Der Name endet in V2 oft auf `...Type` (z.B. `SalesOrderType`), in V4 oft ohne Suffix

---

### 18.2 EntitySet – der Endpunkt

Ein `EntitySet` ist die **URL-Ressource** — der eigentliche OData-Endpunkt für eine Liste von Objekten.
Jeder EntityType braucht genau einen EntitySet (kann aber mehrere haben).

```xml
<EntityContainer Name="MainContainer">
    <EntitySet Name="SalesOrders" EntityType="MainService.SalesOrder"/>
    <!--               ↑ URL-Pfad        ↑ Namespace.EntityType-Name -->
</EntityContainer>
```

**URL-Zugriff:**
```
GET /SalesOrders          → Liste aller SalesOrders
GET /SalesOrders('ORD001') → Einzelner Datensatz
GET /SalesOrders?$top=10  → Mit OData-Parametern
```

**EntityType vs. EntitySet:**

| | EntityType | EntitySet |
|---|---|---|
| Entspricht | Tabellendefinition (Schema) | Tabelle selbst (Instanz) |
| Definiert | Felder und Typen | URL-Endpunkt |
| Analogie | `CREATE TABLE` | `SELECT * FROM` |
| Ort | `<Schema>` | `<EntityContainer>` |

---

### 18.3 Key & Schlüsseleigenschaften

```xml
<!-- Einfacher Schlüssel: -->
<EntityType Name="Product">
    <Key>
        <PropertyRef Name="ProductID"/>
    </Key>
    <Property Name="ProductID" Type="Edm.String" Nullable="false"/>
</EntityType>

<!-- Zusammengesetzter Schlüssel (z.B. bei Navigation-Entities): -->
<EntityType Name="SalesOrderStep">
    <Key>
        <PropertyRef Name="OrderID"/>    <!-- Fremdschlüssel -->
        <PropertyRef Name="StepID"/>     <!-- Eigenständiger Schlüssel -->
    </Key>
    <Property Name="OrderID" Type="Edm.String" Nullable="false" MaxLength="10"/>
    <Property Name="StepID"  Type="Edm.String" Nullable="false" MaxLength="4"/>
</EntityType>
```

> **Faustregel:** Jede Tabelle, die über eine Navigation Property erreichbar ist (1:n),
> braucht den Fremdschlüssel der Elterntabelle **im Key**.

---

### 18.4 Property-Typen Referenz

| OData Typ | Entspricht | Beispiel-Wert (JSON) | Beispiel (XML) |
|---|---|---|---|
| `Edm.String` | VARCHAR | `"ORD001"` | `Type="Edm.String" MaxLength="10"` |
| `Edm.Int32` | INTEGER | `42` | `Type="Edm.Int32"` |
| `Edm.Int64` | BIGINT | `9999999999` | `Type="Edm.Int64"` |
| `Edm.Decimal` | DECIMAL | `19.99` | `Type="Edm.Decimal" Precision="13" Scale="2"` |
| `Edm.Boolean` | BOOLEAN | `true` | `Type="Edm.Boolean"` |
| `Edm.DateTime` | DATETIME (V2) | `"/Date(1711670400000)/"` | `Type="Edm.DateTime"` |
| `Edm.DateTimeOffset` | TIMESTAMP (V4) | `"2024-03-29T00:00:00Z"` | `Type="Edm.DateTimeOffset"` |
| `Edm.Date` | DATE (V4) | `"2024-03-29"` | `Type="Edm.Date"` |
| `Edm.TimeOfDay` | TIME (V4) | `"14:30:00"` | `Type="Edm.TimeOfDay"` |
| `Edm.Guid` | GUID/UUID | `"550e8400-e29b-41d4-a716..."` | `Type="Edm.Guid"` |
| `Edm.Byte` | TINYINT (0-255) | `7` | `Type="Edm.Byte"` |

> **V2 vs. V4 Datum:**
> - V2: `Edm.DateTime` → JSON-Wert: `/Date(1711670400000)/` (Millisekunden seit 1970)
> - V4: `Edm.DateTimeOffset` → JSON-Wert: `2024-03-29T00:00:00Z` (ISO 8601)

---

### 18.5 NavigationProperty – Beziehungen zwischen Entities

Eine `NavigationProperty` ist der **Fremdschlüssel-Zeiger** im OData-Modell.
Sie ermöglicht `$expand` und Navigation-Bindings in Fragmenten (`{path: 'to_Steps'}`).

**1:1 Beziehung** (z.B. SalesOrder → Kunde):
```xml
<NavigationProperty
    Name="to_Customer"
    Type="MainService.Customer"          <!-- kein Collection() -->
    Partner="to_Orders"/>               <!-- Rück-Navigation -->
```

**1:n Beziehung** (z.B. SalesOrder → Steps):
```xml
<NavigationProperty
    Name="to_SalesOrderSteps"
    Type="Collection(MainService.SalesOrderStep)"   <!-- Collection() = 1:n -->
    Partner="to_SalesOrder"/>                       <!-- Rück-Navigation -->
```

**Rück-Navigation** (auf der n-Seite):
```xml
<!-- Im SalesOrderStep EntityType: -->
<NavigationProperty
    Name="to_SalesOrder"
    Type="MainService.SalesOrder"        <!-- kein Collection() = zurück zur 1-Seite -->
    Partner="to_SalesOrderSteps"/>
```

**Naming-Konvention:**
- Präfix `to_` kennzeichnet Navigation Properties (SAP-Standard)
- Name beschreibt das Ziel, nicht die Quelle: `to_SalesOrderSteps`, `to_Customer`, `to_StatusVH`

---

### 18.6 V4 vs. V2: NavigationProperty Syntax

**OData V4** (FE V4 Apps — `edmx Version="4.0"`):

```xml
<!-- V4: NavigationProperty direkt im EntityType, kein Association nötig -->
<EntityType Name="SalesOrder">
    <NavigationProperty
        Name="to_SalesOrderSteps"
        Type="Collection(MainService.SalesOrderStep)"
        Partner="to_SalesOrder"/>
</EntityType>

<!-- V4: NavigationPropertyBinding im EntitySet -->
<EntitySet Name="SalesOrders" EntityType="MainService.SalesOrder">
    <NavigationPropertyBinding Path="to_SalesOrderSteps" Target="SalesOrderSteps"/>
</EntitySet>
```

**OData V2** (Smart Templates — `edmx Version="1.0"`, `m:DataServiceVersion="2.0"`):

```xml
<!-- V2: NavigationProperty referenziert eine Association -->
<EntityType Name="SalesOrderType">
    <NavigationProperty
        Name="to_SalesOrderSteps"
        Relationship="Z_SRV.assoc_Order_Steps"   <!-- Name der Association -->
        FromRole="FromRole_Order"
        ToRole="ToRole_Steps"/>
</EntityType>

<!-- V2: Association MUSS separat definiert werden (Pflicht!) -->
<Association Name="assoc_Order_Steps">
    <End Type="Z_SRV.SalesOrderType"     Multiplicity="1"  Role="FromRole_Order"/>
    <End Type="Z_SRV.SalesOrderStepType" Multiplicity="*"  Role="ToRole_Steps"/>
    <ReferentialConstraint>
        <Principal Role="FromRole_Order">
            <PropertyRef Name="OrderID"/>
        </Principal>
        <Dependent Role="ToRole_Steps">
            <PropertyRef Name="OrderID"/>
        </Dependent>
    </ReferentialConstraint>
</Association>

<!-- V2: AssociationSet im EntityContainer -->
<AssociationSet Name="assoc_Order_Steps_Set"
    Association="Z_SRV.assoc_Order_Steps">
    <End EntitySet="SalesOrderSet"      Role="FromRole_Order"/>
    <End EntitySet="SalesOrderStepSet"  Role="ToRole_Steps"/>
</AssociationSet>
```

**Vergleich V4 vs. V2:**

| | OData V4 | OData V2 |
|---|---|---|
| Navigation definiert in | `NavigationProperty` direkt | `NavigationProperty` + `Association` + `AssociationSet` |
| Pflicht-Elemente | 2 (NavProp + Binding) | 4 (NavProp + Assoc + AssocSet + Binding) |
| Binding-Element | `NavigationPropertyBinding` | `AssociationSet` (+ optional Binding) |
| Komplexität | Einfach | Aufwändig |

---

### 18.7 EntityContainer – Alles zusammen

Der `EntityContainer` ist der **Einstiegspunkt** des OData-Service.
Alle EntitySets, FunctionImports und AssociationSets leben hier.

```xml
<EntityContainer Name="MainContainer"
    m:IsDefaultEntityContainer="true">   <!-- V2: Pflicht! V4: optional -->

    <!-- EntitySets: die URL-Endpunkte -->
    <EntitySet Name="SalesOrders"     EntityType="MainService.SalesOrder">
        <NavigationPropertyBinding Path="to_SalesOrderSteps" Target="SalesOrderSteps"/>
    </EntitySet>
    <EntitySet Name="SalesOrderSteps" EntityType="MainService.SalesOrderStep">
        <NavigationPropertyBinding Path="to_SalesOrder" Target="SalesOrders"/>
    </EntitySet>

    <!-- FunctionImports (V2/V4): Backend-Aktionen -->
    <FunctionImport Name="ConfirmOrder"
        ReturnType="MainService.SalesOrder"
        m:HttpMethod="POST">
        <Parameter Name="OrderID" Type="Edm.String" Mode="In"/>
    </FunctionImport>

</EntityContainer>
```

---

### 18.8 Kardinalitäten (Multiplicity)

Kardinalitäten beschreiben **wie viele** Objekte auf jeder Seite einer Beziehung stehen können.

| Symbol | Bedeutung | Beispiel |
|---|---|---|
| `1` | Genau eines | Eine Bestellung hat genau einen Kunden |
| `0..1` | Null oder eines | Eine Bestellung kann (muss nicht) eine Lieferadresse haben |
| `*` | Null oder viele | Eine Bestellung hat null bis viele Positionen |

**Typische Muster:**

```
1 ──── * :  Eltern-Kind   (SalesOrder → SalesOrderStep)
1 ──── 0..1: Optional 1:1 (SalesOrder → DraftAdminData)
* ──── 1 :  Lookup/VH    (SalesOrder → StatusVH)
* ──── * :  Selten        (eher vermeiden in OData)
```

In V4 erkennt man die Kardinalität am `Type`:
- `Type="Collection(...)"` → n-Seite (viele)
- `Type="MainService.SalesOrder"` → 1-Seite (genau eines)

---

### 18.9 SAP-spezifische Attribute (V2)

In OData V2 verwendet SAP den Namespace `sap:` für UI-Metadaten direkt in der Metadata.

```xml
<EntityType Name="SalesOrderType"
    sap:label="Sales Order"          <!-- Anzeigename in der UI -->
    sap:content-version="1">         <!-- Interne SAP-Versionsangabe -->

    <Property Name="OrderID"
        Type="Edm.String"
        sap:label="Order ID"          <!-- Spaltenüberschrift -->
        sap:creatable="false"         <!-- Kann nicht erstellt werden -->
        sap:updatable="false"         <!-- Kann nicht geändert werden -->
        sap:sortable="true"           <!-- Sortierbar in der Tabelle -->
        sap:filterable="true"         <!-- Filterbar in der Filterbar -->
        sap:searchable="true"         <!-- In der Freitextsuche -->
        sap:display-format="NonNegative"  <!-- Nur positive Zahlen -->
        sap:text="OrderDescription"   <!-- Textwert aus anderem Feld -->
        sap:unit="CurrencyCode"       <!-- Einheit/Währung aus anderem Feld -->
        sap:value-list="standard"     <!-- F4-Hilfe vorhanden -->
        sap:semantics="currency-code"/><!-- Ist ein Währungscode -->

</EntityType>

<EntitySet Name="SalesOrderSet"
    sap:searchable="true"            <!-- Volltextsuche erlaubt -->
    sap:creatable="true"             <!-- POST erlaubt -->
    sap:updatable="true"             <!-- PATCH/PUT erlaubt -->
    sap:deletable="false"/>          <!-- DELETE nicht erlaubt -->
```

**Häufig genutzte `sap:` Attribute:**

| Attribut | Wo | Bedeutung |
|---|---|---|
| `sap:label` | Property, EntityType | Anzeigename |
| `sap:text` | Property | Anderes Feld liefert den Anzeigetext (z.B. `StatusText` für `Status`) |
| `sap:unit` | Property | Anderes Feld liefert die Einheit (z.B. `CurrencyCode`) |
| `sap:value-list` | Property | F4-Werthilfe vorhanden (`"standard"` oder `"fixed-values"`) |
| `sap:semantics` | Property | `"currency-code"`, `"unit-of-measure"`, `"email"`, `"tel"` |
| `sap:creatable` | Property, EntitySet | Kann beim POST mitgesendet werden |
| `sap:updatable` | Property, EntitySet | Kann geändert werden |
| `sap:filterable` | Property | Erscheint in der Filterleiste |
| `sap:sortable` | Property | Spalte sortierbar |
| `sap:display-format` | Property | `"Date"`, `"NonNegative"`, `"UpperCase"` |

---

### 18.10 Schnell-Referenz: metadata.xml Skelett

**OData V4 (FE V4 Apps):**

```xml
<?xml version="1.0" encoding="utf-8"?>
<edmx:Edmx Version="4.0" xmlns:edmx="http://docs.oasis-open.org/odata/ns/edmx">
    <edmx:DataServices>
        <Schema Namespace="MyService" xmlns="http://docs.oasis-open.org/odata/ns/edm">

            <!-- 1. EntityType(s) -->
            <EntityType Name="MyEntity">
                <Key><PropertyRef Name="ID"/></Key>
                <Property Name="ID"   Type="Edm.String" Nullable="false"/>
                <Property Name="Name" Type="Edm.String"/>
                <!-- Navigation zu Child: -->
                <NavigationProperty Name="to_Children"
                    Type="Collection(MyService.ChildEntity)" Partner="to_Parent"/>
            </EntityType>

            <EntityType Name="ChildEntity">
                <Key><PropertyRef Name="ParentID"/><PropertyRef Name="ChildID"/></Key>
                <Property Name="ParentID" Type="Edm.String" Nullable="false"/>
                <Property Name="ChildID"  Type="Edm.String" Nullable="false"/>
                <Property Name="Value"    Type="Edm.String"/>
                <!-- Rück-Navigation: -->
                <NavigationProperty Name="to_Parent"
                    Type="MyService.MyEntity" Partner="to_Children"/>
            </EntityType>

            <!-- 2. EntityContainer -->
            <EntityContainer Name="MyContainer">
                <EntitySet Name="MyEntities" EntityType="MyService.MyEntity">
                    <NavigationPropertyBinding Path="to_Children" Target="ChildEntities"/>
                </EntitySet>
                <EntitySet Name="ChildEntities" EntityType="MyService.ChildEntity">
                    <NavigationPropertyBinding Path="to_Parent" Target="MyEntities"/>
                </EntitySet>
            </EntityContainer>

        </Schema>
    </edmx:DataServices>
</edmx:Edmx>
```

**OData V2 (Smart Templates):**

```xml
<?xml version="1.0" encoding="utf-8"?>
<edmx:Edmx Version="1.0"
    xmlns:edmx="http://schemas.microsoft.com/ado/2007/06/edmx"
    xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata"
    xmlns:sap="http://www.sap.com/Protocols/SAPData">
    <edmx:DataServices m:DataServiceVersion="2.0">
        <Schema Namespace="Z_MY_SRV"
            xmlns="http://schemas.microsoft.com/ado/2008/09/edm">

            <!-- 1. EntityType(s) -->
            <EntityType Name="MyEntityType" sap:label="My Entity">
                <Key><PropertyRef Name="ID"/></Key>
                <Property Name="ID"   Type="Edm.String" Nullable="false" sap:label="ID"/>
                <Property Name="Name" Type="Edm.String" sap:label="Name"/>
                <!-- NavigationProperty referenziert Association: -->
                <NavigationProperty Name="to_Children"
                    Relationship="Z_MY_SRV.assoc_Parent_Child"
                    FromRole="FromRole_Parent" ToRole="ToRole_Child"/>
            </EntityType>

            <EntityType Name="ChildEntityType" sap:label="Child">
                <Key><PropertyRef Name="ParentID"/><PropertyRef Name="ChildID"/></Key>
                <Property Name="ParentID" Type="Edm.String" Nullable="false"/>
                <Property Name="ChildID"  Type="Edm.String" Nullable="false"/>
                <NavigationProperty Name="to_Parent"
                    Relationship="Z_MY_SRV.assoc_Parent_Child"
                    FromRole="ToRole_Child" ToRole="FromRole_Parent"/>
            </EntityType>

            <!-- 2. Association (Pflicht in V2!) -->
            <Association Name="assoc_Parent_Child">
                <End Type="Z_MY_SRV.MyEntityType"  Multiplicity="1" Role="FromRole_Parent"/>
                <End Type="Z_MY_SRV.ChildEntityType" Multiplicity="*" Role="ToRole_Child"/>
                <ReferentialConstraint>
                    <Principal Role="FromRole_Parent"><PropertyRef Name="ID"/></Principal>
                    <Dependent Role="ToRole_Child"><PropertyRef Name="ParentID"/></Dependent>
                </ReferentialConstraint>
            </Association>

            <!-- 3. EntityContainer -->
            <EntityContainer Name="Z_MY_SRV_Entities"
                m:IsDefaultEntityContainer="true">
                <EntitySet Name="MyEntitySet"   EntityType="Z_MY_SRV.MyEntityType"
                    sap:creatable="true" sap:updatable="true" sap:deletable="false"/>
                <EntitySet Name="ChildEntitySet" EntityType="Z_MY_SRV.ChildEntityType"
                    sap:creatable="true" sap:updatable="true" sap:deletable="false"/>
                <!-- AssociationSet (Pflicht in V2!) -->
                <AssociationSet Name="assoc_Parent_Child_Set"
                    Association="Z_MY_SRV.assoc_Parent_Child">
                    <End EntitySet="MyEntitySet"   Role="FromRole_Parent"/>
                    <End EntitySet="ChildEntitySet" Role="ToRole_Child"/>
                </AssociationSet>
            </EntityContainer>

        </Schema>
    </edmx:DataServices>
</edmx:Edmx>
```

**Checkliste für neue metadata.xml:**

```
☐ Jeder EntityType hat einen <Key>
☐ Key-Properties sind Nullable="false"
☐ Zusammengesetzte Keys: Fremdschlüssel-Felder sind im Key enthalten
☐ NavigationProperty Name beginnt mit to_
☐ 1:n Navigation: Type="Collection(...)"  /  1:1: kein Collection()
☐ Partner-Attribute zeigen aufeinander
☐ EntityContainer: NavigationPropertyBinding für beide Richtungen
☐ V2 zusätzlich: Association + AssociationSet vorhanden
☐ Mock-JSON: Schlüsselfelder stimmen mit EntityType Key überein
☐ Mock-JSON: Format plain Array [...] (kein {"value":[...]} Wrapper)
```

---

