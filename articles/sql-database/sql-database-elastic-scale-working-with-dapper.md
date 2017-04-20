<properties 
    pageTitle="Met elastische database client-bibliotheek met Dapper | Microsoft Azure" 
    description="Met behulp van elastische database client-bibliotheek met Dapper." 
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
    ms.date="05/27/2016" 
    ms.author="torsteng"/>

# <a name="using-elastic-database-client-library-with-dapper"></a>Elastische database client library gebruiken met Dapper 

Dit document is bedoeld voor ontwikkelaars die gebruikmaken van Dapper om toepassingen te bouwen, maar ook wilt Profiteer [elastische database gereedschap](sql-database-elastic-scale-introduction.md) geïmplementeerd sharding als u wilt schalen de gegevenslaag maken.  Dit document ziet u de wijzigingen in Dapper-toepassingen die nodig zijn om te integreren met databasehulpmiddelen elastisch. Onze focus is op het opstellen van de elastische shard voor databasebeheer en afhankelijke gegevens routering met Dapper. 

**Voorbeeld van Code**: [elastische Databasehulpmiddelen voor Azure SQL-Database - integratie Dapper](https://code.msdn.microsoft.com/Elastic-Scale-with-Azure-e19fc77f).
 
Integratie van **Dapper** en **DapperExtensions** met de elastische database client library voor Azure SQL-Database is eenvoudig. Uw toepassingen kunnen afhankelijk routing door het wijzigen van het maken en openen van nieuwe [SqlConnection](http://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.aspx) objecten van de gegevens naar de oproep van de [OpenConnectionForKey](http://msdn.microsoft.com/library/azure/dn807226.aspx) van de [client-bibliotheek](http://msdn.microsoft.com/library/azure/dn765902.aspx)gebruiken. Dit houdt in dat wijzigingen in uw toepassing alleen wanneer er nieuwe verbindingen worden gemaakt en geopend. 

## <a name="dapper-overview"></a>Dapper-overzicht
**Dapper** is een object-relationele mapper. .NET-objecten vanuit uw toepassing een relationele database (en omgekeerd) wordt toegewezen. Het eerste deel van de voorbeeldcode wordt geïllustreerd hoe u de clientbibliotheek elastische database kunt integreren met Dapper-toepassingen. Het tweede deel van de voorbeeldcode ziet u hoe u met behulp van Dapper en DapperExtensions worden geïntegreerd.  

De functionaliteit van de toewijzing in Dapper hier uitbreidingsmethoden databaseverbindingen die vereenvoudigen indienende T-SQL-instructies voor de uitvoering of de database opvragen. Bijvoorbeeld, kunt Dapper u eenvoudig tussen uw .NET-objecten en de parameters van SQL-instructies voor het aanroepen van de **Execute** toewijzen of de resultaten van een SQL-query's gebruiken in .NET-objecten met **Query** oproepen vanuit Dapper. 

Wanneer u DapperExtensions gebruikt, moet u niet langer de SQL-instructies bevatten. Extensies methoden zoals **GetList** of **Invoegen** via de databaseverbinding maken voor de SQL-instructies achter de schermen.
 
Een ander voordeel van Dapper en ook de DapperExtensions is dat de toepassing het maken van verbindingen met de database bestuurt. Op deze manier communiceren met de clientbibliotheek elastische database database makelaars-verbindingen op basis van de toewijzing van shardlets tot databases.

Als u de Dapper assembly's, Zie [Dapper dot net](http://www.nuget.org/packages/Dapper/). Zie [DapperExtensions](http://www.nuget.org/packages/DapperExtensions)voor de Dapper extensies.

## <a name="a-quick-look-at-the-elastic-database-client-library"></a>Een kort overzicht van de elastische database client library

Met de clientbibliotheek elastische database partities van de toepassingsgegevens genaamd *shardlets* definiëren, toewijzen aan databases en *sharding sleutels*herkennen. U kunt zoveel databases instellen als u nodig hebt en uw shardlets over deze databases verdelen hebben. De toewijzing van sharding waarden van de databases is door een shard overzicht van API's van de bibliotheek opgeslagen. Deze mogelijkheid heet **shard beheer toewijzen**. De kaart shard fungeert ook als de broker databaseverbindingen voor aanvragen die een sleutel sharding uitvoeren. Deze mogelijkheid wordt genoemd **gegevensafhankelijke routering**.

![Shard kaarten en gegevensafhankelijke routering][1]

De manager van de tekens shard beschermt gebruikers tegen inconsistente weergaven in de shardlet van gegevens die optreden kunnen als er gelijktijdige shardlet beheertaken uit te voeren op de databases zijn gebeurt. De kaarten shard broker hiertoe de databaseverbindingen voor een toepassing die met de bibliotheek. Wanneer shard beheerbewerkingen invloed op de shardlet hebben kunnen, kan hierdoor de functionaliteit van de map shard te doden automatisch een databaseverbinding. 

In plaats van de traditionele manier om verbindingen voor Dapper te maken, moeten we de [methode OpenConnectionForKey](http://msdn.microsoft.com/library/azure/dn824099.aspx)gebruiken. Dit zorgt ervoor dat alle validatie vindt plaats verbindingen goed worden beheerd wanneer gegevens tussen shards beweegt.

### <a name="requirements-for-dapper-integration"></a>Vereisten voor integratie van Dapper

Als u werkt met de elastische database client-bibliotheek en de Dapper-API's, die we willen behouden de volgende eigenschappen:

* **Scaleout**: We willen toevoegen of verwijderen van databases in de gegevenslaag van een laptopgeheugen toepassing die nodig zijn voor de behoeften van de capaciteit van de toepassing. 

-    **Consistentie**: sinds onze toepassing schaalvergroting is toegepast met behulp van sharding, moeten we voeren gegevensafhankelijke routering. We willen de afhankelijke routing mogelijkheden van de bibliotheek gebruiken om dit te doen. In het bijzonder willen we behouden de validatie en consistentie waarborgen verstrekt door de verbindingen die zijn brokered door middel van de toewijzing shard manager om te voorkomen dat de beschadiging of de verkeerde resultaten. Dit zorgt ervoor dat verbindingen met een bepaalde shardlet zijn afgewezen of gestopt als (bijvoorbeeld) de shardlet op dit moment wordt verplaatst naar een andere shard met API's splitsen/samenvoegen.

-    **Object toewijzen**: We willen het gemak van de toewijzingen die door Dapper vertalen tussen klassen in de toepassing en de onderliggende databasestructuren behouden. 

De volgende sectie bevat informatie over deze vereisten voor toepassingen op basis van **Dapper** en **DapperExtensions**.

## <a name="technical-guidance"></a>Technische richtsnoeren
### <a name="data-dependent-routing-with-dapper"></a>Gegevens-afhankelijke routering met Dapper 

Met Dapper is de toepassing meestal verantwoordelijk voor het maken en openen van de verbindingen met de onderliggende database. Een type T wordt gegeven door de toepassing, retourneert Dapper de resultaten van query zoals .NET-collecties van het type T. Dapper de toewijzing van de T-SQL resultaatrijen uitvoert voor de objecten van het type T. Evenzo wijst Dapper .NET-objecten in de SQL-waarden of parameters voor instructies voor data manipulation language (DML). Dapper biedt deze functionaliteit aan via uitbreidingsmethoden op de reguliere [SqlConnection](http://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.aspx) -object uit de bibliotheken van ADO .NET SQL Client. De SQL-verbinding die wordt geretourneerd door de elastische schaal API's voor DDR zijn ook regelmatig [SqlConnection](http://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.aspx) -objecten. Deze manier kunnen wij Dapper extensies direct boven de tekst die wordt geretourneerd door de clientbibliotheek DDR-API gebruiken omdat het is een eenvoudige SQL Client verbinding.

Deze opmerkingen kunnen u eenvoudig verbindingen brokered door de elastische database client library voor Dapper te gebruiken.

(Uit het bijbehorende monster) voorbeeld illustreert de aanpak waar de sleutel sharding wordt geleverd door de toepassing naar de bibliotheek om de verbinding met de juiste shard broker.   

    using (SqlConnection sqlconn = shardingLayer.ShardMap.OpenConnectionForKey(
                     key: tenantId1, 
                     connectionString: connStrBldr.ConnectionString, 
                     options: ConnectionOptions.Validate))
    {
        var blog = new Blog { Name = name };
        sqlconn.Execute(@"
                      INSERT INTO
                            Blog (Name)
                            VALUES (@name)", new { name = blog.Name }
                        );
    }

De [OpenConnectionForKey](http://msdn.microsoft.com/library/azure/dn807226.aspx) API-aanroep vervangt de standaard maken en openen van een verbinding met SQL Client. De aanroep van [OpenConnectionForKey](http://msdn.microsoft.com/library/azure/dn807226.aspx) worden de argumenten die vereist voor het gegevensafhankelijke routering zijn: 

-    De kaart shard toegang te krijgen tot de gegevens afhankelijk routeringsinterfaces
-    De sleutel sharding ter identificatie van de shardlet
-    De referenties (gebruikersnaam en wachtwoord) verbinding maken met de shard

Het object shard kaart maakt een verbinding met de shard die in het bezit van de shardlet voor de sleutel sharding gegeven. De elastische databaseclient-API wordt ook de verbinding met het implementeren van de consistentie waarborgen labelen. Sinds de aanroep van [OpenConnectionForKey](http://msdn.microsoft.com/library/azure/dn807226.aspx) een gewone SQL Client connection-object retourneert, volgt de daarop volgende oproep van de methode **Execute** uitbreiding van Dapper de standaardprocedure Dapper.

Query's werken bijna hetzelfde: de verbinding met de [OpenConnectionForKey](http://msdn.microsoft.com/library/azure/dn807226.aspx) van de client-API voor de eerste keer opent. U de normale Dapper uitbreidingsmethoden toe te wijzen de resultaten van uw SQL-query in .NET-objecten:

    using (SqlConnection sqlconn = shardingLayer.ShardMap.OpenConnectionForKey(
                    key: tenantId1, 
                    connectionString: connStrBldr.ConnectionString, 
                    options: ConnectionOptions.Validate ))
    {    
           // Display all Blogs for tenant 1
           IEnumerable<Blog> result = sqlconn.Query<Blog>(@"
                                SELECT * 
                                FROM Blog
                                ORDER BY Name");

           Console.WriteLine("All blogs for tenant id {0}:", tenantId1);
           foreach (var item in result)
           {
                Console.WriteLine(item.Name);
            }
    }

Merk op dat het **gebruik van** de DDR-verbinding blokkeren bereiken alle databasebewerkingen binnen het blok aan de ene shard waar tenantId1 wordt gehouden. De query retourneert alleen opgeslagen op de huidige shard blogs, maar niet de bestanden die zijn opgeslagen op een andere shards. 

## <a name="data-dependent-routing-with-dapper-and-dapperextensions"></a>Afhankelijke routering met Dapper en DapperExtensions gegevens

Dapper is voorzien van een ecosysteem van extra extensies die meer gebruiksgemak en een abstractielaag ten opzichte van de database bieden kan bij het ontwikkelen van databasetoepassingen. DapperExtensions is een voorbeeld. 

DapperExtensions gebruiken in uw toepassing verandert niet hoe de databaseverbindingen worden gemaakt en beheerd. Nog steeds de verantwoordelijkheid van de toepassing te openen, verbindingen en gewone SQL Client connection-objecten worden verwacht door de uitbreidingsmethoden. We vertrouwen op de [OpenConnectionForKey](http://msdn.microsoft.com/library/azure/dn807226.aspx) als hierboven beschreven. Als de volgende codevoorbeelden ziet, is de enige wijziging die we niet meer moeten de T-SQL-instructies schrijven:

    using (SqlConnection sqlconn = shardingLayer.ShardMap.OpenConnectionForKey(
                    key: tenantId2, 
                    connectionString: connStrBldr.ConnectionString, 
                    options: ConnectionOptions.Validate))
    {
           var blog = new Blog { Name = name2 };
           sqlconn.Insert(blog);
    }

En hier is het voorbeeld voor de query: 

    using (SqlConnection sqlconn = shardingLayer.ShardMap.OpenConnectionForKey(
                    key: tenantId2, 
                    connectionString: connStrBldr.ConnectionString, 
                    options: ConnectionOptions.Validate))
    {
           // Display all Blogs for tenant 2
           IEnumerable<Blog> result = sqlconn.GetList<Blog>();
           Console.WriteLine("All blogs for tenant id {0}:", tenantId2);
           foreach (var item in result)
           {
               Console.WriteLine(item.Name);
           }
    }

### <a name="handling-transient-faults"></a>Tijdelijke fouten afhandelen

Het team van Microsoft Patterns & Practices gepubliceerd het [Tijdelijke fout afhandeling toepassing blokkeren](http://msdn.microsoft.com/library/hh680934.aspx) zodat softwareontwikkelaars gemeenschappelijke tijdelijke storingen opgetreden tijdens het uitvoeren in de cloud te verminderen. Zie voor meer informatie [Perseverance, geheim alle successen: de tijdelijke fout afhandeling toepassing blokkeren met](http://msdn.microsoft.com/library/dn440719.aspx).

In het voorbeeld is gebaseerd op de bibliotheek fout met betrekking tot tijdelijke bescherming tegen fouten van voorbijgaande aard. 

    SqlDatabaseUtils.SqlRetryPolicy.ExecuteAction(() =>
    {
       using (SqlConnection sqlconn = 
          shardingLayer.ShardMap.OpenConnectionForKey(tenantId2, connStrBldr.ConnectionString, ConnectionOptions.Validate))
          {
              var blog = new Blog { Name = name2 };
              sqlconn.Insert(blog);
          }
    });

**SqlDatabaseUtils.SqlRetryPolicy** in de bovenstaande code wordt gedefinieerd als een **SqlDatabaseTransientErrorDetectionStrategy** met een aantal nieuwe pogingen van 10 en 5 seconden wachttijd tussen herhaalde pogingen. Als u transacties gebruikt, moet u uw bereik opnieuw gaat terug naar het begin van de transactie in het geval van een tijdelijke storing.

## <a name="limitations"></a>Beperkingen

De in dit document beschreven aanpak leiden tot een aantal beperkingen:

* De voorbeeldcode voor dit document wordt niet beschreven hoe schema beheren via shards.
* Uitgaande van een aanvraag, nemen we aan dat alle de verwerking van de database zich in een enkel shard zoals aangegeven door de sleutel sharding van de aanvraag. Echter, deze aanname niet altijd bezitten, bijvoorbeeld wanneer het is niet mogelijk om een sleutel sharding beschikbaar te stellen. Om dit op te lossen, bevat de bibliotheek elastische database client de [klasse MultiShardQuery](http://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.multishardexception.aspx). De klasse implementeert een abstractie verbinding voor het uitvoeren van query's via verschillende shards. MultiShardQuery gebruiken in combinatie met Dapper valt buiten het bestek van dit document.

## <a name="conclusion"></a>Conclusie

Toepassingen met Dapper en DapperExtensions kunnen eenvoudig profiteren van extra elastische database voor Azure SQL-Database. Via de stappen in dit document kunnen die toepassingen gebruiken de mogelijkheid van het hulpprogramma voor gegevens afhankelijk routing door het wijzigen van het maken en openen van nieuwe [SqlConnection](http://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.aspx) objecten aan de oproep van de [OpenConnectionForKey](http://msdn.microsoft.com/library/azure/dn807226.aspx) van de client elastische database bibliotheek gebruiken. Dit houdt in dat de wijzigingen in de toepassing nodig is op de plaatsen waar nieuwe verbindingen worden gemaakt en geopend. 

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-scale-working-with-dapper/dapperimage1.png
 