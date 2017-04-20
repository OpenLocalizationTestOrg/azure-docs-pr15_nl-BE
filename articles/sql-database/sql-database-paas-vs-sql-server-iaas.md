<properties
    pageTitle="(PaaS) SQL-Database versus SQL Server in de cloud op VMs (IaaS) | Microsoft Azure"
    description="Informatie over welke optie u SQL Server cloud past bij uw toepassing: Azure SQL (PaaS)-Database of SQL Server in de cloud Azure virtuele Machines."
    services="sql-database, virtual-machines"
    keywords="Wolk-wolk van SQL Server, SQL Server in de cloud, PaaS-database SQL Server, DBaaS"
    documentationCenter=""
    authors="CarlRabeler"
    manager="jhubbard"
    editor="cjgronlund"/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/06/2016"
    ms.author="carlrab"/>

# <a name="choose-a-cloud-sql-server-option-azure-sql-paas-database-or-sql-server-on-azure-vms-iaas"></a>Kies een wolk van SQL Server option: Azure SQL (PaaS)-Database of SQL Server op Azure VMs (IaaS)

Azure heeft twee opties voor het hosten van de werkbelasting in Azure Microsoft SQL Server:

* [Azure SQL-Database](https://azure.microsoft.com/services/sql-database/): een SQL-database in de cloud, ook bekend als een platform als een (PaaS)-servicedatabase of een database als een service die is geoptimaliseerd voor de ontwikkeling van software as a service (SaaS) app (DBaaS). Dit biedt compatibiliteit met de meeste functies van SQL Server. Zie voor meer informatie over PaaS, [Wat is PaaS](https://azure.microsoft.com/overview/what-is-paas/).
* [SQL Server Azure virtuele Machines](https://azure.microsoft.com/services/virtual-machines/sql-server/): SQL Server geïnstalleerd en worden gehost in de wolk op Windows Server virtuele Machines (VMs) waarop Azure, ook bekend als een infrastructuur als service (IaaS).
SQL Server op Azure virtuele machines is geoptimaliseerd voor het migreren van bestaande SQL Server-toepassingen. Alle versies en edities van SQL Server zijn beschikbaar. 100% compatibel met SQL Server, zodat u kunt zoveel databases instellen als nodig en uitvoerende cross database transacties host biedt. Het biedt volledige controle over SQL Server en Windows.

Informatie hoe elke optie past in de Microsoft data platform en help die overeenkomt met de juiste optie op uw zakelijke vereisten. Of u kosten besparen of een minimum aan beheer voor al het andere prioriteiten, kunt in dit artikel u bepalen welke aanpak biedt ten opzichte van de zakelijke vereisten die u het meest bent geïnteresseerd.


## <a name="microsofts-data-platform"></a>Microsoft data platform

Een van de eerste dingen om te begrijpen in een discussie van Azure versus de SQL Server-databases voor ruimten is dat u alles kunt gebruiken. Gegevens-platform van Microsoft maakt gebruik van SQL Server-technologie en beschikbaar via fysieke op gebouwen, machines, persoonlijke cloud-omgevingen, derden gehoste private cloud-omgevingen en openbare cloud. SQL Server op Azure virtual mchines kunt u unieke en diverse zakelijke behoeften door een combinatie van lokale en cloud gehost implementaties tijdens het gebruik van dezelfde set serverproducten, ontwikkelprogramma's en expertise in deze omgevingen.

   ![SQL Server-opties van de wolk: SQL server op IaaS of SaaS SQL-database in de cloud.](./media/sql-database-paas-vs-sql-server-iaas/SQLIAAS_SQL_Server_Cloud_Continuum.png)

Zoals u in het diagram, kan elk aanbod worden gekenmerkt door het niveau van beheer u via de infrastructuur (op de X-as hebt) en de mate van kosten-efficiëntie bereikt door de database op consolidatie en automatisering (op de Y-as).

Wanneer u een toepassing ontwerpt, zijn vier elementaire opties gebruikt als host voor het onderdeel SQL Server van de toepassing:

- SQL Server op een niet-gevirtualiseerde fysieke machines
- SQL Server in op ruimten gevirtualiseerde machines (private cloud)
- SQL Server in Microsoft Azure Virtual Machine (Microsoft public cloud)
- Azure SQL-Database (Microsoft public cloud)

In de volgende secties vindt u meer informatie over SQL Server in de openbare Microsoft-cloud: Azure SQL-Database en SQL Server op Azure VMs. Daarnaast verkennen u algemene zakelijke motivators om te bepalen welke optie is geschikt voor uw toepassing.

## <a name="a-closer-look-at-azure-sql-database-and-sql-server-on-azure-vms"></a>Nader Azure SQL-Database en SQL Server op Azure VMs

**Azure SQL-Database** is een relationele database-as-a-service (DBaaS) die zijn ondergebracht in de Azure cloud die in de industrie categorieën van *Software-as-a-Service (SaaS)* en *Platform-as-a-Service (PaaS)*valt. [SQL-database](sql-database-technical-overview.md) is gebaseerd op gestandaardiseerde hardware en software die eigendom is, gehost en onderhouden door Microsoft. Met de SQL-Database, kunt u rechtstreeks op de service met behulp van ingebouwde functies en functionaliteit ontwikkelen. Wanneer u met opties voor het schalen van of uit voor meer kracht met geen onderbreking pay-as-you-go SQL-Database.

**SQL Server op Azure virtuele Machines (VMs)** valt in de categorie industrie *Infrastructure-as-a-Service (IaaS)* en kunt u SQL Server wordt uitgevoerd binnen een virtuele machine in de cloud. Net als bij een SQL-Database, het is gebaseerd op gestandaardiseerde hardware die eigendom is, gehost en onderhouden door Microsoft. Wanneer u SQL Server op een VM, kunt u ofwel betalen-als u overal voor een SQL Server-licentie al opgenomen in een SQL Server-versie of eenvoudig om een bestaande licentie te gebruiken. U kunt ook gemakkelijk schaal-omhoog/omlaag en de VM desgewenst onderbreken/hervatten.

Deze twee SQL-opties zijn in het algemeen is geoptimaliseerd voor verschillende doeleinden:

- **SQL-Database** is geoptimaliseerd voor de totale kosten te verlagen tot het minimum dat voor het inrichten en beheren van veel databases. Continue beheerkosten vermindert omdat u niet hebt voor het beheren van alle virtuele machines, besturingssysteem of database-software. U hoeft geen upgrades, hoge beschikbaarheid of [back-ups](sql-database-automated-backups.md)beheren. Azure SQL-Database kan in het algemeen aanzienlijk vergroten het aantal databases die worden beheerd door een enkele IT of informatiebron.
- **SQL Server wordt uitgevoerd op Azure VMs** is geoptimaliseerd voor bestaande toepassingen naar Azure te migreren of uitbreiding van bestaande op ruimten toepassingen naar de cloud in hybride installaties. Bovendien kunt u SQL Server op een virtuele machine te ontwikkelen en testen van toepassingen met traditionele SQL Server. Met SQL Server op Azure VMs hebt u de volledige administratieve rechten via een apart exemplaar van SQL Server en een cloud-gebaseerde VM. Wanneer een organisatie al gebruikmaakt van IT-bronnen beschikbaar zijn voor het onderhouden van de virtuele machines is een perfecte keuze. Deze mogelijkheden kunnen u een hoge mate aangepaste systeem om specifieke prestaties van uw toepassing en de vereiste beschikbaarheid op te lossen.

De volgende tabel bevat een overzicht van de belangrijkste kenmerken van de SQL-Database en SQL Server op Azure VMs:

|       | SQL-Database | SQL Server in een Azure Virtual Machine|
| -------------- | ------------ | ---------------------- |
| **Geschikt voor:** | Nieuwe cloud ontworpen toepassingen met beperkingen van de ontwikkeling en marketing. |Bestaande toepassingen die snelle migratie naar de cloud met minimale wijzigingen vereisen. Snelle ontwikkel- en scenario's als u niet wilt kopen op lokalen niet voor productie SQL Server-hardware. |
|| Teams die ingebouwde hoge beschikbaarheid, noodherstel en upgrade voor de database nodig hebt. |De teams die u kunnen configureren en beheren van hoge beschikbaarheid, noodherstel en patches voor SQL Server. Sommige automatische functies aanzienlijk vereenvoudigen opgenomen. |
||Teams die u niet wilt dat het onderliggende besturingssysteem en configuratie-instellingen te beheren.| Als u een aangepaste omgeving met volledige beheerdersrechten nodig.|
||Databases van maximaal 1 TB of grotere databases die [horizontaal of verticaal worden gepartitioneerd](sql-database-elastic-scale-introduction.md#horizontal-and-vertical-scaling) met behulp van een scale-out patroon kunnen worden.|SQL Server-exemplaren met maximaal 64 TB aan opslagruimte. De instantie ondersteunt databases die nodig is. |
||[Gebouw Software-as-a-Service (SaaS) toepassingen](sql-database-design-patterns-multi-tenancy-saas-applications.md).| Migratie en enterprise en hybride toepassingen te bouwen.|
|||||
|**Bronnen:**|U niet wilt gebruiken voor de configuratie en het beheer van de onderliggende infrastructuur van IT-middelen, maar u wilt concentreren op de toepassingslaag.|Hebt u een IT-bronnen voor configuratie en beheer. Sommige automatische functies aanzienlijk vereenvoudigen opgenomen.|
|**Totale eigendomskosten:**|Elimineert de kosten voor hardware en administratieve kosten vermindert.|Elimineert de kosten voor hardware.|
|**Bedrijfscontinuïteit:**|Azure SQL-Database biedt naast de mogelijkheden voor infrastructuur ingebouwde fouttolerantie, functies, zoals [geautomatiseerde back-ups](sql-database-automated-backups.md) [Terugzetten Point-In-Time](sql-database-recovery-using-backups.md#point-in-time-restore), [Geo terugzetten](sql-database-recovery-using-backups.md#geo-restore)en [Actieve Geo-replicatie](sql-database-geo-replication-overview.md) bedrijfscontinuïteit te verhogen. Zie [SQL-Database business continuity-overzicht](sql-database-business-continuity.md)voor meer informatie.|SQL Server op Azure VMs kunt u instellen met een hoge beschikbaarheid en noodherstel hersteloplossing voor de specifieke behoeften van uw database. Daarom kunt u een systeem dat is geoptimaliseerd voor uw toepassing hebben. U kunt testen en failover uitvoeren door zelf als dat nodig is. Zie voor meer informatie, [hoge beschikbaarheid en noodherstel voor SQL Server Azure virtuele Machines](../virtual-machines/virtual-machines-windows-sql-high-availability-dr.md).|
|**Hybride cloud:**|De toepassing op locatie toegang tot de gegevens in Azure SQL-Database.|Bij SQL Server op Azure VMs, kunt u toepassingen die gedeeltelijk in de cloud en deels op-ruimten hebben. U kunt bijvoorbeeld uw netwerk op gebouwen en Active Directory-domein naar de cloud via [Azure virtueel netwerk](../virtual-network/virtual-networks-overview.md)uitbreiden. Bovendien kunt u gegevensbestanden op gebouwen in Azure opslag met behulp van [SQL Server-gegevensbestanden in Azure](http://msdn.microsoft.com/library/dn385720.aspx)opslaan. Zie [Inleiding tot SQL Server 2014 hybride Cloud](http://msdn.microsoft.com/library/dn606154.aspx)voor meer informatie.|
||Biedt ondersteuning voor [transactionele replicatie SQL Server](https://msdn.microsoft.com/library/mt589530.aspx) als abonnee gegevens worden gerepliceerd.|Biedt volledige ondersteuning voor [transactionele replicatie van SQL Server](https://msdn.microsoft.com/library/mt589530.aspx), [AlwaysOn beschikbaarheidsgroepen](../virtual-machines/virtual-machines-windows-sql-high-availability-dr.md)Integration Services en verzending logboek om gegevens te repliceren. Ook worden traditionele back-ups van SQL Server volledig ondersteund|
|||||
|||||

## <a name="business-motivations-for-choosing-azure-sql-database-or-sql-server-on-azure-vms"></a>Zakelijke motieven voor de keuze van Azure SQL-Database of SQL Server op Azure VMs

### <a name="cost"></a>Kosten

Of u nu een startup die is strapped voor contant geld of een team in een gevestigde bedrijf dat onder een krap budget beperkingen opereert, is beperkte financiële middelen vaak de belangrijkste factor bij het bepalen van het hosten van uw databases. In dit gedeelte vindt u informatie over de facturering en Licentieverlening algemeen in Azure met betrekking tot deze twee opties voor de relationele database: SQL-Database en SQL Server op Azure VMs. U leert ook over het berekenen van de toepassing van totale kosten.

#### <a name="billing-and-licensing-basics"></a>Facturering en Licentieverlening algemeen

**SQL-Database** wordt verkocht aan klanten als een service, niet met een licentie.  [SQL Server op Azure VMs](../virtual-machines/virtual-machines-windows-sql-server-iaas-overview.md) wordt verkocht met een opgenomen licentie die u betaalt per minuut. Als u een bestaande licentie hebt, kunt u deze ook gebruiken.  

**SQL-Database** is momenteel beschikbaar in verschillende Servicelagen, die per uur worden gefactureerd tegen een vaste rente op basis van de opgegeven service en prestatieniveau dat u kiest. Bovendien worden u in rekening gebracht voor uitgaande Internet-verkeer op de reguliere [tarieven van de gegevensoverdracht](https://azure.microsoft.com/pricing/details/data-transfers/). De Basic, Standard en Premium service niveaus zijn ontworpen om voorspelbare prestaties met meerdere prestatieniveaus aan uw toepassingsvereisten piek. U kunt schakelen tussen de Servicelagen en de prestaties aan de doorvoer van uiteenlopende behoeften van uw toepassing. Als uw database heeft een hoge transactionele volume en moet worden ondersteund door veel gebruikers tegelijkertijd, raden we aan de laag Premium service. Zie voor actuele informatie over de huidige ondersteunde service tiers, [Azure SQL Database Service Tiers](sql-database-service-tiers.md). Ook kunt u [groepen van elastische database](sql-database-elastic-pool.md) database-exemplaren prestaties bronnen delen.

Met **SQL-Database**, wordt database-software automatisch geconfigureerd, hersteld en bijgewerkt door Microsoft, waardoor de beheerkosten. Ook kunnen de [ingebouwde back-up](sql-database-automated-backups.md) mogelijkheden behalen aanzienlijke kostenbesparingen, vooral wanneer er een groot aantal databases.

U kunt met **SQL Server op Azure VMs**, gebruikt u een van de geleverde platform SQL Server-afbeeldingen (die een licentie bevat) of brengen van uw SQL Server-licentie. Alle ondersteunde versies van SQL Server (2008R2, 2012, 2014, 2016) en edities (ontwikkelaar, Express, Web, Standard, Enterprise) beschikbaar zijn. Bovendien zijn plaatsen-uw-eigen-licentie (BYOL) van de afbeeldingen verkrijgbaar. Wanneer u afbeeldingen met behulp van de Azure worden geleverd, is de operationele kosten afhankelijk van de VM-grootte en de editie van SQL Server die u kiest. VM-grootte of editie van SQL Server betaalt u de licentie kosten per minuut van SQL Server en Windows Server, samen met de kosten van Azure opslag voor de VM-schijven. De betaalwijze per minuut kunt u SQL Server gebruiken om, zolang u licenties nodig hebt zonder kopen toevoeging van SQL Server. Als u uw eigen SQL Server-licentie voor Azure, worden in rekening gebracht voor Windows Server en alleen de kosten voor de opslag. Zie voor meer informatie over licentieverlening brengen zelf [Licentie mobiliteit via Software Assurance op Azure](https://azure.microsoft.com/pricing/license-mobility/).

#### <a name="calculating-the-total-application-cost"></a>Berekening van de toepassing van totale kosten

Wanneer u met een wolk platform start, omvat de kosten van het uitvoeren van uw toepassing de ontwikkeling en de kosten, plus de servicekosten openbare cloud-platform.

Hier is de gedetailleerde kostprijsberekening voor uw toepassing wordt uitgevoerd in de SQL-Database en SQL Server Azure VMs:

**Bij gebruik van Azure SQL-Database:**

*Totale kosten van toepassing = sterk geminimaliseerd administratiekosten + software development kosten + servicekosten voor SQL-Database*

**Wanneer u SQL Server op Azure VMs:**

*Totale kosten van toepassing = sterk geminimaliseerd software ontwikkeling kosten + administratiekosten + licentiekosten voor SQL Server en Windows Server + Azure opslagkosten*

Voor meer informatie over prijzen, Zie de volgende bronnen:

- [Prijzen van de SQL-Database](https://azure.microsoft.com/pricing/details/sql-database/)
- [Prijzen van de virtuele Machine](https://azure.microsoft.com/pricing/details/virtual-machines/) voor [SQL](https://azure.microsoft.com/pricing/details/virtual-machines/#sql) en [Windows](https://azure.microsoft.com/pricing/details/virtual-machines/#windows)
- [Azure prijzen Calculator](https://azure.microsoft.com/pricing/calculator/)

> [AZURE.NOTE] Er is een kleine deelverzameling van functies in SQL Server die niet van toepassing of niet beschikbaar met de SQL-Database. Zie [SQL Database algemene beperkingen en richtsnoeren](sql-database-general-limitations.md) en [SQL Database Transact-SQL-gegevens](sql-database-transact-sql-information.md) voor meer informatie. Als u een bestaande SQL Server-oplossing naar de cloud verplaatst, Zie [migreren naar Azure SQL-Database SQL Server-database](sql-database-cloud-migrate.md). Wanneer u een bestaande toepassing van de SQL Server in ruimten met SQL-Database hebt gemigreerd, kunt u het bijwerken van de toepassing om te profiteren van de mogelijkheden die cloud services bieden. Bijvoorbeeld, kunt u overwegen [Azure App webservice](https://azure.microsoft.com/services/app-service/web/) of [Azure Cloud Services](https://azure.microsoft.com/services/cloud-services/) gebruikt voor het hosten van uw toepassingslaag om voordeel te verhogen.

### <a name="administration"></a>Beheer

Voor veel bedrijven is het besluit om een overgang naar een cloud-service, evenals veel over offloading complexiteit van het beheer van deze kosten. Microsoft beheert met een **SQL-Database**, de onderliggende hardware. Microsoft automatisch alle gegevens voor maximale beschikbaarheid worden gerepliceerd, wordt geconfigureerd en database-software-upgrades beheert taakverdeling en overname wordt als er een storing op de server. U kunt doorgaan met het beheren van uw database, maar u niet meer nodig hebt voor het beheren van de database-engine, besturingssysteem of hardware.  Voorbeelden van items, die kunt u doorgaan met het beheer van zijn databases en aanmeldingen, index en afstemmen van de query en controle en beveiliging.

Met **SQL Server op Azure VMs**hebt u volledige controle over het besturingssysteem en de configuratie van SQL Server-exemplaar. Met een VM is het aan u om te beslissen wanneer update/upgrade van het besturingssysteem en de databasesoftware en wanneer de installatie van extra software, zoals antivirusprogramma's. Sommige automatische functies zijn bedoeld om de patch, back-up en hoge beschikbaarheid aanzienlijk vereenvoudigen. U kunt bovendien bepalen de grootte van de VM, het nummer van de schijven en hun opslagconfiguraties. Azure kunt u de grootte van een VM desgewenst wijzigen. Zie voor meer informatie, de [virtuele Machine en Cloud Service formaten voor Azure](../virtual-machines/virtual-machines-linux-sizes.md). 

### <a name="service-level-agreement-sla"></a>Service Level Agreement (SLA)

Voor veel IT-afdelingen is verplichtingen van de gebruikstijd van een Service niveau overeenkomst (SLA) een topprioriteit. In dit gedeelte bekijken we wat SLA is van toepassing op elke host de optie database.

Voor **SQL-Database** Basic, Standard en Premium service tiers biedt Microsoft een beschikbaarheid van SLA van 99,99%. Zie voor de meest recente informatie, [Service Level Agreement](https://azure.microsoft.com/support/legal/sla/sql-database/). Zie voor actuele informatie over de SQL-Database service niveaus en de ondersteunde business continuity plannen, [Service Tiers](sql-database-service-tiers.md).

Microsoft biedt **SQL Server wordt uitgevoerd op Azure VMs**, een beschikbaarheid van SLA van 99.95% die alleen de virtuele Machine dekt. Deze SERVICEOVEREENKOMST omvat niet de processen (zoals SQL Server) die op de VM en vereist dat u ten minste twee VM-exemplaren in een set beschikbaarheid host. Zie de [VM SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/)voor de meest recente informatie. Voor hoge beschikbaarheid van de databases (HA) binnen het VMs, moet u een van de ondersteunde hoge beschikbaarheid-opties configureren in SQL Server, zoals [AlwaysOn beschikbaarheidsgroepen](http://blogs.technet.com/b/dataplatforminsider/archive/2014/08/25/sql-server-alwayson-offering-in-microsoft-azure-portal-gallery.aspx). Met een hoge beschikbaarheid ondersteunde optie biedt een extra SLA niet maar kunt u bereiken > 99,99% beschikbaarheid van de databases.

### <a name="market"></a>Tot de markt

**SQL-Database** is de juiste oplossing voor cloud ontworpen toepassingen wanneer de productiviteit van ontwikkelaars en snelle time-to-market zijn van cruciaal belang. Programmatische DBA-functies is perfect voor cloud architecten en ontwikkelaars als het verlaagt de noodzaak voor het beheer van het onderliggende besturingssysteem en de database. Bijvoorbeeld, kunt u de [REST API](http://msdn.microsoft.com/library/azure/dn505719.aspx) en de [PowerShell Cmdlets](http://msdn.microsoft.com/library/azure/dn546726.aspx) automatiseren en beheren van administratieve bewerkingen voor duizenden databases. Functies zoals [Groepen van elastische Database](sql-database-elastic-pool.md) kunnen u zich richten op de toepassingslaag en sneller werken uw oplossing op de markt.

**SQL Server wordt uitgevoerd op Azure VMs** is ideaal als u uw bestaande of nieuwe toepassingen vereisen grote databases, databases met elkaar samenhangen of toegang tot alle functies in SQL Server of Windows. Het is ook geschikt als u wilt dat voor het migreren van bestaande op-lokalen toepassingen en databases naar Azure als-is. Aangezien u niet wijzigen van de presentatie, toepassingen en gegevens lagen wilt, opslaan u tijd en geld op uw bestaande oplossing rearchitecting. In plaats daarvan kunt u zich richten op alle oplossingen migreren naar Azure en daarbij enkele prestatieverbeteringen die door het Azure platform nodig. Zie [Aanbevolen procedures voor SQL Server Azure virtuele Machines prestaties](../virtual-machines/virtual-machines-windows-sql-performance.md)voor meer informatie.

## <a name="summary"></a>Samenvatting

In dit artikel SQL-Database en SQL Server op Azure virtuele Machines (VMs) onderzocht en besproken gemeenschappelijke zakelijke motivators invloed kunnen hebben op uw beslissing. Hieronder volgt een samenvatting van de suggesties die u kunt overwegen:

Kies **Azure SQL-Database** als:

- U maakt nieuwe cloud-gebaseerde toepassingen kunnen profiteren van de kostenbesparingen en verbetering van de prestaties die cloud services bieden. Deze benadering biedt de voordelen van een volledig beheerde cloud service helpt lagere eerste keer op de markt en op lange termijn kosten optimalisatie kan bieden.

- Wilt u Microsoft common management bewerkingen uitvoeren op uw databases en betere beschikbaarheid van SLA's voor databases nodig hebt.



Kies **SQL Server op Azure VMs** als:

- Bestaande toepassingen van op de ruimten die u wilt migreren of uitbreiden naar de cloud, of als u wilt bouwen bedrijfstoepassingen die groter zijn dan 1 TB. Deze benadering biedt het voordeel van 100% SQL-compatibiliteit, database met grote capaciteit, volledige controle over SQL Server en Windows en beveiligde tunnel naar op gebouwen. Hierdoor minimaliseert u de kosten voor ontwikkeling en wijzigingen van bestaande toepassingen.

- U hebt van bestaande IT-bronnen en uiteindelijk eigendom patches, back-ups en beschikbaarheid van de database. Merk op dat sommige automatische functies aanzienlijk deze bewerkingen vereenvoudigen. 


## <a name="next-steps"></a>Volgende stappen
- Zie [zelfstudie voor SQL-Database: een SQL-database maken in minuten met behulp van de portal Azure](sql-database-get-started.md) aan de slag met SQL-Database.
- Zie [SQL-Database prijzen] (https://azure.microsoft.com/pricing/details/sql-database/).
- Zie [een SQL Server virtuele machine in Azure bepaling](../virtual-machines/virtual-machines-windows-portal-sql-server-provision.md) aan de slag met SQL Server op Azure VMs.
- Zie [SQL Server op een Azure Virtual Machine: leren pad](https://azure.microsoft.com/documentation/learning-paths/sql-azure-vm/).
