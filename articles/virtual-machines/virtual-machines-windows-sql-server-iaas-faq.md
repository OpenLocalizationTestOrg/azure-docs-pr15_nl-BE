<properties
    pageTitle="SQL Server op Azure virtuele Machines Veelgestelde vragen | Microsoft Azure"
    description="Dit artikel bevat antwoorden op veelgestelde vragen over het uitvoeren van SQL Server op Azure VMs."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="v-shysun"
    manager="felixwu"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="09/13/2016"
    ms.author="v-shysun"/>

# <a name="sql-server-on-azure-virtual-machines-faq"></a>SQL Server op Azure virtuele Machines Veelgestelde vragen

In dit onderwerp vindt antwoorden op enkele veelgestelde vragen over het uitvoeren van [SQL Server op Azure virtuele Machines](https://azure.microsoft.com/services/virtual-machines/sql-server/).

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="frequently-asked-questions"></a>Veelgestelde vragen

1. **Hoe maak ik een Azure VM met SQL Server**

    Er zijn twee manieren om dit te doen. De gemakkelijkste oplossing is voor het maken van een virtuele Machine met SQL Server. Zie voor een zelfstudie over het aanmelden voor Azure en het maken van een SQL VM vanaf de portal, [dient een SQL Server virtuele machine in de Portal Azure](virtual-machines-windows-portal-sql-server-provision.md). U hebt ook de optie van handmatig SQL Server installeren op een VM en opnieuw gebruiken van een licentie op lokalen met de [Mobiliteit van de licentie via Software Assurance op Azure](https://azure.microsoft.com/pricing/license-mobility/).

1. **Wat is het verschil tussen SQL VMs en de service SQL-Database?**

    In dat geval waarop SQL Server op een Azure virtuele machine niet die anders is dan SQL Server wordt uitgevoerd in een extern datacenter. [SQL-Database](../sql-database/sql-database-technical-overview.md) biedt daarentegen database als een service. Met de SQL-Database, u geen toegang tot de computers die host zijn van uw databases. Zie voor een volledige vergelijking [kiest u de optie SQL-Server van een wolk: Azure SQL (PaaS)-Database of SQL Server op Azure VMs (IaaS)](../sql-database/sql-database-paas-vs-sql-server-iaas.md).

1. **Hoe kan ik mijn SQL Server-database op ruimten migreren naar de Cloud?**

    Eerst een Azure virtuele machine maken met een exemplaar van SQL Server. Vervolgens migreren uw databases op gebouwen aan die instantie. Voor strategieën voor migratie van gegevens, Zie [migreren naar SQL Server in een VM Azure SQL Server-database](virtual-machines-windows-migrate-sql.md).

2. **Kan ik een geïnstalleerde onderdelen wijzigen of een tweede exemplaar van SQL Server op dezelfde VM installeren?**

    Ja. De installatiemedia van SQL Server bevindt zich in een map op station **C** . Voer **Setup.exe** uit vanaf die locatie toe te voegen nieuwe exemplaren van SQL Server of andere geïnstalleerde onderdelen van SQL Server op de computer wijzigen.

3. **Hoe voer ik een upgrade naar een nieuwe versie/editie van SQL Server in een VM Azure?**

    Er is momenteel geen in-place upgrade voor SQL Server wordt uitgevoerd in een VM Azure. Een nieuwe Azure virtuele machine maken met de gewenste versie/editie van SQL Server en vervolgens uw databases migreren naar de nieuwe server met behulp van standaard [data migration technieken](virtual-machines-windows-migrate-sql.md).

4. **Hoe kan ik mijn licentie gegeven exemplaar van SQL Server op een Azure VM installeren?**

    De SQL Server-installatiemedia voor Windows Server VM kopiëren en vervolgens installeert u SQL Server op de VM. U moet voor licentieverlening redenen, [Mobiliteit via Software Assurance op Azure licentie](https://azure.microsoft.com/pricing/license-mobility/)hebben.

5. **Hebt u de SQL-kosten van een VM betalen als het alleen wordt gebruikt voor stand-by/failover?**

    Als u de SQL VM door de galerie maakt, moet u een afzonderlijke licentie hebt voor de stand-by-VM met SQL en de prijs is hetzelfde. Als u SQL Server handmatig op een virtuele machine met [Licentie mobiliteit installeert](https://azure.microsoft.com/pricing/license-mobility/), hebt u de optie om een gratis passieve SQL-exemplaar voor failover. Controleer de beperkingen en vereisten.

6. **Hoe worden updates en servicepacks toepassen op een SQL Server-VM?**

    Virtuele machines u zeggenschap geven over de hostcomputer, inclusief wanneer en hoe u updates hebt toegepast. Voor het besturingssysteem, kunt u windows-updates handmatig toepassen of kunt u een planning service met de naam [Automatisch herstellen](virtual-machines-windows-classic-sql-automated-patching.md)inschakelen. Geautomatiseerde patch installeert de updates die zijn gemarkeerd als belangrijk, met inbegrip van SQL Server-updates in die categorie. Andere optionele updates voor SQL Server moeten handmatig worden geïnstalleerd.

7. **Is het mogelijk niet weergegeven in de galerie met virtuele machine (voor bijvoorbeeld Windows 2008 R2 + SQL Server 2012) configuraties instellen?**

    Nr. Voor virtuele machine galerie plaatjes met SQL Server, moet u een van de beschikbare afbeeldingen.

9. **Hoe installeer ik extra SQL-gegevens op mijn Azure VM**

    Download en installeer de hulpprogramma's voor SQL-gegevens vanuit [Microsoft SQL Server Data Tools - Business Intelligence voor Visual Studio 2013 ](https://www.microsoft.com/en-us/download/details.aspx?id=42313).

## <a name="resources"></a>Bronnen

Bekijk de video [Azure VM is het beste platform voor 2016 van SQL Server](https://channel9.msdn.com/Events/DataDriven/SQLServer2016/Azure-VM-is-the-best-platform-for-SQL-Server-2016)voor een overzicht van SQL Server op Azure virtuele Machines. Ook krijgt u een goede inleiding in het onderwerp, de [SQL Server op overzicht Azure virtuele Machines](virtual-machines-windows-sql-server-iaas-overview.md).

Andere bronnen zijn:

- [Inrichten van een SQL Server virtuele machine in de Portal Azure](virtual-machines-windows-portal-sql-server-provision.md)
- [Een Database migreren naar SQL Server op een Azure VM](virtual-machines-windows-migrate-sql.md)
- [Hoge beschikbaarheid en noodherstel voor SQL Server in Azure virtuele Machines](virtual-machines-windows-sql-high-availability-dr.md)
- [De beste prestaties voor SQL Server in Azure virtuele Machines](virtual-machines-windows-sql-performance.md)
- [Patronen van toepassing en van ontwikkelingsstrategieën voor SQL Server in Azure virtuele Machines](virtual-machines-windows-sql-server-app-patterns-dev-strategies.md)
