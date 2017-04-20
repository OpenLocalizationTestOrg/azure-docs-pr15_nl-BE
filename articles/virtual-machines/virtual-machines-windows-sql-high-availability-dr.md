<properties
    pageTitle="Hoge beschikbaarheid en noodherstel voor SQL Server | Microsoft Azure"
    description="Een bespreking van de verschillende soorten HADR strategieën voor SQL Server wordt uitgevoerd in Azure virtuele Machines."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="MikeRayMSFT"
    manager="jhubbard"
    editor=""
    tags="azure-service-management"/>
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="10/20/2016"
    ms.author="MikeRayMSFT" />

# <a name="high-availability-and-disaster-recovery-for-sql-server-in-azure-virtual-machines"></a>Hoge beschikbaarheid en noodherstel herstel voor SQL Server in Azure virtuele Machines

## <a name="overview"></a>Overzicht

Microsoft Azure virtual machines (VMs) met SQL Server kunt lagere kosten voor een hoge beschikbaarheid en noodherstel (HADR) database hersteloplossing. De meeste oplossingen voor SQL Server HADR worden ondersteund in Azure virtuele machines als Azure alleen-lezen en als hybride oplossingen. In een Azure-only-oplossing, de hele HADR wordt uitgevoerd in Azure. In een hybride configuratie uitgevoerd deel van de oplossing in Azure en het andere deel wordt uitgevoerd op-hoofdkwartier in uw organisatie. De flexibiliteit van de Azure-omgeving kunt u geheel of gedeeltelijk voldoen aan het budget en de eisen van uw SQL Server-databasesystemen HADR naar Azure verplaatsen.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]


## <a name="understanding-the-need-for-an-hadr-solution"></a>Wat is de noodzaak van een oplossing voor HADR?

Het is aan u om ervoor te zorgen dat uw databasesysteem beschikt over de mogelijkheden van HADR waarvoor de serviceniveau overeenkomst (SLA). Het feit dat de Azure biedt mechanismen voor hoge beschikbaarheid, zoals service Retoucheerpenseel voor cloud services en herstel foutdetectie voor de virtuele Machines garandeert zelf niet voldoet u aan de gewenste SLA. Deze mechanismen beschermen de hoge beschikbaarheid van de VMs, maar niet de hoge beschikbaarheid van SQL Server wordt uitgevoerd binnen het VMs. Het is mogelijk voor het exemplaar van SQL Server worden uitgevoerd terwijl de VM online en gezond is. Bovendien moeten zelfs de hoge beschikbaarheid mechanismen die zijn vastgesteld door de Azure toestaan voor uitval van het VMs als gevolg van gebeurtenissen zoals het herstellen van software of hardwarefouten en upgrades voor besturingssystemen.

Geo redundante opslag (GRS) in Azure, die wordt geïmplementeerd met een functie genaamd geo-replicatie, mogelijk ook niet een voldoende disaster recovery-oplossing voor uw databases. Omdat geo-replicatie asynchroon gegevens verzendt, recente updates worden in geval van een ramp verloren. Meer informatie over de beperkingen van geo-replicatie worden behandeld in de sectie [Geo-replicatie wordt niet ondersteund voor gegevens- en logboekbestanden op afzonderlijke schijven](#geo-replication-support) .

## <a name="hadr-deployment-architectures"></a>HADR implementatie-architectuur

SQL Server HADR technologieën die worden ondersteund in Azure omvatten:

- [Altijd op de van beschikbaarheidsgroepen](https://technet.microsoft.com/library/hh510230.aspx)
- [Database-Mirroring](https://technet.microsoft.com/library/ms189852.aspx)
- [Logboekbestanden](https://technet.microsoft.com/library/ms187103.aspx)
- [Backup and Restore met Azure Blob Storage-Service](https://msdn.microsoft.com/library/jj919148.aspx)
- [Altijd op de exemplaren van de Failover-Cluster](https://technet.microsoft.com/library/ms189134.aspx)

Het is mogelijk de technologieën samen om het implementeren van een SQL Server-oplossing met hoge beschikbaarheid en een disaster recovery mogelijkheden te combineren. Afhankelijk van de technologie die u gebruikt mogelijk een hybride-implementatie een VPN-tunnel met de Azure virtueel netwerk. In de volgende secties tonen u enkele van de voorbeeld-implementatie-architectuur.

## <a name="azure-only-high-availability-solutions"></a>Alleen Azure: oplossingen met hoge beschikbaarheid

Als u een oplossing met hoge beschikbaarheid voor SQL Server-databases in Azure altijd op beschikbaarheid groepen hebben of database-mirroring.

| Technologie                               | Voorbeeld van de architectuur                    |
| ---------------------------------------- | ---------------------------------------- |
| **Altijd op de van beschikbaarheidsgroepen**        | Alle beschikbaarheid replica's uitgevoerd in Azure VMs voor maximale beschikbaarheid in hetzelfde gebied. Moet u voor het configureren van een domeincontroller VM, omdat Windows Server Failover Clustering (WSFC) een Active Directory-domein vereist.<br/> ![Altijd op de van beschikbaarheidsgroepen](./media/virtual-machines-windows-sql-high-availability-dr/azure_only_ha_always_on.gif)<br/>Voor meer informatie Zie [Configureren altijd op beschikbaarheidsgroepen Azure (GUI)](virtual-machines-windows-portal-sql-alwayson-availability-groups.md). |
| **Altijd op de exemplaren van de Failover-Cluster** | Failover-Cluster exemplaren (FCI), die gedeelde opslag vereist is, kan op 2 verschillende manieren worden gemaakt.<br/><br/>1. een FCI op een opslag wordt ondersteund door een oplossing van derden-clustering in Azure VMs met twee knooppunten-WSFC. Zie [een bestandsshare met behulp van WSFC en 3de partij software SIOS Datakeeper voor maximale beschikbaarheid](https://azure.microsoft.com/blog/high-availability-for-a-file-share-using-wsfc-ilb-and-3rd-party-software-sios-datakeeper/)voor een specifiek voorbeeld SIOS DataKeeper gebruikt.<br/><br/>2. een FCI op een knooppunt van twee WSFC met externe iSCSI-doel in Azure VMs gedeelde blokopslag via ExpressRoute. NetApp particuliere opslag (NPS) heeft bijvoorbeeld een iSCSI-doel via ExpressRoute met Equinix naar Azure VMs.<br/><br/>Voor gedeelde opslag van derden en oplossingen voor replicatie, moet u contact opnemen met de leverancier voor alle problemen met betrekking tot de toegang tot gegevens op een failover.<br/><br/>Houd er rekening mee dat met behulp van FCI op [Azure bestandsopslag](https://azure.microsoft.com/services/storage/files/) wordt nog niet ondersteund, omdat de premie opslag geen gebruik van deze oplossing. Wij werken voor de ondersteuning van dit binnenkort. |

## <a name="azure-only-disaster-recovery-solutions"></a>Alleen Azure: Disaster recovery oplossingen

Als u een disaster recovery oplossing voor uw SQL Server-databases in Azure altijd op beschikbaarheid groepen, database-mirroring of back-up en terugzetten met opslag BLOB's.

| Technologie                               | Voorbeeld van de architectuur                    |
| ---------------------------------------- | ---------------------------------------- |
| **Altijd op de van beschikbaarheidsgroepen**        | Beschikbaarheid van replica's die over meerdere datacenters in Azure VMs voor noodherstel. Deze cross-regio oplossing beschermt u tegen de volledige site onderhoud. <br/> ![Altijd op de van beschikbaarheidsgroepen](./media/virtual-machines-windows-sql-high-availability-dr/azure_only_dr_alwayson.png)<br/>In gebied moeten alle replica's in dezelfde cloud-service en de VNet dezelfde. Omdat elk gebied een afzonderlijke VNet, moeten deze oplossingen VNet VNet connectiviteit. Zie [een VPN website in de klassieke Azure portal configureren](../vpn-gateway/vpn-gateway-site-to-site-create.md)voor meer informatie. |
| **Database-Mirroring**                   | Hoofdsom en mirror en servers in verschillende datacentra voor noodherstel uitgevoerd. U moet implementeren met behulp van servercertificaten omdat active directory-domein kan niet over meerdere datacenters verdeeld.<br/>![Database-Mirroring](./media/virtual-machines-windows-sql-high-availability-dr/azure_only_dr_dbmirroring.gif) |
| **Backup and Restore met Azure Blob Storage-Service** | Productiedatabases back-up rechtstreeks naar een blob-opslag in een ander datacenter voor noodherstel.<br/>![Back-up en terugzetten](./media/virtual-machines-windows-sql-high-availability-dr/azure_only_dr_backup_restore.gif)<br/>Zie [back-up en herstel voor SQL Server in Azure virtuele Machines](virtual-machines-windows-sql-backup-recovery.md)voor meer informatie. |

## <a name="hybrid-it-disaster-recovery-solutions"></a>Hybride IT: Disaster recovery oplossingen

Als u een disaster recovery oplossing hebt voor uw SQL Server-databases in een hybride IT-omgeving met altijd op beschikbaarheidsgroepen, database-mirroring voor logboekbestanden en back-up en terugzetten met Azure blog opslag.

| Technologie                               | Voorbeeld van de architectuur                    |
| ---------------------------------------- | ---------------------------------------- |
| **Altijd op de van beschikbaarheidsgroepen**        | Sommige beschikbaarheid replica's uitgevoerd in Azure VMs en andere replica's in ruimten voor noodherstel van cross-site wordt uitgevoerd. Productie van gebouwen kan worden of in een datacenter Azure.<br/>![Altijd op de van beschikbaarheidsgroepen](./media/virtual-machines-windows-sql-high-availability-dr/hybrid_dr_alwayson.gif)<br/>Omdat alle replica's van beschikbaarheid in hetzelfde cluster WSFC zijn moeten, moet de cluster WSFC beide netwerken (een cluster met meerdere subnet WSFC) omvatten. Deze configuratie is vereist voor een VPN-verbinding tussen Azure en het netwerk op gebouwen.<br/><br/>Storingen te herstellen van uw databases, moet u ook een replica-domeincontroller op de disaster recovery-site installeren.<br/><br/>Het is mogelijk met de Wizard Replica toevoegen in SSMS een Azure replica toevoegen aan een bestaande altijd op beschikbaarheid. Zie zelfstudie voor meer informatie: uw altijd op beschikbaarheid-groep uitbreiden naar Azure. |
| **Database-Mirroring**                   | Een partner wordt uitgevoerd in een VM Azure en andere actieve op-ruimten voor noodherstel van cross-site met behulp van servercertificaten. Partners hoeft niet te worden in hetzelfde Active Directory-domein en geen VPN-verbinding is vereist.<br/>![Database-Mirroring](./media/virtual-machines-windows-sql-high-availability-dr/hybrid_dr_dbmirroring.gif)<br/>Een andere database-mirroring scenario omvat een partner uitgevoerd in een VM Azure en de andere actieve op-hoofdkwartier in hetzelfde Active Directory-domein voor noodherstel van cross-site. Een [VPN-verbinding tussen de Azure virtueel netwerk en het netwerk op ruimten](../vpn-gateway/vpn-gateway-site-to-site-create.md) is vereist.<br/><br/>Storingen te herstellen van uw databases, moet u ook een replica-domeincontroller op de disaster recovery-site installeren. |
| **Logboekbestanden**                         | Één server worden uitgevoerd in een VM Azure en andere actieve op-ruimten voor noodherstel van cross-site. Logboekbestanden afhankelijk van Windows bestanden delen, zodat u een VPN-verbinding tussen de Azure virtueel netwerk en het netwerk op ruimten is vereist.<br/>![Logboekbestanden](./media/virtual-machines-windows-sql-high-availability-dr/hybrid_dr_log_shipping.gif)<br/>Storingen te herstellen van uw databases, moet u ook een replica-domeincontroller op de disaster recovery-site installeren. |
| **Backup and Restore met Azure Blob Storage-Service** | Op-premises productiedatabases reservekopie rechtstreeks naar Azure blob-opslag voor noodherstel.<br/>![Back-up en terugzetten](./media/virtual-machines-windows-sql-high-availability-dr/hybrid_dr_backup_restore.gif)<br/>Zie [back-up en herstel voor SQL Server in Azure virtuele Machines](virtual-machines-windows-sql-backup-recovery.md)voor meer informatie. |

## <a name="important-considerations-for-sql-server-hadr-in-azure"></a>Belangrijke overwegingen voor SQL Server HADR in Azure

Azure VMs, opslag en netwerken hebben andere operationele kenmerken dan een op-lokalen, niet-gevirtualiseerde IT-infrastructuur. Een succesvolle implementatie van een HADR SQL Server-oplossing in Azure, moet u weten over deze verschillen en ontwerpen van uw oplossing voor hen.

### <a name="high-availability-nodes-in-an-availability-set"></a>Hoge beschikbaarheid-knooppunten in een set van beschikbaarheid

Beschikbaarheid sets in Azure, kunt u de knooppunten met hoge beschikbaarheid in afzonderlijke domeinen veroorzaakt (FDs) en Update domeinen (UDs) plaatsen. Voor VMs Azure in bepaalde beschikbaarheid worden geplaatst, moet u ze implementeren in dezelfde cloud-service. Alleen knooppunten in dezelfde service cloud deel uitmaken van dezelfde set beschikbaarheid. Voor meer informatie, Zie [beheren van de beschikbaarheid van virtuele Machines](virtual-machines-windows-manage-availability.md).

### <a name="wsfc-cluster-behavior-in-azure-networking"></a>WSFC cluster gedrag in Azure-netwerken

De niet-RFC-compatibele DHCP-service in Azure kan leiden tot het maken van bepaalde WSFC-clusterconfiguraties mislukt als gevolg van de naam van het cluster wordt een dubbele IP-adres toegewezen, bijvoorbeeld hetzelfde IP-adres als een van de clusterknooppunten. Dit is een probleem wanneer u altijd op beschikbaarheidsgroepen, die afhankelijk van de functie WSFC is implementeren.

Neem het scenario als een cluster met twee knooppunten wordt gemaakt en on line gebracht:

1. Het cluster on line komt vervolgens knooppunt1 vraagt om een dynamisch toegewezen IP-adres voor de naam van het cluster.

2. Geen IP-adres dan knooppunt1 eigen IP-adres wordt gegeven door de DHCP-service, omdat de DHCP-service herkent dat de aanvraag afkomstig van NODE1 is zelf.

3. Windows detecteert dat een dubbel adres wordt toegewezen aan knooppunt1 en de naam van het cluster en de standaardclustergroep niet on line worden gebracht.

4. De standaardclustergroep verplaatst naar knooppunt2 die behandelt knooppunt1 van IP-adres als het cluster-IP-adres en de standaardclustergroep on line brengt.

5. Wanneer Knooppunt2 probeert om verbinding met NODE1, laat pakketten die zijn geadresseerd aan knooppunt1 Knooppunt2 nooit omdat deze knooppunt1 van IP-adres wordt herleid naar zichzelf. Knooppunt2 kan geen verbinding maken met NODE1, quorum verliest en het cluster wordt afgesloten.

6. In de tussentijd knooppunt1 pakketten kunnen worden verzonden naar knooppunt2, maar kan niet antwoorden, Knooppunt2. Knooppunt1 quorum verliest wordt afgesloten en het cluster.

In dit scenario kan worden vermeden door een niet-gebruikte statische IP-adres, bijvoorbeeld een link-local IP-adres zoals 169.254.1.1, toewijzen aan de naam van het cluster om de naam van het cluster on line te brengen. Zie [Windows Failover-Cluster in Azure voor altijd op beschikbaarheidsgroepen configureren](http://social.technet.microsoft.com/wiki/contents/articles/14776.configuring-windows-failover-cluster-in-windows-azure-for-alwayson-availability-groups.aspx)voor het vereenvoudigen van dit proces.

Voor meer informatie Zie [Configureren altijd op beschikbaarheidsgroepen Azure (GUI)](virtual-machines-windows-portal-sql-alwayson-availability-groups.md).

### <a name="availability-group-listener-support"></a>Ondersteuning voor beschikbaarheid listener

Beschikbaarheid groep listeners worden ondersteund op Azure VMs waarop Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2 en Windows Server 2016. Deze ondersteuning wordt mogelijk gemaakt door het gebruik van taakverdeling eindpunten op de Azure VMs die beschikbaarheid groepsknooppunten zijn ingeschakeld. Stappen voor de listeners werkt voor zowel clienttoepassingen die worden uitgevoerd in Azure, alsmede die van gebouwen met een speciale configuratie moet u volgen.

Er zijn twee belangrijke opties voor het instellen van de listener: (openbaar) externe of interne. De listener voor externe (openbare) maakt gebruik van een internetverbinding taakverdeling en is gekoppeld aan een openbare virtuele IP-(VIP) die toegankelijk is via het internet. Een interne listener gebruikt een interne taakverdeling en alleen ondersteuning van clients binnen hetzelfde virtuele netwerk. Voor de verdeling van type laden, u moet directe Server als resultaat geven. 

Als de groep beschikbaarheid omvat meerdere Azure subnetten (zoals een distributie die Azure regio's snijdt), de verbindingsreeks van de client moet zijn "**MultisubnetFailover = True**". Dit resulteert in parallelle verbindingspogingen met de replica's in de verschillende subnetten. Zie voor meer informatie over het instellen van een listener

- [Een listener ILB voor altijd op beschikbaarheidsgroepen in Azure configureren](virtual-machines-windows-portal-sql-ps-alwayson-int-listener.md).
- [Configureren van een externe listener voor altijd op beschikbaarheidsgroepen in Azure](virtual-machines-windows-classic-ps-sql-ext-listener.md).

U kunt nog steeds verbinding maken met elke replica beschikbaarheid afzonderlijk door rechtstreeks verbinding maken met het exemplaar van de. Ook, omdat het altijd op beschikbaarheidsgroepen zijn achterwaarts compatibel met database-mirroring van clients, kunt u de beschikbaarheid van replica's als database-mirroring partners als de replica's zijn vergelijkbaar met de database-mirroring geconfigureerd:

- Replica van een primaire en een secundaire

- De secundaire replica is geconfigureerd als niet-leesbare (**Leesbare secundaire** optie ingesteld op **Nee**)

Een voorbeeld van client-verbindingsreeks die overeenkomt met deze database-mirroring-achtige configuratie met behulp van ADO.NET- of SQL Server Native Client is hieronder:

    Data Source=ReplicaServer1;Failover Partner=ReplicaServer2;Initial Catalog=AvailabilityDatabase;

Zie voor meer informatie op de clients verbinding maken:

- [Met behulp van trefwoorden voor Connection String met SQL Server Native Client](https://msdn.microsoft.com/library/ms130822.aspx)
- [Clients verbinden met een Database-Mirroring sessie (SQL Server)](https://technet.microsoft.com/library/ms175484.aspx)
- [Verbinding maken met de groep beschikbaarheid Listener in hybride IT](http://blogs.msdn.com/b/sqlalwayson/archive/2013/02/14/connecting-to-availability-group-listener-in-hybrid-it.aspx)
- [Beschikbaarheid groep luisteraars, clientverbindingen en toepassing Failover (SQL Server)](https://technet.microsoft.com/library/hh213417.aspx)
- [Met behulp van tekenreeksen voor Database-Mirroring met beschikbaarheidsgroepen](https://technet.microsoft.com/library/hh213417.aspx)

### <a name="network-latency-in-hybrid-it"></a>Netwerklatentie in hybride IT

U moet uw oplossing HADR op de veronderstelling dat er perioden met hoge netwerkvertraging tussen uw netwerk op gebouwen en Azure implementeren. Bij de distributie replica's in Azure, moet u asynchronous commit in plaats van synchrone vastleggen voor de synchronisatie. Wanneer database mirroring servers implementeren zowel op locatie als in Azure, gebruikt de modus hoge prestaties in plaats van de modus Hoog.

### <a name="geo-replication-support"></a>Ondersteuning voor geo-replicatie

Geo-replicatie in Azure schijven biedt geen ondersteuning voor het gegevensbestand en het logboekbestand van dezelfde database kunnen worden opgeslagen op verschillende schijven. GRS repliceert wijzigingen op elke schijf afzonderlijk en asynchroon. Dit mechanisme zorgt ervoor dat de volgorde schrijven binnen een enkele schijf op de kopie geo gerepliceerd, maar niet tussen geo-gerepliceerde kopieën van meerdere schijven. Als u een database voor het opslaan van het bestand en het logboekbestand op afzonderlijke schijven configureert, kunnen het herstelde schijven na een ramp een bijgewerkte kopie van het bestand dan het logboekbestand, waarmee het logboek vooraf geschreven in SQL Server en de ACID-eigenschappen van transacties bevatten. Als u de optie voor het uitschakelen van geo-replicatie op de rekening van de opslag niet hebt, moet u alle gegevens en log-bestanden voor een bepaalde database op dezelfde schijf behouden. Als u meer dan één schijf vanwege de grootte van de database wilt gebruiken, moet u implementeren op een van de disaster recovery oplossingen hierboven vermelde om gegevensredundantie te waarborgen.

## <a name="next-steps"></a>Volgende stappen

Als u een Azure virtuele machine maken met SQL Server nodig, Zie [voor het inrichten van een SQL Server virtuele Machine op Azure](virtual-machines-windows-portal-sql-server-provision.md).

Als u de beste prestaties van SQL Server wordt uitgevoerd op een Azure VM, Zie de richtlijnen in [Aanbevolen procedures voor SQL Server in Azure virtuele Machines prestaties](virtual-machines-windows-sql-performance.md).

Zie [SQL Server Azure virtuele Machines](virtual-machines-windows-sql-server-iaas-overview.md)voor andere onderwerpen die betrekking hebben op het SQL Server wordt uitgevoerd in Azure VMs.

### <a name="other-resources"></a>Andere bronnen

- [Installeer een nieuw Active Directory-forest in Azure](../active-directory/active-directory-new-forest-virtual-machine.md)
- [WSFC Cluster voor altijd op beschikbaarheidsgroepen in Azure VM maken](http://gallery.technet.microsoft.com/scriptcenter/Create-WSFC-Cluster-for-7c207d3a)
