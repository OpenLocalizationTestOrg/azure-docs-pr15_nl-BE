<properties
    pageTitle="Het werken met de back-end-server .NET SDK voor mobiele Apps | Azure App-Service"
    description="Informatie over het werken met de back-end-server .NET SDK voor Mobile Apps in Azure App-Service."
    keywords="App-service, azure app service, mobiele app, mobiele diensten, schaal, schaalbare, app implementatie azure app implementatie"
    services="app-service\mobile"
    documentationCenter=""
    authors="adrianhall"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-multiple"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="work-with-the-net-backend-server-sdk-for-azure-mobile-apps"></a>Werken met de back-end-server .NET SDK voor Azure mobiele Apps

[AZURE.INCLUDE [app-service-mobile-selector-server-sdk](../../includes/app-service-mobile-selector-server-sdk.md)]

In dit onderwerp wordt beschreven hoe u .NET back-endserver SDK in belangrijke scenario's voor mobiele Apps in Azure App-Service te gebruiken. De SDK van Azure mobiele Apps kunt u werken met mobiele clients van uw ASP.NET-toepassing.

>[AZURE.TIP] De [.NET server SDK voor Azure Mobile Apps] [ 2] is open-source op GitHub. De bibliotheek bevat alle broncode, met inbegrip van de hele server SDK unit test suite en enkele voorbeeldprojecten.

## <a name="reference-documentation"></a>Documentatie

De documentatie bij de server SDK bevindt zich hier: [Azure Mobile Apps.NET Reference][1].

## <a name="create-app"></a>Procedure: een .NET Mobile App back-end maken

Als u een nieuw project start, kunt u een App Service toepassing met behulp van de [portal Azure] of Visual Studio. U kunt de App-servicetoepassing lokaal uitvoeren of het project publiceren naar uw cloud-gebaseerde App Service mobiele app.  

Als u mobiele mogelijkheden aan een bestaand project toevoegt, raadpleegt u de sectie [downloaden en initialiseren van de SDK](#install-sdk) .

### <a name="create-a-net-backend-using-the-azure-portal"></a>Een .NET backend met de Azure portal maken

Een mobiele backend-App Service een hand maken de [Quickstart handleiding] [ 3] of als volgt te werk:

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-create-new-service-classic](../../includes/app-service-mobile-dotnet-backend-create-new-service-classic.md)]

In het blad _aan de slag_ onder **een tabel API maken**, kiest u **C#** als de **Backend taal**. Klik op **downloaden**en uitpakken van gecomprimeerde projectbestanden op uw computer opent u de oplossing in Visual Studio.

### <a name="create-a-net-backend-using-visual-studio-2013-and-visual-studio-2015"></a>Maak een met behulp van Visual Studio 2013 en 2015 voor Visual Studio .NET-backend

Installatie van de [SDK voor .NET Azure] [ 4] (versie 2.9.0 of hoger) voor het maken van een Azure Mobile Apps-project in Visual Studio. Als u de SDK hebt geïnstalleerd, maakt u een ASP.NET-toepassing met behulp van de volgende stappen uit:

1. Open het dialoogvenster **Nieuw Project** (uit *bestand* > **Nieuw** > **Project...**).
2. **Sjablonen** > **Visual C#**en selecteer **Web**.
3. Selecteer de **ASP.NET-webtoepassing**.
4. Vul de naam van het project. Klik vervolgens op **OK**.
5. Onder _ASP.NET 4.5.2 sjablonen_, **Azure Mobile App**selecteren. Controleer **Host in de cloud** te maken van een mobiele back-end in de cloud waaraan u dit project kunt publiceren.
6. Klik op **OK**.

## <a name="install-sdk"></a>Procedure: downloaden en initialiseren van de SDK

De SDK is beschikbaar op [NuGet.org]. Dit pakket bevat de basisfunctionaliteit die nodig zijn om aan de slag met de SDK. Als u wilt initialiseren van de SDK, moet u acties uitvoeren op het **HttpConfiguration** -object.

### <a name="install-the-sdk"></a>De SDK installeren

Met de SDK installeren, met de rechtermuisknop op de server-project in Visual Studio, selecteer **NuGet pakketten beheren**, zoeken naar het [Microsoft.Azure.Mobile.Server] -pakket en klik vervolgens op **installeren**.

###<a name="server-project-setup"></a>De serverproject initialiseren

Een .NET back-end serverproject wordt vergelijkbaar met andere ASP.NET-projecten geïnitialiseerd door een klasse OWIN opstarten. Zorg ervoor dat u verwijst naar het pakket NuGet `Microsoft.Owin.Host.SystemWeb`. Als deze klasse in Visual Studio, met de rechtermuisknop op het serverproject en selecteer **toevoegen** > 
**Nieuw Item**en klik op **Web** > **Algemeen** > **OWIN opstarten klasse**.  Een klasse wordt gegenereerd met het volgende kenmerk:

    [assembly: OwinStartup(typeof(YourServiceName.YourStartupClassName))]

In de `Configuration()` methode van de klasse OWIN opstarten, een **HttpConfiguration** -object gebruiken om de mobiele Apps van Azure-omgeving configureren.
In het volgende voorbeeld wordt het serverproject met geen toegevoegde functies geïnitialiseerd:

    // in OWIN startup class
    public void Configuration(IAppBuilder app)
    {
        HttpConfiguration config = new HttpConfiguration();

        new MobileAppConfiguration()
            // no added features
            .ApplyTo(config);

        app.UseWebApi(config);
    }

Afzonderlijke als functies wilt inschakelen, moet u op het **MobileAppConfiguration** -object uitbreidingsmethoden aanroepen voordat **ApplyTo**wordt aangeroepen. Bijvoorbeeld de standaardroutes in de volgende code wordt toegevoegd aan alle API-controllers die het kenmerk `[MobileAppController]` tijdens de initialisatie:

    new MobileAppConfiguration()
        .MapApiControllers()
        .ApplyTo(config);

De quickstart server vanaf de portal Azure roept **UseDefaultConfiguration()**. Dit is gelijk aan de volgende instellingen:

        new MobileAppConfiguration()
            .AddMobileAppHomeController()             // from the Home package
            .MapApiControllers()
            .AddTables(                               // from the Tables package
                new MobileAppTableConfiguration()
                    .MapTableControllers()
                    .AddEntityFramework()             // from the Entity package
                )
            .AddPushNotifications()                   // from the Notifications package
            .MapLegacyCrossDomainController()         // from the CrossDomain package
            .ApplyTo(config);

De gebruikte uitbreidingsmethoden zijn:

* `AddMobileAppHomeController()`biedt de standaard Azure Mobile Apps-startpagina.
* `MapApiControllers()`aangepaste API-functies bevat voor controllers WebAPI gedecoreerd met de `[MobileAppController]` kenmerk.
* `AddTables()`wordt de koppeling van de `/tables` de eindpunten aan de tabel controllers.
* `AddTablesWithEntityFramework()`is een korte-hand voor het omzetten van de `/tables` Framework entiteit met eindpunten op basis van domeincontrollers.
* `AddPushNotifications()`biedt een eenvoudige methode voor het registreren van apparaten voor kennisgeving Hubs van.
* `MapLegacyCrossDomainController()`biedt CORS Standaardkoppen voor plaatselijke ontwikkeling.

### <a name="sdk-extensions"></a>SDK-extensies

De volgende extensie NuGet gebaseerde pakketten bieden verschillende mobiele functies die kunnen worden gebruikt door de toepassing. U inschakelen-extensies tijdens de initialisatie met het **MobileAppConfiguration** -object.

- [Microsoft.Azure.Mobile.Server.Quickstart] ondersteunt de basisinstellingen van mobiele Apps. De configuratie toegevoegd door het aanroepen van de methode **UseDefaultConfiguration** uitbreiding tijdens de initialisatie. Deze uitbreiding bevat de volgende extensies: meldingen, verificatie, entiteit, tabellen, interdomein- en Home-pakketten. Dit pakket wordt gebruikt door de mobiele Apps Quickstart in Azure portal beschikbaar.

- [Microsoft.Azure.Mobile.Server.Home](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Home/) 
   implementeert het standaard *deze mobiele app actief is pagina* voor de hoofdmap van de website. Toevoegen aan de configuratie door het aanroepen van de methode   **AddMobileAppHomeController** -extensie.

- [Microsoft.Azure.Mobile.Server.Tables](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Tables/) 
   bevat klassen voor het werken met gegevens en sets van de pijpleiding gegevens. Toevoegen aan de configuratie door het aanroepen van de methode **AddTables** -extensie.

- [Microsoft.Azure.Mobile.Server.Entity](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Entity/) 
   kunt u het kader van de entiteit aan access-gegevens in de SQL-Database. Toevoegen aan de configuratie door het aanroepen van de methode **AddTablesWithEntityFramework** -extensie.

- [Microsoft.Azure.Mobile.Server.Authentication] Hiermee kunt verificatie en sets van de OWIN middleware gebruikt om tokens te valideren. Toevoegen aan de configuratie door het aanroepen van de **AddAppServiceAuthentication**  
   en **IAppBuilder**. Uitbreidingsmethoden voor **UseAppServiceAuthentication** .

- [Microsoft.Azure.Mobile.Server.Notifications] schakelt push-meldingen en definieert een eindpunt push-registratie. Toevoegen aan de configuratie door het aanroepen van de methode **AddPushNotifications** -extensie.

- [Microsoft.Azure.Mobile.Server.CrossDomain](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.CrossDomain/) 
   maakt een domeincontroller die gegevens oudere webbrowsers vanuit uw mobiele App dient. Toevoegen aan de configuratie door het aanroepen van de methode   **MapLegacyCrossDomainController** -extensie.

- [Microsoft.Azure.Mobile.Server.Login] biedt de AppServiceLoginHandler.CreateToken()-methode een statische methode gebruikt tijdens de aangepaste verificatie-scenario's is.   

## <a name="publish-server-project"></a>Procedure: het serverproject publiceren

In deze sectie wordt beschreven hoe u uw .NET back-end-project van Visual Studio publiceren. U kunt ook uw back-end project Git of een van de andere methoden die worden beschreven in de [documentatie over de implementatie van Azure App Service](../app-service-web/web-sites-deploy.md)implementeren.

1. Bouw het project om te zetten NuGet pakketten opnieuw in Visual Studio.

2. In de Solution Explorer met de rechtermuisknop op het project, klikt u op **publiceren**. De eerste keer dat u publiceert, moet u voor het definiëren van een profiel voor publiceren. U kunt wanneer u al een profiel is gedefinieerd, selecteert u deze en klikt u op **publiceren**.

2. Selecteer een doel publiceren wordt gevraagd, klikt u op **Microsoft Azure App Service** > **volgende**, en vervolgens (indien nodig) aanmelden met uw referenties Azure. 
   Visual Studio downloadt en veilig opgeslagen uw publicatie-instellingen rechtstreeks van Azure.

    ![](./media/app-service-mobile-dotnet-backend-how-to-use-server-sdk/publish-wizard-1.png)

3. Kies uw **abonnement**, **Resource Type** selecteren in de **weergave**, **Mobile App**, uitbreiden en uw backend Mobile App, klik op **OK**.

    ![](./media/app-service-mobile-dotnet-backend-how-to-use-server-sdk/publish-wizard-2.png)

4. Controleer of u de gegevens publiceren en klik op **publiceren**.

    ![](./media/app-service-mobile-dotnet-backend-how-to-use-server-sdk/publish-wizard-3.png)

    Wanneer uw backend Mobile App heeft gepubliceerd, ziet u een landingspagina gemeld.

    ![](./media/app-service-mobile-dotnet-backend-how-to-use-server-sdk/publish-success.png)

##<a name="define-table-controller"></a>Procedure: de controller voor een tabel definiëren

Definieert een tabel Controller om een SQL-tabel aan mobiele clients weer te geven.  Configureren van een domeincontroller tabel vereist drie stappen:

1. Maak een klasse Data Transfer Object (DTO).
2. De verwijzing naar een tabel in de klasse DbContext Mobile configureren.
3. Maak een tabel controller.

Een Data Transfer Object (DTO) is een gewone C#-object dat gegevens van overneemt `EntityData`.  Bijvoorbeeld:

    public class TodoItem : EntityData
    {
        public string Text { get; set; }
        public bool Complete {get; set;}
    }

De DTO wordt gebruikt voor het definiëren van de tabel in de SQL-database.  U maakt de database-invoer toevoegen een `DbSet<>` aan de DbContext die u met de eigenschap.  In het project standaardsjabloon voor Mobile Apps in Azure, het DbContext heet `Models\MobileServiceContext.cs`:

    public class MobileServiceContext : DbContext
    {
        private const string connectionStringName = "Name=MS_TableConnectionString";

        public MobileServiceContext() : base(connectionStringName)
        {

        }

        public DbSet<TodoItem> TodoItems { get; set; }

        protected override void OnModelCreating(DbModelBuilder modelBuilder)
        {
            modelBuilder.Conventions.Add(
                new AttributeToColumnAnnotationConvention<TableColumnAttribute, string>(
                    "ServiceColumnTable", (property, attributes) => attributes.Single().ColumnType.ToString()));
        }
    }

Als u de SDK Azure is geïnstalleerd, kunt u een domeincontroller sjabloon tabel nu als volgt maken:

1. Klik met de rechtermuisknop op de map Controllers en selecteer **toevoegen** > **...-Controller**.
2. Selecteer de optie **Azure Mobile Apps tabel Controller** en klik vervolgens op **toevoegen**.
3. Klik in het dialoogvenster **Domeincontroller toevoegen** :
    * Selecteer in de vervolgkeuzelijst **klasse Model** de nieuwe DTO.
    * Selecteer in de vervolgkeuzelijst **DbContext** de mobiele Service DbContext-klasse.
    * De naam van de domeincontroller is gemaakt.
4. Klik op **toevoegen**.

Het project quickstart server bevat een voorbeeld van een eenvoudige **TodoItemController**.

### <a name="how-to-adjust-the-table-paging-size"></a>Procedure: de grootte van de wisselbestanden tabel aanpassen

Standaard retourneert Azure Mobile Apps 50 records per aanvraag.  Paginering zorgt ervoor dat de client wordt niet in beslag nemen hun UI-thread of op de server te lang voor een goede gebruikerservaring te waarborgen. Om de grootte van de wisselbestanden tabel, verhogen de serverzijde 'query grootte toegestaan' en de client-side pagina de grootte van de serverzijde "toegestane grootte query" wordt aangepast met behulp van de `EnableQuery` kenmerk:

    [EnableQuery(PageSize = 500)]

Zorgen de PageSize is gelijk of groter is dan de door de client is aangevraagd.  Verwijzen naar de specifieke procedure-documentatie voor meer informatie over het wijzigen van het formaat van de client-client.

## <a name="how-to-define-a-custom-api-controller"></a>Procedure: Definieer een aangepaste API-controller

De aangepaste API-controller biedt basisfuncties voor uw backend Mobile App door een eindpunt bloot. U kunt een mobiele-specifieke API-controller met behulp van het kenmerk [MobileAppController] registreren. De `MobileAppController` kenmerk registreert de route, stelt de serializer Mobile Apps JSON en [versiecontrole client](app-service-mobile-client-and-server-versioning.md)wordt ingeschakeld.

1. Klik met de rechtermuisknop op de map met domeincontrollers in Visual Studio, en klik op **toevoegen** > -**Controller**, selecteer **Web API 2 Controller&mdash;leeg** en klik op **toevoegen**.

2. Geef een **naam van de domeincontroller**, zoals `CustomController`, en klik op **toevoegen**.

3. In het nieuwe bestand in de klasse controller, voeg de volgende instructie:

        using Microsoft.Azure.Mobile.Server.Config;

4. Het kenmerk **[MobileAppController]** van toepassing op de API-controller klassendefinitie, zoals in het volgende voorbeeld:

        [MobileAppController]
        public class CustomController : ApiController
        {
              //...
        }

4. Voeg toe een aanroep van de methode van de extensie **MapApiControllers** App_Start/Startup.MobileApp.cs bestand:

        new MobileAppConfiguration()
            .MapApiControllers()
            .ApplyTo(config);

U kunt ook de `UseDefaultConfiguration()` uitbreidingsmethode in plaats van `MapApiControllers()`. Een controller die geen **MobileAppControllerAttribute** zijn toegepast nog steeds toegankelijk is voor clients, maar het kan niet worden correct worden gebruikt door clients met behulp van een mobiele App client SDK.

## <a name="how-to-work-with-authentication"></a>Procedure: werken met verificatie

Azure mobiele Apps gebruikt App Service verificatie / vergunning voor het beveiligen van uw mobiele back-end.  In deze sectie wordt beschreven hoe u de volgende taken met betrekking tot verificatie uitvoeren in uw project .NET back-end-server:

+ [Procedure: verificatie toevoegen aan een serverproject](#add-auth)
+ [Procedure: aangepaste verificatie gebruiken voor uw toepassing](#custom-auth)
+ [Procedure: ophalen geverifieerde gebruikersinformatie](#user-info)
+ [Procedure: toegang tot gegevens voor geautoriseerde gebruikers beperken](#authorize)

### <a name="add-auth"></a>Procedure: verificatie toevoegen aan een serverproject

Door uitbreiding van het object **MobileAppConfiguration** en OWIN middleware te configureren, kunt u verificatie toevoegen aan uw serverproject. Bij het installeren van het pakket [Microsoft.Azure.Mobile.Server.Quickstart] en roept de methode **UseDefaultConfiguration** -extensie, kunt u verdergaan met stap 3.

1. Installeer het pakket [Microsoft.Azure.Mobile.Server.Authentication] in Visual Studio.

2. Voeg de volgende regel code aan het begin van de methode **configuratie** in het projectbestand Startup.cs:

        app.UseAppServiceAuthentication(config);

    Dit onderdeel OWIN middleware valideert tokens die worden uitgegeven door de bijbehorende App Service gateway.

3. Voeg toe de `[Authorize]` van het kenmerk aan een controller of een methode die verificatie vereist. 

Zie voor meer informatie over het verifiëren van clients naar uw mobiele Apps backend, [verificatie van uw app toevoegen](app-service-mobile-ios-get-started-users.md).

### <a name="custom-auth"></a>Procedure: aangepaste verificatie gebruiken voor uw toepassing

Als u niet gebruiken op een van de providers App Service verificatie/vergunning wilt, kunt u uw eigen login-systeem implementeren. Installeer het pakket [Microsoft.Azure.Mobile.Server.Login] om te helpen bij de verificatie-token genereren.  Geef uw eigen code voor het valideren van de referenties van de gebruiker. U kunt bijvoorbeeld controleren tegen gezouten en gecodeerde wachtwoorden in een database. In het volgende voorbeeld zijn de `isValidAssertion()` methode (elders gedefinieerd) verantwoordelijk is voor deze controles.

De aangepaste verificatie wordt weergegeven door het maken van een ApiController en blootstelling van `register` en `login` acties. De client moet een aangepaste gebruikersinterface gebruiken om de gegevens van de gebruiker verzamelen.  De informatie wordt vervolgens naar de API met een aanroep van de standaard HTTP POST ingediend. Zodra de server de bewering valideert, een token verleend met de `AppServiceLoginHandler.CreateToken()` methode.  Het gebruik van ApiController **niet** de `[MobileAppController]` kenmerk. 

Een voorbeeld van de `login` actie:

        public IHttpActionResult Post([FromBody] JObject assertion)
        {
            if (isValidAssertion(assertion)) // user-defined function, checks against a database
            {
                JwtSecurityToken token = AppServiceLoginHandler.CreateToken(new Claim[] { new Claim(JwtRegisteredClaimNames.Sub, assertion["username"]) },
                    mySigningKey,
                    myAppURL,
                    myAppURL,
                    TimeSpan.FromHours(24) );
                return Ok(new LoginResult()
                {
                    AuthenticationToken = token.RawData,
                    User = new LoginResultUser() { UserId = userName.ToString() }
                });
            }
            else // user assertion was not valid
            {
                return this.Request.CreateUnauthorizedResponse();
            }
        }

In het vorige voorbeeld zijn LoginResult en LoginResultUser objecten serialiseerbaar vereiste eigenschappen bloot. De client verwacht dat antwoorden worden geretourneerd als JSON-objecten van het formulier aanmelding:

        {
            "authenticationToken": "<token>",
            "user": {
                "userId": "<userId>"
            }
        }

De `AppServiceLoginHandler.CreateToken()` de methode bevat een _publiek_ en een parameter van de _uitgevende instelling_ . Beide parameters worden ingesteld op de URL van de hoofdmap van de toepassing, met behulp van het HTTPS-schema. Stel op dezelfde manier _secretKey_ om de waarde van uw toepassing de handtekeningsleutel. De handtekeningsleutel in een client niet distribueren als u voorkomt daarmee sleutels en uitgeven voor gebruikers kan worden gebruikt. U vindt de handtekeningsleutel terwijl in App-Service wordt gehost door te verwijzen naar de _WEBSITE\_AUTH\_ondertekening\_sleutel_ omgevingsvariabele. Volg de instructies in de sectie [lokale verificatie foutopsporing](#local-debug) de sleutel ophalen en opslaan als een toepassingsinstelling desgewenst in een lokale context voor foutopsporing.

Het uitgegeven token kan ook andere claims en een vervaldatum.  Het uitgegeven token moet minimaal, een claim (**sub**) onderwerp bevatten.

U kunt de standaard client ondersteunt `loginAsync()` methode door overbelasting van de route van de verificatie.  Als de client roept `client.loginAsync('custom');` te melden de route moet worden `/.auth/login/custom`.  U kunt instellen dat de route voor de aangepaste verificatie-controller met `MapHttpRoute()`:

    config.Routes.MapHttpRoute("custom", ".auth/login/custom", new { controller = "CustomAuth" });

>[AZURE.TIP] Met behulp van de `loginAsync()` aanpak zorgt ervoor dat de verificatietoken voor elke volgende aanroep van de service is gekoppeld.

###<a name="user-info"></a>Procedure: ophalen geverifieerde gebruikersinformatie

Wanneer een gebruiker is geverifieerd door App-Service, kunt u de toegewezen gebruikers-ID en andere informatie in uw back-end .NET code openen. De gebruikersinformatie kan worden gebruikt om autorisatiebeslissingen te nemen in de back-end. De volgende code haalt de gebruikers-ID die is gekoppeld aan een aanvraag:

    // Get the SID of the current user.
    var claimsPrincipal = this.User as ClaimsPrincipal;
    string sid = claimsPrincipal.FindFirst(ClaimTypes.NameIdentifier).Value;

De SID is afgeleid van de provider-specifieke gebruikers-ID en voor een bepaalde gebruiker en de provider login statisch is.  De SID is voor ongeldige verificatietokens null.

App-Service kunt u ook specifieke claims aanvragen bij uw aanmelding. Elke identiteitsprovider kan bieden meer informatie met behulp van de identiteitsprovider SDK.  U kunt bijvoorbeeld de Facebook Graph API voor vrienden informatie.  In het blad provider in Azure portal kunt u claims die worden aangevraagd. Sommige vorderingen vereisen aanvullende configuratie met de id-provider.

De volgende code roept de methode **GetAppServiceIdentityAsync** extensie als u de inloggegevens, waaronder het toegangstoken die nodig zijn om aanvragen tegen de Facebook Graph API:

    // Get the credentials for the logged-in user.
    var credentials =
        await this.User
        .GetAppServiceIdentityAsync<FacebookCredentials>(this.Request);

    if (credentials.Provider == "Facebook")
    {
        // Create a query string with the Facebook access token.
        var fbRequestUrl = "https://graph.facebook.com/me/feed?access_token="
            + credentials.AccessToken;

        // Create an HttpClient request.
        var client = new System.Net.Http.HttpClient();

        // Request the current user info from Facebook.
        var resp = await client.GetAsync(fbRequestUrl);
        resp.EnsureSuccessStatusCode();

        // Do something here with the Facebook user information.
        var fbInfo = await resp.Content.ReadAsStringAsync();
    }

Toevoegen met een instructie voor `System.Security.Principal` voor de methode **GetAppServiceIdentityAsync** -extensie.

### <a name="authorize"></a>Procedure: toegang tot gegevens voor geautoriseerde gebruikers beperken

In de vorige sectie, we hebben laten zien hoe u kunt de gebruikers-ID van een geverifieerde gebruiker ophalen. U kunt de toegang beperken tot gegevens en andere bronnen op basis van deze waarde. Een gebruikers-id-kolom toevoegen aan tabellen en filteren van de resultaten van de query door de gebruikers-ID is bijvoorbeeld een eenvoudige manier om de geretourneerde gegevens alleen voor geautoriseerde gebruikers te beperken. De volgende code retourneert rijen met gegevens alleen als de beveiligings-id overeenkomt met de waarde in de kolom gebruikers-id in de tabel TodoItem:

    // Get the SID of the current user.
    var claimsPrincipal = this.User as ClaimsPrincipal;
    string sid = claimsPrincipal.FindFirst(ClaimTypes.NameIdentifier).Value;

    // Only return data rows that belong to the current user.
    return Query().Where(t => t.UserId == sid);

De `Query()` methode geeft als resultaat een `IQueryable` die kunnen worden bewerkt door LINQ verwerken filteren.

## <a name="how-to-add-push-notifications-to-a-server-project"></a>Procedure: toevoegen van push meldingen naar een serverproject

Push-meldingen toevoegen aan uw serverproject door uitbreiding van het **MobileAppConfiguration** -object en het maken van een melding Hubs.

1. In Visual Studio met de rechtermuisknop op het serverproject en klik op **Beheren NuGet pakketten**, zoeken naar `Microsoft.Azure.Mobile.Server.Notifications`, klikt u op **installeren**. 

2. Herhaal deze stap voor het installeren van de `Microsoft.Azure.NotificationHubs` -pakket, inclusief de clientbibliotheek melding Hubs.

3. In App_Start/Startup.MobileApp.cs, en een aanroep van de methode **AddPushNotifications()** -extensie toevoegen tijdens de initialisatie:

        new MobileAppConfiguration()
            // other features...
            .AddPushNotifications()
            .ApplyTo(config);

4. Voeg de volgende code die een client Hubs melding worden gemaakt:

        // Get the settings for the server project.
        HttpConfiguration config = this.Configuration;
        MobileAppSettingsDictionary settings =
            config.GetMobileAppSettingsProvider().GetMobileAppSettings();

        // Get the Notification Hubs credentials for the Mobile App.
        string notificationHubName = settings.NotificationHubName;
        string notificationHubConnection = settings
            .Connections[MobileAppSettingsKeys.NotificationHubConnectionString].ConnectionString;

        // Create a new Notification Hub client.
        NotificationHubClient hub = NotificationHubClient
            .CreateClientFromConnectionString(notificationHubConnection, notificationHubName);

Nu kunt u de client melding Hubs push-meldingen verzenden naar geregistreerde apparaten. Zie [push-meldingen toevoegen aan uw app](app-service-mobile-ios-get-started-push.md)voor meer informatie. Voor meer informatie over Hubs Notification, overzicht [melding Hubs](../notification-hubs/notification-hubs-push-notification-overview.md).

##<a name="tags"></a>Procedure: inschakelen met behulp van Tags push gericht

Melding Hubs kunt u gerichte berichten verzenden naar specifieke registraties met behulp van tags. Verschillende labels worden automatisch gemaakt:

* De installatie-ID geeft een specifiek apparaat.
* De op basis van de SID voor geverifieerde gebruikers-Id identificeert een specifieke gebruiker.

De installatie-ID kan worden geopend vanuit de eigenschap **omwille** van de **MobileServiceClient**.  In het volgende voorbeeld ziet u hoe een label toevoegen aan een specifieke installatie in kennisgeving Hubs met een installatie-ID:

    hub.PatchInstallation("my-installation-id", new[]
    {
        new PartialUpdateOperation
        {
            Operation = UpdateOperationType.Add,
            Path = "/tags",
            Value = "{my-tag}"
        }
    });

Labels verstrekt door de client tijdens de registratie van push-meldingen worden genegeerd door de backend bij het maken van de installatie. Als u wilt dat een client labels toevoegen aan de installatie, moet u een aangepaste API die codes met behulp van de voorgaande patroon toegevoegd. 

Zie [Client toegevoegd push notification tags] [ 5] in het voorbeeld van de voltooide quickstart App Service Mobile Apps voor een voorbeeld.

##<a name="push-user"></a>Procedure: push-meldingen verzenden aan een geverifieerde gebruiker

Wanneer een geverifieerde gebruiker geregistreerd voor push-meldingen, ID-code aan een gebruiker automatisch toegevoegd aan de registratie. Met behulp van deze code, kunt u push-meldingen verzenden naar alle apparaten die door die persoon zijn geregistreerd. De volgende code wordt de SID van de gebruiker die de aanvraag en een sjabloon push-bericht verzendt naar elke registratie voor die persoon:

    // Get the current user SID and create a tag for the current user.
    var claimsPrincipal = this.User as ClaimsPrincipal;
    string sid = claimsPrincipal.FindFirst(ClaimTypes.NameIdentifier).Value;
    string userTag = "_UserId:" + sid;

    // Build a dictionary for the template with the item message text.
    var notification = new Dictionary<string, string> { { "message", item.Text } };

    // Send a template notification to the user ID.
    await hub.SendTemplateNotificationAsync(notification, userTag);

Wanneer u registreert voor push-meldingen van een geverifieerde client, moet u ervoor zorgen dat de verificatie is voltooid voordat u de registratie. Voor meer informatie, Zie [Push-gebruikers] [ 6] in de steekproef van de voltooide quickstart App Service Mobile Apps voor .NET back-end.

## <a name="how-to-debug-and-troubleshoot-the-net-server-sdk"></a>Procedure: fouten opsporen en oplossen van problemen met de SDK voor .NET Server

Azure App Service biedt verschillende fouten opsporen en oplossen van technieken voor ASP.NET-toepassingen:

- [Een Azure App-Service controleren](../app-service-web/web-sites-monitor.md)
- [Diagnostische gegevens vastleggen in Azure App-Service inschakelen](../app-service-web/web-sites-enable-diagnostic-log.md)
- [Problemen oplossen met een Azure Service App in Visual Studio](../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md)

### <a name="logging"></a>Logboekregistratie

Met behulp van de standaard ASP.NET-trace schrijven kunt u schrijven naar diagnostische logboeken App-Service. Voordat u in de logboeken schrijven kan, moet u de diagnostische gegevens in uw mobiele App backend inschakelen.

Diagnose inschakelen en schrijven naar de logboeken:

1. Volg de stappen in [het inschakelen van diagnostische gegevens](../app-service-web/web-sites-enable-diagnostic-log.md#enablediag).

2. Voeg de volgende instructie gebruiken in uw codebestand:

        using System.Web.Http.Tracing;

3. Een trace schrijver van de .NET-end schrijven naar de diagnostische logboeken als volgt maken:

        ITraceWriter traceWriter = this.Configuration.Services.GetTraceWriter();
        traceWriter.Info("Hello, World");

4. Uw serverproject publiceren en toegang tot de backend mobiele App voor het uitvoeren van de codepad met de registratie.

5. Downloaden en evalueren van de logs, zoals wordt beschreven in [hoe: Logboeken downloaden](../app-service-web/web-sites-enable-diagnostic-log.md#download).

### <a name="local-debug"></a>Lokale verificatie voor foutopsporing

U kunt uw toepassing lokaal om wijzigingen te testen voordat u deze publiceert naar de cloud uitvoeren. Voor de meeste backends Azure Mobile Apps, druk op *F5* terwijl u in Visual Studio. Er zijn echter enkele aanvullende overwegingen bij het gebruik van verificatie.

Hebt u een op cloud-gebaseerde met App Service Authentication/Authorization geconfigureerd voor app op mobiele en de client het eindpunt wolk is opgegeven als de alternatieve aanmeldings-host moet hebben. Raadpleeg de documentatie bij uw client platform voor de specifieke stappen vereist.

Zorg ervoor dat uw back-end mobiele [Microsoft.Azure.Mobile.Server.Authentication] geïnstalleerd. Vervolgens in uw toepassing van OWIN Opstartklasse de volgende toevoegen na `MobileAppConfiguration` is toegepast op de `HttpConfiguration`:

        app.UseAppServiceAuthentication(new AppServiceAuthenticationOptions()
        {
            SigningKey = ConfigurationManager.AppSettings["authSigningKey"],
            ValidAudiences = new[] { ConfigurationManager.AppSettings["authAudience"] },
            ValidIssuers = new[] { ConfigurationManager.AppSettings["authIssuer"] },
            TokenHandler = config.GetAppServiceTokenHandler()
        });

In het vorige voorbeeld, moet u de instellingen voor _authAudience_ en _authIssuer_ configureren in het bestand Web.config op elk worden de URL van de hoofdmap van de toepassing, met behulp van het HTTPS-schema. Stel op dezelfde manier _authSigningKey_ om de waarde van uw toepassing de handtekeningsleutel. Als u de bijbehorende sleutel:

1. Ga naar de app binnen de [Azure portal] 
2. Klik op **Extra**, **Kudu**, **gaan**.
3. Klik op de site Management Kudu **omgeving**.
4. De waarde voor _WEBSITE\_AUTH\_ondertekening\_sleutel_. 

Gebruik de handtekeningsleutel voor de parameter _authSigningKey_ in uw lokale toepassing config.  Uw mobiele backend is nu uitgerust om tokens als lokaal worden uitgevoerd die de client het token van de cloud-gebaseerde eindpunt verkrijgt te valideren.

[1]: https://msdn.microsoft.com/library/azure/dn961176.aspx
[2]: https://github.com/Azure/azure-mobile-apps-net-server
[3]: app-service-mobile-ios-get-started.md
[4]: https://azure.microsoft.com/downloads/
[5]: https://github.com/Azure-Samples/app-service-mobile-dotnet-backend-quickstart/blob/master/README.md#client-added-push-notification-tags
[6]: https://github.com/Azure-Samples/app-service-mobile-dotnet-backend-quickstart/blob/master/README.md#push-to-users
[Azure portal]: https://portal.azure.com
[NuGet.org]: http://www.nuget.org/
[Microsoft.Azure.Mobile.Server]: http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server/
[Microsoft.Azure.Mobile.Server.Quickstart]: http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Quickstart/
[Microsoft.Azure.Mobile.Server.Authentication]: http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Authentication/
[Microsoft.Azure.Mobile.Server.Login]: http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Login/
[Microsoft.Azure.Mobile.Server.Notifications]: http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Notifications/
[MapHttpAttributeRoutes]: https://msdn.microsoft.com/library/dn479134(v=vs.118).aspx

