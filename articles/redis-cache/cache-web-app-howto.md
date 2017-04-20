<properties 
    pageTitle="Het maken van een Web App met Cache bestand Vgx. | Microsoft Azure" 
    description="Informatie over het maken van een Web App met Cache bestand Vgx." 
    services="redis-cache" 
    documentationCenter="" 
    authors="steved0x" 
    manager="douge" 
    editor=""/>

<tags 
    ms.service="cache" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="cache-redis" 
    ms.devlang="na" 
    ms.topic="hero-article" 
    ms.date="10/11/2016" 
    ms.author="sdanie"/>

# <a name="how-to-create-a-web-app-with-redis-cache"></a>Het maken van een Web App met Cache bestand Vgx.

> [AZURE.SELECTOR]
- [.NET](cache-dotnet-how-to-use-azure-redis-cache.md)
- [ASP.NET](cache-web-app-howto.md)
- [Node.js](cache-nodejs-get-started.md)
- [Java](cache-java-get-started.md)
- [Python](cache-python-get-started.md)

Deze zelfstudie laat zien hoe maken en implementeren van een ASP.NET-webtoepassing op een web app in Azure App-Service met behulp van Visual Studio 2015. De voorbeeldtoepassing bevat een overzicht van de statistieken team uit een database en worden verschillende manieren getoond Azure bestand Vgx. Cache opslaan en ophalen van gegevens uit de cache gebruiken. Wanneer u de zelfstudie hebt u een actieve web app die leest en schrijft naar een database met Azure bestand Vgx. Cache geoptimaliseerd en gehost in Azure.

U leert:

-   Het maken van een webtoepassing ASP.NET MVC 5 in Visual Studio.
-   Hoe toegang krijgen tot gegevens uit een database met behulp van entiteit Framework.
-   Hoe u de gegevensdoorvoer verbeteren en database laden verkleinen door opslaan en ophalen van gegevens met behulp van Azure bestand Vgx. Cache.
-   Het gebruik van een bestand Vgx. set op te halen van de top 5 teams gesorteerd.
-   Het inrichten van de Azure middelen voor de toepassing met behulp van de sjabloon voor een Resource Manager.
-   Het publiceren van de toepassing op Azure met behulp van Visual Studio.

## <a name="prerequisites"></a>Vereisten

Om de zelfstudie te voltooien, moet u de volgende vereisten hebben.

-   [Azure-account](#azure-account)
-   [Visual Studio 2015 met de Azure SDK voor .NET](#visual-studio-2015-with-the-azure-sdk-for-net)

### <a name="azure-account"></a>Azure-account

U moet een Azure rekening om de zelfstudie te voltooien. U kunt:

* [Gratis een Azure account openen](/pricing/free-trial/?WT.mc_id=redis_cache_hero). U krijgt credits die kunnen worden gebruikt om te proberen betaald Azure Services. Zelfs nadat de kredieten zijn gebruikt, kunt u de rekening houden en gratis Azure services en functies gebruiken.
* [Visual Studio activeren abonnee vergoedingen](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=redis_cache_hero). Uw MSDN-abonnement geeft u credits elke maand die u voor betaalde Azure services gebruiken kunt.

### <a name="visual-studio-2015-with-the-azure-sdk-for-net"></a>Visual Studio 2015 met de Azure SDK voor .NET

De zelfstudie is geschreven voor Visual Studio 2015 met de [SDK voor .NET Azure](../dotnet-sdk.md) punt 2.8.2 of hoger. [De meest recente Azure SDK voor Visual Studio 2015 hier downloaden](http://go.microsoft.com/fwlink/?linkid=518003). Visual Studio wordt automatisch geïnstalleerd met de SDK als u nog geen hebt.

Als u beschikt over Visual Studio 2013, kunt u [downloaden van de nieuwste Azure SDK voor Visual Studio 2013](http://go.microsoft.com/fwlink/?LinkID=324322). Sommige schermen kunnen er anders uitzien dan de illustraties in deze zelfstudie wordt weergegeven.

>[AZURE.NOTE] Afhankelijk van hoeveel van de SDK-afhankelijkheden er al op uw computer, kan installeert de SDK lang duren, van enkele minuten tot een half uur of meer.

## <a name="create-the-visual-studio-project"></a>De Visual Studio-project maken

1. Visual Studio openen en klik op **bestand**, **Nieuw** **Project**.

2. Vouw het knooppunt **Visual C#** in de lijst met **sjablonen** , **Cloud**selecteren en op **ASP.NET-webtoepassingen**. Zorg ervoor dat **.NET Framework 4.5.2** is geselecteerd.  Typ **ContosoTeamStats** in het tekstvak **naam** en klik op **OK**.
 
    ![Project maken][cache-create-project]

3. **MVC** selecteert als het projecttype. Schakel het selectievakje **Host in de cloud** . U zult [de Azure bronnen ter beschikking stellen](#provision-the-azure-resources) en [de toepassing uitgeeft aan Azure](#publish-the-application-to-azure) in opeenvolgende stappen in de zelfstudie. Zie voor een voorbeeld van een App Service web app vanuit Visual Studio ingericht door te laten **opnemen in de cloud** , gecontroleerd, [aan de slag met Web-Apps in App-Azure Service, met behulp van ASP.NET en Visual Studio](../app-service-web/web-sites-dotnet-get-started.md).

    ![Selecteer de project-sjabloon][cache-select-template]

4. Klik op **OK** om het project te maken.

## <a name="create-the-aspnet-mvc-application"></a>De ASP.NET MVC-toepassing maken

In deze sectie van de handleiding maakt u de eenvoudige toepassing die gelezen en statistieken team uit een database weergegeven.

-   [Het model toevoegen](#add-the-model)
-   [De controller toevoegen](#add-the-controller)
-   [De weergaven configureren](#configure-the-views)

### <a name="add-the-model"></a>Het model toevoegen

1. **Modellen** in de **Solution Explorer**met de rechtermuisknop en kies **toevoegen**, **klasse**. 

    ![Model toevoegen][cache-model-add-class]

2. Voer `Team` voor de klasse een naam en klik op **toevoegen**.

    ![Klasse model toevoegen][cache-model-add-class-dialog]

3. Vervang de `using` instructies aan de bovenkant van de `Team.cs` -bestand met de volgende instructies.


        using System;
        using System.Collections.Generic;
        using System.Data.Entity;
        using System.Data.Entity.SqlServer;


4. Vervang de definitie van de `Team` klasse met het volgende codefragment met een bijgewerkte `Team` definitie, evenals enkele andere entiteit Framework hulpklassen klasse. Zie voor meer informatie over de eerste methode code entiteit kader dat wordt gebruikt in deze zelfstudie, [Code eerst naar een nieuwe database](https://msdn.microsoft.com/data/jj193542).


        public class Team
        {
            public int ID { get; set; }
            public string Name { get; set; }
            public int Wins { get; set; }
            public int Losses { get; set; }
            public int Ties { get; set; }
        
            static public void PlayGames(IEnumerable<Team> teams)
            {
                // Simple random generation of statistics.
                Random r = new Random();
        
                foreach (var t in teams)
                {
                    t.Wins = r.Next(33);
                    t.Losses = r.Next(33);
                    t.Ties = r.Next(0, 5);
                }
            }
        }
        
        public class TeamContext : DbContext
        {
            public TeamContext()
                : base("TeamContext")
            {
            }
        
            public DbSet<Team> Teams { get; set; }
        }
        
        public class TeamInitializer : CreateDatabaseIfNotExists<TeamContext>
        {
            protected override void Seed(TeamContext context)
            {
                var teams = new List<Team>
                {
                    new Team{Name="Adventure Works Cycles"},
                    new Team{Name="Alpine Ski House"},
                    new Team{Name="Blue Yonder Airlines"},
                    new Team{Name="Coho Vineyard"},
                    new Team{Name="Contoso, Ltd."},
                    new Team{Name="Fabrikam, Inc."},
                    new Team{Name="Lucerne Publishing"},
                    new Team{Name="Northwind Traders"},
                    new Team{Name="Consolidated Messenger"},
                    new Team{Name="Fourth Coffee"},
                    new Team{Name="Graphic Design Institute"},
                    new Team{Name="Nod Publishers"}
                };
        
                Team.PlayGames(teams);
        
                teams.ForEach(t => context.Teams.Add(t));
                context.SaveChanges();
            }
        }
        
        public class TeamConfiguration : DbConfiguration
        {
            public TeamConfiguration()
            {
                SetExecutionStrategy("System.Data.SqlClient", () => new SqlAzureExecutionStrategy());
            }
        }


2. Dubbelklik op **web.config** om deze te openen in de **Solution Explorer**.

    ![Web.config][cache-web-config]

3.  Toevoegen van de volgende verbindingsreeks met de `connectionStrings` sectie. De naam van de tekenreeks moet overeenkomen met de naam van de entiteit Framework database context klasse `TeamContext`.

        <add name="TeamContext" connectionString="Data Source=(LocalDB)\v11.0;AttachDbFilename=|DataDirectory|\Teams.mdf;Integrated Security=True" providerName="System.Data.SqlClient" />


    Na het toevoegen van dit, de `connectionStrings` ziet er als volgt uit.


        <connectionStrings>
            <add name="DefaultConnection" connectionString="Data Source=(LocalDb)\MSSQLLocalDB;AttachDbFilename=|DataDirectory|\aspnet-ContosoTeamStats-20160216120918.mdf;Initial Catalog=aspnet-ContosoTeamStats-20160216120918;Integrated Security=True"
                providerName="System.Data.SqlClient" />
            <add name="TeamContext" connectionString="Data Source=(LocalDB)\v11.0;AttachDbFilename=|DataDirectory|\Teams.mdf;Integrated Security=True"  providerName="System.Data.SqlClient" />
        </connectionStrings>

### <a name="add-the-controller"></a>De controller toevoegen

1. Druk op **F6** om te bouwen van het project. 
2. In de **Solution Explorer**met de rechtermuisknop op de map **Controllers** en kies **toevoegen**, **Controller**.

    ![Domeincontroller toevoegen][cache-add-controller]

3. **MVC 5 Controller met weergaven, entiteit Framework gebruiken**, en klik op **toevoegen**. Als u een foutbericht nadat u op **toevoegen**te klikken, zorgen ervoor dat u het project eerst hebt gemaakt.

    ![Controller-klasse toevoegen][cache-add-controller-class]

5. Selecteer **Team (ContosoTeamStats.Models)** in de vervolgkeuzelijst **klasse Model** . **TeamContext (ContosoTeamStats.Models)** selecteert in de vervolgkeuzelijst **klasse context** . Type `TeamsController` in het tekstvak voor de **Controller** (als deze niet automatisch ingevuld). Klik op **toevoegen** om de controller klasse maken en toevoegen van de standaardweergaven.

    ![Domeincontroller configureren][cache-configure-controller]

4. In de **Solution Explorer** **Global.asax** uitgebreid en dubbelklik op **Global.asax.cs** om deze te openen.

    ![Global.asax.cs][cache-global-asax]

5. Toevoegen van de volgende twee instructies gebruiken aan het begin van het bestand in de andere instructies.


        using System.Data.Entity;
        using ContosoTeamStats.Models;


6. Voeg de volgende regel code toe aan het einde van de `Application_Start` methode.


        Database.SetInitializer<TeamContext>(new TeamInitializer());


7. Vouw in de **Solution Explorer**, `App_Start` en dubbelklik op `RouteConfig.cs`.

    ![RouteConfig.cs][cache-RouteConfig-cs]

8. Vervangen `controller = "Home"` in de volgende code in de `RegisterRoutes` methode met `controller = "Teams"` zoals in het volgende voorbeeld wordt getoond.


        routes.MapRoute(
            name: "Default",
            url: "{controller}/{action}/{id}",
            defaults: new { controller = "Teams", action = "Index", id = UrlParameter.Optional }
        );


### <a name="configure-the-views"></a>De weergaven configureren

1. Vouw de map **weergaven** en klik op de **gedeelde** map en dubbelklik op **_Layout.cshtml**in de **Solution Explorer**. 

    ![_Layout.cshtml][cache-layout-cshtml]

2. Wijzig de inhoud van de `title` -element en vervangen `My ASP.NET Application` met `Contoso Team Stats` zoals in het volgende voorbeeld wordt getoond.


        <title>@ViewBag.Title - Contoso Team Stats</title>


3. In de `body` sectie, het bijwerken van de eerste `Html.ActionLink` -instructie en vervangen `Application name` met `Contoso Team Stats` en vervangt u `Home` met `Teams`.
    -   Vóór:`@Html.ActionLink("Application name", "Index", "Home", new { area = "" }, new { @class = "navbar-brand" })`
    -   Na:`@Html.ActionLink("Contoso Team Stats", "Index", "Teams", new { area = "" }, new { @class = "navbar-brand" })`

    ![Codewijzigingen][cache-layout-cshtml-code]

4. Druk op **Ctrl + F5** om te bouwen en uitvoeren van de toepassing. Deze versie van de toepassing leest de resultaten rechtstreeks uit de database. Opmerking de acties **Nieuw**, **bewerken**, **Details**en **verwijderen** die automatisch zijn toegevoegd aan de toepassing door de scaffold **MVC 5 Controller met weergaven met entiteit-Framework** . In het volgende gedeelte van de zelfstudie voegt u bestand Vgx. Cache om de toegang tot gegevens te optimaliseren en bieden extra voorzieningen voor de toepassing.

![Starter-toepassing][cache-starter-application]

## <a name="configure-the-application-to-use-redis-cache"></a>De toepassing configureren voor gebruik van Cache bestand Vgx.

In dit gedeelte van de zelfstudie, zult u de voorbeeldtoepassing opslaan en ophalen van Contoso team statistieken van een exemplaar van het bestand Vgx. Azure-Cache via de client [StackExchange.Redis](https://github.com/StackExchange/StackExchange.Redis) cache configureren.

-   [De toepassing configureren voor gebruik van StackExchange.Redis](#configure-the-application-to-use-stackexchangeredis)
-   [De klasse TeamsController retourneert de resultaten uit de cache of de database bijwerken](#update-the-teamscontroller-class-to-return-results-from-the-cache-or-the-database)
-   [De methoden maken, bewerken en verwijderen om te werken met de cache bijwerken](#update-the-create-edit-and-delete-methods-to-work-with-the-cache)
-   [De Teams Index als u wilt werken met de cache bijwerken](#update-the-teams-index-view-to-work-with-the-cache)


### <a name="configure-the-application-to-use-stackexchangeredis"></a>De toepassing configureren voor gebruik van StackExchange.Redis

1. Een clienttoepassing in Visual Studio met behulp van het pakket StackExchange.Redis NuGet configureren, klik met de rechtermuisknop op het project in de **Solution Explorer** en kies **Beheren NuGet pakketten**. 

    ![NuGet pakketten beheren][redis-cache-manage-nuget-menu]

2. **StackExchange.Redis** in het zoekvak typt, selecteert u de gewenste versie uit de resultaten en klik op **installeren**.

    ![StackExchange.Redis NuGet package][redis-cache-stack-exchange-nuget]

    Het pakket NuGet gedownload en voegt de vereiste assembly-verwijzing voor de clienttoepassing toegang tot bestand Vgx. Azure-Cache met de cache StackExchange.Redis client. Als u liever een sterke naam versie van de bibliotheek **StackExchange.Redis** -client gebruiken, kiest u **StackExchange.Redis.StrongName**; Klik anders op **StackExchange.Redis**.

3. Vouw de map met **domeincontrollers** in de **Solution Explorer**en dubbelklik op **TeamsController.cs** om deze te openen.

    ![Teams-controller][cache-teamscontroller]

4. Toevoegen van de volgende twee instructies naar **TeamsController.cs**.

        using System.Configuration;
        using StackExchange.Redis;

5. Toevoegen van de volgende twee eigenschappen van de `TeamsController` klasse.

        // Redis Connection string info
        private static Lazy<ConnectionMultiplexer> lazyConnection = new Lazy<ConnectionMultiplexer>(() =>
        {
            string cacheConnection = ConfigurationManager.AppSettings["CacheConnection"].ToString();
            return ConnectionMultiplexer.Connect(cacheConnection);
        });
    
        public static ConnectionMultiplexer Connection
        {
            get
            {
                return lazyConnection.Value;
            }
        }
  
1. Maak een bestand op uw computer met de naam `WebAppPlusCacheAppSecrets.config` en plaats deze op een locatie die won't worden gecontroleerd met de broncode van de voorbeeldtoepassing moet u beslissen het inchecken ergens. In dit voorbeeld de `AppSettingsSecrets.config` bestand bevindt zich op `C:\AppSecrets\WebAppPlusCacheAppSecrets.config`.

    Bewerkt de `WebAppPlusCacheAppSecrets.config` -bestand en de volgende inhoud toevoegen. Als u de toepassing lokaal uitgevoerd wordt deze informatie gebruikt voor verbinding met uw exemplaar van het bestand Vgx. Azure-Cache. Verderop in de zelfstudie voor u inrichten van een exemplaar van het bestand Vgx. Azure-Cache en bijwerken van de Cachenaam en wachtwoord. Als u niet van plan bent de voorbeeldtoepassing lokaal uitvoeren kunt u het maken van dit bestand en de volgende stappen die verwijzen naar het bestand overslaan omdat bij het implementeren van Azure de toepassing de gegevens van de cache van de instelling voor het Web App van de app en niet van dit bestand haalt. Aangezien de `WebAppPlusCacheAppSecrets.config` niet is geïmplementeerd op Azure met uw toepassing, hoeft u niet het tenzij u gaat de toepassing lokaal wordt uitgevoerd.


        <appSettings>
          <add key="CacheConnection" value="MyCache.redis.cache.windows.net,abortConnect=false,ssl=true,password=..."/>
        </appSettings>


2. Dubbelklik op **web.config** om deze te openen in de **Solution Explorer**.

    ![Web.config][cache-web-config]

3. Voeg de volgende `file` kenmerk aan de `appSettings` element. Als u een andere naam of locatie hebt gebruikt, vervangen door de waarden voor de voorbeelden in het voorbeeld.
    -   Vóór:`<appSettings>`
    -   Na:` <appSettings file="C:\AppSecrets\WebAppPlusCacheAppSecrets.config">`

    De ASP.NET-runtime worden samengevoegd voor de inhoud van het externe bestand met de opmaak van de `<appSettings>` element. De runtime wordt het kenmerk genegeerd als het opgegeven bestand kan niet worden gevonden. Uw geheimen (de verbindingstekenreeks aan uw cache) wordt niet opgenomen als onderdeel van de broncode van de toepassing. Wanneer u uw web app op Azure implementeren, het `WebAppPlusCacheAppSecrests.config` bestand won't worden geïmplementeerd (dat is wat u wilt). Er zijn verschillende manieren om op te geven van deze geheimen in Azure en in deze zelfstudie ze automatisch geconfigureerd voor u wanneer [de Azure bronnen ter beschikking stellen](#provision-the-azure-resources) van een zelfstudie voor latere stap. Zie [Aanbevolen procedures voor het implementeren van wachtwoorden en andere gevoelige gegevens ASP.NET en Azure App-Service](http://www.asp.net/identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure)voor meer informatie over het werken met geheimen in Azure.


### <a name="update-the-teamscontroller-class-to-return-results-from-the-cache-or-the-database"></a>De klasse TeamsController retourneert de resultaten uit de cache of de database bijwerken

In dit voorbeeld wordt worden de statistieken team opgehaald uit de database of uit de cache. Team statistieken worden opgeslagen in de cache als een geserialiseerde `List<Team>`, en ook als een gesorteerde set bestand Vgx. gegevenstypen gebruiken. Wanneer u items ophaalt uit een gesorteerde set, kunt u sommige, alle ophalen of query uitvoert voor bepaalde artikelen. In dit voorbeeld moet u de gesorteerde set voor de top 5 teams, gerangschikt op aantal wins opvragen.

>[AZURE.NOTE] Het is niet vereist voor het opslaan van de statistieken van het team in meerdere indelingen in de cache Azure bestand Vgx. Cache gebruiken. In deze zelfstudie wordt verschillende indelingen om aan te tonen dat de verschillende manieren en andere gegevenstypen die kunt u cachegegevens.



1. Voeg de volgende instructies om de `TeamsController.cs` bestand bovenaan met de andere instructies.

        using System.Diagnostics;
        using Newtonsoft.Json;

2. Vervang de huidige `public ActionResult Index()` methode met de volgende implementatie.


        // GET: Teams
        public ActionResult Index(string actionType, string resultType)
        {
            List<Team> teams = null;
        
            switch(actionType)
            {
                case "playGames": // Play a new season of games.
                    PlayGames();
                    break;
        
                case "clearCache": // Clear the results from the cache.
                    ClearCachedTeams();
                    break;
        
                case "rebuildDB": // Rebuild the database with sample data.
                    RebuildDB();
                    break;
            }
        
            // Measure the time it takes to retrieve the results.
            Stopwatch sw = Stopwatch.StartNew();
        
            switch(resultType)
            {
                case "teamsSortedSet": // Retrieve teams from sorted set.
                    teams = GetFromSortedSet();
                    break;
        
                case "teamsSortedSetTop5": // Retrieve the top 5 teams from the sorted set.
                    teams = GetFromSortedSetTop5();
                    break;
        
                case "teamsList": // Retrieve teams from the cached List<Team>.
                    teams = GetFromList();
                    break;
        
                case "fromDB": // Retrieve results from the database.
                default:
                    teams = GetFromDB();
                    break;
            }
        
            sw.Stop();
            double ms = sw.ElapsedTicks / (Stopwatch.Frequency / (1000.0));

            // Add the elapsed time of the operation to the ViewBag.msg.
            ViewBag.msg += " MS: " + ms.ToString();
        
            return View(teams);
        }


3. Toevoegen van de volgende drie methoden om de `TeamsController` klasse voor de uitvoering van de `playGames`, `clearCache`, en `rebuildDB` actietypen uit de switchinstructie in het vorige codefragment toegevoegd.

    De `PlayGames` methode worden de statistieken team bijgewerkt door het simuleren van een seizoen van games, de resultaten worden opgeslagen in de database en wist de nu verouderde gegevens uit de cache.


        void PlayGames()
        {
            ViewBag.msg += "Updating team statistics. ";
            // Play a "season" of games.
            var teams = from t in db.Teams
                        select t;
    
            Team.PlayGames(teams);
    
            db.SaveChanges();
    
            // Clear any cached results
            ClearCachedTeams();
        }


    De `RebuildDB` methode wordt de database met de standaardset teams opnieuw geïnitialiseerd statistieken gegenereerd voor hen en wist de nu verouderde gegevens uit de cache.
    
        void RebuildDB()
        {
            ViewBag.msg += "Rebuilding DB. ";
            // Delete and re-initialize the database with sample data.
            db.Database.Delete();
            db.Database.Initialize(true);

            // Clear any cached results
            ClearCachedTeams();
        }


    De `ClearCachedTeams` methode statistieken team in de cache worden verwijderd uit de cache.

    
        void ClearCachedTeams()
        {
            IDatabase cache = Connection.GetDatabase();
            cache.KeyDelete("teamsList");
            cache.KeyDelete("teamsSortedSet");
            ViewBag.msg += "Team data removed from cache. ";
        } 


4. Toevoegen van de volgende vier methoden voor het `TeamsController` klasse voor de uitvoering van de verschillende manieren om de statistieken van het team uit de cache en de database op te halen. De methode geeft als resultaat een `List<Team>` die wordt weergegeven in de weergave.

    De `GetFromDB` methode leest de statistieken van het team van de database.

        List<Team> GetFromDB()
        {
            ViewBag.msg += "Results read from DB. ";
            var results = from t in db.Teams
                orderby t.Wins descending
                select t; 
    
            return results.ToList<Team>();
        }


    De `GetFromList` methode leest de statistieken van het team uit de cache als een geserialiseerde `List<Team>`. Als er een cache-missers, worden de statistieken team lezen uit de database en vervolgens in de cache zijn opgeslagen voor volgende keer. In dit voorbeeld we JSON.NET serialisatie serialiseren van de .NET-objecten naar en uit de cache gebruiken. Zie [werken met objecten in de Cache van Azure bestand Vgx. .NET](cache-dotnet-how-to-use-azure-redis-cache.md#work-with-net-objects-in-the-cache)voor meer informatie.

        List<Team> GetFromList()
        {
            List<Team> teams = null;

            IDatabase cache = Connection.GetDatabase();
            string serializedTeams = cache.StringGet("teamsList");
            if (!String.IsNullOrEmpty(serializedTeams))
            {
                teams = JsonConvert.DeserializeObject<List<Team>>(serializedTeams);

                ViewBag.msg += "List read from cache. ";
            }
            else
            {
                ViewBag.msg += "Teams list cache miss. ";
                // Get from database and store in cache
                teams = GetFromDB();

                ViewBag.msg += "Storing results to cache. ";
                cache.StringSet("teamsList", JsonConvert.SerializeObject(teams));
            }
            return teams;
        }


    De `GetFromSortedSet` methode de statistieken van het team wordt gelezen uit een cache gesorteerde set. Als er een cache-missers, worden de statistieken team lezen uit de database en opgeslagen in de cache als gesorteerde set.


        List<Team> GetFromSortedSet()
        {
            List<Team> teams = null;
            IDatabase cache = Connection.GetDatabase();
            // If the key teamsSortedSet is not present, this method returns a 0 length collection.
            var teamsSortedSet = cache.SortedSetRangeByRankWithScores("teamsSortedSet", order: Order.Descending);
            if (teamsSortedSet.Count() > 0)
            {
                ViewBag.msg += "Reading sorted set from cache. ";
                teams = new List<Team>();
                foreach (var t in teamsSortedSet)
                {
                    Team tt = JsonConvert.DeserializeObject<Team>(t.Element);
                    teams.Add(tt);
                }
            }
            else
            {
                ViewBag.msg += "Teams sorted set cache miss. ";
    
                // Read from DB
                teams = GetFromDB();
    
                ViewBag.msg += "Storing results to cache. ";
                foreach (var t in teams)
                {
                    Console.WriteLine("Adding to sorted set: {0} - {1}", t.Name, t.Wins);
                    cache.SortedSetAdd("teamsSortedSet", JsonConvert.SerializeObject(t), t.Wins);
                }
            }
            return teams;
        }


    De `GetFromSortedSetTop5` methode leest de top 5 teams van de gesorteerde set die in de cache. Het begint met het controleren van de cache voor het bestaan van de `teamsSortedSet` sleutel. Als deze sleutel niet aanwezig is, de `GetFromSortedSet` methode aangeroepen om de statistieken team lezen en opslaan in de cache. Vervolgens wordt de cache gesorteerde set opgevraagd voor de top 5 teams die worden geretourneerd.


        List<Team> GetFromSortedSetTop5()
        {
            List<Team> teams = null;
            IDatabase cache = Connection.GetDatabase();

            // If the key teamsSortedSet is not present, this method returns a 0 length collection.
            var teamsSortedSet = cache.SortedSetRangeByRankWithScores("teamsSortedSet", stop: 4, order: Order.Descending);
            if(teamsSortedSet.Count() == 0)
            {
                // Load the entire sorted set into the cache.
                GetFromSortedSet();

                // Retrieve the top 5 teams.
                teamsSortedSet = cache.SortedSetRangeByRankWithScores("teamsSortedSet", stop: 4, order: Order.Descending);
            }

            ViewBag.msg += "Retrieving top 5 teams from cache. ";
            // Get the top 5 teams from the sorted set
            teams = new List<Team>();
            foreach (var team in teamsSortedSet)
            {
                teams.Add(JsonConvert.DeserializeObject<Team>(team.Element));
            }
            return teams;
        }


### <a name="update-the-create-edit-and-delete-methods-to-work-with-the-cache"></a>De methoden maken, bewerken en verwijderen om te werken met de cache bijwerken

De steiger-code die is gegenereerd als onderdeel van dit voorbeeld bevat methoden voor het toevoegen, bewerken en verwijderen van teams. Wanneer een team is toegevoegd, bewerkt of verwijderd, wordt de gegevens in de cache verouderd. In deze sectie zult u deze drie methoden om de teams in de cache te wissen, zodat de cache niet gesynchroniseerd met de database te wijzigen.

1. Ga naar de `Create(Team team)` -methode in de `TeamsController` klasse. Toevoegen van een aanroep naar de `ClearCachedTeams` methode, zoals in het volgende voorbeeld wordt getoond.


        // POST: Teams/Create
        // To protect from overposting attacks, please enable the specific properties you want to bind to, for 
        // more details see http://go.microsoft.com/fwlink/?LinkId=317598.
        [HttpPost]
        [ValidateAntiForgeryToken]
        public ActionResult Create([Bind(Include = "ID,Name,Wins,Losses,Ties")] Team team)
        {
            if (ModelState.IsValid)
            {
                db.Teams.Add(team);
                db.SaveChanges();
                // When a team is added, the cache is out of date.
                // Clear the cached teams.
                ClearCachedTeams();
                return RedirectToAction("Index");
            }
    
            return View(team);
        }


2. Ga naar de `Edit(Team team)` -methode in de `TeamsController` klasse. Toevoegen van een aanroep naar de `ClearCachedTeams` methode, zoals in het volgende voorbeeld wordt getoond.


        // POST: Teams/Edit/5
        // To protect from overposting attacks, please enable the specific properties you want to bind to, for 
        // more details see http://go.microsoft.com/fwlink/?LinkId=317598.
        [HttpPost]
        [ValidateAntiForgeryToken]
        public ActionResult Edit([Bind(Include = "ID,Name,Wins,Losses,Ties")] Team team)
        {
            if (ModelState.IsValid)
            {
                db.Entry(team).State = EntityState.Modified;
                db.SaveChanges();
                // When a team is edited, the cache is out of date.
                // Clear the cached teams.
                ClearCachedTeams();
                return RedirectToAction("Index");
            }
            return View(team);
        }


3. Ga naar de `DeleteConfirmed(int id)` -methode in de `TeamsController` klasse. Toevoegen van een aanroep naar de `ClearCachedTeams` methode, zoals in het volgende voorbeeld wordt getoond.


        // POST: Teams/Delete/5
        [HttpPost, ActionName("Delete")]
        [ValidateAntiForgeryToken]
        public ActionResult DeleteConfirmed(int id)
        {
            Team team = db.Teams.Find(id);
            db.Teams.Remove(team);
            db.SaveChanges();
            // When a team is deleted, the cache is out of date.
            // Clear the cached teams.
            ClearCachedTeams();
            return RedirectToAction("Index");
        }


### <a name="update-the-teams-index-view-to-work-with-the-cache"></a>De Teams Index als u wilt werken met de cache bijwerken

1. Vouw de map **weergaven** , vervolgens de map **Teams** in de **Solution Explorer**en dubbelklik op **Index.cshtml**.

    ![Index.cshtml][cache-views-teams-index-cshtml]

2. Zoek de volgende alinea-element in de buurt van de bovenkant van het bestand.

    ![De actietabel][cache-teams-index-table]

    Dit is de koppeling voor het maken van een nieuw team. Het element alinea vervangen door de volgende tabel. Deze tabel bevat Actiekoppelingen naar de voor het maken van een nieuw team, een nieuwe seizoen van spelletjes spelen de cache wissen, ophalen van de teams van de cache in verschillende indelingen, teams ophalen uit de database en de database met voorbeeldgegevens vers opnieuw.


        <table class="table">
            <tr>
                <td>
                    @Html.ActionLink("Create New", "Create")
                </td>
                <td>
                    @Html.ActionLink("Play Season", "Index", new { actionType = "playGames" })
                </td>
                <td>
                    @Html.ActionLink("Clear Cache", "Index", new { actionType = "clearCache" })
                </td>
                <td>
                    @Html.ActionLink("List from Cache", "Index", new { resultType = "teamsList" })
                </td>
                <td>
                    @Html.ActionLink("Sorted Set from Cache", "Index", new { resultType = "teamsSortedSet" })
                </td>
                <td>
                    @Html.ActionLink("Top 5 Teams from Cache", "Index", new { resultType = "teamsSortedSetTop5" })
                </td>
                <td>
                    @Html.ActionLink("Load from DB", "Index", new { resultType = "fromDB" })
                </td>
                <td>
                    @Html.ActionLink("Rebuild DB", "Index", new { actionType = "rebuildDB" })
                </td>
            </tr>    
        </table>


3. Ga naar de onderkant van het bestand **Index.cshtml** en voeg de volgende `tr` element, zodat deze de laatste rij in de laatste tabel in het bestand.

        <tr><td colspan="5">@ViewBag.Msg</td></tr>

    Deze rij bevat de waarde van `ViewBag.Msg` waarin een rapport over de huidige bewerking die wordt ingesteld wanneer u op een van de koppelingen uit de vorige stap.   

    ![Statusbericht][cache-status-message]

4. Druk op **F6** om te bouwen van het project.

## <a name="provision-the-azure-resources"></a>De Azure bronnen ter beschikking stellen

Als host voor de toepassing in Azure, moet u eerst de Azure services waarvoor uw toepassing inrichten. De voorbeeldtoepassing in deze zelfstudie worden de volgende Azure services gebruikt.

-   Cache bestand Vgx Azure.
-   App Service Web App
-   SQL-Database

Voor de implementatie van deze diensten aan een nieuwe of bestaande groep van uw keuze, klikt u op de volgende **distribueren naar Azure** .

[! [Implementeren op Azure] [deploybutton]](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-web-app-redis-cache-sql-database%2Fazuredeploy.json)

Deze knop **distribueren naar Azure** wordt het [maken van een Web App plus bestand Vgx. Cache en Database SQL](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-redis-cache-sql-database) [Azure Quickstart](https://github.com/Azure/azure-quickstart-templates) sjabloon richt deze services en stelt u de verbindingsreeks voor de SQL-Database en de toepassingsinstelling van voor de verbindingsreeks Azure bestand Vgx. Cache.

>[AZURE.NOTE] Als u geen Azure account hebt, kunt u [een gratis account Azure maken](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=redis_cache_hero) binnen een paar minuten.

**Distribueren naar Azure** te klikken, gaat u naar de portal Azure en start het proces van het maken van de bronnen die worden beschreven in de sjabloon.

![Implementeren op Azure][cache-deploy-to-azure-step-1]

1. Op de **implementatie van aangepaste** blade, selecteert u het abonnement Azure te gebruiken, en selecteert u een bestaande resourcegroep of een nieuwe maken en geef de locatie van de groep resource.
2. Geef de naam van een administrator-account op de bladeserver **Parameters** (**ADMINISTRATORLOGIN** - **admin**niet gebruiken), aanmelding beheerderswachtwoord (**ADMINISTRATORLOGINPASSWORD**), en de database (**DATABASENAAM**). De overige parameters worden geconfigureerd voor een gratis App Service hosting plan en opties voor lagere kosten voor de SQL-Database en Azure Cache bestand Vgx., die niet met een vrij laag komen.
3. Een van de andere instellingen desgewenst wijzigen of houden de standaardinstellingen en klik op **OK**.


![Implementeren op Azure][cache-deploy-to-azure-step-2]

1. Klik op **de juridische voorwaarden controleren**.
2. Lees de voorwaarden op de **Inkoop** -blade en klik op **Inkoop**.
3. Om te beginnen met het inrichten van de resources, klikt u op **maken** op de **implementatie van aangepaste** blade.

De voortgang van de installatie, klikt u op het pictogram in het systeemvak en klik op **de implementatie is gestart**.

![Implementatie gestart][cache-deployment-started]

U kunt de status van uw implementatie weergeven op het blad **Microsoft.Template** .

![Implementeren op Azure][cache-deploy-to-azure-step-3]

Bij het inrichten is voltooid, kunt u uw toepassing Azure vanuit Visual Studio kunt publiceren.

>[AZURE.NOTE] Eventuele fouten die tijdens het inrichtingsproces optreden kunnen worden weergegeven op het blad **Microsoft.Template** . Veelvoorkomende fouten zijn te veel SQL Servers of te veel gratis App Service hosting plannen per abonnement. Corrigeer eventuele fouten en het proces opnieuw starten door te klikken op **opnieuw implementeren** op het blad van de **Microsoft.Template** of de knop **distribueren naar Azure** in deze zelfstudie.

## <a name="publish-the-application-to-azure"></a>Publiceer de toepassing naar Azure

In deze stap van de zelfstudie is u de toepassing uitgeeft aan Azure en uitvoeren in de cloud.

1. Klik met de rechtermuisknop op het **ContosoTeamStats** -project in Visual Studio en kies **publiceren**.

    ![Publiceren][cache-publish-app]

2. Klik op **Microsoft Azure App-Service**.

    ![Publiceren][cache-publish-to-app-service]

3. Selecteer het abonnement gebruikt bij het maken van de bronnen Azure, vouw de bronnengroep met de bronnen en selecteert u de gewenste Web App klikt u op **OK**. Als u de knop **distribueren naar Azure** is uw Web App naam met de **webSite begint** , gevolgd door enkele extra tekens gebruikt.

    ![Selecteer Web App][cache-select-web-app]

4. **Verbinding controleren** om te controleren of de instellingen op en klik vervolgens op **publiceren**.

    ![Publiceren][cache-publish]

    Na een paar seconden het publicatieproces wordt voltooid en wordt een browser gestart met de uitvoering voorbeeldtoepassing. Als u een DNS-fout bij het valideren of te publiceren en het inrichtingsproces voor de Azure resources voor de toepassing is onlangs voltooid, wacht even en probeer het opnieuw.

    ![Cache toegevoegd][cache-added-to-application]

De volgende tabel beschrijft de actiekoppeling van elke van de voorbeeldtoepassing.

| Actie                  | Beschrijving                                                                                                                                                      |
|-------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Nieuw maken              | Maak een nieuw Team.                                                                                                                                               |
| Seizoen spelen             | Een seizoen van spelletjes spelen, de statistieken team bijwerken en een verouderde gegevens uit de cache-team.                                                                          |
| Cache wissen             | De statistieken team uit de cache wissen.                                                                                                                             |
| Lijst van Cache         | De statistieken van het team uit de cache opgehaald. Als er een cache-missers, de statistieken van de database laden en opslaan in de cache voor volgende keer.                                        |
| Gesorteerde Set uit Cache   | De statistieken van het team van de cache met behulp van een gesorteerde set ophalen. Als er een cache-missers, de statistieken van de database laden en opslaan in de cache met behulp van een gesorteerde set.  |
| Top 5 Teams uit Cache  | De top 5 teams van de cache met behulp van een gesorteerde set ophalen. Als er een cache-missers, de statistieken van de database laden en opslaan in de cache met behulp van een gesorteerde set. |
| Laden vanuit de database            | De statistieken van het team worden opgehaald uit de database.                                                                                                                       |
| DB opnieuw opbouwen              | De database opnieuw maken en deze opnieuw vullen met voorbeeldgegevens team.                                                                                                        |
| Bewerken / Details / verwijderen | Bewerken van een team, details weergeven voor een team, een team verwijderen.                                                                                                             |


Op sommige van de acties en experimenteren met het ophalen van de gegevens uit verschillende bronnen. Niet de verschillen in de tijd die nodig is voor het voltooien van de verschillende manieren om de gegevens op te halen uit de database en de cache.

## <a name="delete-the-resources-when-you-are-finished-with-the-application"></a>De bronnen als u klaar met de toepassing bent verwijderen

Wanneer u klaar met de zelfstudie voorbeeldtoepassing bent, kunt u de Azure bronnen gebruikt om het besparen van kosten en resources verwijderen. Als u de knop **distribueren naar Azure** in de sectie [de Azure bronnen ter beschikking stellen gebruiken](#provision-the-azure-resources) en alle resources bevinden zich in dezelfde resourcegroep, kunt u verwijderen ze samen in één bewerking door het verwijderen van de resourcegroep.

1. Aanmelden bij de [Azure portal](https://portal.azure.com) en op **resourcegroepen**.
2. Typ de naam van de resourcegroep in de textbox **... items filteren** .
3. Klik op **...** aan de rechterkant van de resourcegroep.
4. Klik op **verwijderen**.

    ![Verwijderen][cache-delete-resource-group]

5. Typ de naam van de bronnengroep en klikt u op **verwijderen**.

    ![Verwijderen bevestigen][cache-delete-confirm]

Na een paar seconden de resourcegroep en alle bijbehorende bronnen opgenomen verwijderd.

>[AZURE.IMPORTANT] Houd er rekening mee dat een bronnengroep onomkeerbaar is en de resourcegroep en alle bronnen in het permanent verwijderd. Zorg ervoor dat u niet per ongeluk de verkeerde resourcegroep of resources verwijdert. Als u de bronnen voor het hosten van dit voorbeeld in een bestaande resourcegroep hebt gemaakt, kunt u elke bron afzonderlijk verwijderen van hun respectieve blades.

## <a name="run-the-sample-application-on-your-local-machine"></a>De voorbeeldtoepassing op uw lokale computer uitvoeren

Als de toepassing lokaal op uw computer uitvoert, moet u een exemplaar van het bestand Vgx. Azure-Cache waarin de gegevens in de cache. 

-   Als u hebt uw toepassing Azure gepubliceerd, zoals beschreven in de vorige sectie, kunt u het bestand Vgx. Azure-Cache exemplaar dat tijdens die stap werd ingericht.
-   Als u een ander bestaand bestand Vgx. Azure-Cache exemplaar hebt, kunt u die lokaal uitvoeren van dit voorbeeld.
-   Als u een exemplaar van het bestand Vgx. Azure-Cache te maken, kunt u de stappen in [een cache maken](cache-dotnet-how-to-use-azure-redis-cache.md#create-a-cache).

Zodra u hebt geselecteerd of gemaakt van de cache te gebruiken, Ga naar de cache in Azure portal en de [hostnaam](cache-configure.md#properties) en de [toegangstoetsen](cache-configure.md#access-keys) voor de cache ophalen. Zie [cache-instellingen configureren bestand Vgx.](cache-configure.md#configure-redis-cache-settings)voor meer informatie.

1. Open de `WebAppPlusCacheAppSecrets.config` -bestand dat u hebt gemaakt tijdens het [configureren van de toepassing voor het gebruik van Cache bestand Vgx.](#configure-the-application-to-use-redis-cache) stap van deze zelfstudie met behulp van de editor van uw keuze.

2. Bewerken de `value` kenmerk en vervangen `MyCache.redis.cache.windows.net` met de [hostnaam](cache-configure.md#properties) van de cache, en op de [primaire of secundaire sleutel](cache-configure.md#access-keys) van de cache als het wachtwoord op te geven.


        <appSettings>
          <add key="CacheConnection" value="MyCache.redis.cache.windows.net,abortConnect=false,ssl=true,password=..."/>
        </appSettings>


3. Druk op **Ctrl + F5** om de toepassing te starten.

>[AZURE.NOTE] Houd er rekening mee dat omdat de toepassing, met inbegrip van de database op de lokale computer wordt uitgevoerd en de Cache bestand Vgx. wordt gehost in Azure, de cache kan worden weergegeven voor het uitvoeren van de database onder. De clienttoepassing en Azure bestand Vgx. Cache exemplaar voor de beste prestaties moeten zijn op dezelfde locatie. 

## <a name="next-steps"></a>Volgende stappen

-   Meer informatie over [Aan de slag met ASP.NET MVC 5](http://www.asp.net/mvc/overview/getting-started/introduction/getting-started) op de [ASP.NET](http://asp.net/) -website.
-   Zie voor meer voorbeelden van het maken van een ASP.NET Web App in de App-Service [maken en implementeren van een ASP.NET-webtoepassing in Azure App Service](https://github.com/Microsoft/HealthClinic.biz/wiki/Create-and-deploy-an-ASP.NET-web-app-in-Azure-App-Service) van de [HealthClinic.biz](https://github.com/Microsoft/HealthClinic.biz) 2015 Connect [demo](https://blogs.msdn.microsoft.com/visualstudio/2015/12/08/connectdemos-2015-healthclinic-biz/).
    -   Zie voor meer QuickStart uit de HealthClinic.biz-demo [Azure Developer Tools QuickStart](https://github.com/Microsoft/HealthClinic.biz/wiki/Azure-Developer-Tools-Quickstarts).
-   Meer informatie over de aanpak van Framework entiteit die wordt gebruikt in deze zelfstudie [Code eerst naar een nieuwe database](https://msdn.microsoft.com/data/jj193542) .
-   Meer informatie over [web-apps in Azure App-Service](../app-service-web/app-service-web-overview.md).
-   Leren hoe u [monitor](cache-how-to-monitor.md) uw cache in Azure portal.

-   Premium-functies Azure-Cache bestand Vgx.
    -   [Het configureren van persistentie van een Cache Premium Azure bestand Vgx.](cache-how-to-premium-persistence.md)
    -   [Het configureren van clusters voor een Cache Premium Azure bestand Vgx.](cache-how-to-premium-clustering.md)
    -   [Het configureren van virtuele netwerk ondersteuning voor een Cache Premium Azure bestand Vgx.](cache-how-to-premium-vnet.md)
    -   Zie de [Azure bestand Vgx. Cache Veelgestelde vragen](cache-faq.md#what-redis-cache-offering-and-size-should-i-use) voor meer informatie over de grootte, doorvoer en bandbreedte met premium-caches.



<!-- IMAGES -->
[cache-starter-application]: ./media/cache-web-app-howto/cache-starter-application.png
[cache-added-to-application]: ./media/cache-web-app-howto/cache-added-to-application.png
[cache-create-project]: ./media/cache-web-app-howto/cache-create-project.png
[cache-select-template]: ./media/cache-web-app-howto/cache-select-template.png
[cache-model-add-class]: ./media/cache-web-app-howto/cache-model-add-class.png
[cache-model-add-class-dialog]: ./media/cache-web-app-howto/cache-model-add-class-dialog.png
[cache-add-controller]: ./media/cache-web-app-howto/cache-add-controller.png
[cache-add-controller-class]: ./media/cache-web-app-howto/cache-add-controller-class.png
[cache-configure-controller]: ./media/cache-web-app-howto/cache-configure-controller.png
[cache-global-asax]: ./media/cache-web-app-howto/cache-global-asax.png
[cache-RouteConfig-cs]: ./media/cache-web-app-howto/cache-RouteConfig-cs.png
[cache-layout-cshtml]: ./media/cache-web-app-howto/cache-layout-cshtml.png
[cache-layout-cshtml-code]: ./media/cache-web-app-howto/cache-layout-cshtml-code.png
[redis-cache-manage-nuget-menu]: ./media/cache-web-app-howto/redis-cache-manage-nuget-menu.png
[redis-cache-stack-exchange-nuget]: ./media/cache-web-app-howto/redis-cache-stack-exchange-nuget.png
[cache-teamscontroller]: ./media/cache-web-app-howto/cache-teamscontroller.png
[cache-web-config]: ./media/cache-web-app-howto/cache-web-config.png
[cache-views-teams-index-cshtml]: ./media/cache-web-app-howto/cache-views-teams-index-cshtml.png
[cache-teams-index-table]: ./media/cache-web-app-howto/cache-teams-index-table.png
[cache-status-message]: ./media/cache-web-app-howto/cache-status-message.png
[deploybutton]: ./media/cache-web-app-howto/deploybutton.png
[cache-deploy-to-azure-step-1]: ./media/cache-web-app-howto/cache-deploy-to-azure-step-1.png
[cache-deploy-to-azure-step-2]: ./media/cache-web-app-howto/cache-deploy-to-azure-step-2.png
[cache-deploy-to-azure-step-3]: ./media/cache-web-app-howto/cache-deploy-to-azure-step-3.png
[cache-deployment-started]: ./media/cache-web-app-howto/cache-deployment-started.png
[cache-publish-app]: ./media/cache-web-app-howto/cache-publish-app.png
[cache-publish-to-app-service]: ./media/cache-web-app-howto/cache-publish-to-app-service.png
[cache-select-web-app]: ./media/cache-web-app-howto/cache-select-web-app.png
[cache-publish]: ./media/cache-web-app-howto/cache-publish.png
[cache-delete-resource-group]: ./media/cache-web-app-howto/cache-delete-resource-group.png
[cache-delete-confirm]: ./media/cache-web-app-howto/cache-delete-confirm.png

