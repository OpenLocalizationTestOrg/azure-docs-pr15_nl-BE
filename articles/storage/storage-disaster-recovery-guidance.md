<properties
    pageTitle="Wat te doen in geval van een stroomstoring Azure opslag | Microsoft Azure"
    description="Wat te doen in geval van een stroomstoring Azure opslag"
    services="storage"
    documentationCenter=".net"
    authors="robinsh"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="08/03/2016"
    ms.author="robinsh"/>


# <a name="what-to-do-if-an-azure-storage-outage-occurs"></a>Wat te doen als een Azure opslag uitgevallen

Bij Microsoft werken we hard om ervoor te zorgen dat onze diensten zijn altijd beschikbaar. Soms zorgt ervoor dat buiten onze invloed ons op manieren die ongeplande storingen in een of meer regio's veroorzaken. Om u te helpen deze zeldzame exemplaren te verwerken, bieden we de volgende algemene richtlijnen voor Azure Storage-services.

## <a name="how-to-prepare"></a>Het voorbereiden van 

Het is essentieel voor elke klant in hun eigen calamiteitenplan voorbereiden. De inspanning meestal herstellen na een stroomstoring opslag omvat zowel operationele staf en geautomatiseerde procedures om het opnieuw activeren van uw toepassingen in een functionerende staat. Raadpleeg de onderstaande te bouwen uw eigen calamiteitenplan Azure documentatie:

-   [Noodherstel en hoge beschikbaarheid voor Azure toepassingen](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md)

-   [Technische richtsnoeren Azure tolerantie](../resiliency/resiliency-technical-guidance.md)

-   [Azure-Site-Recovery-service](https://azure.microsoft.com/services/site-recovery/)

-   [Azure opslag, replicatie](storage-redundancy.md)

-   [Azure back-up service](https://azure.microsoft.com/services/backup/)

## <a name="how-to-detect"></a>Hoe te detecteren 

De aanbevolen manier om te bepalen van de status van de Azure is te abonneren op de [Azure Service Health Dashboard](https://azure.microsoft.com/status/).

## <a name="what-to-do-if-a-storage-outage-occurs"></a>Wat te doen als een opslag is uitgevallen

Als een of meer opslag tijdelijk niet beschikbaar is op een of meer regio's zijn, zijn er twee opties die u kunt overwegen. Indien u direct toegang tot uw gegevens, overweeg dan optie 2.

### <a name="option-1-wait-for-recovery"></a>Optie 1: Wachten op herstel

In dat geval is geen actie van uw kant vereist. Wij werken naar eer en geweten de beschikbaarheid van Azure Services herstellen. U kunt de status van de service op de [Azure Service Health Dashboard](https://azure.microsoft.com/status/)kunt controleren.

### <a name="option-2-copy-data-from-secondary"></a>Optie 2: Gegevens uit secundaire kopiëren

Als u [leestoegang geo-redundante opslag (RA-GRS)](storage-redundancy.md#read-access-geo-redundant-storage) (aanbevolen) voor uw opslag rekeningen hebt gekozen, zal u leestoegang hebben tot uw gegevens uit het gebied met secundaire. Kunt u hulpprogramma's zoals [AzCopy](storage-use-azcopy.md), [Azure PowerShell](storage-powershell-guide-full.md)en de [bibliotheek Azure gegevens verplaatsen](https://azure.microsoft.com/blog/introducing-azure-storage-data-movement-library-preview-2/) uit de regio secundaire kopiëren naar een andere account voor opslag in een unimpacted gebied en wijst u vervolgens uw toepassingen op die account opslag voor zowel lezen en schrijven van beschikbaarheid.

## <a name="what-to-expect-if-a-storage-failover-occurs"></a>Wat u kunt verwachten als een opslag failover plaatsvindt

Als u hebt gekozen voor [Geo-redundante opslag (GRS)](storage-redundancy.md#geo-redundant-storage) of [leestoegang geo-redundante opslag (RA-GRS)](storage-redundancy.md#read-access-geo-redundant-storage) (aanbevolen), houdt Azure opslag uw gegevens duurzaam in twee regio's (primaire en secundaire). In beide regio's houdt Azure opslag voortdurend meerdere replica's van uw gegevens.

Wanneer een regionale ramp is van invloed op uw primaire regio, wordt eerst geprobeerd om de service in die regio te herstellen. Afhankelijk van de aard van de ramp en de effecten daarvan, in bepaalde uitzonderlijke gevallen die we mogelijk niet de primaire regio te herstellen. We zullen op dat moment een geo-failover uitvoeren. De gegevensreplicatie tussen regio is een asynchroon proces waarbij kan een vertraging, dus het is mogelijk dat de wijzigingen die nog niet zijn gerepliceerd naar de secundaire regio mogelijk verloren. U kunt de ["Tijd van laatste synchronisatie ' van uw account opslag](https://blogs.msdn.microsoft.com/windowsazurestorage/2013/12/11/windows-azure-storage-redundancy-options-and-read-access-geo-redundant-storage/) als u informatie over de replicatiestatus zoeken.

Een aantal punten met betrekking tot de opslag geo-failover-ervaring:

-   Opslag geo-failover wordt alleen geactiveerd door het team Azure-opslag: Er is geen klant actie vereist.

-   Uw bestaande storage service-endpoints voor BLOB's, tabellen, wachtrijen en bestanden blijven nadat de failover is uitgevoerd. de DNS-vermelding moet worden bijgewerkt als u wilt overschakelen van de primaire regio naar het secundaire regio.

-   Vóór en tijdens de geo-failover geen schrijven-toegang tot uw account opslag aan de impact van de ramp, maar kunt u nog steeds lezen van het secundair als uw account opslag is geconfigureerd als RA GRS.

-   Wanneer de geo-failover is voltooid en de DNS-wijzigingen worden doorgevoerd, worden de lees- en schrijftoegang hebben tot uw account opslag hervat. U kunt [' laatste Geo failover-tijd ' van uw account opslag](https://msdn.microsoft.com/library/azure/ee460802.aspx) als u meer informatie opvragen.

-   Na de overname, uw account opslag volledig functionele, maar "slechtere" status, zoals deze is werkelijk ondergebracht in een zelfstandige regio met geen geo-replicatie mogelijk. Om dit risico te beperken, wordt de oorspronkelijke primaire regio te herstellen en voert een geo-failback-bewerking als u de oorspronkelijke staat herstellen. Als het oorspronkelijke primaire gebied onherstelbaar is, zullen we een andere secundaire regio toewijzen.
Raadpleeg het artikel op de opslag teamblog over de [Opties voor redundantie en RA GRS](https://blogs.msdn.microsoft.com/windowsazurestorage/2013/12/11/windows-azure-storage-redundancy-options-and-read-access-geo-redundant-storage/)voor meer informatie over de infrastructuur van de opslag van Azure geo-replicatie.

##<a name="best-practices-for-protecting-your-data"></a>Aanbevolen procedures voor het beveiligen van uw gegevens

Er zijn enkele aanbevolen methoden voor back-up van uw gegevens in de opslag op een regelmatige basis.

-   De [back-up Azure service](https://azure.microsoft.com/services/backup/) VM schijven – gebruiken back-up van de VM-schijven die worden gebruikt door uw Azure virtuele machines.

-   Blok BLOB's – een [momentopname](https://msdn.microsoft.com/library/azure/hh488361.aspx) van elk blok blob of kopie de BLOB's naar opslag op een andere account in een ander gebied met [AzCopy](storage-use-azcopy.md), [Azure PowerShell](storage-powershell-guide-full.md)of de [verplaatsing van gegevens Azure bibliotheek](https://azure.microsoft.com/blog/introducing-azure-storage-data-movement-library-preview-2/)maken.

-   Tabellen: Gebruik [AzCopy](storage-use-azcopy.md) om gegevens in de tabel exporteren naar een andere account voor opslag in een ander gebied.

-   Bestanden: Gebruik [AzCopy](storage-use-azcopy.md) of [Azure PowerShell](storage-powershell-guide-full.md) uw bestanden kopiëren naar een andere account voor opslag in een ander gebied.
