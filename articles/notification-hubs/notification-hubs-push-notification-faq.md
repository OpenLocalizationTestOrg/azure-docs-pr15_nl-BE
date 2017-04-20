<properties
    pageTitle="Azure melding Hubs - Veelgestelde vragen (FAQ's)"
    description="Veelgestelde vragen op ontwerpen/implementeren van oplossingen op melding Hubs"
    services="notification-hubs"
    documentationCenter="mobile"
    authors="ysxu"
    manager="erikre"
    keywords="push-bericht, push-meldingen, push-meldingen voor iOS, android push-meldingen, push ios, android push"
    editor="" />

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-multiple"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="yuaxu" />

#<a name="push-notifications-with-azure-notification-hubs---frequently-asked-questions"></a>Push-meldingen met Azure melding Hubs - Veelgestelde vragen

##<a name="general"></a>Algemeen
###<a name="1---what-is-the-price-model-for-notification-hubs"></a>1. Wat is de prijsmodel voor kennisgeving Hubs?
Hubs kennisgeving wordt aangeboden in drie lagen:

* **Vrije** - ladingen per abonnement per maand tot 1 miljoen ophalen.
* **Basic** - get 10 miljoen dit gereedschap duwt per abonnement per maand als basis, met quota groei-opties.
* **Standaard** - ladingen per abonnement per maand als een basislijn 10 miljoen ophalen, verhogen met quota-opties, plus uitgebreide telemetrie capabilties.

De meest recente details vindt u op de pagina [Meldingen Hubs prijzen] . De prijzen op het niveau van het abonnement tot stand is gebracht en is gebaseerd op het aantal berichtuitwisselingen voor push-meldingen, dus het maakt niet uit hoeveel naamruimten of kennisgeving hubs die u hebt gemaakt in uw abonnement Azure.

**Vrij** laag wordt voor ontwikkeling doeleinden met geen SLA garantie aangeboden. Tijdens deze fase een goed uitgangspunt voor degenen die willen verkennen van de mogelijkheden van push-meldingen via Azure melding Hubs zijn kan, kan het niet de beste keuze voor middelgrote tot grote schaal toepassingen zijn.

**Basic** & **standaard** niveaus worden aangeboden voor productie-gebruik met de volgende kenmerken ingeschakeld *voor het standaard alleen trapsgewijs*:

- *Telemetrie RTF* - bericht Hubs bieden een aantal mogelijkheden om uw telemetriegegevens exporteren als push-melding registratie-informatie voor off line weergave en analyse.
- *Meerdere pacht* - ideaal als u een mobiele app met Hubs kennisgeving ter ondersteuning van meerdere huurders maakt. Hiermee kunt u de referenties van de Push Notification Services (PNS) instellen op het niveau van de naamruimte melding Hub voor de app en vervolgens kunt u de afzonderlijke hubs onder deze gemeenschappelijke naamruimte meeleveren huurder scheiden. Hiermee kunt eenvoudig onderhoud terwijl te houden van de sleutels voor SA's gescheiden om push-meldingen ontvangen van de kennisgeving hubs & verzenden voor elke huurder niet cross-huurder overlapping te waarborgen.
- *Gepland Push* - kunt u plannen push-meldingen die wordt vervolgens in de wachtrij geplaatst en wordt verzonden.
- *Bulkimport* - kunt u registraties in bulk importeren.

###<a name="2---what-is-the-notification-hubs-sla"></a>2. Wat is de melding Hubs SLA?
Voor **Basic** en **Standard** melding Hubs lagen garanderen wij dat ten minste 99,9% van de tijd, correct geconfigureerde toepassingen zal kunnen push-meldingen verzenden of bewerkingen registratie van beheer van een kennisgeving Hub geïmplementeerd in een ondersteunde laag. Meer informatie over onze SLA, gaat u naar de pagina [Melding Hubs SLA] .

> [AZURE.NOTE] Er zijn geen SLA garanties voor de arm tussen de meldingenservice Platform en het apparaat na kennisgeving Hubs afhankelijk van externe platform providers zijn leveren de push-bericht naar het apparaat.

###<a name="3---which-customers-are-using-notification-hubs"></a>3. welke klanten Hubs Notification gebruikt?
We hebben een groot aantal klanten die gebruikmaken van kennisgeving Hubs met een paar opvallende onderstaande:

* Sochi 2014 – 100s van belangengroepen, 3 miljoen apparaten, 150 miljoen kennisgeving verzonden in 2 weken. [CaseStudy - Sochi]
* Skanska - [CaseStudy - Skanska]
* Seattle Times - [CaseStudy - Seattle Times]
* Mural.LY - [CaseStudy - Mural.ly]
* 7Digital - [CaseStudy - 7Digital]
* Bing Apps – 10s miljoenen apparaten voor het verzenden van meldingen 3 miljoen per dag.

###<a name="4-how-do-i-upgrade-or-downgrade-my-notification-hubs-to-change-my-service-tier"></a>4. hoe ik upgraden of downgraden mijn melding Hubs om mijn tier service wijzigen?
Ga naar de [Klassieke Portal Azure]Service Bus en klikt u op in de naamruimte vervolgens uw hub-melding. Klik op het tabblad schaal zijn u kunt uw service melding Hubs laag wijzigen.

![](./media/notification-hubs-faq/notification-hubs-classic-portal-scale.png)

##<a name="design--development"></a>Ontwerp & ontwikkeling
###<a name="1---which-server-side-platforms-do-you-support"></a>1. welke server-side-platforms ondersteund?
Wij bieden SDK's en [volledige monsters] voor .NET, Java, PHP, Python, Node.js zodat een backend app kan worden ingesteld om te communiceren met melding Hubs met een van deze platforms. Melding Hubs API's zijn gebaseerd op REST interfaces, zodat u rechtstreeks contact opnemen met die in plaats daarvan als u niet wilt toevoegen van een extra afhankelijkheid kunt kiezen. Meer informatie vindt u op de pagina [NH - REST API's] .

###<a name="2---which-client-platforms-do-you-support"></a>2. welke clientplatforms ondersteund?
We ondersteunen push-meldingen verzenden naar [Apple iOS](notification-hubs-ios-apple-push-notification-apns-get-started.md), [Android](notification-hubs-android-push-notification-google-gcm-get-started.md), [Universele Windows](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md), [Windows Phone](notification-hubs-windows-mobile-push-notifications-mpns.md), [Kindle](notification-hubs-kindle-amazon-adm-push-notification.md), [Android China (via Baidu)](notification-hubs-baidu-china-android-notifications-get-started.md), Xamarin ([iOS](xamarin-notification-hubs-ios-push-notification-apns-get-started.md) & [Android](xamarin-notification-hubs-push-notifications-android-gcm.md)), [Apps in Chrome](notification-hubs-chrome-push-notifications-get-started.md) en [Safari](https://github.com/Azure/azure-notificationhubs-samples/tree/master/PushToSafari) -platforms. Voor een volledige lijst met gestarte zelfstudies aanpakken van push-meldingen verzenden op deze platforms ophalen, gaat u naar onze pagina [NH - slag zelfstudies opvragen] .

###<a name="3---do-you-support-smsemailweb-notifications"></a>3. ondersteund meldingen E-mail-SMS-website?
Melding Hubs is vooral bedoeld om meldingen te verzenden naar mobiele toepassingen met behulp van de hierboven vermelde platforms. Wij nog bieden niet de mogelijkheid voor het verzenden van e-mail of SMS-berichten; platforms van derden die deze mogelijkheden bieden kunnen echter met Hubs native push-meldingen verzenden met [Azure Mobile Apps]meldingen worden geïntegreerd.

Melding Hubs beschikken niet over een browser push notification levering service out-of-the-box. Klanten kunnen kiezen voor dit implementeren met behulp van SignalR op de ondersteunde platforms voor server-side. Raadpleeg de [zelfstudie chroom Apps]als u op zoek bent om meldingen te verzenden naar apps in de sandbox Chrome browser.

###<a name="4---what-is-the-relation-between-azure-mobile-apps-and-azure-notification-hubs-and-when-do-i-use-what"></a>4. Wat is de relatie tussen Azure mobiele Apps en Azure melding Hubs en wanneer gebruik ik wat?
Als u een bestaande back-end voor mobiele app hebt en wilt u alleen de mogelijkheid om push-meldingen te verzenden toevoegen kunt u Azure melding Hubs. Als u wilt instellen uw backend mobiele app helemaal moet vervolgens u overwegen Azure Mobile Apps. Een Azure Mobile App bepalingen automatisch een melding Hub u push-meldingen eenvoudig verzenden van de mobiele app-end. Prijzen voor Azure Mobile Apps inclusief de basis voor een Hub kennisgeving en u betaalt alleen wanneer u verder gaan dan dit gereedschap duwt de opgenomen. Meer informatie over de kosten zijn beschikbaar op de pagina [App serviceprijzen] .

###<a name="5---how-many-devices-can-i-support-if-i-send-push-notifications-via-notification-hubs"></a>5. hoeveel apparaten kan ik ondersteunen als ik push-meldingen via Hubs melding verzenden
Raadpleeg de [Melding Hubs prijzen] pagina voor meer informatie over het aantal ondersteunde apparaten.

Voor bepaalde scenario's, als u ondersteuning nodig hebt voor meer dan 10.000.000 geregistreerde apparaten, Neem direct [contact met ons op](https://azure.microsoft.com/overview/contact-us/) en wij kunt u uw oplossing geschaald.

###<a name="6---how-many-push-notifications-can-i-send-out"></a>6. hoeveel push-meldingen kunnen ik verzenden?
Afhankelijk van de geselecteerde laag Azure automatisch aangepast om op basis van het aantal meldingen die door het systeem.

>[AZURE.NOTE] De totale gebruikskosten kan stijgen op basis van het aantal push-meldingen worden verzonden. Zorg ervoor dat u zich bewust zijn van de bestaande laag limieten die worden beschreven op de pagina [Meldingen Hubs prijzen] .

Onze bestaande klanten zijn met behulp van kennisgeving Hubs dagelijks miljoenen push-meldingen verzenden. U hoeft niet speciaal voor het schalen van uw push meldingen bereiken als u van Azure melding Hubs gebruikmaakt niets te doen.

###<a name="7---how-long-does-it-take-for-sent-push-notifications-to-reach-my-device"></a>7. hoe lang duurt het voor verzonden push-meldingen te bereiken mijn apparaat?
Azure melding Hubs is kunnen verwerken ten minste **1 miljoen push-bericht verzendt een minuut** in een gewone scenario waar de binnenkomende belasting redelijk consistent is en wordt niet in de natuur spikey gebruiken. Dit percentage kan variëren afhankelijk van het aantal labels, aard van de binnenkomende verzendt en andere externe factoren.

De service tijdens de geschatte levertijd is kunnen de doelen per platform en de route berichten naar de respectieve push melding delivery services op basis van de geregistreerde tags/tag expressies berekenen. Het is de verantwoordelijkheid van de diensten van de Push-meldingen (PNS) voor het verzenden van de kennisgeving aan het apparaat hier op.

Een PNS is een SLA geen garantie voor het bezorgen van berichten; echter meestal een overgrote meerderheid van de push-meldingen worden geleverd met doelapparaten binnen enkele minuten (meestal binnen de grenzen van 10 minuten) vanaf het moment dat ze worden verzonden naar ons platform. Er zijn een paar uitschieters die meer tijd in beslag kunnen nemen.

>[AZURE.NOTE] Azure melding Hubs heeft een beleid om alle push-meldingen die kunnen worden bezorgd bij de PNS in 30 minuten niet weghalen. Deze vertraging kan gebeuren om een aantal redenen, meest vaak omdat de PNS is de toepassing beperken.

###<a name="8---is-there-any-latency-guarantee"></a>8. is er een garantie latentie?
Vanwege de aard van het push-meldingen (ze worden geleverd door een externe, platform-specifieke Push Notification Service), is er geen garantie latentie. Het merendeel van de push-meldingen verzonden meestal binnen een paar minuten.

###<a name="9---what-are-the-considerations-i-need-to-take-into-account-when-designing-a-solution-with-namespaces-and-notification-hubs"></a>9. Wat zijn de overwegingen ik rekening houden moet bij het ontwerpen van een oplossing met naamruimten en Hubs melding?

####<a name="mobile-appenvironment"></a>Mobiele App/omgeving

* Er moet één melding Hub per mobiele app, per omgeving.
* In een scenario met meerdere huurder moet elke huurder een afzonderlijke hub.
* Als dit mogelijk de regel naar beneden tijdens het verzenden van berichten, moet u nooit dezelfde melding Hub tussen testomgeving als operationele omgeving delen. Apple biedt bijvoorbeeld Sandbox en Push productie eindpunten met elk met afzonderlijke referenties.
* U kunt standaard test meldingen verzenden naar uw geregistreerde apparaten via de Portal Azure of de Azure geïntegreerd onderdeel van Visual Studio. De drempel is ingesteld op 10-apparaten die willekeurig worden geselecteerd uit de registratie van toepassingen.

>[AZURE.NOTE] Als de hub was oorspronkelijk met een Apple sandbox-certificaat is geconfigureerd en vervolgens opnieuw geconfigureerd voor het gebruik van een productiecertificaat Apple, zou het oude apparaat tokens met het nieuwe certificaat ongeldig en ervoor zorgen dat dit gereedschap duwt mislukken. Het is raadzaam om te scheiden van de productie en testen omgevingen en andere hubs gebruiken voor verschillende omgevingen.

####<a name="pns-credentials"></a>PNS-referenties

Wanneer een mobiele app is geregistreerd bij een platform van developer-portal (zoals Apple of Google enz) vervolgens krijgt u een app-id en security tokens die een backend app van het Platform Push Notification services moet om push-meldingen te verzenden naar de apparaten te kunnen bieden. Deze beveiligingstokens die in de vorm van certificaten (bv. voor Apple iOS of Windows Phone) of beveiligingssleutels (Google Android, Windows, enzovoort worden kunnen) moeten worden geconfigureerd in de kennisgeving Hubs. Meestal op het niveau van de hub kennisgeving is gedaan, maar ook op het niveau van de naamruimte in een scenario met meerdere huurder kan worden uitgevoerd.

####<a name="namespaces"></a>Naamruimten

Naamruimten kunnen worden gebruikt voor het groeperen van implementatie.  Het kan ook voor alle meldingen Hubs voor alle huurders van de in het scenario met meerdere huurder dezelfde app gebruikt.

####<a name="geo-distribution"></a>Geo-verdeling

Geo-verdeling is niet altijd kritiek in push notification-scenario's. Het is om te worden opgemerkt dat verschillende Push Notification Services (bijv. APNS, GCM enz), die uiteindelijk de push-meldingen bezorgen aan de apparaten, niet gelijkmatig worden verdeeld hetzij.

Als u een toepassing die over de hele wereld wordt gebruikt, kunt u meerdere hubs in verschillende naamruimten profiteren van de beschikbaarheid van de service melding Hubs in verschillende Azure gebieden over de hele wereld.

>[AZURE.NOTE] Hierdoor neemt de beheerkosten - met name rond registraties, zodat dit echt wordt niet aanbevolen en moet alleen worden uitgevoerd als er een expliciete behoefte.

###<a name="10--should-i-do-registrations-from-the-app-backend-or-directly-through-client-devices"></a>10. moet ik doen registraties van de app-end of rechtstreeks via de client apparaten?
Registraties van de backend app zijn handig wanneer u doen moet voordat u de registratie van verificatie van de client of wanneer u tags die moeten worden gemaakt of gewijzigd door de app end op basis van de logica van sommige app. Voor meer informatie kunt u meer lezen in de [Backend registratie richtlijnen] en [richtlijnen voor registratie van de back-end - 2] pagina's.

###<a name="11--what-is-the-push-notification-delivery-security-model"></a>11. Wat is het beveiligingsmodel voor levering van push melding?
Azure melding Hubs gebruiken een [Gedeeld Access handtekening (SAS)](../storage/storage-dotnet-shared-access-signature-part-1.md)-beveiligingsmodel gebaseerd. U kunt de tokens SAS in de hoofdmap van de naamruimte of de gedetailleerde kennisgeving Hubs. Deze tokens SAS kunt u instellen met verschillende verificatieregels bijvoorbeeld machtigingen voor verzenden van berichten, luister melding machtigingen enz. Meer details zijn beschikbaar in het document [NH beveiligingsmodel] .

###<a name="12--how-should-i-handle-sensitive-payload-in-the-push-notifications"></a>12. Hoe moet ik omgaan met gevoelige nettolading in de push-meldingen?
Alle berichten worden afgeleverd aan doelapparaten door het platform van Push Notification Services (PNS). Als een afzender een bericht naar Azure melding Hubs verzendt vervolgens we verwerken en doorgeven van de kennisgeving aan de respectieve PNS.

Alle verbindingen van de afzender naar de meldingen Azure Hubs en vervolgens naar de PNS HTTPS gebruiken.

>[AZURE.NOTE] Azure meldingen Hubs wordt niet geregistreerd voor de nettolading van het bericht op geen enkele manier.

Voor het verzenden van gevoelige payloads echter raadzaam een Push-beveiligde patroon waarbij de afzender 'ping' bericht met bericht-id naar het apparaat zonder de gevoelige nettolading en wanneer de app op het apparaat deze payload ontvangt biedt, is het kunnen een veilige API om het te halen de berichtgegevens aan te roepen. Een gids over het implementeren van de hierboven beschreven patroon is beschikbaar op de pagina [NH - Secure Push-zelfstudie] .

##<a name="operations"></a>Bewerkingen
###<a name="1---what-is-the-disaster-recovery-dr-story"></a>1. Wat is het verhaal Disaster Recovery (DR)?
Wij bieden metagegevens Disaster Recovery dekking aan onze kant (melding Hub naam verbindingsreeks en andere gevoelige informatie). Wanneer een DR scenario wordt geactiveerd, is de gegevens van de registraties van het **alleen segment** van de infrastructuur voor kennisgeving Hubs die verloren gaan. Moet u voor de implementatie van een oplossing voor om deze gegevens opnieuw in te vullen in uw nieuwe hub na herstel.

- *Stap 1* - secundaire Hub melding maken in een ander datacenter. Kunt u dit op elk gewenst moment op het tijdstip van de gebeurtenis DR of kunt u een van de get gaan. Het maakt niet veel verschil welke optie u kiest, omdat de melding Hub inrichten is een relatief snel proces in de volgorde van een paar seconden. Met een vanaf het begin wordt voorkomt dat u aan de gebeurtenis DR beïnvloeden uw mogelijkheden voor beheer, zodat u de optie ten zeerste aanbevolen.

- *Stap 2* - de secundaire Hub kennisgeving de registraties van de primaire Hub voor kennisgeving hydraat. Het is niet aanbevolen om registraties op beide hubs onderhouden en proberen ze om synchroon te houden onderweg als registraties afkomstig zijn in de - meestal die nog niet goed vanwege de inherente tendens van registraties verlopen gewerkt aan de kant van de PNS. Hubs melding opschonen ze als we PNS feedback over registraties verlopen of ongeldig ontvangen.  

Aanbeveling is het gebruik van een app backend waarvan:

- Een bepaalde set van registraties aan het einde wordt bijgehouden zodat deze een massaal invoegen in de kennisgeving voor secundaire hub bij DR kunt doen

**OR**

- Haalt een gewone dumpen van registraties van de primaire hub als een back-up en heeft een massa plaats in de secundaire NH.

>[AZURE.NOTE] Registraties Export/Import functionaliteit beschikbaar in standaard laag wordt beschreven in het document [Exporteren/importeren van registraties] .

Als er geen een backend, vervolgens wanneer de toepassing wordt gestart op een van de doelapparaten, zal zij een nieuwe registratie uitvoeren in de secundaire Hub voor kennisgeving en uiteindelijk de secundaire Hub melding hebben alle actieve hulpmiddelen die zijn geregistreerd.

Het nadeel is dat er wordt een periode wanneer apparaten waar apps nog niet geopend kunnen geen meldingen ontvangen.

###<a name="2---is-there-any-audit-log-capability"></a>2. is er een audit log mogelijkheden?
Alle Hubs berichtgevingsbeheer bewerkingen Ga naar Logboeken die beschikbaar worden gemaakt in de [Klassieke Azure-Portal].

##<a name="monitoring--troubleshooting"></a>Controle en probleemoplossing
###<a name="1---what-troubleshooting-capabilities-are-available"></a>1. welke probleemoplossing zijn beschikbaar?
Azure melding Hubs bieden diverse functies die u veelvoorkomende problemen oplossen, met name in de meeste gevallen rond de genegeerde meldingen doen. Zie voor meer informatie onze [NH - probleemoplossing] white paper.

###<a name="2---what-telemetry-features-are-available"></a>2. welke telemetrie functies zijn beschikbaar?
Azure melding Hubs kunnen telemetriegegevens bekijken in de [Klassieke Azure-Portal]. Details van de beschikbare statistieken zijn beschikbaar op de pagina [NH - maatstaven] .

>[AZURE.NOTE] Geslaagde meldingen alleen betekenen dat de push-meldingen zijn geleverd om de externe Push Notification Service (bv. APNS voor Apple, GCM voor Google etc). Het is aan de PNS de melding om doelapparaten te leveren. Meestal wordt de PNS niet levering Maatstelsel van derden weergegeven.  

Wij bieden ook de mogelijkheid om de telemetriegegevens exporteren via programmacode (in **standaard** laag). Zie de [NH - Metrics monster] voor meer informatie.

[Azure klassieke Portal]: https://manage.windowsazure.com
[Melding Hubs prijzen]: http://azure.microsoft.com/pricing/details/notification-hubs/
[Melding Hubs SLA]: http://azure.microsoft.com/support/legal/sla/
[CaseStudy - Sochi]: https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=7942
[CaseStudy - Skanska]: https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=5847
[CaseStudy - Seattle Times]: https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=8354
[CaseStudy - Mural.ly]: https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=11592
[CaseStudy - 7Digital]: https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=3684
[NH - REST API's]: https://msdn.microsoft.com/library/azure/dn530746.aspx
[NH - zelfstudie aan de slag]: http://azure.microsoft.com/documentation/articles/notification-hubs-ios-get-started/
[Zelfstudie voor Apps chroom]: http://azure.microsoft.com/documentation/articles/notification-hubs-chrome-get-started/
[Mobile Services Pricing]: http://azure.microsoft.com/pricing/details/mobile-services/
[Registratie van de back-end-richtlijnen]: https://msdn.microsoft.com/library/azure/dn743807.aspx
[Richtlijnen voor registratie van de back-end - 2]: https://msdn.microsoft.com/library/azure/dn530747.aspx
[NH beveiligingsmodel]: https://msdn.microsoft.com/library/azure/dn495373.aspx
[NH - beveiligde Push-zelfstudie]: http://azure.microsoft.com/documentation/articles/notification-hubs-aspnet-backend-ios-secure-push/
[NH - problemen oplossen]: http://azure.microsoft.com/documentation/articles/notification-hubs-diagnosing/
[NH - statistieken]: https://msdn.microsoft.com/library/dn458822.aspx
[NH - Metrics monster]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/FetchNHTelemetryInExcel
[Rapporten exporteren/importeren]: https://msdn.microsoft.com/library/dn790624.aspx
[Azure Portal]: https://portal.azure.com
[volledige monsters]: https://github.com/Azure/azure-notificationhubs-samples
[Azure mobiele Apps]: https://azure.microsoft.com/en-us/services/app-service/mobile/
[Serviceprijsstelling App]: https://azure.microsoft.com/en-us/pricing/details/app-service/
