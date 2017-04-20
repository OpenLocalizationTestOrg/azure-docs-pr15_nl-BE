<properties
    pageTitle="Oplossing voor capaciteit in logboek Analytics | Microsoft Azure"
    description="U kunt de capaciteitsplanning oplossing in Analytics logboek u inzicht geven in de capaciteit van de Hyper-V-servers beheerd door System Center Virtual Machine Manager"
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="banders"/>

# <a name="capacity-management-solution-in-log-analytics"></a>Oplossing voor capaciteit in logboek Analytics


U kunt de capaciteitsplanning oplossing in Analytics logboek u inzicht geven in de capaciteit van de Hyper-V-servers beheerd door System Center Virtual Machine Manager. Deze oplossing vereist System Center Operations Manager en System Center Virtual Machine Manager. Capacity Planning is niet beschikbaar als u alleen direct verbonden agenten. U installeert de oplossing om de agent Operations Manager bijwerken. De oplossing prestatiemeteritems op de gecontroleerde server leest en gebruiksgegevens naar de OMS-service in de cloud voor verwerking. Logica wordt toegepast op de gegevens over het gebruik en de cloud-service de gegevens worden vastgelegd. Na verloop van tijd gebruikspatronen worden geïdentificeerd en capaciteit wordt geprojecteerd op basis van het huidige verbruik.

Een projectie kan bijvoorbeeld worden aangegeven als extra processor-cores of extra geheugen nodig voor een afzonderlijke is. In dit voorbeeld wordt kan de projectie wijzen op dat in 30 dagen de server extra geheugen moeten. Zo kunt u plannen voor een geheugenupgrade tijdens van de server volgende onderhoudsvenster, welke eenmaal per twee weken optreden.

>[AZURE.NOTE] De oplossing voor capaciteit is niet kan worden toegevoegd aan de werkruimten. Klanten die beschikken over de capaciteit-oplossing geïnstalleerd kunnen blijven gebruiken van de oplossing.  

De oplossing voor de capaciteitsplanning is worden bijgewerkt om de volgende klant gerapporteerde problemen:

- Behoefte aan gebruik Virtual Machine Manager en Operations Manager
- Onvermogen om aanpassen/filter op basis van groepen
- Per uur Gegevenssamenvoeging niet frequent genoeg
- Geen niveau inzichten VM
- Betrouwbaarheid van gegevens

Voordelen van de nieuwe capaciteit oplossing:

- Ondersteuning voor het verzamelen van gedetailleerde gegevens met verbeterde betrouwbaarheid en nauwkeurigheid
- Ondersteuning voor Hyper-V zonder VMM
- Visualisatie van parameters in PowerBI
- Inzichten op niveau VM-gebruik


## <a name="installing-and-configuring-the-solution"></a>Installeren en configureren van de oplossing
Gebruik de volgende informatie te installeren en configureren van de oplossing.

- Operations Manager is vereist voor de oplossing van het beheer van de capaciteit.
- Virtual Machine Manager is vereist voor de oplossing van het beheer van de capaciteit.
- Operations Manager-connectiviteit met Virtual Machine Manager (VMM) is vereist. Zie voor meer informatie over het aansluiten van de systemen, [het aansluiten van VMM met Operations Manager](http://technet.microsoft.com/library/hh882396.aspx).
- Operations Manager moet worden verbonden met Analytics logboek.
- De oplossing voor capaciteit toevoegen aan uw Kantoorbeheersysteem werkruimte met behulp van het proces beschreven in [oplossingen logboek Analytics toevoegen uit de galerie met oplossingen](log-analytics-add-solutions.md).  Er is geen verdere configuratie nodig.


## <a name="capacity-management-data-collection-details"></a>Capaciteit Management data collectie details

Capaciteit Management verzamelt prestatiegegevens, metagegevens en staat gegevens met behulp van de agenten die u hebt ingeschakeld.

De volgende tabel bevat de methoden van de collectie gegevens en andere informatie over de manier waarop gegevens worden verzameld voor het capaciteitsbeheer.

| platform | Directe Agent | SCOM agent | Azure opslag | SCOM nodig? | SCOM agent gegevens verzonden via management groep | frequentie van verzameling |
|---|---|---|---|---|---|---|
|Windows|![Nee](./media/log-analytics-capacity/oms-bullet-red.png)|![Ja](./media/log-analytics-capacity/oms-bullet-green.png)|![Nee](./media/log-analytics-capacity/oms-bullet-red.png)|            ![Ja](./media/log-analytics-capacity/oms-bullet-green.png)|![Ja](./media/log-analytics-capacity/oms-bullet-green.png)| per uur|

In de volgende tabel ziet u voorbeelden van gegevenstypen zijn verzameld door het Management capaciteit:

|**Gegevenstype**|**Velden**|
|---|---|
|Metagegevens|BaseManagedEntityId, ObjectStatus, OrganizationalUnit, ActiveDirectoryObjectSid, PhysicalProcessors, netwerknaam, IP-adres, ForestDNSName, NetbiosComputerName, VirtualMachineName, LastInventoryDate, HostServerNameIsVirtualMachine, IP-adres, NetbiosDomainName, LogicalProcessors, DNS-naam, naam, DomainDnsName, ActiveDirectorySite, PrincipalName, OffsetInMinuteFromGreenwichTime|
|Prestaties|Objectnaam, CounterName, PerfmonInstanceName, PerformanceDataId, PerformanceSourceInternalID, SampleValue, TimeSampled, TimeAdded|
|Staat|StateChangeEventId, StateId, NewHealthState, OldHealthState, Context, TimeGenerated, TimeAdded, StateId2, BaseManagedEntityId, MonitorId, HealthState, LastModified, LastGreenAlertGenerated, DatabaseTimeModified|

## <a name="capacity-management-page"></a>Capaciteit beheerpagina


 Nadat de capaciteitsplanning oplossing is geïnstalleerd, kunt u de capaciteit van de gecontroleerde servers bekijken met behulp van de tegel **Capaciteitsplanning** op de pagina **Overzicht** van OMS.

![afbeelding van de tegel capaciteitsplanning](./media/log-analytics-capacity/oms-capacity01.png)

De tegel wordt geopend de **Capaciteit Management** dashboard waar u een overzicht van de servercapaciteit van de kunt bekijken. De volgende tegels waarop u kunt klikken op de pagina weergegeven:

- *Virtuele machine count*: geeft het aantal resterende dagen aan voor de capaciteit van virtuele machines
- *Berekenen*: toont processorcores en het beschikbare geheugen
- *Opslag*: ziet u de beschikbare schijfruimte en de gemiddelde latentie van schijf
- *Zoeken*: de explorer gegevens die u om te zoeken naar gegevens in het systeem OMS gebruiken kunt

![afbeelding van capaciteitsbeheer dashboard](./media/log-analytics-capacity/oms-capacity02.png)


### <a name="to-view-a-capacity-page"></a>Een geïnstalleerde capaciteit

- Klik op **Beheer van de capaciteit**op de pagina **Overzicht** en klik op **berekenen** of **opslag**.

## <a name="compute-page"></a>Pagina berekenen

U kunt het dashboard **berekenen** in Microsoft Azure OMS capaciteit informatie weergeven over gebruik, dagen van de capaciteit, verwachte en efficiëntie die betrekking hebben op uw infrastructuur. U het gebied van het **gebruik van** CPU-core en geheugen gebruik in uw hosts virtuele machine bekijken. U kunt het hulpprogramma projectie schatten hoeveel capaciteit is naar verwachting beschikbaar voor een bepaald datumbereik. Het gebied van **efficiëntie** kunt u zien hoe efficiënt uw virtuele machinehosts zijn. U kunt details over gekoppelde items weergeven door erop te klikken.

U kunt genereren een Excel-werkmap voor de volgende categorieën:

- Top hosts met hoogste core-gebruik
- Top hosts met hoogste geheugengebruik
- Top hosts met inefficiënte virtuele machines
- Top hosts, door het gebruik van
- Onder hosts door gebruik

![afbeelding van de pagina capaciteit Management berekenen](./media/log-analytics-capacity/oms-capacity03.png)


De volgende gebieden worden weergegeven in het dashboard **berekenen** :

**Gebruik**: weergave CPU core- en gebruik in uw hosts virtuele machine.

- *Kernen gebruikt*: som voor alle hosts (% van de CPU gebruikt vermenigvuldigd met het aantal fysieke cores op host).
- *Gratis Cores*: totale fysieke cores min gebruikte cores.
- *Percentage Cores beschikbaar*: fysieke cores, gedeeld door het totale aantal fysieke cores vrij.
- *Virtuele Cores per VM*: totale virtuele cores in het systeem gedeeld door het totale aantal virtuele machines in het systeem.
- *Virtuele Cores aan fysieke Cores verhouding*: de verhouding van het totale fysieke cores aan fysieke cores die worden gebruikt door virtuele machines in het systeem.
- *Het aantal virtuele Cores beschikbaar*: virtuele core fysieke cores verhouding vermenigvuldigd met de beschikbare fysieke cores.
- *Geheugen gebruikt*: de som van het geheugen dat wordt gebruikt door alle hosts.
- *Geheugen*: totaal fysiek geheugen min geheugen gebruikt.
- *Percentage geheugen beschikbaar*: fysieke geheugen gedeeld door het totale fysieke geheugen vrij te maken.
- *Virtueel geheugen per VM*: Totaal virtueel geheugen in het systeem wordt gedeeld door het totale aantal virtuele machines in het systeem.
- *Virtueel geheugen naar verhouding van het fysieke geheugen*: Totaal virtueel geheugen in het systeem wordt gedeeld door het totale fysieke geheugen in het systeem.
- *Beschikbaar virtueel geheugen*: virtueel geheugen aan fysiek geheugen verhouding vermenigvuldigd met het beschikbare fysieke geheugen.

**Projectie, gereedschap**

U kunt met het hulpprogramma projectie, historische trends weergeven voor het gebruik van bronnen. Dit omvat het gebruik van trends voor virtuele machines, geheugen, core en opslag. De mogelijkheid projectie gebruikt een algoritme projectie om te weten wanneer u niet op elk van de resources. Dit helpt u de juiste capaciteit plannen zodat u weet wanneer u moet aanschaffen, meer capaciteit (zoals geheugen, cores of opslag) wordt berekend.

**Efficiëntie**

- *Actieve VM*: minder dan 10% van de CPU en 10% geheugen gebruiken voor de opgegeven periode.
- *Overutilized VM*: met meer dan 90% van het geheugen CPU en 90% voor de periode.
- *Host inactief*: minder dan 10% van de CPU en 10% geheugen gebruiken voor de opgegeven periode.
- *Host Overutilized*: meer dan 90% van de CPU en 90% geheugen gebruiken voor de opgegeven periode.

### <a name="to-work-with-items-on-the-compute-page"></a>Werken met items op de pagina berekenen

1. Op het dashboard **berekenen** in het gebied **Gebruik** capaciteit informatie weergeven over de CPU-kernen en het geheugen in gebruik.
2. Klik op een item te openen op **de pagina** en gedetailleerde informatie over het weergeven.
3. Verplaats de schuifknop datum weer te geven van een projectie van de capaciteit die wordt gebruikt op de datum die u kiest in het hulpprogramma **projectie** .
4. In het gebied van **efficiëntie** , capaciteit efficiëntie informatie weergeven over virtuele machines en hosts virtuele machine.

## <a name="direct-attached-storage-page"></a>Direct Attached Storage-pagina

U kunt het dashboard van **Direct Attached Storage** in OMS capaciteit informatie weergeven over opslag, schijfprestaties en de geschatte dagen van de schijfcapaciteit. Het gebied **Gebruik** kunt u weer gebruik van schijfruimte in de virtuele machine-hosts. Het gebied van **Schijfprestaties** kunt u weergeven schijfdoorvoer en latentie in de virtuele machine-hosts. U kunt ook het hulpprogramma projectie te schatten hoeveel capaciteit is naar verwachting beschikbaar voor een bepaald datumbereik. U kunt details over gekoppelde items weergeven door erop te klikken.

U kunt een Excel-werkmap genereren op basis van deze informatie over de capaciteit voor de volgende categorieën:

- Bovenste gebruik van schijfruimte door host
- Hoogste gemiddelde latentie van host

De volgende gebieden worden weergegeven op de pagina van de **opslag** :

- *Gebruik*: gebruik van schijfruimte in de virtuele machinehosts bekijken.
- *Totale schijfruimte*: som (logische schijfruimte) voor alle hosts
- *Schijfruimte gebruikt*: som (gebruikte logische schijfruimte) voor alle hosts
- *Beschikbare schijfruimte*: totale schijfruimte minus de gebruikte schijfruimte
- *Percentage schijf gebruikt*: schijfruimte gedeeld door de totale hoeveelheid schijfruimte gebruikt
- *Percentage schijfruimte beschikbaar*: beschikbare schijfruimte gedeeld door de totale schijfruimte

![afbeelding van de pagina capaciteit Management Direct Attached Storage](./media/log-analytics-capacity/oms-capacity04.png)

**Prestaties van de schijf**

U kunt de historische ontwikkeling van de ruimte op de schijf weergeven met behulp van OMS. De mogelijkheid projectie gebruikt een algoritme aan het toekomstige gebruik van project. Voor het gebruik van schijfruimte in het bijzonder kunt de projectie-mogelijkheid u project wanneer er onvoldoende ruimte op de schijf. Hierdoor kunt u de juiste opslag plannen en weet waarop u moet meer opslagruimte kopen.

**Projectie, gereedschap**

U kunt met het hulpprogramma projectie, historische trends weergeven voor uw schijf ruimtegebruik. De projectie-mogelijkheid kunt u project als u weinig schijfruimte. Hierdoor kunt u de juiste capaciteit plannen en weet wanneer u moet kopen meer opslagcapaciteit.

### <a name="to-work-with-items-on-the-direct-attached-storage-page"></a>Werken met items op de pagina Direct Attached Storage

1. In het gebied **Gebruik** op het dashboard **Direct Attached Storage** , kunt u de gegevens over gebruik weergeven.
2. Klik op een gekoppeld item te openen op **de pagina** en gedetailleerde informatie over het weergeven.
3. In het gebied **Schijfprestaties** kunt u schijfinformatie doorvoer en latentie weergeven.
4. Verplaats de schuifknop datum weer te geven van een projectie van de capaciteit die wordt gebruikt op de datum die u kiest in het **hulpprogramma voor projectie**.


## <a name="next-steps"></a>Volgende stappen

- U [zoekopdrachten in Analytics logboek logboek](log-analytics-log-searches.md) capaciteit gedetailleerde gegevens weergeven.
