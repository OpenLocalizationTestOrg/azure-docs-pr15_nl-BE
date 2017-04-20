<properties
    pageTitle="Aanbevolen procedures voor SQL Server Performance | Microsoft Azure"
    description="Bevat de aanbevolen procedures voor het optimaliseren van de prestaties van SQL Server in Microsoft Azure VM's."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="rothja"
    manager="jhubbard"
    editor=""
    tags="azure-service-management" />

<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="09/07/2016"
    ms.author="jroth" />

# <a name="performance-best-practices-for-sql-server-in-azure-virtual-machines"></a>De beste prestaties voor SQL Server in Azure virtuele Machines

## <a name="overview"></a>Overzicht

In dit onderwerp vindt u aanbevolen procedures voor het optimaliseren van de prestaties van SQL Server in Microsoft Azure Virtual Machine. Tijdens het uitvoeren van SQL Server in Azure virtuele Machines, is het raadzaam dat u met behulp van dezelfde databaseprestaties afstemmen opties die van toepassing op SQL Server in de serveromgeving voor gebouwen zijn blijven. De prestaties van een relationele database in een openbare cloud is afhankelijk van vele factoren zoals de grootte van een virtuele machine en de configuratie van de gegevensschijven.

Bij het maken van afbeeldingen voor SQL Server, [kunt u uw VMs in Azure portal inrichten](virtual-machines-windows-portal-sql-server-provision.md). SQL Server VMs ingericht in de Portal met Resource Manager implementeert alle deze best practices, met inbegrip van de opslagconfiguratie.

Dit artikel is gericht op het verkrijgen van de *beste* prestaties voor SQL Server op Azure VMs. Als uw werkbelasting minder zwaar worden belast is, kunt u alle onderstaande optimalisatie mogelijk niet nodig. Rekening houden met de prestaties en de werkbelasting patronen zoals de evaluatie van deze aanbevelingen.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

## <a name="quick-check-list"></a>Lijst Snelle controle

Hier volgt een lijst met snelle controle voor optimale prestaties van SQL Server op Azure virtuele Machines:

|Gebied|Optimalisatie|
|---|---|
|[VM-grootte](#vm-size-guidance)|[DS3](virtual-machines-windows-sizes.md#standard-tier-ds-series) of hoger voor SQL Enterprise edition.<br/><br/>[DS2](virtual-machines-windows-sizes.md#standard-tier-ds-series) of hoger voor SQL Standard en Web-edities.|
|[Opslag](#storage-guidance)|[Premium opslag](../storage/storage-premium-storage.md)gebruiken. Standaardopslag, wordt alleen aanbevolen voor dev/test.<br/><br/>Houd de [rekening voor opslag](../storage/storage-create-storage-account.md) en SQL Server VM in hetzelfde gebied.<br/><br/>Azure [geo-redundante opslag](../storage/storage-redundancy.md) (geo-replicatie) op de rekening van de opslag uit te schakelen.|
|[Schijven](#disks-guidance)|Gebruik een minimum van 2 [schijven P30](../storage/storage-premium-storage.md#scalability-and-performance-targets-when-using-premium-storage) (1 voor logboekbestanden; 1 voor gegevens- en TempDB).<br/><br/>Vermijd het gebruik van besturingssysteem of tijdelijke schijven voor opslag in een databank of registratie.<br/><br/>Lezen inschakelen op de schijven waarop de gegevens- en TempDB-caching.<br/><br/>Schakel opslaan in cache op schijf of schijven die als host fungeert voor het logboekbestand.<br/><br/>Belangrijk: De SQL Server-service stoppen bij het wijzigen van de cache-instellingen voor een schijf Azure VM.<br/><br/>Als u grotere i/o-gegevensdoorvoer meerdere Azure gegevensschijven stripe.<br/><br/>Indeling met grootte van de toewijzing beschreven.|
|[I/O](#io-guidance)|Database pagina compressie inschakelen.<br/><br/>Instant-bestand de initialisatie van data-bestanden inschakelen.<br/><br/>Beperken of uitschakelen van de optie autogrow op de database.<br/><br/>Autoshrink op de database uit te schakelen.<br/><br/>Alle databases verplaatst naar de gegevensschijven, met inbegrip van de systeemdatabases.<br/><br/>SQL Server fout log en trace mappen verplaatsen naar gegevensschijven.<br/><br/>Setup standaardbestandslocaties voor back-up en de database.<br/><br/>Vergrendelde pagina's inschakelen.<br/><br/>SQL Server performance correcties toepassen.|
|[De specifieke functie](#feature-specific-guidance)|Back-ups rechtstreeks op de blob-opslag.|

Bekijk de details en de richtsnoeren die in de volgende secties voor meer informatie over het *hoe* en *Waarom* deze optimalisaties maken.

## <a name="vm-size-guidance"></a>Richtlijnen voor VM-grootte

Prestaties-gevoelige toepassingen wordt aangeraden de volgende [formaten voor virtuele machines](virtual-machines-windows-sizes.md)te gebruiken:

- **SQL Server Enterprise Edition**: DS3 of hoger

- **SQL Server Standard en Web-edities**: DS2 of hoger


## <a name="storage-guidance"></a>Richtlijnen voor opslag

DS-serie (in combinatie met DSv2-serie en GS serie) VMs ondersteuning [Premium opslag](../storage/storage-premium-storage.md). Premium-opslagruimte wordt aanbevolen voor alle productie werkbelasting.

> [AZURE.WARNING] Standaardopslag heeft verschillende vertragingstijden en bandbreedte en wordt alleen aanbevolen voor een standaardwerkbelasting dev/test. Werkbelasting van de productie moeten Premium opslag gebruiken.

Bovendien is het raadzaam uw account Azure opslag in het datacenter hetzelfde als uw SQL Server virtuele machines te bekorten overdracht te maken. Bij het maken van een account voor de opslag, geo-replicatie uitschakelen tijdens het schrijven van consistente volgorde over meerdere schijven is niet gegarandeerd. In plaats daarvan kunt u een SQL Server-technologie disaster recovery tussen twee Azure datacenters te configureren. Zie voor meer informatie, [hoge beschikbaarheid en noodherstel voor SQL Server in Azure virtuele Machines](virtual-machines-windows-sql-high-availability-dr.md).

## <a name="disks-guidance"></a>Richtlijnen voor schijven

Er zijn drie belangrijke schijf op een Azure VM:

- **OS schijf**: bij het maken van een virtuele Machine van Azure het platform ten minste één schijf (aangeduid als station **C** ) voor de VM voor uw besturingssysteem schijf worden gekoppeld. Deze schijf is een VHD opgeslagen als een blob pagina in de opslag.
- **Tijdelijke schijf**: Azure virtuele Machines bevatten de tijdelijke schijf een andere schijf (aangeduid als de **D**: station). Dit is een schijf op het knooppunt dat kan worden gebruikt voor tijdelijke ruimte.
- **Gegevensschijven**: U kunt ook extra schijven koppelen aan uw virtuele machine als gegevensschijven en deze in de opslag worden opgeslagen als BLOB's pagina.

De volgende secties worden de aanbevelingen voor het gebruik van deze verschillende schijven.

### <a name="operating-system-disk"></a>Besturingssysteem schijf

Een schijf is een VHD die u kunt starten en koppelen als een versie van een besturingssysteem en wordt aangeduid als station **C** .

Standaardcache-beleid op de schijf is **Lezen/schrijven**. Prestaties gevoelige toepassingen, wordt u aangeraden u gegevensschijven gebruiken in plaats van de schijf. Zie de sectie op gegevensschijven hieronder.

### <a name="temporary-disk"></a>Tijdelijke schijf

Het station voor tijdelijke opslag, aangeduid als de **D**: drive, niet persistent gemaakt naar Azure blob-opslag. Uw gebruiker-databasebestanden of gebruiker transactie-logbestanden op de **D**niet opslaan: station.

Voor D-reeks Dv2-serie en de G-serie VMs is het tijdelijke station op deze VMs op basis van SSD-technologie. Als uw werkbelasting intensief gebruik van TempDB maakt (bv. voor tijdelijke objecten of complexe joins) kan TempDB op station **D** opslaan resulteren in hogere TempDB-doorvoer en latentie van lagere TempDB.

VMs die ondersteuning bieden voor Premium opslag (DS serie DSv2-serie en GS-reeks), wordt u aangeraden TempDB en/of Buffer Pool-extensies op te slaan op een schijf die premie opslag ondersteunt met lezen caching is ingeschakeld. Er is één uitzondering op deze aanbeveling; Als uw gebruik van TempDB schrijven-intensieve, kunt u betere prestaties behalen doordat TempDB op het lokale station **D** , die ook SSD-technologie op basis van de omvang van deze machine.

### <a name="data-disks"></a>Gegevensschijven

- **Gegevensschijven voor gegevens-en logboekbestanden gebruiken**: gebruik ten minste 2 Premium [P30 schijven](../storage/storage-premium-storage.md#scalability-and-performance-targets-when-using-premium-storage) waar één schijf bevat de logboekbestanden en de andere gegevens, bestanden en TempDB.

- **Schijf-Striping**: voor meer doorvoer kunt u gegevensschijven meer toevoegen en gebruiken van de schijf-Striping. Als u wilt bepalen hoeveel gegevensschijven, moet u het aantal IOP's beschikbaar voor schijven gegevens- en logboekbestanden analyseren. Die informatie, Zie de tabellen voor op IOP's per [VM-grootte](virtual-machines-windows-sizes.md) en de schijf grootte in het volgende artikel: [Premium opslag gebruiken voor schijven](../storage/storage-premium-storage.md). Gebruik de volgende deze richtlijnen:

    - Voor Windows 8 / Windows Server 2012 of later, [Opslag spaties](https://technet.microsoft.com/library/hh831739.aspx)te gebruiken. Stripe grootte instelt op 64 KB voor OLTP workloads en 256 KB voor magazijnbeheer werkbelasting gegevens om te voorkomen dat invloed op de prestaties als gevolg van onjuiste uitlijning partitie. Stel bovendien een aantal kolommen = het aantal fysieke schijven. Een ruimte met meer dan 8 schijven configureren gebruikt u PowerShell (geen Server Manager UI) expliciet het aantal kolommen overeenkomt met het aantal schijven instellen. Zie voor meer informatie over het configureren van de [Ruimten voor opslag](https://technet.microsoft.com/library/hh831739.aspx), [Opslag spaties Cmdlets in Windows PowerShell](https://technet.microsoft.com/library/jj851254.aspx)

    - Voor Windows 2008 R2 of lager gebruikt, kunt u dynamische schijven (OS striped volumes) en de stripe-grootte is altijd 64 KB. Houd er rekening mee dat deze optie vanaf Windows 8 Windows Server 2012 is verouderd. Voor meer informatie, Zie de support-instructie op de [Virtual Disk-Service is overstappen naar Windows Storage Management API](https://msdn.microsoft.com/library/windows/desktop/hh848071.aspx).

    - Als uw werkbelasting is geen logboek intensieve speciale IOP's niet nodig heeft, kunt u slechts één groep. Anders maakt u twee opslagpools, één voor de logboekbestanden en een andere groep voor de gegevens, bestanden en TempDB. Zoek het nummer van de schijven die zijn gekoppeld aan elke opslag van toepassingen op basis van uw verwachtingen laden. Houd er rekening mee dat verschillende grootten voor VM toestaan dat verschillende nummers van de gegevensschijven van de gekoppelde. Zie voor meer informatie [voor virtuele Machines](virtual-machines-windows-sizes.md).

    - Als u geen premie opslag (dev/Testscenario's), is de aanbeveling het maximum aantal gegevensschijven worden ondersteund door uw [VM-grootte](virtual-machines-windows-sizes.md) toevoegen en schijf-Striping.

- **Cachebeleid**: voor Premium data schijven, lezen caching op de gegevensschijven die alleen gegevensbestanden en TempDB hosting inschakelen. Als u geen premie opslag, schakel eventuele cachegegevens op gegevensschijven. Zie voor instructies over het configureren van schijfcaching, de volgende onderwerpen: [Set AzureOSDisk](https://msdn.microsoft.com/library/azure/jj152847) en een [Set AzureDataDisk](https://msdn.microsoft.com/library/azure/jj152851.aspx).

    >[AZURE.WARNING] Stop de service SQL Server bij het wijzigen van de cache-instelling van Azure VM schijven om te voorkomen dat de kans op beschadiging van de database.

- **Grootte van de toewijzingseenheid NTFS**: wanneer u de gegevensschijf opmaakt, wordt aanbevolen dat een clustergrootte van 64 KB te voor gegevens-en logboekbestanden, alsmede TempDB gebruiken.

- **Best practices voor schijf beheer**: wanneer een gegevensschijf verwijderen of wijzigen van het cachetype, de SQL Server-service stopt tijdens het wijzigen van de. Wanneer de cache-instellingen worden gewijzigd op de schijf OS, Azure VM stopt wijzigt het cachetype en de VM opnieuw wordt opgestart. Wanneer de cache-instellingen van een schijf met gegevens worden gewijzigd, wordt de VM is niet gestopt, maar de gegevensschijf is losgekoppeld van de VM tijdens het wijzigen en klik vervolgens opnieuw.

    >[AZURE.WARNING] De SQL Server-service stoppen tijdens deze bewerkingen niet kan leiden tot beschadiging van de database.

## <a name="io-guidance"></a>I/o-instructies

- De beste resultaten met de premie opslag worden bereikt wanneer u uw toepassing en aanvragen parallelize. Premium-opslag is ontworpen voor scenario's waarbij de i/o-wachtrijdiepte groter dan 1, is dus worden er weinig of geen hogere prestaties voor seriële single thread-aanvragen (zelfs als ze intensieve opslag). Bijvoorbeeld, dit kan invloed hebben op de single thread-testresultaten van prestatie-analyse-hulpmiddelen, zoals SQLIO.

- Overweeg het gebruik van [compressie van database-pagina](https://msdn.microsoft.com/library/cc280449.aspx) zoals deze prestaties kunt verbeteren van i/o-intensieve belasting. De compressie kan echter het CPU-verbruik op de databaseserver te verhogen.

- Kunt u expresberichten bestand initialisatie reduceert de tijd die nodig is voor de toewijzing van het eerste bestand inschakelen. Als u wilt profiteren van instant bestand initialisatie, verlenen de serviceaccount van SQL Server (MSSQLSERVER) met SE_MANAGE_VOLUME_NAME en toe te voegen aan het beveiligingsbeleid **Onderhoudstaken voor volumes uitvoeren** . Als u een SQL Server-platform images voor Azure, is de standaardaccount (NT Service\MSSQLSERVER) is niet toegevoegd aan het beveiligingsbeleid **Onderhoudstaken voor volumes uitvoeren** . Met andere woorden, is instant bestand initialisatie niet ingeschakeld in een SQL Server-Azure platform images. Na de SQL Server-serviceaccount toevoegen aan het beveiligingsbeleid **Onderhoudstaken voor volumes uitvoeren** , de SQL Server-service opnieuw te starten. Er zijn beveiligingsoverwegingen voor het gebruik van deze functie. Zie [Bestand initialiseren van de Database](https://msdn.microsoft.com/library/ms175935.aspx)voor meer informatie.

- **autogrow** wordt beschouwd als louter een gebeurtenis voor onverwachte groei. De groei van uw gegevens- en logboekbestanden op een dagelijkse basis met autogrow niet beheren. Als autogrow wordt gebruikt, moet u vooraf het bestand met de schakeloptie grootte groeien.

- Zorg ervoor dat **autoshrink** is uitgeschakeld om te voorkomen dat onnodige overhead die kan de prestaties negatief beïnvloeden.

- Alle databases verplaatst naar de gegevensschijven, met inbegrip van de systeemdatabases. Zie [System Databases verplaatsen](https://msdn.microsoft.com/library/ms345408.aspx)voor meer informatie.

- SQL Server fout log en trace mappen verplaatsen naar gegevensschijven. Dit is mogelijk in SQL Server Configuration Manager met de rechtermuisknop op de SQL Server-instantie en selecteer Eigenschappen. Fout log en trace instellingen van het bestand kunnen worden gewijzigd op het tabblad **Opstartparameters** . De Dump Directory is opgegeven op het tabblad **Geavanceerd** . Het volgende screenshot toont waar de parameter error log opstarten.

    ![Screenshot van SQL foutenlogboek](./media/virtual-machines-windows-sql-performance/sql_server_error_log_location.png)

- Setup standaardbestandslocaties voor back-up en de database. In dit onderwerp leest u de aanbevelingen en de wijzigingen aanbrengen in het eigenschappenvenster van de Server. Zie voor meer informatie [weergeven of wijzigen van de standaard locaties voor gegevens- en logboekbestanden (SQL Server Management Studio)](https://msdn.microsoft.com/library/dd206993.aspx). De volgende schermafbeelding ziet u waar u deze wijzigingen aanbrengt.

    ![Logboekbestanden van SQL-gegevens en back-up](./media/virtual-machines-windows-sql-performance/sql_server_default_data_log_backup_locations.png)

- Inschakelen van vergrendelde pagina's, i/o en paginering activiteiten te verminderen. Zie het onderwerp over het [inschakelen van de Lock-pagina's in geheugen, optie (Windows)](https://msdn.microsoft.com/library/ms190730.aspx)voor meer informatie.

- Als u SQL Server 2012, installeert u Service Pack 1 cumulatieve Update 10. Deze update bevat de correctie voor slechte prestaties in I/O tijdens het uitvoeren van select into-instructie in SQL Server 2012 tijdelijke tabel. Raadpleeg dit [knowledge base-artikel](http://support.microsoft.com/kb/2958012)voor informatie.

- U kunt gegevensbestanden comprimeren bij de overdracht in of uit van Azure.

## <a name="feature-specific-guidance"></a>Specifieke richtsnoeren voor functie

Sommige implementaties kunnen extra voordelen met meer geavanceerde configuratie-technieken bereiken. In de volgende lijst worden sommige onderdelen van SQL Server waarmee u betere prestaties behalen gemarkeerd:

- **Back-up naar Azure opslag**: bij het uitvoeren van back-ups van SQL Server wordt uitgevoerd in Azure virtuele machines, kunt u [SQL Server back-up naar een URL](https://msdn.microsoft.com/library/dn435916.aspx). Deze functie is beschikbaar vanaf SQL Server 2012 SP1 CU2 en aanbevolen voor reservekopieën maken op de gegevensschijven van de gekoppelde. Wanneer u back-up en terugzetten van Azure opslag, gaat u als volgt de aanbevelingen die op [SQL Server back-URL Best Practices en problemen oplossen en terugzetten van back-ups opgeslagen in Azure opslag](https://msdn.microsoft.com/library/jj919149.aspx). Deze back-ups [Automatisch back-up van SQL Server in Azure virtuele Machines](virtual-machines-windows-classic-sql-automated-backup.md)kunt u ook automatiseren.

    Voor SQL Server 2012 kunt u [SQL Server back-up hulpprogramma Azure](https://www.microsoft.com/download/details.aspx?id=40740). Met dit hulpprogramma kunt u back-up met behulp van meerdere back-stripe doelen de doorvoer te verhogen.

- **SQL Server-gegevensbestanden in Azure**: deze nieuwe functie, de [SQL Server-gegevensbestanden in Azure](https://msdn.microsoft.com/library/dn385720.aspx), is beschikbaar vanaf SQL Server 2014. Met SQL Server en gegevensbestanden in Azure ziet vergelijkbare prestatie-eigenschappen als schijven Azure gegevens.

## <a name="next-steps"></a>Volgende stappen

Als u geïnteresseerd bent in SQL Server- en Premium verkennen in meer diepte, Zie het artikel [Gebruik Azure Premium opslag met SQL Server op de virtuele Machines](virtual-machines-windows-classic-sql-server-premium-storage.md).

Zie voor aanbevolen procedures voor beveiliging, [Beveiligingsoverwegingen voor SQL Server in Azure virtuele Machines](virtual-machines-windows-sql-security.md).

Zie de andere onderwerpen SQL Server virtuele Machine op [SQL Server op overzicht van Azure Virtual Machines](virtual-machines-windows-sql-server-iaas-overview.md).
