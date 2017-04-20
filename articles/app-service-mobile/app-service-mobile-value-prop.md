<properties
    pageTitle="Wat zijn mobiele Apps"
    description="Ontdek welke voordelen App Service brengen aan uw onderneming mobiele toepassingen."
    services="app-service\mobile"
    documentationCenter=""
    authors="adrianhall"
    manager="yochayk"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="na"
    ms.tgt_pltfrm="mobile-multiple"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="getting-started"> </a>Wat is Mobile Apps?

Azure App-Service is een volledig beheerde [Platform als Service](https://azure.microsoft.com/overview/what-is-paas/) (PaaS) voor professionele ontwikkelaars die ervoor zorgt dat een groot aantal mogelijkheden voor het web, mobiele scenario's en integratie. *Mobile Apps* in *Azure App Service* bieden een zeer schaalbare, algemeen beschikbaar mobiel development platform voor softwareontwikkelaars en systeemintegrators die een rijke verzameling functies voor ontwikkelaars van mobiele biedt.

![Mobiele toepassingen](./media/app-service-mobile-value-prop/overview.png)

##<a name="why-mobile-apps"></a>Waarom mobiele Apps?
*Mobile Apps* in *Azure App-Service* biedt een zeer schaalbare, algemeen beschikbaar mobiel development platform voor softwareontwikkelaars en systeemintegrators die een rijke verzameling functies voor ontwikkelaars van mobiele biedt. Met mobiele Apps kunt u:

- **Native en cross-platform apps bouwen** - of u bouwt native iOS, Android en Windows apps of Xamarin of Cordova (Phonegap) van cross-platform apps, kunt u profiteren van de App met native SDK's.
- **Verbinding maken met uw bedrijfssystemen** - met mobiele Apps kunt u zakelijke teken op in minuten, en verbinding maken met uw onderneming op locatie of cloud resources.
- **Off line-ready apps met gegevenssynchronisatie maken** - dat uw mobiele medewerkers productief door apps bouwen die offline werken en mobiele Apps gegevens te synchroniseren op de achtergrond gebruiken wanneer er verbinding is met een van de gegevensbronnen van de onderneming of SaaS APIs.
- **Push-meldingen naar miljoenen in seconden** - uitoefenen van uw klanten met direct push-meldingen op elk apparaat, afgestemd op hun behoeften, verzonden als de juiste is.

## <a name="mobile-app-features"></a>Mobiele App-functies
De volgende functies zijn belangrijk voor mobiele ontwikkeling wolk is ingeschakeld:

- **Verificatie en autorisatie** - selecteren uit een groeiende lijst van id-providers voor de verificatie van de onderneming, met inbegrip van Azure Active Directory plus sociale voorzieningen zoals Facebook, Google, Twitter en Microsoft-Account.  Azure Mobile Apps biedt een service OAuth 2.0 voor elke provider.  U kunt de SDK ook voor de id-provider voor specifieke provider-functionaliteit integreren.

  Ontdek meer over onze [verificatiefuncties].

- **Data Access** - Azure mobiele Apps kunt u een mobiele-vriendelijke OData v3 gegevensbron gekoppeld aan SQL Azure of een SQL-Server op locatie.  Deze service kan worden gebaseerd op de entiteit kader, zodat u eenvoudig integreren met andere NoSQL en SQL-gegevensproviders, met inbegrip van [Azure tabelopslag], MongoDB, [DocumentDB] en API SaaS-aanbieders, zoals Office 365 en Salesforce.com.
- **Off line synchronisatie** - onze Client SDK's moet u robuuste en responsieve mobiele toepassingen maken die met een off line gegevens werken gemakkelijk instellen die kunnen automatisch worden gesynchroniseerd met de backend-gegevens, inclusief ondersteuning voor conflict resolutie.

  Ontdek meer over onze [functies].

- **Push-meldingen** : onze Client SDK's naadloos integreren met de mogelijkheden van de registratie van Azure melding Hubs, zodat u kunt de push-meldingen voor miljoenen gebruikers tegelijk verzenden.

  Ontdek meer over onze [functies voor meldingen push].

- **Client SDK's** - wij bieden een volledige set van de Client-SDK's die voorzien in eigen ontwikkeling ([iOS], [Android] en [Windows]), cross-platform development ([Xamarin voor iOS en Android], [Xamarin vormen]) en hybride toepassingen ontwikkelen ([Apache Cordova]).  Elke client SDK is beschikbaar met een MIT-licentie en open source.

## <a name="azure-app-service-features"></a>Azure App functies.
De volgende functies zijn in het algemeen nuttig voor mobiele productielocaties.

- **Automatisch schalen** - App Service kunt u snel opschaling of uit te verwerken binnenkomende klant belasting. Handmatig selecteren van het aantal en de grootte van VMs of automatische schaling instellen voor het schalen van uw mobiele app backend op basis van de lading of schema.

  Ontdek meer over de [Automatische schaling].

- **Staging-omgevingen** - App Service kan worden uitgevoerd meerdere versies van uw site, zodat u kunt uitvoeren van A / B testen, testen in de productie als onderdeel van een groter plan met DevOps en ter plaatse gefaseerde installatie van een nieuwe back-end.

  Ontdek meer over [testomgevingen].

- **Continuous Deployment** - App Service kan worden geïntegreerd met algemene SCM-systemen, zodat u automatisch een nieuwe versie van uw back-end implementatie door te drukken op een tak van de SCM-systeem.

  Ontdek meer over [implementatie-opties].

- **Virtuele netwerken** - App Service verbinding kan maken met voor bedrijfsruimten resources met behulp van virtuele netwerk, ExpressRoute of hybride verbindingen.

  Ontdek meer over [hybride verbindingen], [virtuele netwerken]en [ExpressRoute].

- **Geïsoleerde / specifieke omgevingen** -App-Service kan worden uitgevoerd in een volledig geïsoleerd en exclusieve omgeving voor het veilig uitvoeren van Azure App Service apps op grote schaal.  Dit is ideaal voor de werkbelasting van toepassingen aan zeer hoge schaal, isolatie of beveiligde netwerktoegang vereist.

  Ontdek meer over [App Service-omgevingen].

## <a name="getting-started"></a>Aan de slag ##
Om te beginnen met mobiele Apps, volgt u de zelfstudie [Aan de slag] .  Hieronder vallen de basisbeginselen van het produceren van een mobiele back-end en een client van uw keuze, wordt verificatie, off line synchronisatie en push-meldingen te integreren.  U kunt volgen de zelfstudie [Aan de slag] meerdere malen - eenmaal voor elke clienttoepassing.

Bekijk onze [kaart leren]voor meer informatie over Azure Mobile Apps.
Zie voor meer informatie over het platform Azure App Service, [Azure App-Service].

>[AZURE.NOTE]Als u wilt aan de slag met Azure App-Service voordat u aanmelden voor een account met Azure, gaat u naar de [App-Service probeert](https://tryappservice.azure.com/?appServiceName=mobile), waar u direct een starter tijdelijk web app in de App-Service maken kunt. Geen creditcard vereist; geen verplichtingen.

<!-- URLs. -->
[Migrate your Mobile Service to App Service]: app-service-mobile-migrating-from-mobile-services.md
[Azure App-Service]: ../app-service/app-service-value-prop-what-is.md
[Aan de slag]: app-service-mobile-ios-get-started.md
[Azure tabelopslag]: ../storage/storage-getting-started-guide.md
[DocumentDB]: ../documentdb/documentdb-get-started.md
[verificatiefuncties]: ./app-service-mobile-auth.md
[functies van gegevens]: ./app-service-mobile-offline-data-sync.md
[functies voor push-meldingen]: ../notification-hubs/notification-hubs-push-notification-overview.md
[iOS]: ./app-service-mobile-ios-how-to-use-client-library.md
[Android]: ./app-service-mobile-android-how-to-use-client-library.md
[Windows]: ./app-service-mobile-dotnet-how-to-use-client-library.md
[Xamarin voor iOS en Android]: ./app-service-mobile-dotnet-how-to-use-client-library.md
[Xamarin formulieren]: ./app-service-mobile-xamarin-forms-get-started.md
[Apache-Cordova]: ./app-service-mobile-cordova-how-to-use-client-library.md
[automatisch schalen]: ../app-service-web/web-sites-scale.md
[testomgevingen]: ../app-service-web/web-sites-staged-publishing.md
[implementatie-opties]: ../app-service-web/web-sites-deploy.md
[hybride verbindingen]: ../app-service-web/web-sites-hybrid-connection-get-started.md
[virtuele netwerken]: ../app-service-web/web-sites-integrate-with-vnet.md
[ExpressRoute]: ../app-service-web/app-service-app-service-environment-network-configuration-expressroute.md
[De Service App omgevingen]: ../app-service-web/app-service-app-service-environment-intro.md
[leren werken met hyperlinks]: https://azure.microsoft.com/en-us/documentation/learning-paths/appservice-mobileapps/
