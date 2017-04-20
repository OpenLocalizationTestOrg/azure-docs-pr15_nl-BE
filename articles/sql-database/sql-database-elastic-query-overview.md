<properties
    pageTitle="Azure SQL-Database elastische database query overzicht | Microsoft Azure"
    description="Overzicht van de functie query elastisch"    
    services="sql-database"
    documentationCenter=""  
    manager="jhubbard"
    authors="torsteng"/>

<tags
    ms.service="sql-database"
    ms.workload="sql-database"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="04/27/2016"
    ms.author="torsteng" />

# <a name="azure-sql-database-elastic-database-query-overview-preview"></a>Azure SQL-Database elastische database query overzicht (voorbeeld)

De elastische database query (in het voorbeeld) Hiermee kunt u een Transact-SQL-query uitvoert die meerdere databases in Azure SQL Database (SQLDB omvat). U kunt voor het uitvoeren van query's cross database toegang te krijgen tot externe tabellen en verbinding maken met Microsoft en derden programma's (Excel, PowerBI, Tableau, enz.) in lagen met meerdere databases gegevens opvragen. Met deze functie kunt u query's op grote lagen in de SQL-Database geschaald en visualiseren van de resultaten in rapporten van business intelligence (BI).

## <a name="documentation"></a>Documentatie

* [Aan de slag met cross-databasequery 's](sql-database-elastic-query-getting-started-vertical.md)
* [Rapport over schaalvergroting cloud-databases](sql-database-elastic-query-getting-started.md)
* [Query binnen een laptopgeheugen wolk databases (horizontaal partitioneren)](sql-database-elastic-query-horizontal-partitioning.md)
* [Query over cloud databases met verschillende schema's (gepartitioneerd verticaal)](sql-database-elastic-query-vertical-partitioning.md)
* [SP\_uitvoeren \_externe](https://msdn.microsoft.com/library/mt703714)


## <a name="why-use-elastic-queries"></a>Waarom elastische query's gebruiken?

**Azure SQL-Database**

Via Azure SQL-databases volledig in T-SQL query. Hierdoor kunnen alleen-lezen bij het controleren van externe databases. Dit is een optie voor huidige klanten van op gebouwen SQL Server voor het migreren van toepassingen met drie en vierkleuren part namen of gekoppelde server SQL DB.

**Beschikbaar op standaard laag** Elastische query wordt ondersteund op de Standard performance laag naast de laag Premium prestaties. Zie de sectie over beperkingen onder voorbeeld op prestaties beperkingen voor de lagere niveaus van prestaties.

**Push naar externe databases**

Elastische query's kunnen nu push-SQL-parameters met de externe databases voor uitvoering.

**Opgeslagen procedure kan worden uitgevoerd**

Extern opgeslagen procedure-aanroepen of externe functies met [sp\_uitvoeren \_externe](https://msdn.microsoft.com/library/mt703714).

**Flexibiliteit**

Externe tabellen met elastische query kunnen nu verwijzen naar externe tabellen met een ander schema of de naam van de tabel.

## <a name="elastic-database-query-scenarios"></a>Elastische database query-scenario 's

Het doel is om het opvragen scenario's waarbij meerdere databases rijen in één algemene resultaat bijdragen te vergemakkelijken. De query kan ofwel worden samengesteld door de gebruiker of de toepassing direct of indirect via hulpprogramma's die zijn verbonden met de database. Dit is vooral handig bij het maken van rapporten, met behulp van de opties voor commerciële BI of data-integratie, of een toepassing die niet kan worden gewijzigd. U kunt met een elastische query opvragen via verschillende databases met behulp van de vertrouwde SQL Server-connectiviteit ervaring in hulpprogramma's zoals Excel, PowerBI, Tableau of Cognos.
Een elastische query kunt eenvoudig toegang tot een volledige verzameling van databases via query's die zijn uitgegeven door SQL Server Management Studio of Visual Studio en vergemakkelijkt cross database opvragen vanaf entiteit Framework of andere ORM-omgevingen. In afbeelding 1 ziet u een scenario waarbij een bestaande wolk toepassing (met behulp van de [elastische database client library](sql-database-elastic-database-client-library.md)) is gebaseerd op een gegevenslaag schaalvergroting en een elastische query wordt gebruikt voor het melden van het cross-database.

**Figuur 1** Elastische databasequery op schaalvergroting van gegevens gebruikt

![Elastische databasequery op schaalvergroting van gegevens gebruikt][1]

Scenario's van klanten voor elastische query worden gekenmerkt door de volgende topologieën:

* **Verticale partitioneren: Cross-databasequery 's** (Topologie 1): de gegevens verticaal tussen een aantal databases in een gegevenslaag is gepartitioneerd. Meestal verschillende sets van tabellen bevinden zich op verschillende databases. Dit betekent dat het schema op verschillende databases verschilt. Bijvoorbeeld zijn alle tabellen voor de voorraad op de ene database alle accounting-gerelateerde tabellen zijn op een tweede database. Algemene use-cases met deze topologie moeten een query in of bij het samenstellen van rapporten over tabellen in verschillende databases.
* **Horizontaal partitioneren – Sharding** (Topologie 2): gegevens horizontaal om rijen verdelen over een schaal van gegevens is gepartitioneerd laag. Het schema is identiek voor alle deelnemende databases met deze benadering. Deze aanpak wordt ook 'sharding' genoemd. Sharding kan worden uitgevoerd en beheerd met behulp van (1) de database elastische extra bibliotheken of (2) self-sharding. Een elastisch-query wordt gebruikt voor query's of rapporten over veel shards verzamelen.

> [AZURE.NOTE] Elastische databasequery werkt het beste voor incidentele rapportage scenario's waar de meeste van de verwerking kan worden uitgevoerd op de gegevenslaag. Voor zware werklasten van rapportage of gegevens magazijnbeheer scenario's met meer complexe query's, ook overwegen [Azure SQL Data Warehouse](https://azure.microsoft.com/services/sql-data-warehouse/).


## <a name="elastic-database-query-topologies"></a>Elastische topologieën voor query

### <a name="topology-1-vertical-partitioning--cross-database-queries"></a>Topologie 1: Verticale partities – query's met meerdere databases

Zie [aan de slag met cross - databasequery (verticale partities)](sql-database-elastic-query-getting-started-vertical.md)wilt coderen.

Een elastische query kan worden gebruikt om gegevens in een database voor SQLDB voor andere databases SQLDB. U kunt query's van de ene database naar tabellen in andere externe SQLDB database. De eerste stap is het definiëren van een externe gegevensbron voor de externe database. De externe gegevensbron is gedefinieerd in de lokale database waaruit u wilt toegang te krijgen tot tabellen die zich op de externe database. Er zijn geen wijzigingen nodig zijn op de externe database. Voor verticale partitionering standaardsituaties waarin verschillende databases verschillende schema's hebben, kunnen elastische query's algemene use-cases, zoals toegang tot referentiegegevens implementeren en uitvoeren van query's van meerdere databases worden gebruikt.

**Verwijzingsgegevens**: 1 topologie wordt gebruikt voor het beheer van verwijzing. In de onderstaande afbeelding worden twee tabellen (T1 en T2) met referentiegegevens opgeslagen in een speciaal hiervoor bestemde database. Met behulp van een elastische query nu toegankelijk tabellen T1 en T2 op afstand in andere databases, zoals in de afbeelding. Topologie gebruik 1 als tabellen met kleine of externe query's in de tabel hebben selectieve predikaten.

**Figuur 2** Verticale partitionering - elastische referentiegegevens query-query gebruiken

![Verticale partitionering - elastische referentiegegevens query-query gebruiken][3]

**Bij het controleren van meerdere databases**: elastische query's inschakelen use-cases die bij het controleren van verschillende databases van verschillende SQLDB. In figuur 3 ziet u vier verschillende databases: CRM, voorraad, HR en producten. Query's die worden uitgevoerd in een van de databases moeten ook toegang tot alle andere databases. Met behulp van een elastische query, kunt u de database voor deze aanvraag door een paar eenvoudige DDL-instructies uit te voeren op elk van de vier databases. Toegang tot een externe tabel is na deze eenmalige configuratie zo eenvoudig verwijzen naar een lokale tabel van T-SQL-query's of BI-hulpprogramma's. Deze aanpak wordt aanbevolen als de externe query's geen grote resultaten retourneren.

**Figuur 3** Verticale partitionering - elastische query-query gebruiken in verschillende databases

![Verticale partitionering - elastische query-query gebruiken in verschillende databases][4]

### <a name="topology-2-horizontal-partitioning--sharding"></a>Topologie 2: Horizontaal partitioneren – sharding

Met elastische query uitvoeren rapportage via een een laptopgeheugen, dat wil zeggen, horizontaal gepartitioneerd, vereist gegevenslaag een [database elastische shard kaart](sql-database-elastic-scale-shard-map-management.md) voor de databases van de gegevenslaag. Meestal slechts een enkele shard kaart in dit scenario wordt gebruikt en een speciaal hiervoor bestemde database met elastische querymogelijkheden fungeert als het ingangspunt voor het melden van query's. Alleen deze specifieke database toegang tot de shard kaart nodig heeft. In figuur 4 ziet u deze topologie en de configuratie ervan met het overzicht van database- en shard elastische query. De databases in de gegevenslaag kunnen van Azure SQL-Database versie of editie worden. Zie voor meer informatie over de client elastische database bibliotheek en het maken van kaarten shard [Shard beheer toewijzen](sql-database-elastic-scale-shard-map-management.md).

**Figuur 4** Horizontaal partitioneren - elastische query gebruiken voor het melden via een laptopgeheugen gegevens lagen

![Horizontaal partitioneren - elastische query gebruiken voor het melden via een laptopgeheugen gegevens lagen][5]

> [AZURE.NOTE] De query speciale elastische database database moet een database SQL DB v12. Er zijn geen beperkingen voor de shards zelf.

Als u wilt coderen, Zie [aan de slag met elastische databasequery voor horizontaal partitioneren (sharding)](sql-database-elastic-query-getting-started.md).

## <a name="implementing-elastic-database-queries"></a>Elastische databasequery's implementeren

De stappen voor het implementeren van elastische query voor de verticale en horizontale partitionering scenario's worden in de volgende secties besproken. Ze ook verwijzen meer gedetailleerde documentatie voor de verschillende scenario's voor partitionering.

### <a name="vertical-partitioning---cross-database-queries"></a>Verticale partities - query's met meerdere databases

Elastische databasequery's voor verticale partitionering scenario's waarvoor toegang tot een tabel die zich op externe databases, SQLDB met hetzelfde schema configureren voor de volgende stappen uit:

*    [Hoofdsleutel maken](https://msdn.microsoft.com/library/ms174382.aspx) mymasterkey
*    [DATABASE maken binnen het bereik van referentie](https://msdn.microsoft.com/library/mt270260.aspx) -mycredential
*    Mydatasource van het type **RDBMS** [Externe gegevensbron maken en neerzetten](https://msdn.microsoft.com/library/dn935022.aspx)
*    [Externe tabel maken/DROP](https://msdn.microsoft.com/library/dn935021.aspx) MijnTabel

Nadat de DDL-instructies is uitgevoerd, kunt u de externe tabel "mytable" benaderen alsof het een lokale tabel. Azure SQL-Database automatisch een verbinding met de externe database wordt geopend, verwerkt uw aanvraag voor de externe database en de resultaten geretourneerd.
Meer informatie over de stappen die nodig zijn voor het maken van partities verticale scenario vindt u in de [elastische query voor verticale partities](sql-database-elastic-query-vertical-partitioning.md).  

### <a name="horizontal-partitioning---sharding"></a>Horizontaal partitioneren - sharding

De volgende stappen configureren elastische databasequery's voor horizontale partitionering scenario's waarvoor toegang tot een aantal van de tabel die zich bevinden op (meestal) verschillende externe SQLDB-databases:

*    [Hoofdsleutel maken](https://msdn.microsoft.com/library/ms174382.aspx) mymasterkey
*    [DATABASE maken binnen het bereik van referentie](https://msdn.microsoft.com/library/mt270260.aspx) -mycredential
*    Een [shard kaart](sql-database-elastic-scale-shard-map-management.md) die de gegevenslaag met behulp van de elastische database client-bibliotheek maken.   
*    [De externe gegevensbron maken/DROP](https://msdn.microsoft.com/library/dn935022.aspx) -mydatasource van het type **SHARD_MAP_MANAGER**
*    [Externe tabel maken/DROP](https://msdn.microsoft.com/library/dn935021.aspx) MijnTabel

Nadat u deze stappen hebt uitgevoerd, kunt u het horizontaal gepartitioneerde tabel "mytable" benaderen alsof het een lokale tabel. Azure SQL-Database automatisch geopend meerdere parallelle verbindingen met externe databases waarin de tabellen fysiek worden opgeslagen, verwerkt de aanvragen voor externe databases en de resultaten geretourneerd.
Meer informatie over de stappen die nodig zijn voor het horizontale partitionering scenario vindt u in de [elastische query voor horizontaal partitioneren](sql-database-elastic-query-horizontal-partitioning.md).

## <a name="t-sql-querying"></a>T-SQL-query 's
Als u de externe gegevensbronnen en externe tabellen hebt gedefinieerd, kunt u gewone tekenreeksen voor SQL Server verbinding maken met de databases waarin u de externe tabellen hebt gedefinieerd. Vervolgens kunt u uitvoeren T-SQL-instructies via de externe tabellen voor die verbinding met de beperkingen die hieronder worden beschreven. U vindt meer informatie en voorbeelden van T-SQL-query's in de onderwerpen van de documentatie voor het [partitioneren van horizontale](sql-database-elastic-query-horizontal-partitioning.md) en [verticale partitioneren](sql-database-elastic-query-vertical-partitioning.md).

## <a name="connectivity-for-tools"></a>Connectiviteit voor hulpprogramma 's
Gewone tekenreeksen voor SQL Server kunt u verbinding maken met uw toepassingen en gegevens of BI-integratiefuncties tot databases met externe tabellen. Zorg ervoor dat de SQL Server als gegevensbron voor het hulpprogramma wordt ondersteund. Wanneer een verbinding, verwijzen naar de querydatabase elastisch en de externe tabellen in de database, net zoals u zou doen met een andere SQL Server-database waarmee u verbinding met het gereedschap maakt.

> [AZURE.IMPORTANT] Verificatie met behulp van Azure Active Directory met elastische query's wordt momenteel niet ondersteund.

## <a name="cost"></a>Kosten

Elastische query is opgenomen in de kosten van Azure SQL Database databases. Houd er rekening mee dat waar uw externe databases in een ander datacenter dan het eindpunt elastische query worden topologieën worden ondersteund, maar de egress gegevens uit externe databases regelmatig [Azure tarieven](https://azure.microsoft.com/pricing/details/data-transfers/)in rekening worden gebracht.

## <a name="preview-limitations"></a>Voorbeeld van beperkingen
* De eerste elastische query wordt uitgevoerd kan enkele minuten duren op het niveau van de Standard performance. Deze tijd is nodig om te laden van de elastische queryfunctionaliteit; laden van de prestaties worden verbeterd met hogere niveaus van prestaties.
* Scripting van externe gegevensbronnen of externe tabellen van SSMS of SSDT wordt nog niet ondersteund.
* Importeren/exporteren voor SQL DB ondersteunt nog geen externe gegevensbronnen en externe tabellen. Als u importeren/exporteren gebruiken wilt, deze objecten te verwijderen voordat u exporteert en vervolgens opnieuw maken na het importeren.
* Elastische databasequery ondersteunt momenteel alleen-lezen toegang tot externe tabellen. Echter kunt u volledige functionaliteit van T-SQL van de database waarin de externe tabel is gedefinieerd. Dit is handig voor bijvoorbeeld tijdelijke resultaten met bijvoorbeeld aanhouden, selecteert u < column_list > in < local_table > of opgeslagen procedures definiëren op de querydatabase van elastische die verwijzen naar externe tabellen.
* Met uitzondering van nvarchar(max), worden LOB-typen niet ondersteund in de externe tabeldefinities. Als tijdelijke oplossing kunt u een weergave op de externe database die door het LOB-type in de nvarchar(max) geworpen maken, uw externe tabel definiëren via de weergave in plaats van de basistabel en cast deze naar het oorspronkelijke LOB-type in uw query's.
* Kolom statistieken over externe tabellen worden momenteel niet ondersteund. Statistieken van de tabellen worden ondersteund, maar moeten handmatig worden gemaakt.

## <a name="feedback"></a>Feedback
Geef feedback over uw ervaringen met delen elastische query's met ons op onderstaande Disqus MSDN forums of Stackoverflow. Wij zijn geïnteresseerd in alle soorten feedback over de service (defecten, ruwe randen, lacunes functie).

## <a name="more-information"></a>Meer informatie

U vindt meer informatie over het opvragen van meerdere databases en verticale partitionering scenario's in de volgende documenten:

* [Cross-database opvragen en verticale partities-overzicht](sql-database-elastic-query-vertical-partitioning.md)
* Probeer onze stapsgewijze zelfstudie als u wilt dat een volledig werkend voorbeeld uitgevoerd in minuten: [aan de slag met cross - databasequery (verticale partities)](sql-database-elastic-query-getting-started-vertical.md).


Meer informatie over scenario's horizontaal partitioneren en sharding is hier beschikbaar:

* [Horizontaal partitioneren en sharding-overzicht](sql-database-elastic-query-horizontal-partitioning.md)
* Probeer onze stapsgewijze zelfstudie als u wilt dat een volledig werkend voorbeeld uitgevoerd in minuten: [aan de slag met elastische databasequery voor horizontaal partitioneren (sharding)](sql-database-elastic-query-getting-started.md).


[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-query-overview/overview.png
[2]: ./media/sql-database-elastic-query-overview/topology1.png
[3]: ./media/sql-database-elastic-query-overview/vertpartrrefdata.png
[4]: ./media/sql-database-elastic-query-overview/verticalpartitioning.png
[5]: ./media/sql-database-elastic-query-overview/horizontalpartitioning.png

<!--anchors-->
