<properties
   pageTitle="Systeemvereisten voor StorSimple | Microsoft Azure"
   description="Beschrijving van software, netwerken en hoge beschikbaarheid eisen en aanbevelingen voor een oplossing met Microsoft Azure StorSimple."
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor=""/>

<tags
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="08/31/2016"
   ms.author="alkohli"/>

# <a name="storsimple-software-high-availability-and-networking-requirements"></a>StorSimple software, hoge beschikbaarheid en netwerkvereisten

## <a name="overview"></a>Overzicht

Welkom bij Microsoft Azure StorSimple. Dit artikel bevat belangrijke informatie over de systeemvereisten en aanbevelingen voor uw apparaat StorSimple en de opslag-clients toegang tot het apparaat. Wij raden aan dat u lees de informatie zorgvuldig door voordat u uw systeem StorSimple te implementeren en vervolgens terug naar deze indien nodig tijdens de implementatie en verdere bewerking verwijzen.

De systeemvereisten zijn:

- **Softwarevereisten voor clients opslag** - beschrijving van de ondersteunde besturingssystemen en eventuele aanvullende voorschriften voor deze besturingssystemen.
- **Vereisten voor het apparaat StorSimple networking** - geeft informatie over de poorten die zijn geopend in de firewall moeten voor iSCSI, cloud of beheer van verkeer.
- **Vereisten voor hoge beschikbaarheid voor StorSimple** - beschrijft de vereisten voor hoge beschikbaarheid en aanbevolen procedures voor de StorSimple-apparaat en de host-computer. 


## <a name="software-requirements-for-storage-clients"></a>Softwarevereisten voor clients opslag

De volgende software-eisen zijn voor de opslag op clients die toegang hebben tot uw apparaat StorSimple.

| Ondersteunde besturingssystemen | Vereiste versie | Aanvullende eisen en/of notities |
| --------------------------- | ---------------- | ------------- |
| Windows Server              | 2008R2 SP1, 2012, 2012R2 |StorSimple iSCSI-volumes worden ondersteund voor gebruik op alleen de volgende Windows schijftypen:<ul><li>Eenvoudig volume op een standaardschijf</li><li>Eenvoudige en mirrored volume op een dynamische schijf</li></ul>Windows Server 2012 thin provisioning en ODX functies worden ondersteund als u van een StorSimple iSCSI-volume gebruikmaakt.<br><br>StorSimple kunt maken, dun ingerichte en volledig ingerichte volumes. Er kan geen gedeeltelijk ingerichte volumes maken.<br><br>Opnieuw formatteren van een volume dun ingerichte kan erg lang duren. Het is raadzaam om het volume te verwijderen en vervolgens een nieuwe maakt in plaats van het opnieuw formatteren. Echter, als u liever een volume formatteren:<ul><li>Voer de volgende opdracht voordat het herformatteren ruimte terugwinning vertragingen te voorkomen: <br>`fsutil behavior set disabledeletenotify 1`</br></li><li>Nadat het formatteren voltooid is, gebruikt u de volgende opdracht ruimte terugwinnen opnieuw inschakelen:<br>`fsutil behavior set disabledeletenotify 0`</br></li><li>De hotfix Windows Server 2012 toepassen zoals is beschreven in [KB 2878635](https://support.microsoft.com/kb/2870270) op uw computer Windows Server.</li></ul></li></ul></ul> Als u voor SharePoint StorSimple Snapshot Manager of StorSimple Adapter configureert, gaat u naar de [softwarevereisten voor optionele onderdelen](#software-requirements-for-optional-components).|
| VMWare ESX | 5.5 en 6.0 | Met VMWare vSphere ondersteund als de iSCSI-client. VAAI-block-functie wordt met VMware vSphere op StorSimple apparaten ondersteund.
| RHEL Linux/CentOS | 5, 6 en 7 | Ondersteuning voor Linux iSCSI-clients met open-iSCSI-initiator versie 5, 6 en 7. |
| Linux | SUSE Linux 11 | |
 > [AZURE.NOTE] IBM AIX wordt momenteel niet ondersteund met StorSimple.

## <a name="software-requirements-for-optional-components"></a>Softwarevereisten voor optionele onderdelen

De volgende softwarevereisten zijn voor de optionele onderdelen StorSimple (StorSimple Snapshot Manager en StorSimple Adapter voor SharePoint).

| Onderdeel | Hostplatform | Aanvullende eisen en/of notities |
| --------------------------- | ---------------- | ------------- |
| StorSimple Snapshot Manager | Windows Server 2008R2 SP1, 2012, 2012R2 | Gebruik van StorSimple Snapshot Manager op Windows Server is vereist voor de back-up of het herstellen van mirrored dynamische schijven en voor elke toepassing consistent back-ups.<br> StorSimple Snapshot Manager wordt alleen ondersteund op Windows Server 2008 R2 SP1 (64-bits), Windows 2012 R2 en Windows Server 2012.<ul><li>Als u venster Server 2012 gebruikt, moet u .NET 3.5-4.5 installeren voordat u StorSimple Snapshot Manager installeren.</li><li>Als u Windows Server 2008 R2 SP1 gebruikt, moet u Windows Management Framework 3.0 installeren voordat u StorSimple Snapshot Manager installeren.</li></ul> |
| StorSimple Adapter voor SharePoint | Windows Server 2008R2 SP1, 2012, 2012R2 |<ul><li>Adapter voor SharePoint StorSimple wordt alleen ondersteund voor SharePoint 2010 en SharePoint 2013.</li><li>RBS vereist SQL Server Enterprise Edition, versie 2008 R2 of 2012.</li></ul>|

## <a name="networking-requirements-for-your-storsimple-device"></a>Vereisten voor het apparaat StorSimple Networking

Uw apparaat StorSimple is een vergrendeld. Echter poorten moeten worden geopend in de firewall voor iSCSI-cloud en beheer van verkeer. De volgende tabel worden de poorten die moeten worden geopend in de firewall. In deze tabel verwijst *in* of *inkomende* naar de richting waarin binnenkomende clientaanvragen toegang het apparaat tot. *Out* of *uitgaande* verwijst naar de richting waarin het apparaat StorSimple gegevens extern, na de implementatie verzendt: bijvoorbeeld, uitgaande met het Internet.

| Poort nr.<sup>1,2</sup> | In- of uitzoomen | Bereik van poort | Vereist | Notities |
|------------------------|-----------|------------|----------|-------|
|TCP 80 (HTTP)<sup>3</sup>|  Out |  WAN | Nee |<ul><li>Uitgaande poort wordt gebruikt voor toegang tot het Internet voor het ophalen van updates.</li><li>De uitgaande webproxy is een gebruiker worden geconfigureerd.</li><li>Als u systeemupdates, moet deze poort ook zijn geopend voor de controller vaste IP-adressen.</li></ul> |
|TCP 443 (HTTPS)<sup>3</sup>| Out | WAN | Ja |<ul><li>Uitgaande poort wordt gebruikt voor toegang tot gegevens in de cloud.</li><li>De uitgaande webproxy is een gebruiker worden geconfigureerd.</li><li>Als u systeemupdates, moet deze poort ook zijn geopend voor de controller vaste IP-adressen.</li><li>Deze poort wordt ook gebruikt op beide domeincontrollers de opschoonbewerking is.</li></ul>|
|UDP 53 (DNS) | Out | WAN | In sommige gevallen; Zie ' Opmerkingen '. |Deze poort is alleen vereist als u een Internet-based DNS-server gebruikt. |
| UDP 123 (NTP) | Out | WAN | In sommige gevallen; Zie ' Opmerkingen '. |Deze poort is alleen vereist als u een Internet-based NTP-server gebruikt. |
| 9354 TCP | Out | WAN | Ja |De uitgaande poort wordt gebruikt door het apparaat StorSimple te communiceren met de StorSimple Manager-service. |
| 3260 (iSCSI) | In | LAN | Nee | Deze poort wordt gebruikt voor toegang tot gegevens via iSCSI.|
| 5985 | In | LAN | Nee | Inkomende poort wordt door StorSimple Snapshot Manager gebruikt om te communiceren met het apparaat StorSimple.<br>Deze poort wordt ook gebruikt wanneer u extern verbinding met Windows PowerShell voor StorSimple via HTTP maken. |
| 5986 | In | LAN | Nee | Deze poort wordt gebruikt wanneer u extern verbinding met Windows PowerShell voor StorSimple via HTTPS maken. |

<sup>1</sup> geen inkomende poorten moeten worden geopend op het openbare Internet.

<sup>2</sup> als meerdere poorten een bestandsgateway-configuratie, de volgorde van de route het uitgaande verkeer wordt bepaald op basis van de poort volgorde in het [bewerkingsplan poort](#routing-metric), die hieronder worden beschreven.

<sup>3</sup> de vaste IP-adressen op het apparaat StorSimple controller moet routeerbaar en verbinding kunnen maken met het Internet zijn. Het vaste IP-adressen worden gebruikt voor het onderhoud van de updates voor het apparaat. Apparaten verbinding maken met het Internet via het vaste IP-adressen, is het niet mogelijk het apparaat StorSimple bijwerken.

> [AZURE.IMPORTANT] Zorg ervoor dat de firewall niet wijzigen of de SSL-verkeer tussen de StorSimple apparaat en Azure decoderen.

### <a name="url-patterns-for-firewall-rules"></a>URL-patronen voor de firewall-regels

Netwerkbeheerders kunnen vaak geavanceerde firewall-regels op basis van de URL-patronen voor het filteren van het inkomend en uitgaand verkeer configureren. Het apparaat StorSimple en de StorSimple Manager-service zijn afhankelijk van andere Microsoft-toepassingen zoals Azure Service Bus, Azure Active Directory-toegangsbeheer opslag rekeningen en Microsoft Update-servers. De URL-patronen die zijn gekoppeld aan deze toepassingen kunnen worden gebruikt voor het configureren van de firewall-regels. Het is belangrijk te begrijpen dat de URL-patronen die zijn gekoppeld aan deze toepassingen kunnen wijzigen. Dit vereist op zijn beurt de netwerkbeheerder om te controleren en bijwerken van firewall-regels voor de StorSimple als en wanneer dat nodig is.

Het is raadzaam dat u uw firewall-regels voor uitgaand verkeer, op basis van vaste IP-adressen, opneemt in de meeste gevallen StorSimple ingesteld. Echter, kunt u onderstaande informatie over de geavanceerde firewall-regels die nodig zijn voor het maken van beveiligde omgevingen in te stellen.

> [AZURE.NOTE] Het apparaat (bron) IP-adressen moet altijd worden ingesteld op de ingeschakelde netwerkinterfaces. De bestemming die IP-adressen moet worden ingesteld op [IP-adresbereiken Azure datacenter](https://www.microsoft.com/en-us/download/confirmation.aspx?id=41653).

#### <a name="url-patterns-for-azure-portal"></a>URL-patronen voor Azure portal
| URL-patroon                                                      | Component/functionaliteit                                           | Apparaat IP-adressen                           |
|------------------------------------------------------------------|---------------------------------------------------------------|-----------------------------------------|
| `https://*.storsimple.windowsazure.com/*`<br>`https://*.accesscontrol.windows.net/*`<br>`https://*.servicebus.windows.net/*`   | StorSimple Manager-service<br>Access Control-Service<br>Azure Service Bus| Cloud-netwerkinterfaces        |
|`https://*.backup.windowsazure.com`|De registratie| Alleen gegevens 0|
|`http://crl.microsoft.com/pki/*`<br>`http://www.microsoft.com/pki/*`|Het intrekken van certificaten |Cloud-netwerkinterfaces |
| `https://*.core.windows.net/*` <br>`https://*.data.microsoft.com`<br>`http://*.msftncsi.com` | Azure opslag rekeningen en controle | Cloud-netwerkinterfaces        |
| `http://*.windowsupdate.microsoft.com`<br>`https://*.windowsupdate.microsoft.com`<br>`http://*.update.microsoft.com`<br> `https://*.update.microsoft.com`<br>`http://*.windowsupdate.com`<br>`http://download.microsoft.com`<br>`http://wustat.windows.com`<br>`http://ntservicepack.microsoft.com`| Microsoft Update-servers<br>                             | Vaste IP-adressen alleen controller               |
| `http://*.deploy.akamaitechnologies.com`                         |Akamai CDN |Vaste IP-adressen alleen controller   |
| `https://*.partners.extranet.microsoft.com/*`                    | Support-pakket                                                  | Cloud-netwerkinterfaces        |

#### <a name="url-patterns-for-azure-government-portal"></a>URL-patronen voor Azure regering portal
| URL-patroon                                                      | Component/functionaliteit                                           | Apparaat IP-adressen                           |
|------------------------------------------------------------------|---------------------------------------------------------------|-----------------------------------------|
| `https://*.storsimple.windowsazure.us/*`<br>`https://*.accesscontrol.usgovcloudapi.net/*`<br>`https://*.servicebus.usgovcloudapi.net/*`   | StorSimple Manager-service<br>Access Control-Service<br>Azure Service Bus| Cloud-netwerkinterfaces        |
|`https://*.backup.windowsazure.us`|De registratie| Alleen gegevens 0|
|`http://crl.microsoft.com/pki/*`<br>`http://www.microsoft.com/pki/*`|Het intrekken van certificaten |Cloud-netwerkinterfaces |
| `https://*.core.usgovcloudapi.net/*` <br>`https://*.data.microsoft.com`<br>`http://*.msftncsi.com` | Azure opslag rekeningen en controle | Cloud-netwerkinterfaces        |
| `http://*.windowsupdate.microsoft.com`<br>`https://*.windowsupdate.microsoft.com`<br>`http://*.update.microsoft.com`<br> `https://*.update.microsoft.com`<br>`http://*.windowsupdate.com`<br>`http://download.microsoft.com`<br>`http://wustat.windows.com`<br>`http://ntservicepack.microsoft.com`| Microsoft Update-servers<br>                             | Vaste IP-adressen alleen controller               |
| `http://*.deploy.akamaitechnologies.com`                         |Akamai CDN |Vaste IP-adressen alleen controller   |
| `https://*.partners.extranet.microsoft.com/*`                    | Support-pakket                                                  | Cloud-netwerkinterfaces        |

### <a name="routing-metric"></a>Routeringsmetric

Een routeringsmetriek is gekoppeld aan de interfaces en de gateway die de gegevens doorsturen naar de opgegeven netwerken. Routeringsmetriek wordt gebruikt door het protocol voor het berekenen van het beste pad naar een bepaalde bestemming, als er meerdere paden naar hetzelfde doel bestaan achterhaalt. Hoe lager de routeringsmetric, hoe hoger de voorkeur.

In het kader van de StorSimple als meerdere netwerkinterfaces en gateways zijn geconfigureerd voor het verkeer kanalen, de routeringsmetrics zal een rol spelen om te bepalen van de relatieve volgorde waarin de interfaces gewend. De routeringsmetrics kan niet worden gewijzigd door de gebruiker. U kunt echter de `Get-HcsRoutingTable` cmdlet af te drukken de routing tabel (en metrics) op uw apparaat StorSimple. Meer informatie over de cmdlet Get-HcsRoutingTable in [het oplossen van StorSimple implementatie](storsimple-troubleshoot-deployment.md).

De metrische routeringsalgoritmen zijn afhankelijk van de versie van de software op uw apparaat StorSimple uitgevoerd.

**Eerdere versies dan Update 1**

Inclusief softwareversies voorafgaand aan Update 1 zoals de GA, 0,1, 0,2 of 0,3 release. De volgorde op basis van routeringsmetrics is als volgt:

   *Laatste 10 GbE-netwerk-interface geconfigureerd > overige 10 GbE-netwerkinterface > laatste 1 GbE-netwerk-interface geconfigureerd > andere 1 GbE-netwerk-interface*


**Releases van Update 1 en dan Update 2**

Dit omvat softwareversies zoals 1, punten 1.1 en 1.2. De volgorde op basis van routeringsmetrics wordt als volgt bepaald:

   *DATA 0 > laatste 10 GbE-netwerk-interface geconfigureerd > overige 10 GbE-netwerk-interface > laatste 1 GbE-netwerk-interface geconfigureerd > Overige 1 GbE-netwerk-interface*

   Update 1, de routeringsmetric gegevens 0 wordt gedaan de laagste; Daarom wordt alle cloud-verkeer doorgestuurd via DATA 0. Noteer dit als er meer dan één cloud-netwerk-interface op uw apparaat StorSimple.


**Releases Update 2 vanaf**

Update 2 heeft verschillende netwerk-gerelateerde verbeteringen en de routeringsmetrics is gewijzigd. Het gedrag kan als volgt worden verklaard.

- Een set vooraf ingestelde waarden zijn toegewezen aan netwerkinterfaces.   

- U kunt een van de voorbeeldtabel hieronder weergegeven met waarden die zijn toegewezen aan de verschillende netwerkinterfaces wanneer deze cloud-zijn ingeschakeld of cloud uitgeschakeld maar met een geconfigureerde gateway. Opmerking de waarden toegewezen hier slechts voorbeelden van waarden zijn.


  	| Netwerk-interface | Cloud-ingeschakeld | Wolk met de gateway is uitgeschakeld |
  	|-----|---------------|---------------------------|
  	| Data 0  | 1            | -                        |
  	| 1 gegevens  | 2            | 20                       |
  	| 2 gegevens  | 3            | 30                       |
  	| 3 gegevens  | 4            | 40                       |
  	| 4 gegevens  | 5            | 50                       |
  	| Gegevens 5  | 6            | 60                       |


- De volgorde waarin de cloud-verkeer wordt doorgestuurd via de netwerkinterfaces is:

    *Data 0 > gegevens 1 > datum 2 > gegevens 3 > gegevens 4 > gegevens 5*

    Dit kan worden verklaard door het volgende voorbeeld.

    U kunt een apparaat StorSimple met twee cloud-netwerkinterfaces gegevens 0 en 5 gegevens. Gegevens 1 tot en met 4 gegevens zijn wolk is uitgeschakeld, maar hebben een geconfigureerde gateway. De volgorde waarin verkeer wordt doorgestuurd voor dit apparaat zijn:

    *0 (1) gegevens > gegevens 5 (6) > gegevens 1 (20) > gegevens 2 (30) > gegevens 3 (40) > gegevens 4 (50)*

    *de cijfers tussen haakjes geven waar de respectieve routeringsmetrics.*

    Als gegevens 0 is mislukt, wordt het verkeer wolk tot en met 5 van de gegevens ophalen gerouteerd. Gezien het feit dat er een gateway is geconfigureerd op een ander netwerk als zowel gegevens 0 en 5 gegevens zou crashen, doorloopt de wolk verkeer gegevens 1.


- Als een interface voor cloud-netwerk mislukt, vervolgens zijn 3 nieuwe pogingen met een 30 tweede vertraging verbinding maken met de interface. Als alle pogingen mislukken, wordt het verkeer doorgestuurd naar de volgende beschikbare cloud-compatibele interface zoals bepaald door de routeringstabel. Als u alle ingeschakelde cloud netwerk interfaces mislukt, vervolgens failover het apparaat uitgevoerd naar de andere controller (opnieuw opstarten is niet in dit geval).

- Als er een storing VIP voor een iSCSI-netwerk-interface, zullen er 3 nieuwe pogingen met een vertraging van 2 seconden. Dit probleem heeft uit de vorige versies hetzelfde gebleven. Als alle iSCSI-netwerkinterfaces mislukken, wordt een controller failover (vergezeld van een reboot) plaatsvinden.


- Een waarschuwing treedt ook op uw apparaat StorSimple wanneer er een VIP-fout. Ga naar de [Waarschuwing naslag](storsimple-manage-alerts.md)voor meer informatie.

- Voor nieuwe pogingen voorrang iSCSI op wolk.

    Bekijk het volgende voorbeeld: A StorSimple apparaat twee netwerkinterfaces ingeschakeld heeft, gegevens 0 en 1 met gegevens. Data 0 is dat beide cloud Data 1 cloud ingeschakeld en iSCSI-ondersteuning. Er zijn geen andere netwerkinterfaces op dit apparaat zijn ingeschakeld voor cloud of iSCSI.

    Als gegevens 1 mislukt, krijgt het de laatste iSCSI-netwerk-interface, resulteert dit in een controller failover-gegevens 1 op de andere domeincontroller.


### <a name="networking-best-practices"></a>Aanbevolen procedures voor netwerken

Naast de bovenstaande netwerkvereisten voor de optimale prestaties van uw oplossing StorSimple zich houden aan de volgende procedures:

- Zorg ervoor dat het apparaat StorSimple heeft een specifieke bandbreedte van 40 Mbps (of meer) beschikbaar te allen tijde. Deze bandbreedte mag niet worden gedeeld (of toewijzing moet worden gewaarborgd door het gebruik van QoS-beleid) met andere toepassingen.

- Controleer de netwerkverbinding met het Internet te allen tijde beschikbaar is. Enkel geval of onbetrouwbare Internet-verbindingen met de apparaten, met inbegrip van welke aard dan ook, geen internetverbinding resulteert in een niet-ondersteunde configuratie.

- De iSCSI- en cloud-verkeer door netwerkinterfaces op uw apparaat voor iSCSI- en cloud toegang hebben speciale isoleren. Zie voor meer informatie de [netwerkinterfaces wijzigen](storsimple-modify-device-config.md#modify-network-interfaces) op het apparaat StorSimple.

- Een configuratie met Link Aggregation Control Protocol (LACP) niet gebruiken voor uw netwerkinterfaces. Dit is een niet-ondersteunde configuratie.


## <a name="high-availability-requirements-for-storsimple"></a>Vereisten voor hoge beschikbaarheid voor StorSimple

De hardwareplatform dat deel uitmaakt van de StorSimple oplossing heeft de beschikbaarheid en betrouwbaarheid van de functies die kunnen worden gebruikt voor een hoge beschikbaarheid en fouttolerante opslaginfrastructuur in uw datacenter. Er zijn echter eisen en aanbevelingen die u voldoen moet om ervoor te zorgen dat de beschikbaarheid van uw oplossing StorSimple. Voordat u StorSimple implementeert, zorgvuldig door de volgende vereisten en aanbevelingen voor de StorSimple apparaat en verbonden host-computers.

Ga voor meer informatie over het controleren en beheren van de hardware-onderdelen van het apparaat StorSimple naar [gebruik van de service Manager StorSimple hardwareonderdelen en de status controleren](storsimple-monitor-hardware-status.md) en [StorSimple hardware component vervangen](storsimple-hardware-component-replacement.md).

### <a name="high-availability-requirements-and-procedures-for-your-storsimple-device"></a>Hoge beschikbaarheid-voorschriften en procedures voor het apparaat StorSimple

Lees de volgende informatie zorgvuldig door om ervoor te zorgen de hoge beschikbaarheid van uw apparaat StorSimple.

#### <a name="pcms"></a>PCMs

StorSimple apparaten zijn redundante, hot-swappable voedings- en koelingsmodules (PCMs). Elke PCM heeft voldoende capaciteit om de service voor het hele chassis. Hoge beschikbaarheid, zodat moeten beide PCMs worden geïnstalleerd.

- Uw PCMs verbinding met verschillende energiebronnen om beschikbaarheid te bieden als een energiebron is mislukt.
- Als een PCM mislukt, aanvragen van een vervanging onmiddellijk.
- Een mislukte PCM alleen verwijderd wanneer u de vervanging hebt en bent klaar om te installeren.
- Beide PCMs niet tegelijkertijd verwijderen. De PCM-module bevat de back-up batterij module. Verwijderen van de PCMs resulteert in een afsluiten zonder de bescherming van de accu en de status van het apparaat, worden niet opgeslagen. Ga naar [de back-up batterij module onderhouden](storsimple-battery-replacement.md#maintain-the-backup-battery-module)voor meer informatie over de batterij.

#### <a name="controller-modules"></a>Controller-modules

StorSimple apparaten bevatten redundante, hot-swappable controller modules. De modules voor de controller functioneren op een actieve/passieve wijze. Op een bepaald moment één controllermodule actief is en biedt service, terwijl de andere controllermodule passief is. De passieve controllermodule wordt ingeschakeld en in werking treedt als de controllermodule active uitvalt of wordt verwijderd. Elke controllermodule heeft voldoende capaciteit om de service voor het hele chassis. Beide modules controller moeten zijn geïnstalleerd om een hoge beschikbaarheid garanderen.

- Zorg ervoor dat beide modules controller te allen tijde zijn geïnstalleerd.

- Als een controllermodule uitvalt, aanvragen van een vervanging onmiddellijk.

- Een defecte controllermodule alleen verwijderd wanneer u de vervanging hebt en bent klaar om te installeren. Verwijderen van een module voor langere perioden van invloed zijn op de luchtstroom en dus het koelen van het systeem.

- Zorg ervoor dat de netwerkverbindingen aan beide modules controller identiek zijn en een identieke configuratie van de aangesloten netwerkinterfaces hebben.

- Als een controllermodule uitvalt of vervanging moet, zorg ervoor dat de controllermodule geactiveerd voordat de controllermodule defecte vervangen. Om te controleren dat een domeincontroller actief is, gaat u naar [de actieve controller op uw apparaat identificeren](storsimple-controller-replacement.md#identify-the-active-controller-on-your-device).

- Verwijder beide modules controller niet op hetzelfde moment. Als u een controller failover wordt uitgevoerd, geen afgesloten in de stand-by-controllermodule, en verwijderen uit het chassis.

- Wacht ten minste vijf minuten voordat u een controllermodule verwijderen na een failover controller.

#### <a name="network-interfaces"></a>Netwerkinterfaces

StorSimple controller apparaatmodules elke hebben vier 1 Gigabit en twee 10 Gigabit Ethernet-netwerkinterfaces.

- Zorg ervoor dat de netwerkverbindingen aan beide modules controller identiek zijn, en dat de controller-module-interfaces zijn verbonden met een identieke configuratie hebt netwerkinterfaces.

- Indien mogelijk, implementeren van netwerkverbindingen voor verschillende switches te waarborgen van de beschikbaarheid van de service bij een storing netwerk apparaat.

- Wanneer de enige of de laatste resterende ingeschakeld iSCSI interface (met IP-adressen toegewezen) los, eerst de interface uitschakelen en haal de kabels. Als de interface eerste losgekoppeld is, wordt dit de actieve domeincontroller tot de controller voor passieve failover. Als de passieve controller ook de bijbehorende interfaces losgekoppeld heeft, vervolgens de beide domeincontrollers wordt opnieuw opgestart meerdere keren voordat op een domeincontroller.

- Ten minste twee DATA-interfaces verbinding met het netwerk van elke controllermodule.

- Als u beide hebt ingeschakeld 10 GbE-interfaces moet implementeren die voor verschillende switches.

- Gebruik indien mogelijk MPIO op servers om ervoor te zorgen dat de servers mogelijk zonder een koppeling, een netwerk of een storing in de netwerkinterface.

Voor meer informatie over netwerken het apparaat voor hoge beschikbaarheid en prestaties, gaat u naar de [installatie van het apparaat StorSimple 8100](storsimple-8100-hardware-installation.md#cable-your-storsimple-8100-device) of [uw apparaat StorSimple 8600](storsimple-8600-hardware-installation.md#cable-your-storsimple-8600-device).

#### <a name="ssds-and-hdds"></a>SSD en harde schijven

Solid state schijven (SSD) zijn voorbeelden van StorSimple apparaten en vaste schijven (harde schijven) die zijn beveiligd met behulp van spaties gespiegeld. Gebruik van mirrored spaties zorgt ervoor dat het apparaat is bestand tegen storingen in één of meer SSD of harde schijven.

- Zorg ervoor dat alle SSD en HDD-modules zijn geïnstalleerd.

- Aanvraag vervangende onmiddellijk als een SSD of harde schijf uitvalt.

- Als een SSD of harde schijf uitvalt of moet worden vervangen, ervoor zorgen dat u alleen de SSD of harde schijf die moet worden vervangen.

- Verwijder niet meer dan één SSD of harde schijf van het systeem op elk punt in de tijd.
Een storing van 2 of meer schijven van een bepaald type (harde schijven, SSD) of opeenvolgende storing binnen een korte tijd mogelijk systeem storing en potentieel gegevensverlies. In dat geval [contact op met Microsoft Support](storsimple-contact-microsoft-support.md) voor hulp.

- Tijdens vervanging, de **Status van de Hardware** op de pagina **onderhoud** voor de stations in de SSD en harde schijven te controleren. De status van een groen vinkje geeft aan dat de schijven in orde of OK, dat wijst u een rood uitroepteken geeft aan dat een mislukte SSD of harde schijf.

- Het is raadzaam om momentopnamen van de wolk voor alle volumes die u wilt beschermen in het geval van een systeemstoring te configureren.

#### <a name="ebod-enclosure"></a>EBOD-behuizing

StorSimple apparaat model 8600 bevat een uitgebreide bundel van schijven (EBOD) bijlage naast de primaire behuizing. Een EBOD bevat EBOD-controllers en harde schijven (harde schijven) die zijn beveiligd met behulp van spaties gespiegeld. Gebruik van mirrored spaties zorgt ervoor dat het apparaat is bestand tegen het uitvallen van een of meer harde schijven. De behuizing van de EBOD is verbonden met de primaire behuizing via redundante SAS-kabels.

- Zorg ervoor dat beide modules EBOD behuizing controller, zowel SAS-kabels en alle van de vaste schijven zijn geïnstalleerd op alle tijden.

- Als een module EBOD behuizing controller uitvalt, aanvragen van een vervanging onmiddellijk.

- Als een module EBOD behuizing controller uitvalt, zorg ervoor dat de controllermodule actief is voordat u de module vervangen. Om te controleren dat een domeincontroller actief is, gaat u naar [de actieve controller op uw apparaat identificeren](storsimple-controller-replacement.md#identify-the-active-controller-on-your-device).

- Tijdens een EBOD vervanging module controller, continu bewaken de status van het onderdeel in de StorSimple Manager-service via **onderhoud** > **status van de Hardware**.

- Als een SAS-kabel uitvalt of moet worden vervangen (Microsoft Support moeten worden betrokken om een dergelijke bepaling), moet u de SAS-kabel die moet worden vervangen.

- Gelijktijdig Verwijder zowel SAS-kabels van het systeem op elk punt in de tijd.

### <a name="high-availability-recommendations-for-your-host-computers"></a>Hoge beschikbaarheid-aanbevelingen voor de host-computers

Deze best practices om ervoor te zorgen de hoge beschikbaarheid van de hosts die zijn verbonden met het apparaat StorSimple zorgvuldig controleren.

- StorSimple configureren met [twee knooppunten bestand clusterconfiguraties][1]. Doordat potentiële storingen en opbouw in redundantie op de host-zijde wordt de gehele oplossing uiterst beschikbaar.

- Gebruik continu beschikbaar (CA) deelt tijdens een overname van de opslagcontrollers beschikbaar met Windows Server 2012 (SMB 3.0) voor hoge beschikbaarheid. Verwijzen naar deze [video demo](http://channel9.msdn.com/Events/IT-Camps/IT-Camps-On-Demand-Windows-Server-2012/DEMO-Continuously-Available-File-Shares)voor meer informatie voor het configureren van serverclusters bestand en aandelen continu beschikbaar met Windows Server 2012.

## <a name="next-steps"></a>Volgende stappen

- [Meer informatie over limieten voor StorSimple systeem](storsimple-limits.md).
- [Informatie over het implementeren van uw oplossing StorSimple](storsimple-deployment-walkthrough-u2.md).

<!--Reference links-->
[1]: https://technet.microsoft.com/library/cc731844(v=WS.10).aspx
