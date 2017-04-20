<properties
    pageTitle="Azure melding Hubs"
    description="Informatie over het gebruik van push-meldingen in Azure. Voorbeelden van code is geschreven in C# .NET API gebruiken."
    authors="ysxu"
    manager="erikre"
    editor=""
    services="notification-hubs"
    documentationCenter=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="multiple"
    ms.devlang="multiple"
    ms.topic="hero-article"
    ms.date="08/25/2016"
    ms.author="yuaxu"/>


#<a name="azure-notification-hubs"></a>Azure melding Hubs

##<a name="overview"></a>Overzicht

Azure melding Hubs bieden een gemakkelijk te gebruiken, meerdere platforms, schaalvergroting push-infrastructuur waarmee u mobiele push-meldingen verzenden vanuit een back-end (in de cloud of op locatie) op een mobiel platform.

Met kennisgeving Hubs, kunt u gemakkelijk verzenden platforms, persoonlijke push-meldingen samenvatten van de details van de kennisgeving systemen voor verschillende platforms (PNS). U kunt afzonderlijke gebruikers of hele publiek segmenten met miljoenen gebruikers, over al hun apparaten richten met een enkele API-aanroep.

Voor de onderneming en de consument scenario's kunt u melding Hubs. Bijvoorbeeld:

- Laatste nieuws meldingen verzenden naar miljoenen met lage latentie (melding Hubs bevoegdheden Bing toepassingen vooraf geïnstalleerd op alle Windows en Windows Phone-apparaten).
- Op basis van locatie coupons op Gebruikersegmenten verzenden.
- Meldingen verzenden naar gebruikers of groepen voor sportspellen/Financiën/toepassingen.
- Waarschuw alle gebruikers van enterprise gebeurtenissen zoals nieuwe berichten en/of e-mails en potentiële verkopen.
- Één-eenmalige-wachtwoorden nodig zijn voor een meerledige verificatie verzenden.



##<a name="what-are-push-notifications"></a>Wat zijn Push-meldingen?

Smartphones en tablets kunnen "" gebruikers waarschuwen wanneer een gebeurtenis heeft plaatsgevonden. Deze meldingen kunnen vele vormen aannemen.

In de Windows Store en Windows Phone-toepassingen, kan de mededeling worden in de vorm van een _toast_: een niet-modaal venster wordt geopend met een geluid, een nieuwe melding signaal. Andere meldingstypen die worden ondersteund zijn _naast_ _raw_en _badge_ -meldingen. Zie voor meer informatie over de verschillende typen meldingen die worden ondersteund op apparaten met Windows, [tegels, Badges, en meldingen](http://msdn.microsoft.com/library/windows/apps/hh779725.aspx).

Op Apple iOS-apparaten wordt druk op dezelfde manier een dialoogvenster vraagt de gebruiker om te bekijken of de melding te sluiten. Op **weergave** te klikken opent de toepassing waarmee het bericht wordt ontvangen. Zie voor meer informatie over meldingen van iOS, [iOS-meldingen](http://go.microsoft.com/fwlink/?LinkId=615245).

Push-meldingen met behulp van mobiele apparaten vers weergeven terwijl de resterende energie-efficiënt. Meldingen kunnen worden verzonden door back-end systemen voor mobiele apparaten, zelfs wanneer de corresponderende apps op een apparaat zijn niet actief. Push-meldingen zijn een essentieel onderdeel voor de consument apps, waar ze worden gebruikt om te vergroten en het gebruik van app engagement. Meldingen zijn ook handig voor ondernemingen, wanneer de actuele informatie verhoogt de reactiesnelheid van de werknemer aan zakelijke activiteiten.

Specifieke voorbeelden van betrokkenheid bij de mobiele scenario's zijn:

1.  Een tegel op Windows 8 of Windows Phone wordt bijgewerkt met de huidige financiële informatie.
2.  Een gebruiker met een toast die een bepaalde taak is toegewezen aan die gebruiker, in een workflow gebaseerd enterprise app waarschuwen.
3.  Leads weergeven van een badge met het nummer van de huidige verkoop in een CRM-toepassing (zoals Microsoft Dynamics CRM).

##<a name="how-push-notifications-work"></a>Hoe Push-meldingen werk

Push-berichten worden afgeleverd via platform-specifieke infrastructuren _Meldingsregelingen Platform_ (PNS) genoemd. Een PNS biedt functies voor barebones (geen ondersteuning voor uitzending, personalisatie) en hebben geen gemeenschappelijke interface. Bijvoorbeeld een bericht verzenden naar een Windows Store-app, een ontwikkelaar moet contact opnemen met de WNS (Windows Notification Service). Een bericht verzenden naar een iOS-apparaat, heeft de dezelfde ontwikkelaar te maken met APNS (Apple Push Notification Service), en het bericht nogmaals te verzenden. Azure melding hubs helpen door middel van een algemene interface, ook andere functies ter ondersteuning van push-meldingen op elk platform.

Op een hoog niveau, alle platform Meldingsregelingen hetzelfde patroon volgen:

1.  De clienttoepassing contact op met de PNS voor het ophalen van de _ingang_. Het type handle is afhankelijk van het systeem. Voor WNS, is een URI of "melding channel". Voor APNS is een token.
2.  De clienttoepassing slaat deze ingang in de app _back-end_ voor later gebruik. De back-end is voor WNS, meestal een cloud-service. Apple heet het systeem een _provider_.
3.  Voor het verzenden van een push-bericht contact app back-end op met de PNS met de greep aan een specifieke client app-exemplaar als doel.
4.  De PNS stuurt de melding naar het apparaat dat is opgegeven door de greep.

![][0]

##<a name="the-challenges-of-push-notifications"></a>De uitdagingen van de Push-meldingen

Deze systemen zijn zeer krachtig, ze blijven veel werk aan de ontwikkelaar van app zelfs veelvoorkomende push notification scenario's, zoals uitzendt of push-meldingen verzenden naar gebruikers van gesegmenteerde worden geïmplementeerd.

Push-meldingen zijn een van de meest gevraagde functies van cloud-diensten voor mobiele toepassingen. De reden hiervoor is dat de infrastructuur die nodig zijn om deze werken vrij ingewikkeld en meestal niet gerelateerd aan de belangrijkste zakelijke logica van de app. Enkele van de uitdagingen bij het bouwen van een infrastructuur op verzoek push zijn:

- **Platform afhankelijkheid.** Om meldingen te verzenden naar apparaten op verschillende platforms, moeten meerdere interfaces worden gecodeerd in de back-end. Niet alleen de basisgegevens verschillend zijn, maar de presentatie van de kennisgeving (naast elkaar, toast of badge) ook platformafhankelijke. Deze verschillen kunnen leiden tot back-end-code complex en moeilijk te onderhouden.

- **Schaal.** Schalen van deze infrastructuur heeft twee aspecten:
    + Per PNS-richtlijnen worden apparaat tokens vernieuwd telkens wanneer de toepassing wordt gestart. Dit leidt tot een grote hoeveelheid verkeer (en daaruit voortvloeiende database toegang) alleen op de tokens apparaat up-to-date te houden. Wanneer het aantal apparaten groter wordt (eventueel miljoenen), is de kosten van het maken en onderhouden van deze infrastructuur nonnegligible.

    + De meeste PNSs bieden geen ondersteuning voor broadcast met meerdere apparaten. Hieruit volgt dat een uitzending miljoenen apparaten tot miljoenen aanroepen naar de PNSs leidt. Kunnen deze verzoeken schalen is het zeer lastig, omdat het app-ontwikkelaars wilt u meestal de totale vertraging beperken. Bijvoorbeeld het laatste apparaat in het bericht niet ontvangt de melding 30 minuten na verzending van de kennisgevingen, als voor veel gevallen zou het doel om push-meldingen verslaan.
- **Routering.** PNSs kunt u een bericht verzenden naar een apparaat. Meldingen zijn echter in de meeste apps gericht op gebruikers en/of belangengroepen (bijvoorbeeld alle werknemers die zijn toegewezen aan een bepaalde klant account). Als zodanig, zodat de berichten doorsturen naar de juiste apparaten, app back-end moet ervoor zorgen dat een register dat belangengroepen met tokens apparaat koppelt. Deze belasting wordt toegevoegd aan de totale tijd op de markt en het onderhoud kosten van een app.

##<a name="why-use-notification-hubs"></a>Waarom melding Hubs gebruiken?

Melding Hubs complexiteit te elimineren: u hebt niet de uitdagingen van de push-meldingen. In plaats daarvan kunt u een melding Hub. Melding Hubs met een volledige meerdere platforms, schaalvergroting push notification infrastructuur en aanzienlijk verkorten de push-specifieke code die wordt uitgevoerd in de backend app. Hubs kennisgeving implementeren de functionaliteit van een push-infrastructuur. Apparaten zijn alleen verantwoordelijk voor het registreren van PNS-ingangen en de back-end verantwoordelijk is voor platform-onafhankelijke berichten verzenden naar gebruikers of groepen van belang, zoals in de volgende afbeelding:

![][1]


Melding hubs bieden een kant-en-klare push notification-infrastructuur met de volgende voordelen:

- **Meerdere platforms.**
    +  Ondersteuning voor alle belangrijke mobiele platforms. Melding hubs te push-meldingen verzenden naar de Windows Store, iOS, Android en Windows Phone apps.

    +  Melding hubs biedt een algemene interface om meldingen te verzenden naar alle ondersteunde platforms. Platform-specifieke protocollen zijn niet vereist. App back-end te meldingen verzenden in de indelingen platformspecifieke of platform-onafhankelijke. De toepassing communiceert alleen met melding Hubs.

    +  Ingang van Apparaatbeheer. Melding Hubs onderhoudt het register van de greep en de feedback van de PNSs.

- **Werkt met een back-end**: Cloud of op-lokalen, .NET, PHP, Java, knooppunt, enzovoort.

- **Schaal.** Melding hubs schaal miljoenen apparaten zonder de noodzaak om opnieuw te bouwen of shard.


- **Rich van patronen levering instellen**:

    - *Uitzending*: kunt u in de buurt van gelijktijdige uitzending miljoenen apparaten met een enkele API-aanroep.

    - *Unicast/Multicast*: Push naar codes voor individuele gebruikers, met inbegrip van al hun apparatuur; of een grotere groep; bijvoorbeeld afzonderlijke vormfactoren (tablet of telefoon).

    - *Segmentatie*: Push naar complexe segmenten als gedefinieerd door de tag expressies (bijvoorbeeld apparaten in New York na de Yankees).

    Elk apparaat bij het verzenden van de koppeling met een hub kennisgeving kan een of meer _codes_opgeven. Voor meer informatie over [tags]. Tags hoeft niet vooraf geïnstalleerd of verwijderd. Tags bieden een eenvoudige manier om meldingen te verzenden naar gebruikers of groepen van belang. Omdat tags kunnen bevatten specifieke app-id (zoals een gebruiker of groep-id's), maakt het gebruik app back-end van hoeven opslaan en beheren van apparaat grepen.

- **Personalisatie**: elk apparaat kan een of meer sjablonen voor het bereiken van per apparaat te lokaliseren en persoonlijke instellingen zonder back-end-code hebben.

- **Beveiliging**: gedeelde toegang geheim (SAS) of federatieve verificatie.

- **Rich telemetrie**: beschikbaar in de portal en via programmacode.


##<a name="integration-with-app-service-mobile-apps"></a>Integratie met mobiele Apps App Service

[Mobile Apps in App Service] heeft te vergemakkelijken een naadloze en uniforme ervaring in Azure services, ingebouwde ondersteuning voor push-meldingen met melding Hubs. [Mobile Apps in App Service] biedt een zeer schaalbare, algemeen beschikbaar mobiel development platform voor softwareontwikkelaars en systeemintegrators die een rijke verzameling functies voor ontwikkelaars van mobiele biedt.

Ontwikkelaars van mobiele Apps kunnen melding Hubs gebruiken met de volgende workflow:

1. PNS ingang ophalen
2. Apparaat en [sjablonen] registreren met Hubs melding via handige mobiele Apps Client SDK register-API
    + Houd er rekening mee dat Mobile Apps weg alle tags op registraties voor beveiligingsdoeleinden oprollen. Werken met Hubs melding uit uw back-end rechtstreeks aan tags koppelen aan apparaten.
3. Meldingen van uw app end met Hubs melding verzenden

Hier zijn enkele voordelen voor ontwikkelaars met deze integratie worden gebracht:

- **Mobiele Apps Client SDK's.** Deze multi platform SDK's bieden eenvoudige API's voor registratie en neem contact op met de melding hub automatisch met de mobiele app gekoppeld. Ontwikkelaars hoeft niet te graven via Hubs melding referenties en werken met extra service.
    + De SDK's label automatisch het opgegeven apparaat met mobiele Apps geverifieerde gebruikers-ID doorgeven aan gebruikersscenario inschakelen.
    + De mobiele Apps installatie-ID de SDK's automatisch gebruikt als GUID registreren bij melding Hubs, ontwikkelaars de moeite van het onderhouden van meerdere service-GUID's opslaan.
    
- **Model van de installatie.** Mobiele Apps werkt met de nieuwste push-model melding Hubs voor alle push-eigenschappen die zijn gekoppeld aan een apparaat in een JSON-installatie die wordt uitgelijnd met Push Notification Services en is eenvoudig te gebruiken.

- **Flexibiliteit.** Ontwikkelaars kunt altijd met kennisgeving Hubs rechtstreeks werken zelfs met de integratie in de plaats.

- **Geïntegreerde ervaring in [Azure portal].** Push-als een mogelijkheid visueel in Mobile Apps aangegeven wordt en ontwikkelaars eenvoudig met de bijbehorende melding hub via mobiele Apps werken kunnen.



##<a name="next-steps"></a>Volgende stappen

U vindt meer informatie over de melding Hubs in deze onderwerpen:

+ **[Hoe klanten melding Hubs gebruiken]**

+ **[Melding Hubs zelfstudies en hulplijnen]**

+ **Zelfstudies voor kennisgeving Hubs aan de slag** ([iOS], [Android], [Universal Windows], [Windows Phone], [Kindle], [Xamarin.iOS], [Xamarin.Android])

De relevante .NET managed API-referenties voor push-meldingen, zich hier bevinden:

+ [Microsoft.WindowsAzure.Messaging.NotificationHub]
+ [Microsoft.ServiceBus.Notifications]


  [0]: ./media/notification-hubs-overview/registration-diagram.png
  [1]: ./media/notification-hubs-overview/notification-hub-diagram.png
  [Hoe klanten melding Hubs gebruiken]: http://azure.microsoft.com/services/notification-hubs
  [Melding Hubs zelfstudies en hulplijnen]: http://azure.microsoft.com/documentation/services/notification-hubs
  [iOS]: http://azure.microsoft.com/documentation/articles/notification-hubs-ios-get-started
  [Android]: http://azure.microsoft.com/documentation/articles/notification-hubs-android-get-started
  [Universele Windows]: http://azure.microsoft.com/documentation/articles/notification-hubs-windows-store-dotnet-get-started
  [Windows Phone]: http://azure.microsoft.com/documentation/articles/notification-hubs-windows-phone-get-started
  [Kindle]: http://azure.microsoft.com/documentation/articles/notification-hubs-kindle-get-started
  [Xamarin.iOS]: http://azure.microsoft.com/documentation/articles/partner-xamarin-notification-hubs-ios-get-started
  [Xamarin.Android]: http://azure.microsoft.com/documentation/articles/partner-xamarin-notification-hubs-android-get-started
  [Microsoft.WindowsAzure.Messaging.NotificationHub]: http://msdn.microsoft.com/library/microsoft.windowsazure.messaging.notificationhub.aspx
  [Microsoft.ServiceBus.Notifications]: http://msdn.microsoft.com/library/microsoft.servicebus.notifications.aspx
  [Mobiele Apps van App-Service]: https://azure.microsoft.com/en-us/documentation/articles/app-service-mobile-value-prop/
  [sjablonen]: notification-hubs-templates-cross-platform-push-messages.md
  [Azure portal]: https://portal.azure.com
  [tags]: (http://msdn.microsoft.com/library/azure/dn530749.aspx)
