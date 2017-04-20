<properties
    pageTitle="Een upgrade uitvoeren van mobiele Services bij Azure App Service - Node.js"
    description="Meer informatie over het eenvoudige upgrade van uw mobiele Services-toepassing naar een App Service Mobile App"
    services="app-service\mobile"
    documentationCenter=""
    authors="adrianhall"
    manager="yochayk"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile"
    ms.devlang="node"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="upgrade-your-existing-nodejs-azure-mobile-service-to-app-service"></a>Uw bestaande Node.js Azure Mobile-Service upgraden naar App Service

Mobiele App-Service is een nieuwe manier van mobiele toepassingen met Microsoft Azure. Voor meer informatie, Zie [Wat zijn mobiele Apps?].

In dit onderwerp wordt beschreven hoe u een bestaande back-end-toepassing van Node.js upgrade van Azure Mobile Services naar een nieuwe mobiele Apps in App-Service. Terwijl u deze upgrade uitvoert, blijven uw bestaande Mobile Services-toepassing functioneren.  Als u een back-end Node.js toepassing bijwerken, kunt u verwijzen naar [uw .NET Mobile Services bijwerken](./app-service-mobile-net-upgrading-from-mobile-services.md).

Wanneer een mobiele backend is bijgewerkt naar Azure App Service, heeft toegang tot alle functies van de App Service en volgens de [serviceprijzen App], geen mobiele Services, prijzen worden gefactureerd.

## <a name="migrate-vs-upgrade"></a>Migreren te upgraden

[AZURE.INCLUDE [app-service-mobile-migrate-vs-upgrade](../../includes/app-service-mobile-migrate-vs-upgrade.md)]

>[AZURE.TIP] Het wordt aanbevolen dat u [een migratie uit te voeren](app-service-mobile-migrating-from-mobile-services.md) voordat u verdergaat met de upgrade. Op deze manier kunt u beide versies van de toepassing op de dezelfde App Service Plan te plaatsen en er geen extra kosten in rekening gebracht.

### <a name="improvements-in-mobile-apps-nodejs-server-sdk"></a>Verbeteringen in mobiele Apps Node.js server SDK

Een upgrade naar de nieuwe [Mobiele Apps SDK](https://www.npmjs.com/package/azure-mobile-apps) biedt een groot aantal verbeteringen, waaronder:

- Op basis van het [framework Express](http://expressjs.com/en/index.html), het nieuwe knooppunt SDK is lichtgewicht en ontworpen om u te houden met nieuwe versies van het knooppunt als zij afkomstig zijn uit. U kunt het gedrag van de toepassing met Express middleware.

- Aanzienlijke prestatieverbeteringen in vergelijking met de SDK van mobiele Services.

- U kunt nu een website hosten met uw mobiele back-end; op dezelfde manier is het eenvoudig om de Azure Mobile SDK voor alle bestaande express.v4-toepassingen.

- Ontwikkeld voor cross-platform en de lokale ontwikkeling en kunnen de SDK van mobiele Apps worden ontwikkeld en lokaal uitvoeren op Windows, Linux en OSX. Het is nu te gebruiken gemeenschappelijke knooppunt ontwikkelingstechnieken zoals [Mocha](https://mochajs.org/) tests vóór implementatie uitvoert.

## <a name="overview"></a>Basic upgrade, overzicht

Om u te helpen bij het upgraden van een backend Node.js, Azure App-Service heeft een pakket compatibiliteit zijn opgegeven.  Na de upgrade hebt u een niew site die kan worden toegepast op een nieuwe site voor App-Service.

De mobiele Services client SDK's zijn **niet** compatibel met de nieuwe mobiele Apps server SDK. Met het oog op de continuïteit van de dienst voor uw app, moet u geen wijzigingen publiceren naar een site die momenteel bedient gepubliceerde clients. In plaats daarvan maakt u een nieuwe mobiele toepassing die als een duplicaat fungeert. U kunt deze toepassing op hetzelfde plan App-Service om te voorkomen dat aanvullende financiële kosten plaatsen.

Vervolgens hebt u twee versies van de toepassing: dat blijft hetzelfde en dat apps gepubliceerd in de natuur, en de andere die u vervolgens kunt bijwerken en doel met een nieuwe versie van de client. U kunt verplaatsen en testen van uw code in uw tempo, maar zorg ervoor dat u bug fixes, worden toegepast op beide. Als u van mening bent dat een aantal client-toepassingen in de natuur hebt bijgewerkt naar de nieuwste versie, u de oorspronkelijke gemigreerde app verwijderen kunt als u wenst. Het wordt niet alle aanvullende kosten monetaire, als opgenomen in hetzelfde plan als uw App mobiele App-Service.

Het volledige overzicht voor het upgradeproces is als volgt:

1. Download uw bestaande (gemigreerde) Azure Mobile-Service.
2. Het project converteren naar een Azure Mobile App met behulp van het pakket compatibiliteit.
3. Corrigeer eventuele verschillen (zoals de verificatie-instellingen).
4. Uw geconverteerde Azure Mobile App project implementeren op een nieuwe App-Service.
4. Een nieuwe versie van uw clienttoepassing die de nieuwe mobiele App te gebruiken.
5. (Optioneel) Verwijder de oorspronkelijke gemigreerde mobiele-app.

Verwijdering kan optreden als u niet al het verkeer op uw oorspronkelijke gemigreerde mobiele service ziet.

## <a name="install-npm-package"></a>Installeren van de noodzakelijke voorwaarden

[Knooppunt] moet u installeren op uw lokale computer.  U dient ook het pakket compatibiliteit installeren.  Nadat een knooppunt is geïnstalleerd, kunt u de volgende opdracht uitvoeren vanaf een nieuwe cmd of PowerShell-prompt:

```npm i -g azure-mobile-apps-compatibility```

## <a name="obtain-ams-scripts"></a>Uw mobiele Services Azure-Scripts downloaden

- Log in op de [Azure Portal].
- Met **Alle bronnen** of **Services App**, uw mobiele Services-site vinden.
- Binnen de site, klik op **Extra** -> **Kudu** -> **gaan** de Kudu-website te openen.
- Klik in de **Foutopsporingsconsole** -> **PowerShell** de Foutopsporingsconsole openen.
- Ga naar `site/wwwroot/App_Data/config` door te klikken op elke map op zijn beurt
- Klik op de downloadpictogram naast de `scripts` directory.

Deze worden scripts in ZIP-indeling gedownload.  Maak een nieuwe map op uw lokale computer en uitpakken van de `scripts.ZIP` bestand in de directory.  Hiermee maakt u een `scripts` directory.

## <a name="scaffold-app"></a>De nieuwe mobiele Apps in Azure end scaffold

Voer de volgende opdracht uit vanuit de map die de map scripts:

```scaffold-mobile-app scripts out```

Hiermee maakt u een scaffolded backend Azure Mobile Apps in de `out` directory.  Hoewel dit niet is vereist, is het een goed idee om de `out` map in een opslagplaats van de bron code van uw keuze.

## <a name="deploy-ama-app"></a>Uw backend Azure Mobile Apps implementeren

Tijdens de implementatie moet u het volgende doen:

1. Maak een nieuwe Mobile App in de [Portal Azure].
2. Voer het `createViews.sql` script op de database verbonden.
3. Koppel de database die is gekoppeld aan uw mobiele Service naar uw nieuwe App-Service.
4. Andere bronnen (zoals Hubs kennisgeving) een koppeling naar de nieuwe App-Service.
5. De gegenereerde code implementeren op uw nieuwe site.

### <a name="create-a-new-mobile-app"></a>Maak een nieuwe Mobile App

1. Inloggen op de [Portal Azure].

2. Klik op **+ Nieuw** > **Web + Mobile** > **Mobile App**, Geef een naam op voor uw mobiele App backend.

3. Selecteer een bestaande brongroep voor de **Resourcegroep**of een nieuwe maken (met dezelfde naam als uw app). 
 
    Selecteer een andere App Service plan of een nieuwe maken. Voor meer informatie over App Services plannen en maken van een nieuw plan in verschillende prijzen trapsgewijs en Zie [Azure App Service plannen diepgaand overzicht](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md)op de gewenste locatie.

4. Voor de **App serviceplan**is het standaardschema (in de [standaard laag](https://azure.microsoft.com/pricing/details/app-service/)) geselecteerd. U kunt ook een ander schema, of [Maak een nieuwe](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md#create-an-app-service-plan)selecteren. De App Service-schema-instellingen bepalen de [locatie, functies, de kosten en resources te berekenen](https://azure.microsoft.com/pricing/details/app-service/) die zijn gekoppeld aan uw app. 

    Nadat u een over het plan besluit, klikt u op **maken**. Hiermee maakt u de back-end mobiele App. 


### <a name="run-createviewssql"></a>CreateViews.SQL uitvoeren

De scaffolded app bevat een bestand met de naam `createViews.sql`.  Dit script moet worden uitgevoerd op de doeldatabase.  De verbindingsreeks voor de doeldatabase kan worden verkregen uit de gemigreerde mobiele service van de bladeserver **Instellingen** onder **Verbindingsreeksen**.  Het heet `MS_TableConnectionString`.

U kunt dit script in SQL Server Management Studio of Visual Studio kunt uitvoeren.

### <a name="link-the-database-to-your-app-service"></a>De Database met uw Service App koppelen

De bestaande database een koppeling naar uw App-Service:

- Open in de [Azure Portal]uw App-Service.
- Selecteer **alle instellingen** -> **gegevensverbindingen**.
- Klik op **+ toevoegen**.
- Selecteer in de vervolgkeuzelijst **SQL-Database**
- Onder **SQL-Database**, selecteert u de bestaande database, klik vervolgens op **selecteren**.
- Onder de **verbindingsreeks**, voert u de gebruikersnaam en het wachtwoord voor de database, klik vervolgens op **OK**.
- Klik op **OK**in het blad **gegevensverbindingen toevoegen** .

De gebruikersnaam en het wachtwoord kunnen u vinden door de verbindingsreeks voor de doeldatabase in het gemigreerde Mobile Service weergeven.


### <a name="set-up-authentication"></a>Verificatie instellen

Azure mobiele Apps kunt u Azure Active Directory, Facebook, Google, Microsoft en Twitter-verificatie configureren in de service.  Aangepaste verificatie moet afzonderlijk worden ontwikkeld.  Raadpleeg de documentatie [Concepten voor verificatie] en [Verificatie Quickstart] -documentatie voor meer informatie.  

## <a name="updating-clients"></a>Mobiele clients bijwerken

Zodra er een operationele Mobile App back-end, kunt u werken op een nieuwe versie van uw clienttoepassing die het verbruikt. Mobiele Apps bevat ook een nieuwe versie van de client-SDK's en vergelijkbaar is met de serverupgrade hierboven, moet u alle verwijzingen naar de mobiele Services SDK's verwijderen voordat u de installatie van de versies van mobiele Apps.

Een van de belangrijkste verschillen tussen de versies is dat de constructors niet langer een sleutel van toepassing vereist. U nu doorgeven gewoon in de URL van uw mobiele App. Bijvoorbeeld op de clients .NET de `MobileServiceClient` constructor is nu:

        public static MobileServiceClient MobileService = new MobileServiceClient(
            "https://contoso.azurewebsites.net", // URL of the Mobile App
        );

U kunt lezen over de nieuwe SDK's installeren en gebruiken van de nieuwe structuur via de onderstaande koppelingen:

- [Android versie 2.2 of hoger](app-service-mobile-android-how-to-use-client-library.md)
- [iOS versie 3.0.0 of hoger](app-service-mobile-ios-how-to-use-client-library.md)
- [.NET (Windows/Xamarin) versie 2.0.0 of hoger](app-service-mobile-dotnet-how-to-use-client-library.md)
- [Cordova Apache versie 2.0 of hoger](app-service-mobile-cordova-how-to-use-client-library.md)

Als uw toepassing maakt gebruik van push-meldingen, noteer de specifieke registratie instructies voor elk platform, als er enkele wijzigingen er ook zijn.

Wanneer u een nieuwe versie van de client klaar hebt, probeer het tegen uw serverproject bijgewerkt. Na de validatie van het werkt, kunt u een nieuwe versie van uw toepassing naar klanten vrijgeven. Uiteindelijk, zodra uw klanten hebben al een kans om deze updates te ontvangen, kunt u de diensten voor mobiele versie van uw app verwijderen. U hebt nu volledig bijgewerkt naar een App Service Mobile App met behulp van de nieuwste mobiele Apps server SDK.

<!-- URLs. -->

[Azure portal]: https://portal.azure.com/
[Azure classic portal]: https://manage.windowsazure.com/
[Wat zijn mobiele Apps?]: app-service-mobile-value-prop.md
[I already use web sites and mobile services – how does App Service help me?]: /en-us/documentation/articles/app-service-mobile-value-prop-migration-from-mobile-services
[Mobile App Server SDK]: https://www.npmjs.com/package/azure-mobile-apps
[Create a Mobile App]: app-service-mobile-xamarin-ios-get-started.md
[Add push notifications to your mobile app]: app-service-mobile-xamarin-ios-get-started-push.md
[Add authentication to your mobile app]: app-service-mobile-xamarin-ios-get-started-users.md
[Azure Scheduler]: /en-us/documentation/services/scheduler/
[Web Job]: ../app-service-web/websites-webjobs-resources.md
[How to use the .NET server SDK]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Migrate from Mobile Services to an App Service Mobile App]: app-service-mobile-migrating-from-mobile-services.md
[Migrate your existing Mobile Service to App Service]: app-service-mobile-migrating-from-mobile-services.md
[Serviceprijsstelling App]: https://azure.microsoft.com/en-us/pricing/details/app-service/
[.NET server SDK overview]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Verificatie-concepten]: ../app-service/app-service-authentication-overview.md
[Quickstart verificatie]: app-service-mobile-auth.md

[Azure Portal]: https://portal.azure.com/
[OData]: http://www.odata.org
[Promise]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise
[basicapp sample on GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/basic-app
[todo sample on GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/todo
[samples directory on GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples
[static-schema sample on GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/static-schema
[QueryJS]: https://github.com/Azure/queryjs
[Node.js Tools 1.1 for Visual Studio]: https://github.com/Microsoft/nodejstools/releases/tag/v1.1-RC.2.1
[mssql Node.js package]: https://www.npmjs.com/package/mssql
[Microsoft SQL Server 2014 Express]: http://www.microsoft.com/en-us/server-cloud/Products/sql-server-editions/sql-server-express.aspx
[ExpressJS Middleware]: http://expressjs.com/guide/using-middleware.html
[Winston]: https://github.com/winstonjs/winston
