<properties
    pageTitle="Azure App serviceplannen voor diepgaand overzicht | Microsoft Azure"
    description="Lees hoe App serviceplannen voor Azure App Service werk, en hoe zij uw management-ervaring genieten."
    keywords="App-service, azure app service, schaal, schaalbare, app-serviceplan servicekosten app"
    services="app-service"
    documentationCenter=""
    authors="btardif"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/13/2016"
    ms.author="byvinyal"/>

# <a name="azure-app-service-plans-in-depth-overview"></a>Azure serviceplannen App voor diepgaand overzicht#

Een App serviceplan vertegenwoordigt een set van functies en capaciteit die u tussen meerdere apps delen kunt. Web Apps, mobiele Apps, Apps functie of API Apps, in alle [Azure App-Service](http://go.microsoft.com/fwlink/?LinkId=529714) worden uitgevoerd in een App serviceplan. Deze plannen ondersteunt vijf prijzen lagen: *vrije*, *gedeeld*, *Basic*, *Standard*en *Premium*. Elke fase heeft zijn eigen mogelijkheden en capaciteit. Apps in de geografische locatie en hetzelfde abonnement kunnen delen van een plan. Alle apps die een plan delen kunnen gebruiken voor de mogelijkheden en functies die zijn gedefinieerd door de fase van het plan. Alle apps die gekoppeld aan een plan zijn uitgevoerd voor de bronnen die in het plan worden gedefinieerd.

Bijvoorbeeld, als uw plan is geconfigureerd voor het gebruik van twee exemplaren van 'klein' in de standaard service-laag, alle apps die gekoppeld aan dat plan zijn uitgevoerd op beide exemplaren en toegang hebben tot de standaard service-tier functionaliteit. Plan exemplaren waarop toepassingen worden uitgevoerd, zijn volledig beheerde en hoge beschikbaarheid.

Dit artikel behandelt de belangrijkste kenmerken, zoals laag en de schaal van een serviceplan App en hoe deze rol spelen bij het beheren van uw toepassingen.

## <a name="apps-and-app-service-plans"></a>Apps en App serviceplannen

Een app in de App-Service kan worden gekoppeld aan slechts één App serviceplan op elk gewenst moment.

'S en apps zijn opgenomen in een resourcegroep. Een resourcegroep dient als de lifecycle-grens voor elke bron die valt binnen het. U kunt resourcegroepen gezamenlijk beheren van alle onderdelen van een toepassing.

Omdat één brongroep meerdere serviceplannen App hebben kan, kunt u verschillende apps voor verschillende fysieke middelen toewijzen. U kunt bijvoorbeeld resources tussen dev, test en productie omgevingen scheiden. Met aparte omgevingen voor productie en dev/test, kunt u bronnen te isoleren. Op deze manier testen tegen een nieuwe versie van uw apps laden niet zou concurreren voor dezelfde bronnen als de productie-apps, die zijn echte klanten bedienen.

Wanneer u meerdere plannen in een enkele groep hebt, kunt u ook een toepassing meerdere geografische regio's. Bijvoorbeeld, een hoge mate van beschikbaarheid app uitgevoerd in twee regio's opgenomen ten minste twee plannen, één voor elke regio en één app die is gekoppeld aan elk plan. In een dergelijke situatie worden vervolgens alle kopieën van de app opgenomen in één brongroep. Met een groep met meerdere plannen en meerdere apps gemakkelijk beheren, beheren en weergeven van de status van de toepassing.

## <a name="create-an-app-service-plan-or-use-existing-one"></a>Een App Service maken of bestaande gebruiken

Wanneer u een toepassing maakt, moet u overwegen een groep te maken. Aan de andere kant als de toepassing die u maakt een onderdeel van een grotere toepassing is, kan dit app moet worden gemaakt in de resourcegroep die toegewezen voor die toepassing groter.

Of de nieuwe app een geheel nieuwe toepassing of een deel van een groter is, kunt u met behulp van een bestaande App serviceplan host deze of een nieuwe maken. Deze beschikking is meer een kwestie van capaciteit en de verwachte belasting.

Als deze nieuwe app zal veel bronnen gebruiken en verschillende schaalfactoren vanuit andere apps in een bestaand schema gehost hebben, wordt u aangeraden in een eigen plan te isoleren.

Wanneer u een plan maakt, als u een nieuwe set bronnen toewijzen voor uw app en meer controle over de toewijzing van middelen krijgen omdat elk plan een eigen set exemplaren krijgt.

Omdat u apps over plannen verplaatsen kunt, kunt u de manier waarop resources worden toegewezen via de toepassing groter.

Tot slot, als u wilt een app maken in een andere regio en die regio niet een bestaand schema hebt, een schema maken in die regio kunnen als host voor uw app er.

## <a name="create-an-app-service-plan"></a>Een App Service maken

>[AZURE.TIP] Als u een App-omgeving kunt u de documentatie specifiek voor Service-omgevingen App hier bekijken: [een App Service plannen in een App-omgeving maken](../app-service-web/app-service-web-how-to-create-a-web-app-in-an-ase.md#createplan)

Kunt u een lege serviceplan App uit de App plan bladeren ervaring of als onderdeel van een app maken.

Klik op **Nieuw**in [Azure portal](https://portal.azure.com) > **Web + mobile**, en selecteer vervolgens **Web App** of ander type App Service app.
![Een app maken in de portal Azure.][createWebApp]

Vervolgens kunt u selecteren of het abonnement App voor de nieuwe toepassing maken.

 ![Maak een plan voor App-Service.][createASP]

Een nieuwe App-Service om plan te maken, klik op **[+] nieuw**en typ de naam van de **App serviceplan** Selecteer vervolgens de juiste **locatie**. Klik op **prijzen laag**en selecteer vervolgens een geschikte prijzen laag voor de service. Selecteer **Alles weergeven** bekijken meer prijsopties, zoals **vrije** en **gedeeld**. Nadat u de prijzen laag hebt geselecteerd, klikt u op de knop **selecteren** .

## <a name="move-an-app-to-a-different-app-service-plan"></a>Een app verplaatsen naar een andere App-serviceplan

U kunt een app verplaatsen naar een andere app serviceplan in [Azure portal](https://portal.azure.com). U kunt apps verplaatsen tussen plannen, zolang de plannen in de resourcegroep en geografische regio zijn.

Als u wilt een app verplaatsen naar een ander plan, gaat u naar de app die u wilt verplaatsen. Zoek in het menu **Instellingen** **Wijzigen App Service Plan**.

**Wijziging App serviceplan** opent de selector **App serviceplan** . Op dit punt kunt u een bestaand schema selecteren of een nieuwe maken. Alleen geldige plannen (in de resourcegroep en de geografische ligging) worden weergegeven.

![App Service plan selector.][change]

Elk plan heeft zijn eigen prijzen laag. Bijvoorbeeld wanneer u een site van een vrij laag verplaatsen naar een standaard laag uw app kunt nu gebruiken de functies en bronnen van de standaard laag.

## <a name="clone-an-app-to-a-different-app-service-plan"></a>Klonen van een app naar een andere App-serviceplan
Als u wilt dat de toepassing verplaatst naar een andere regio, een alternatieve app is klonen. Klonen, wordt een kopie van uw app in een nieuwe of bestaande App serviceplan of App-omgeving in elke regio.

 ![Klonen van een app.][appclone]

**Kloon App** kunt u vinden in het menu **Extra** .

Klonen kent een aantal beperkingen die u over op [Azure App Service App klonen met Azure portal lezen kunt](../app-service-web/app-service-web-app-cloning-portal.md).

## <a name="scale-an-app-service-plan"></a>Schaal van een App-serviceplan

Er zijn drie manieren voor het schalen van een plan:

- **Wijziging van het plan prijzen laag**. Bijvoorbeeld, een plan in de elementaire fase kan worden geconverteerd naar een Standard of Premium laag en alle apps die gekoppeld aan dat plan nu zijn kunnen de functies gebruiken die de nieuwe servicelaag biedt.
- **Grootte van de instantie van het plan te wijzigen**. Als u bijvoorbeeld kunt een plan in de elementaire fase met kleine exemplaren wijzigen voor het gebruik van grote exemplaren. Alle apps die gekoppeld aan dat plan nu zijn kunnen gebruiken voor het extra geheugen en CPU-bronnen die het groter exemplaar biedt.
- **Instantie het aantal van het plan te wijzigen**. Een standaard plan dat schaalvergroting toegepast op drie instanties kan bijvoorbeeld 10 exemplaren worden geschaald. Een Premium plan kan worden geschaald uit naar 20 exemplaren (afhankelijk van beschikbaarheid). Alle apps die gekoppeld aan dat plan nu zijn kunnen gebruiken voor het extra geheugen en CPU-bronnen die de telling van de grotere exemplaar biedt.

U kunt de prijzen laag en exemplaar grootte wijzigen door te klikken op de **Schaal van** item onder instellingen voor de app of de App serviceplan. Wijzigingen toepassen op het abonnement App en zijn van invloed op alle toepassingen die deze host.

 ![Stel de waarden voor het schalen van een app.][pricingtier]

## <a name="app-service-plan-cleanup"></a>App Service Plan opruimen
**App serviceniveaus** die hebben geen apps die zijn gekoppeld aan deze gelden de tarieven sinds worden geconfigureerd in de eigenschappen van App Service plan schaal berekenen capaciteit gereserveerd.
Om te voorkomen dat onverwachte kosten, wanneer de laatste app gehost in een App serviceplan wordt verwijderd, wordt de resulterende lege App serviceplan ook verwijderd.


## <a name="summary"></a>Samenvatting

App serviceplannen vertegenwoordigen een reeks functies en capaciteit die u via uw apps delen kunt. App serviceplannen geven u de flexibiliteit om specifieke apps voor een reeks resources toewijzen en het brongebruik Azure verder te optimaliseren. Op deze manier als u wilt dat om geld te besparen op uw testomgeving kunt u een plan delen tussen meerdere apps. U kunt ook doorvoer maximaliseren voor uw productieomgeving door schalen voor meerdere regio's en plannen.

## <a name="whats-changed"></a>Wat er veranderd

* Zie voor een uitleg met de wijziging van Websites met App-Service: [Azure App Service en de Impact op de bestaande Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)

[pricingtier]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/appserviceplan-pricingtier.png
[assign]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/assing-appserviceplan.png
[change]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/change-appserviceplan.png
[createASP]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/create-appserviceplan.png
[createWebApp]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/create-web-app.png
[appclone]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/app-clone.png
