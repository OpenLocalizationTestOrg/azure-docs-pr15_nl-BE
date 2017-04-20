<properties
   pageTitle="Met de DB2-connector in Microsoft Azure App Service | Microsoft Azure"
   description="Het gebruik van de DB2-connector met logica app triggers en acties"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="gplarsen"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="05/31/2016"
   ms.author="plarsen"/>

# <a name="db2-connector"></a>DB2-connector
>[AZURE.NOTE] Deze versie van het artikel is van toepassing op de schemaversie van logica apps 2014-12-01-voorbeeld.

Microsoft-connector voor DB2 is een API-app voor de toepassingen via Azure App verbinding met bronnen die zijn opgeslagen in een IBM DB2-database. Connector bevat een Microsoft Client verbinding maken met externe computers voor DB2 server via een TCP/IP-netwerkverbinding, met inbegrip van Azure hybride verbindingen voor bedrijfsruimten DB2-servers met behulp van de Relay Azure Service Bus connector ondersteunt de volgende databasebewerkingen:

- Selecteer alleen rijen met
- Poll te lezen met behulp van gevolgd door selecteren selecteert u het aantal rijen
- Toevoegen van één rij of meerdere (bulk) rijen invoegen gebruiken
- Een rij of meerdere (bulk) rijen met UPDATE wijzigen
- Verwijderen van een rij of meerdere (bulk) rijen met DELETE
- Alleen rijen met selecteren CURSOR, gevolgd door UPDATE waar huidige van de CURSOR wijzigen
- Alleen rijen met selecteren CURSOR, gevolgd door UPDATE waar huidige van de CURSOR verwijderen
- Procedure uitvoeren met de parameters voor invoer en uitvoer, waarde, een resultaatset, GESPREK met
- Aangepaste opdrachten en samengestelde bewerkingen met SELECT, INSERT, UPDATE, DELETE

## <a name="triggers-and-actions"></a>Triggers en acties
Connector ondersteunt de volgende logica app triggers en acties:

Triggers | Acties
--- | ---
<ul><li>Poll gegevens</li></ul> | <ul><li>Massaal invoegen</li><li>Invoegen</li><li>Bulkupdate</li><li>Update</li><li>Oproep</li><li>Bulk verwijderen</li><li>Verwijderen</li><li>Selecteer</li><li>Voorwaardelijke update</li><li>Boeken naar EntitySet</li><li>Voorwaardelijke verwijderen</li><li>Selecteer één entiteit</li><li>Verwijderen</li><li>Upsert naar EntitySet</li><li>Aangepaste opdrachten</li><li>Samengestelde bewerkingen</li></ul>


## <a name="create-the-db2-connector"></a>Maak de DB2-connector
Kunt u een verbindingslijn in een app logica of van de markt Azure, net zoals in het volgende voorbeeld:  

1. Selecteer in de Azure startboard **Marketplace**.
2. In de blade **Alles** **db2** typt in het vak **Zoeken alles** en klik vervolgens op de enter-toets.
3. Bij het zoeken resultaten alles deelvenster, selecteert u **DB2-connector**.
4. Selecteer in de DB2-connector beschrijving-blade **maken**.
5. Voer de naam (bijvoorbeeld ' Db2ConnectorNewOrders'), App Service plannen en andere eigenschappen in de blade-pakket van DB2-connector.
6. Selecteer **Pakketinstellingen**en geef de volgende pakketinstellingen:  

    Naam | Vereist |  Beschrijving
--- | --- | ---
ConnectionString | Ja | Verbindingsreeks DB2-Client (bijvoorbeeld ' Network Address = servernaam; Network Port = 50000; Gebruikers-ID = gebruikersnaam; Wachtwoord = wachtwoord; Initial Catalog = monster; Pakket collectie NWIND; = standaard Schema = NWIND ').
Tabellen | Ja | Door komma's gescheiden lijst van de tabel, weergave en alias namen die nodig zijn voor de OData-bewerkingen en voor het genereren van documentatie swagger met voorbeelden (b.v. "*NEWORDERS*").
Procedures | Ja | Door komma's gescheiden lijst met namen van procedure en functie (b.v. "SPORDERID").
Lokale | Nee | Over lokalen met Azure Service Bus Relay implementeren.
ServiceBusConnectionString | Nee | Azure Service Bus Relay-verbindingsreeks.
PollToCheckData | Nee | Instructie COUNT selecteren voor gebruik met een trigger logica app (bijvoorbeeld ' SELECT COUNT (\*) uit NEWORDERS waar VERZENDDATUM IS NULL ').
PollToReadData | Nee | SELECT-instructie te gebruiken met een trigger logica app (bijvoorbeeld ' selecteren \* van NEWORDERS waarbij VERZENDDATUM NULL voor UPDATE is ").
PollToAlterData | Nee | UPDATE- of DELETE-instructie te gebruiken met een trigger logica app (bijvoorbeeld ' UPDATE NEWORDERS SET VERZENDDATUM = huidige datum waarop huidige van &lt;CURSOR&gt;").

7. Klik op **OK**en selecteer vervolgens **maken**.
8. Wanneer voltooid, er de instellingen van het pakket met de volgende strekking:  
![][1]


## <a name="logic-app-with-db2-connector-action-to-add-data"></a>Logica app met DB2 connector actie gegevens toe te voegen ##
U kunt een logische app actie gegevens toevoegen aan een DB2-tabel met behulp van een API invoegen of Post entiteit OData-bewerking definiëren. Zo kunt u een nieuwe klantrecord volgorde door de verwerking van een SQL INSERT-instructie ten opzichte van een tabel met een identiteitskolom worden gedefinieerd als het resultaat de waarde of de rijen aan de app logica (ORDID van uiteindelijke tabel selecteren (invoegen in NWIND. WAARDEN VAN NEWORDERS (CUSTID, VERZENDNAAM, SHIPADDR, VERZENDPLAATS, SHIPREG, SHIPZIP) (?,?,?,?,?,?))).

> [AZURE.TIP] DB2-verbinding "*boeken naar EntitySet*" retourneert de waarde van de identiteitskolom en '*API invoegen*' retourneert rijen beïnvloed

1. Selecteer in de Azure startboard **+** (plusteken) **Web + Mobile**en **logica app**.
2. Voer de naam (bijvoorbeeld ' NewOrdersDb2'), App Service plannen, andere eigenschappen en selecteer **maken**.
3. Selecteer in de Azure startboard de logica app die u zojuist hebt gemaakt, **Instellingen**en vervolgens **Triggers en acties**.
4. Selecteer **maken** binnen de app logica sjablonen in de blade Triggers en acties.
5. In het deelvenster API Apps **Terugkeerpatroon**selecteert, stelt u een frequentie-interval en vervolgens **ingeschakeld**.
6. In het deelvenster API Apps **DB2 verbindingslijn**selecteert, vouw de lijst activiteiten **plaats in NEWORDER**.
7. Voer de volgende waarden in de parameterlijst uitvouwen:  

    Naam | Waarde
--- | --- 
CUSTID | 10042
VERZENDNAAM | Lazy K Kountry winkel 
SHIPADDR | Terras 12 orkesten
VERZENDPLAATS | Walla Walla 
SHIPREG | WA
SHIPZIP | 99362 

8. Schakel het **selectievakje** opslaan van de actie-instellingen en klik op **Opslaan**.
9. De instellingen moeten er als volgt uitzien:  
![][3]

10. Selecteer in de lijst **alle wordt uitgevoerd** onder **bewerkingen**het eerst vermeld item (meest recente uitvoeren). 
11. Selecteer het **actie** -item- **db2connectorneworders**in het blad **logica app uitvoeren** .
12. Selecteer de **Koppeling "inputs"**in het blad **logische app actie** . De ingangen DB2-connector gebruikt voor het verwerken van een INSERT-instructie parameters.
13. Selecteer de **Koppeling UITGANGEN**in de blade **logische app actie** . De invoer moeten er als volgt uitzien:  
![][4]

#### <a name="what-you-need-to-know"></a>Wat u moet weten

- Connector DB2 tabelnamen worden afgekapt wanneer die logica app actienamen. Bijvoorbeeld wordt de bewerking **plaats in NEWORDERS** afgekapt in NEWORDER in te **voegen**.
- Na het opslaan van de logica app **Triggers en acties**verwerkt logica app u de bewerking. Kan er een vertraging van een aantal seconden (bijvoorbeeld 3-5 seconden) voordat de logica verwerkt app de bewerking. Desgewenst kunt u **Nu uitvoeren** voor het verwerken van de bewerking.
- DB2-connector definieert EntitySet leden met kenmerken, inclusief het lid komt overeen met een DB2-kolom met een standaard of kolommen (bv. identiteit) gegenereerd. Logica app geeft een rood sterretje naast de naam EntitySet lid-ID, voor DB2 kolommen waarden vereisen. Niet moet u een waarde opgeven voor een lid van de ORDID die overeenkomt met de id-kolom DB2. U kunt waarden invoeren voor andere optionele leden (artikelen, ORDDATE, REQDATE, EXPEDITEUR, vracht, SHIPCTRY), die met kolommen met standaardwaarden DB2 overeenkomen. 
- DB2-connector wordt logica app de reactie op de Post naar EntitySet waarin de waarden voor id-kolommen, die wordt afgeleid van de SQLDARD DRDA (SQL Data gebied antwoordgegevens) op de voorbereide SQL INSERT-instructie. DB2 server retourneert de ingevoegde waarden voor kolommen met standaardwaarden.  


## <a name="logic-app-with-db2-connector-action-to-add-bulk-data"></a>Logica app met DB2 connector actie grote hoeveelheden gegevens toe te voegen ##
U kunt een logische app actie gegevens toevoegen aan een DB2-tabel met behulp van een API-Bulk Insert-bewerking definiëren. Bijvoorbeeld, kunt u twee nieuwe order klantrecords, door verwerking van een SQL INSERT-instructie met behulp van een matrix met rijwaarden met een tabel met een identiteitskolom worden gedefinieerd retourneren de rijen aan de app logica (ORDID van uiteindelijke tabel selecteren (invoegen in NWIND. WAARDEN VAN NEWORDERS (CUSTID, VERZENDNAAM, SHIPADDR, VERZENDPLAATS, SHIPREG, SHIPZIP) (?,?,?,?,?,?))).

1. Selecteer in de Azure startboard **+** (plusteken) **Web + Mobile**en **logica app**.
2. Voer de naam (bijvoorbeeld ' NewOrdersBulkDb2'), App Service plannen, andere eigenschappen en selecteer **maken**.
3. Selecteer in de Azure startboard de logica app die u zojuist hebt gemaakt, **Instellingen**en vervolgens **Triggers en acties**.
4. Selecteer **maken** binnen de app logica sjablonen in de blade Triggers en acties.
5. In het deelvenster API Apps **Terugkeerpatroon**selecteert, stelt u een frequentie-interval en vervolgens **ingeschakeld**.
6. Selecteer **DB2-connector**, vouw de lijst bewerkingen **Bulk Insert in nieuw**in het deelvenster API Apps.
7. De **rows** -waarde invoert als een matrix. Bijvoorbeeld, kopieer en plak het volgende:

    ```
    [{"CUSTID":10081,"SHIPNAME":"Trail's Head Gourmet Provisioners","SHIPADDR":"722 DaVinci Blvd.","SHIPCITY":"Kirkland","SHIPREG":"WA","SHIPZIP":"98034"},{"CUSTID":10088,"SHIPNAME":"White Clover Markets","SHIPADDR":"305 14th Ave. S. Suite 3B","SHIPCITY":"Seattle","SHIPREG":"WA","SHIPZIP":"98128","SHIPCTRY":"USA"}]
    ```

8. Schakel het **selectievakje** opslaan van de actie-instellingen en klik op **Opslaan**. De instellingen moeten er als volgt uitzien:  
![][6]

9. In de lijst **alle wordt uitgevoerd** onder **bewerkingen**, klikt u op het item eerst vermeld (meest recente uitvoeren).
10. Klik op **actie** -item in de blade **logica app uitvoeren** .
11. Klik op de **Koppeling "inputs"**in het blad **logische app actie** . De uitvoer ziet er als volgt uit:  
[][7]
12. Klik op de **Koppeling van de UITGANGEN**in de blade **logische app actie** . De uitvoer ziet er als volgt uit:  
![][8]

#### <a name="what-you-need-to-know"></a>Wat u moet weten

- Connector DB2 tabelnamen worden afgekapt wanneer die logica app actienamen. De **Bulk Insert in NEWORDERS** bewerking is zo afgekapt tot **Bulk Insert in nieuw**.
- Zonder id-kolommen (bv. ORDID), (b.v. VERZENDDATUM) kolommen en kolommen met standaardwaarden (bv. ORDDATE, REQDATE, EXPEDITEUR, vracht, SHIPCTRY), genereert de DB2-database waarden.
- 'Vandaag' en 'morgen' opgeeft, DB2 connector genereert 'Vandaag' en "De huidige datum plus 1 dag" functies (bijv. REQDATE). 


## <a name="logic-app-with-db2-connector-trigger-to-read-alter-or-delete-data"></a>Logica app met DB2 connector trigger om te lezen, wijzigen of verwijderen van gegevens ##
U kunt een trigger logica app om te peilen en gegevens lezen uit een DB2-tabel met behulp van een API Poll samengestelde gegevensbewerking definiëren. Zo vindt u een of meer nieuwe klant bestelling records, records retourneren naar de app logica. De instellingen voor de DB2-pakket/app ziet er als volgt:

    App-instelling | Waarde
--- | --- | ---
PollToCheckData | SELECT COUNT (\*) van NEWORDERS waarbij de VERZENDDATUM NULL is
PollToReadData | Selecteer \* in NEWORDERS waar de VERZENDDATUM leeg voor UPDATE is
PollToAlterData | <no value specified>


Ook kunt u een trigger logica app om te peilen, lezen en wijzigen van gegevens in een DB2-tabel met behulp van een API Poll samengestelde gegevensbewerking definiëren. Zo vindt u een of meer nieuwe volgorde de records voor klanten, de rijwaarden (vóór de update) geselecteerde records retourneren logica App bijwerken. De instellingen voor de DB2-pakket/app ziet er als volgt:

    App-instelling | Waarde
--- | --- | ---
PollToCheckData | SELECT COUNT (\*) van NEWORDERS waarbij de VERZENDDATUM NULL is
PollToReadData | Selecteer \* in NEWORDERS waar de VERZENDDATUM leeg voor UPDATE is
PollToAlterData | UPDATE NEWORDERS SET VERZENDDATUM = huidige datum waarop huidige van &lt;CURSOR&gt;


Bovendien kunt u een trigger logica app om te peilen, lezen en verwijderen van gegevens uit een DB2-tabel met behulp van een API Poll samengestelde gegevensbewerking definiëren. Zo vindt u een of meer nieuwe order klantrecords, verwijder de rijen, de geselecteerde (vóór verwijderen) records als resultaat naar de toepassing van de logica. De instellingen voor de DB2-pakket/app ziet er als volgt:

    App-instelling | Waarde
--- | --- | ---
PollToCheckData | SELECT COUNT (\*) van NEWORDERS waarbij de VERZENDDATUM NULL is
PollToReadData | Selecteer \* in NEWORDERS waar de VERZENDDATUM leeg voor UPDATE is
PollToAlterData | VERWIJDEREN NEWORDERS waar van huidige &lt;CURSOR&gt;

In dit voorbeeld zal app logica controleren, lezen, bijwerken en vervolgens opnieuw de gegevens in de DB2-tabel te lezen.

1. Selecteer in de Azure startboard **+** (plusteken) **Web + Mobile**en **logica app**.
2. Voer de naam (bijvoorbeeld ' ShipOrdersDb2'), App Service plannen, andere eigenschappen en selecteer **maken**.
3. Selecteer in de Azure startboard de logica app die u zojuist hebt gemaakt, **Instellingen**en vervolgens **Triggers en acties**.
4. Selecteer **maken** binnen de app logica sjablonen in de blade Triggers en acties.
5. In het deelvenster API Apps **DB2 verbindingslijn**selecteert, stelt u een frequentie-interval en vervolgens **ingeschakeld**.
6. In het deelvenster API Apps **DB2 verbindingslijn**selecteert, vouw de lijst van bewerkingen te selecteren **uit de NEWORDERS**.
7. Schakel het **selectievakje** opslaan van de actie-instellingen en klik op **Opslaan**. De instellingen moeten er als volgt uitzien:  
![][10]  
8. Klik om te sluiten van de bladeserver **Triggers en acties** , en klik vervolgens op om de bladeserver **Instellingen** te sluiten.
9. In de lijst **alle wordt uitgevoerd** onder **bewerkingen**, klikt u op het item eerst vermeld (meest recente uitvoeren).
10. Klik op **actie** -item in de blade **logica app uitvoeren** .
11. Klik op de **Koppeling van de UITGANGEN**in de blade **logische app actie** . De uitvoer ziet er als volgt uit:  
![][11]


## <a name="logic-app-with-db2-connector-action-to-remove-data"></a>Logica app met DB2 connector actie om gegevens te verwijderen ##
U kunt een logische app actie om gegevens te verwijderen uit een DB2-tabel met behulp van een API verwijderen of Post entiteit OData-bewerking definiëren. Zo kunt u een nieuwe klantrecord volgorde door de verwerking van een SQL INSERT-instructie ten opzichte van een tabel met een identiteitskolom worden gedefinieerd als het resultaat de waarde of de rijen aan de app logica (ORDID van uiteindelijke tabel selecteren (invoegen in NWIND. WAARDEN VAN NEWORDERS (CUSTID, VERZENDNAAM, SHIPADDR, VERZENDPLAATS, SHIPREG, SHIPZIP) (?,?,?,?,?,?))).

## <a name="create-logic-app-using-db2-connector-to-remove-data"></a>Logica app met DB2-connector voor het verwijderen van gegevens maken ##
U kunt een nieuwe logica-app op de markt Azure maken en vervolgens de DB2-connector gebruiken als een actie voor het verwijderen van orders van klanten. Bijvoorbeeld, kunt u de DB2-connector voorwaardelijke bewerking voor het verwerken van een SQL DELETE-instructie (verwijderen uit NEWORDERS waar ORDID > = 10000).

1. Klik in het menu hub van het bord Azure **Start** op **+** (plusteken) klikt u op het **Web + Mobile**en klik op **logica app**. 
2. Typ een **naam**, bijvoorbeeld **RemoveOrdersDb2**in het blad **app logica maken** .
3. Selecteren of waarden definiëren voor de andere instellingen (bv. serviceplan, resourcegroep).
4. De instellingen er als volgt uit. Klik op **maken**:  
![][12]  
5. Klik in de blade **Instellingen** **Triggers en acties**.
6. Klik in de blade **Triggers en acties** in de lijst **logica app sjablonen** **maken helemaal**.
7. Klik op **Terugkeerpatroon**in het blad **Triggers en acties** in het deelvenster **API Apps** in de resourcegroep.
8. Klik op het item van het **Terugkeerpatroon** in de ontwerpweergave logica app, stelt u een **frequentie** en **Interval**, bijvoorbeeld **dagen** en **1**, en klik op het **vinkje** om de instellingen voor terugkeerpatroon item opslaan.
9. Klik in de blade **Triggers en acties** in het deelvenster **API Apps** in de resourcegroep **DB2-connector**.
10. In de ontwerpweergave logica app klikt u op de **verbindingslijn DB2** actie-item, klikt u op de weglatingstekens (**...**), vouw de lijst bewerkingen en klik op **voorwaardelijke verwijderen van N**.
11. Typ op het actiepunt DB2 connector **ge ORDID 10000** voor een **expressie die een subset van gegevens wordt geïdentificeerd**.
12. Klik op het **vinkje** als de actie-instellingen wilt opslaan en klik vervolgens op **Opslaan**. De instellingen moeten er als volgt uitzien:  
![][13]  
13. Klik om te sluiten van de bladeserver **Triggers en acties** , en klik vervolgens op om de bladeserver **Instellingen** te sluiten.
14. In de lijst **alle wordt uitgevoerd** onder **bewerkingen**, klikt u op het item eerst vermeld (meest recente uitvoeren).
15. Klik op **actie** -item in de blade **logica app uitvoeren** .
16. Klik op de **Koppeling van de UITGANGEN**in de blade **logische app actie** . De uitvoer ziet er als volgt uit:  
![][14]

**Opmerking:** Logica app designer tabelnamen worden afgekapt. Bijvoorbeeld is de **voorwaarde verwijderen uit NEWORDERS** bewerking **voorwaardelijk**verwijderen van N afgekapt.


> [AZURE.TIP] Gebruik de volgende SQL-instructies voor het maken van de voorbeeldtabel en opgeslagen procedures. 

U kunt de tabel NEWORDERS met behulp van de volgende DB2-SQL DDL-instructies maken:
 
    CREATE TABLE ORDERS (  
        ORDID INT NOT NULL GENERATED BY DEFAULT AS IDENTITY (START WITH 10000, INCREMENT BY 1) ,  
        CUSTID INT NOT NULL ,  
        EMPID INT NOT NULL DEFAULT 10000 ,  
        ORDDATE DATE NOT NULL DEFAULT CURRENT DATE ,  
        REQDATE DATE DEFAULT CURRENT DATE ,  
        SHIPDATE DATE ,  
        SHIPID INT NOT NULL DEFAULT 10000,  
        FREIGHT DECIMAL (9,2) NOT NULL DEFAULT 0.00 ,  
        SHIPNAME CHAR (40) NOT NULL ,  
        SHIPADDR CHAR (60) NOT NULL ,  
        SHIPCITY CHAR (20) NOT NULL ,  
        SHIPREG CHAR (15) NOT NULL ,  
        SHIPZIP CHAR (10) NOT NULL ,  
        SHIPCTRY CHAR (15) NOT NULL DEFAULT 'USA' ,  
        PRIMARY KEY(ORDID)  
        )  
 
    CREATE UNIQUE INDEX XORDID ON ORDERS (ORDID ASC)  



U kunt het voorbeeld SPOERID opgeslagen procedure met behulp van de volgende DB2 DDL-instructie maken:
 
    CREATE OR REPLACE PROCEDURE NWIND.SPORDERID (IN ORDERID VARCHAR(128))  
        DYNAMIC RESULT SETS 1  
    P1: BEGIN  
        DECLARE CURSOR1 CURSOR WITH RETURN FOR  
            SELECT * FROM NWIND.NEWORDERS  
                WHERE ORDID = ORDERID;  
        OPEN CURSOR1;  
    END P1  
    ') 


## <a name="hybrid-configuration-optional"></a>Hybride configuratie (optioneel)

> [AZURE.NOTE] Deze stap is alleen vereist als u achter uw firewall DB2-connector op de gebouwen gebruikt.

App-Service gebruikt de hybride Configuration Manager om een veilige verbinding met uw systeem op locatie. Als connector een op ruimten IBM DB2 Server voor Windows gebruikt, is de hybride Connection Manager vereist.

Zie [werken met de hybride Connection Manager](app-service-logic-hybrid-connection-manager.md).


## <a name="do-more-with-your-connector"></a>Doe meer met uw connector
Nu dat de verbindingslijn is gemaakt, kunt u deze toevoegen aan een zakelijke werkstroom met een app logica. Zie [Wat zijn apps logica?](app-service-logic-what-are-logic-apps.md).

De API Apps met REST API's maken. Zie [verbindingslijnen en API-Naslaggids voor Apps](http://go.microsoft.com/fwlink/p/?LinkId=529766).

U kunt ook statistieken en beheer van zekerheid aan de verbindingslijn bekijken. Zie [de ingebouwde API Apps en de verbindingen controleren en beheren](app-service-logic-monitor-your-connectors.md).


<!--Image references-->
[1]: ./media/app-service-logic-connector-db2/ApiApp_Db2Connector_Create.png
[2]: ./media/app-service-logic-connector-db2/LogicApp_NewOrdersDb2_Create.png
[3]: ./media/app-service-logic-connector-db2/LogicApp_NewOrdersDb2_TriggersActions.png
[4]: ./media/app-service-logic-connector-db2/LogicApp_NewOrdersDb2_Outputs.png
[5]: ./media/app-service-logic-connector-db2/LogicApp_NewOrdersBulkDb2_Create.png
[6]: ./media/app-service-logic-connector-db2/LogicApp_NewOrdersBulkDb2_TriggersActions.png
[7]: ./media/app-service-logic-connector-db2/LogicApp_NewOrdersBulkDb2_Inputs.png
[8]: ./media/app-service-logic-connector-db2/LogicApp_NewOrdersBulkDb2_Outputs.png
[9]: ./media/app-service-logic-connector-db2/LogicApp_UpdateOrdersDb2_Create.png
[10]: ./media/app-service-logic-connector-db2/LogicApp_UpdateOrdersDb2_TriggersActions.png
[11]: ./media/app-service-logic-connector-db2/LogicApp_UpdateOrdersDb2_Outputs.png
[12]: ./media/app-service-logic-connector-db2/LogicApp_RemoveOrdersDb2_Create.png
[13]: ./media/app-service-logic-connector-db2/LogicApp_RemoveOrdersDb2_TriggersActions.png
[14]: ./media/app-service-logic-connector-db2/LogicApp_RemoveOrdersDb2_Outputs.png

