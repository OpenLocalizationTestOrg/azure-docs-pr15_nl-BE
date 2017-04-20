<properties
    pageTitle="Een back-up en herstel voor SQL Server | Microsoft Azure"
    description="Overwegingen bij het maken en terugzetten van back-up voor SQL Server-databases met Azure virtuele Machines beschreven."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="rothja"
    manager="jhubbard"
    editor=""
    tags="azure-resource-management" />

<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="08/19/2016"
    ms.author="jroth" />

# <a name="backup-and-restore-for-sql-server-in-azure-virtual-machines"></a>Back-up en herstel voor SQL Server in Azure virtuele Machines

## <a name="overview"></a>Overzicht

Back-ups van gegevens in SQL Server-databases is een belangrijk onderdeel van de strategie om te beschermen tegen gegevensverlies als gevolg van de toepassing of gebruiker fouten. Dit geldt eveneens voor SQL Server wordt uitgevoerd op Microsoft Azure virtuele Machines (VMs).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Voor SQL Server wordt uitgevoerd in Azure VMs, kunt u eigen back-up gebruiken en technieken met gekoppelde schijven voor de bestemming van de back-upbestanden terugzetten. Er is echter een grens aan het aantal schijven die u aan een Azure virtuele machine op basis van de [grootte van de virtuele machine koppelen kunt](virtual-machines-linux-sizes.md). Er is ook de overhead van Schijfbeheer te overwegen.

Beginnen met SQL Server 2014, kunt u een back-up en terugzetten op Microsoft Azure Blob-opslag. SQL Server 2016 biedt ook verbeteringen voor deze optie. Ook voor de databasebestanden opgeslagen in Microsoft Azure Blob-opslag, biedt SQL Server 2016 een optie voor back-ups van bijna onmiddellijk opgeleverd en snelle herstelt met Azure momentopnamen. Dit artikel biedt een overzicht van deze opties en meer informatie vindt u op [SQL Server back-up en terugzetten met Microsoft Azure Blob Storage-Service](https://msdn.microsoft.com/library/jj919148.aspx).

>[AZURE.NOTE] Zie voor een bespreking van de opties voor back-ups van zeer grote databases, [Multi-Terabyte SQL Server Database back-up strategieën voor Azure virtuele Machines](http://blogs.msdn.com/b/igorpag/archive/2015/07/28/multi-terabyte-sql-server-database-backup-strategies-for-azure-virtual-machines.aspx).

In de volgende secties bevatten informatie over de verschillende versies van SQL Server worden ondersteund in een Azure virtuele machine.

## <a name="sql-server-virtual-machines"></a>SQL Server virtuele Machines

Als uw exemplaar van SQL Server wordt uitgevoerd op een Azure Virtual Machine, uw databasebestanden al bevinden zich op gegevensschijven in Azure. Deze schijven live in Azure Blob-opslag. Dus nemen de redenen voor het maken van back-up van uw database en de benaderingen u wijzigen iets. De volgende oorzaken hebben. 

- Niet meer moet u back-ups voor een bescherming tegen hardware- of mediatoegangsbeheer omdat Microsoft Azure deze bescherming als onderdeel van de service Microsoft Azure biedt.

- Toch moet u back-ups voor een bescherming tegen gebruikersfouten of voor archiveringsdoeleinden, redenen regelgevende of administratieve doeleinden.

- U kunt het back-upbestand opslaan in Azure. Zie de volgende secties die richtsnoeren te voor de verschillende versies van SQL Server verschaffen voor meer informatie.

## <a name="sql-server-2016"></a>SQL Server 2016

Microsoft SQL Server 2016 ondersteunt functies voor [back-up en herstel met Azure BLOB's](https://msdn.microsoft.com/library/jj919148.aspx) gevonden in SQL Server 2014. Maar het bevat ook de volgende uitbreidingen:

| Verbetering van 2016               | Details                          |
|---------------------|-------------------------------|
| **Striping**              | Back-ups van Microsoft Azure blob opslag, ondersteunt SQL Server 2016 reservekopieën maken op meerdere BLOB's waarmee u back-ups van grote databases, tot een maximum van 12,8 TB.      |
| **Momentopname**                | Door het gebruik van momentopnamen Azure biedt back-up van SQL Server-bestand momentopname vrijwel ogenblikkelijke back-ups en herstelt de snelle voor databasebestanden die zijn opgeslagen met behulp van de Azure Blob storage-service. Hierdoor kunt u de back-up te vereenvoudigen en te herstellen. Back-up bestand momentopname ondersteunt ook punt in tijd herstellen. Voor meer informatie Zie [Momentopname back-ups van databasebestanden in Azure](https://msdn.microsoft.com/library/mt169363%28v=sql.130%29.aspx).   |
| **Beheerde back-up plannen**            | SQL Server beheerd Backup Azure biedt nu ondersteuning voor aangepaste schema's. Zie [SQL Server beheerd Backup Microsoft Azure](https://msdn.microsoft.com/library/dn449496.aspx)voor meer informatie.   |

Zie voor een zelfstudie van de mogelijkheden van SQL Server 2016 bij het gebruik van Azure Blob-opslag [Zelfstudie: 2016 van SQL Server-databases met behulp van de Microsoft Azure Blob storage-service](https://msdn.microsoft.com/library/dn466438.aspx).

## <a name="sql-server-2014"></a>SQL Server 2014

SQL Server 2014 bevat de volgende uitbreidingen:

1. **Back-up en herstel van Azure**:

 - *Back-up van SQL Server naar URL* heeft nu ondersteuning in SQL Server Management Studio. De optie voor back-up op Azure is nu beschikbaar bij het gebruik van back-up of terugzetten taak, of de wizard Onderhoudsplan in SQL Server Management Studio. Zie [Back-up van SQL Server naar URL](https://msdn.microsoft.com/library/jj919148%28v=sql.120%29.aspx)voor meer informatie.
 - *SQL Server beheerd Backup Azure* is nieuwe functionaliteit waarmee geautomatiseerde back-up beheer. Dit is vooral handig voor het automatiseren van back-up beheer voor SQL Server 2014 exemplaren die worden uitgevoerd op een computer met Azure. Zie [SQL Server beheerd Backup Microsoft Azure](https://msdn.microsoft.com/library/dn449496%28v=sql.120%29.aspx)voor meer informatie.
 - *Automatische back-up* biedt extra automatisering *SQL Server beheerd Backup Azure* op alle bestaande en nieuwe databases automatisch is ingeschakeld voor een SQL Server-VM in Azure. Zie voor meer informatie, [Geautomatiseerde back-up van SQL Server in Azure virtuele Machines](virtual-machines-windows-sql-automated-backup.md).
 - Zie voor een overzicht van de opties voor SQL Server 2014 Backup Azure [SQL Server back-up en terugzetten met Microsoft Azure Blob Storage-Service](https://msdn.microsoft.com/library/jj919148%28v=sql.120%29.aspx).

1. **Codering**: SQL Server 2014 ondersteunt het coderen van gegevens bij het maken van een back-up. Ondersteunt verschillende coderingsalgoritmen en het gebruik osf een certificaat of asymmetrische sleutel. Zie [Back-up-codering](https://msdn.microsoft.com/library/dn449489%28v=sql.120%29.aspx)voor meer informatie.

## <a name="sql-server-2012"></a>SQL Server 2012

Zie voor meer informatie over SQL Server back-up en terugzetten in SQL Server 2012, [back-up en terugzetten van SQL Server-Databases (SQL Server 2012)](https://msdn.microsoft.com/library/ms187048%28v=sql.110%29.aspx).

In SQL Server 2012 SP1 cumulatieve Update 2 starten, kunt u reservekopie en terugzetten vanuit de Azure Blob Storage-service. Deze verbetering kan worden gebruikt om back-up van SQL Server-databases op een SQL-Server waarop een Azure Virtual Machine of een exemplaar op gebouwen. Zie [SQL Server-back-up en terugzetten met Azure Blob Storage-Service](https://msdn.microsoft.com/library/jj919148%28v=sql.110%29.aspx)voor meer informatie.

Enkele van de voordelen van het gebruik van de Azure Blob storage-service omvatten de mogelijkheid om de 16 limiet voor gekoppelde schijven, beheergemak, de directe beschikbaarheid van het back-upbestand met een ander exemplaar van SQL Server-exemplaar op een Azure virtuele machine wordt uitgevoerd, of een exemplaar van ruimten voor migratie of noodherstel overslaan. Zie de sectie *prestaties* in [SQL Server back-up en terugzetten met Azure Blob Storage-Service](https://msdn.microsoft.com/library/jj919148%28v=sql.110%29.aspx)voor een volledige lijst van voordelen bij het gebruik van een Azure blob storage-service voor back-ups van SQL Server.

Zie voor informatie over probleemoplossing en aanbevelingen voor beste praktijken, [back-up en terugzetten Best Practices (Azure Blob Storage-Service)](https://msdn.microsoft.com/library/jj919149%28v=sql.110%29.aspx).

## <a name="sql-server-2008"></a>SQL Server 2008

Zie [back-ups maken en terugzetten van Databases in SQL Server (SQL Server 2008 R2)](https://msdn.microsoft.com/library/ms187048%28v=sql.105%29.aspx)voor SQL Server back-up en herstellen in SQL Server 2008 R2.

Zie [back-ups maken en terugzetten van Databases in SQL Server (SQL Server 2008)](https://msdn.microsoft.com/library/ms187048%28v=sql.100%29.aspx)voor SQL Server back-up en herstellen in SQL Server 2008.

## <a name="next-steps"></a>Volgende stappen

Als u van plan bent uw implementatie van SQL Server in een VM Azure, vindt u provisioning richtsnoeren in de volgende zelfstudie: [inrichten van een SQL Server virtuele Machine op Azure Azure Resource Manager](virtual-machines-windows-portal-sql-server-provision.md).

Hoewel het maken en terugzetten van back-up kunnen worden gebruikt om uw gegevens te migreren, zijn mogelijk gemakkelijker migratiepaden naar SQL Server op een Azure VM. Zie [een Database aan SQL Server op een Azure VM migreren](virtual-machines-windows-migrate-sql.md)voor een volledige beschrijving van opties voor de migratie en aanbevelingen.

Bekijk andere [bronnen voor het uitvoeren van SQL Server in Azure virtuele Machines](virtual-machines-windows-sql-server-iaas-overview.md).
