<properties
   pageTitle="Toepassing afhankelijkheid Monitor (ADM) in Operations Management Suite (OMS) | Microsoft Azure"
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

# <a name="using-application-dependency-monitor-solution-in-operations-management-suite-oms"></a>Met behulp van toepassing afhankelijkheid Monitor oplossing in Operations Management Suite (OMS)
![Waarschuwingspictogram voor beheer](media/operations-management-suite-application-dependency-monitor/icon.png) Toepassing afhankelijkheid Monitor (ADM) automatisch detecteert componenten van toepassing op Windows en Linux-systemen en de communicatie tussen services worden toegewezen. U kunt uw servers laten weergeven als u hen beschouwen als onderling verbonden systemen die kritieke diensten te leveren.  Toepassing afhankelijkheid Monitor ziet u de verbindingen tussen servers, processen, en de poorten via een TCP-verbinding architectuur met geen configuratie nodig, andere dan de installatie van een agent.

Dit artikel beschrijft de details van de toepassing afhankelijkheid Monitor.  Zie voor meer informatie over het configureren van ADM en onboarding agents [configureren toepassing afhankelijkheid Monitor oplossing in Operations Management Suite (OMS)](operations-management-suite-application-dependency-monitor-configure.md)

>[AZURE.NOTE]Toepassing afhankelijkheid Monitor is momenteel in privé-voorbeeld.  U kunt de toegang tot de persoonlijke voorbeeld ADM op [https://aka.ms/getadm](https://aka.ms/getadm)vragen.
>
>Tijdens een persoonlijke voorvertoning, alle OMS-accounts hebben onbeperkt toegang tot ADM.  ADM-knooppunten zijn gratis, maar wordt logboek Analytics-gegevens voor de typen AdmComputer_CL en AdmProcess_CL worden gemeten als een andere oplossing.
>
>Nadat ADM public preview invoert, worden deze alleen beschikbaar voor klanten gratis en betaalde van inzicht & Analytics in het Kantoorbeheersysteem prijzen plannen.  Gratis tier rekeningen worden beperkt tot 5 ADM-knooppunten.  Als u aan de persoonlijke voorbeeld deelnemen en niet zijn ingeschreven in het Kantoorbeheersysteem prijzen plannen wanneer ADM public preview invoert, wordt op dat moment ADM uitgeschakeld. 


## <a name="use-cases-make-your-it-processes-dependency-aware"></a>Use-Cases: Controleer uw IT processen op de hoogte van afhankelijkheid

### <a name="discovery"></a>Discovery
ADM maakt automatisch een algemene verwijzing kaart van afhankelijkheden in uw servers, processen en services van 3e partij.  Het detecteert en wijst alle TCP-afhankelijkheden, identificatie van onverwachte, externe verbindingen 3de partij systemen u afhankelijk van en de onderlinge afhankelijkheden aan traditionele donkere gebieden van uw netwerk, zoals DNS en AD.  ADM ontdekt mislukte netwerkverbindingen die uw beheerde systemen probeert te maken, zodat u potentiële server onjuiste configuratie, storingen en netwerkproblemen te identificeren.

### <a name="incident-management"></a>Oproepbeheer
ADM schittering groot deel van het probleem isoleren door te tonen hoe systemen met elkaar zijn verbonden en elkaar beïnvloeden.  Naast de mislukte verbindingen kunnen informatie over de aangesloten clients identificeren van netwerktaakverdeling niet goed is geconfigureerd, verrassend of overmatige belasting van de kritieke diensten en rogue-clients zoals praten met productiesystemen developer-machines.  Geïntegreerde werkstromen met OMS wijzigen bijhouden ook kunt u zien of een wijzigingsgebeurtenis op een back-end-computer of service de oorzaak van een incident wordt uitgelegd.

### <a name="migration-assurance"></a>Migratie Assurance
ADM kunt u efficiënt plannen, versnellen en valideren Azure migraties, ervoor te zorgen dat er niets wordt achtergelaten en er geen onverwachte storingen.  U kunt alle onderling afhankelijke systemen die migreren samen en bepalen of u een werkend systeem is nog steeds voor de gebruikers of een kandidaat is voor het uit bedrijf nemen in plaats van migratie beoordelen systeemconfiguratie en capaciteit moeten vinden.  Na de verplaatsing is voltooid, kunt u controleren op de client laden en identiteit te verifiëren testsystemen en klanten verbinden.  Als de definities van uw subnet planning en firewall problemen hebt, wijst mislukte verbindingen in ADM-kaarten u naar de systemen die verbinding nodig.

### <a name="business-continuity"></a>Bedrijfscontinuïteit
Als u Azure Site herstellen en moet definiëren de volgorde recovery voor uw omgeving, ADM help automatisch wordt weergegeven hoe de systemen zijn afhankelijk van elkaar om ervoor te zorgen dat is het herstelplan betrouwbaar.  Door het kiezen van een kritieke server en clients bekijkt, kunt u de front-systemen die moeten worden hersteld nadat die kritieke server hersteld en is identificeren.  Afhankelijkheden van een kritieke server back-end kijkt, kunt u daarentegen systemen die moeten worden hersteld voordat u uw focus systeem wordt teruggezet identificeren.

### <a name="patch-management"></a>Patch Management
ADM verbetert uw gebruik van OMS systeem Update beoordeling door aan te geven die andere teams en servers afhankelijk van uw service, zijn zodat u deze vooraf in kennis stellen kunt voordat u uw systemen naar beneden voor het patchen.  Patchbeheer in OMS ADM ook verbetert door aan te geven of uw diensten beschikbaar zijn en goed verbonden nadat ze zijn hersteld en opnieuw gestart. 


## <a name="mapping-overview"></a>Overzicht van de toewijzing
ADM-agents verzamelen informatie over alle processen die TCP-verbinding op de server waar deze zijn geïnstalleerd, evenals details over de binnenkomende en uitgaande verbindingen voor elk proces.  Met behulp van de lijst van de Machine aan de linkerkant van de ADM-oplossing, worden machines met ADM agenten geselecteerd voor het visualiseren van hun afhankelijkheden gedurende een geselecteerde periode.  Machine afhankelijkheid focus op een specifieke computer worden toegewezen en weergeven van alle computers die directe TCP-clients of servers van de desbetreffende computer zijn.

![ADM-overzicht](media/operations-management-suite-application-dependency-monitor/adm-overview.png)

Machines kunnen worden uitgebreid in het toegewezen aan de actieve processen met actieve netwerkverbindingen weergeven tijdens de geselecteerde periode.  Wanneer een externe computer met een agent ADM Procesdetails weergeven is uitgevouwen, worden processen communiceert met de machine focus weergegeven.  Aan de linkerkant van de processen die zij verbinding met maken het aantal agentless front-end-computers verbinding maken in de machine focus aangeduid.  Als de focus-computer een verbinding met een back-end machine zonder een agent, dat back-end wordt aangegeven met een knooppunt in de kaart met het IPv4-adres en het knooppunt kan worden uitgevouwen om afzonderlijke poorten en services die met de machine focus communiceert te geven.

ADM toewijzingen weergeven standaard de laatste 10 minuten van afhankelijkheidsgegevens.  Met behulp van de besturingselementen voor tijd in de linkerbovenhoek, kunnen toewijzingen worden doorzocht voor historische tijd bereiken, tot aan een uur wide, weer te geven hoe afhankelijkheden gezocht in het verleden, bijvoorbeeld tijdens een incident of voordat een wijziging is opgetreden.    ADM-gegevens worden opgeslagen gedurende 30 dagen betaalde werkruimten en 7 dagen in de vrije werkruimten.

![Kaart met de eigenschappen van de geselecteerde computer machine](media/operations-management-suite-application-dependency-monitor/machine-map.png)

## <a name="failed-connections"></a>Mislukte verbindingen
Mislukte verbindingen in ADM-toewijzingen voor processen en computers, worden weergegeven met een rode stippellijn wordt weergegeven als een clientsysteem te bereiken, een proces of een poort is mislukt.  Mislukte verbindingen wordt gerapporteerd vanaf elk systeem met een geïmplementeerde ADM-agent als dat systeem is de poging van de mislukte verbinding.  ADM meet dit met inachtneming van de TCP-sockets die niet een verbinding tot stand brengen.  Dit kan het gevolg zijn van een firewall, een onjuiste configuratie van de client of server of een externe service is tijdelijk niet beschikbaar zijn. 

![Mislukte verbindingen](media/operations-management-suite-application-dependency-monitor/failed-connections.png)

Mislukte verbindingen kunnen helpen bij problemen met migratie validatie beveiligingsanalyse en algehele architectuur wat begrijpen.  Soms mislukt verbindingen zijn onschadelijk, maar ze vaak wijst direct een probleem, zoals een failover-omgeving plotseling steeds onbereikbaar,.. .of twee lagen niet meer kunt praten na de migratie van een wolk.  In de afbeelding, IIS en WebSphere beide actief zijn, maar ze kunnen geen verbinding maken. 

## <a name="computer-and-process-properties"></a>Computer en Proceseigenschappen
U kunt bij het navigeren in een map ADM, machines en processen krijgen meer context over hun eigenschappen selecteren.  Machines bevatten informatie over de DNS-naam, IPv4-adressen CPU en geheugen capaciteit, Type VM, besturingssysteemversie, laatste Reboot-tijd en de id's van de OMS en ADM-agents.

Procesgegevens worden verzameld uit het besturingssysteem metagegevens over het uitvoeren van processen, waaronder procesnaam Procesbeschrijving, gebruikersnaam en domein (in Windows), bedrijfsnaam, productnaam, versie van het product, werkmap, opdrachtregel en Begintijd proces.

![Proceseigenschappen](media/operations-management-suite-application-dependency-monitor/process-properties.png)

Het deelvenster Overzicht kunt u extra informatie over het proces van connectiviteit, met inbegrip van de afhankelijke poorten, binnenkomende en uitgaande verbindingen en verbindingen is mislukt. 

![Proces-overzicht](media/operations-management-suite-application-dependency-monitor/process-summary.png)

## <a name="oms-change-tracking-integration"></a>OMS bijhouden-integratie
Integratie met het bijhouden van ADM is automatisch als beide oplossingen zijn ingeschakeld en geconfigureerd in uw werkruimte OMS.

Het deelvenster Machine samenvatting geeft aan of gebeurtenissen bijhouden op de geselecteerde computer zijn opgetreden tijdens de geselecteerde periode.

![Samenvatting van de machine](media/operations-management-suite-application-dependency-monitor/machine-summary.png)

De Machine wijzigen bijhouden deelvenster bevat een overzicht van alle wijzigingen, met de meest recente eerste, en een koppeling naar details in het logboek zoeken voor meer informatie.
![Machine wijzigen traceren, deelvenster](media/operations-management-suite-application-dependency-monitor/machine-change-tracking.png)

Hier volgt een weergave van de gebeurtenis Change configuratie inzoomen na het selecteren van **weergeven in het logboek voor Analytics**.
![Configuration Change, gebeurtenis](media/operations-management-suite-application-dependency-monitor/configuration-change-event.png)


## <a name="log-analytics-records"></a>Logboekrecords Analytics
ADM van computer- en voorraadgegevens is beschikbaar voor [Zoeken](../log-analytics/log-analytics-log-searches.md) in logboek Analytics.  Dit kan worden toegepast op de scenario's zoals migratieplanning, capaciteitsanalyse discovery en ad hoc-prestaties problemen oplossen. 

Een record gegenereerd per uur voor elke computer uniek en proces naast records wanneer of de computer wordt gestart of in-aangehouden ADM. is  Deze records bevatten de eigenschappen in de volgende tabellen. 

Er zijn intern gegenereerde eigenschappen die u gebruiken kunt om unieke processen en computers te identificeren:

- PersistentKey_s wordt uniek gedefinieerd door de Procesconfiguratie, bijvoorbeeld vanaf de opdrachtregel en gebruikers-id  Deze is uniek voor een bepaalde computer, maar kan worden herhaald op alle computers.
- ProcessId_s en ComputerId_s zijn uniek in het ADM-model.



### <a name="admcomputercl-records"></a>AdmComputer_CL records
Records van het type **AdmComputer_CL** hebben voorraadgegevens voor servers met ADM-agenten.  Deze records bevatten de eigenschappen in de volgende tabel.  

| Eigenschap | Beschrijving |
|:--|:--|
| Type | *AdmComputer_CL* |
| SourceSystem | *OpsManager* |
| ComputerName_s | Windows- of Linux-computernaam |
| CPUSpeed_d | CPU-snelheid in MHz |
| DnsNames_s | Lijst van alle DNS-namen voor deze computer |
| IPv4s_s | Lijst van alle IPv4-adressen gebruikt door deze computer. |
| IPv6s_s | Lijst van alle IPv6-adressen gebruikt door deze computer.  (ADM IPv6-adressen wordt aangeduid, maar wordt IPv6-afhankelijkheden niet detecteren.) |
| Is64Bit_b | waar of onwaar op basis van het type besturingssysteem |
| MachineId_s | Een interne GUID die uniek zijn voor een werkruimte OMS  |
| OperatingSystemFamily_s | Windows- of Linux |
| OperatingSystemVersion_s | Lange tekenreeks van de OS-versie |
| TimeGenerated | Datum en tijd waarop de record is gemaakt. |
| TotalCPUs_d | Het aantal processor-cores |
| TotalPhysicalMemory_d | Geheugencapaciteit in MB |
| VirtualMachine_b | waar of onwaar op basis van of het OS VM Gast |
| VirtualMachineID_g | Hyper-V VM-ID |
| VirtualMachineName_g | Naam van de Hyper-V VM |
| VirtualMachineType_s | Hyperv, Vmware, Xen, Kvm, Ldom, Lpar, Virtualpc |


### <a name="admprocesscl-type-records"></a>Type AdmProcess_CL records 
Records met een soort **AdmProcess_CL** hebben voorraadgegevens voor TCP verbonden processen op servers met ADM-agenten.  Deze records bevatten de eigenschappen in de volgende tabel.

| Eigenschap | Beschrijving |
|:--|:--|
| Type | *AdmProcess_CL* |
| SourceSystem | *OpsManager* |
| CommandLine_s | Volledige opdrachtregel van het proces |
| CompanyName_s | Bedrijfsnaam (vanuit Windows PE of Linux RPM) |
| Description_s | Lang Procesbeschrijving (van Windows PE of Linux RPM) |
| FileVersion_s | Uitvoerbaar bestandsversie (van Windows PE alleen Windows) |
| FirstPid_d | OS proces-ID |
| InternalName_s | De interne naam van het uitvoerbare bestand (van Windows PE alleen Windows) |
| MachineId_s | Interne GUID die uniek zijn voor een werkruimte OMS  |
| Name_s | De uitvoerbare procesnaam |
| Path_s | Pad van het uitvoerbare bestand voor het systeem |
| PersistentKey_s | Interne GUID is uniek in deze computer |
| PoolId_d | Interne ID voor het verzamelen van processen op basis van vergelijkbare regels. |
| ProcessId_s | Interne GUID die uniek zijn voor een werkruimte OMS  |
| ProductName_s | Naam product (van Windows PE of Linux RPM) |
| ProductVersion_s | Product versie string (vanuit Windows PE of Linux RPM) |
| StartTime_t | Begintijd proces op de lokale computerklok |
| TimeGenerated | Datum en tijd waarop de record is gemaakt. |
| UserDomain_s | Domein van de eigenaar-proces (alleen Windows) |
| UserName_s | Naam van proceseigenaar (alleen Windows) |
| WorkingDirectory_s | Werkmap-proces |


## <a name="sample-log-searches"></a>Monster logboek zoeken

### <a name="list-the-physical-memory-capacity-of-all-managed-computers"></a>Een overzicht van de capaciteit van het fysieke geheugen van alle beheerde computers. 
Type = AdmComputer_CL | Selecteer TotalPhysicalMemory_d, ComputerName_s | Dedup ComputerName_s

![Voorbeeld van de query ADM](media/operations-management-suite-application-dependency-monitor/adm-example-01.png)

### <a name="list-computer-name-dns-ip-and-os-version"></a>Computernaam, DNS, IP- en OS-versie.
Type = AdmComputer_CL | Selecteer het ComputerName_s, OperatingSystemVersion_s, DnsNames_s, IPv4s_s | dedup ComputerName_s

![Voorbeeld van de query ADM](media/operations-management-suite-application-dependency-monitor/adm-example-02.png)

### <a name="find-all-processes-with-sql-in-the-command-line"></a>Alle processen met 'sql' te vinden op de opdrachtregel
Type = AdmProcess_CL CommandLine_s = \*sql\* | dedup ProcessId_s

![Voorbeeld van de query ADM](media/operations-management-suite-application-dependency-monitor/adm-example-03.png)

### <a name="after-viewing-event-data-for-given-process-use-its-machine-id-to-retrieve-the-computers-name"></a>Na het bekijken van gegevens voor de gegeven proces, de computer-ID gebruiken voor het ophalen van de naam van de computer
Type = "m!m-9bb187fa-e522-5f73-66d2-211164dc4e2b" AdmComputer_CL | Verschillende ComputerName_s

![Voorbeeld van de query ADM](media/operations-management-suite-application-dependency-monitor/adm-example-04.png)

### <a name="list-all-computers-running-sql"></a>Lijst met alle computers met SQL
Type = AdmComputer_CL MachineId_s IN {Type AdmProcess_CL = \*sql\* | Verschillende MachineId_s} | Verschillende ComputerName_s

![Voorbeeld van de query ADM](media/operations-management-suite-application-dependency-monitor/adm-example-05.png)

### <a name="list-of-all-unique-product-versions-of-curl-in-my-datacenter"></a>Lijst met alle unieke productversies van krul in mijn datacenter
Type = AdmProcess_CL Name_s = krul | Verschillende ProductVersion_s

![Voorbeeld van de query ADM](media/operations-management-suite-application-dependency-monitor/adm-example-06.png)

### <a name="create-a-computer-group-of-all-computers-running-centos"></a>Maak een computergroep van alle computers die zijn met CentOS

![Voorbeeld van de query ADM](media/operations-management-suite-application-dependency-monitor/adm-example-07.png)



## <a name="diagnostic-and-usage-data"></a>Diagnose en het gebruik van gegevens
Microsoft verzamelt automatisch gegevens gebruik en de prestaties door middel van het gebruik van de toepassing afhankelijkheid Monitor-service. Microsoft gebruikt deze gegevens te verstrekken en het verbeteren van de kwaliteit, veiligheid en integriteit van de toepassing afhankelijkheid Monitor-service. Gegevens bevat informatie over de configuratie van de software, zoals het besturingssysteem en de versie en bevat ook IP-adres en DNS-naam naam van werkstation zodat nauwkeurige en efficiënte probleemoplossing. Wij doen niet de namen, adressen of andere contactgegevens verzameld.

Zie de [Privacyverklaring van Microsoft Online Services](hhttps://go.microsoft.com/fwlink/?LinkId=512132)voor meer informatie over het verzamelen van gegevens en gebruik.



## <a name="next-steps"></a>Volgende stappen
- Meer informatie over [zoekopdrachten melden](../log-analytics/log-analytics-log-searches.md] in Log Analytics to retrieve data collected by Application Dependency Monitor.)
