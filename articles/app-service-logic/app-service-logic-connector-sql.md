<properties
   pageTitle="Met de SQL-Connector in logica Apps | Microsoft Azure App Service"
   description="Het maken en configureren van de SQL-Connector of API-app en gebruiken in een logica in Azure App Service app"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="anuragdalmia"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="09/01/2016"
   ms.author="sameerch"/>


# <a name="get-started-with-the-microsoft-sql-connector-and-add-it-to-your-logic-app"></a>Aan de slag met de Microsoft SQL-Connector en toevoegen aan uw App logica
>[AZURE.NOTE] Deze versie van het artikel is van toepassing op de schemaversie van logica apps 2014-12-01-voorbeeld. Klik op [SQL Azure API](../connectors/connectors-create-api-sqlazure.md)voor de schemaversie Azure SQL 2015-08-01-voorbeeld.

Verbinding maken met een locatie in SQL Server of een Azure SQL-Database te maken en wijzigen van uw gegevens of gegevens. Verbindingslijnen kunnen worden gebruikt in de logica Apps te halen, proces, of als onderdeel van een 'workflow' push-gegevens. Wanneer u de SQL-Connector in uw werkstroom gebruikt, kunt u verschillende scenario's te bereiken. U kunt bijvoorbeeld:

- Een gedeelte van de gegevens die zich in de SQL-database met behulp van een web of mobiele toepassing worden blootgesteld.
- Gegevens invoegen in een SQL-databasetabel voor opslag. Bijvoorbeeld: u kunt records van werknemers invoeren, verkooporders bijwerken, enzovoort.
- Gegevens ophalen uit SQL en gebruiken in een bedrijfsproces. Bijvoorbeeld, kunt u records voor klanten krijgt en die klantrecords in televergaderingen.

U kunt de SQL-Connector toevoegen aan workflow- en bedrijfsgegevens als onderdeel van deze werkstroom in een App logica. 

## <a name="triggers-and-actions"></a>Triggers en acties
*Triggers* zijn gebeurtenissen die plaatsvinden. Bijvoorbeeld, wordt wanneer een order wordt bijgewerkt of wanneer u een nieuwe klant toegevoegd. Een *actie* is het resultaat van de trigger. Bijvoorbeeld, wanneer een order wordt bijgewerkt, een waarschuwing verzenden aan de verkoper. Of wanneer een nieuwe klant wordt toegevoegd, stuur een welkomstbericht aan de nieuwe klant.

De SQL-Connector kan worden gebruikt als een trigger of een actie in een logica app en ondersteunt de gegevens in JSON en XML-indelingen. Voor elke tabel in het pakket opgenomen instellingen (meer hierover verderop in dit onderwerp), er is een reeks acties JSON en een set XML-acties.

De SQL-Connector heeft de volgende Triggers en acties beschikbaar:

Triggers | Acties
--- | ---
Poll gegevens | <ul><li>In tabel invoegen</li><li>Tabel bijwerken</li><li>Selecteer in de tabel</li><li>Uit tabel verwijderen</li><li>Opgeslagen Procedure aanroepen</li></ul>

## <a name="create-the-sql-connector"></a>Maak de SQL-Connector

Een verbindingslijn kan worden gemaakt binnen een logica app of rechtstreeks vanuit de markt Azure worden gemaakt. Een verbindingslijn maakt de markt:  

1. Selecteer in de Azure startboard **Marketplace**.
2. 'SQL Connector' zoekt, selecteert u het en selecteer **maken**.
3. Voer de naam van de App Service plannen en andere eigenschappen.
4. Geef de volgende pakketinstellingen:

    Naam | Vereist |  Beschrijving
--- | --- | ---
Naam van de server | Ja | Voer de naam van de SQL Server. Voer bijvoorbeeld *SQL Server/sqlexpress* of *SQLserver.mydomain.com*.
Poort | Nee | De standaardwaarde is 1433.
Gebruikersnaam | Ja | Voer de gebruikersnaam van een zich kan aanmelden in de SQL-Server. Als u verbinding maakt met een SQL-Server op locatie, kunt u SQL-verificatie referenties invoeren.
Wachtwoord | Ja | Voer de gebruikersnaam wachtwoord.
Naam van de database | Ja | Geef de database die u verbinding wilt maken. U kunt bijvoorbeeld *klanten* of *dbo/orders*invoeren.
On-Premises | Ja | De standaardwaarde is False. Geef False als verbinding maakt met een Azure SQL-database. Typ True als u verbinding maakt met een SQL-Server op locatie.
Service Bus verbindingsreeks | Nee | Als u verbinding met een op gebouwen maakt, voert u de verbindingsreeks Service Bus relay.<br/><br/>[Met behulp van de hybride Connection Manager](app-service-logic-hybrid-connection-manager.md)<br/>[Prijzen van Bus-service](https://azure.microsoft.com/pricing/details/service-bus/)
Partnernaam | Nee | Als de primaire server niet beschikbaar is, kunt u een partnerserver als een back-up- of alternatieve server.
Tabellen | Nee | Een overzicht van de databasetabellen die kunnen worden bijgewerkt door de connector. Voer bijvoorbeeld *OrdersTable* of *EmployeeTable*. Als er geen tabellen worden ingevoerd, kunnen alle tabellen worden gebruikt. Geldige tabellen en/of opgeslagen Procedures moet deze connector gebruiken als een actie.
Opgeslagen Procedures | Nee | Voer een bestaande opgeslagen procedure die kan worden aangeroepen door de connector. Voer bijvoorbeeld *sp_IsEmployeeEligible* of *sp_CalculateOrderDiscount*. Geldige tabellen en/of opgeslagen Procedures moet deze connector gebruiken als een actie.
Gegevensquery beschikbaar | Voor ondersteuning van de trigger | SQL-instructie om te bepalen of er gegevens beschikbaar is voor een SQL Server-databasetabel polling. Dit moet een numerieke waarde die het aantal rijen van de beschikbare gegevens retourneren. Voorbeeld: SELECT COUNT(*) uit table_name.
Poll Data Query | Voor ondersteuning van de trigger | De SQL-instructie op te vragen voor de SQL Server-databasetabel. U kunt een onbeperkt aantal SQL-instructies die zijn gescheiden door een puntkomma. Deze instructie is uitgevoerd transactioneel en alleen vastgelegd wanneer de gegevens veilig in uw app logica opgeslagen worden. Voorbeeld: Kies *table_name; VERWIJDEREN van table_name. <br/>**Note**<br/>U dient een poll instructie een lus voorkomt door verwijderen, verplaatsen of het bijwerken van de geselecteerde gegevens om ervoor te zorgen dat dezelfde gegevens opnieuw wordt niet gecontroleerd.

5. Wanneer voltooid, er de instellingen van het pakket met de volgende strekking:  
![][1]  

6. Selecteer **maken**. 


## <a name="use-the-connector-as-a-trigger"></a>De verbindingslijn gebruiken als een Trigger
We bekijken een eenvoudige logica app die gegevens uit een SQL-tabel worden opgevraagd, worden de gegevens in een andere tabel en gegevens moeten worden bijgewerkt.

Geef waarden op **Data beschikbaar Query** en **Poll Data Query** om de SQL-connector gebruikt als een trigger. **Gegevens beschikbaar Query** wordt uitgevoerd op de planning die u opgeeft en bepaalt of er gegevens beschikbaar is. Aangezien deze query alleen een scalaire waarde retourneert, kan het worden afgestemd en geoptimaliseerd voor frequent worden uitgevoerd.

**Poll Data Query** wordt alleen uitgevoerd wanneer de gegevens beschikbaar Query geeft aan dat gegevens beschikbaar. Met deze instructie wordt uitgevoerd binnen een transactie en wordt alleen doorgevoerd als de opgehaalde gegevens blijvend worden opgeslagen in uw workflow. Het is belangrijk om te voorkomen dat steeds opnieuw dezelfde gegevens wordt uitgepakt. De transactionele aard van deze uitvoering kan worden gebruikt om te verwijderen of bijwerken van de gegevens om dat deze gegevens wordt gevraagd zodra wordt niet verzameld.

> [AZURE.NOTE] Het schema die wordt geretourneerd door deze instructie geeft de beschikbare eigenschappen in de connector. Alle kolommen moeten worden genoemd.

#### <a name="data-available-query-example"></a>Voorbeeld van de beschikbare Query gegevens

    SELECT COUNT(*) FROM [Order] WHERE OrderStatus = 'ProcessedForCollection'

#### <a name="poll-data-query-example"></a>Voorbeeld van de poll gegevens Query

    SELECT *, GetData() as 'PollTime' FROM [Order]
        WHERE OrderStatus = 'ProcessedForCollection'
        ORDER BY Id DESC;
    UPDATE [Order] SET OrderStatus = 'ProcessedForFrontDesk'
        WHERE Id =
        (SELECT Id FROM [Order] WHERE OrderStatus = 'ProcessedForCollection' ORDER BY Id DESC)

### <a name="add-the-trigger"></a>De Trigger toevoegen
1. Bij het maken of bewerken van een app logica, selecteert u de SQL-Connector die u hebt gemaakt als trigger. Hier worden de beschikbare triggers: **Poll gegevens (JSON)** en **Poll-gegevens (XML)**:  
![][5]

2. Selecteer de trigger **Poll gegevens (JSON)** , voer de frequentie en de ✓ op:  
![][6]

3. De trigger wordt nu weergegeven in de app logica worden geconfigureerd. De output(s) van de trigger worden weergegeven en kan worden gebruikt als input in de volgende acties:  
![][7]

## <a name="use-the-connector-as-an-action"></a>Gebruik van de Connector als een actie
Met behulp van onze eenvoudige logica app scenario die controleert of de gegevens uit een SQL-tabel, worden de gegevens in een andere tabel toegevoegd en werkt u de gegevens.

Voor het gebruik van de SQL-Connector als een actie, voer de naam van de tabellen en/of opgeslagen Procedures die u hebt opgegeven bij het maken van de SQL-Connector:

1. Nadat de trigger (of kies 'Deze logica handmatig uitvoeren'), de SQL-connector die u hebt gemaakt in de galerie toevoegen. Selecteer een van de acties, invoegen, zoals *Invoegen in TempEmployeeDetails (JSON)*:  
![][8]

2. De ingevoerde waarden van de record moet worden ingevoegd en klik op de ✓ invoeren:  
![][9]

3. Selecteer de galerie dezelfde SQL-aansluiting die u hebt gemaakt. Als een actie, selecteert u de actie bijwerken op dezelfde tabel, zoals *De EmployeeDetails Update*:  
![][11]

4. De invoerwaarden voor de bijwerkactie, en klik op de ✓:  
![][12]

U kunt de app logica testen door een nieuwe record toe te voegen in de tabel die moet worden doorzocht kunnen worden.

## <a name="what-you-can-and-cannot-do"></a>Wat u wel en niet kan doen

SQL-Query | Ondersteund | Niet ondersteund
--- | --- | ---
Waar clausule | <ul><li>Operatoren: En, of, =, <>, <, < =, >, > = en zoals</li><li>Meerdere sub-voorwaarden kunnen worden gecombineerd door "(" en")"</li><li>Letterlijke tekenreeksen, datum/tijd (tussen enkele aanhalingstekens), getallen (mag alleen numerieke tekens bevatten)</li><li>Strikt moet een expressie binaire indeling, zoals ((operand operator operand) en/of (operand operand operator)) *</li></ul> | <ul><li>Operatoren: Tussen IN</li><li>Alle ingebouwde functies, zoals ADD(), MAX(), nu() en POWER()</li><li>Rekenkundige operatoren, zoals *, -, +, enzovoort</li><li>Samenvoegingen van tekenreeksen met +.</li><li>Alle Joins</li><li>IS NULL en IS niet Null</li><li>Getallen met een niet-numerieke tekens, zoals hexadecimale getallen</li></ul>
Velden (in de Select-query) | <ul><li>Geldige kolomnamen, gescheiden door komma's. Geen naam tabel voorvoegsels toegestaan (de connector werkt op één tabel tegelijk).</li><li>Namen kunnen worden voorafgegaan door ' [' en ']'</li></ul> | <ul><li>Trefwoorden zoals boven en DISTINCT</li><li>Aliasing zoals straat, plaats + Zip als adres</li><li>Alle ingebouwde functies, zoals ADD(), MAX(), nu() en POWER()</li><li>Rekenkundige operatoren, zoals *, -, +, enzovoort</li><li>Samenvoegingen van tekenreeksen met +</li></ul>

#### <a name="tips"></a>Tips

- Het is raadzaam maken van een opgeslagen procedure uitvoeren met de execute opgeslagen procedure API voor geavanceerde zoekopdrachten.
- Wanneer u de inner query's, kunt u deze in opgeslagen procedures gebruiken.
- Voor deelname aan meerdere voorwaarden, kunt u 'En' en 'OR'-operatoren.

## <a name="hybrid-configuration-optional"></a>Hybride configuratie (optioneel)

> [AZURE.NOTE] Deze stap is alleen vereist als u van SQL Server op lokalen achter uw firewall gebruikmaakt.

App-Service gebruikt de hybride Configuration Manager om een veilige verbinding met uw systeem op locatie. Als u een connector gebruikt een SQL-Server op locatie bent, is de hybride Connection Manager vereist.

> [AZURE.NOTE] Als u aan de slag met Azure logica Apps wilt voor het aanmelden voor een account met Azure, gaat u naar de [Logica App proberen](https://tryappservice.azure.com/?appservice=logic), waar u direct een tijdelijk starter logica app in de App-Service maken kunt. Geen creditcard vereist; geen verplichtingen.

Zie [werken met de hybride Connection Manager](app-service-logic-hybrid-connection-manager.md).


## <a name="do-more-with-your-connector"></a>Doe meer met uw Connector
Nu dat de verbindingslijn is gemaakt, kunt u deze toevoegen aan een zakelijke werkstroom met een App logica. Zie [Wat zijn Apps logica?](app-service-logic-what-are-logic-apps.md).

De Swagger REST API reference op [verbindingslijnen en API-Naslaggids voor Apps](http://go.microsoft.com/fwlink/p/?LinkId=529766)bekijken.

U kunt ook statistieken en beheer van zekerheid aan de verbindingslijn bekijken. Zie [de ingebouwde API Apps en de verbindingen controleren en beheren](app-service-logic-monitor-your-connectors.md).


<!--Image references-->
[1]: ./media/app-service-logic-connector-sql/Create.png
[5]: ./media/app-service-logic-connector-sql/LogicApp1.png
[6]: ./media/app-service-logic-connector-sql/LogicApp2.png
[7]: ./media/app-service-logic-connector-sql/LogicApp3.png
[8]: ./media/app-service-logic-connector-sql/LogicApp4.png
[9]: ./media/app-service-logic-connector-sql/LogicApp5.png
[10]: ./media/app-service-logic-connector-sql/LogicApp6.png
[11]: ./media/app-service-logic-connector-sql/LogicApp7.png
[12]: ./media/app-service-logic-connector-sql/LogicApp8.png
