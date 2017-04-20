<properties 
    pageTitle="Besturingssysteem-functionaliteit op Azure App Service" 
    description="Meer informatie over de OS-functionaliteit beschikbaar voor web apps en mobiele app backends API apps Azure App-service" 
    services="app-service" 
    documentationCenter="" 
    authors="cephalin" 
    manager="wpickett" 
    editor="mollybos"/>

<tags 
    ms.service="app-service" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/01/2016" 
    ms.author="cephalin"/>

# <a name="operating-system-functionality-on-azure-app-service"></a>Besturingssysteem-functionaliteit op Azure App Service #

Dit artikel bevat algemene besturingssysteem basisfunctionaliteit die beschikbaar is voor alle toepassingen die worden uitgevoerd op [Azure App-Service](http://go.microsoft.com/fwlink/?LinkId=529714). Deze functionaliteit omvat bestands-, netwerk en toegang tot het register en diagnostische logboeken en gebeurtenissen. 

<a id="tiers"></a>
## <a name="app-service-plan-tiers"></a>App Service plan lagen

Klant apps App-Service uitgevoerd in een omgeving met meerdere huurder hosting. Apps geïmplementeerd in de **vrije** en **gedeelde** lagen uitgevoerd in de werkprocessen op gedeelde virtuele machines, terwijl apps geïmplementeerd in de **standaard** - en **Premium** -lagen worden uitgevoerd op virtual machine(s) is speciaal ontworpen voor de apps die is gekoppeld aan één klant.

Omdat App Service een aantrekkelijker schaal tussen de verschillende niveaus ondersteunt, blijft de beveiligingsconfiguratie afgedwongen voor App Service apps hetzelfde. Dit zorgt ervoor dat apps niet plotseling zich anders gedragen, bij gebreke daarvan op onverwachte wijze wanneer App serviceplan van één laag naar een andere overschakelt.

<a id="developmentframeworks"></a>
## <a name="development-frameworks"></a>Development frameworks

App Service prijzen lagen bepalen het bedrag van de compute resources (CPU, schijfruimte, geheugen en netwerk egress) beschikbaar voor apps. De breedte van de framework-functionaliteit beschikbaar voor apps blijft echter hetzelfde, ongeacht de schaal lagen.

App-Service ondersteunt een groot aantal development frameworks, waaronder ASP.NET, klassieke ASP, node.js, PHP en Python - die allemaal als-extensies in IIS uitgevoerd. Ter vereenvoudiging en beveiligingsconfiguratie normaliseren, App Service apps meestal worden uitgevoerd de verschillende frameworks voor het ontwikkelen met de standaardinstellingen. Een benadering voor het configureren van apps kan zijn voor het aanpassen van het oppervlak van de API en de functionaliteit voor elke afzonderlijke development framework. App-Service maakt in plaats daarvan een meer algemene benadering doordat een gemeenschappelijke basislijn besturingssysteem functionaliteit onafhankelijk van een app development framework.

In de volgende secties worden samengevat de algemene soorten besturingssysteemfunctionaliteit beschikbaar voor apps in App-Service.

<a id="FileAccess"></a>
##<a name="file-access"></a>Toegang tot bestanden

Verschillende stations zijn binnen App-Service, met inbegrip van lokale stations en netwerkstations.

<a id="LocalDrives"></a>
### <a name="local-drives"></a>Lokale stations

De kern App-Service is actief op de infrastructuur Azure PaaS (platform als een service). Als gevolg hiervan zijn de lokale stations die zijn 'gekoppeld' aan een virtuele machine dezelfde stationstypen beschikbaar zijn voor de rol van elke werknemer in Azure uitgevoerd. Dit omvat een besturingssysteem station (de D:\), een toepassing station met Azure cspkg bestanden uitsluitend door App-Service gebruikt (en niet toegankelijk voor klanten) en een "gebruiker" station (de C:\), waarvan de grootte is afhankelijk van de grootte van de VM.

<a id="NetworkDrives"></a>
### <a name="network-drives-aka-unc-shares"></a>Netwerkstations (aka UNC deelt)

Een van de unieke aspecten van App-Service waarmee app-implementatie en het onderhoud eenvoudig is dat alle gebruikersinhoud is opgeslagen op een reeks UNC-shares. Dit model zeer netjes wordt toegewezen aan het algemene patroon van inhoud kunnen opslaan die worden gebruikt door voor bedrijfsruimten webhosting-omgevingen met meerdere servers verdeeld. 

Binnen de App-Service zijn van UNC-shares gemaakt in elk Datacenter. Een percentage van de gebruikersinhoud voor alle klanten in elk datacenter is toegewezen aan elke UNC-share. Bovendien deelt het bestand de inhoud van een enkele klant abonnement altijd op hetzelfde UNC geplaatst wordt. 

Houd er rekening mee dat de specifieke virtuele machine die verantwoordelijk is voor het hosten van een UNC-share als gevolg van cloud hoe werk diensten, na verloop van tijd worden gewijzigd. Er wordt gegarandeerd dat UNC-shares worden gemonteerd door middel van verschillende virtuele machines zoals omhoog en omlaag worden gebracht tijdens het normale verloop van de wolk. Apps moet daarom nooit hardcoded veronderstellingen dat de informatie van de machine in een UNC-pad stabiel in de tijd blijft. Zij moeten gebruiken de handige *faux* absolute pad **D:\home\site** App-Service biedt. Deze faux absoluut pad biedt een methode voor draagbare, app en gebruiker-agnostic voor verwijzingen naar eigen app. Met behulp van **D:\home\site**, kunt een overbrengen gedeelde bestanden app app zonder een nieuwe absolute pad voor elke overdracht configureren.

<a id="TypesOfFileAccess"></a>
### <a name="types-of-file-access-granted-to-an-app"></a>Typen toegang voor bestanden toegekend aan een app

Abonnement van de klant heeft een gereserveerde mapstructuur op een specifieke UNC-share in een datacenter. Een klant kan meerdere apps gemaakt binnen een bepaald Datacenter, zodat alle directory's die deel uitmaken van een abonnement van één klant zijn gemaakt op hetzelfde UNC delen hebben. Het aandeel bevatten mappen zoals die voor de inhoud, fout en diagnostische logboeken en eerdere versies van de app gemaakt door het besturingselement. Zoals verwacht, de klant app mappen zijn beschikbaar voor lezen en schrijven-toegang in runtime door de code van de toepassing van de app.

App Service reserves op de lokale vaste schijven die zijn gekoppeld aan de virtuele machine die wordt uitgevoerd een app, een deel van de ruimte op het station C:\ voor app-specifieke tijdelijke lokale opslag. Hoewel een app heeft volledige lees/schrijf-toegang tot de eigen lokale tijdelijke opslag, is niet die opslag bedoeld rechtstreeks door de code van de toepassing worden gebruikt. In plaats daarvan is de bedoeling voor tijdelijke opslag van bestanden van IIS en web application frameworks. App-Service ook beperkt de hoeveelheid tijdelijke lokale opslagruimte beschikbaar is voor elke app om te voorkomen dat individuele apps verbruiken grote hoeveelheden lokale bestandsopslag.

Twee voorbeelden van het gebruik van App Service tijdelijke lokale opslag zijn de directory voor tijdelijke ASP.NET-bestanden en de map voor IIS gecomprimeerde bestanden. De ASP.NET-compilatie-systeem wordt de map 'Temporary ASP.NET Files' als compilatie tijdelijke cachelocatie. IIS gebruikt de ' IIS gecomprimeerde "map met tijdelijke bestanden voor het opslaan van gecomprimeerde antwoord uitvoer. Beide soorten bestand gebruik (evenals anderen) worden opnieuw toegewezen in App-Service naar een tijdelijke lokale opslag per app. Deze opnieuw toe te wijzen, zorgt u ervoor dat functionaliteit blijft zoals verwacht.

Elke app in de App-Service wordt uitgevoerd als een willekeurig uniek weinig rechten werkprocesidentiteit genoemd de "identiteit groep van toepassingen', hier nader beschreven: [http://www.iis.net/learn/manage/configuring-security/application-pool-identities](http://www.iis.net/learn/manage/configuring-security/application-pool-identities). Toepassingscode wordt deze identiteit gebruikt voor eenvoudige alleen-lezen toegang tot het station van het besturingssysteem (het station D:\). Dit betekent toepassingscode kunt lijst voorkomende directory-structuren en gangbare bestanden op het station besturingssysteem lezen. Hoewel dit ogenschijnlijk een ietwat breed niveau van toegang, dezelfde mappen en bestanden toegankelijk zijn wanneer u inrichten is een functie van de werknemer in een Azure service gehost en lezen de inhoud van het station. 

<a name="multipleinstances"></a>
### <a name="file-access-across-multiple-instances"></a>Toegang tot bestanden via meerdere exemplaren

De basismap bevat de inhoud van de Apps en toepassingscode naartoe kunt schrijven. Als een app op meerdere instanties wordt uitgevoerd, wordt de basismap gedeeld door alle instanties zodat overal dezelfde map zien. Dus bijvoorbeeld zijn als een app geüploade bestanden naar de basismap slaat, die bestanden onmiddellijk beschikbaar voor alle exemplaren. 

<a id="NetworkAccess"></a>
## <a name="network-access"></a>Toegang tot het netwerk
Toepassingscode kan gebruikmaken van TCP/IP en UDP gebaseerde protocollen waarmee uitgaande netwerkverbindingen naar Internet toegankelijk eindpunten die diensten van buitenaf zichtbaar. Apps kunnen u deze dezelfde protocollen gebruiken verbinding maken met services in Azure & #151, bijvoorbeeld door HTTPS verbinding maken met SQL-Database.

Er is een beperkte mogelijkheid voor toepassingen voor één lokale loopback-verbinding tot stand brengen en hebben een app die lokale loopback-socket luisteren. Deze functie is vooral apps die op lokale loopback-sockets als onderdeel van hun functionaliteit luistert inschakelen. Houd er rekening mee dat elke app een "persoonlijk" loopback-verbinding ziet. "A" App kan niet luisteren naar een lokale loopback-socket tot stand gebracht door de app 'B'.

Named pipes worden ook ondersteund als een mechanisme voor de communicatie tussen processen (IPC) tussen de verschillende processen die samen een toepassing uitvoeren. De module IIS FastCGI is bijvoorbeeld gebaseerd op named pipes te coördineren van de afzonderlijke processen die PHP-pagina's worden uitgevoerd.


<a id="Code"></a>
## <a name="code-execution-processes-and-memory"></a>Tot uitvoering van code, processen en geheugen
Zoals eerder opgemerkt, worden apps in een willekeurige identiteit voor groep van toepassingen met weinig rechten werkprocessen uitgevoerd. Toepassingscode heeft toegang tot de geheugenruimte die is gekoppeld aan het werkproces als onderliggende processen die kunnen worden geïnitieerd door CGI-processen of andere toepassingen. Echter een app geen toegang tot het geheugen of de gegevens van een andere app zelfs als deze zich op dezelfde virtuele machine.

Apps kunnen uitvoeren van scripts of pagina's die zijn geschreven met ondersteunde web development frameworks. App-Service configureren geen instellingen voor beperkte modi web framework. ASP.NET-toepassingen op App-Service uitvoeren bijvoorbeeld in "full" vertrouwen in plaats van een meer beperkte modus, de vertrouwensrelatie. Frameworks voor het web, met inbegrip van zowel klassieke ASP en ASP.NET, kunnen bellen in-process COM-onderdelen (maar niet out process COM-onderdelen) zoals ADO (ActiveX Data Objects), die standaard op het Windows-besturingssysteem zijn geregistreerd.

Apps kunnen brengen gang en willekeurige code kan uitvoeren. Het is toegestaan voor een app doen zoals spawn een opdrachtshell of een PowerShell script uitvoeren. Ondanks dat willekeurige code en processen kunnen worden geïnitieerd vanuit een app, zijn uitvoerbare programma's en scripts echter nog steeds beperkt tot de bevoegdheden die zijn toegewezen aan de bovenliggende groep van toepassingen. Bijvoorbeeld een app kunt brengen gang een uitvoerbaar bestand dat als een uitgaande HTTP-oproep, maar die hetzelfde uitvoerbare bestand kan niet proberen losmaken van het IP-adres van een virtuele machine van de netwerkadapterkaart. Een netwerk uitgaande oproep te weinig rechten code is toegestaan, maar probeert te configureren van netwerkinstellingen op een virtuele machine hebt beheerdersrechten nodig.


<a id="Diagnostics"></a>
## <a name="diagnostics-logs-and-events"></a>Diagnostische logboeken en gebeurtenissen
Logboekgegevens is een andere set gegevens die sommige apps proberen te krijgen. De soorten logboekgegevens die beschikbaar zijn voor code die wordt uitgevoerd in de App-Service bevat diagnostische en logboekgegevens gegenereerd door een toepassing die ook gemakkelijk toegankelijk is voor de app. 

HTTP-W3C-logboeken gegenereerd door een actieve app zijn bijvoorbeeld beschikbaar zijn op een logboekmap op de netwerkshare voor de app, of in een blob-opslag beschikbaar gemaakt als een klant de W3C-logboekregistratie naar opslag heeft ingesteld. De laatste optie kan grote hoeveelheden logboeken worden verzameld zonder het risico van overschrijding van het bestand opslag grenzen die is gekoppeld aan een netwerkshare.

In een vergelijkbare vein real-time diagnostische informatie van .NET-toepassingen kan ook worden vastgelegd met de .NET tracering en diagnostische infrastructuur, met opties om de traceergegevens schrijven naar de netwerkshare van de app, of u kunt ook naar een blob-opslaglocatie.

Gebieden van diagnostische logboekregistratie en tracering niet beschikbaar zijn voor toepassingen zijn Windows ETW gebeurtenissen en gebeurtenislogboeken gemeenschappelijke Windows (bijvoorbeeld systeem, gebeurtenislogboeken toepassing en beveiliging). Aangezien de ETW-traceergegevens kan mogelijk worden zichtbare alle computers (met het recht van ACL's), worden lees- en schrijftoegang tot de ETW-gebeurtenissen geblokkeerd. Ontwikkelaars kunnen merken dat API-oproepen worden gelezen en geschreven ETW gebeurtenissen en gebeurtenislogboeken met gemeenschappelijke Windows lijken te werken, maar dat komt omdat de App Service is 'faking' aanroepen van de zodat deze worden weergegeven om te slagen. In werkelijkheid heeft de code van de toepassing geen toegang tot gegevens van deze gebeurtenis.

<a id="RegistryAccess"></a>
## <a name="registry-access"></a>Toegang tot het register
Apps zijn alleen-lezen toegang tot veel (maar niet alle) van het register van de virtuele machine op uitvoert. In de praktijk betekent dit registersleutels die alleen-lezen toegang tot de lokale groep gebruikers zijn toegankelijk voor apps. Een gedeelte van het register dat wordt momenteel niet ondersteund voor lees- of schrijftoegang is de HKEY\_huidige\_component van gebruiker.

Schrijftoegang tot het register wordt geblokkeerd, met inbegrip van de toegang tot registersleutels per gebruiker. Vanuit het oogpunt van de app, schrijftoegang tot het register moet nooit worden aangehaald in de Azure-omgeving omdat apps kunnen (en doen) over verschillende virtuele machines worden overgezet. Het alleen permanente beschrijfbare opslag die door een toepassing kan worden afhankelijk van is de per-app content mapstructuur opgeslagen op de App Service UNC-shares. 

>[AZURE.NOTE] Als u wilt aan de slag met Azure App-Service voordat u aanmelden voor een account met Azure, gaat u naar de [App-Service probeert](http://go.microsoft.com/fwlink/?LinkId=523751), waar u direct een starter tijdelijk web app in de App-Service maken kunt. Geen creditcard vereist; geen verplichtingen.

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]
 
 
