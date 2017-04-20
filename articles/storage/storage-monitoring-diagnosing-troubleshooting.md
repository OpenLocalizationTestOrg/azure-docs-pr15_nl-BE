<properties
    pageTitle="Controleren, opsporen en oplossen van problemen met opslag | Microsoft Azure"
    description="Gebruik de functies zoals analytics opslag, registratie voor client-side en andere externe hulpprogramma's te identificeren, opsporen en oplossen van problemen met Azure opslag met."
    services="storage"
    documentationCenter=""
    authors="jasonnewyork"
    manager="tadb"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/22/2016"
    ms.author="jahogg"/>

# <a name="monitor-diagnose-and-troubleshoot-microsoft-azure-storage"></a>Controleren, opsporen en oplossen van problemen met Microsoft Azure opslag

[AZURE.INCLUDE [storage-selector-portal-monitoring-diagnosing-troubleshooting](../../includes/storage-selector-portal-monitoring-diagnosing-troubleshooting.md)]

## <a name="overview"></a>Overzicht

Diagnose en het oplossen van problemen in een gedistribueerde toepassing die wordt gehost in een cloud-omgeving kunnen complexer zijn dan in een traditionele omgeving. Toepassingen kunnen worden geïmplementeerd in een infrastructuur PaaS of IaaS op locatie, op een mobiel apparaat of een combinatie hiervan. Meestal netwerkverkeer van uw toepassing kan via openbare en particuliere netwerken en toepassingen kan gebruikmaken van meerdere opslagtechnologieën zoals Microsoft Azure opslag tabellen, BLOB's, wachtrijen of bestanden op en andere gegevens worden opgeslagen die als relationele databases document.

Voor het beheer van dergelijke toepassingen is ze proactief controleren en u weten hoe u kunt vaststellen en oplossen van alle aspecten van hen en hun afhankelijke technologie. Als een gebruiker van Azure Storage-services, moet u continu de Storage-services die uw toepassing wordt gebruikt voor onverwachte wijzigingen in gedrag (zoals langzamer dan de gebruikelijke responstijden) bewaken en logboekregistratie gebruiken om gedetailleerde gegevens te verzamelen en analyseren van een probleem in de diepte. De diagnostische informatie die u verkrijgen bij het toezicht en logboekregistratie kunt u de oorzaak van het probleem uw toepassing bepalen. U kunt vervolgens het probleem oplossen en bepalen van de juiste stappen die u ondernemen kunt om deze te verhelpen. Azure opslag is een kern Azure service en vormt een belangrijk onderdeel van het merendeel van de oplossingen die klanten op de Azure-infrastructuur implementeren. Azure opslag omvat mogelijkheden voor controle, diagnose en het oplossen van problemen met opslag in de cloud-gebaseerde toepassingen te vereenvoudigen.

> [AZURE.NOTE] Opslag-rekeningen van het replicatietype van Zone redundante opslag (ZRS) beschikt niet over de metrics of de mogelijkheid tot het vastleggen op dit moment is ingeschakeld. De bestanden Azure Service ondersteunt ook niet aanmelden op dit moment.

Zie voor een praktische handleiding voor het oplossen van end-to-end in Azure, opslag, toepassingen, [End-to-End probleemoplossing met Azure opslag Metrics en logboekregistratie, AzCopy, en bericht Analyzer](storage-e2e-troubleshooting.md).

+ [Inleiding]
    + [De rangschikking van deze handleiding]
+ [De storage-service controleren]
    + [De status van de service controleren]
    + [Toezichthoudende capaciteit]
    + [Beschikbaarheid controleren]
    + [Prestaties controleren]
+ [Diagnose van problemen met opslag]
    + [Service gezondheidsvraagstukken]
    + [Problemen met prestaties]
    + [Fouten opsporen]
    + [Problemen met opslag emulator]
    + [Opslag-logboekprogramma 's]
    + [Met behulp van hulpprogramma's voor logboekregistratie]
+ [End-to-end tracering]
    + [Logboekgegevens correleren]
    + [Aanvraag-ID van client]
    + [Aanvraag-ID van server]
    + [Tijdstempels]
+ [Richtlijnen voor het oplossen van problemen]
    + [AverageE2ELatency met hoge en lage AverageServerLatency zien]
    + [Lange wachttijden op de client voordoet, maar weinig AverageE2ELatency en lage AverageServerLatency zien]
    + [Hoge AverageServerLatency zien]
    + [Er onverwachte vertragingen in de bezorging van berichten in een wachtrij]
    + [Een stijging zien in PercentThrottlingError]
    + [Een stijging zien in PercentTimeoutError]
    + [Een stijging zien in PercentNetworkError]
    + [De client ontvangt berichten HTTP 403 (niet toegestaan)]
    + [De client ontvangt berichten HTTP 404 (niet gevonden)]
    + [De client ontvangt berichten HTTP 409 (Conflict)]
    + [Lage PercentSuccess zien of analytics logitems zijn transacties met de status van ClientOtherErrors]
    + [Capaciteit zien een onverwachte toename in het gebruik van opslag capaciteit]
    + [Ondervindt u onverwacht opnieuw opstarten van de virtuele Machines met een groot aantal gekoppelde VHD 's]
    + [Uw probleem voortvloeit uit het gebruik van de opslag-emulator voor ontwikkel- of testomgeving]
    + [U ondervindt problemen bij het installeren van de Azure SDK voor .NET]
    + [U hebt een ander probleem met storage-service]
    + [Het oplossen van problemen met Windows en Linux Azure-bestanden](storage-troubleshoot-file-connection-problems.md)
+ [Aanhangsels]
    + [Aanhangsel 1: Fiddler gebruiken voor het vastleggen van HTTP- en HTTPS-verkeer]
    + [Aanhangsel 2: Wireshark netwerkverkeer vastleggen met behulp]
    + [Aanhangsel 3: Microsoft Message Analyzer netwerkverkeer vastleggen met behulp]
    + [Aanhangsel 4: Met behulp van Excel statistieken bekijken en gegevens vastleggen]
    + [Aanhangsel 5: Met inzichten van toepassing voor Visual Studio teamservices controleren]

## <a name="introduction"></a>Inleiding

Deze handleiding wordt beschreven hoe u functies zoals Analytics Azure opslag, u problemen client-side vastleggen in de clientbibliotheek Azure opslag en andere hulpprogramma's van derden te identificeren, opsporen en oplossen van problemen met Azure opslag.

![][1]

*Figuur 1 bewakings-, diagnose, en het oplossen van problemen*

Deze handleiding is bedoeld voor voornamelijk door ontwikkelaars van on line services die gebruikmaken van Azure Storage-Services en IT-professionals die verantwoordelijk zijn voor het beheer van deze on line services worden gelezen. De doelstellingen van deze handleiding zijn:

- Om u te helpen handhaven van de gezondheid en de prestaties van uw opslag Azure-accounts.
- Voor de noodzakelijke processen en hulpmiddelen om te beslissen als een probleem of een probleem in een toepassing betrekking op de Azure-opslag heeft.
- Zodat u sneller richtlijnen voor het oplossen van problemen met betrekking tot de opslag van Azure.

### <a name="how-this-guide-is-organized"></a>De rangschikking van deze handleiding

De sectie '[de storage-service controleren]' wordt beschreven hoe om te controleren van de gezondheid en de prestaties van uw opslag Azure services met Azure opslag Analytics Metrics (opslag Metrics).

De sectie '[problemen met opslag diagnose]' beschrijving van het vaststellen van problemen met Azure opslag Analytics Logging (opslag registreren). Ook wordt beschreven hoe met behulp van de faciliteiten van een van de clientbibliotheken zoals de clientbibliotheek opslag voor .NET of de Azure SDK voor Java client-side-logboekregistratie inschakelen.

De sectie '[End-to-end tracering]' wordt beschreven hoe u de gegevens in verschillende metrische gegevens en logboekbestanden kan correleren.

De "[richtlijnen voor Troubleshooting]" vindt u richtlijnen voor het oplossen van problemen voor sommige van de opslag-gerelateerde problemen besproken die kunnen optreden.

De '[bijlagen]' vindt u informatie over andere hulpprogramma's zoals Wireshark en Netmon voor packet netwerkgegevens, Fiddler voor het analyseren van HTTP/HTTPS-berichten en bericht Analyzer van Microsoft voor het correleren van logboekgegevens analyseren.


## <a name="monitoring-your-storage-service"></a>De storage-service controleren

Als u bekend met Windows Prestatiemeter bent, kunt u als equivalent van de prestatiemeters van Windows Azure opslag opslag Metrics beschouwen. Opslag Metrics vindt u een uitgebreide reeks metrics (items in de terminologie van Windows Prestatiemeter) zoals de beschikbaarheid van een service Totaal aantal aanvragen voor service of percentage van de aanvragen voor service. Zie [Opslag Analytics Metrics tabelschema](http://msdn.microsoft.com/library/azure/hh343264.aspx)voor een volledige lijst van de beschikbare statistieken. U kunt opgeven of u wilt dat de storage-service voor het verzamelen en statistische maatstaven elk uur of elke minuut. Zie voor meer informatie over het inschakelen metrics en bewaken van uw opslag rekeningen [metrics opslag inschakelen en weergeven van metrische gegevens](http://go.microsoft.com/fwlink/?LinkId=510865).

U kunt kiezen welke per uur maatstaven die u wilt weergeven in de [Portal Azure](https://portal.azure.com) en regels configureren die beheerders van kennis een e-mail wanneer een metric per uur een bepaalde drempelwaarde overschrijdt. Zie voor meer informatie, [Waarschuwingen ontvangen](../monitoring-and-diagnostics/insights-receive-alert-notifications.md). 

De storage-service worden verzameld met behulp van een Qualitative metrics, maar kunt niet elke opslagbewerking registreren.

In de Portal Azure kunt u statistieken bekijken, zoals beschikbaarheid, totaal aantal aanvragen en gemiddelde latentie getallen voor een account voor opslag. Beheerder waarschuwen als beschikbaarheid onder een bepaald niveau is ook een melding regel ingesteld. Deze gegevens bekijken, een mogelijke gebied voor onderzoek is het succespercentage van de tabel service onder de 100% (Zie voor meer informatie de sectie '[lage PercentSuccess zien of analytics logitems bewerkingen hebben met de status ClientOtherErrors]').

U moet uw Azure toepassingen zijn gezond blijft en naar verwachting door voortdurend bewaken:

- Tot vaststelling van sommige metrics volgens de basislijn voor de toepassing waarmee u de huidige gegevens vergelijken en identificatie van significante wijzigingen in het gedrag van Azure opslag en uw toepassing. De waarden van de basislijn cijfers, in veel gevallen worden specifieke toepassing en u moet tot stand worden gebracht als u de prestaties van uw toepassing te testen.
- Minuut maatstaven vastleggen en deze actief controleren op onverwachte fouten en afwijkingen zoals pieken in fout telt of tarieven aanvragen.
- Statistieken per uur opnemen en deze gemiddelde waarden, zoals controleren fout telt gemiddeld en tarieven aanvragen.
- Onderzoek naar mogelijke problemen met diagnostische hulpprogramma's zoals verderop wordt besproken in de sectie '[problemen met diagnose opslag]'.

De grafieken in afbeelding 3 hieronder wordt geïllustreerd hoe gemiddelden die voor elk uur statistieken plaatsvindt kunt verbergen pieken in activiteit. Het uurtarief statistieken worden weergegeven te geven een gelijkmatig tempo van aanvragen, terwijl de minuut statistieken blijkt dat de schommelingen die daadwerkelijk plaatsvinden.

![][3]

De rest van deze sectie wordt beschreven welke parameters u moet controleren en waarom.

### <a name="monitoring-service-health"></a>De status van de service controleren

De [Azure Portal](https://portal.azure.com) kunt u de status van de Storage-service (en andere Azure diensten) weergeven in de Azure gebieden over de hele wereld. Hiermee kunt u direct zien als een probleem buiten uw controle is die invloed hebben op de Storage-service in de regio die u voor uw toepassing.

De [Portal Azure](https://portal.azure.com) biedt ook meldingen van incidenten die van invloed zijn op de verschillende Azure services.
Opmerking: Deze informatie, samen met de historische gegevens, op de [Azure Service Dashboard](http://status.azure.com), eerder beschikbaar was.

Terwijl de [Azure Portal](https://portal.azure.com) verzamelt gezondheidsinformatie van binnen de Azure datacenters (inside out monitoring), kunt u ook heeft een benadering buiten in voor het genereren van synthetische transacties die regelmatig toegang de webtoepassing met Azure gehost vanaf meerdere locaties tot. De services van [Dynatrace](http://www.dynatrace.com/en/synthetic-monitoring) en inzichten van toepassing voor Visual Studio Team Services zijn voorbeelden van deze aanpak buiten in. Zie de bijlage voor meer informatie over de inzichten van toepassing voor Visual Studio Team Services "[aanhangsel 5: controleren met de inzichten van toepassing voor Visual Studio teamservices](#appendix-5)."

### <a name="monitoring-capacity"></a>Toezichthoudende capaciteit

Metrics opslag capaciteit maatstaven voor de blob-service alleen worden opgeslagen omdat BLOB's meestal rekening voor het grootste deel van de opgeslagen gegevens (op het moment van schrijven, het is niet mogelijk Metrics opslag gebruiken voor het controleren van de capaciteit van uw tabellen en wachtrijen). U vindt deze gegevens in de tabel **$MetricsCapacityBlob** als u de controle voor de Blob-service hebt ingeschakeld. Opslag Metrics registreert deze gegevens één keer per dag en kunt u de waarde van de **RowKey** om te bepalen of de rij een entiteit dat betrekking op de gebruikersgegevens ( **gegevens**) of analytics-gegevens (waarde **analytics heeft**) bevat. Elke entiteit opgeslagen bevat informatie over de hoeveelheid opslagruimte die wordt gebruikt (**capaciteit** gemeten in bytes) en het huidige aantal BLOB's (**ObjectCount**) de recipiënten (**ContainerCount**) en in de opslag-account gebruikt. Zie voor meer informatie over de capaciteit metrische gegevens opgeslagen in de tabel **$MetricsCapacityBlob** [Opslag Analytics Metrics tabelschema](http://msdn.microsoft.com/library/azure/hh343264.aspx).

> [AZURE.NOTE] U moet deze waarden voor een vroegtijdige waarschuwing dat u de grenzen van de capaciteit van de opslag-account nadert controleren. U kunt in de Portal Azure waarschuwingsregels om u te waarschuwen als statistische opslag gebruik overschrijdt of onderschrijdt drempelwaarden die u opgeeft toevoegen.

BLOB's, Zie voor hulp bij het schatten van de grootte van verschillende objecten voor gegevensopslag, zoals de blog posten [Wat Azure opslag facturering: bandbreedte, transacties, en de capaciteit](http://blogs.msdn.com/b/windowsazurestorage/archive/2010/07/09/understanding-windows-azure-storage-billing-bandwidth-transactions-and-capacity.aspx).

### <a name="monitoring-availability"></a>Beschikbaarheid controleren

Moet u de beschikbaarheid van de storage-services in uw account opslag controleren door de waarde in de kolom **beschikbaarheid** in de tabellen per uur of minuut metrics controleren, **$MetricsHourPrimaryTransactionsBlob**, **$MetricsHourPrimaryTransactionsTable**, **$MetricsHourPrimaryTransactionsQueue**, **$MetricsMinutePrimaryTransactionsBlob**, **$MetricsMinutePrimaryTransactionsTable**, **$MetricsMinutePrimaryTransactionsQueue**, **$MetricsCapacityBlob**. De kolom **beschikbaarheid** bevat een percentagewaarde die aangeeft van de beschikbaarheid van de service of de bewerking API van de rij (de **RowKey** wordt weergegeven als de rij statistieken voor de service als geheel of voor een bepaalde bewerking in de API bevat).

Een waarde van minder dan 100% geeft aan dat bepaalde opslag aanvragen lukt niet. U kunt zien waarom zij door het onderzoeken van de andere kolommen in de metrische gegevens die het aantal aanvragen met verschillende fouttypen zoals **ServerTimeoutError**mislukt. U kunt verwachten voor een overzicht van **beschikbaarheid** vallen tijdelijk onder de 100% om redenen zoals tijdelijke servertime terwijl de service partities naar betere taakverdeling verzoek verplaatst; deze periodieke voorwaarden moet worden omgegaan met de logica opnieuw in de clienttoepassing. Artikel [opslag Analytics geregistreerd en statusberichten](http://msdn.microsoft.com/library/azure/hh343260.aspx) worden de typen met maatstaven voor opslag in de berekening van de **beschikbaarheid** .

In de [Azure Portal](https://portal.azure.com)kunt u toevoegen waarschuwingsregels om u te waarschuwen als de **beschikbaarheid** van een service valt onder een drempel die u opgeeft.

De "[richtlijnen voor Troubleshooting]" sectie van deze handleiding wordt beschreven dat sommige opslag serviceproblemen besproken die betrekking hebben op serverbeschikbaarheid.

### <a name="monitoring-performance"></a>Prestaties controleren

Voor het controleren van de prestaties van de storage-services, kunt u de volgende parameters uit de tabellen per uur en minuut.

- De waarden in de kolommen **AverageE2ELatency** en **AverageServerLatency** geven de gemiddelde tijd de storage-service of API-bewerkingstype genomen aanvragen verwerkt. **AverageE2ELatency** is een maatstaf voor end-to-end latentie met de tijd die de aanvraag lezen en verzenden van de reactie de tijd die nodig is om de aanvraag te verwerken (dus inclusief netwerkvertraging zodra de storage-service het verzoek is bereikt); **AverageServerLatency** is een maatregel van alleen de verwerkingstijd en omvat derhalve niet de netwerkvertraging betrekking hebben op de client communiceert. Zie de sectie '[zien AverageE2ELatency hoge en lage AverageServerLatency]' verderop in deze handleiding voor een bespreking van waarom er een groot verschil tussen deze twee waarden kan zijn.
- De waarden in de kolommen **TotalIngress** en **TotalEgress** weergeven de totale hoeveelheid gegevens, in bytes, bijna en uit uw storage-service of door middel van een specifiek type van de API-bewerking.
- De waarden in de kolom **TotalRequests** geven het totale aantal aanvragen dat is ontvangen van de storage-service van de API. **TotalRequests** is het totale aantal aanvragen dat de storage-service ontvangt.

Meestal controleert u op onverwachte wijzigingen in een van deze waarden als een indicator dat er een probleem dat onderzoek vereist.

U kunt in de [Portal Azure](https://portal.azure.com)waarschuwingsregels om u te waarschuwen als een van de prestatiegegevens voor deze service vallen onder toevoegen of groter zijn dan een drempel die u opgeeft.

De "[richtlijnen voor Troubleshooting]" sectie van deze handleiding wordt beschreven dat sommige opslag serviceproblemen besproken die betrekking hebben op prestaties.


## <a name="diagnosing-storage-issues"></a>Diagnose van problemen met opslag

Er zijn een aantal manieren die u op de hoogte van een probleem of een probleem in uw toepassing worden kan, deze omvatten:

- Een ernstige fout dat ervoor zorgt de toepassing dat vastloopt of niet meer werkt.
- Belangrijke wijzigingen van basislijnwaarden in de statistieken u controleert als beschreven in de vorige sectie "van[uw storage-service controleren]."
- Rapporten van gebruikers van uw toepassing die een bepaalde bewerking niet voltooid zoals verwacht of dat sommige functie niet werkt.
- Fouten die worden gegenereerd binnen de toepassing die worden weergegeven in de logboekbestanden of via een andere methode voor de kennisgeving.

Meestal vallen kwesties in verband met Azure storage-services in een van de vier algemene categorieën:

- De toepassing heeft een prestatieprobleem gerapporteerd door gebruikers of door wijzigingen in de prestatiegegevens worden onthuld.
- Er is een probleem met de Azure opslaginfrastructuur in een of meer regio's.
- De toepassing ondervindt een fout gerapporteerd door gebruikers of aan het licht gebracht door een verhoging van de fout aantal cijfers over de controleren.
- Tijdens het ontwikkelen en testen, kunt u de emulator lokale opslag; enkele problemen die specifiek betrekking op het gebruik van de emulator opslag hebben kunnen optreden.

De volgende gedeelten worden de stappen die u moet volgen voor het opsporen en oplossen van problemen in elk van deze vier categorieën. De '[richtlijnen voor Troubleshooting]' verderop in deze handleiding vindt u meer details voor enkele veelvoorkomende problemen besproken die kunnen optreden.

### <a name="service-health-issues"></a>Service gezondheidsvraagstukken

Service gezondheid worden meestal buiten het besturingselement. De [Azure Portal](https://portal.azure.com) biedt informatie over aanhoudende problemen met Azure services, met inbegrip van de opslag. Als u ervoor voor Geo-redundante opslag lezen toegang gekozen bij het maken van uw account voor de opslag, uw gegevens in de primaire locatie tijdelijk niet beschikbaar zijn in het geval van kan uw toepassing schakelt u tijdelijk naar de alleen-lezen kopie in de secundaire locatie. Hiervoor moet uw toepassing kunt schakelen tussen de primaire en secundaire locaties en kunnen werken in de modus van verminderde functionaliteit met alleen-lezen gegevens. De clientbibliotheken Azure opslag kunnen u beleid opnieuw dat in secundaire opslag lezen kan in het geval van primaire opslag gelezen mislukt. Uw toepassing moet ook rekening houden dat de gegevens in de secundaire locatie uiteindelijk consistent is. Voor meer informatie, Zie de blog [redundantieopties voor Azure opslag en leestoegang Geo-redundante opslag](https://blogs.msdn.microsoft.com/windowsazurestorage/2013/12/11/windows-azure-storage-redundancy-options-and-read-access-geo-redundant-storage/)boeken.

### <a name="performance-issues"></a>Problemen met prestaties

De prestaties van een toepassing kan worden subjectieve, met name vanuit het perspectief van een gebruiker. Het is dus belangrijk om maatstaven van basislijn beschikbaar waarmee u kunt vaststellen waar er mogelijk een probleem met de prestaties. Veel factoren kunnen van invloed op de prestaties van een Azure storage-service vanuit het perspectief van de toepassing client. Deze factoren kunnen werken in de storage-service, de client of de netwerkinfrastructuur; Daarom is het belangrijk om een strategie voor de identificatie van de oorsprong van het probleem met de prestaties.

Nadat u hebt aangegeven dat de waarschijnlijke locatie van de oorzaak van het probleem met de prestaties van de cijfers, vervolgens kunt u de logboekbestanden naar gedetailleerde informatie voor het opsporen en oplossen van het probleem verder.

De sectie "[richtlijnen voor Troubleshooting]' verderop in deze handleiding vindt u meer informatie over de algemene prestaties gerelateerde problemen u tegenkomen.

### <a name="diagnosing-errors"></a>Fouten opsporen

Gebruikers van uw toepassing kunnen een melding van fouten die door de clienttoepassing. Opslag Metrics registreert ook de aantallen van de verschillende fouttypen van uw storage-services zoals **NetworkError**, **ClientTimeoutError**of **AuthorizationError**. Terwijl de opslag statistieken alleen tellingen van verschillende fouttypen records, kunt u meer informatie over afzonderlijke aanvragen door het onderzoeken van server-side en client-side netwerk Logboeken. Meestal geeft de HTTP-statuscode die wordt geretourneerd door de storage-service een opgave van de reden waarom de aanvraag is mislukt.

> [AZURE.NOTE] Houd er rekening mee dat u voor een overzicht van enkele regelmatig terugkerende fouten verwachten: bijvoorbeeld fouten als gevolg van tijdelijke netwerkproblemen of toepassingsfouten.

De volgende bronnen zijn handig voor informatie over opslag-gerelateerde status en foutcodes:

- [Algemene REST API-fout Codes](http://msdn.microsoft.com/library/azure/dd179357.aspx)
- [Foutcodes BLOB-Service](http://msdn.microsoft.com/library/azure/dd179439.aspx)
- [Foutcodes voor wachtrij-Service](http://msdn.microsoft.com/library/azure/dd179446.aspx)
- [Tabel Service-foutcodes](http://msdn.microsoft.com/library/azure/dd179438.aspx)
- [Bestand Service-foutcodes](https://msdn.microsoft.com/library/azure/dn690119.aspx)

### <a name="storage-emulator-issues"></a>Problemen met opslag emulator

De Azure SDK bevat een opslag emulator die u op een werkstation ontwikkeling uitvoeren kunt. Deze emulator simuleert de meeste van de werking van de van Azure opslagservices en is handig tijdens het ontwikkelen en testen, zodat u de toepassingen die gebruikmaken van Azure opslagservices zonder de noodzaak van een Azure-abonnement en een account Azure opslag uit te voeren.

De "[richtlijnen voor Troubleshooting]" sectie van deze handleiding beschrijft enkele problemen opgetreden met de emulator opslag.

### <a name="storage-logging-tools"></a>Opslag-logboekprogramma 's

Logboekregistratie voor opslag biedt aan de serverzijde logboekregistratie van aanvragen voor opslag in uw account Azure opslag. Zie voor meer informatie over het server-side-logboekregistratie inschakelen en toegang tot de logboekgegevens [opslag vastleggen inschakelen en toegang tot gegevens](http://go.microsoft.com/fwlink/?LinkId=510867).

De Client-bibliotheek van opslag voor .NET kunt u voor het verzamelen van logboekgegevens voor client-side met betrekking tot opslagbewerkingen die worden uitgevoerd door de toepassing. Voor meer informatie Zie [Client-side vastleggen met de .NET opslag Client Library](http://go.microsoft.com/fwlink/?LinkId=510868).

> [AZURE.NOTE] In sommige gevallen (zoals SAS vergunning fouten), kan een gebruiker waarvoor u geen gegevens in de logboeken van server-side Storage vindt fout gemeld. U kunt gebruiken de logboekfuncties van Client-bibliotheek van de opslag te onderzoeken als de oorzaak van het probleem op de client of hulpprogramma's voor Netwerkcontrole gebruiken bij het onderzoeken van het netwerk.

### <a name="using-network-logging-tools"></a>Met behulp van hulpprogramma's voor logboekregistratie

U kunt het verkeer tussen de client en de server vindt u gedetailleerde informatie over de gegevens die de client en server worden uitgewisseld en de onderliggende netwerk voorwaarden vastleggen. Nuttige hulpprogramma's voor registratie zijn:

- [Fiddler](http://www.telerik.com/fiddler) is een gratis web debugging proxy waarmee u de koptekst en de nettolading van HTTP- en HTTPS-aanvraag en het antwoord berichten bekijken. Zie voor meer informatie [aanhangsel 1: Fiddler voor het vastleggen van HTTP- en HTTPS-verkeer met behulp van](#appendix-1).
- [Microsoft Network Monitor (Netmon)](http://www.microsoft.com/download/details.aspx?id=4865) en [Wireshark](http://www.wireshark.org/) zijn gratis protocol netwerkanalysesystemen waarmee u een pakket gedetailleerde informatie weergeven voor een groot aantal netwerkprotocollen. Zie voor meer informatie over Wireshark '[aanhangsel 2: Wireshark netwerkverkeer vastleggen met behulp van](#appendix-2)".
- Microsoft Message Analyzer is een hulpprogramma van Microsoft waarmee Netmon en die naast het opnemen van netwerkgegevens pakket vervangt, kunt u bekijken en analyseren van gegevens uit andere programma's zijn vastgelegd. Zie voor meer informatie "[van aanhangsel 3: Microsoft Message Analyzer netwerkverkeer vastleggen met behulp van](#appendix-3)".
- Als u testen om te controleren wilt dat uw clientcomputer verbinding met de Azure storage-service via het netwerk maken kan verbinding, doen u dit niet met het hulpprogramma **ping** van de standaard op de client. U kunt echter het [hulpprogramma **tcping** ](http://www.elifulkerson.com/projects/tcping.php) connectiviteit te controleren.

In veel gevallen de logboekgegevens van registratie van de opslag en de clientbibliotheek opslag zal volstaan voor het vaststellen van een probleem, maar in sommige scenario's moet u mogelijk de meer gedetailleerde informatie die door deze hulpprogramma's voor logboekregistratie kunnen bieden. Bijvoorbeeld kunt u bekijken-header en de nettolading van gegevens die worden verzonden naar en van de storage-services waarmee u bekijken hoe een clienttoepassing opslagbewerkingen pogingen Fiddler via HTTP en HTTPS-berichten bekijken. Protocol analyzers zoals Wireshark werken op het pakketniveau van het waarmee u TCP-gegevens waarmee u verloren pakketten en verbindingsproblemen oplossen. Bericht Analyzer kunt werken met HTTP- en TCP-lagen.

## <a name="end-to-end-tracing"></a>End-to-end tracering

End-to-end tracering met behulp van een groot aantal logboekbestanden is een handige techniek voor het onderzoeken van potentiële problemen. Kunt u de datum/tijd-gegevens van de metrische gegevens als een opgave waar te bekijken in de logboekbestanden voor de gedetailleerde informatie die helpt bij het oplossen van het probleem.

### <a name="correlating-log-data"></a>Logboekgegevens correleren

Bij het bekijken van de logboeken van clienttoepassingen traceert u netwerk en server-side storage-registratie is het belangrijk om te kunnen correleren vraagt over de verschillende logboekbestanden. De logboekbestanden bevatten een aantal verschillende velden die gebruikt als correlatie-id worden. Aanvraag-id van client is het handigste veld met gegevens in verschillende logboeken correleren. Maar soms kan het handig zijn met de aanvraag-id van server- of tijdstempels. De volgende secties bevatten meer informatie over deze opties.

### <a name="client-request-id"></a>Aanvraag-ID van client

De bibliotheek opslag Client genereert automatisch een aanvraag voor unieke client-id voor elke aanvraag.

- In het logboek voor client-side waarmee de Client-bibliotheek van opslag, verschijnt de aanvraag-id van client in het veld **Client aanvraag-ID** van elke vermelding met betrekking tot het verzoek.
- In een netwerk-trace zoals vastgelegd door Fiddler is de aanvraag-id van client zichtbaar in request-berichten als de waarde van de **x-ms-client-aanvraag-id** in de HTTP-header.
- In het logboek server-side Storage Logging weergegeven client aanvraag-id in de kolom Client aanvraag-ID.

> [AZURE.NOTE]Het is mogelijk meerdere aanvragen delen dezelfde id van de client omdat de client deze waarde toewijzen kunt (Hoewel de clientbibliotheek opslag wordt automatisch een nieuwe waarde toegewezen). Alle pogingen delen voor nieuwe pogingen van de client dezelfde id van de client. Wanneer een partij van de client is verzonden, heeft de partij een aanvraag-id van één client.


### <a name="server-request-id"></a>Aanvraag-ID van server

Server aanvraag-id wordt automatisch gegenereerd door de storage-service.

- De aanvraag-id weergegeven in het logboek aan de serverzijde opslag registreren de **kop van de aanvraag-ID** -kolom.
- In een netwerk-trace zoals vastgelegd door Fiddler, weergegeven de aanvraag-id als de waarde van de **x-ms-aanvraag-id** in de HTTP-header in antwoordberichten.
- In het logboek voor client-side waarmee de Client-bibliotheek van opslag, wordt de aanvraag-id weergegeven in de kolom **Tekst bewerking** voor de vermelding met de details van het antwoord van de server.

> [AZURE.NOTE] De storage-service altijd een unieke server aanvraag-id toewijst aan elke aanvraag ontvangen, zodat elke nieuwe poging van de client en elke bewerking in een batch worden opgenomen, de unieke id van een is.

Als de bibliotheek opslag Client een **StorageException** op de client genereert, bevat de eigenschap **RequestInformation** een **RequestResult** -object met een eigenschap van de **ServiceRequestID** . U kunt ook toegang tot een **RequestResult** -object uit een **OperationContext** exemplaar.

Het volgende codevoorbeeld laat zien hoe de waarde van een aangepaste **ClientRequestId** door een object **OperationContext** de aanvraag koppelen aan de storage-service ingesteld. Ook ziet u hoe de waarde **ServerRequestId** ophalen uit het response-bericht.

    //Parse the connection string for the storage account.
    const string ConnectionString = "DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key";
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(ConnectionString);
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Create an Operation Context that includes custom ClientRequestId string based on constants defined within the application along with a Guid.
    OperationContext oc = new OperationContext();
    oc.ClientRequestID = String.Format("{0} {1} {2} {3}", HOSTNAME, APPNAME, USERID, Guid.NewGuid().ToString());

    try
    {
        CloudBlobContainer container = blobClient.GetContainerReference("democontainer");
        ICloudBlob blob = container.GetBlobReferenceFromServer("testImage.jpg", null, null, oc);  
        var downloadToPath = string.Format("./{0}", blob.Name);
        using (var fs = File.OpenWrite(downloadToPath))
        {
            blob.DownloadToStream(fs, null, null, oc);
            Console.WriteLine("\t Blob downloaded to file: {0}", downloadToPath);
        }
    }
    catch (StorageException storageException)
    {
        Console.WriteLine("Storage exception {0} occurred", storageException.Message);
        // Multiple results may exist due to client side retry logic - each retried operation will have a unique ServiceRequestId
        foreach (var result in oc.RequestResults)
        {
                Console.WriteLine("HttpStatus: {0}, ServiceRequestId {1}", result.HttpStatusCode, result.ServiceRequestID);
        }
    }


### <a name="timestamps"></a>Tijdstempels

U kunt ook de tijdstempels gebruiken om te zoeken naar verwante logboekvermeldingen, maar wees voorzichtig met een klokafwijking tussen de client en de server die zich kan voordoen. Plus of min 15 minuten voor het afstemmen van server-side posten op basis van het tijdstempel op de client moet worden gezocht. Houd er rekening mee dat de blob-metagegevens voor het BLOB's met parameters het tijdsbereik voor de metrische gegevens opgeslagen in de blob; Dit is handig als er veel metrics BLOB's voor dezelfde minuut of uur.

## <a name="troubleshooting-guidance"></a>Richtlijnen voor het oplossen van problemen

Deze sectie zal u helpen bij de diagnose en het oplossen van een aantal veelvoorkomende problemen van uw toepassing kan optreden wanneer u de Azure storage-services. Met de onderstaande lijst zoeken naar de informatie die relevant zijn voor uw specifieke probleem.

**De beslissingsstructuur van het oplossen van problemen**

----------

Het probleem betrekking hebben op de prestaties van een van de storage-services?

- [AverageE2ELatency met hoge en lage AverageServerLatency zien]
- [Lange wachttijden op de client voordoet, maar weinig AverageE2ELatency en lage AverageServerLatency zien]
- [Hoge AverageServerLatency zien]
- [Er onverwachte vertragingen in de bezorging van berichten in een wachtrij]

----------

Het probleem betrekking hebben op de beschikbaarheid van een van de storage-services?

- [Een stijging zien in PercentThrottlingError]
- [Een stijging zien in PercentTimeoutError]
- [Een stijging zien in PercentNetworkError]

----------

De clienttoepassing ontvangt een 4XX (bijvoorbeeld 404) HTTP-antwoord van storage-service?

- [De client ontvangt berichten HTTP 403 (niet toegestaan)]
- [De client ontvangt berichten HTTP 404 (niet gevonden)]
- [De client ontvangt berichten HTTP 409 (Conflict)]

----------

[Lage PercentSuccess zien of analytics logitems zijn transacties met de status van ClientOtherErrors]

----------

[Capaciteit zien een onverwachte toename in het gebruik van opslag capaciteit]

----------

[Ondervindt u onverwacht opnieuw opstarten van de virtuele Machines met een groot aantal gekoppelde VHD 's]

----------

[Uw probleem voortvloeit uit het gebruik van de opslag-emulator voor ontwikkel- of testomgeving]

----------

[U ondervindt problemen bij het installeren van de Azure SDK voor .NET]

----------

[U hebt een ander probleem met storage-service]

----------

### <a name="metrics-show-high-AverageE2ELatency-and-low-AverageServerLatency"></a>AverageE2ELatency met hoge en lage AverageServerLatency zien

In de afbeelding hieronder van [Azure Portal](https://portal.azure.com) monitoring tool toont een voorbeeld waarin de **AverageE2ELatency** aanzienlijk hoger is dan de **AverageServerLatency**is.

![][4]

Houd er rekening mee dat de storage-service alleen de metric **AverageE2ELatency** voor aanvragen berekent en, in tegenstelling tot **AverageServerLatency**, ook de duur voor de client gegevens verzenden en ontvangen van de bevestiging van de storage-service. Een verschil tussen **AverageE2ELatency** en **AverageServerLatency** kan dus door de clienttoepassing wordt traag reageren, of als gevolg van de voorwaarden in het netwerk.

> [AZURE.NOTE] U kunt ook **E2ELatency** en **ServerLatency** voor van afzonderlijke opslagbewerkingen in de opslag registreren logboekgegevens bekijken.

#### <a name="investigating-client-performance-issues"></a>Onderzoek naar problemen met de prestaties van client

Mogelijke oorzaken voor de client reageert traag zijn met een beperkt aantal beschikbare verbindingen of threads of weinig bronnen, zoals CPU, geheugen of netwerk bandbreedte wordt. U kunt mogelijk het probleem opgelost doordat de clientcode worden efficiënter (bijvoorbeeld door met asynchrone oproepen aan de storage-service) of met behulp van een grotere virtuele Machine (met meer cores en meer geheugen).

Voor de tabel- en -services, het algoritme Nagle kan ook worden hoge **AverageE2ELatency** in vergelijking met **AverageServerLatency**: Zie de post [van Nagle algoritme is geen beschrijvende kleine aanvragen](http://blogs.msdn.com/b/windowsazurestorage/archive/2010/06/25/nagle-s-algorithm-is-not-friendly-towards-small-requests.aspx)voor meer informatie. U kunt het algoritme Nagle in code met behulp van de klasse **ServicePointManager** in de naamruimte **System.Net** uitschakelen. U moet dit doen voordat u de aanroepen van de tabel of wachtrijservices in uw toepassing omdat dit geen invloed op verbindingen die al zijn openen. In het volgende voorbeeld is afkomstig van de methode **Application_Start** in functie van een werknemer.

    var storageAccount = CloudStorageAccount.Parse(connStr);
    ServicePoint tableServicePoint = ServicePointManager.FindServicePoint(storageAccount.TableEndpoint);
    tableServicePoint.UseNagleAlgorithm = false;
    ServicePoint queueServicePoint = ServicePointManager.FindServicePoint(storageAccount.QueueEndpoint);
    queueServicePoint.UseNagleAlgorithm = false;

Controleer de logboekbestanden van client-side om te zien hoeveel aanvragen dat de clienttoepassing verzendt en controleren op algemene .NET gerelateerde knelpunten in de mailclient zoals CPU, .NET garbagecollection, netwerkgebruik of geheugen. Zie [Foutopsporing, traceren, en profileren](http://msdn.microsoft.com/library/7fe0dd2y)als een beginpunt voor het oplossen van .NET client-toepassingen.

#### <a name="investigating-network-latency-issues"></a>Onderzoekt de latentie van de netwerkverbinding

High end-to-end latentie veroorzaakt door het netwerk is meestal als gevolg van tijdelijke situaties. Beide tijdelijke en permanente netwerkproblemen zoals verloren gegane pakketten kunt u onderzoeken met behulp van hulpprogramma's zoals Wireshark of Microsoft Message Analyzer.

Zie voor meer informatie over het oplossen van problemen met netwerken met Wireshark '[aanhangsel 2: Wireshark netwerkverkeer vastleggen met]. "

Zie voor meer informatie over het oplossen van netwerkproblemen met Microsoft Message Analyzer '[aanhangsel 3: Analyzer van Microsoft Message netwerkverkeer vastleggen met]. "

### <a name="metrics-show-low-AverageE2ELatency-and-low-AverageServerLatency"></a>Lange wachttijden op de client voordoet, maar weinig AverageE2ELatency en lage AverageServerLatency zien

In dit scenario is de meest waarschijnlijke oorzaak een vertraging in de opslagverzoeken de storage-service is bereikt. U moet onderzoeken waarom aanvragen van de client niet maken tot en met de blob-service.

Een mogelijke reden voor de client aanvragen verzenden vertragen is dat er een beperkt aantal beschikbare verbindingen of threads.

U moet ook controleren of de client uitvoert meerdere pogingen en onderzoek de reden als dit het geval is. Om te bepalen of de client meerdere pogingen uitvoert, kunt u het volgende doen:

- Bekijk de logboekbestanden opslag Analytics. Als meerdere pogingen zijn voordoet, worden er meerdere bewerkingen met dezelfde client aanvraag-ID, maar met een andere serveraanvraag-id's.
- Bekijk de logboekbestanden van client. Uitgebreide logboekregistratie wordt aangegeven dat er een nieuwe poging is opgetreden.
- Fouten opsporen in uw code en controleert u de eigenschappen van het **OperationContext** -object dat is gekoppeld aan de aanvraag. Als de bewerking opnieuw is gestart, wordt de eigenschap **RequestResults** meerdere aanvragen voor de unieke id's bevatten. U kunt ook de begin- en eindtijden voor elke aanvraag controleren. Zie het voorbeeld in de sectie [ID van Server]voor meer informatie.

Als er geen problemen in de client zijn, moet u potentiële problemen met netwerken zoals pakketverlies onderzoeken. Hulpmiddelen voor Wireshark of Microsoft Message Analyzer kunt u netwerkproblemen te onderzoeken.

Zie voor meer informatie over het oplossen van problemen met netwerken met Wireshark '[aanhangsel 2: Wireshark netwerkverkeer vastleggen met]. "

Zie voor meer informatie over het oplossen van netwerkproblemen met Microsoft Message Analyzer '[aanhangsel 3: Analyzer van Microsoft Message netwerkverkeer vastleggen met]. "

### <a name="metrics-show-high-AverageServerLatency"></a>Hoge AverageServerLatency zien

Bij hoge **AverageServerLatency** voor aanvragen voor het blob downloaden, moet u de opslag registreren Logboeken om te zien of er herhaalde aanvragen voor dezelfde blob (of het aantal BLOB's). Uploaden voor blob aanvragen, moet u onderzoeken welk blok grootte van de client wordt gebruikt (bijvoorbeeld wordt geblokkeerd kleiner zijn dan 64 kB kan leiden tot overheadkosten tenzij het leest ook in minder dan 64K stukken), en als meerdere clients blokken naar de dezelfde blob parallel wilt uploaden. Controleer ook de statistieken per minuut voor pieken in het aantal aanvragen dat leiden meer dan tot het per tweede schaalbaarheid doelen: Zie ook '[zien een toename van PercentTimeoutError]'.

Als u hoge **AverageServerLatency** voor blob aanvragen downloaden wanneer er herhaalde aanvragen voor dezelfde blob of het aantal BLOB's ziet, moet u vervolgens overwegen deze BLOB's Azure-Cache of de Azure Content Delivery Network (CDN) met cache. Voor aanvragen voor uploaden, kunt u de doorvoer verbeteren met behulp van een groter blok. Voor query's naar tabellen is het ook kunnen worden toegepast op clients die querybewerkingen uitvoeren en waar de gegevens veranderen niet vaak caching aan clientzijde.

Hoge zijn **AverageServerLatency** een symptoom van een slecht geschreven tabellen of query's resulteren in scanbewerkingen of die als het toevoegen/Voeg anti-patroon volgt. Zie '[zien een toename van de PercentThrottlingError]' voor meer informatie.

> [AZURE.NOTE] Vindt u een uitgebreide checklist prestaties controlelijst hier: [Microsoft Azure opslag prestaties en schaalbaarheid, controlelijst](storage-performance-checklist.md).

### <a name="you-are-experiencing-unexpected-delays-in-message-delivery"></a>Er onverwachte vertragingen in de bezorging van berichten in een wachtrij

Als er een vertraging tussen het moment dat een toepassing een bericht naar een wachtrij wordt toegevoegd en het moment dat deze beschikbaar in de wachtrij lezen, moet u de volgende stappen uit om de oorzaak van het probleem te nemen:

- Controleer of dat de toepassing met succes de berichten naar de wachtrij toevoegt. Controleer dat de toepassing wordt niet opnieuw de methode **AddMessage** meerdere malen voordat het lukt. De logboekbestanden van Client-bibliotheek van opslag wordt een herhaalde pogingen van de opslag weergegeven.
- Controleer of er geen klok scheeftrekken tussen de rol van de werknemer die het bericht worden toegevoegd aan de wachtrij en de rol van de werknemer die leest het bericht in de wachtrij die u het maakt verschijnen als er een vertraging in de verwerking van is.
- Controleer als de rol van de werknemer die de berichten uit de wachtrij leest niet goed werkt. Als een wachtrij-client de methode **GetMessage roept** maar niet reageert met een bevestiging, blijft het bericht zichtbaar in de wachtrij totdat de **invisibilityTimeout** verstreken. Het bericht wordt nu beschikbaar voor het opnieuw verwerken.
- Controleer als de lengte van de loop der tijd groeit. Deze fout kan optreden als er niet voldoende werknemers beschikbaar voor het verwerken van alle berichten die door andere werknemers in de wachtrij plaatst. U moet ook controleren de cijfers zien als delete-aanvragen mislukken en de telling van de wachtrij halen op berichten die worden herhaald mislukte pogingen om het bericht te verwijderen.
- Bekijk de logboekbestanden opslag registreren voor alle bewerkingen die hoger dan verwachte waarden voor **E2ELatency** en **ServerLatency** over een langere periode dan gebruikelijk zijn.


### <a name="metrics-show-an-increase-in-PercentThrottlingError"></a>Een stijging zien in PercentThrottlingError

Bandbreedteregeling fouten optreden wanneer u de schaalbaarheid doelen van storage-service overschrijden. De storage-service doet dit om ervoor te zorgen dat geen enkele client of de huurder de service ten koste van anderen gebruiken kan. Zie voor meer informatie [Azure opslag schaalbaarheid en prestaties, doelen](storage-scalability-targets.md) voor meer informatie over de schaalbaarheid doelen voor opslag en prestatiedoelstellingen vaststellen voor de partities binnen opslag rekeningen.

Als de metric **PercentThrottlingError** een stijging in het percentage van de aanvragen die met een fout bandbreedteregeling mislukken wilt weergeven, moet u een van de twee scenario's te onderzoeken:

- [Tijdelijke toename van de PercentThrottlingError]
- [Permanente toename van de PercentThrottlingError fout]

Een verhoging van de **PercentThrottlingError** gebeurt vaak op hetzelfde moment als een toename van het aantal aanvragen voor opslag of als u in eerste instantie Laad uw toepassing te testen. Dit kan ook tot uiting komen in de client als '503 Server bezet' of 'time-out van 500 bewerking' http-statusberichten van opslag-activiteiten.

#### <a name="transient-increase-in-PercentThrottlingError"></a>Tijdelijke toename van de PercentThrottlingError

Als u pieken in de waarde van **PercentThrottlingError** die met tijdvakken van hoge activiteit voor de toepassing samenvallen, u een exponentiële (niet lineair) terug uit de strategie voor nieuwe pogingen te implementeren in uw client: dit helpen uw toepassing vloeiend op pieken in het verkeer en verminder de belasting onmiddellijk op de partitie. Zie voor meer informatie over het implementeren van beleid voor opnieuw met de Client-bibliotheek voor opslag [Microsoft.WindowsAzure.Storage.RetryPolicies Namespace](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.retrypolicies.aspx).

> [AZURE.NOTE] U ziet ook pieken in de waarde van **PercentThrottlingError** die niet met tijdvakken van hoge activiteit voor de toepassing samenvallen: hier de meest waarschijnlijke oorzaak is het verplaatsen van partities voor het verbeteren van taakverdeling storage-service.

#### <a name="permanent-increase-in-PercentThrottlingError"></a>Permanente toename van de PercentThrottlingError fout

Als u een consistent hoge waarde voor **PercentThrottlingError** na een permanente verhoging in uw transactie volumes, of als u uw initiële laden voert proeven van uw toepassing, moet u om te evalueren hoe uw toepassing partities opslag wordt gebruikt en of deze nadert de schaalbaarheid doelen voor een account voor opslag. Bijvoorbeeld, als u fouten in een wachtrij (die telt als één partitie) beperken, moet vervolgens u overwegen aanvullende wachtrijen aan de transacties verspreid over meerdere partities. Als u fouten in een tabel te beperken, moet u uw transacties verspreid over meerdere partities met behulp van een breder bereik van waarden van de partitie met een ander partitieschema. Een veel voorkomende oorzaak van dit probleem is de prepend/append anti-patroon waar u de datum als de partitiesleutel selecteren en vervolgens alle gegevens op een bepaalde dag aan één partitie worden geschreven: onder belasting, kan dit resulteren in een knelpunt schrijven. U moet rekening houden met een ander ontwerp maken van partities of beoordelen of blob-opslag kan een betere oplossing zijn. Ook als de beperking als gevolg van de pieken in het verkeer voordoet zich te controleren en u manieren vloeiend het patroon van de aanvragen te onderzoeken.

Als u uw transacties over meerdere partities verdelen, moet u nog steeds op de hoogte van de schaalbaarheid grenzen voor de opslag-account zijn. Bijvoorbeeld, als u tien wachtrijen elke verwerking van het maximum van 2.000 berichten van 1KB per seconde gebruikt, u niet op de totale limiet van 20.000 berichten per seconde voor de opslag. Als u meer dan 20.000 entiteiten per seconde verwerken, moet u rekening houden met behulp van meerdere accounts voor opslag. U moet ook moet er rekening mee dat de grootte van uw aanvragen en entiteiten invloed is op wanneer de storage-service bandbreedte voor uw klanten: hebt u grotere aanvragen en entiteiten, u kunt de snelheid van de eerder.

Query inefficiënt ontwerp kan ook worden de limieten van schaalbaarheid voor Tabelpartities bestookt. Een query met een filter dat slechts één procent van de entiteiten in een partitie wordt geselecteerd, maar die scant alle entiteiten in een partitie moet bijvoorbeeld toegang tot elke entiteit. Elke entiteit lezen meetellen bij het totale aantal transacties in die partitie; u kunt dus gemakkelijk de schaalbaarheid doelen bereiken.

> [AZURE.NOTE] Uw prestaties testen indien geen inefficiënte queryontwerpen in uw toepassing blijkt.

### <a name="metrics-show-an-increase-in-PercentTimeoutError"></a>Een stijging zien in PercentTimeoutError

Uw cijfers zien een toename in **PercentTimeoutError** voor een van uw storage-services. Op hetzelfde moment ontvangt de client een groot aantal '500 bewerking time-out' http-statusberichten van opslagbewerkingen.

> [AZURE.NOTE] Er kunnen time-outfouten als de storage-service tijdelijk saldi aanvragen door een partitie verplaatsen naar een nieuwe server te laden.

De **PercentTimeoutError** metric is een samenvoeging van de volgende criteria: **ClientTimeoutError**, **AnonymousClientTimeoutError**, **SASClientTimeoutError**, **ServerTimeoutError**, **AnonymousServerTimeoutError**en **SASServerTimeoutError**.

De time-outs van de server worden veroorzaakt door een fout op de server. De client time-outs gebeuren omdat een bewerking op de server heeft overschreden de time-out is opgegeven door de client. bijvoorbeeld, kunt een client met het Client-bibliotheek van opslag een time-out voor een bewerking instellen met de eigenschap **ServerTimeout** van de klasse **QueueRequestOptions** .

Time-outs voor server wijzen op een probleem met de storage-service die verder onderzoek vereist. Kunt u statistieken te zien als u de maximale schaalbaarheid voor de service zijn raken en het identificeren van eventuele pieken in het verkeer dat dit probleem veroorzaakt. Als het probleem onderbroken wordt, kan dit liggen aan Netwerktaakverdeling activiteit in de service. Als het probleem permanent is en wordt niet veroorzaakt door uw toepassing kunt u door de grenzen van de schaalbaarheid van de service, moet u een probleem kunt verhogen. U moet beslissen als de time-out is ingesteld op de gewenste waarde in de client en een van de wijzigingen in de client stelt u de time-outwaarde of onderzoeken hoe u kunt de prestaties verbeteren van de bewerkingen in de storage-service, bijvoorbeeld door het optimaliseren van uw tabel query's of de grootte van berichten beperken voor time-outs van de client.

### <a name="metrics-show-an-increase-in-PercentNetworkError"></a>Een stijging zien in PercentNetworkError

Uw cijfers zien een toename in **PercentNetworkError** voor een van uw storage-services. De **PercentNetworkError** metric is een samenvoeging van de volgende criteria: **NetworkError**, **AnonymousNetworkError**en **SASNetworkError**. Deze treden op wanneer de storage-service een fout detecteert wanneer de client een aanvraag voor opslag.

De meest voorkomende oorzaak van deze fout is een client verbroken voordat er een time-out is verstreken in de storage-service. U moet de code op de client om te begrijpen waarom en wanneer de client de storage-service verbreekt onderzoeken. U kunt ook Wireshark, Microsoft Message Analyzer of Tcping bij het onderzoeken van problemen met de netwerkverbinding van de client. Deze hulpprogramma's worden beschreven in de [aanhangsels].

### <a name="the-client-is-receiving-403-messages"></a>De client ontvangt berichten HTTP 403 (niet toegestaan)

Als de clienttoepassing HTTP 403 (niet toegestaan) fouten genereren wordt, wordt er een waarschijnlijke oorzaak is dat de client een verlopen gedeelde toegang handtekening (SAS) wordt gebruikt wanneer een aanvraag voor de opslag (Hoewel andere mogelijke oorzaken klok scheeftrekken, ongeldige sleutels en lege headers zijn) verzonden. Als de sleutel van een verlopen SA's de oorzaak is, ziet u niet alle vermeldingen in de logboekgegevens van server-side opslag registreren. De volgende tabel ziet u een voorbeeld van de client-side logboek dat wordt gegenereerd door de Client-bibliotheek van opslag die dit probleem optreedt illustreert:

Bron|Gegevens opnemen|Gegevens opnemen|Aanvraag-id van client|Tekst bewerking
---|---|---|---|---
Microsoft.WindowsAzure.Storage|Informatie|3|85d077ab-...|Bewerking wordt gestart met de primaire locatie per locatie modus PrimaryOnly.
Microsoft.WindowsAzure.Storage|Informatie|3|85d077ab-...|Synchrone aanvraag voor het https://domemaildist.blob.core.windows.netazureimblobcontainer/blobCreatedViaSAS.txt?sv=2014-02-14 starten&amp;sr = c&amp;si mypolicy =&amp;sig = OFnd4Rd7z01fIvh % 2BmcR6zbudIH2F5Ikm % 2FyhNYZEmJNQ % 3D&amp;api versie 2014-02-14 =.
Microsoft.WindowsAzure.Storage|Informatie|3|85d077ab-...|Wachten op antwoord.
Microsoft.WindowsAzure.Storage|Waarschuwing|2|85d077ab-...|Uitzondering opgetreden tijdens het wachten op antwoord: de externe server heeft een fout geretourneerd: (403)...
Microsoft.WindowsAzure.Storage|Informatie|3|85d077ab-...|Het antwoord is ontvangen. Statuscode = 403, een aanvraag-ID = 9d67c64a-64ed-4b0d-9515-3b14bbcdc63d, Content-MD5 =, ETag =.
Microsoft.WindowsAzure.Storage|Waarschuwing|2|85d077ab-...|Uitzondering opgetreden tijdens de bewerking: de externe server heeft een fout geretourneerd: (403)...
Microsoft.WindowsAzure.Storage|Informatie|3 |85d077ab-...|Als de bewerking opnieuw moet worden uitgevoerd wordt gecontroleerd. Aantal herhaalpogingen = 0, HTTP-statuscode = 403, uitzondering = van de externe server heeft een fout geretourneerd: (403)...
Microsoft.WindowsAzure.Storage|Informatie|3|85d077ab-...|De volgende locatie is ingesteld op primair, op basis van de locatie.
Microsoft.WindowsAzure.Storage|Fout|1|85d077ab-...|Beleid opnieuw was niet mogelijk voor een nieuwe poging. Fout met de externe server heeft een fout geretourneerd: (403).

In dit scenario moet u onderzoeken waarom de SAS-token is verlopen voordat de client het token naar de server stuurt:

- Meestal moet u een begintijd bij het maken van een SAS-clients kunnen direct niet instellen. Als er klokje verschillen tussen de host genereren van de SA's met behulp van de huidige tijd en de opslag zijn, dan is het mogelijk voor de storage-service voor het ontvangen van een SA's dat is nog niet geldig.
- U moet een zeer korte verlooptijd niet instellen op een SAS. Nogmaals, klokje verschillen tussen de host voor het genereren van de SA's en de storage-service kunnen leiden tot een SAS blijkbaar eerder dan verwacht verlopen.
- Is de parameter versie in de sleutel SAS (bijvoorbeeld **sv 2015-04-05 =**) overeenkomen met de versie van de opslag Client-bibliotheek die u gebruikt? Wij raden aan dat u altijd de meest recente versie van de [Client-bibliotheek van opslag](https://www.nuget.org/packages/WindowsAzure.Storage/)gebruiken.
- Als u de sleutels van de toegang tot opslag, kan dit een bestaande SAS tokens ongeldig. Dit kan een probleem zijn als u SAS tokens met een lange verlooptijd voor clienttoepassingen cache genereren.

Als u de clientbibliotheek opslag gebruikt SAS-tokens te genereren, is het gemakkelijk om te bouwen van een geldige token. Als u de REST API van opslag en de tokens SAS met de hand maken u zorgvuldig moet lezen echter het onderwerp [Toegang delegeren met een gedeelde Access-handtekening](http://msdn.microsoft.com/library/azure/ee395415.aspx).

### <a name="the-client-is-receiving-404-messages"></a>De client ontvangt berichten HTTP 404 (niet gevonden)
Als de clienttoepassing een HTTP 404 (niet gevonden) weergegeven van de server ontvangt, betekent dit dat de poging van de client moet gebruiken (bijvoorbeeld een entiteit, tabel, blob, container of wachtrij) object niet in de storage-service bestaat. Er zijn een aantal mogelijke oorzaken, zoals:

- [De client of een ander proces eerder verwijderd van het object]
- [Een gedeeld Access handtekening (SAS) vergunning probleem]
- [Client-side JavaScript-code is niet gemachtigd voor toegang tot het object.]
- [Netwerkfout]

#### <a name="client-previously-deleted-the-object"></a>De client of een ander proces eerder verwijderd van het object
In scenario's waarin de client wordt geprobeerd te lezen, bijwerken of verwijderen van gegevens in een storage-service is het meestal vrij gemakkelijk om aan te duiden in de logboeken van de server van een voorafgaande bewerking die het desbetreffende object verwijderd uit de storage-service. De logboekgegevens worden zeer vaak dat een andere gebruiker of proces verwijderd van het object. In het logboek server-side Storage logboekregistratie weergeven het type bewerking en het gevraagde object sleutel kolommen wanneer een client een object verwijderd.

In het scenario waarbij een client probeert een object in te voegen, het mogelijk niet direct duidelijk waarom dit in een antwoord HTTP 404 (niet gevonden), resulteert gezien het feit dat de client wordt een nieuw object maken. Als de client maakt een blob moet de container blob te vinden als de client een bericht moet kunnen vinden van een wachtrij maakt en als de client is een rij toe te voegen moet het wel kunnen vinden in de tabel.

Kunt u de logboekbestanden van client-side van de clientbibliotheek opslag krijgen een meer gedetailleerde begrip van wanneer de client specifieke verzoeken aan de storage-service verzendt.

De volgende client-side logboek dat wordt gegenereerd door de clientbibliotheek opslag illustreert het probleem bij de client kan de container voor de blob het maakt niet vinden. Dit logboek bevat gedetailleerde gegevens van de volgende opslagbewerkingen:

Aanvraag-ID|Bewerking
---|---
07b26a5d-...|**DeleteIfExists** -methode voor het verwijderen van de blob-container. Houd er rekening mee dat deze bewerking een **HEAD** -verzoek om te controleren op de aanwezigheid van de container.
e2d06d78...|**CreateIfNotExists** -methode voor het maken van de blob-container. Houd er rekening mee dat deze bewerking een **HEAD** -verzoek waarmee wordt gecontroleerd op de aanwezigheid van de container. Het **hoofd** geeft als resultaat een 404-bericht, maar blijft.
de8b1c3c-...|**UploadFromStream** -methode voor het maken van de blob. **Plaats** verzoek mislukt met een 404-bericht

Vermeldingen in het beveiligingslogboek:

Aanvraag-ID |  Tekst bewerking
---|---
07b26a5d-...|Synchrone aanvraag voor het https://domemaildist.blob.core.windows.net/azuremmblobcontainer wordt gestart.
07b26a5d-...|StringToSign = HEAD...x-ms-client-request-id:07b26a5d-...x-ms-date:Tue, 03 Jun 2014 10:33:11 GMT.x-ms-version:2014-02-14./domemaildist/azuremmblobcontainer.restype:container.
07b26a5d-...|Wachten op antwoord.
07b26a5d-... | Het antwoord is ontvangen. Statuscode = 200, een aanvraag-ID = eeead849... Content-MD5 =, ETag = &quot;0x8D14D2DC63D059B&quot;.
07b26a5d-... | Response-headers zijn verwerkt, u doorgaat met de rest van de bewerking.
07b26a5d-... | Het hoofdgedeelte van de response downloaden.
07b26a5d-... | De bewerking is voltooid.
07b26a5d-... | Synchrone aanvraag voor het https://domemaildist.blob.core.windows.net/azuremmblobcontainer wordt gestart.
07b26a5d-... | StringToSign = DELETE...x-ms-client-request-id:07b26a5d-...x-ms-date:Tue, 03 Jun 2014 10:33:12 GMT.x-ms-version:2014-02-14./domemaildist/azuremmblobcontainer.restype:container.
07b26a5d-... | Wachten op antwoord.
07b26a5d-... | Het antwoord is ontvangen. Statuscode = 202 aanvraag-ID = 6ab2a4cf-..., Content-MD5 =, ETag =.
07b26a5d-... | Response-headers zijn verwerkt, u doorgaat met de rest van de bewerking.
07b26a5d-... | Het hoofdgedeelte van de response downloaden.
07b26a5d-... | De bewerking is voltooid.
e2d06d78-... | Asynchrone aanvraag om https://domemaildist.blob.core.windows.net/azuremmblobcontainer te starten.</td>
e2d06d78-... | StringToSign = HEAD...x-ms-client-request-id:e2d06d78-...x-ms-date:Tue, 03 Jun 2014 10:33:12 GMT.x-ms-version:2014-02-14./domemaildist/azuremmblobcontainer.restype:container.
e2d06d78-...| Wachten op antwoord.
de8b1c3c-... | Synchrone aanvraag voor het https://domemaildist.blob.core.windows.net/azuremmblobcontainer/blobCreated.txt wordt gestart.
de8b1c3c-... |  StringToSign = opslag... 64.qCmF+TQLPhq/YYK50mP9ZQ==...x-MS-BLOB-type:BlockBlob.x-MS-Client-Request-id:de8b1c3c-...x-MS-Date:TUE, 03 Jun 2014 10:33:12 GMT.x-ms-version:2014-02-14./domemaildist/azuremmblobcontainer/blobCreated.txt.
de8b1c3c-... | Voorbereiden voor het schrijven van gegevens.
e2d06d78-... | Uitzondering opgetreden tijdens het wachten op antwoord: de externe server heeft een fout geretourneerd: (404) niet gevonden...
e2d06d78-... | Het antwoord is ontvangen. Statuscode 404, een aanvraag-ID = 353ae3bc..., Content-MD5 = =, ETag =.
e2d06d78-... | Response-headers zijn verwerkt, u doorgaat met de rest van de bewerking.
e2d06d78-... | Het hoofdgedeelte van de response downloaden.
e2d06d78-... | De bewerking is voltooid.
e2d06d78-... | Asynchrone aanvraag om https://domemaildist.blob.core.windows.net/azuremmblobcontainer te starten.
e2d06d78-...|StringToSign = opslag... 0...x-MS-Client-Request-id:e2d06d78-...x-MS-Date:TUE, 03 Jun 2014 10:33:12 GMT.x-ms-version:2014-02-14./domemaildist/azuremmblobcontainer.restype:container.
e2d06d78-... | Wachten op antwoord.
de8b1c3c-... | Gegevens schrijven.
de8b1c3c-... | Wachten op antwoord.
e2d06d78-... | Uitzondering opgetreden tijdens het wachten op antwoord: de externe server heeft een fout geretourneerd: Conflict (409)...
e2d06d78-... | Het antwoord is ontvangen. Statuscode = 409, een aanvraag-ID = c27da20e-..., Content-MD5 =, ETag =.
e2d06d78-... | Fout antwoord instantie downloaden.
de8b1c3c-... | Uitzondering opgetreden tijdens het wachten op antwoord: de externe server heeft een fout geretourneerd: (404) niet gevonden...
de8b1c3c-... | Het antwoord is ontvangen. Statuscode 404, een aanvraag-ID = 0eaeab3e..., Content-MD5 = =, ETag =.
de8b1c3c-...| Uitzondering opgetreden tijdens de bewerking: de externe server heeft een fout geretourneerd: (404) niet gevonden...
de8b1c3c-... | Beleid opnieuw was niet mogelijk voor een nieuwe poging. Fout met de externe server heeft een fout geretourneerd: (404) niet gevonden...
e2d06d78-... | Beleid opnieuw was niet mogelijk voor een nieuwe poging. Fout met de externe server heeft een fout geretourneerd: Conflict (409)...

In dit voorbeeld bevat het logboek voor de client aanvragen via de methode **CreateIfNotExists** (aanvraag-id e2d06d78...) met de verzoeken van de methode **UploadFromStream** wordt interleaving (de8b1c3c...); Dit gebeurt omdat de clienttoepassing wordt asynchroon deze methoden aanroept. U kunt de asynchrone code op de client om ervoor te zorgen dat de container wordt gemaakt voordat u probeert te uploaden van gegevens naar een blob in die container moet wijzigen. In het ideale geval moet u alle uw containers van tevoren maken.

#### <a name="SAS-authorization-issue"></a>Een gedeeld Access handtekening (SAS) vergunning probleem

Als de clienttoepassing wordt geprobeerd om met een SAS-sleutel die niet de vereiste machtigingen voor de bewerking, de storage-service een bericht HTTP 404 (niet gevonden) geretourneerd naar de client. Tegelijkertijd is het ook ziet u een andere waarde dan nul voor **SASAuthorizationError** in de statistieken.

In de volgende tabel ziet u een voorbeeld aan de serverzijde bericht uit het logboekbestand opslag registreren:

<table>
  <tr>
    <td>Begintijd van de aanvraag</td>
    <td>2014-05-30T06:17:48.4473697Z</td>
  </tr>
  <tr>
    <td>Bewerkingstype</td>
    <td>GetBlobProperties</td>
  </tr>
  <tr>
    <td>De status van aanvraag</td>
    <td>SASAuthorizationError</td>
  </tr>
  <tr>
    <td>HTTP-statuscode</td>
    <td>404</td>
  </tr>
  <tr>
    <td>Verificatietype</td>
    <td>SAS</td>
  </tr>
  <tr>
    <td>Type service</td>
    <td>BLOB</td>
  </tr>
  <tr>
    <td>Aanvraag-URL</td>
    <td>
    https://domemaildist.BLOB.Core.Windows.NET/azureimblobcontainer/blobCreatedViaSAS.txt?sv=2014-02-14&amp;amp; sr = c&amp;amp; si mypolicy =&amp;amp; sig XXXXX =&amp;amp; api versie 2014-02-14 =&amp;amp;</td>
  </tr>
  <tr>
    <td>Koptekst voor aanvraag-id</td>
    <td>a1f348d5-8032-4912-93ef-b393e5252a3b</td>
  </tr>
  <tr>
    <td>Aanvraag-ID van client</td>
    <td>2d064953-8436-4ee0-aa0c-65cb874f7929</td>
  </tr>
</table>

U moet onderzoeken waarom de clienttoepassing probeert uit te voeren een bewerking die er geen machtigingen heeft voor.

#### <a name="JavaScript-code-does-not-have-permission"></a>Client-side JavaScript-code is niet gemachtigd voor toegang tot het object.

Als u een JavaScript-client en de storage-service HTTP 404 berichten retourneert, die u voor de volgende fouten met JavaScript in de browser controleren:

    SEC7120: Origin http://localhost:56309 not found in Access-Control-Allow-Origin header.
    SCRIPT7002: XMLHttpRequest: Network Error 0x80070005, Access is denied.

> [AZURE.NOTE] Kunt u de F12-ontwikkelprogramma's in Internet Explorer voor het traceren van berichten uitgewisseld tussen de browser en de storage-service als u client-side JavaScript problemen wilt oplossen.

Deze fouten treden op omdat de webbrowser de beveiligingsbeperkingen voor [dezelfde oorsprong beleid](http://www.w3.org/Security/wiki/Same_Origin_Policy) waarmee wordt voorkomen een webpagina in een ander domein dan het domein dat de pagina is afkomstig implementeert dat van een API wordt aangeroepen.

De JavaScript als probleem wilt verhelpen, kunt u Cross-oorsprong Resource Sharing (CORS) voor de client toegang heeft tot de storage-service. Zie [Cross-oorsprong Resource Sharing (CORS) ondersteuning voor Azure Storage-Services](http://msdn.microsoft.com/library/azure/dn535601.aspx)voor meer informatie.

In het volgende voorbeeld ziet u hoe de blob-service zodat JavaScript uitvoeren in het domein Contoso toegang te krijgen tot een blob in de blob storage-service te configureren:

    CloudBlobClient client = new CloudBlobClient(blobEndpoint, new StorageCredentials(accountName, accountKey));
    // Set the service properties.
    ServiceProperties sp = client.GetServiceProperties();
    sp.DefaultServiceVersion = "2013-08-15";
    CorsRule cr = new CorsRule();
    cr.AllowedHeaders.Add("*");
    cr.AllowedMethods = CorsHttpMethods.Get | CorsHttpMethods.Put;
    cr.AllowedOrigins.Add("http://www.contoso.com");
    cr.ExposedHeaders.Add("x-ms-*");
    cr.MaxAgeInSeconds = 5;
    sp.Cors.CorsRules.Clear();
    sp.Cors.CorsRules.Add(cr);
    client.SetServiceProperties(sp);

#### <a name="network-failure"></a>Netwerkfout

Verloren pakketten kunnen in sommige gevallen leiden tot de storage-service HTTP 404 berichten terugsturen naar de client. Bijvoorbeeld wanneer uw clienttoepassing van een entiteit uit de service tabel verwijderen ziet u de client genereert een opslag uitzondering melden een ' HTTP 404 (niet gevonden) ' statusbericht van de tabel service. Wanneer u de tabel in de tabel storage-service onderzoeken, ziet u dat de service de entiteit heeft verwijderd zoals gevraagd.

De details van uitzondering in de client de aanvraag-id (7e84f12d...) die wordt toegewezen door de tabel service voor de aanvraag opnemen: u kunt deze informatie de aanvraaggegevens in de logboeken van server-side storage vinden door te zoeken in de **koptekst van een aanvraag-id** -kolom in het logboekbestand. U kunt ook de maatstaven te identificeren wanneer u fouten zoals dit optreden en zoek de logboekbestanden die zijn gebaseerd op de tijd dat de cijfers deze fout vastgelegd. Deze vermelding bevat dat het verwijderen is mislukt met een statusbericht 'HTTP-(fout 404) Client andere fout'. De dezelfde vermelding bevat ook de aanvraag-id gegenereerd door de client in de kolom **client-aanvraag-id** (813ea74f...).

De server-side logboek bevat ook een post met dezelfde **client-aanvraag-id-** waarde (813ea74f...) voor een geslaagde bewerking van dezelfde entiteit en van dezelfde client. Deze geslaagde bewerking plaatsgevonden zeer kort voordat de mislukte aanvraag verwijderen.

De meest waarschijnlijke oorzaak van dit scenario is dat de client voor de entiteit een verzoek tot verwijdering naar de tabel service is geslaagd, maar geen bevestiging hebt ontvangen van de server (bijvoorbeeld als gevolg van een tijdelijk netwerkprobleem) verzonden. De client vervolgens automatisch opnieuw geprobeerd de bewerking (met behulp van dezelfde **client-aanvraag-id**) en deze poging is mislukt omdat de entiteit was al verwijderd.

Als dit probleem regelmatig optreedt, moet u onderzoeken waarom de client ontvangen bevestigingen van de tabel service is mislukt. Als het probleem onderbroken wordt, moet u overlappen de fout 'HTTP (404) niet gevonden' en het aanmelden bij de client, maar dat de client om door te gaan.

### <a name="the-client-is-receiving-409-messages"></a>De client ontvangt berichten HTTP 409 (Conflict)

De volgende tabel ziet u een uittreksel uit het logboek van de server-side voor twee client bewerkingen: **DeleteIfExists** onmiddellijk gevolgd door **CreateIfNotExists** met dezelfde naam van de container blob. Merk op dat elke client bewerking resulteert in twee aanvragen naar de server verzonden, eerst een **GetContainerProperties** verzoek om te controleren of de container bestaat, gevolgd door de aanvraag van **DeleteContainer** of **CreateContainer** .

Tijdstempel|Bewerking|Resultaat|Naam van container|Aanvraag-id van client
---|---|---|---|---
05:10:13.7167225|GetContainerProperties|200|mmcont|c9f52c89-...
05:10:13.8167325|DeleteContainer|202|mmcont|c9f52c89-...
05:10:13.8987407|GetContainerProperties|404|mmcont|bc881924-...
05:10:14.2147723|CreateContainer|409|mmcont|bc881924-...

De code in de clienttoepassing verwijderd en vervolgens onmiddellijk opnieuw een blob-container met dezelfde naam gemaakt: de **CreateIfNotExists** -methode (clientaanvraag ID bc881924-...) uiteindelijk mislukken met het foutbericht HTTP 409 (Conflict). Wanneer een client verwijdert blob containers, tabellen of er een korte periode vóór de naam van de is wachtrijen weer beschikbaar.

De clienttoepassing moet container unieke namen gebruiken wanneer nieuwe eindverpakking wordt gemaakt als het patroon verwijderen/opnieuw gebruikt wordt.

### <a name="metrics-show-low-percent-success"></a>Lage PercentSuccess zien of analytics logitems zijn transacties met de status van ClientOtherErrors

De metric van de **PercentSuccess** wordt het percentage van de bewerkingen die aangebracht op basis van de HTTP-statuscode zijn vastgelegd. Transacties met de statuscodes van 2XX tellen als geslaagd dat bewerkingen met statuscodes in 3XX, 4XX en 5XX bereiken als mislukt worden geteld en verlaagt u de metrische waarde **PercentSucess** . Deze bewerkingen zijn in de logboekbestanden van de server-side storage met een status van de **ClientOtherErrors**vastgelegd.

Het is belangrijk te weten dat deze bewerkingen zijn voltooid en daarom geen invloed op andere criteria zoals beschikbaarheid. Enkele voorbeelden van bewerkingen die met succes worden uitgevoerd, maar dat kan leiden tot mislukte HTTP-statuscodes zijn:
- **ResourceNotFound** (Niet vinden 404), bijvoorbeeld van een GET-verzoek naar een blob die niet bestaat.
- **ResouceAlreadyExists** (Conflict 409), bijvoorbeeld bij een **CreateIfNotExist** waarin de resource al bestaat.
- **ConditionNotMet** (Niet gewijzigd 304), bijvoorbeeld van een voorwaardelijke bewerking, zoals wanneer een client verzendt een **ETag** -waarde en een header van de HTTP- **If-None-Match** voor het aanvragen van een afbeelding alleen als deze is bijgewerkt sinds de laatste bewerking.

Hier vindt u een lijst met veelvoorkomende REST API foutcodes die resulteren in de storage-services op de pagina [Algemene foutcodes van REST API](http://msdn.microsoft.com/library/azure/dd179357.aspx).

### <a name="capacity-metrics-show-an-unexpected-increase"></a>Capaciteit zien een onverwachte toename in het gebruik van opslag capaciteit


Als er plotselinge, onverwachte wijzigingen in het gebruik van de capaciteit in uw account opslag kunt u onderzoeken de redenen door eerst te zoeken naar de maatstaven van beschikbaarheid; een toename van het aantal mislukte verwijderen aanvragen zou kunnen tot een verhoging van de blob-opslag als specifieke opschoonbewerkingen toepassingen die u mogelijk hebt verwacht worden vrijmaken ruimte werkt niet leiden zoals verwacht (bijvoorbeeld omdat de SAS-tokens die worden gebruikt voor het vrijmaken van schijfruimte zijn verlopen).

### <a name="you-are-experiencing-unexpected-reboots"></a>Ondervindt u onverwacht opnieuw opstarten van Azure virtuele Machines die een groot aantal gekoppelde VHD 's

Als een Azure Virtual Machine (VM) een groot aantal gekoppelde VHD's die zich in dezelfde opslag account is, kan u de schaalbaarheid doelen voor een individuele opslag account waardoor de VM niet overschrijden. Controleer de minuut maatstaven voor de opslag (**TotalRequests**/**TotalIngress**/**TotalEgress**) pieken die hoger zijn dan de schaalbaarheid doelen voor een account voor opslag. Zie de sectie '[zien een toename van de PercentThrottlingError]' voor hulp bij het bepalen of procesbeperking is opgetreden op uw account voor opslag.

In het algemeen elke afzonderlijke input of output bewerking op een VHD vanuit een virtuele Machine wordt omgezet in bewerkingen of **Pagina plaatsen** **Krijgen** op de onderliggende pagina-blob. Daarom kunt u de geschatte IOP's voor uw omgeving te optimaliseren, hoeveel VHD's u kunt hebben in een opslag op één account op basis van het specifieke gedrag van uw toepassing. Met schijven van meer dan 40 in een opslag voor één account wordt niet aanbevolen. Zie [Azure opslag schaalbaarheid en prestaties, doelen](storage-scalability-targets.md) voor meer informatie over de huidige schaalbaarheid doelen voor opslag, met name de totale verzoek rente en de totale bandbreedte voor het type opslag account die u gebruikt.
Als u de schaalbaarheid doelen voor uw opslag account overschrijdt, plaatst u de VHD in meerdere verschillende opslag rekeningen ter vermindering van de activiteit in elke rekening.

### <a name="your-issue-arises-from-using-the-storage-emulator"></a>Uw probleem voortvloeit uit het gebruik van de opslag-emulator voor ontwikkel- of testomgeving

U meestal de emulator opslag tijdens het ontwikkelen en testen om te voorkomen dat de behoefte aan een account Azure opslag. De algemene problemen die optreden kunnen wanneer u de emulator opslag zijn:

- [Functie 'X' werkt niet in de emulator opslag]
- [Foutbericht 'de waarde voor een van de HTTP-headers is niet de juiste indeling' wanneer u de emulator opslag]
- [De opslag-emulator wordt uitgevoerd beheerdersrechten vereist zijn]

#### <a name="feature-X-is-not-working"></a>Functie 'X' werkt niet in de emulator opslag

De emulator opslag ondersteunt alle functies van de Azure opslagservices zoals de file service niet. Zie [werken met de Emulator Azure opslag voor ontwikkeling en testen](storage-use-emulator.md)voor meer informatie.

Gebruik de Azure storage-service in de cloud voor functies die niet worden ondersteund door de emulator opslag.

#### <a name="error-HTTP-header-not-correct-format"></a>Foutbericht 'de waarde voor een van de HTTP-headers is niet de juiste indeling' wanneer u de emulator opslag

U test uw toepassing die u gebruikt de clientbibliotheek opslag tegen de lokale opslag-emulator en methode aanroept, zoals **CreateIfNotExists** mislukken met het foutbericht "de waarde voor een van de HTTP-headers is niet de juiste indeling." Dit geeft aan dat de versie van de opslag-emulator die u met de versie van de opslag client-bibliotheek die u gebruikt niet ondersteunt. De header **x-ms-versie** de clientbibliotheek opslag toegevoegd aan alle aanvragen die worden aangebracht. Als de waarde in de header **x-ms-versie** van de emulator opslag niet herkent, wordt het verzoek geweigerd.

De opslag bibliotheek Client zich aanmeldt kunt u de waarde van de **header x-ms-versie** die het verzendt. Ook ziet u de waarde van de **header x-ms-versie** als u Fiddler gebruikt voor het traceren van de verzoeken van de clienttoepassing.

In dit scenario treedt meestal op als u installeert en de meest recente versie van de opslag-clientbibliotheek gebruikt zonder bij te werken van de opslag-emulator. U moet Installeer de nieuwste versie van de emulator opslag of cloud-opslag in plaats van de emulator gebruikt voor ontwikkel- en testomgeving.

#### <a name="storage-emulator-requires-administrative-privileges"></a>De opslag-emulator wordt uitgevoerd beheerdersrechten vereist zijn

Wordt u gevraagd om beheerdersreferenties tijdens het uitvoeren van de emulator opslag. Dit gebeurt alleen wanneer u bij het initialiseren van de opslag-emulator voor de eerste keer. Nadat u de emulator opslag hebt geïnitialiseerd, hoeft u geen beheerdersbevoegdheden opnieuw uitvoeren.

Zie [werken met de Emulator Azure opslag voor ontwikkeling en testen](storage-use-emulator.md)voor meer informatie. Opmerking u kunt ook de opslag-emulator in Visual Studio, die ook beheerdersrechten moet initialiseren.

### <a name="you-are-encountering-problems-installing-the-Windows-Azure-SDK"></a>U ondervindt problemen bij het installeren van de Azure SDK voor .NET

Wanneer u de SDK hebt geïnstalleerd, mislukt de bewerking probeert te installeren van de emulator opslag op de lokale computer. In het installatielogboek bevat een van de volgende berichten:

- CAQuietExec: Fout: geen toegang tot SQL-exemplaar
- CAQuietExec: Foutbericht: kan geen database maken

De oorzaak is een probleem met de bestaande installatie van LocalDB. Standaard gebruikt de emulator opslag LocalDB om gegevens wanneer het simuleert de Azure storage-services. U kunt uw LocalDB-exemplaar herstellen door de volgende opdrachten in een opdrachtpromptvenster voordat u probeert te installeren van de SDK.

    sqllocaldb stop v11.0
    sqllocaldb delete v11.0
    delete %USERPROFILE%\WAStorageEmulatorDb3*.*
    sqllocaldb create v11.0

De opdracht **delete** verwijdert alle oude databasebestanden uit eerdere installaties van de opslag-emulator.

### <a name="you-have-a-different-issue-with-a-storage-service"></a>U hebt een ander probleem met storage-service

Als de voorgaande secties voor het oplossen van problemen het probleem met een storage-service niet opneemt, moet u de volgende benadering voor de diagnose en het oplossen van het probleem vast.

- Controleer uw statistieken te zien als er een wijziging van het verwachte gedrag van base line. Uit de cijfers, is het mogelijk om te bepalen of het probleem tijdelijk of permanent is en welke opslagbewerkingen het probleem beïnvloedt.
- U kunt de metrische gegevens kunt u zoeken op de server-side logboekgegevens voor meer gedetailleerde informatie over eventuele fouten die optreden. Deze informatie kunt u het probleem oplossen.
- Als de informatie in de logboeken van de server niet voldoende is voor het oplossen van het probleem is, kunt u de client-side Storage clientbibliotheek Logboeken bij het onderzoeken van het gedrag van de clienttoepassing en hulpmiddelen zoals Fiddler, Wireshark en Microsoft Message Analyzer om uw netwerk te onderzoeken.

Zie voor meer informatie over het gebruik van Fiddler "[aanhangsel 1: Fiddler voor het vastleggen van HTTP- en HTTPS-verkeer met]."

Zie voor meer informatie over het gebruik van Wireshark '[aanhangsel 2: Wireshark netwerkverkeer vastleggen met]. "

Zie voor meer informatie over het gebruik van Microsoft Message Analyzer '[aanhangsel 3: Analyzer van Microsoft Message netwerkverkeer vastleggen met]. "

## <a name="appendices"></a>Aanhangsels

De aanhangsels beschrijven de verschillende hulpprogramma's die u van pas komen kan bij het oplossen van en het oplossen van problemen met Azure opslag (en andere diensten). Deze tools maken geen deel uit van Azure opslag en andere producten van andere leveranciers. De hulpprogramma's die worden besproken in deze bijlagen worden niet gedekt door de ondersteuningsovereenkomst met Microsoft Azure of Azure opslag als zodanig en daarom als onderdeel van het evaluatieproces onderzoekt u de licentie- en ondersteuningsbeleid beschikbare opties van de aanbieders van deze hulpprogramma's.

### <a name="appendix-1"></a>Aanhangsel 1: Fiddler gebruiken voor het vastleggen van HTTP- en HTTPS-verkeer

[Fiddler](http://www.telerik.com/fiddler) is een handig hulpprogramma voor het analyseren van HTTP- en HTTPS-verkeer tussen de clienttoepassing en de Azure storage-service die u gebruikt.

> [AZURE.NOTE] Fiddler kan decoderen HTTPS-verkeer; Lees de documentatie Fiddler zorgvuldig om te begrijpen hoe dit gebeurt en inzicht krijgen in implicaties van de beveiliging.

Deze bijlage bevat een kort overzicht van Fiddler voor het vastleggen van verkeer tussen de lokale computer waarop u Fiddler hebt geïnstalleerd en de Azure storage-services configureren.

Wanneer u Fiddler hebt gestart, begint het vastleggen van HTTP- en HTTPS-verkeer op uw lokale computer. Hier volgen enkele handige opdrachten voor het beheren van Fiddler:

- Stop en start het vastleggen van verkeer. Ga naar het **bestand** in het hoofdmenu en klik vervolgens op **Verkeer opnemen** als u wilt opnemen in en uit schakelen.
- Opgenomen verkeersgegevens opslaan. Ga naar het **bestand**, klikt u op **Opslaan**en klik vervolgens op **Alle sessies**in het hoofdmenu: Hiermee kunt u het verkeer in een archiefbestand sessie opslaan. U kunt een sessie archief later voor analyse opnieuw laden of verzenden als een verzoek om ondersteuning van Microsoft.

Als u wilt beperken de hoeveelheid verkeer die Fiddler worden vastgelegd, kunt u filters die u kunt op het tabblad **Filters configureren** . De volgende schermafbeelding ziet u een filter waarmee alleen het verkeer dat wordt verzonden naar het eindpunt **contosoemaildist.table.core.windows.net** opslag worden vastgelegd:

![][5]

### <a name="appendix-2"></a>Aanhangsel 2: Wireshark netwerkverkeer vastleggen met behulp

[Wireshark](http://www.wireshark.org/) is een programma voor netwerkprotocolanalyse waarmee u een pakket gedetailleerde informatie weergeven voor een groot aantal netwerkprotocollen.

De volgende procedure wordt beschreven hoe u pakket gedetailleerde informatie voor het verkeer vanaf de lokale computer vastleggen waar u Wireshark geïnstalleerd met de service van de tabel in uw account Azure opslag.

1.  Wireshark starten op de lokale computer.
2.  Selecteer in de sectie **starten** , het lokale netwerk of interfaces die zijn verbonden met het internet.
3.  Klik op **Opties voor vastleggen**.
4.  Een filter toevoegen aan het tekstvak van het **Opnamefilter** . **Host contosoemaildist.table.core.windows.net** wordt bijvoorbeeld Wireshark om vast te leggen alleen pakketten die worden verzonden naar of uit de tabel service-eindpunt in de account **contosoemaildist** opslag configureren. Bekijk de [volledige lijst met Filters vastleggen](http://wiki.wireshark.org/CaptureFilters).

    ![][6]

5.  Klik op **Start**. Wireshark vastleggen wordt nu alle pakketten verzenden naar of uit de tabel service-eindpunt als u de clienttoepassing op de lokale computer gebruiken.
6.  Wanneer u klaar bent, klik op **vastleggen** en **stoppen**in het hoofdmenu.
7.  Klik op **bestand** en klik vervolgens op **Opslaan**in het hoofdmenu om de opgenomen gegevens opslaan in een bestand Wireshark vastleggen.

WireShark zal markeren van eventuele fouten die in het venster **packetlist voorkomen** . U kunt ook het informatievenster **Expert** (Klik op **analyseren**en vervolgens **Info Expert**) geeft een overzicht van de fouten en waarschuwingen.

![][7]

Ook hebt u de TCP-gegevens weergeven als de toepassingsobjectlaag ziet het met de rechtermuisknop op de TCP-gegevens en selecteert u **TCP-Stream volgen**. Dit is vooral handig als u uw dump zonder een opnamefilter vastgelegd. Zie voor meer informatie [na TCP-Streams] (http://www.wireshark.org/docs/wsug_html_chunked/ChAdvFollowTCPSection.html).

![][8]

> [AZURE.NOTE] Zie voor meer informatie over het gebruik van Wireshark [Wireshark gebruikers gids](http://www.wireshark.org/docs/wsug_html_chunked).

### <a name="appendix-3"></a>Aanhangsel 3: Microsoft Message Analyzer netwerkverkeer vastleggen met behulp

U kunt Microsoft Message Analyzer gebruiken voor het vastleggen van HTTP- en HTTPS-verkeer op een soortgelijke manier aan Fiddler en netwerkverkeer vastleggen op een vergelijkbare manier aan Wireshark.

#### <a name="configure-a-web-tracing-session-using-microsoft-message-analyzer"></a>Configureer een websessie tracering met behulp van Microsoft Message Analyzer

Als u wilt configureren een websessie tracering voor HTTP- en HTTPS-verkeer met behulp van Microsoft Message Analyzer, de toepassing Microsoft Message Analyzer wordt uitgevoerd en klik vervolgens in het menu **bestand** op **Capture/Trace**. Selecteer in de lijst van beschikbare trace scenario's **Web Proxy**. Klik in het deelvenster **Trace Scenario configuratie** in het tekstvak **HostnameFilter** toevoegen de namen van de eindpunten van de opslag (u kunt deze namen opzoeken in de [Portal Azure](https://portal.azure.com)). Als de naam van uw account Azure opslag **contosodata**is, moet u het volgende toevoegen aan het tekstvak **HostnameFilter** :

    contosodata.blob.core.windows.net contosodata.table.core.windows.net contosodata.queue.core.windows.net

> [AZURE.NOTE] Een spatie van elkaar scheidt de hostnamen.

Wanneer u klaar bent om te beginnen met het verzamelen van traceergegevens, klikt u op de knop **Start met** .

Zie [Microsoft-PEF-WebProxy-Provider](http://technet.microsoft.com/library/jj674814.aspx)voor meer informatie over het traceren van Microsoft Message Analyzer **Web Proxy** .

De ingebouwde **Web Proxy** -tracering in Microsoft Message Analyzer is gebaseerd op Fiddler; het kan vastleggen van HTTPS-verkeer van client-side en ongecodeerde HTTPS-berichten worden weergegeven. De **Web Proxy** -trace werkt door een lokale proxy voor alle HTTP- en HTTPS-verkeer die deze toegang tot de niet-gecodeerde berichten geeft te configureren.

#### <a name="diagnosing-network-issues-using-microsoft-message-analyzer"></a>Diagnose van netwerkproblemen met met Microsoft Message Analyzer

De trace bericht Analyzer van Microsoft **Web Proxy** gebruiken voor het vastleggen van details van het HTTP/HTTPs-verkeer tussen de clienttoepassing en de storage-service, ook kunt u de ingebouwde **Lokale Link Layer** trace packet Netwerkgegevens vastleggen. Dit kunt u voor het vastleggen van gegevens, vergelijkbaar met die welke met Wireshark vastleggen en analyseren, zoals problemen met netwerk verloren pakketten.

De volgende schermafdruk toont een voorbeeld van **Lokale Link Layer** trace met bepaalde **informatieve** berichten in de kolom **DiagnosisTypes** . Te klikken op een pictogram in de kolom **DiagnosisTypes** bevat de details van het bericht. In dit voorbeeld wordt de server opnieuw is verzonden bericht #305 omdat er geen bevestiging is ontvangen van de client:

![][9]

Wanneer u de traceersessie in Microsoft Message Analyzer maakt, kunt u filters om de hoeveelheid ruis in de trace. Klik op de koppeling **Configure** naast **Microsoft Windows-NDIS PacketCapture**op de **Capture / Trace** pagina waar u de tracering definiëren. Het volgende screenshot toont een configuratie waarmee TCP-verkeer voor de IP-adressen van de drie storage-services:

![][10]

Zie [Microsoft-PEF-NDIS-PacketCapture-Provider](http://technet.microsoft.com/library/jj659264.aspx)voor meer informatie over het traceren van Microsoft Message Analyzer lokale Link-laag.

### <a name="appendix-4"></a>Aanhangsel 4: Met behulp van Excel statistieken bekijken en gegevens vastleggen

Veel hulpmiddelen kunt u downloaden van de opslag metrische gegevens van Azure tabelopslag gescheiden indeling waarmee u gemakkelijk de gegevens te laden in Excel voor het weergeven en analyseren. Registreren van gegevens van Azure blob-opslag opslag wordt al opgemaakt met een scheidingsteken die u in Excel laden kunt. Echter, moet u desbetreffende kolomkoppen op basis van de informatie aan [Opslag Analytics Log Format](http://msdn.microsoft.com/library/azure/hh343259.aspx) en [Opslag Analytics Metrics tabelschema](http://msdn.microsoft.com/library/azure/hh343264.aspx)toevoegen.

De opslag registreren om gegevens te importeren in Excel nadat u het downloaden van blob-opslag:

- Klik op de **Tekst**in het menu **Data** .
- Ga naar het logboekbestand dat u wilt weergeven en klik op **importeren**.
- Selecteer in stap 1 van de **Wizard Tekst importeren**, **gescheiden**.

In stap 1 van de **Wizard Tekst importeren**, **puntkomma** als scheidingsteken alleen selecteren en kiest u dubbel-quote als het **Tekstscheidingsteken**. Klik op **Voltooien** en kies waar u de gegevens in de werkmap geplaatst.

### <a name="appendix-5"></a>Aanhangsel 5: Met inzichten van toepassing voor Visual Studio teamservices controleren

Kun je de functie Application inzichten voor Visual Studio Team Services als onderdeel van uw prestaties en beschikbaarheid controleren. Dit hulpprogramma kan:

- Zorg ervoor dat uw webservice beschikbaar is en reageert. Of uw app is een website of de app van een apparaat dat gebruikmaakt van een webservice, kan het uw URL testen om de paar minuten vanaf locaties over de hele wereld en laten het u weten als er een probleem is.
- Snel eventuele problemen met de prestaties of uitzonderingen in uw webservice te analyseren. Ontdek als de CPU of andere bronnen worden uitgerekt, kunt u stacktraces van uitzonderingen en gemakkelijk zoeken door middel van logboek sporen. Als de prestaties van de app onder aanvaardbare grenzen, kunnen we u een e-mail sturen. U kunt zowel .NET en Java webservices controleren.

U vindt meer informatie op [Wat inzichten van toepassing is?](../application-insights/app-insights-overview.md).

<!--Anchors-->
[Inleiding]: #introduction
[De rangschikking van deze handleiding]: #how-this-guide-is-organized

[De storage-service controleren]: #monitoring-your-storage-service
[De status van de service controleren]: #monitoring-service-health
[Toezichthoudende capaciteit]: #monitoring-capacity
[Beschikbaarheid controleren]: #monitoring-availability
[Prestaties controleren]: #monitoring-performance

[Diagnose van problemen met opslag]: #diagnosing-storage-issues
[Service gezondheidsvraagstukken]: #service-health-issues
[Problemen met prestaties]: #performance-issues
[Fouten opsporen]: #diagnosing-errors
[Problemen met opslag emulator]: #storage-emulator-issues
[Opslag-logboekprogramma 's]: #storage-logging-tools
[Met behulp van hulpprogramma's voor logboekregistratie]: #using-network-logging-tools

[End-to-end tracering]: #end-to-end-tracing
[Logboekgegevens correleren]: #correlating-log-data
[Aanvraag-ID van client]: #client-request-id
[Aanvraag-ID van server]: #server-request-id
[Tijdstempels]: #timestamps

[Richtlijnen voor het oplossen van problemen]: #troubleshooting-guidance
[AverageE2ELatency met hoge en lage AverageServerLatency zien]: #metrics-show-high-AverageE2ELatency-and-low-AverageServerLatency
[Lange wachttijden op de client voordoet, maar weinig AverageE2ELatency en lage AverageServerLatency zien]: #metrics-show-low-AverageE2ELatency-and-low-AverageServerLatency
[Hoge AverageServerLatency zien]: #metrics-show-high-AverageServerLatency
[Er onverwachte vertragingen in de bezorging van berichten in een wachtrij]: #you-are-experiencing-unexpected-delays-in-message-delivery

[Een stijging zien in PercentThrottlingError]: #metrics-show-an-increase-in-PercentThrottlingError
[Tijdelijke toename van de PercentThrottlingError]: #transient-increase-in-PercentThrottlingError
[Permanente toename van de PercentThrottlingError fout]: #permanent-increase-in-PercentThrottlingError
[Een stijging zien in PercentTimeoutError]: #metrics-show-an-increase-in-PercentTimeoutError
[Een stijging zien in PercentNetworkError]: #metrics-show-an-increase-in-PercentNetworkError

[De client ontvangt berichten HTTP 403 (niet toegestaan)]: #the-client-is-receiving-403-messages
[De client ontvangt berichten HTTP 404 (niet gevonden)]: #the-client-is-receiving-404-messages
[De client of een ander proces eerder verwijderd van het object]: #client-previously-deleted-the-object
[Een gedeeld Access handtekening (SAS) vergunning probleem]: #SAS-authorization-issue
[Client-side JavaScript-code is niet gemachtigd voor toegang tot het object.]: #JavaScript-code-does-not-have-permission
[Netwerkfout]: #network-failure
[De client ontvangt berichten HTTP 409 (Conflict)]: #the-client-is-receiving-409-messages

[Lage PercentSuccess zien of analytics logitems zijn transacties met de status van ClientOtherErrors]: #metrics-show-low-percent-success
[Capaciteit zien een onverwachte toename in het gebruik van opslag capaciteit]: #capacity-metrics-show-an-unexpected-increase
[Ondervindt u onverwacht opnieuw opstarten van de virtuele Machines met een groot aantal gekoppelde VHD 's]: #you-are-experiencing-unexpected-reboots
[Uw probleem voortvloeit uit het gebruik van de opslag-emulator voor ontwikkel- of testomgeving]: #your-issue-arises-from-using-the-storage-emulator
[Functie 'X' werkt niet in de emulator opslag]: #feature-X-is-not-working
[Foutbericht 'de waarde voor een van de HTTP-headers is niet de juiste indeling' wanneer u de emulator opslag]: #error-HTTP-header-not-correct-format
[De opslag-emulator wordt uitgevoerd beheerdersrechten vereist zijn]: #storage-emulator-requires-administrative-privileges
[U ondervindt problemen bij het installeren van de Azure SDK voor .NET]: #you-are-encountering-problems-installing-the-Windows-Azure-SDK
[U hebt een ander probleem met storage-service]: #you-have-a-different-issue-with-a-storage-service

[Aanhangsels]: #appendices
[Aanhangsel 1: Fiddler gebruiken voor het vastleggen van HTTP- en HTTPS-verkeer]: #appendix-1
[Aanhangsel 2: Wireshark netwerkverkeer vastleggen met behulp]: #appendix-2
[Aanhangsel 3: Microsoft Message Analyzer netwerkverkeer vastleggen met behulp]: #appendix-3
[Aanhangsel 4: Met behulp van Excel statistieken bekijken en gegevens vastleggen]: #appendix-4
[Aanhangsel 5: Met inzichten van toepassing voor Visual Studio teamservices controleren]: #appendix-5

<!--Image references-->
[1]: ./media/storage-monitoring-diagnosing-troubleshooting/overview.png
[3]: ./media/storage-monitoring-diagnosing-troubleshooting/hour-minute-metrics.png
[4]: ./media/storage-monitoring-diagnosing-troubleshooting/high-e2e-latency.png
[5]: ./media/storage-monitoring-diagnosing-troubleshooting/fiddler-screenshot.png
[6]: ./media/storage-monitoring-diagnosing-troubleshooting/wireshark-screenshot-1.png
[7]: ./media/storage-monitoring-diagnosing-troubleshooting/wireshark-screenshot-2.png
[8]: ./media/storage-monitoring-diagnosing-troubleshooting/wireshark-screenshot-3.png
[9]: ./media/storage-monitoring-diagnosing-troubleshooting/mma-screenshot-1.png
[10]: ./media/storage-monitoring-diagnosing-troubleshooting/mma-screenshot-2.png
