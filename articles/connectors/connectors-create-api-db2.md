<properties
    pageTitle="De DB2-connector toevoegen in uw logica Apps | Microsoft Azure"
    description="Overzicht van de connector met REST API parameters DB2"
    services=""
    documentationCenter="" 
    authors="gplarsen"
    manager="erikre"
    editor=""
    tags="connectors"/>

<tags
   ms.service="logic-apps"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration" 
   ms.date="09/26/2016"
   ms.author="plarsen"/>


# <a name="get-started-with-the-db2-connector"></a>Aan de slag met de DB2-connector
Microsoft-connector voor DB2 verbindt logica Apps tot bronnen in een IBM DB2-database is opgeslagen. Deze connector bevat een client voor Microsoft om te communiceren met externe DB2 server-computers in een TCP/IP-netwerk. Hiertoe behoren cloud-databases, zoals IBM, Bluemix, dashDB of IBM DB2 voor Windows gebruikt in Azure virtualisatie, en het databases met behulp van de gegevens in de lokalen gateway ruimten. Zie de [lijst met ondersteunde](connectors-create-api-db2.md#supported-db2-platforms-and-versions) platforms voor IBM DB2 en versies (in dit onderwerp).

>[AZURE.NOTE] Deze versie van het artikel is van toepassing op logica Apps algemene beschikbaarheid (GA). 

De DB2-connector ondersteunt de volgende databasebewerkingen:

- Lijst databasetabellen
- Een rij selecteren met lezen
- Lees alle rijen selecteren met
- Een rij invoegen met toevoegen
- Een rij met UPDATE wijzigen
- Verwijderen van een rij met DELETE

In dit onderwerp wordt beschreven hoe u de verbindingslijn in een proces databasebewerkingen logica-toepassingen gebruiken.

Meer informatie over Logic Apps, Zie [maken van een app logica](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="available-actions"></a>Beschikbare acties
De DB2-connector ondersteunt de volgende logica app acties:

- GetTables
- GetRow
- GetRows
- InsertRow
- UpdateRow
- DeleteRow


## <a name="list-tables"></a>Lijst met tabellen
Maken van een app logica voor elke bewerking bestaat uit vele stappen die worden uitgevoerd via de portal van Microsoft Azure.

U kunt een actie binnen de app logica toevoegen aan de lijst met tabellen in een DB2-database. De actie geeft de connector voor het verwerken van een schema DB2-instructie zoals `CALL SYSIBM.SQLTABLES`.

### <a name="create-a-logic-app"></a>Maken van een app logica
1.  In **Azure board start**, selecteert u **+** (plusteken) **Web + Mobile**en **Logica App**.
2.  Voer de **naam**, zoals `Db2getTables`, **abonnement**, **resourcegroep**, **locatie**en **Plannen van App-Service**. Selecteer **Pin dashboard**en selecteer **maken**.

### <a name="add-a-trigger-and-action"></a>Een trigger en een actie toevoegen
1.  Selecteer in de **Logica Apps Designer** **Leeg LogicApp** in de lijst met **sjablonen** .
2.  Selecteer in de lijst met **triggers** **Terugkeerpatroon**. 
3.  In de trigger **Terugkeerpatroon** selecteert u **bewerken**en selecteer **frequentie** vervolgkeuzelijst Selecteer **dag**stelt het **Interval** voor het type **7**.  
4.  Schakel het **+ nieuwe stap** in en selecteer vervolgens **een actie toevoegen**.
5.  Typ in de lijst **Acties** `db2` in het **Zoeken naar meer acties** vak bewerken en selecteer vervolgens de **DB2 - tabellen (voorbeeld) ophalen**.

    ![](./media/connectors-create-api-db2/Db2connectorActions.png)  

6.  Selecteer **checkbox** **Verbinden via op ruimten gegevensgateway**inschakelen in het venster configuratie **DB2 - tabellen ophalen** . U ziet dat de instellingen van cloud op locatie wijzigen.
    - Typ de waarde voor de **Server**in de vorm van een poortnummer of -alias dubbele punt. Typ bijvoorbeeld `ibmserver01:50000`.
    - Typ de waarde voor de **Database**. Typ bijvoorbeeld `nwind`.
    - Selecteer de waarde voor de **verificatie**. Selecteer bijvoorbeeld **standaard**.
    - Typ de waarde voor **gebruikersnaam**. Typ bijvoorbeeld `db2admin`.
    - Typ de waarde voor het **wachtwoord**. Typ bijvoorbeeld `Password1`.
    - Selecteer de waarde voor de **Gateway**. Selecteer bijvoorbeeld **datagateway01**.
7. Selecteer **maken**en selecteer vervolgens **Opslaan**. 

    ![](./media/connectors-create-api-db2/Db2connectorOnPremisesDataGatewayConnection.png)

8.  Selecteer in de blade **Db2getTables** in de lijst **alle wordt uitgevoerd** onder **Samenvatting**, het eerste vermeld item (meest recente uitvoeren).
9.  Selecteer **Details worden uitgevoerd**in de blade **logica app uitvoeren** . Selecteer in de lijst **actie** **Get_tables**. Zie de waarde voor de **Status**, die moet worden **uitgevoerd**. Selecteer de **koppeling invoer** om de invoer weer te geven. Selecteer de **koppeling wordt de uitvoer**en bekijk de uitgangen; een lijst met tabellen die dient te bevatten.

    ![](./media/connectors-create-api-db2/Db2connectorGetTablesLogicAppRunOutputs.png)

## <a name="create-the-connections"></a>De verbindingen maken
Deze connector ondersteunt verbindingen naar databases staan op gebouwen en in de cloud met behulp van de volgende eigenschappen van de verbinding. 

Eigenschap | Beschrijving
--- | ---
Server | Vereist. Een string-waarde die een IP-adres of de alias in de IPv4- of IPv6-indeling, gevolgd (gescheiden door dubbele punt) door een TCP/IP-poortnummer vertegenwoordigt accepteert. 
database | Vereist. Een string-waarde die een DRDA relationele Database naam (RDBNAM aangeeft) accepteert. DB2 voor z/OS accepteert een tekenreeks van 16 bytes (database is bekend als een IBM DB2 z/OS, locatie). DB2 voor i5/OS accepteert een reeks bytes 18 (database staat bekend als een IBM DB2 voor ik relationele database). DB2 voor LUW accepteert een tekenreeks van 8-byte.
verificatie | Dit is optioneel. Een lijst met item-waarde, Basic of Windows (kerberos) accepteert. 
gebruikersnaam | Vereist. Een string-waarde accepteert. DB2 voor z/OS accepteert een tekenreeks van 8-byte. DB2 voor ik een reeks van 10 bytes accepteert. DB2 voor Linux of UNIX accepteert een tekenreeks van 8-byte. DB2 voor Windows accepteert een tekenreeks 30 bytes.
wachtwoord | Vereist. Een string-waarde accepteert.
gateway | Vereist. Artikelwaarde in een lijst, die de op ruimten gegevensgateway naar Apps logica gedefinieerd binnen de opslaggroep accepteert.  

## <a name="create-the-on-premises-gateway-connection"></a>De lokalen op gateway verbinding maken
Deze connector toegang tot een op ruimten DB2-database met behulp van de gateway op gebouwen. Zie gateway onderwerpen voor meer informatie. 

1. Selecteer in het venster configuratie **Gateways** **checkbox** **Verbinden via gateway**inschakelen. U ziet dat de instellingen van cloud op locatie wijzigen.
2. Typ de waarde voor de **Server**in de vorm van een poortnummer of -alias dubbele punt. Typ bijvoorbeeld `ibmserver01:50000`.
3. Typ de waarde voor de **Database**. Typ bijvoorbeeld `nwind`.
4. Selecteer de waarde voor de **verificatie**. Selecteer bijvoorbeeld **standaard**.
5. Typ de waarde voor **gebruikersnaam**. Typ bijvoorbeeld `db2admin`.
6. Typ de waarde voor het **wachtwoord**. Typ bijvoorbeeld `Password1`.
7. Selecteer de waarde voor de **Gateway**. Selecteer bijvoorbeeld **datagateway01**.
8. Selecteer **maken** om door te gaan. 

    ![](./media/connectors-create-api-db2/Db2connectorOnPremisesDataGatewayConnection.png)

## <a name="create-the-cloud-connection"></a>De cloud-verbinding maken
Deze connector toegang tot een wolk DB2-database. 

1. Laat het **selectievakje** uitgeschakeld in het venster configuratie **Gateways** (gekleurd) **verbinding maken via de gateway**. 
2. Typ de waarde voor de **naam van de verbinding**. Typ bijvoorbeeld `hisdemo2`.
3. Typ de waarde voor **DB2 servernaam**in de vorm van een poortnummer of -alias dubbele punt. Typ bijvoorbeeld `hisdemo2.cloudapp.net:50000`.
3. Typ de waarde voor **de naam van de DB2-database**. Typ bijvoorbeeld `nwind`.
4. Typ de waarde voor **gebruikersnaam**. Typ bijvoorbeeld `db2admin`.
5. Typ de waarde voor het **wachtwoord**. Typ bijvoorbeeld `Password1`.
6. Selecteer **maken** om door te gaan. 

    ![](./media/connectors-create-api-db2/Db2connectorCloudConnection.png)

## <a name="fetch-all-rows-using-select"></a>Ophalen van alle rijen selecteren met
U kunt een logische app actie wilt ophalen van alle rijen in een DB2-tabel definiëren. Dit geeft de connector voor het verwerken van een DB2-SELECT-instructie, zoals `SELECT * FROM AREA`.

### <a name="create-a-logic-app"></a>Maken van een app logica
1.  In **Azure board start**, selecteert u **+** (plusteken) **Web + Mobile**en **Logica App**.
2.  Voer de **naam**, zoals `Db2getRows`, **abonnement**, **resourcegroep**, **locatie**en **Plannen van App-Service**. Selecteer **Pin dashboard**en selecteer **maken**.

### <a name="add-a-trigger-and-action"></a>Een trigger en een actie toevoegen
1.  Selecteer in de **Logica Apps Designer** **Leeg LogicApp** in de lijst met **sjablonen** .
2.  Selecteer in de lijst met **triggers** **Terugkeerpatroon**. 
3.  In de trigger **Terugkeerpatroon** selecteert u **bewerken**en selecteer **frequentie** vervolgkeuzelijsten **dag**selecteren Selecteer **Interval** type **7**. 
4.  Schakel het **+ nieuwe stap** in en selecteer vervolgens **een actie toevoegen**.
5.  Typ in de lijst **Acties** `db2` in het **Zoeken naar meer acties** vak bewerken en selecteer vervolgens de **DB2 - rijen (voorbeeld) ophalen**.
6. Selecteer **verbinding wijzigen**in de actie **halen rijen (voorbeeld)** .
7. Selecteer **Nieuw**in het deelvenster **verbindingen** configureren. 

    ![](./media/connectors-create-api-db2/Db2connectorNewConnection.png)
  
8. Laat het **selectievakje** uitgeschakeld in het venster configuratie **Gateways** (gekleurd) **verbinding maken via de gateway**.
    - Typ de waarde voor de **naam van de verbinding**. Typ bijvoorbeeld `HISDEMO2`.
    - Typ de waarde voor **DB2 servernaam**in de vorm van een poortnummer of -alias dubbele punt. Typ bijvoorbeeld `HISDEMO2.cloudapp.net:50000`.
    - Typ de waarde voor **de naam van de DB2-database**. Typ bijvoorbeeld `NWIND`.
    - Typ de waarde voor **gebruikersnaam**. Typ bijvoorbeeld `db2admin`.
    - Typ de waarde voor het **wachtwoord**. Typ bijvoorbeeld `Password1`.
9. Selecteer **maken** om door te gaan.

    ![](./media/connectors-create-api-db2/Db2connectorCloudConnection.png)

10. Selecteer de **pijl-omlaag**in de lijst **naam van de tabel** en selecteer **gebied**.
11. Selecteer **Geavanceerde opties tonen** queryopties opgeven.
12. Selecteer **Opslaan**. 

    ![](./media/connectors-create-api-db2/Db2connectorGetRowsTableName.png)

13. Selecteer in de blade **Db2getRows** in de lijst **alle wordt uitgevoerd** onder **Samenvatting**, het eerste vermeld item (meest recente uitvoeren).
14. Selecteer **Details worden uitgevoerd**in de blade **logica app uitvoeren** . Selecteer in de lijst **actie** **Get_rows**. Zie de waarde voor de **Status**, die moet worden **uitgevoerd**. Selecteer de **koppeling invoer** om de invoer weer te geven. Selecteer de **koppeling wordt de uitvoer**en bekijk de uitgangen; een lijst met rijen die dient te bevatten.

    ![](./media/connectors-create-api-db2/Db2connectorGetRowsOutputs.png)

## <a name="add-one-row-using-insert"></a>Een rij invoegen met toevoegen
Hier kunt u een logische app actie om een rij in een DB2-tabel toevoegen. Deze actie geeft de connector voor het verwerken van een DB2-INSERT-instructie zoals `INSERT INTO AREA (AREAID, AREADESC, REGIONID) VALUES ('99999', 'Area 99999', 102)`.

### <a name="create-a-logic-app"></a>Maken van een app logica
1.  In **Azure board start**, selecteert u **+** (plusteken) **Web + Mobile**en **Logica App**.
2.  Voer de **naam**, zoals `Db2insertRow`, **abonnement**, **resourcegroep**, **locatie**en **Plannen van App-Service**. Selecteer **Pin dashboard**en selecteer **maken**.

### <a name="add-a-trigger-and-action"></a>Een trigger en een actie toevoegen
1.  Selecteer in de **Logica Apps Designer** **Leeg LogicApp** in de lijst met **sjablonen** .
2.  Selecteer in de lijst met **triggers** **Terugkeerpatroon**. 
3.  In de trigger **Terugkeerpatroon** selecteert u **bewerken**en selecteer **frequentie** vervolgkeuzelijsten **dag**selecteren Selecteer **Interval** type **7**. 
4.  Schakel het **+ nieuwe stap** in en selecteer vervolgens **een actie toevoegen**.
5.  **Db2** typt in het vak **Zoeken naar meer acties** in de lijst **Acties** en selecteert u **DB2 - rij invoegen (voorbeeld)**.
6. Selecteer **verbinding wijzigen**in de actie **halen rijen (voorbeeld)** . 
7. Selecteer een verbinding in het deelvenster **verbindingen** configureren. Selecteer bijvoorbeeld **hisdemo2**.

    ![](./media/connectors-create-api-db2/Db2connectorChangeConnection.png)

8. Selecteer de **pijl-omlaag**in de lijst **naam van de tabel** en selecteer **gebied**.
9. Voer waarden in voor alle vereiste kolommen (Zie rood sterretje). Typ bijvoorbeeld `99999` typt **AREAID**, `Area 99999`, en het type `102` voor **REGIONID**. 
10. Selecteer **Opslaan**.

    ![](./media/connectors-create-api-db2/Db2connectorInsertRowValues.png)
 
11. Selecteer in de blade **Db2insertRow** in de lijst **alle wordt uitgevoerd** onder **Samenvatting**, het eerste vermeld item (meest recente uitvoeren).
12. Selecteer **Details worden uitgevoerd**in de blade **logica app uitvoeren** . Selecteer in de lijst **actie** **Get_rows**. Zie de waarde voor de **Status**, die moet worden **uitgevoerd**. Selecteer de **koppeling invoer** om de invoer weer te geven. Selecteer de **koppeling wordt de uitvoer**en bekijk de uitgangen; waar de nieuwe rij moet opnemen.

    ![](./media/connectors-create-api-db2/Db2connectorInsertRowOutputs.png)

## <a name="fetch-one-row-using-select"></a>Een rij selecteren met ophalen
U kunt een logische app actie als u een rij in een DB2-tabel ophalen wilt definiëren. Deze actie geeft de connector voor het verwerken van een instructie DB2 selecteren waar, zoals `SELECT FROM AREA WHERE AREAID = '99999'`.

### <a name="create-a-logic-app"></a>Maken van een app logica
1.  In **Azure board start**, selecteert u **+** (plusteken) **Web + Mobile**en **Logica App**.
2.  Voer de **naam** (bijvoorbeeld '**Db2getRow**'), **abonnement**, **resourcegroep**, **locatie**en **Plannen van App-Service**. Selecteer **Pin dashboard**en selecteer **maken**.

### <a name="add-a-trigger-and-action"></a>Een trigger en een actie toevoegen
1.  Selecteer in de **Logica Apps Designer** **Leeg LogicApp** in de lijst met **sjablonen** . 
2.  Selecteer in de lijst met **triggers** **Terugkeerpatroon**. 
3.  In de trigger **Terugkeerpatroon** selecteert u **bewerken**en selecteer **frequentie** vervolgkeuzelijsten **dag**selecteren Selecteer **Interval** type **7**. 
4.  Schakel het **+ nieuwe stap** in en selecteer vervolgens **een actie toevoegen**.
5.  **Db2** typt in het vak **Zoeken naar meer acties** in de lijst **Acties** en selecteert u **DB2 - rijen (voorbeeld) ophalen**.
6. Selecteer **verbinding wijzigen**in de actie **halen rijen (voorbeeld)** . 
7. Selecteer een bestaande verbinding in het deelvenster **verbindingen** configuraties. Selecteer bijvoorbeeld **hisdemo2**.

    ![](./media/connectors-create-api-db2/Db2connectorChangeConnection.png)

8. Selecteer de **pijl-omlaag**in de lijst **naam van de tabel** en selecteer **gebied**.
9. Voer waarden in voor alle vereiste kolommen (Zie rood sterretje). Typ bijvoorbeeld `99999` voor de **Gebieds-id**. 
10. Selecteer **Geavanceerde opties tonen** queryopties opgeven.
11. Selecteer **Opslaan**. 

    ![](./media/connectors-create-api-db2/Db2connectorGetRowValues.png)

12. Selecteer in de blade **Db2getRow** in de lijst **alle wordt uitgevoerd** onder **Samenvatting**, het eerste vermeld item (meest recente uitvoeren).
13. Selecteer **Details worden uitgevoerd**in de blade **logica app uitvoeren** . Selecteer in de lijst **actie** **Get_rows**. Zie de waarde voor de **Status**, die moet worden **uitgevoerd**. Selecteer de **koppeling invoer** om de invoer weer te geven. Selecteer de **koppeling wordt de uitvoer**en bekijk de uitgangen; welke rij moet opnemen.

    ![](./media/connectors-create-api-db2/Db2connectorGetRowOutputs.png)

## <a name="change-one-row-using-update"></a>Een rij met UPDATE wijzigen
U kunt een logische app actie als u wilt wijzigen van een rij in een DB2-tabel definiëren. Deze actie geeft de connector voor het verwerken van een DB2-UPDATE-instructie zoals `UPDATE AREA SET AREAID = '99999', AREADESC = 'Area 99999', REGIONID = 102)`.

### <a name="create-a-logic-app"></a>Maken van een app logica
1.  In **Azure board start**, selecteert u **+** (plusteken) **Web + Mobile**en **Logica App**.
2.  Voer de **naam**, zoals `Db2updateRow`, **abonnement**, **resourcegroep**, **locatie**en **Plannen van App-Service**. Selecteer **Pin dashboard**en selecteer **maken**.

### <a name="add-a-trigger-and-action"></a>Een trigger en een actie toevoegen
1.  Selecteer in de **Logica Apps Designer** **Leeg LogicApp** in de lijst met **sjablonen** .
2.  Selecteer in de lijst met **triggers** **Terugkeerpatroon**. 
3.  In de trigger **Terugkeerpatroon** selecteert u **bewerken**en selecteer **frequentie** vervolgkeuzelijsten **dag**selecteren Selecteer **Interval** type **7**. 
4.  Schakel het **+ nieuwe stap** in en selecteer vervolgens **een actie toevoegen**.
5.  **Db2** typt in het vak **Zoeken naar meer acties** in de lijst **Acties** en selecteert u **DB2 - Update rij (voorbeeld)**.
6. Selecteer **verbinding wijzigen**in de actie **halen rijen (voorbeeld)** . 
7. Selecteer een bestaande verbinding selecteren in het deelvenster **verbindingen** configuraties. Selecteer bijvoorbeeld **hisdemo2**.

    ![](./media/connectors-create-api-db2/Db2connectorChangeConnection.png)

8. Selecteer de **pijl-omlaag**in de lijst **naam van de tabel** en selecteer **gebied**.
9. Voer waarden in voor alle vereiste kolommen (Zie rood sterretje). Typ bijvoorbeeld `99999` typt **AREAID**, `Updated 99999`, en het type `102` voor **REGIONID**. 
10. Selecteer **Opslaan**. 

    ![](./media/connectors-create-api-db2/Db2connectorUpdateRowValues.png)

11. Selecteer in de blade **Db2updateRow** in de lijst **alle wordt uitgevoerd** onder **Samenvatting**, het eerste vermeld item (meest recente uitvoeren).
12. Selecteer **Details worden uitgevoerd**in de blade **logica app uitvoeren** . Selecteer in de lijst **actie** **Get_rows**. Zie de waarde voor de **Status**, die moet worden **uitgevoerd**. Selecteer de **koppeling invoer** om de invoer weer te geven. Selecteer de **koppeling wordt de uitvoer**en bekijk de uitgangen; waar de nieuwe rij moet opnemen.

    ![](./media/connectors-create-api-db2/Db2connectorUpdateRowOutputs.png)

## <a name="remove-one-row-using-delete"></a>Verwijderen van een rij met DELETE
U kunt een logische app actie als u wilt verwijderen van een rij in een DB2-tabel definiëren. Deze actie geeft de connector voor het verwerken van een instructie DB2 verwijderen, zoals `DELETE FROM AREA WHERE AREAID = '99999'`.

### <a name="create-a-logic-app"></a>Maken van een app logica
1.  In **Azure board start**, selecteert u **+** (plusteken) **Web + Mobile**en **Logica App**.
2.  Voer de **naam**, zoals `Db2deleteRow`, **abonnement**, **resourcegroep**, **locatie**en **Plannen van App-Service**. Selecteer **Pin dashboard**en selecteer **maken**.

### <a name="add-a-trigger-and-action"></a>Een trigger en een actie toevoegen
1.  Selecteer in de **Logica Apps Designer** **Leeg LogicApp** in de lijst met **sjablonen** . 
2.  Selecteer in de lijst met **triggers** **Terugkeerpatroon**. 
3.  In de trigger **Terugkeerpatroon** selecteert u **bewerken**en selecteer **frequentie** vervolgkeuzelijsten **dag**selecteren Selecteer **Interval** type **7**. 
4.  Schakel het **+ nieuwe stap** in en selecteer vervolgens **een actie toevoegen**.
5.  **Db2** selecteren in het vak **Zoeken naar meer acties** en **DB2 - rij (voorbeeld) verwijderen**selecteert in de lijst **Acties** .
6. Selecteer **verbinding wijzigen**in de actie **halen rijen (voorbeeld)** . 
7. Selecteer een bestaande verbinding in het deelvenster **verbindingen** configuraties. Selecteer bijvoorbeeld **hisdemo2**.

    ![](./media/connectors-create-api-db2/Db2connectorChangeConnection.png)

8. Selecteer de **pijl-omlaag**in de lijst **naam van de tabel** en selecteer **gebied**.
9. Voer waarden in voor alle vereiste kolommen (Zie rood sterretje). Typ bijvoorbeeld `99999` voor de **Gebieds-id**. 
10. Selecteer **Opslaan**. 

    ![](./media/connectors-create-api-db2/Db2connectorDeleteRowValues.png)

11. Selecteer in de blade **Db2deleteRow** in de lijst **alle wordt uitgevoerd** onder **Samenvatting**, het eerste vermeld item (meest recente uitvoeren).
12. Selecteer **Details worden uitgevoerd**in de blade **logica app uitvoeren** . Selecteer in de lijst **actie** **Get_rows**. Zie de waarde voor de **Status**, die moet worden **uitgevoerd**. Selecteer de **koppeling invoer** om de invoer weer te geven. Selecteer de **koppeling wordt de uitvoer**en bekijk de uitgangen; dat de verwijderde rij moet opnemen.

    ![](./media/connectors-create-api-db2/Db2connectorDeleteRowOutputs.png)

## <a name="technical-details"></a>Technische Details

## <a name="actions"></a>Acties
Een actie is een bewerking die wordt uitgevoerd door de werkstroom in een app logica gedefinieerd. De DB2-database connector omvat de volgende acties. 

|Actie|Beschrijving|
|--- | ---|
|[GetRow](connectors-create-api-db2.md#get-row)|Een enkele rij opgehaald uit een DB2-tabel|
|[GetRows](connectors-create-api-db2.md#get-rows)|Rijen opgehaald uit een DB2-tabel|
|[InsertRow](connectors-create-api-db2.md#insert-row)|Met deze methode voegt een nieuwe rij in een DB2-tabel|
|[DeleteRow](connectors-create-api-db2.md#delete-row)|Hiermee verwijdert u een rij uit een DB2-tabel|
|[GetTables](connectors-create-api-db2.md#get-tables)|Tabellen opgehaald uit een DB2-database|
|[UpdateRow](connectors-create-api-db2.md#update-row)|Een bestaande rij in een DB2-tabel bijwerken|

### <a name="action-details"></a>Actiedetails

In dit gedeelte ziet u de specifieke details over elke actie, met inbegrip van eventuele verplichte of optionele eigenschappen voor de invoer en de bijbehorende uitvoer die is gekoppeld aan de verbindingslijn.

#### <a name="get-row"></a>Rij ophalen 
Een enkele rij opgehaald uit een DB2-tabel.  

| Naam van eigenschap| Weergavenaam |Beschrijving|
| ---|---|---|
|tabel * | Naam van de tabel |Naam van DB2-tabel|
|ID * | Rij-id |De unieke id van de rij ophalen|

Een sterretje (*) betekent dat de eigenschap is vereist.

##### <a name="output-details"></a>Uitvoerdetails
Artikel

| Naam van eigenschap | Gegevenstype |
|---|---|
|ItemInternalId|tekenreeks|


#### <a name="get-rows"></a>Rijen ophalen 
Rijen opgehaald uit een DB2-tabel.  

|Naam van eigenschap| Weergavenaam|Beschrijving|
| ---|---|---|
|tabel *|Naam van de tabel|Naam van DB2-tabel|
|$skip|Aantal overgeslagen|Aantal items over te slaan (standaard = 0)|
|$top|Get maximum aantal|Maximum aantal items op te halen (standaard = 256)|
|$filter|Filteren van Query|Een query ODATA filter het aantal items beperken|
|$orderby|Order By|Een query ODATA sorteren op voor het opgeven van de volgorde van items|

Een sterretje (*) betekent dat de eigenschap is vereist.

##### <a name="output-details"></a>Uitvoerdetails
ItemsList

| Naam van eigenschap | Gegevenstype |
|---|---|
|waarde|matrix|


#### <a name="insert-row"></a>Rij invoegen 
Hiermee voegt een nieuwe rij in een DB2-tabel.  

|Naam van eigenschap| Weergavenaam|Beschrijving|
| ---|---|---|
|tabel *|Naam van de tabel|Naam van DB2-tabel|
|artikel *|Rij|Rij invoegen in de opgegeven tabel in DB2|

Een sterretje (*) betekent dat de eigenschap is vereist.

##### <a name="output-details"></a>Uitvoerdetails
Artikel

| Naam van eigenschap | Gegevenstype |
|---|---|
|ItemInternalId|tekenreeks|


#### <a name="delete-row"></a>Rij verwijderen 
Hiermee verwijdert u een rij uit een DB2-tabel.  

|Naam van eigenschap| Weergavenaam|Beschrijving|
| ---|---|---|
|tabel *|Naam van de tabel|Naam van DB2-tabel|
|ID *|Rij-id|De unieke id van de rij verwijderen|

Een sterretje (*) betekent dat de eigenschap is vereist.

##### <a name="output-details"></a>Uitvoerdetails
Geen.

#### <a name="get-tables"></a>Tabellen ophalen 
Tabellen ophaalt uit een DB2-database.  

Er zijn geen parameters voor deze oproep. 

##### <a name="output-details"></a>Uitvoerdetails 
TablesList

| Naam van eigenschap | Gegevenstype |
|---|---|
|waarde|matrix|

#### <a name="update-row"></a>Rij bijwerken 
Een bestaande rij in een DB2-tabel bijgewerkt.  

|Naam van eigenschap| Weergavenaam|Beschrijving|
| ---|---|---|
|tabel *|Naam van de tabel|Naam van DB2-tabel|
|ID *|Rij-id|De unieke id van de rij bijwerken|
|artikel *|Rij|Rij met de bijgewerkte waarden|

Een sterretje (*) betekent dat de eigenschap is vereist.

##### <a name="output-details"></a>Uitvoerdetails  
Artikel

| Naam van eigenschap | Gegevenstype |
|---|---|
|ItemInternalId|tekenreeks|


### <a name="http-responses"></a>HTTP-antwoorden

Als u de verschillende acties, krijgt u mogelijk bepaalde antwoorden. De volgende tabel geeft een overzicht van de antwoorden en de bijbehorende beschrijvingen:  

|Naam|Beschrijving|
|---|---|
|200|OK|
|202|Geaccepteerd|
|400|Ongeldig verzoek|
|401|Niet-geautoriseerde|
|403|Verboden|
|404|Niet gevonden|
|500|Interne serverfout. Er is een onbekende fout opgetreden|
|Standaard|De bewerking is mislukt.|


## <a name="supported-db2-platforms-and-versions"></a>Ondersteunde platforms voor DB2 en versies
Deze connector ondersteunt de volgende IBM DB2 platforms en versies, evenals IBM DB2 compatibele producten (bv. Bluemix van IBM-dashDB) die ondersteuning bieden voor gedistribueerde relationele Database Architecture (DRDA) SQL Access Manager versie 10 en 11 (SQLAM):

- IBM DB2 voor z/OS 11.1
- IBM DB2 voor z/OS 10.1
- IBM DB2, voor 7,3 i
- IBM DB2 voor 7.2 i
- IBM DB2 voor ik 7.1
- IBM DB2 voor LUW 11
- IBM DB2 voor LUW 10.5


## <a name="next-steps"></a>Volgende stappen

[Maken van een app logica](../app-service-logic/app-service-logic-create-a-logic-app.md). Bekijk de beschikbare connectors in Apps logica op onze [lijst van API's](apis-list.md).

