<properties
    pageTitle="Een upgrade uitvoeren van mobiele Services naar Azure App-Service"
    description="Meer informatie over het eenvoudige upgrade van uw mobiele Services-toepassing naar een App Service Mobile App"
    services="app-service\mobile"
    documentationCenter=""
    authors="adrianhall"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="upgrade-your-existing-net-azure-mobile-service-to-app-service"></a>Uw bestaande .NET Azure Mobile-Service upgraden naar App Service

Mobiele App-Service is een nieuwe manier van mobiele toepassingen met Microsoft Azure. Voor meer informatie, Zie [Wat zijn mobiele Apps?].

In dit onderwerp wordt beschreven hoe u een bestaande back-end-toepassing van .NET van Azure Mobile Services bijwerken naar een nieuwe mobiele Apps in App-Service. Terwijl u deze upgrade uitvoert, blijven uw bestaande Mobile Services-toepassing functioneren.   Als u een back-end Node.js toepassing bijwerken, kunt u verwijzen naar [uw Node.js Mobile Services bijwerken](./app-service-mobile-node-backend-upgrading-from-mobile-services.md).

Wanneer een mobiele backend is bijgewerkt naar Azure App Service, heeft toegang tot alle functies van de App Service en volgens de [serviceprijzen App], geen mobiele Services, prijzen worden gefactureerd.

##<a name="migrate-vs-upgrade"></a>Migreren te upgraden

[AZURE.INCLUDE [app-service-mobile-migrate-vs-upgrade](../../includes/app-service-mobile-migrate-vs-upgrade.md)]

>[AZURE.TIP] Het wordt aanbevolen dat u [een migratie uit te voeren](app-service-mobile-migrating-from-mobile-services.md) voordat u verdergaat met de upgrade. Op deze manier kunt u beide versies van de toepassing op de dezelfde App Service Plan te plaatsen en er geen extra kosten in rekening gebracht.

###<a name="improvements-in-mobile-apps-net-server-sdk"></a>Verbeteringen in de mobiele Apps .NET server SDK

Een upgrade naar de nieuwe [Mobiele Apps SDK](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Server/) biedt de volgende voordelen:

- Meer flexibiliteit op NuGet afhankelijkheden. De hosting-omgeving biedt niet langer zijn eigen versies van NuGet-pakketten, zodat u alternatieve compatibele versies kunt. Echter, als er nieuwe belangrijke bugfixes en beveiligingspatches voor de mobiele Server SDK of afhankelijkheden, u moet de service handmatig bijwerken.

- Meer flexibiliteit in de mobiele SDK. Expliciet kunt u bepalen welke functies en routes ingesteld, zoals verificatie, tabel API's en het eindpunt van de registratie van push. Zie voor meer informatie, [het gebruik van het .NET server SDK voor Azure Mobile Apps](app-service-mobile-net-upgrading-from-mobile-services.md#server-project-setup).

- Ondersteuning voor andere projecttypen ASP.NET en routes. U kunt nu hostcontrollers MVC en Web-API in hetzelfde project als uw project mobiele back-end.

- Ondersteuning voor nieuwe App Service verificatie-functies waarmee u een veelgebruikte configuratie voor verificatie gebruiken in uw web en mobiele toepassingen.

##<a name="overview"></a>Basic upgrade, overzicht

In veel gevallen een upgrade worden zo eenvoudig overschakelen naar de nieuwe mobiele Apps server SDK en de code naar een nieuwe Mobile App-exemplaar opnieuw te publiceren. Er zijn echter enkele scenario's waarvoor extra instellingen configureren, zoals geavanceerde verificatie-scenario's en het werken met geplande taken. Elk van deze valt in de latere secties.

>[AZURE.TIP] Wordt aanbevolen dat u lees- en de rest van dit onderwerp volledig begrijpt voordat u een upgrade start. Noteer eventuele functies die u gebruikt die hieronder worden genoemd.

De mobiele Services client SDK's zijn **niet** compatibel met de nieuwe mobiele Apps server SDK. Met het oog op de continuïteit van de dienst voor uw app, moet u geen wijzigingen publiceren naar een site die momenteel bedient gepubliceerde clients. In plaats daarvan maakt u een nieuwe mobiele toepassing die als een duplicaat fungeert. U kunt deze toepassing op hetzelfde plan App-Service om te voorkomen dat aanvullende financiële kosten plaatsen.

Vervolgens hebt u twee versies van de toepassing: die blijft hetzelfde en dient gepubliceerd apps in de natuur, en de andere die u vervolgens kunt bijwerken en doel met een nieuwe versie van de client. U kunt verplaatsen en testen van uw code in uw tempo, maar zorg ervoor dat u bug fixes, worden toegepast op beide. Als u van mening bent dat een aantal client-toepassingen in de natuur hebt bijgewerkt naar de nieuwste versie, u de oorspronkelijke gemigreerde app verwijderen kunt als u wenst.

Het volledige overzicht voor het upgradeproces is als volgt:

1. Maak een nieuwe Mobile App
2. Het project voor het gebruik van de nieuwe Server SDK's bijwerken
3. Een nieuwe versie van de clienttoepassing
4. (Optioneel) Uw oorspronkelijke exemplaar van de gemigreerde verwijderen

##<a name="mobile-app-version"></a>Een tweede toepassingsexemplaar maken
De eerste stap bij het uitvoeren van een upgrade is de bron van de mobiele App die is van de nieuwe versie van uw toepassing host maken. Als u een bestaande mobiele service al hebt gemigreerd, wilt u deze versie op hetzelfde hosting plan maken. Open de [Azure portal] en Ga naar de gemigreerde toepassing. Noteer de App Service Plan wordt uitgevoerd.

Maak vervolgens het tweede toepassingsexemplaar door de [.NET back-end maken instructies](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#create-app). Kies het plan van de gemigreerde toepassing wanneer u wordt gevraagd om te selecteren dat u serviceplan App of "hosting plan".

Waarschijnlijk zult u dezelfde database en kennisgeving Hub gebruiken zoals in mobiele Services. U kunt deze waarden door [Azure portal] te openen en navigeert naar de oorspronkelijke toepassing kopiëren en klik vervolgens op **Instellingen** > **Toepassingsinstellingen**. Kopiëren onder **Verbindingsreeksen**, `MS_NotificationHubConnectionString` en `MS_TableConnectionString`. Ga naar de nieuwe site upgrade en plak deze in alle bestaande waarden overschrijven. Herhaal dit proces voor andere toepassingsinstellingen uw app behoeften. Als een gemigreerde service niet gebruikt, kunt u op het tabblad **configureren** van de sectie Mobile Services van de [Azure klassieke portal]verbindingsreeksen en app-instellingen lezen.

Maak een kopie van de ASP.NET-project voor uw toepassing en publiceren naar uw nieuwe site. Een kopie van de aanvraag van de client bijgewerkt met de nieuwe URL gebruikt, controleren of alles werkt zoals verwacht.

## <a name="updating-the-server-project"></a>Bijwerken van de serverproject

Mobiele Apps biedt een nieuwe [Mobile App Server SDK] die vrijwel dezelfde functionaliteit als de runtime mobiele Services biedt. Eerst moet u alle verwijzingen naar de mobiele Services-pakketten. Zoeken in de NuGet package manager, `WindowsAzure.MobileServices.Backend`. De meeste apps zien verschillende pakketten, met inbegrip van `WindowsAzure.MobileServices.Backend.Tables` en `WindowsAzure.MobileServices.Backend.Entity`. In een dergelijk geval beginnen met het laagste pakket in de afhankelijkheidsstructuur van de, zoals `Entity`, en deze te verwijderen. Wanneer dat wordt gevraagd, verwijder alle afhankelijke pakketten niet. Herhaal deze procedure totdat u hebt verwijderd `WindowsAzure.MobileServices.Backend` zelf.

Op dit punt hebt u een project dat niet meer wordt verwezen naar Mobile Services SDK's.

Vervolgens voegt u verwijzingen naar de SDK van mobiele Apps. Voor deze upgrade, de meeste ontwikkelaars wilt downloaden en installeren van de `Microsoft.Azure.Mobile.Server.Quickstart` pakket, zoals dit, in de hele set vereist klikt wordt.

Er is nogal wat Compilerfouten die voortvloeien uit verschillen tussen de SDK's, maar deze eenvoudige adres en vallen in de rest van deze sectie.

### <a name="base-configuration"></a>Basisconfiguratie

Klik in het WebApiConfig.cs, u het volgende vervangen kunt:

        // Use this class to set configuration options for your mobile service
        ConfigOptions options = new ConfigOptions();

        // Use this class to set WebAPI configuration options
        HttpConfiguration config = ServiceConfig.Initialize(new ConfigBuilder(options));

met

        HttpConfiguration config = new HttpConfiguration();
        new MobileAppConfiguration()
            .UseDefaultConfiguration()
        .ApplyTo(config);

>[AZURE.NOTE] Als u weten over de nieuwe .NET server SDK en functies van uw app toevoegen of verwijderen wilt, raadpleegt u het onderwerp van [het gebruik van het .NET server SDK] .

Als uw app maakt gebruik van de verificatiefuncties voor, moet u ook een middleware OWIN registreren. In dit geval moet u de bovenstaande configuratiecode verplaatsen naar een nieuwe klasse van OWIN opstarten.

1. Het pakket NuGet toevoegen `Microsoft.Owin.Host.SystemWeb` als het niet in uw project opgenomen is.
2. Klik met de rechtermuisknop op het project in Visual Studio en selecteer **toevoegen** -> **Nieuw Item**. Selecteer **Web** -> **Algemeen** -> **OWIN opstarten klasse**.
3. Verplaatsen van de bovenstaande code voor de MobileAppConfiguration van `WebApiConfig.Register()` op de `Configuration()` methode van de opstartklasse van uw nieuwe.

Zorg ervoor dat de `Configuration()` methode eindigt met:

        app.UseWebApi(config)
        app.UseAppServiceAuthentication(config);

Er zijn aanvullende wijzigingen betrekking hebben op verificatie waarvoor volledige verificatie hieronder.

### <a name="working-with-data"></a>Werken met gegevens

In mobiele Services, de naam van de mobiele app dienden als de naam van het standaard-schema in de entiteit Framework setup.

Om ervoor te zorgen dat u beschikt over hetzelfde schema als voorheen, gebruik het volgende in de DbContext voor uw toepassing stelt u het schema waarnaar wordt verwezen:

        string schema = System.Configuration.ConfigurationManager.AppSettings.Get("MS_MobileServiceName");

Zorg ervoor dat u hebt ingesteld als u de bovenstaande MS_MobileServiceName. Als uw toepassing eerder dit aangepast, kunt u de naam van een ander schema bieden.

### <a name="system-properties"></a>Systeemeigenschappen

#### <a name="naming"></a>Naamgeving

In de server Azure Mobile Services SDK Systeemeigenschappen altijd een dubbele onderstrepingsteken bevatten (`__`) voorvoegsel voor de eigenschappen:

- __createdAt
- __updatedAt
- __deleted
- __version

De mobiele Services client SDK's speciale logica voor het parseren van Systeemeigenschappen in deze indeling hebben.

In Azure Mobile Apps, Systeemeigenschappen is niet langer een speciale indeling hebben en hebben de volgende namen:

- createdAt
- updatedAt
- verwijderd
- Versie

De mobiele Apps client SDK's gebruiken de nieuwe eigenschappen van namen, zodat er geen wijzigingen zijn vereist voor clientcode. Echter, als u rechtstreeks REST aanroepen van de service vervolgens moet u uw query's dienovereenkomstig.

#### <a name="local-store"></a>Lokale winkel

De wijzigingen in de namen van de eigenschappen van het bestandssysteem dat een lokale database off line synchronisatie voor mobiele Services is niet compatibel met mobiele Apps. Indien mogelijk, Vermijd client apps van mobiele Services voor mobiele Apps pas na wijzigingen in behandeling zijn verzonden naar de server bijgewerkt. De bijgewerkte app moet vervolgens een nieuwe bestandsnaam van de database gebruikt.

Een client-app is bijgewerkt van mobiele Services naar mobiele Apps terwijl er off line wijzigingen in behandeling in de wachtrij voor bewerking, moet de database worden bijgewerkt voor het gebruik van de nieuwe namen. Op iOS, kan dit worden bereikt met lichte migraties de kolomnamen wijzigen. Op Android en de beheerde client .NET, moet u aangepaste SQL om de naam van de kolommen voor de gegevenstabellen-object te schrijven.

Op iOS, moet u uw gegevens Core schema voor de gegevensentiteiten overeenkomen met de volgende. Houd er rekening mee dat de eigenschappen `createdAt`, `updatedAt` en `version` niet langer een `ms_` prefix:

| Kenmerk |  Type   | Opmerking                                                 |
|---------- |  ------ | -----------------------------------------------------|
| ID        | String, gemarkeerd als vereist  | primaire sleutel in de externe opslag         |
| createdAt | Datum    | (optioneel) wordt toegewezen aan de eigenschap createdAt-systeem         |
| updatedAt | Datum    | (optioneel) wordt toegewezen aan de eigenschap updatedAt-systeem         |
| Versie   | Tekenreeks  | (optioneel) gebruikt voor het detecteren van conflicten tussen toewijzingen naar versie |

#### <a name="querying-system-properties"></a>Bij het controleren van het dialoogvenster Systeemeigenschappen

In Azure Mobile Services Systeemeigenschappen worden niet verzonden standaard, maar alleen als ze worden opgevraagd met de querytekenreeks `__systemProperties`. Daarentegen in Azure Mobile Apps systeem zijn **altijd geselecteerd** omdat ze deel van het objectmodel van de server-SDK uitmaken.

Deze wijziging heeft vooral gevolgen voor aangepaste implementaties van domein managers, zoals uitbreidingen van `MappedEntityDomainManager`. In mobiele diensten, als een client nooit vraagt om de Systeemeigenschappen van een is het mogelijk gebruik van een `MappedEntityDomainManager` die alle eigenschappen niet daadwerkelijk toewijzen. Deze niet-toegewezen eigenschappen wordt echter in Azure Mobile Apps veroorzaken een fout in query's krijgen.

De eenvoudigste manier om het probleem te verhelpen is het wijzigen van uw DTOs zodat ze van overnemen `ITableData` in plaats van `EntityData`. Voeg vervolgens de `[NotMapped]` van het kenmerk in de velden die moeten worden weggelaten.

Het volgende definieert bijvoorbeeld `TodoItem` zonder systeem eigenschappen:

    using System.ComponentModel.DataAnnotations.Schema;

    public class TodoItem : ITableData
    {
        public string Text { get; set; }

        public bool Complete { get; set; }

        public string Id { get; set; }

        [NotMapped]
        public DateTimeOffset? CreatedAt { get; set; }

        [NotMapped]
        public DateTimeOffset? UpdatedAt { get; set; }

        [NotMapped]
        public bool Deleted { get; set; }

        [NotMapped]
        public byte[] Version { get; set; }
    }

Opmerking: als er fouten op `NotMapped`, Voeg een verwijzing naar de assembly `System.ComponentModel.DataAnnotations`.

### <a name="cors"></a>CORS

Sommige ondersteuning voor CORS Mobile Services opgenomen door de ASP.NET-CORS-oplossing. Deze onmiddellijke laag is zodat de ontwikkelaars meer controle, zodat u rechtstreeks van [Ondersteuning voor ASP.NET-CORS profiteren kunt](http://www.asp.net/web-api/overview/security/enabling-cross-origin-requests-in-web-api)verwijderd.

De belangrijkste gebieden van belang als CORS zijn die de `eTag` en `Location` headers moeten worden toegestaan om de client-SDK's goed te laten werken.

### <a name="push-notifications"></a>Push-meldingen
Voor push is het belangrijkste artikel dat wellicht ontbreken uit de Server-SDK de klasse PushRegistrationHandler. Registraties iets anders worden verwerkt in de mobiele Apps en tagless registraties zijn standaard ingeschakeld. Beheer van codes worden uitgevoerd met behulp van aangepaste API's. Raadpleeg de instructies [voor codes registreren](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#tags) voor meer informatie.

### <a name="scheduled-jobs"></a>Geplande taken
Geplande taken worden niet ingebouwd in mobiele Apps, zodat alle bestaande taken die u in uw backend .NET hebt moet afzonderlijk worden bijgewerkt. Eén mogelijkheid is voor het maken van een geplande [Taak Web] op de website Mobile App-code. U kan ook een domeincontroller die de taakcode instellen en configureren de [Azure Scheduler] om dat eindpunt op de verwachte planning geraakt.

### <a name="miscellaneous-changes"></a>Diverse wijzigingen
Alle ApiControllers die wordt verbruikt door een mobiele client moet nu de `[MobileAppController]` kenmerk. Dit is niet meer standaard zodat andere ApiControllers gaan niet beïnvloed door de mobiele formatters opgenomen.

De `ApiServices` object niet langer deel uitmaakt van de SDK. Mobiele App om instellingen te openen, kunt u het volgende:

    MobileAppSettingsDictionary settings = this.Configuration.GetMobileAppSettingsProvider().GetMobileAppSettings();

Logboekregistratie is ook nu uitgevoerd met behulp van de standaard ASP.NET-trace geschreven:

    ITraceWriter traceWriter = this.Configuration.Services.GetTraceWriter();
    traceWriter.Info("Hello, World");  

##<a name="authentication"></a>Overwegingen met betrekking tot verificatie

De onderdelen van de verificatie van mobiele diensten zijn nu in de App Service verificatie/machtigingsvoorziening verplaatst. Meer over het inschakelen van dit voor uw site door te lezen van het onderwerp van de [verificatie van uw mobiele app toevoegen](app-service-mobile-ios-get-started-users.md) .

Voor sommige providers, zoals AAD, Facebook en Google, moet u gebruikmaken van de bestaande registratie vanuit uw toepassing kopiëren. U hoeft alleen naar de identiteitsprovider portal en een nieuwe Omleidings-URL toevoegen aan de registratie. App Service Authentication/Authorization configureert met de client-ID en een geheim.

### <a name="controller-action-authorization"></a>Controller actie vergunning
Alle exemplaren van de `[AuthorizeLevel(AuthorizationLevel.User)]` kenmerk moet nu worden gewijzigd voor het gebruik van de standaard ASP.NET `[Authorize]` kenmerk. Bovendien zijn domeincontrollers nu Anoniem standaard als in andere ASP.NET-toepassingen.
Als u een van de andere AuthorizeLevel opties zoals Admin of toepassing, houd er rekening mee dat deze verdwenen zijn. In plaats daarvan kunt u AuthorizationFilters instellen voor gedeelde geheimen of een AAD Service Principal veilig zodat gesprekken Services configureren.

### <a name="getting-additional-user-information"></a>Aanvullende informatie ophalen

Krijgt u extra informatie, met inbegrip van toegangstokens door middel van de `GetAppServiceIdentityAsync()` methode:

        FacebookCredentials creds = await this.User.GetAppServiceIdentityAsync<FacebookCredentials>();

Ook als uw toepassing afhankelijkheden op gebruikers-id's, zoals ze op te slaan in een database, is het belangrijk te weten dat de gebruikers-id's tussen Mobile Services en Mobile Apps in App Service verschillend zijn. U kunt de gebruikersnaam van mobiele diensten echter nog steeds. Alle subklassen van ProviderCredentials hebben een eigenschap UserId. Dus door te gaan in het voorbeeld voor:

        string mobileServicesUserId = creds.Provider + ":" + creds.UserId;

Als uw app alle afhankelijkheden op gebruikers-id's neemt, is het belangrijk dat u gebruikmaken van dezelfde registratie bij een identiteitsprovider indien mogelijk. Gebruikers-id's zijn meestal binnen het bereik van de registratie van de toepassing die werd gebruikt, zodat de introductie van een nieuwe registratie problemen met overeenkomende gebruikers hun gegevens kan maken.

### <a name="custom-authentication"></a>Aangepaste verificatie

Als uw toepassing een aangepaste verificatie-oplossing gebruikt, zult u om ervoor te zorgen dat de bijgewerkte site toegang tot het systeem heeft. Volg de nieuwe instructies voor aangepaste verificatie in het [overzicht van .NET server SDK] uw oplossing integreren. Houd er rekening mee dat de aangepaste verificatie-onderdelen nog steeds in de voorbeeldweergave zijn.

##<a name="updating-clients"></a>Bijwerken van clients
Zodra er een operationele Mobile App back-end, kunt u werken op een nieuwe versie van uw clienttoepassing die het verbruikt. Mobiele Apps bevat ook een nieuwe versie van de client-SDK's en vergelijkbaar is met de serverupgrade hierboven, moet u alle verwijzingen naar de mobiele Services SDK's verwijderen voordat u de installatie van de versies van mobiele Apps.

Een van de belangrijkste verschillen tussen de versies is dat de constructors niet langer een sleutel van toepassing vereist. U nu doorgeven gewoon in de URL van uw mobiele App. Bijvoorbeeld op de clients .NET de `MobileServiceClient` constructor is nu:

        public static MobileServiceClient MobileService = new MobileServiceClient(
            "https://contoso.azurewebsites.net", // URL of the Mobile App
        );

U kunt lezen over de nieuwe SDK's installeren en gebruiken van de nieuwe structuur via de onderstaande koppelingen:

- [iOS versie 3.0.0 of hoger](app-service-mobile-ios-how-to-use-client-library.md)
- [.NET (Windows/Xamarin) versie 2.0.0 of hoger](app-service-mobile-dotnet-how-to-use-client-library.md)

Als uw toepassing maakt gebruik van push-meldingen, noteer de specifieke registratie instructies voor elk platform, als er enkele wijzigingen er ook zijn.

Wanneer u een nieuwe versie van de client klaar hebt, probeer het tegen uw serverproject bijgewerkt. Na de validatie van het werkt, kunt u een nieuwe versie van uw toepassing naar klanten vrijgeven. Uiteindelijk, zodra uw klanten hebben al een kans om deze updates te ontvangen, kunt u de diensten voor mobiele versie van uw app verwijderen. U hebt nu volledig bijgewerkt naar een App Service Mobile App met behulp van de nieuwste mobiele Apps server SDK.

<!-- URLs. -->

[Azure portal]: https://portal.azure.com/
[Azure klassieke portal]: https://manage.windowsazure.com/
[Wat zijn mobiele Apps?]: app-service-mobile-value-prop.md
[I already use web sites and mobile services – how does App Service help me?]: /en-us/documentation/articles/app-service-mobile-value-prop-migration-from-mobile-services
[Mobiele App Server SDK]: http://www.nuget.org/packages/microsoft.azure.mobile.server
[Create a Mobile App]: app-service-mobile-xamarin-ios-get-started.md
[Add push notifications to your mobile app]: app-service-mobile-xamarin-ios-get-started-push.md
[Add authentication to your mobile app]: app-service-mobile-xamarin-ios-get-started-users.md
[Azure Scheduler]: /en-us/documentation/services/scheduler/
[Web taak]: ../app-service-web/websites-webjobs-resources.md
[Het gebruik van het .NET server SDK]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Migrate from Mobile Services to an App Service Mobile App]: app-service-mobile-migrating-from-mobile-services.md
[Migrate your existing Mobile Service to App Service]: app-service-mobile-migrating-from-mobile-services.md
[Serviceprijsstelling App]: https://azure.microsoft.com/en-us/pricing/details/app-service/
[.NET server SDK-overzicht]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
