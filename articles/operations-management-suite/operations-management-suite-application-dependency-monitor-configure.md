<properties
   pageTitle="Toepassing afhankelijkheid Monitor (ADM) configureren in Operations Management Suite (OMS) | Microsoft Azure"
   description="Toepassing afhankelijkheid Monitor (ADM) is een Operations Management Suite (OMS) oplossing die automatisch detecteert componenten van toepassing op Windows en Linux-systemen en de communicatie tussen services worden toegewezen.  Dit artikel bevat informatie voor ADM in uw omgeving implementeren en gebruiken in verschillende scenario's."
   services="operations-management-suite"
   documentationCenter=""
   authors="daseidma"
   manager="jwhit"
   editor="tysonn" />
<tags
   ms.service="operations-management-suite"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/28/2016"
   ms.author="daseidma;bwren" />

# <a name="configuring-application-dependency-monitor-solution-in-operations-management-suite-oms"></a>Toepassing afhankelijkheid Monitor oplossing in Operations Management Suite (OMS) configureren
![Waarschuwingspictogram voor beheer](media/operations-management-suite-application-dependency-monitor/icon.png) Toepassing afhankelijkheid Monitor (ADM) automatisch detecteert componenten van toepassing op Windows en Linux-systemen en de communicatie tussen services worden toegewezen. U kunt uw servers laten weergeven als u hen beschouwen als onderling verbonden systemen die kritieke diensten te leveren.  Toepassing afhankelijkheid Monitor ziet u de verbindingen tussen servers, processen, en de poorten via een TCP-verbinding architectuur met geen configuratie nodig, andere dan de installatie van een agent.

Dit artikel beschrijft de details van de toepassing afhankelijkheid Monitor en onboarding agents configureren.  Zie voor meer informatie over het gebruik van ADM [-oplossing met behulp van toepassing afhankelijkheid Monitor in Operations Management Suite (OMS)](operations-management-suite-application-dependency-monitor.md)

>[AZURE.NOTE]Toepassing afhankelijkheid Monitor is momenteel in privé-voorbeeld.  U kunt de toegang tot de persoonlijke voorbeeld ADM op [https://aka.ms/getadm](https://aka.ms/getadm)vragen.
>
>Tijdens een persoonlijke voorvertoning, alle OMS-accounts hebben onbeperkt toegang tot ADM.  ADM-knooppunten zijn gratis, maar wordt logboek Analytics-gegevens voor de typen AdmComputer_CL en AdmProcess_CL worden gemeten als een andere oplossing.
>
>Nadat ADM public preview invoert, worden deze alleen beschikbaar voor klanten gratis en betaalde van inzicht & Analytics in het Kantoorbeheersysteem prijzen plannen.  Gratis tier rekeningen worden beperkt tot 5 ADM-knooppunten.  Als u aan de persoonlijke voorbeeld deelnemen en niet zijn ingeschreven in het Kantoorbeheersysteem prijzen plannen wanneer ADM public preview invoert, wordt op dat moment ADM uitgeschakeld. 



## <a name="connected-sources"></a>Verbonden bronnen
De volgende tabel beschrijft de gekoppelde bronnen die worden ondersteund door de ADM-oplossing.

| Aangesloten bron | Ondersteund | Beschrijving |
|:--|:--|:--|
| [Windows-agenten](../log-analytics/log-analytics-windows-agents.md) | Ja | ADM analyseert en verzamelt gegevens uit een Windows-agent-computers.  <br><br>Houd er rekening mee dat naast de agent OMS Windows agenten zijn vereist voor de afhankelijkheid Microsoft Agent.  Zie de [ondersteunde besturingssystemen](#supported-operating-systems) voor een volledige lijst met versies van besturingssystemen. |
| [Linux-agenten](../log-analytics/log-analytics-linux-agents.md) | Ja | ADM analyseert en verzamelt gegevens van Linux agentcomputers.  <br><br>Houd er rekening mee dat naast de agent OMS Linux agenten zijn vereist voor de afhankelijkheid Microsoft Agent.  Zie de [ondersteunde besturingssystemen](#supported-operating-systems) voor een volledige lijst met versies van besturingssystemen. |
| [SCOM management groep](../log-analytics/log-analytics-om-agents.md) | Ja | ADM analyseert en verzamelt gegevens van Windows en Linux-agents in een verbonden SCOM management groep. <br><br>Een directe verbinding vanaf de computer van de agent SCOM met OMS is vereist. Gegevens verzonden rechtstreeks van uit de management-groep doorgestuurd naar de opslagplaats OMS.|
| [Azure opslag account](../log-analytics/log-analytics-azure-storage.md) | Nee | ADM verzamelt gegevens van agentcomputers, zodat er geen gegevens te verzamelen van Azure opslag. |

Houd er rekening mee dat ADM biedt alleen ondersteuning voor 64-bits platforms.

Op Windows, Microsoft Monitoring Agent (MMA) wordt gebruikt door zowel SCOM en OMS verzamelen en verzenden gegevens controleren.  (Deze agent heet de Agent SCOM, OMS-Agent, MMA of directe Agent, afhankelijk van de context.)  SCOM en OMS bieden verschillende uit het vak versies van MMA, maar deze versies kunnen elk rapport met SCOM, OMS of naar beide.  Op Linux, voor Linux worden verzameld en verzonden gegevens OMS controle de OMS-Agent.  U kunt ADM op servers met een directe OMS-agenten of op servers die zijn gekoppeld aan OMS via SCOM beheer groepen.  Voor de toepassing van deze documentatie, we verwijzen naar alle van deze agenten – op Linux of Windows, of verbonden met een MG SCOM of rechtstreeks met OMS – als de "OMS-Agent", tenzij de naam van de specifieke implementatie van de agent voor context nodig is.

De ADM-agent stuurt geen gegevens zelf en het vereist geen wijzigingen in de firewalls en poorten.  De ADM altijd gegevensoverdracht door de Agent OMS OMS, hetzij rechtstreeks, hetzij via de Gateway OMS.

![ADM-agenten](media/operations-management-suite-application-dependency-monitor/agents.png)

Als u een klant SCOM met een Management groep verbonden met OMS:

- Als de agenten SCOM toegang het internet verbinding maken met OMS tot, is geen aanvullende configuratie vereist.  
- Als de agenten SCOM OMS via het internet, moet u het werken met SCOM OMS-Gateway configureren.
  
Als u de directe OMS-Agent, moet u de OMS Agent zelf verbinding maken met OMS of aan uw Kantoorbeheersysteem Gateway configureren.  De OMS-Gateway kan worden gedownload van [https://www.microsoft.com/en-us/download/details.aspx?id=52666](https://www.microsoft.com/en-us/download/details.aspx?id=52666)


### <a name="avoiding-duplicate-data"></a>Voorkomen van dubbele gegevens

Als u een klant SCOM, moet u niet uw SCOM agents te communiceren rechtstreeks met OMS configureren of gegevens twee keer worden vermeld.  In ADM resulteert dit in computers die voorkomen in de lijst computer tweemaal.

Configuratie van OMS moet gebeuren in slechts een van de volgende locaties:

- Het deelvenster SCOM Console Operations Management Suite voor beheerde Computers
- Configuratie van Azure operationeel inzicht in de eigenschappen van de MMA

Beide configuraties gebruiken met dezelfde werkruimte in elk zal dubbele gegevens. Beide met verschillende werkruimten kan leiden tot conflicterende configuratie (één met ADM-oplossing is ingeschakeld en de ander zonder) is het dat mogelijk doorloopt tot ADM volledig.  

Houd er rekening mee dat zelfs als de computer zelf niet is opgegeven in de configuratie van de Console SCOM OMS, als een instantie zoals 'Windows Server-exemplaren Group' actief is, het nog steeds leiden de ontvangende OMS Computerconfiguratie via SCOM tot kan.



## <a name="management-packs"></a>Beheerpakketten
Wanneer ADM in een werkruimte OMS is geactiveerd, een 300KB Management Pack wordt verzonden naar alle de Microsoft controle agenten in die werkruimte.  Als u in een [groep management verbonden](../log-analytics/log-analytics-om-agents.md)agenten SCOM gebruikt, wordt het ADM Management Pack van SCOM worden geïmplementeerd.  Als de agents rechtstreeks zijn verbonden, worden de MP bezorgd door OMS.

Het Beheerderspaneel heet Microsoft.IntelligencePacks.ApplicationDependencyMonitor*.  Het is geschreven naar *%Programfiles%\Microsoft Monitoring Agent\Agent\Health State\Management servicepacks\*.  De gegevensbron die wordt gebruikt door het management pack is *% Program files%\Microsoft Monitoring Agent\Agent\Health Service State\Resources\<AutoGeneratedID > \Microsoft.EnterpriseManagement.Advisor.ApplicationDependencyMonitorDataSource.dll*.



## <a name="configuration"></a>Configuratie
Windows en Linux-computers hebben een agent geïnstalleerd en aangesloten op de OMS, de installateur afhankelijkheid Agent moet worden gedownload van de ADM-oplossing en vervolgens als root of Admin op elke beheerde server geïnstalleerd.  Zodra de ADM-agent is geïnstalleerd op een server rapporteren aan OMS weergegeven ADM afhankelijkheid toewijzingen binnen 10 minuten.  Als u problemen hebt, stuur e-mail [oms-adm-support@microsoft.com](mailto:oms-adm-support@microsoft.com).


### <a name="migrating-from-bluestripe-factfinder"></a>Migratie van BlueStripe FactFinder
Toepassing afhankelijkheid Monitor leveren in OMS in fasen BlueStripe technologie. FactFinder wordt nog steeds ondersteund voor bestaande klanten, maar is niet langer beschikbaar voor afzonderlijke aankoop.  Deze preview-versie van de Agent afhankelijkheid kan alleen communiceren met OMS.  Als u een bestaande klant van FactFinder, Identificeer u een set servers test voor ADM die niet worden beheerd door FactFinder. 

### <a name="download-the-dependency-agent"></a>De Agent afhankelijkheid downloaden
Naast de Microsoft Management Agent (MMA) en OMS Linux-Agent die de verbinding tussen de computer en OMS moeten alle geanalyseerd door afhankelijkheid Monitor van toepassing zijn de afhankelijkheid Agent is geïnstalleerd.  Op Linux, moet de Agent OMS voor Linux zijn geïnstalleerd voordat de Agent afhankelijkheid. 

![Toepassing afhankelijkheid Monitor naast elkaar](media/operations-management-suite-application-dependency-monitor/tile.png)

Om de afhankelijkheid Agent downloaden, klikt u op **Oplossing configureren** in de tegel **Toepassing afhankelijkheid Monitor** openen de **Afhankelijkheid Agent** blade.  De Agent afhankelijkheid blade bevat koppelingen voor de Windows- en de Linux-agenten. Klik op de link voor het downloaden van elke agent. Zie de volgende secties voor meer informatie over het installeren van de agent op verschillende systemen.

### <a name="install-the-dependency-agent"></a>De afhankelijkheid Agent installeren

#### <a name="microsoft-windows"></a>Microsoft Windows
Installeren of verwijderen van de agent zijn beheerdersbevoegdheden vereist.

De afhankelijkheid Agent is geïnstalleerd op Windows-computers met ADM-Agent-Windows.exe. Als u dit uitvoerbare bestand zonder opties wordt uitgevoerd, wordt er een wizard die u volgen kunt om de installatie interactief uitvoeren gestart.  

Gebruik de volgende stappen de afhankelijkheid-Agent installeren op elke Windows-computer.

1.  Zorg ervoor dat de OMS-agent wordt geïnstalleerd volgens de aanwijzingen op het verbinden van computers rechtstreeks aan OMS.
2.  De agent Windows downloadt en uitvoert met de volgende opdracht.<br>*ADM-Agent-Windows.exe*
3.  Volg de wizard om de agent te installeren.
4.  Als de Agent afhankelijkheid niet kunt starten, controleert u de logboeken voor gedetailleerde foutgegevens. Op Windows-agenten is de logboekmap *C:\Program Files\BlueStripe\Collector\logs*. 

De Agent voor Windows van de afhankelijkheid kan worden verwijderd door een beheerder via het Configuratiescherm.


#### <a name="linux"></a>Linux
Toegang tot de hoofdmap is vereist voor het installeren of configureren van de agent.

De afhankelijkheid Agent is geïnstalleerd op de computers Linux met een shellscript met een zelf-uitpakkend binaire ADM-Agent-Linux64.bin. U kunt het bestand met sh of toevoegen uitvoeren machtigingen voor het bestand zelf.
 
Gebruik de volgende stappen de afhankelijkheid-Agent installeren op elke computer met Linux.

1.  Zorg ervoor dat de OMS-agent wordt geïnstalleerd volgens de instructies op [verzamelen en beheren van gegevens van Linux-computers.  Dit moet worden geïnstalleerd voordat de Agent Linux afhankelijkheid](https://technet.microsoft.com/library/mt622052.aspx).
2.  De afhankelijkheid van de Linux-agent installeren als root met de volgende opdracht.<br>*sh ADM-Agent-Linux64.bin*.
3.  Als de Agent afhankelijkheid niet kunt starten, controleert u de logboeken voor gedetailleerde foutgegevens. Op Linux-agenten is de logboekmap */var/opt/microsoft/dependency-agent/log*.

### <a name="uninstalling-the-dependency-agent-on-linux"></a>De Agent afhankelijkheid op Linux verwijderen
Als u wilt volledig verwijderen van de Agent afhankelijk van Linux, moet u de agent zelf en de proxy die automatisch met de agent is geïnstalleerd.  U kunt met de volgende enkele opdracht verwijderen.

    rpm -e dependency-agent dependency-agent-connector


### <a name="installing-from-a-command-line"></a>Installeren vanaf een opdrachtregel
De vorige sectie bevat over het installeren van de afhankelijkheid Netwerkmonitor standaardopties.  In de volgende secties bevatten richtlijnen voor het installeren van de agent vanaf een opdrachtregel met aangepaste opties.

#### <a name="windows"></a>Windows
Met opties uit de onderstaande tabel kunt u de installatie uitvoert vanaf een opdrachtregel. Voor een overzicht van de installatie uitgevoerd vlaggen de installer met de /? markeren als volgt.

    ADM-Agent-Windows.exe /?

| Vlag | Beschrijving |
|:--|:--|
| /S | Een stille installatie zonder vragen gebruiker uitvoeren. |

Standaard worden bestanden voor Windows afhankelijkheid Agent in *C:\Program Files\BlueStripe\Collector* geplaatst.


#### <a name="linux"></a>Linux
Opties uit de onderstaande tabel gebruiken voor het uitvoeren van de installatie. Voor een overzicht van de installatie uitgevoerd vlaggen de installatie programma dankzij - vlag als volgt.

    ADM-Agent-Linux64.bin -help

| Beschrijving van de vlag
|:--|:--|
| -s | Een stille installatie zonder vragen gebruiker uitvoeren. |
| --controleren | Machtigingen en het besturingssysteem wordt gecontroleerd, maar de agent kan niet worden geïnstalleerd. |

Bestanden voor de Agent afhankelijkheid worden in de volgende mappen geplaatst.

| Bestanden | Locatie |
|:--|:--|
| Core-bestanden | /usr/lib/bluestripe-collector |
| Logboekbestanden | /var/opt/Microsoft/Dependency-agent/log |
| Config-bestanden | /etc/opt/Microsoft/Dependency-agent/config |
| Service uitvoerbare bestanden | /sbin/bluestripe-collector<br>/sbin/bluestripe-collector-Manager |
| Bestanden in binaire opslag | /var/opt/Microsoft/Dependency-agent/Storage |



## <a name="troubleshooting"></a>Het oplossen van problemen
Als u problemen met toepassing afhankelijkheid Monitor ondervindt, kunt u informatie verzamelen uit meerdere componenten met behulp van de volgende informatie.

### <a name="windows-agents"></a>Windows-agenten

#### <a name="microsoft-dependency-agent"></a>Afhankelijkheid van Microsoft Agent
Genereren van gegevens voor het oplossen van problemen van de Agent van de afhankelijkheid, open een opdrachtprompt als administrator en het CollectBluestripeData.vbs script met de volgende opdracht uitvoeren.  U kunt toevoegen de--help vlag worden extra opties weergegeven.

    cd C:\Program Files\Bluestripe\Collector\scripts
    cscript CollectDependencyData.vbs

De Support-gegevenspakket wordt opgeslagen in de map % USERPROFILE % voor de huidige gebruiker.  U kunt het - bestand <filename> optie wilt opslaan op een andere locatie.

#### <a name="microsoft-dependency-agent-management-pack-for-mma"></a>Afhankelijkheid van Microsoft Agent Management Pack voor MMA
Het Management Pack afhankelijkheid Agent wordt uitgevoerd in Microsoft Management Agent.  Deze gegevens worden ontvangen van de Agent van de afhankelijkheid en stuurt het naar de ADM cloud-service.
  
Controleer of het management pack wordt gedownload door de volgende stappen uit te voeren.

1.  Zoek een bestand genaamd Microsoft.IntelligencePacks.ApplicationDependencyMonitor.mp in C:\Program Files\Microsoft Monitoring Agent\Agent\Health State\Management servicepacks.  
2.  Als het bestand niet aanwezig is en de agent is verbonden met een groep SCOM management, controleren dat deze wordt geïmporteerd in SCOM door beheerpakketten in de werkruimte van het beheer van de Console van de verrichtingen te controleren.

Het Beheerderspaneel ADM gebeurtenissen vastgelegd in het gebeurtenislogboek van Windows van Operations Manager.  Het logboek kan worden [gezocht in de OMS](../log-analytics/log-analytics-log-searches.md) via de system log-oplossing, waar u kunt configureren welke logboekbestanden te uploaden.  Als gebeurtenissen voor foutopsporing is ingeschakeld, wordt deze geschreven naar het gebeurtenislogboek van de toepassing met de bron van de gebeurtenis *AdmProxy*.

#### <a name="microsoft-monitoring-agent"></a>Microsoft Monitoring Agent
Traceren van diagnostische informatie verzamelen, open een opdrachtprompt als administrator en voer de volgende opdrachten: 

    cd \Program Files\Microsoft Monitoring Agent\Agent\Tools
    net stop healthservice 
    StartTracing.cmd ERR
    net start healthservice

Sporen worden geschreven naar c:\Windows\Logs\OpsMgrTrace.  U kunt de tracering met StopTracing.cmd stoppen.


### <a name="linux-agents"></a>Linux-agenten

#### <a name="microsoft-dependency-agent"></a>Afhankelijkheid van Microsoft Agent
Analytische gegevens genereren van de Agent van de afhankelijkheid, meld u aan met een account met sudo of root privileges en voer de volgende opdracht uit.  U kunt toevoegen de--help vlag worden extra opties weergegeven.

    /usr/lib/bluestripe-collector/scripts/collect-dependency-agent-data.sh

De Support-gegevenspakket wordt opgeslagen in /var/opt/microsoft/dependency-agent/log (als root) onder de installatiemap van de Agent, of naar de basismap van de gebruiker die het script (als niet-root).  U kunt het - bestand <filename> optie wilt opslaan op een andere locatie.

#### <a name="microsoft-dependency-agent-fluentd-plug-in-for-linux"></a>Afhankelijkheid van Microsoft Agent Fluentd Plug-in voor Linux
De invoegtoepassing Fluentd van afhankelijkheid Agent wordt uitgevoerd binnen de OMS Linux-Agent.  Deze gegevens worden ontvangen van de Agent van de afhankelijkheid en stuurt het naar de ADM cloud-service.  

Logboekbestanden worden geschreven naar de volgende twee bestanden.

- /var/opt/Microsoft/omsagent/log/omsagent.log
- /var/log/messages

#### <a name="oms-agent-for-linux"></a>OMS Agent voor Linux
Een bron voor probleemoplossing voor de verbinding met OMS Linux servers vindt u hier: [https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/Troubleshooting.md](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/Troubleshooting.md) 

De logboeken voor het Kantoorbeheersysteem Agent voor Linux bevinden zich in */var/opt/microsoft/omsagent/log/*.  

De logboeken voor omsconfig (agentconfiguratie) bevinden zich in */var/opt/microsoft/omsconfig/log/*.
 
Het logboek voor de OMI en SCX onderdelen die metrische gegevens leveren bevinden zich in */var/opt/omi/log/* en */var/opt/microsoft/scx/log*.


## <a name="data-collection"></a>Het verzamelen van gegevens
U kunt elke agent voor het verzenden van ongeveer 25MB per dag, afhankelijk van hoe complex de afhankelijkheden van uw systeem worden verwacht.  ADM worden afhankelijkheid verzonden door elke agent elke 15 seconden.  

De ADM-Agent verbruikt doorgaans 0,1% van het systeemgeheugen en 0,1% van CPU-systeem.


## <a name="supported-operating-systems"></a>Ondersteunde besturingssystemen
Hieronder worden de ondersteunde besturingssystemen voor de Agent afhankelijkheid.   32-bits architecturen worden niet ondersteund voor elk besturingssysteem.

### <a name="windows-server"></a>Windows Server
- Windows Server 2012 R2
- Windows Server 2012
- Windows Server 2008 R2 SP1

### <a name="windows-desktop"></a>Windows-bureaublad
- Opmerking: Windows 10 is nog niet ondersteund
- Windows 8.1
- Windows 8
- Windows 7

### <a name="red-hat-enterprise-linux-centos-linux-and-oracle-linux-with-rhel-kernel"></a>Red Hat Enterprise Linux, Linux CentOS en Oracle Linux (met RHEL Kernel)
- Alleen standaard en SMP Linux kernel versies worden ondersteund.
- Niet-standaard kernel releases zoals PAE en Xen, worden niet ondersteund voor elke Linux-distributie. Bijvoorbeeld, wordt een systeem met de tekenreeks van de versie van '2.6.16.21-0.8-xen' niet ondersteund.
- Aangepaste kernels, waaronder compilaties van standaard kernels, worden niet ondersteund.
- Centos Plus kernel wordt niet ondersteund.
- Oracle Unbreakable Kernel (UEK) valt in een andere sectie hieronder.


#### <a name="red-hat-linux-7"></a>Red Hat Linux 7
| Versie van het besturingssysteem | Kernelversie |
|:--|:--|
| 7.0 | 3.10.0-123 |
| 7.1 | 3.10.0-229 |
| 7.2 | 3.10.0-327 |

#### <a name="red-hat-linux-6"></a>Red Hat Linux 6
| Versie van het besturingssysteem | Kernelversie |
|:--|:--|
| 6.0 | 2.6.32-71 |
| 6.1 | 2.6.32-131 |
| 6.2 | 2.6.32-220 |
| 6.3 | 2.6.32-279 |
| 6.4 | 2.6.32-358 |
| 6.5 | 2.6.32-431 |
| 6.6 | 2.6.32-504 |
| 6.7 | 2.6.32-573 |
| 6,8 | 2.6.32-642 |

#### <a name="red-hat-linux-5"></a>Red Hat Linux 5
| Versie van het besturingssysteem | Kernelversie |
|:--|:--|
| 5.8 | 2.6.18-308 |
| 5.9 | 2.6.18-348 |
| 5.10 | 2.6.18-371 |
| 5.11 | 2.6.18-398<br>2.6.18-400<br>2.6.18-402<br>2.6.18-404<br>2.6.18-406<br>2.6.18-407<br>2.6.18-408<br>2.6.18-409<br>2.6.18-410<br>2.6.18-411 |

#### <a name="oracle-enterprise-linux-w-unbreakable-kernel-uek"></a>Oracle Enterprise Linux met Unbreakable Kernel (UEK)

#### <a name="oracle-linux-6"></a>Oracle Linux 6
| Versie van het besturingssysteem | Kernelversie
|:--|:--|
| 6.2 | Oracle 2.6.32-300 (R1 UEK) |
| 6.3 | Oracle 2.6.39-200 (UEK R2) |
| 6.4 | Oracle 2.6.39-400 (UEK R2) |
| 6.5 | Oracle 2.6.39-400 (UEK R2 i386) |
| 6.6 | Oracle 2.6.39-400 (UEK R2 i386) |

#### <a name="oracle-linux-5"></a>Oracle Linux 5

| Versie van het besturingssysteem | Kernelversie
|:--|:--|
| 5.8 | Oracle 2.6.32-300 (R1 UEK) |
| 5.9 | Oracle 2.6.39-300 (UEK R2) |
| 5.10 | Oracle 2.6.39-400 (UEK R2) |
| 5.11 | Oracle 2.6.39-400 (UEK R2) |

#### <a name="suse-linux-enterprise-server"></a>SUSE Linux Enterprise Server

#### <a name="suse-linux-11"></a>SUSE Linux 11
| Versie van het besturingssysteem | Kernelversie
|:--|:--|
| 11 | 2.6.27 |
| 11 SP1 | 2.6.32 |
| 11 SP2 | 3.0.13 |
| 11 SP3 | 3.0.76 |
| 11 SP4 | 3.0.101 |

#### <a name="suse-linux-10"></a>SUSE Linux 10
| Versie van het besturingssysteem | Kernelversie
|:--|:--|
| 10 SP4 | 2.6.16.60 |

## <a name="diagnostic-and-usage-data"></a>Diagnose en het gebruik van gegevens
Microsoft verzamelt automatisch gegevens gebruik en de prestaties door middel van het gebruik van de toepassing afhankelijkheid Monitor-service. Microsoft gebruikt deze gegevens te verstrekken en het verbeteren van de kwaliteit, veiligheid en integriteit van de toepassing afhankelijkheid Monitor-service. Gegevens bevat informatie over de configuratie van de software, zoals het besturingssysteem en de versie en bevat ook IP-adres en DNS-naam naam van werkstation zodat nauwkeurige en efficiënte probleemoplossing. Wij doen niet de namen, adressen of andere contactgegevens verzameld.

Zie de [Privacyverklaring van Microsoft Online Services](https://go.microsoft.com/fwlink/?LinkId=512132)voor meer informatie over het verzamelen van gegevens en gebruik.



## <a name="next-steps"></a>Volgende stappen
- Informatie over hoe [toepassing afhankelijkheid beeldscherm](operations-management-suite-application-dependency-monitor.md) eenmaal is geïmplementeerd en geconfigureerd.
