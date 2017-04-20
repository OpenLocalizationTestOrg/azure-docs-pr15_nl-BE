<properties
    pageTitle="Schaal out met Azure SQL-Database | Microsoft Azure"
    description="Software als een Service (SaaS)-ontwikkelaars gemakkelijk elastisch, schaalbare databases kunt maken in de cloud met deze hulpmiddelen"
    services="sql-database"
    documentationCenter=""
    manager="jhubbard"
    authors="ddove"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="sql-database"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="ddove"/>

# <a name="scaling-out-with-azure-sql-database"></a>Geschaald uitbreiden met Azure SQL-Database

U kunt eenvoudig schalen van Azure SQL-databases met behulp van de **Elastische Database** tools. Deze hulpprogramma's en functies kunnen u de database vrijwel onbeperkte middelen van **Azure SQL-Database** gebruiken om oplossingen voor transactionele belasting en met name Software als een Service (SaaS)-toepassingen te maken. Elastische databasefuncties bestaan uit een van de volgende opties:

* [Elastische Database client library](sql-database-elastic-database-client-library.md): de client library is een functie waarmee u kunt maken en onderhouden van een laptopgeheugen van databases.  Zie [aan de slag met elastische Databasehulpmiddelen](sql-database-elastic-scale-get-started.md).
* [Gereedschap voor elastische Database splitsen samenvoegen](sql-database-elastic-scale-overview-split-and-merge.md): verplaatst gegevens tussen een laptopgeheugen van databases. Dit is handig voor het verplaatsen van gegevens uit een database met meerdere huurder naar een enkele huurder database (of omgekeerd). Zie [elastische database splitsen samenvoegen tool zelfstudie](sql-database-elastic-scale-configure-deploy-split-and-merge.md).
* [Elastische Database taken](sql-database-elastic-jobs-overview.md) (voorbeeld): taken gebruiken voor het beheren van een groot aantal Azure SQL-databases. Administratieve bewerkingen zoals schemawijzigingen, beheer van referenties, verwijzing bijwerkt, prestatiegegevens verzamelen of huurder (klant) telemetrie-collectie met behulp van taken eenvoudig uitvoeren.
* [Elastische databasequery](sql-database-elastic-query-overview.md) (voorbeeld): Hiermee kunt u een Transact-SQL-query die meerdere databases omvat uit te voeren. Hiermee kunt verbinding met de hulpprogramma's voor rapportage zoals Excel, PowerBI, Tableau, enz.
* [Elastische transacties](sql-database-elastic-transactions-overview.md): deze functie kunt u transacties die zich uitstrekken over meerdere databases in Azure SQL-Database uitvoeren. Elastische databasetransacties zijn beschikbaar voor .NET-toepassingen met behulp van ADO .NET en integreren met de bekende programmeren met behulp van de [System.Transaction-klassen](https://msdn.microsoft.com/library/system.transactions.aspx).

De onderstaande afbeelding ziet u een architectuur die de **elastische Database functies** met betrekking tot een van de databases-collectie bevat.

In deze afbeelding vertegenwoordigen de kleuren van de database schema's. Databases met dezelfde kleur delen hetzelfde schema.

1. Een set van **Azure SQL databases** worden gehost op sharding architectuur met Azure.
2. De **elastische Database client-bibliotheek** wordt gebruikt voor het beheren van een set shard.
3. Een subset van de databases worden in een **groep met elastische Database**geplaatst. (Zie [Wat is een groep met?](sql-database-elastic-pool.md)).
4. Een **elastische Database taak** wordt uitgevoerd op een geplande of ad hoc-T-SQL-scripts op alle databases.
5. De **gesplitste samenvoegen tool** wordt gebruikt om gegevens te verplaatsen vanuit een shard naar de andere.
6. De **elastische Database query** kunt u een query schrijven die alle databases in de set shard beslaat.
7. **Elastische transacties** kunt u transacties die zich uitstrekken over meerdere databases worden uitgevoerd. 


![Elastische hulpmiddelen voor databases][1]


## <a name="why-use-the-tools"></a>Waarom de hulpprogramma's gebruiken?

Belangengroepen elasticiteit en schaal voor cloud-toepassingen eenvoudig voor VMs en blob-opslag is gewoon toevoegen aftrekken eenheden of vermogen te verhogen. Maar het bleef een uitdaging voor stateful verwerking van gegevens in relationele databases. Uitdagingen ontstaan in deze scenario's:

* Vergroten en verkleinen van de capaciteit voor het deel van de relationele database van uw werkbelasting.
* Hotspots die zich kunnen voordoen op het gebied van een bepaalde subset van gegevens – zoals een bijzonder drukke eind-klant (huurder) beheren.

Traditioneel is scenario's zoals deze behandeld door te investeren in een grotere databaseservers ter ondersteuning van de toepassing. Deze optie is echter beperkt in de cloud waarbij de verwerking op vooraf gedefinieerde goederen hardware gebeurt. In plaats daarvan biedt distribueren van gegevens en verwerkt in veel databases met dezelfde structuur (scale-out patroon "sharding" genoemd) een alternatief voor traditionele opschaling benaderingen, zowel in termen van kosten en elasticiteit.

## <a name="horizontal-and-vertical-scaling"></a>Horizontale en verticale schaal

In de volgende afbeelding ziet u de horizontale en verticale afmetingen van de schaal, worden de manieren waarop die de elastische databases kunnen worden geschaald.

![Horizontale en verticale Scaleout][2]

Horizontaal schalen verwijst naar het toevoegen of verwijderen van databases te passen capaciteit of de algehele prestaties. Dit wordt ook wel genoemd 'schalen' uit. Sharding, waarin gegevens over een identiek gestructureerde databases, collectie is gepartitioneerd is een veelgebruikte methode voor het implementeren van horizontaal schalen.  

Verticaal schalen verwijst naar verhogen of verlagen van het prestatieniveau van een afzonderlijke database: dit wordt ook wel "schaalt."

De meeste databasetoepassingen voor cloud-schaal gebruikt een combinatie van deze twee strategieën. Bijvoorbeeld, kunt een Software als een Service-toepassing horizontaal schalen om nieuwe eindklanten inrichten en verticaal schalen dat elke eindgebruiker database groter of kleiner bronnen door de werklast.

* Horizontaal schalen wordt beheerd met behulp van de [elastische Database client library](sql-database-elastic-database-client-library.md).

* Azure PowerShell-cmdlets gebruiken om te wijzigen van de opgegeven service, verticaal schalen moet worden uitgevoerd of door het plaatsen van databases in een elastische toepassingen.

## <a name="sharding"></a>Sharding

*Sharding* is een techniek om grote hoeveelheden gegevens identiek gestructureerd verdelen over een aantal onafhankelijke databases. Het is vooral populair met cloud-ontwikkelaars maken van Software als een Service (SAAS)-aanbiedingen voor eind-klanten en bedrijven. Deze end-klanten zijn vaak "huurders" genoemd. Sharding kunnen nodig zijn voor een aantal redenen:  

* De totale hoeveelheid gegevens is te groot om te passen binnen de beperkingen van één database
* De transactiedoorvoer van de totale belasting groter is dan de mogelijkheden van één enkele database
* Huurders mogelijk fysieke afscherming van elkaar, zodat afzonderlijke databases nodig zijn voor elke huurder
* Verschillende delen van een database moet zich bevinden in verschillende locaties voor conformiteit, prestaties of geopolitieke redenen.

In andere scenario's, zoals ingestie van gegevens van gedistribueerde apparaten, worden sharding in te vullen, een set van databases die zijn geordend ondersteuningsbeheerder gebruikt. Een afzonderlijke database kan bijvoorbeeld worden toegewezen aan elke dag of week. In dat geval kan de sleutel sharding een geheel getal dat de datum (aanwezig in alle rijen van de tabellen een laptopgeheugen) en query's op te halen voor een datumbereik dat moeten worden gerouteerd door de toepassing op de deelverzameling van databases die betrekking hebben op het bereik in kwestie.

Sharding werkt het best wanneer elke transactie in een toepassing beperkt tot een enkele waarde van een sleutel sharding worden kan. Dat zorgt ervoor dat alle transacties wordt lokaal op een specifieke database.

## <a name="multi-tenant-and-single-tenant"></a>Meerdere huurder en één huurder

Sommige toepassingen gebruiken de eenvoudigste oplossing voor het maken van een afzonderlijke database voor elke huurder. Dit is het **patroon van één huurder sharding** isolatie, mogelijkheid voor back-up en terugzetten en resource schaal de granulatie van de huurder. Elke database is gekoppeld aan een bepaalde huurder id-waarde (of een waarde van de klant) met één huurder sharding, maar die sleutel hoeft niet altijd aanwezig zijn in de gegevens zelf. Het is de verantwoordelijkheid van de toepassing voor het routeren van elke aanvraag naar de juiste database – en de clientbibliotheek dit kunt vereenvoudigen.

![Één huurder versus meerdere huurder][4]

Andere huurders van meerdere scenario's samen pack in databases in plaats van ze te isoleren in afzonderlijke databases. Dit is een typisch **patroon met meerdere huurder sharding** – en het mag worden aangedreven door het feit dat een groot aantal zeer kleine huurders worden beheerd door een toepassing. In meerdere huurder sharding, zijn de rijen in de databasetabellen allemaal bedoeld voor het uitvoeren van een identificatie van de huurder-ID of sharding sleutel. Nogmaals, de toepassingslaag is verantwoordelijk voor de aanvraag van een huurder routering naar de juiste database en kan dit worden ondersteund door de elastische database client library. Beveiliging kan bovendien filter welke rijen toegang elke huurder – voor meer informatie tot kunt Zie [huurder met meerdere toepassingen met elastische Databasehulpmiddelen en beveiliging](sql-database-elastic-tools-multi-tenant-row-level-security.md)worden gebruikt. Gegevens tussen databases die nodig zijn met het patroon met meerdere huurder sharding en dit wordt vergemakkelijkt door het hulpprogramma elastische database splitsen samenvoegen. Zie voor meer informatie over ontwerppatronen voor SaaS-toepassingen met elastische van toepassingen, [Patronen ontwerpen voor SaaS-toepassingen met meerdere huurder met Azure SQL-Database](sql-database-design-patterns-multi-tenancy-saas-applications.md).

### <a name="move-data-from-multiple-to-single-tenancy-databases"></a>Verplaatsen van gegevens uit meerdere databases één pacht

Bij het maken van een SaaS-toepassing, is het typische aan te bieden potentiële klanten een evaluatieversie van de software. In dit geval is het efficiënt gebruik van een database met meerdere huurder voor de gegevens. Wanneer een prospect een klant is, is een database met één huurder echter beter omdat het biedt betere prestaties. Als de klant had gemaakt van gegevens tijdens de proefperiode, gebruiken de [gesplitste samenvoegen tool](sql-database-elastic-scale-overview-split-and-merge.md) om de gegevens van de huurder met meerdere verplaatsen naar de nieuwe database voor één huurder.

## <a name="next-steps"></a>Volgende stappen

Zie voor een monster app waarop u de clientbibliotheek, [aan de slag met elastische Datababase's](sql-database-elastic-scale-get-started.md).

Zie converteren van bestaande databases om de services [migreren van bestaande databases te schalen](sql-database-elastic-convert-to-use-elastic-tools.md).

Zie de specifieke kenmerken van de elastische Database-toepassingen, [Overwegingen voor prijs- prestatieverhouding voor een elastische database toepassingen](sql-database-elastic-pool-guidance.md)zien of een nieuwe groep maken met de [zelfstudie](sql-database-elastic-pool-create-portal.md).  

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Anchors-->
<!--Image references-->
[1]:./media/sql-database-elastic-scale-introduction/tools.png
[2]:./media/sql-database-elastic-scale-introduction/h_versus_vert.png
[3]:./media/sql-database-elastic-scale-introduction/overview.png
[4]:./media/sql-database-elastic-scale-introduction/single_v_multi_tenant.png

