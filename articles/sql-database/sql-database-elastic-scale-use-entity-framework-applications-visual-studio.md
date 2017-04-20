<properties 
    pageTitle="Met elastische database client-bibliotheek met entiteit Framework | Microsoft Azure" 
    description="Elastische Database client-bibliotheek en entiteit Framework gebruiken voor het coderen van databases" 
    services="sql-database" 
    documentationCenter="" 
    manager="jhubbard" 
    authors="torsteng" 
    editor=""/>

<tags 
    ms.service="sql-database" 
    ms.workload="sql-database" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="05/27/2016" 
    ms.author="torsteng"/>

# <a name="elastic-database-client-library-with-entity-framework"></a>Elastische Database client-bibliotheek met entiteit-Framework 
 
Dit document bevat de wijzigingen in een entiteit Framework-toepassing die nodig zijn om te integreren met [Databasehulpmiddelen elastisch](sql-database-elastic-scale-introduction.md). De nadruk ligt op het opstellen van [shard beheer toewijzen](sql-database-elastic-scale-shard-map-management.md) en [gegevensafhankelijke routering](sql-database-elastic-scale-data-dependent-routing.md) met de **Eerste Code** Framework entiteit aanpak. De [Eerste Code – nieuwe Database](http://msdn.microsoft.com/data/jj193542.aspx) zelfstudie voor EF fungeert als ons voorbeeld uitgevoerd in het gehele document. De voorbeeldcode die bij dit document maakt deel uit van de elastische Databasehulpmiddelen instellen van de monsters in de Visual Studio-codevoorbeelden.
  
## <a name="downloading-and-running-the-sample-code"></a>Downloaden en uitvoeren van de voorbeeldcode
De code voor dit artikel downloaden:

* Visual Studio 2012 of hoger is vereist. 
* Start Visual Studio. 
* In Visual Studio-Selecteer Bestand > Nieuw Project. 
* Klik in het dialoogvenster 'Nieuw Project' Ga naar de **Online voorbeelden** voor **Visual C#** en het type "elastische db' in het zoekvak in de rechterbovenhoek.
    
    ![Entiteit Framework en elastische database monster app][1] 

    Selecteer het monster **Elastische DB Tools voor Azure SQL-entiteit Framework integratie**genoemd. Nadat u de gebruiksrechtovereenkomst accepteert, wordt het monster geladen. 

Het voorbeeld alleen uitvoeren, moet u drie lege databases maken in Azure SQL-Database:

* Shard kaart Manager-database
* 1 shard database
* Shard 2 database

Zodra u deze databases hebt gemaakt, vult u de plaatsaanduiding in **Program.cs** met uw Azure SQL server-databasenaam, de databasenamen en uw referenties verbinding maken met de databases. Maak de oplossing in Visual Studio. Visual Studio worden gedownload van de vereiste NuGet pakketten voor de clientbibliotheek elastische database entiteit Framework en tijdelijke storing verwerken als onderdeel van het bouwproces. Zorg ervoor dat NuGet pakketten herstellen is ingeschakeld voor uw oplossing. U kunt deze instelling inschakelen door met de rechtermuisknop op het oplossingsbestand in Visual Studio Solution Explorer. 

## <a name="entity-framework-workflows"></a>Entiteit Framework workflows 

Entiteit Framework ontwikkelaars vertrouwen op een van de volgende vier workflows om toepassingen te bouwen en ervoor te zorgen persistentie voor toepassingsobjecten: 

* **Eerste code (nieuwe Database)**: de EF-ontwikkelaar maakt het model in de toepassingscode en EF genereert vervolgens de database uit. 
* **Eerste code (bestaande Database)**: de ontwikkelaar kunt EF de toepassingscode genereren voor het model vanuit een bestaande database.
* **Eerste model**: de ontwikkelaar maakt het model in de ontwerpfunctie voor EF en EF maakt vervolgens de database uit het model.
* **Eerste database**: de ontwikkelaar EF gereedschap voor het afleiden van het model vanuit een bestaande database gebruikt. 

Alle deze benaderingen zijn gebaseerd op de klasse DbContext transparant beheer van databaseverbindingen en databaseschema voor een toepassing. Database maken bootstrappen en schema zoals bespreken we later in het document uitvoeriger verschillende constructors van de basisklasse van DbContext voor verschillende niveaus van controle over het maken van een verbinding toestaan. Uitdagingen ontstaan voornamelijk uit het feit dat het beheer van de database die door EF met de beheermogelijkheden van de verbinding van de gegevens afhankelijk routeringsinterfaces geleverd snijdt door de elastische database client library. 

## <a name="elastic-database-tools-assumptions"></a>Elastische database extra veronderstellingen 

Zie [elastische Database extra woordenlijst](sql-database-elastic-scale-glossary.md)voor definities van de term.

Met elastische database client library definieert u de partities van de toepassingsgegevens shardlets genoemd. Shardlets geïdentificeerd zijn met een sleutel sharding en worden toegewezen aan specifieke databases. Een toepassing kan aantal databases nodig hebben en verspreiden de shardlets om voldoende capaciteit of prestaties, gegeven de huidige zakelijke vereisten. De toewijzing van sharding waarden van de databases wordt opgeslagen in een shard overzicht van de elastische databaseclient-API. We noemen deze mogelijkheid **Shard kaart Management**of SMM voor korte. De kaart shard fungeert ook als de broker databaseverbindingen voor aanvragen die een sleutel sharding uitvoeren. We verwijzen naar deze mogelijkheid als **gegevensafhankelijke routering**. 
 
De manager van de tekens shard beschermt gebruikers tegen inconsistente weergaven in de shardlet van gegevens die optreden kunnen als er gelijktijdige shardlet beheertaken uit te voeren (bijvoorbeeld gegevens uit een shard verplaatsen naar een andere) zijn er aan de hand. Hiertoe beheerd de shard toewijzingen door de client-bibliotheek makelaar de databaseverbindingen voor een toepassing. Hierdoor kan de functionaliteit van de map shard voor het doden van een databaseverbinding automatisch als shard beheertaken uit te voeren invloed op de shardlet die hebben kunnen voor de verbinding is gemaakt. Deze aanpak moet integreren met een aantal van EF-functionaliteit, zoals het maken van nieuwe verbindingen van een bestaande database bestaan controleren. In het algemeen, is onze waarneming dat de standaard DbContext constructors alleen werken voor gesloten databaseverbindingen die veilig kunnen worden gekloond voor EF betrouwbaar werken. In plaats daarvan is het beginsel van het ontwerp van de elastische database broker alleen geopende verbindingen. Een misschien denkt dat het sluiten van een verbinding door de clientbibliotheek brokered voordat de overdracht aan de EF-DbContext dit probleem kan oplossen. Echter, foregoes een door het sluiten van de verbinding en vertrouwen op EF opnieuw om het te openen, de validatie en consistentie-controles uitgevoerd door de bibliotheek. De functionaliteit voor migraties in EF, echter deze verbindingen worden beheerd met de onderliggende databaseschema op een manier die transparant voor de toepassing. In het ideale geval willen we behouden en alle deze mogelijkheden van de elastische database client-bibliotheek en de EF in dezelfde toepassing te combineren. De volgende sectie worden deze eigenschappen en eisen nader besproken. 


## <a name="requirements"></a>Vereisten 

Als u werkt met de elastische database client-bibliotheek en de entiteit Framework API's, die we willen behouden de volgende eigenschappen: 

* **Schalen**: toevoegen of verwijderen van databases van de gegevenslaag van een laptopgeheugen toepassing die nodig zijn voor de behoeften van de capaciteit van de toepassing. Dit betekent controle het toewijzen van het maken en verwijderen van databases en het gebruik van de elastische database shard beheer-API's voor het beheren van databases en toewijzingen van shardlets. 

* **Consistentie**: de toepassing sharding in dienst, en de afhankelijke routing mogelijkheden van de clientbibliotheek gebruikt. Voorkom beschadiging of verkeerde queryresultaten verbindingen zijn brokered door middel van de toewijzing shard manager. Dit houdt ook validatie en consistentie.
 
* **Eerste code**: het gemak van het eerste paradigma voor de EF-code behouden. Klassen in de toepassing worden in de eerste Code, transparant toegewezen aan de onderliggende databasestructuren. De code van de toepassing communiceert met DbSets die de meeste aspecten van de verwerking van de onderliggende database te maskeren.
 
* **Schema**: eerste database schema maken en de ontwikkeling van de volgende schema via migraties entiteit Framework worden verwerkt. Door het behoud van deze mogelijkheden is tot aanpassing van uw app eenvoudig als de gegevens worden ontwikkeld. 

Hoe om te voldoen aan de eisen voor de eerste Code-toepassingen met elastische Databasehulpmiddelen Hiermee geeft u de volgende richtlijnen. 

## <a name="data-dependent-routing-using-ef-dbcontext"></a>Gegevens-afhankelijke DbContext EF-routering 

Databaseverbindingen met entiteit Framework worden gewoonlijk beheerd door subklassen van **DbContext**. Deze subklassen maken die zijn afgeleid van **DbContext**. Dit is waar het definiëren van de **DbSets** die de database-backed collecties van CLR-objecten voor uw toepassing te implementeren. In het kader van de gegevensafhankelijke routering, kunnen we verschillende nuttige eigenschappen die niet per se voor andere EF code eerste toepassingsscenario bewaart identificeren: 

* De database al bestaat en in de database elastische shard kaart is geregistreerd. 
* Het schema van de toepassing is al geïmplementeerd met de database (Zie hieronder). 
* Gegevensafhankelijke routering verbindingen met de database zijn brokered door de kaart shard. 

Voor de integratie van **DbContexts** met gegevensafhankelijke routering voor schalen:

1. Fysieke database verbindingen tot stand brengen via de client-database elastische interfaces van de manager van de map shard 
2. De verbinding met de subklasse **DbContext** laten teruglopen
3. Laat de verbinding door in de basisklassen die **DbContext** om ervoor te zorgen dat de verwerking aan de EF ook gebeurt. 

In het volgende voorbeeld ziet u dat deze aanpak. (Deze code is ook in het bijbehorende Visual Studio-project)

    public class ElasticScaleContext<T> : DbContext
    {
    public DbSet<Blog> Blogs { get; set; }
    …

        // C'tor for data dependent routing. This call will open a validated connection 
        // routed to the proper shard by the shard map manager. 
        // Note that the base class c'tor call will fail for an open connection
        // if migrations need to be done and SQL credentials are used. This is the reason for the 
        // separation of c'tors into the data-dependent routing case (this c'tor) and the internal c'tor for new shards.
        public ElasticScaleContext(ShardMap shardMap, T shardingKey, string connectionStr)
            : base(CreateDDRConnection(shardMap, shardingKey, connectionStr), 
            true /* contextOwnsConnection */)
        {
        }

        // Only static methods are allowed in calls into base class c'tors.
        private static DbConnection CreateDDRConnection(
        ShardMap shardMap, 
        T shardingKey, 
        string connectionStr)
        {
            // No initialization
            Database.SetInitializer<ElasticScaleContext<T>>(null);

            // Ask shard map to broker a validated connection for the given key
            SqlConnection conn = shardMap.OpenConnectionForKey<T>
                                (shardingKey, connectionStr, ConnectionOptions.Validate);
            return conn;
        }    

## <a name="main-points"></a>Belangrijkste punten
* Een constructor new vervangt de standaardconstructor in de subklasse DbContext 
* De constructor new worden de argumenten die vereist zijn voor het gegevensafhankelijke routering via elastische database client-bibliotheek:
    * de kaart shard toegang te krijgen tot de gegevens-afhankelijke routeringsinterfaces
    * de sleutel sharding ter identificatie van de shardlet,
    * een verbindingsreeks met de referenties voor de gegevensafhankelijke routering verbinding met de shard. 
 
* De aanroep naar de constructor van de klasse base heeft een detour in een statische methode waarmee alle stappen nodig zijn voor het gegevensafhankelijke routering. 
   * Wordt de oproep van de OpenConnectionForKey van de client-database elastische interfaces op de kaart shard een open verbinding tot stand brengen.
   * De kaart shard maakt de verbinding met de shard die in het bezit van de shardlet voor de opgegeven sharding sleutel openen.
   * Deze open verbinding wordt teruggegeven aan de constructor van de klasse base van DbContext om aan te geven dat deze verbinding wordt gebruikt door EF in plaats van dat EF automatisch een nieuwe verbinding maken. Op deze manier de verbinding zijn door de elastische databaseclient API gelabeld zodat deze consistentie onder shard kaart management bewerkingen kan garanderen.
 
  
De constructor new gebruiken voor de subklasse van DbContext in plaats van de standaardconstructor in uw code. Hier volgt een voorbeeld: 

    // Create and save a new blog.

    Console.Write("Enter a name for a new blog: "); 
    var name = Console.ReadLine(); 

    using (var db = new ElasticScaleContext<int>( 
                            sharding.ShardMap,  
                            tenantId1,  
                            connStrBldr.ConnectionString)) 
    { 
        var blog = new Blog { Name = name }; 
        db.Blogs.Add(blog); 
        db.SaveChanges(); 

        // Display all Blogs for tenant 1 
        var query = from b in db.Blogs 
                    orderby b.Name 
                    select b; 
     … 
    }

De constructor new opent de verbinding met de shard die in het bezit van de gegevens voor de shardlet die door de waarde van **tenantid1**. De code in het blok **met behulp van** blijft toegang tot de **DbSet** voor blogs met EF op de shard voor **tenantid1**ongewijzigd. Hiermee wijzigt u de semantiek voor de code in het met blokkeren zodat alle databasebewerkingen zijn nu binnen het bereik van de ene shard waar **tenantid1** wordt gehouden. Een LINQ query via de blogs **DbSet** retourneert bijvoorbeeld alleen opgeslagen op de huidige shard blogs, maar niet de bestanden die zijn opgeslagen op andere shards.  

#### <a name="transient-faults-handling"></a>Tijdelijke fouten afhandelen
Het team van Microsoft Patterns & Practices gepubliceerd het [De tijdelijke fout afhandeling toepassing blokkeren](https://msdn.microsoft.com/library/dn440719.aspx). De bibliotheek wordt gebruikt met elastische schaal in combinatie met de EF-clientbibliotheek. Er echter voor zorgen dat een tijdelijke uitzondering als resultaat naar een plaats waar we ervoor zorgen gegeven kunnen dat de constructor new wordt gebruikt na een tijdelijke storing zodat elke nieuwe verbindingspoging wordt gedaan met behulp van de constructors die we hebben toegepast. Anders een verbinding met de juiste shard is niet gegarandeerd en er zijn geen garanties dat de verbinding blijft bestaan als er wijzigingen aan de kaart shard optreden. 

In het volgende voorbeeld ziet u hoe een beleid SQL opnieuw kan worden gebruikt om de nieuwe **DbContext** subklasse constructors: 

    SqlDatabaseUtils.SqlRetryPolicy.ExecuteAction(() => 
    { 
        using (var db = new ElasticScaleContext<int>( 
                                sharding.ShardMap,  
                                tenantId1,  
                                connStrBldr.ConnectionString)) 
            { 
                    var blog = new Blog { Name = name }; 
                    db.Blogs.Add(blog); 
                    db.SaveChanges(); 
            … 
            } 
        }); 

**SqlDatabaseUtils.SqlRetryPolicy** in de bovenstaande code wordt gedefinieerd als een **SqlDatabaseTransientErrorDetectionStrategy** met een aantal nieuwe pogingen van 10 en 5 seconden wachttijd tussen herhaalde pogingen. Deze benadering is vergelijkbaar met de richtlijnen voor EF en de gebruiker geïnitieerde transacties (Zie [beperkingen met de uitvoering van strategieën voor opnieuw proberen (ingang EF6)](http://msdn.microsoft.com/data/dn307226). Beide situaties is vereist dat de toepassing bepaalt het bereik de tijdelijke uitzondering als resultaat gegeven: de transactie opnieuw of maak het kader van de juiste constructor (zoals) die gebruikmaakt van de elastische database client library.

De noodzaak om te bepalen waar tijdelijke uitzonderingen in beslag in het bereik wordt ook het gebruik van de ingebouwde **SqlAzureExecutionStrategy** die wordt geleverd met EF uitsluit. **SqlAzureExecutionStrategy** zou opnieuw een verbinding, maar niet met **OpenConnectionForKey** en daarom de validatie die wordt uitgevoerd als onderdeel van de oproep **OpenConnectionForKey** overslaan. In plaats daarvan het codevoorbeeld gebruikt de ingebouwde **DefaultExecutionStrategy** die wordt geleverd met EF. In plaats van **SqlAzureExecutionStrategy**werkt correct in combinatie met het beleid opnieuw tijdelijke fout verwerkt. De uitvoering van beleid is ingesteld in de klasse **ElasticScaleDbConfiguration** . Houd er rekening mee dat we besloten niet te gebruiken **DefaultSqlExecutionStrategy** , aangezien het gebruik van **SqlAzureExecutionStrategy** als tijdelijke uitzonderingen optreden - die tot verkeerde gedrag leiden kunnen zoals besproken wordt voorgesteld. Zie voor meer informatie over de verschillende opnieuw beleid en EF [Verbinding tolerantie in EF](http://msdn.microsoft.com/data/dn456835.aspx).     

#### <a name="constructor-rewrites"></a>Constructor herschreven
De bovenstaande codevoorbeelden illustreren de standaard constructor opnieuw worden geschreven voor uw toepassing vereist voor het gebruik van gegevens afhankelijke routering met het kader van de entiteit. De volgende tabel generaliseert verdwijnt deze aanpak naar andere constructors. 


Huidige Constructor  | De Constructor herschreven voor gegevens | Basis Constructor | Notities
---------- | ----------- | ------------|----------
MyContext() |ElasticScaleContext (ShardMap, TKey) |DbContext (DbConnection, bool) |De verbinding moet een functie van de kaart shard en de gegevensafhankelijke routering sleutel. U moet overbruggen automatisch verbinding maken met EF en in plaats daarvan de connection broker met de shard kaart. 
MyContext(string)|ElasticScaleContext (ShardMap, TKey) |DbContext (DbConnection, bool) |De verbinding is een functie van de kaart shard en de gegevensafhankelijke routering sleutel. Een vaste database naam of de verbindingstekenreeks zal niet werken als ze omloopleiding validatie door de kaart shard. 
MyContext(DbCompiledModel) |ElasticScaleContext (ShardMap, TKey, DbCompiledModel) |DbContext (DbConnection, DbCompiledModel, bool) |De verbinding wordt gemaakt voor de opgegeven shard sharding kaart en de sleutel met het model. Het gecompileerde model zal worden doorgegeven aan de basis c'tor.
MyContext (DbConnection, bool) |ElasticScaleContext (ShardMap, TKey, bool) |DbContext (DbConnection, bool) |De verbinding moet worden afgeleid van de shard-kaart en de sleutel. Dit kan niet als invoer aangeboden (tenzij deze invoer is al de shard-kaart en de sleutel). De Boole-variabele wordt doorgegeven op. 
MyContext (string DbCompiledModel) |ElasticScaleContext (ShardMap, TKey, DbCompiledModel) |DbContext (DbConnection, DbCompiledModel, bool) |De verbinding moet worden afgeleid van de shard-kaart en de sleutel. Dit kan niet als invoer aangeboden (tenzij deze invoer is de shard-kaart en de sleutel). Het gecompileerde model worden doorberekend. 
MyContext (ObjectContext bool) |ElasticScaleContext (ShardMap, TKey, ObjectContext, bool) |DbContext (ObjectContext bool) |De constructor new nodig om ervoor te zorgen dat elke verbinding in de ObjectContext doorgegeven als invoer omgeleid naar een verbinding die wordt beheerd door de elastische schaal wordt. Gedetailleerde informatie over de ObjectContexts valt buiten het bestek van dit document.
MyContext (DbConnection, DbCompiledModel, bool) |ElasticScaleContext (ShardMap, TKey, DbCompiledModel, bool)| DbContext (DbConnection, DbCompiledModel, bool); |De verbinding moet worden afgeleid van de shard-kaart en de sleutel. De verbinding kan niet worden verstrekt als input (tenzij deze invoer is al de shard-kaart en de sleutel). Model en Boole-waarde doorgegeven aan de constructor van de klasse base. 

## <a name="shard-schema-deployment-through-ef-migrations"></a>Shard schema implementatie via de EF migraties 

Automatische Schemabeheer is gemakkelijker te maken die door de entiteit Framework. In het kader van de toepassingen die gebruikmaken van databasehulpmiddelen elastisch, willen we deze mogelijkheid om automatisch te stellen het schema naar de zojuist gemaakte shards wanneer databases worden toegevoegd aan de toepassing een laptopgeheugen behouden. Vergroot de capaciteit op de gegevenslaag voor een laptopgeheugen met EF-toepassingen is de primaire use-case. Vertrouwen op de mogelijkheden voor Schemabeheer van EF minder de inspanning database beheer met een een laptopgeheugen toepassing gebaseerd op EF. 

Implementatie via EF migraties schema werkt het beste op **niet-geopende verbindingen**. Dit is in tegenstelling tot het scenario voor afhankelijke gegevens routering die afhankelijk zijn van de geopende verbinding die door de elastische databaseclient-API. Een ander verschil is de vereiste samenhang: tijdens het wenselijk om ervoor te zorgen dat voor alle gegevensafhankelijke routering verbindingen te beschermen tegen gelijktijdige shard kaart manipulatie, het is niet een probleem met de implementatie van initiële schema naar een nieuwe database die nog niet is geregistreerd in de map shard en nog niet is toegewezen voor het opslaan van shardlets. Daarom vertrouwen we op regelmatige databaseverbindingen voor dit scenario's, in plaats van gegevensafhankelijke routering.  

Dit leidt tot een aanpak waarbij schema implementatie via de EF migraties is nauw verbonden met de registratie van de nieuwe database als een shard in shard-map van de toepassing. Dit is afhankelijk van de volgende vereisten: 

* De database is al gemaakt. 
* De database is leeg: het bevat geen gebruikersschema en geen gebruikersgegevens.
* De database niet nog toegankelijk via de elastische databaseclient-API voor het gegevensafhankelijke routering. 

Met deze vereisten in plaats maken we een gewone niet geopende **SqlConnection** Activeer EF migraties voor schema-implementatie. In het volgende voorbeeld ziet u dat deze aanpak. 

        // Enter a new shard - i.e. an empty database - to the shard map, allocate a first tenant to it  
        // and kick off EF intialization of the database to deploy schema 

        public void RegisterNewShard(string server, string database, string connStr, int key) 
        { 

            Shard shard = this.ShardMap.CreateShard(new ShardLocation(server, database)); 

            SqlConnectionStringBuilder connStrBldr = new SqlConnectionStringBuilder(connStr); 
            connStrBldr.DataSource = server; 
            connStrBldr.InitialCatalog = database; 

            // Go into a DbContext to trigger migrations and schema deployment for the new shard. 
            // This requires an un-opened connection. 
            using (var db = new ElasticScaleContext<int>(connStrBldr.ConnectionString)) 
            { 
                // Run a query to engage EF migrations 
                (from b in db.Blogs 
                    select b).Count(); 
            } 

            // Register the mapping of the tenant to the shard in the shard map. 
            // After this step, data-dependent routing on the shard map can be used 

            this.ShardMap.CreatePointMapping(key, shard); 
        } 
 

In dit voorbeeld ziet de methode **RegisterNewShard** die registreert de shard in de map shard, implementeert u het schema via EF migraties en toewijzing van een verklaring van de shard sharding worden opgeslagen. Dit is afhankelijk van de constructor van de subklasse **DbContext** (**ElasticScaleContext** in de steekproef) waarmee een SQL-verbindingsreeks als invoer. De code van deze constructor is rechtlijnig, zoals in het volgende voorbeeld wordt getoond: 


        // C'tor to deploy schema and migrations to a new shard 
        protected internal ElasticScaleContext(string connectionString) 
            : base(SetInitializerForConnection(connectionString)) 
        { 
        } 

        // Only static methods are allowed in calls into base class c'tors 
        private static string SetInitializerForConnection(string connnectionString) 
        { 
            // We want existence checks so that the schema can get deployed 
            Database.SetInitializer<ElasticScaleContext<T>>( 
        new CreateDatabaseIfNotExists<ElasticScaleContext<T>>()); 

            return connnectionString; 
        } 
 
Een mogelijk gebruikt de versie van de constructor van de klasse base overgenomen. Maar de code nodig om ervoor te zorgen dat de standaard initialiseren voor EF wordt gebruikt wanneer verbinding wordt gemaakt. Vandaar de korte detour in de statische methode aan voordat u aan de constructor van de klasse base met de verbindingsreeks. Houd er rekening mee dat de registratie van shards wordt uitgevoerd in een andere app-domein of een proces om ervoor te zorgen dat de instellingen initialiseren voor EF niet in strijd zijn. 


## <a name="limitations"></a>Beperkingen 

De in dit document beschreven aanpak leiden tot een aantal beperkingen: 

* EF-toepassingen die gebruikmaken van **LocalDb** eerst om te migreren naar een SQL Server-database regelmatig voordat u Elastische database client-bibliotheek. Een toepassing via sharding schalen met elastische schaal is niet mogelijk met **LocalDb**. Houd er rekening mee dat ontwikkeling **LocalDb**nog steeds kunt gebruiken. 

* Wijzigingen in de toepassing die houdt in wijzigingen in de database schema dat moeten EF migraties op alle shards doorlopen. De voorbeeldcode voor dit document wordt niet laten zien hoe u dit doet. Overweeg het gebruik van de Database bijwerken met een parameter ConnectionString worden herhaald voor alle shards; of Pak de T-SQL-script voor de in behandeling zijnde migratie met behulp van de Database bijwerken met het Script de optie en de T-SQL-script van toepassing op uw shards.  

* Een aanvraag wordt gegeven, wordt ervan uitgegaan dat alle van de verwerking van de database zich in een enkel shard zoals aangegeven door de sleutel sharding van de aanvraag. Echter, deze aanname niet altijd over de waarde true. Bijvoorbeeld, wanneer het is niet mogelijk om een sleutel sharding beschikbaar te stellen. Om dit op te lossen, biedt de client-bibliotheek van de **MultiShardQuery** -klasse die een abstractie verbinding voor het uitvoeren van query's via verschillende shards implementeert. Leren de **MultiShardQuery** gebruiken in combinatie met de EF valt buiten het bestek van dit document

## <a name="conclusion"></a>Conclusie

Via de stappen in dit document kunnen EF toepassingen de mogelijkheid de elastische database client-bibliotheek gebruiken voor het gegevensafhankelijke routering door refactoring constructors van de subklassen van de **DbContext** in de EF-toepassing gebruikt. Dit houdt in dat de wijzigingen die vereist zijn op die plaatsen waar **DbContext** klassen bestaan al. EF-toepassingen kunnen bovendien blijven profiteren van automatische schema implementatie door de combinatie van de stappen die de nodige EF migraties met de registratie van nieuwe shards en toewijzingen in de overzichtsweergave shard aanroepen. 


[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-scale-use-entity-framework-applications-visual-studio/sample.png
 