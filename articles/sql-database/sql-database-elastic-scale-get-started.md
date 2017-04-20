<properties 
    pageTitle="Aan de slag met elastische Databasehulpmiddelen" 
    description="Eenvoudige uitleg van elastische database extra functie van Azure SQL-Database, inclusief eenvoudig voorbeeld app uitvoeren." 
    services="sql-database" 
    documentationCenter="" 
    manager="jhubbard" 
    authors="ddove" 
    editor="CarlRabeler"/>

<tags 
    ms.service="sql-database" 
    ms.workload="sql-database" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="05/27/2016" 
    ms.author="ddove"/>

# <a name="get-started-with-elastic-database-tools"></a>Aan de slag met elastische Databasehulpmiddelen

Dit document bevat een inleiding tot de functionaliteit voor ontwikkelaars door het monster app uit te voeren. Het monster wordt een eenvoudige toepassing een laptopgeheugen en behandelt de belangrijkste mogelijkheden van elastische Databasehulpmiddelen. In het voorbeeld worden de functies van de [elastische database client library](sql-database-elastic-database-client-library.md)

Als u wilt installeren in de bibliotheek, gaat u naar [Microsoft.Azure.SqlDatabase.ElasticScale.Client](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/). Houd er rekening mee dat de bibliotheek wordt geïnstalleerd met het monster app die hieronder worden beschreven.

## <a name="prerequisites"></a>Vereisten

1. Visual Studio 2012 of hoger met C# is vereist. Download een gratis versie van [Visual Studio downloadt](http://www.visualstudio.com/downloads/download-visual-studio-vs.aspx).
2. Nuget 2.7 of hoger. Als u de meest recente versie, Zie [NuGet installeren](http://docs.nuget.org/docs/start-here/installing-nuget)

## <a name="download-and-run-the-sample-app"></a>Download en installeer de app monster

De **Elastisch Database SQL Azure — aan de slag** voorbeeldtoepassing illustreert de belangrijkste aspecten van de ontwikkeling-ervaring voor een laptopgeheugen toepassingen met Azure SQL database elastische tools. De nadruk ligt op de belangrijkste use-cases voor [shard beheer toewijzen](sql-database-elastic-scale-shard-map-management.md), [gegevensafhankelijke routering](sql-database-elastic-scale-data-dependent-routing.md) en [meerdere shard opvragen](sql-database-elastic-scale-multishard-querying.md). Als u wilt downloaden en uitvoeren van het monster, de volgende stappen uit: 

1. Visual Studio openen en selecteer **Bestand -> Nieuw -> Project**.
2. Klik in het dialoogvenster op **Online**.

    ![Nieuw Project > Online][2]
3. Klik vervolgens op **Visual C#** onder **voorbeelden**.

    ![Klik op Visual C#][3]
4. Typ in het zoekvak **elastische db** te zoeken voor het monster. De titel **Elastische DB Tools voor Azure SQL - aan de slag** weergegeven.

    ![Zoekvak][1]
 
5. Selecteer het monster, kies een naam en een locatie voor het nieuwe project en klik op **OK** om het project te maken.
6. Open het bestand **app.config** in de oplossing voor het voorbeeldproject en volg de instructies in het bestand de naam van uw Azure SQL database server en uw aanmeldingsgegevens (gebruikersnaam en wachtwoord) toe te voegen.
7. Bouwen en uitvoeren van de toepassing. Wanneer u wordt gevraagd, kan Visual Studio om te herstellen van de pakketten NuGet van de oplossing. Dit zal de meest recente versie van de bibliotheek elastische database client downloaden van NuGet.
8. Spelen met de verschillende opties voor meer informatie over de mogelijkheden van de client-bibliotheek. Opmerking de stappen die de toepassing in de console wordt-uitvoer kunnen u de code achter de schermen te verkennen.

    ![voortgang][4]

Gefeliciteerd, je hebt gebouwd en de eerste een laptopgeheugen toepassing met elastische Databasehulpmiddelen op Azure SQL-Database wordt uitgevoerd. Bekijk snel de shards die het monster gemaakt door verbinding te maken met Visual Studio of SQL Server Management Studio met de databaseserver Azure. U zult de nieuwe shard voorbeelddatabases en een shard kaart manager-database die is gemaakt met het monster.

> [AZURE.IMPORTANT] Het wordt aanbevolen dat u altijd de meest recente versie van het Management Studio gebruiken om met updates voor Microsoft Azure en SQL-Database gesynchroniseerd blijven. [Bijwerken van SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).


### <a name="key-pieces-of-the-code-sample"></a>Belangrijke onderdelen van het codevoorbeeld

1. **Shards beheren en Shard kaarten**: de code ziet u hoe u werkt met shards, bereiken, en de toewijzingen in het bestand **ShardMapManagerSample.cs**. U vindt meer informatie over dit onderwerp hier: [Shard Map Management](http://go.microsoft.com/?linkid=9862595).  
2. **Gegevensafhankelijke routering**: routering van transacties naar de juiste shard wordt weergegeven in de **DataDependentRoutingSample.cs**. Zie de [Gegevensafhankelijke routering](http://go.microsoft.com/?linkid=9862596)voor meer informatie. 
3. **Query's op meerdere Shards**: query's via shards wordt geïllustreerd in het bestand **MultiShardQuerySample.cs**. Zie [Meerdere Shard opvragen](http://go.microsoft.com/?linkid=9862597)voor meer informatie.
4. **Lege shards toevoegen**: het toevoegen van nieuwe lege shards iteratieve wordt uitgevoerd door de code in het bestand **AddNewShardsSample.cs**. Details over dit onderwerp vindt u hier: [Shard Map Management](http://go.microsoft.com/?linkid=9862595).

### <a name="other-elastic-scale-operations"></a>Andere bewerkingen elastische schaal

1. **Splitsen van een bestaande shard**: de mogelijkheid om te splitsen shards is beschikbaar via het **hulpprogramma gesplitste samenvoegen**. U vindt meer informatie over dit hulpprogramma hier: [Overzicht gesplitste samenvoegen](sql-database-elastic-scale-overview-split-and-merge.md).
2. **Samenvoegen van bestaande shards**: Shard samenvoegingen worden ook uitgevoerd met het **hulpprogramma gesplitste samenvoegen**. Raadpleeg voor meer informatie: [Overzicht gesplitste samenvoegen](sql-database-elastic-scale-overview-split-and-merge.md).   


## <a name="cost"></a>Kosten

De elastische database tools zijn gratis. Elastische Databasehulpmiddelen biedt geen extra kosten bovenop de kosten voor het gebruik van uw Azure opleggen. 

De voorbeeldtoepassing maakt u bijvoorbeeld nieuwe databases. De kosten zijn afhankelijk van de Azure SQL DB database-editie die u kiest en de Azure gebruik van uw toepassing.

Zie [SQL Database prijzen Details](https://azure.microsoft.com/pricing/details/sql-database/)voor prijsinformatie.

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over de elastische database tools:

* [Elastische database extra Documentatieschema](https://azure.microsoft.com/documentation/learning-paths/sql-database-elastic-scale/) 
-    Voorbeelden van code: 
    -    [Elastische DB met Azure SQL - aan de slag](http://code.msdn.microsoft.com/Elastic-Scale-with-Azure-a80d8dc6?SRC=VSIDE)
    -    [Elastische DB met Azure SQL - integratie met entiteit-Framework](http://code.msdn.microsoft.com/Elastic-Scale-with-Azure-bae904ba?SRC=VSIDE)
    -    [Shard elasticiteit in het Script Center](https://gallery.technet.microsoft.com/scriptcenter/Elastic-Scale-Shard-c9530cbe)
-    Blog: [elastische schaal aankondiging](https://azure.microsoft.com/blog/2014/10/02/introducing-elastic-scale-preview-for-azure-sql-database/)
-    Channel 9: [elastische schaal overzicht Video](http://channel9.msdn.com/Shows/Data-Exposed/Azure-SQL-Database-Elastic-Scale)
-    Discussieforum: [forum Azure SQL-Database](http://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted)
-    Om prestaties te meten: [prestatiemeteritems voor shard kaart manager](sql-database-elastic-database-client-library.md)


<!--Anchors-->
[The Elastic Scale Sample Application]: #The-Elastic-Scale-Sample-Application
[Download and Run the Sample App]: #Download-and-Run-the-Sample-App
[Cost]: #Cost
[Next steps]: #next-steps

<!--Image references-->
[1]: ./media/sql-database-elastic-scale-get-started/newProject.png
[2]: ./media/sql-database-elastic-scale-get-started/click-online.png
[3]: ./media/sql-database-elastic-scale-get-started/click-CSharp.png
[4]: ./media/sql-database-elastic-scale-get-started/output2.png
 
