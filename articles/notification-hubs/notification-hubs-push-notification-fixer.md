<properties 
    pageTitle="Azure melding Hubs - richtlijnen voor de diagnose" 
    description="Richtlijnen voor het opsporen van veelvoorkomende problemen met Azure melding Hubs." 
    services="notification-hubs" 
    documentationCenter="Mobile" 
    authors="ysxu" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="notification-hubs" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="NA" 
    ms.devlang="multiple" 
    ms.topic="article" 
    ms.date="10/03/2016" 
    ms.author="yuaxu"/>

#<a name="azure-notification-hubs---diagnosis-guidelines"></a>Azure melding Hubs - richtlijnen voor de diagnose

##<a name="overview"></a>Overzicht

Een van de meest gestelde vragen die we vaak van klanten Azure melding Hubs horen is hoe om erachter te komen waarom ze niet ziet een melding verzonden vanuit de backend van hun toepassing worden weergegeven op het clientapparaat - waar en waarom meldingen is gegaan en hoe u kunt dit probleem oplossen. In dit artikel doorloopt wij de verschillende redenen waarom berichten neergezet kunnen krijgen of niet terecht op de apparaten. We eruit via manieren kunt u analyseren en de onderliggende oorzaak weten. 

Ten eerste is het belangrijk dat u begrijpt hoe Azure melding Hubs duwt meldingen aan de apparaten.
![][0]

In een melding verzenden normale stroom, wordt het bericht van de **backend van toepassing** op **Azure melding Hub (NH)** die op zijn beurt enkele verwerking op alle registraties rekening houdend met de geconfigureerde tags & tag expressies om te bepalen 'doelen' d.w.z. alle registraties die nodig zijn heeft voor het ontvangen van de push-bericht. Deze registraties kunnen overspannen een of meer van onze ondersteunde platforms - iOS, Google, Windows, Windows Phone, Kindle en Baidu voor Android China. Zodra de doelen zijn vastgesteld, NH en vervolgens dit gereedschap duwt meldingen, splitsen over meerdere batch van registraties voor het apparaatplatform specifieke **Push Notification Service (PNS)** - zoals APNS voor Apple, GCM voor Google etc. NH verifieert met de respectieve PNS op basis van de referenties die u in de klassieke Azure-Portal op de pagina meldingen Hub configureren instelt. De PNS stuurt de kennisgevingen aan de respectieve **clientapparaten**. Dit is de aanbevolen manier voor het leveren van push-meldingen en houd er rekening mee dat de laatste fase van het bericht van levering tussen de PNS-platform en het apparaat plaatsvindt platform. Daarom hebben we vier belangrijke onderdelen - *client*, *toepassing backend*, *Azure melding Hubs (NH)* en *Push Notification Services (PNS)* en een van deze meldingen steeds verbroken kunnen veroorzaken. Meer informatie over deze architectuur is beschikbaar in het [Systeemvak Hubs overzicht].

Om meldingen kan gebeuren tijdens de eerste test/fasering die kan duiden op een probleem met de fase of het kan gebeuren in de productie waarbij alle of sommige van de kennisgevingen vervallen te verloren sommige diepere toepassing die aangeeft of patroon probleem messaging. In de sectie gaan hieronder we verschillende scenario's voor decoratieve meldingen variërend van algemene tot de zeldzame soort, die waarvan sommige voor de hand liggende wellicht en andere niet zo veel. 

##<a name="azure-notifications-hub-mis-configuration"></a>Azure meldingen Hub foutieve configuratie 

Azure melding Hubs moet in het kader van de toepassing van de ontwikkelaar is om meldingen te verzenden naar de respectieve PNS kunnen worden geverifieerd. Dit wordt mogelijk gemaakt door de ontwikkelaar een developer-account maken met het desbetreffende platform (Google, Apple, Windows enz) en vervolgens registreren hun toepassing waar ze referenties die moeten worden geconfigureerd in de portal onder de configuratiesectie Hubs melding krijgen. Als er geen meldingen via, moet eerst zijn om ervoor te zorgen dat de juiste referenties zijn geconfigureerd in de kennisgeving Hub ze overeenkomen met de toepassing gemaakt onder hun specifieke developer platform account. Handig onze [Gestart zelfstudies opvragen] om te gaan over dit proces op een wijze die stap voor stap. Hier volgen enkele algemene foutieve configuraties:

1. **Algemeen**
 
    een) Zorg ervoor dat de naam van uw melding hub (zonder typefouten) is hetzelfde:

    - Waar u de client registreren 
    - Waar u meldingen wilt verzenden vanuit de back-end  
    - Waar u de PNS-referenties hebt geconfigureerd en 
    - Met SAS referenties die u hebt geconfigureerd op de client en de back-end. 
        
    b) Zorg ervoor dat u de juiste tekenreeksen voor SAS-configuratie op de client en de backend van toepassing. Als een vuistregel, moet u de **DefaultListenSharedAccessSignature** op de client- en **DefaultFullSharedAccessSignature** op de backend van toepassing (die wordt gemachtigd om melding te verzenden naar de NH kunnen)

2. **Configuratie van de Apple Push Notification Service (APNS)**
 
    U moet ervoor zorgen dat twee verschillende hubs - een voor de productie en een andere voor het testen van doel. Dit betekent dat het certificaat dat u wilt gebruiken in sandbox-omgeving op een aparte hub en het certificaat dat u wilt gebruiken in de productie op een aparte hub uploaden. Probeer niet te uploaden van verschillende typen certificaten op dezelfde hub als kennisgeving storingen in de regel naar beneden is het mogelijk dat. Als u zelf in een positie waar u per ongeluk verschillende soorten certificaat hebt geüpload op dezelfde hub vindt, is het raadzaam de hub verwijderen en opnieuw beginnen. Als voor een of andere reden u niet kunt verwijderen van de hub vervolgens tenminste, moet u de bestaande registraties van de hub. 

3. **Configuratie van Google Cloud Messaging (GCM)** 

    een) Zorg ervoor dat u 'Google Cloud Messaging voor Android' onder de wolk-project inschakelen wilt. 
    
    ![][2]
    
    b) Zorg ervoor dat u een sleutel"Server" maakt, terwijl het verkrijgen van de referenties die NH wordt gebruikt voor verificatie bij GCM. 
    
    ![][3]
    
    c) Zorg ervoor dat u 'Project-ID' hebt geconfigureerd op de client is een volledig numerieke entiteit die u van het dashboard ophalen kunt:
    
    ![][1]

##<a name="application-issues"></a>Problemen met toepassingen

1) **Tags / labels expressies**

Als u labels of tag expressies segmenten van uw publiek, is het altijd mogelijk dat wanneer u het bericht verzendt, er is geen doel op basis van de tags/tag expressies die u in uw oproep verzenden opgeeft kan worden gevonden. Het is raadzaam uw registraties om ervoor te zorgen dat er labels die overeenkomen met zijn bij het verzenden van meldingen en controleer vervolgens of het ontvangstbewijs van de clients met deze registraties alleen bekijken. Bv. Als al uw registraties met NH zijn gemaakt met label "Politiek" zegt en u een bericht met label verzendt "Sports", het niet verzonden naar elk apparaat. Een complexe zaak kan betrekking hebben op tag expressies waar u alleen geregistreerd met "A Tag" of "B-code", maar tijdens het verzenden van meldingen, u "Label A & & Tag B" zijn gericht. In de onderstaande sectie tips zelf-diagnose zijn er manieren waarin u uw registraties met de tags hebben kunt bekijken. 

2) **Sjabloon kwesties**

Als u met behulp van sjablonen en ervoor te dat zorgen volgt u de richtlijnen aan de [richtlijnen van de sjabloon]beschreven. 

3) **Ongeldige registraties**

Ervan uitgaande dat de melding Hub correct is geconfigureerd en eventuele expressies tags/code werden gebruikt goed waardoor het zoeken naar geldige doelen waarnaar de kennisgeving moeten worden verzonden, NH wordt gestart uit verschillende batches voor verwerking parallel - elke batch berichten verzenden naar een set van registraties. 

> [AZURE.NOTE] Omdat we in een parallelle verwerking doen, garanderen niet wij dat de volgorde waarin de berichten worden bezorgd. 

Azure meldingen Hub is nu geoptimaliseerd voor een model voor "eenmaal op de meeste" bericht. Dit betekent dat we proberen de dubbel zodat er geen meldingen meer dan eens aan een apparaat worden geleverd. Om dit te waarborgen we zoeken via de registraties en zorg ervoor dat slechts één bericht per apparaat-id voordat daadwerkelijk het bericht verzonden naar de PNS. Zoals elke batch wordt verzonden naar de PNS, die op zijn beurt accepteert en valideren van de registraties, is het mogelijk dat de PNS detecteert een fout met een of meer van de registraties in een batch, wordt een fout geretourneerd naar Azure NH en verwerking weghalen die partij daarmee volledig gestopt. Dit geldt vooral met APNS die een stream TCP-protocol wordt gebruikt. Hoewel we zijn geoptimaliseerd voor op de meeste zodra de levering, in dit geval moeten we de ontlast registratie verwijderen uit onze database en probeer het bericht van levering voor de rest van de apparaten in die partij.

Krijgt u informatie over fouten bij de poging mislukte bezorging tegen een registratie met behulp van de API's van Azure melding Hubs REST: [Per bericht telemetrie: krijg melding bericht telemetrie](https://msdn.microsoft.com/library/azure/mt608135.aspx) en [PNS Feedback](https://msdn.microsoft.com/library/azure/mt705560.aspx). Zie [SendRESTExample](https://github.com/Azure/azure-notificationhubs-samples/tree/master/dotnet/SendRestExample) voor de code.

##<a name="pns-issues"></a>PNS problemen

Zodra het bericht is ontvangen door de respectieve PNS is haar verantwoordelijkheid voor het leveren van de kennisgeving aan het apparaat. Azure melding Hubs niet in de afbeelding hier en heeft geen controle over wanneer, of als de melding gaat naar het apparaat worden geleverd. Omdat het platform notification services redelijk robuust zijn, vaak meldingen de apparaten bereiken in een paar seconden van de PNS. Als de PNS beperken echter wordt vervolgens Azure melding Hubs geldt een exponentiële terug uit de strategie en als de PNS onbereikbaar voor 30 min blijft moet we hebben een beleid te laten verlopen en deze berichten permanent verwijderen. 

Als een PNS probeert een melding, maar het apparaat offline is, is de melding voor een beperkte periode door de PNS opgeslagen en geleverd aan het apparaat wanneer deze beschikbaar is. Slechts één recente melding voor een bepaalde toepassing is opgeslagen. Als er meerdere meldingen worden verzonden als het apparaat offline is, wordt elke nieuwe kennisgeving van de voorafgaande kennisgeving kunnen worden verwijderd. Dit gedrag om alleen de meest recente melding is aanwezig meldingen in APNS- en samenvouwen in GCM (die gebruikmaakt van een sleutel samenvouwen) genoemd. Als het apparaat gedurende langere tijd off line blijft, worden alle berichten die zijn opgeslagen voor deze verwijderd. Bron - [richtlijnen voor APNS] & [GCM-richtlijnen]

Met Azure melding Hubs - kunt u een sleutel voor samenvoegen doorgeven via een HTTP-header met de algemene `SendNotification` API (bv. voor .NET SDK: `SendNotificationAsync`) die neemt ook HTTP-headers die worden doorgegeven als de respectieve PNS is. 

##<a name="self-diagnose-tips"></a>Tips voor zelf-diagnose

Hier bespreken we de verschillende mogelijkheden voor het opsporen en de hoofd-Hub melding problemen veroorzaken:

###<a name="verify-credentials"></a>Referenties controleren

1. **PNS developer-portal**

    Controleer of ze op de respectieve PNS developer-portal (APNS, GCM, WNS enz) met behulp van onze [Gestart zelfstudies opvragen].

2. **Azure klassieke portal**

    Ga naar het tabblad configureren om te controleren en overeenkomen met de referenties die worden verkregen uit de PNS developer-portal. 

    ![][4]

###<a name="verify-registrations"></a>Controleer of registraties

1. **Visual Studio**

    Als u Visual Studio voor ontwikkeling kunt u verbinding maken met Microsoft Azure en weergeven en beheren van een aantal Azure services, met inbegrip van de Hub van meldingen van 'Server Explorer'. Dit is vooral handig als uw dev/testomgeving. 

    ![][9]

    U kunt weergeven en beheren van alle inschrijvingen in de hub die netjes gecategoriseerd voor platform native of sjabloon registratie, labels, PNS-id, registratie-id en de vervaldatum. Ook kunt u een registratie op elk gewenst moment - nuttig zeggen als u wilt bewerken van labels. 

    ![][8]
 
    > [AZURE.NOTE] Visual Studio-functionaliteit voor het bewerken van registraties mag alleen worden gebruikt tijdens dev/test met een beperkt aantal registraties. Als er behoefte aan uw registraties in bulk te corrigeren ontstaat, kunt u met de functie exporteren/importeren registratie die hier worden beschreven - [Registraties exporteren/importeren] (https://msdn.microsoft.com/library/dn790624.aspx)

2. **Explorer Service Bus**

    Veel klanten gebruiken ServiceBus explorer hier - [ServiceBus Explorer] voor het weergeven en beheren van hun hub kennisgeving beschreven. Het is een open-source project van code.microsoft.com - [code ServiceBus Explorer]

###<a name="verify-message-notifications"></a>Controleer of de melding

1. **Azure klassieke Portal**

    Gaat u naar het tabblad "Debug" test om meldingen te verzenden naar clients zonder die behoefte hebben aan een service backend omhoog en uitgevoerd. 

    ![][7]

2. **Visual Studio**

    U kunt ook test meldingen verzenden vanuit de comforts van Visual Studio:

    ![][10]

    U kunt meer lezen over de Visual Studio melding Hub Azure explorer functionaliteit hier - 
    
    - [VS Server Explorer-overzicht]
    - [VS Server Explorer blogbericht - 1]
    - [VS Server Explorer blogbericht - 2]

###<a name="debug-failed-notifications-review-notification-outcome"></a>Debug mislukte meldingen / melding resultaten bekijken

**EnableTestSend, eigenschap**

Bij het verzenden van een melding via Hubs melding in eerste instantie het net wordt in de wachtrij geplaatst voor NH te doen voor het berekenen van alle doelen worden verwerkt en vervolgens uiteindelijk NH verzendt naar de PNS. Dit betekent dat bij het gebruik van REST API of een van de client-SDK de succesvolle terugkeer van uw oproep verzenden alleen betekent dat het bericht zijn succes in de wachtrij met melding Hub geplaatst heeft. Geeft een beter inzicht in wat er is gebeurd bij NH uiteindelijk kreeg het bericht te verzenden naar de PNS geen. Als uw melding is niet die op het clientapparaat, bestaat de kans bij NH geprobeerd het bericht te bezorgen aan de PNS, er is een fout die bijvoorbeeld de grootte van de nettolading langer dan toegestaan door de PNS of de referenties die zijn geconfigureerd in NH ongeldig enz zijn. Als u een beter inzicht in de PNS fouten, hebben we een eigenschap met de naam van [de functie EnableTestSend]geïntroduceerd. Deze eigenschap wordt automatisch ingeschakeld wanneer u testberichten vanuit de portal of Visual Studio verzenden en daarom kunt u gedetailleerde informatie over foutopsporing. U kunt dit via API's het voorbeeld van de SDK voor .NET waarin deze nu beschikbaar is en wordt toegevoegd aan alle client-SDK's uiteindelijk. Om dit te gebruiken bij het aanroepen van de REST, toe te voegen een querystring-parameter de naam 'test' aan het einde van uw oproep verzenden bv. 

    https://mynamespace.servicebus.windows.net/mynotificationhub/messages?api-version=2013-10&test

*Voorbeeld van de .NET SDK)*
 
Stel dat u een eigen toast melding verzenden via .NET SDK:

    NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString(connString, hubName);
    var result = await hub.SendWindowsNativeNotificationAsync(toast);
    Console.WriteLine(result.State);
 
`result.State`wordt gewoon staat `Enqueued` aan het einde van de uitvoering zonder een inzicht in wat is er gebeurd met de push. Nu kunt u de `EnableTestSend` Booleaanse eigenschap tijdens het initialiseren van de `NotificationHubClient` en gedetailleerde status over de PNS fouten opgetreden bij het verzenden van het bericht kunt krijgen. De send-aanroep gaat meer tijd om terug te keren, omdat deze alleen is terug na de levering van NH heeft de kennisgeving aan de PNS de uitkomst te achterhalen. 
 
    bool enableTestSend = true;
    NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString(connString, hubName, enableTestSend);
    
    var outcome = await hub.SendWindowsNativeNotificationAsync(toast);
    Console.WriteLine(outcome.State);
    
    foreach (RegistrationResult result in outcome.Results)
    {
        Console.WriteLine(result.ApplicationPlatform + "\n" + result.RegistrationId + "\n" + result.Outcome);
    }

*Voorbeeld van de uitvoer*

    DetailedStateAvailable
    windows
    7619785862101227384-7840974832647865618-3
    The Token obtained from the Token Provider is wrong
 
Dit bericht geeft aan dat een ongeldige referenties zijn geconfigureerd in de kennisgeving hub of een probleem met de registraties op de hub en de aanbevolen cursus zou deze registratie verwijderen en kunt de client opnieuw maken voordat u het bericht verzendt. 
 
> [AZURE.NOTE] Houd er rekening mee dat het gebruik van deze eigenschap is sterk verminderd en dus u alleen dit in dev/testomgeving met beperkt aantal registraties gebruiken moet. Wij alleen verzenden debug meldingen tot 10 apparaten. We hebben ook een limiet van foutopsporing verzendt 10 per minuut wordt verwerkt. 

###<a name="review-telemetry"></a>Telemetrie bekijken 

1. **Azure klassieke Portal gebruiken**

    De portal kunt u snel een overzicht van alle activiteiten op uw Hub-melding krijgen. 
    
    een) u kunt een samengevoegde weergave van de registraties, meldingen, evenals fouten per platform weergeven via het tabblad 'dashboard'. 
    
    ![][5]
    
    b) u kunt ook veel andere platform specifieke parameters toevoegen op het tabblad 'Monitor' diepere bekijken met name aan de specifieke fouten geretourneerd als NH probeert het bericht te verzenden naar de PNS PNS. 
    
    ![][6]
    
    c) u moet beginnen met de beoordeling van de **Binnenkomende berichten**, **Registreren**, **Geslaagde meldingen** en gaat u naar per tabblad platform de PNS specifieke fouten bekijken. 
    
    d) als u de melding hub onjuist is geconfigureerd met de verificatie-instellingen ziet u PNS verificatiefout. Dit is een goede indicatie om te controleren de PNS-referenties. 

2) **Toegang op programmeerniveau**

Meer details hier- 

- [Telemetrie programmatische toegang]
- [Telemetrie-toegang via API's monster] 

> [AZURE.NOTE] Telemetrie verschillende gerelateerde functies zoals **Export/Import registraties**, **Telemetrie toegang via API's** etc zijn alleen beschikbaar in standaard laag. Als u deze functies gebruiken als vrije of Basic tier probeert vervolgens krijgt u uitzonderingsbericht daartoe tijdens het gebruik van de SDK en een HTTP 403 (niet toegestaan) wanneer u ze rechtstreeks vanuit de REST API's. Zorg ervoor dat u hebt verplaatst, trapsgewijs via Azure klassieke Portal norm voldoen.  

<!-- IMAGES -->
[0]: ./media/notification-hubs-diagnosing/Architecture.png
[1]: ./media/notification-hubs-diagnosing/GCMConfigure.png
[2]: ./media/notification-hubs-diagnosing/GCMEnable.png
[3]: ./media/notification-hubs-diagnosing/GCMServerKey.png
[4]: ./media/notification-hubs-diagnosing/PortalConfigure.png
[5]: ./media/notification-hubs-diagnosing/PortalDashboard.png
[6]: ./media/notification-hubs-diagnosing/PortalMonitoring.png
[7]: ./media/notification-hubs-diagnosing/PortalTestNotification.png
[8]: ./media/notification-hubs-diagnosing/VSRegistrations.png
[9]: ./media/notification-hubs-diagnosing/VSServerExplorer.png
[10]: ./media/notification-hubs-diagnosing/VSTestNotification.png
 
<!-- LINKS -->
[Melding Hubs-overzicht]: notification-hubs-push-notification-overview.md
[Zelfstudie aan de slag]: notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md
[Richtlijnen voor sjabloon]: https://msdn.microsoft.com/library/dn530748.aspx 
[Richtlijnen voor APNS]: https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html#//apple_ref/doc/uid/TP40008194-CH100-SW4
[GCM-richtlijnen]: http://developer.android.com/google/gcm/adv.html
[Rapporten exporteren/importeren]: http://msdn.microsoft.com/library/dn790624.aspx
[ServiceBus Explorer]: http://msdn.microsoft.com/library/dn530751.aspx
[Code ServiceBus Explorer]: https://code.msdn.microsoft.com/windowsazure/Service-Bus-Explorer-f2abca5a
[VS Server Explorer-overzicht]: http://msdn.microsoft.com/library/windows/apps/xaml/dn792122.aspx 
[VS Server Explorer blogbericht - 1]: http://azure.microsoft.com/blog/2014/04/09/deep-dive-visual-studio-2013-update-2-rc-and-azure-sdk-2-3/#NotificationHubs 
[VS Server Explorer blogbericht - 2]: http://azure.microsoft.com/blog/2014/08/04/announcing-release-of-visual-studio-2013-update-3-and-azure-sdk-2-4/ 
[EnableTestSend, functie]: http://msdn.microsoft.com/library/microsoft.servicebus.notifications.notificationhubclient.enabletestsend.aspx
[Telemetrie programmatische toegang]: http://msdn.microsoft.com/library/azure/dn458823.aspx
[Telemetrie-toegang via API's monster]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/FetchNHTelemetryInExcel

 