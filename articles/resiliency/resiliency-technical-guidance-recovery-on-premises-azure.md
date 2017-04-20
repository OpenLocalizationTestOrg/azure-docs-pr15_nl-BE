<properties
   pageTitle="Technische begeleiding: herstel op ruimten naar Azure | Microsoft Azure"
   description="Artikel op het begrip en herstel ontwerpen systemen van infrastructuur voor bedrijven om Azure"
   services=""
   documentationCenter="na"
   authors="adamglick"
   manager="saladki"
   editor=""/>

<tags
   ms.service="resiliency"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/18/2016"
   ms.author="aglick"/>

#<a name="azure-resiliency-technical-guidance-recovery-from-on-premises-to-azure"></a>Technische richtsnoeren Azure tolerantie: herstel op ruimten naar Azure

Azure biedt een uitgebreide reeks van services voor het inschakelen van de uitbreiding van een datacenter in ruimten met Azure voor hoge beschikbaarheid en noodherstel:

* __Netwerk__: met een virtueel particulier netwerk uitbreiden u veilig uw netwerk op het bedrijf naar de cloud.
* __Berekenen__: klanten die werken met Hyper-V on-premises kunnen "tilt en shift" bestaande virtuele machines (VMs) naar Azure.
* __Opslag__: StorSimple uw bestandssysteem uitbreidt naar Azure opslag. De Azure back-service biedt back-up van bestanden en databases van SQL Azure opslag.
* __Databasereplicatie__: met SQL Server 2014 (of hoger) beschikbaarheidsgroepen, kunt u hoge beschikbaarheid en herstel na storing herstel implementeren voor uw gegevens van ruimten.

##<a name="networking"></a>Netwerken

Azure virtueel netwerk kunt u een logisch geïsoleerde sectie maken in Azure en veilig uw datacenter in ruimten of een enkele clientcomputer verbinding met behulp van een IPSec-verbinding. Met een virtueel netwerk, kunt u profiteren van de infrastructuur voor schaalbare, op verzoek in Azure terwijl de verbinding met gegevens en toepassingen op-ruimten, met inbegrip van systemen waarop Windows Server, mainframes en UNIX. Zie [Azure networking-documentatie](../virtual-network/virtual-networks-overview.md) voor meer informatie.

##<a name="compute"></a>Berekenen

Als u Hyper-V on-premises, kunt u "tilt en shift' bestaande virtuele machines, Azure en serviceproviders voor Windows Server 2012 (of hoger), uitgevoerd zonder aanbrengen in de VM wijzigingen of VM converteren. Zie voor meer informatie [over schijven en VHD's voor virtuele machines die Azure](../virtual-machines/virtual-machines-linux-about-disks-vhds.md).

##<a name="azure-site-recovery"></a>Azure-Site herstellen

Als u Systeemherstel wilt als service (DRaaS), biedt Azure [Azure Site herstellen](https://azure.microsoft.com/services/site-recovery/). Azure-Site-Recovery biedt uitgebreide bescherming van VMware, Hyper-V en fysieke servers. Met herstel Azure-Site kunt u een andere server voor op gebouwen of Azure als de recovery-site. Zie de [documentatie Azure Site herstellen](https://azure.microsoft.com/documentation/services/site-recovery/)voor meer informatie over Azure Site herstellen.

##<a name="storage"></a>Opslag

Er zijn verschillende opties voor het gebruik van Azure als een back-up site voor gegevens voor gebouwen.

###<a name="storsimple"></a>StorSimple

StorSimple integreert veilig en transparant cloud opslag voor toepassingen voor gebouwen. Hij biedt ook een enkel toestel die krachtige trapsgewijze lokaal en cloud opslag, live te archiveren, cloud-gebaseerde gegevensbescherming en noodherstel biedt. Zie voor meer informatie de [productpagina StorSimple](https://azure.microsoft.com/services/storsimple/).

###<a name="azure-backup"></a>Azure back-up

Azure Backup kunt u back-ups van de wolk met behulp van de bekende hulpprogramma's in Windows Server 2012 (of hoger), Windows Server 2012 Essentials (of hoger) en System Center 2012 Data Protection Manager (of hoger). Deze hulpprogramma's kunt een workflow voor het beheer van back-up die onafhankelijk is van de locatie van de back-ups, of een lokale schijf of Azure opslag. Nadat de back-up naar de cloud, kunnen geautoriseerde gebruikers eenvoudig back-ups naar een willekeurige server herstellen.

Met incrementele back-ups worden alleen wijzigingen in bestanden overgebracht naar de cloud. Hierdoor efficiënt opslagruimte gebruiken, het bandbreedteverbruik verminderen en point-in-time herstel van meerdere versies van de gegevens. U kunt ook extra functies, zoals het bewaarbeleid voor gegevens, gegevenscompressie en overdracht van gegevens beperken. Azure gebruiken als locatie voor de back-up worden de back-ups automatisch 'elders' voor de hand liggende voordeel heeft. Hierdoor wordt de extra eisen voor het beveiligen en beschermen on-site back-upmedia.

Zie voor meer informatie [Wat is Azure back-up?](../backup/backup-introduction-to-azure-backup.md) en [Azure back-up voor DPM gegevens configureren](https://technet.microsoft.com/library/jj728752.aspx).

##<a name="database"></a>Database

Als u een disaster recovery oplossing hebt voor uw SQL Server-databases in een hybride IT-omgeving met behulp van AlwaysOn beschikbaarheidsgroepen, database-mirroring voor logboekbestanden en back-up en terugzetten met Azure Blob-opslag. Al deze oplossingen gebruiken SQL Server wordt uitgevoerd op Azure virtuele Machines.

AlwaysOn beschikbaarheidsgroepen kunnen worden gebruikt in een hybride IT-omgeving waar databasereplica bestaan beide ruimten op en in de cloud. Dit wordt weergegeven in het volgende diagram.

![SQL Server AlwaysOn beschikbaarheidsgroepen in een hybride cloud-architectuur](./media/resiliency-technical-guidance-recovery-on-premises-azure/SQL_Server_Disaster_Recovery-3.png)

Database-mirroring kan ook omvatten op gebouwen-servers en de cloud bij een instelling op basis van certificaten. In het volgende diagram illustreert dit concept.

![SQL Server database-mirroring in een hybride cloud-architectuur](./media/resiliency-technical-guidance-recovery-on-premises-azure/SQL_Server_Disaster_Recovery-4.png)

Logboekbestanden kan worden gebruikt voor het synchroniseren van een database op de ruimten met een SQL Server-database in een Azure virtuele machine.

![Logboekbestanden van SQL Server in een hybride cloud-architectuur](./media/resiliency-technical-guidance-recovery-on-premises-azure/SQL_Server_Disaster_Recovery-5.png)

Ten slotte, u kunt back-up van een database op het bedrijf rechtstreeks naar Azure Blob-opslag.

![Back-up van SQL Server naar Azure Blob-opslag in een hybride cloud-architectuur](./media/resiliency-technical-guidance-recovery-on-premises-azure/SQL_Server_Disaster_Recovery-6.png)

Zie voor meer informatie, [hoge beschikbaarheid en noodherstel herstel voor SQL Server in Azure virtuele machines](../virtual-machines/virtual-machines-windows-sql-high-availability-dr.md) en [back-up en herstel voor SQL Server in Azure virtuele machines](../virtual-machines/virtual-machines-windows-sql-backup-recovery.md).

##<a name="checklists-for-on-premises-recovery-in-microsoft-azure"></a>Controlelijsten voor herstel in Microsoft Azure voor gebouwen

###<a name="networking"></a>Netwerken

  1. Raadpleeg de sectie netwerk van dit document.
  2. Virtueel netwerk gebruiken beveiligde verbinding tot op het bedrijf naar de cloud.

###<a name="compute"></a>Berekenen

  1. Bekijk de Compute-sectie van dit document.
  2. VMs verplaatsen tussen Hyper-V en Azure.

###<a name="storage"></a>Opslag

  1. Raadpleeg de sectie opslag van dit document.
  2. Profiteer van de StorSimple services voor het gebruik van cloud opslag.
  3. De service Azure back-up gebruiken.

###<a name="database"></a>Database

  1. Raadpleeg de sectie Database van dit document.
  2. Overweeg het gebruik van SQL Server op Azure VMs als de back-up.
  3. AlwaysOn beschikbaarheidsgroepen instellen.
  4. Configureren op basis van certificaten spiegelen.
  5. Gebruik de logboekbestanden.
  6. Back-up voor bedrijfsruimten databases op Azure Blob-opslag.

##<a name="next-steps"></a>Volgende stappen

Dit artikel maakt deel uit van een serie gericht op [Azure resiliency technische richtsnoeren](./resiliency-technical-guidance.md). Het volgende artikel in deze serie is [herstel van gegevens beschadigd raken of per ongeluk wordt verwijderd](./resiliency-technical-guidance-recovery-data-corruption.md).