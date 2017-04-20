<properties
    pageTitle="Automatisch patches voor SQL Server VMs (klassiek) | Microsoft Azure"
    description="Wordt de functie automatisch patches voor SQL Server virtuele Machines die worden uitgevoerd in de implementatie van de klassieke modus met Azure uitgelegd."
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
    ms.date="09/26/2016"
    ms.author="jroth" />

# <a name="automated-patching-for-sql-server-in-azure-virtual-machines-classic"></a>Automatisch patches voor SQL Server in Azure virtuele Machines (klassiek)

> [AZURE.SELECTOR]
- [Resource Manager](virtual-machines-windows-sql-automated-patching.md)
- [Klassiek](virtual-machines-windows-classic-sql-automated-patching.md)

Geautomatiseerde patch stelt een onderhoudsvenster voor een SQL Server met Azure Virtual Machine. Automatische Updates kunnen alleen worden geïnstalleerd tijdens dit onderhoudsvenster. Op deze manier systeemupdates en eventuele bijbehorende opnieuw bij de best mogelijke voor de database optreden voor SQL Server. Geautomatiseerde Patching is afhankelijk van de [SQL Server IaaS agentextensie](virtual-machines-windows-classic-sql-server-agent-extension.md).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]De Resource Manager versie van dit artikel, Zie [Automatisch patches voor SQL Server in Azure virtuele Machines Resource Manager](virtual-machines-windows-sql-automated-patching.md).

## <a name="prerequisites"></a>Vereisten

Houd rekening met de volgende vereisten voor het gebruik van geautomatiseerde patches:

**Besturingssysteem**:

- Windows Server 2012
- Windows Server 2012 R2

**SQL Server-versie**:

- SQL Server 2012
- SQL Server 2014
- SQL Server 2016

**Azure PowerShell**:

- [Installeer de meest recente Azure PowerShell-opdrachten](../powershell-install-configure.md).

**IaaS extensie van SQL Server**:

- [De extensie IaaS van SQL Server te installeren](virtual-machines-windows-classic-sql-server-agent-extension.md).

## <a name="settings"></a>Instellingen

De volgende tabel beschrijft de opties die kunnen worden geconfigureerd voor het automatisch herstellen. U moet voor klassieke VMs, PowerShell gebruiken om deze instellingen te configureren.

|Instelling|Mogelijke waarden|Beschrijving|
|---|---|---|
|**Automatisch herstellen**|Uitschakelen (uitgeschakeld)|Hiermee schakelt u of automatisch patches voor een Azure virtuele machine.|
|**Onderhoudsplanning**|Elke dag, maandag, dinsdag, woensdag, donderdag, vrijdag, zaterdag, zondag|De planning voor het downloaden en installeren van Windows, SQL Server en Microsoft updates voor uw virtuele machine.|
|**Onderhoud start uur**|0-24|De lokale tijd voor het bijwerken van de virtuele machine start.|
|**Onderhoud venster duur**|30-180|Het aantal minuten mag het downloaden en installeren van updates.|
|**Patch-categorie**|Belangrijk|De categorie van de updates te downloaden en te installeren.|

## <a name="configuration-with-powershell"></a>Configuratie met PowerShell

In het volgende voorbeeld wordt de PowerShell patches automatisch te configureren op een bestaande SQL Server VM gebruikt. De opdracht **Nieuw AzureVMSqlServerAutoPatchingConfig** zorgt ervoor dat een nieuw onderhoudsvenster voor automatische updates.

    $aps = New-AzureVMSqlServerAutoPatchingConfig -Enable -DayOfWeek "Thursday" -MaintenanceWindowStartingHour 11 -MaintenanceWindowDuration 120  -PatchCategory "Important"

    Get-AzureVM -ServiceName <vmservicename> -Name <vmname> | Set-AzureVMSqlServerExtension -AutoPatchingSettings $aps | Update-AzureVM

Op basis van het volgende voorbeeld wordt beschrijft de volgende tabel de praktische gevolgen voor het doel Azure VM:

|Parameter|Effect|
|---|---|
|**Dag van de week**|Patches geïnstalleerd elke donderdag.|
|**MaintenanceWindowStartingHour**|Begin updates om 11:00 uur.|
|**MaintenanceWindowsDuration**|Patches moeten worden geïnstalleerd binnen 120 minuten. Gebaseerd op de begintijd, moeten zij voltooien door 13:00 uur.|
|**PatchCategory**|De enige mogelijke instelling voor deze parameter is 'Belangrijk'.|

Het kan enige tijd duren om te installeren en configureren van de IaaS SQL Server Agent.

Uitvoeren om automatisch herstellen uitschakelen, hetzelfde script zonder de - parameter inschakelen voor de nieuw-AzureVMSqlServerAutoPatchingConfig. Net als bij de installatie, kan het automatisch herstellen uitschakelen enkele minuten duren.

## <a name="next-steps"></a>Volgende stappen

Zie voor informatie over andere beschikbare automatiseringstaken, [IaaS agentextensie voor SQL Server](virtual-machines-windows-classic-sql-server-agent-extension.md).

Zie [SQL Server op de virtuele Machines van Azure-overzicht](virtual-machines-windows-sql-server-iaas-overview.md)voor meer informatie over het uitvoeren van SQL Server op Azure VMs.
