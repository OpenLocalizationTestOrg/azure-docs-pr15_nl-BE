<properties
    pageTitle="Overzicht van SQL Server op Azure virtuele Machines | Microsoft Azure"
    description="Meer informatie over het uitvoeren van volledige edities van SQL Server op Azure Virtual machines. Directe koppelingen naar alle afbeeldingen van SQL Server VM en verwante inhoud ophalen."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="rothja"
    manager="jhubbard"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="10/19/2016"
    ms.author="jroth"/>

# <a name="overview-of-sql-server-on-azure-virtual-machines"></a>Overzicht van SQL Server op Azure virtuele Machines

Dit onderwerp beschrijft de opties voor het uitvoeren van SQL Server op Azure virtuele machines (VMs), en de [koppelingen naar portal-afbeeldingen](#option-1-create-a-sql-vm-with-per-minute-licensing) en een overzicht van [veelvoorkomende taken](#manage-your-sql-vm).

>[AZURE.NOTE] Als u al vertrouwd met SQL Server bent en u wilt zien hoe u een SQL Server-VM implementeren, Zie [een SQL Server virtuele machine in de portal Azure bepaling](virtual-machines-windows-portal-sql-server-provision.md).

## <a name="overview"></a>Overzicht
Als u een databasebeheerder of ontwikkelaar bent, kunt Azure VMs u uw SQL Server-werklasten op gebouwen en toepassingen naar de Cloud te verplaatsen. De volgende video biedt een technisch overzicht van SQL Server Azure VMs.

> [AZURE.VIDEO data-driven-sql-server-2016-azure-vm-is-the-best-platform-for-sql-server-2016]

De video heeft betrekking op de volgende gebieden:

|Tijd|Gebied|
|---|---|
| 00:21 | Wat zijn Azure VMs? |
| 01:45 | Beveiliging |
| 02:50 | Connectiviteit |
| 03:30 | Opslag, betrouwbaarheid en prestaties |
| 05:20 | VM-formaten |
| 05:54 | Hoge beschikbaarheid en SLA |
| 07:30 | Ondersteuning voor configuratie |
| 08:00 | Monitoring |
| 08:32 | Demo: Maak een VM met SQL Server 2016 |

>[AZURE.NOTE] De video is gericht op SQL Server 2016, maar Azure VM afbeeldingen biedt voor veel versies van SQL Server, met inbegrip van 2008, 2012, 2014 en 2016. 

## <a name="scenarios"></a>Scenario 's
Er zijn vele redenen die u kiezen kunt voor het hosten van uw gegevens in Azure. Als uw toepassing wordt verplaatst naar Azure, verbetert de prestaties als u wilt de gegevens ook verplaatsen. Maar er zijn nog andere voordelen. U hebt automatisch toegang tot meerdere datacenters voor een globale aanwezigheid en disaster recovery. De gegevens zijn ook zeer beveiligde en duurzaam.

SQL Server wordt uitgevoerd op Azure VMs is een optie voor het opslaan van relationele gegevens in Azure. Het is een goede keuze voor verschillende scenario's. U wilt de Azure VM zo mogelijk naar een SQL Server-computer op de locatie configureren. Of u kunt extra toepassingen en services worden uitgevoerd op de databaseserver. Er zijn twee belangrijke bronnen waarmee u nog meer scenario's en overwegingen bedenken:

 - [SQL Server op Azure virtuele machines](https://azure.microsoft.com/services/virtual-machines/sql-server/) biedt een overzicht van de beste scenario's voor het gebruik van SQL Server op Azure VMs. 
 - [Een wolk van SQL Server-optie kiezen: Azure SQL (PaaS)-Database of SQL Server op Azure VMs (IaaS)](../sql-database/sql-database-paas-vs-sql-server-iaas.md) bevat een gedetailleerde vergelijking tussen SQL-Database en SQL Server op een VM.

## <a name="create-a-new-sql-vm"></a>Maak een nieuwe SQL VM
De volgende secties vindt rechtstreekse koppelingen naar de Azure portal voor de SQL Server virtuele machine-afbeeldingen. Afhankelijk van de afbeelding die u selecteert, kunt u ofwel betalen voor SQL Server-licentieverlening kosten per minuut of u kunt doen om uw eigen licentie (BYOL).

Stapsgewijze instructies voor dit proces in de zelfstudie, [een SQL Server virtuele machine in de portal Azure bepaling](virtual-machines-windows-portal-sql-server-provision.md)vinden. Bekijk tevens de [Aanbevolen procedures voor SQL Server VMs prestaties](virtual-machines-windows-sql-performance.md), waarin wordt uitgelegd hoe u de grootte van de desbetreffende computer en andere functies die beschikbaar zijn tijdens het inrichten te selecteren.

## <a name="option-1-create-a-sql-vm-with-per-minute-licensing"></a>Optie 1: Maak een VM SQL met licenties per minuut
De volgende tabel bevat een matrix van beschikbare SQL Server-afbeeldingen in de galerie met virtuele machine. Klik op een koppeling om te beginnen met het maken van een nieuwe SQL VM met de opgegeven versie, editie en besturingssysteem.

|Versie|Besturingssysteem|Edition|
|---|---|---|
|**SQL-2016**|Windows Server 2012 R2|[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2016RTMEnterpriseWindowsServer2012R2), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2016RTMStandardWindowsServer2012R2), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2016RTMWebWindowsServer2012R2), [Dev](https://portal.azure.com/#create/Microsoft.SQLServer2016RTMDeveloperWindowsServer2012R2), [Express](https://portal.azure.com/#create/Microsoft.SQLServer2016RTMExpressWindowsServer2012R2)|
|**SQL 2014 SP1**|Windows Server 2012 R2|[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2014SP1EnterpriseWindowsServer2012R2), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2014SP1StandardWindowsServer2012R2), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2014SP1WebWindowsServer2012R2), [Express](https://portal.azure.com/#create/Microsoft.SQLServer2014SP1ExpressWindowsServer2012R2)|
|**SQL 2014**|Windows Server 2012 R2|[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2014EnterpriseWindowsServer2012R2), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2014StandardWindowsServer2012R2), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2014WebWindowsServer2012R2)|
|**SQL 2012 SP3**|Windows Server 2012 R2|[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2012SP3EnterpriseWindowsServer2012R2), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2012SP3StandardWindowsServer2012R2), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2012SP3WebWindowsServer2012R2), [Express](https://portal.azure.com/#create/Microsoft.SQLServer2012SP3ExpressWindowsServer2012R2)|
|**SQL 2012 SP2**|Windows Server 2012 R2|[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2012SP2EnterpriseWindowsServer2012R2), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2012SP2StandardWindowsServer2012R2), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2012SP2WebWindowsServer2012R2)|
|**SQL 2012 SP2**|Windows Server 2012|[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2012SP2EnterpriseWindowsServer2012), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2012SP2StandardWindowsServer2012), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2012SP2WebWindowsServer2012), [Express](https://portal.azure.com/#create/Microsoft.SQLServer2012SP2ExpressWindowsServer2012)|
|**SQL 2008 R2 SP3**|Windows Server 2008 R2|[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2008R2SP3EnterpriseWindowsServer2008R2), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2008R2SP3StandardWindowsServer2008R2), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2008R2SP3WebWindowsServer2008R2)|
|**SQL 2008 R2 SP3**|Windows Server 2012|[Express](https://portal.azure.com/#create/Microsoft.SQLServer2008R2SP3ExpressWindowsServer2012)|

## <a name="option-2-create-a-sql-vm-with-an-existing-license"></a>Optie 2: Een VM SQL maken met een bestaande licentie
U kunt ook uw eigen licentie (BYOL) brengen. In dit scenario betaalt u alleen voor de VM zonder eventuele extra kosten voor SQL Server-licentieverlening. Als u uw eigen licentie, gebruiken de matrix van SQL Server-versies, edities en besturingssystemen hieronder. Deze afbeelding namen worden in de portal voorafgegaan door **{BYOL}**.

|Versie|Besturingssysteem|Edition|
|---|---|---|
|**SQL Server 2016**|Windows Server 2012 R2|[Onderneming BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2016RTMStandardWindowsServer2012R2), [standaard BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2016RTMStandardWindowsServer2012R2)|
|**SQL Server 2014 SP1**|Windows Server 2012 R2|[Onderneming BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2014SP1EnterpriseWindowsServer2012R2), [standaard BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2014SP1StandardWindowsServer2012R2)|
|**SQL Server 2012 SP2**|Windows Server 2012 R2|[Onderneming BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2012SP3EnterpriseWindowsServer2012R2), [standaard BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2012SP3StandardWindowsServer2012R2)|

> [AZURE.IMPORTANT] BYOL VM-images, u moet hebben en het Enterprise Agreement met [Licentie mobiliteit via Software Assurance op Azure](https://azure.microsoft.com/pricing/license-mobility/). Ook moet u een geldige licentie voor de versie/editie van SQL Server die u wilt gebruiken. U moet [de nodige BYOL informatie naar Microsoft](http://d36cz9buwru1tt.cloudfront.net/License_Mobility_Customer_Verification_Guide.pdf) binnen **10** dagen na het inrichten van uw VM.

## <a name="manage-your-sql-vm"></a>Beheer uw VM met SQL
Er zijn verschillende taken die optioneel beheer na het inrichten van uw SQL Server-VM. In veel aspecten die u kunt configureren en beheren van SQL Server, precies zoals u zou een exemplaar van de SQL Server op locatie beheren. Echter, sommige taken zijn specifiek voor Azure. De volgende gedeelten van deze gebieden met koppelingen naar meer informatie.

### <a name="connect-to-the-vm"></a>Verbinding maken met de VM
Een van de meest elementaire stappen voor management is verbinding maken met de SQL Server-VM via hulpprogramma's, zoals SQL Server Management Studio (SSMS). Zie voor meer informatie over verbinding maken met de nieuwe SQL Server-VM, [verbinding maken met een virtuele Machine op Azure in SQL Server](virtual-machines-windows-sql-connect.md).

### <a name="migrate-your-data"></a>Uw gegevens migreren
Als u een bestaande database hebt, zult u die naar de nieuw ingerichte SQL VM. Zie voor een lijst met opties voor de migratie en richtlijnen, [een Database aan SQL Server op een Azure VM migratie](virtual-machines-windows-migrate-sql.md).

### <a name="configure-high-availability"></a>Configureren, hoge beschikbaarheid
Als u maximale beschikbaarheid vereist, kunt u SQL Server beschikbaarheidsgroepen configureren. Hierbij worden meerdere Azure VMs in een virtueel netwerk. De Azure portal is een sjabloon met deze configuratie voor u ingesteld. Zie [een groep AlwaysOn beschikbaarheid in Azure Resource Manager virtuele machines configureren](virtual-machines-windows-portal-sql-alwayson-availability-groups.md)voor meer informatie. Als u handmatig configureren uw groep beschikbaarheid en bijbehorende listener wilt, Zie [AlwaysOn beschikbaarheidsgroepen configureren in Azure VM](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md).

Zie voor andere overwegingen hoge beschikbaarheid [voor hoge beschikbaarheid en noodherstel voor SQL Server in Azure virtuele Machines](virtual-machines-windows-sql-high-availability-dr.md).

### <a name="back-up-your-data"></a>Back-up van uw gegevens
Azure VMs kunnen profiteren van [Automatische back-up](virtual-machines-windows-sql-automated-backup.md), die het maakt regelmatig back-ups van de database naar een blobopslag. U kunt deze techniek ook handmatig gebruiken. Zie [Opslag van Azure gebruiken voor het maken en terugzetten van back-up van SQL Server](virtual-machines-windows-use-storage-sql-server-backup-restore.md)voor meer informatie. Zie voor een overzicht van alle opties voor back-up en terugzetten van [back-up en herstel voor SQL Server in Azure virtuele Machines](virtual-machines-windows-sql-backup-recovery.md).

### <a name="automate-updates"></a>Updates automatiseren
Azure VMs kunnen [Automatisch patches](virtual-machines-windows-sql-automated-patching.md) voor het plannen van een onderhoudsvenster voor het installeren van belangrijke windows en SQL Server automatisch bijgewerkt.

### <a name="customer-experience-improvement-program-ceip"></a>Gebruikerservaring (CEIP)
Het programma (Kwaliteitsverbetering) is standaard ingeschakeld. Dit verzendt regelmatig rapporten naar Microsoft SQL Server verbeteren. Er is geen beheertaak met CEIP vereist, tenzij u uitschakelen wilt na het inrichten. U kunt aanpassen of uitschakelen van het programma voor Kwaliteitsverbetering door verbinding te maken met de VM met extern bureaublad. Voer het hulpprogramma **SQL Server-fout- en gebruiksrapportage** . Volg de instructies voor het melden van uitschakelen. 

Zie voor meer informatie de sectie programma voor Kwaliteitsverbetering van het onderwerp [Licentievoorwaarden accepteren](https://msdn.microsoft.com/library/ms143343.aspx) . 

## <a name="next-steps"></a>Volgende stappen
[Het pad leren verkennen](https://azure.microsoft.com/documentation/learning-paths/sql-azure-vm/) voor SQL Server op Azure virtuele machines.

Vraag? Kijk eerst de [SQL Server op veelgestelde vragen over het Azure virtuele Machines](virtual-machines-windows-sql-server-iaas-faq.md). Maar ook uw vragen of opmerkingen toevoegen aan de onderkant van alle onderwerpen die VM SQL interactief werken met Microsoft en de Gemeenschap.
