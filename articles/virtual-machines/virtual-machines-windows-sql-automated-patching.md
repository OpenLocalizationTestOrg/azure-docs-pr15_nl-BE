<properties
    pageTitle="Automatisch patches voor SQL Server VMs (Resource Manager) | Microsoft Azure"
    description="Wordt de functie automatisch patches voor SQL Server virtuele Machines die worden uitgevoerd in Azure met behulp van bronbeheer uitgelegd."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="rothja"
    manager="jhubbard"
    editor=""
    tags="azure-resource-manager"/>
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="08/19/2016"
    ms.author="jroth" />

# <a name="automated-patching-for-sql-server-in-azure-virtual-machines-resource-manager"></a>Automatisch patches voor SQL Server in Azure virtuele Machines (Resource Manager)

> [AZURE.SELECTOR]
- [Resource Manager](virtual-machines-windows-sql-automated-patching.md)
- [Klassiek](virtual-machines-windows-classic-sql-automated-patching.md)

Geautomatiseerde patch stelt een onderhoudsvenster voor een SQL Server met Azure Virtual Machine. Automatische Updates kunnen alleen worden geïnstalleerd tijdens dit onderhoudsvenster. Voor SQL Server zorgt deze rescriction systeemupdates en eventuele bijbehorende opnieuw bij de best mogelijke voor de database optreden. Geautomatiseerde Patching is afhankelijk van de [SQL Server IaaS agentextensie](virtual-machines-windows-sql-server-agent-extension.md).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]klassieke implementatiemodel. De klassieke versie van dit artikel, Zie [Automatisch patches voor SQL Server in Azure klassieke voor virtuele Machines](virtual-machines-windows-classic-sql-automated-patching.md).

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

- [Installeer de meest recente Azure PowerShell-opdrachten](../powershell-install-configure.md) als u wilt configureren automatisch patches met PowerShell.

>[AZURE.NOTE] Geautomatiseerde Patching is afhankelijk van de SQL Server IaaS Agent. Deze extensie standaard de huidige SQL-virtuele machine afbeeldingen toegevoegd. Zie [SQL Server IaaS Agent Extensions](virtual-machines-windows-sql-server-agent-extension.md)voor meer informatie.

## <a name="settings"></a>Instellingen

De volgende tabel beschrijft de opties die kunnen worden geconfigureerd voor het automatisch herstellen. De werkelijke configuratiestappen zijn afhankelijk van of u de Azure portal of Azure Windows PowerShell-opdrachten gebruiken.

|Instelling|Mogelijke waarden|Beschrijving|
|---|---|---|
|**Automatisch herstellen**|Uitschakelen (uitgeschakeld)|Hiermee schakelt u of automatisch patches voor een Azure virtuele machine.|
|**Onderhoudsplanning**|Elke dag, maandag, dinsdag, woensdag, donderdag, vrijdag, zaterdag, zondag|De planning voor het downloaden en installeren van Windows, SQL Server en Microsoft updates voor uw virtuele machine.|
|**Onderhoud start uur**|0-24|De lokale tijd voor het bijwerken van de virtuele machine start.|
|**Onderhoud venster duur**|30-180|Het aantal minuten mag het downloaden en installeren van updates.|
|**Patch-categorie**|Belangrijk|De categorie van de updates te downloaden en te installeren.|

## <a name="configuration-in-the-portal"></a>Configuratie in de Portal
U kunt de Azure portal patches automatisch te configureren tijdens het inrichten of voor bestaande VMs.

### <a name="new-vms"></a>Nieuwe VMs
Via de portal Azure automatisch herstellen wanneer u een nieuwe virtuele Machine voor SQL Server in het implementatiemodel Resource Manager configureren.

Selecteer **automatisch patches**in de **instellingen van de SQL Server** -blade. De volgende Azure portal screenshot toont de blade **SQL automatisch herstellen** .

![SQL in Azure portal automatisch herstellen](./media/virtual-machines-windows-sql-automated-patching/azure-sql-arm-patching.png)

Zie het onderwerp volledig op het [inrichten van een SQL Server virtuele machine in Azure](virtual-machines-windows-portal-sql-server-provision.md)voor context.

### <a name="existing-vms"></a>Bestaande VMs
Selecteer uw SQL Server virtuele machine voor bestaande SQL Server virtuele machines. Selecteer de sectie **SQL Server-configuratie** van de **Instellingen voor** blade.

![Automatisch herstellen van SQL voor bestaande VMs](./media/virtual-machines-windows-sql-automated-patching/azure-sql-rm-patching-existing-vms.png)

Klik op de knop **bewerken** in de sectie patches automatisch in de **configuratie van SQL Server** -blade.

![Configureer SQL automatisch patches voor bestaande VMs](./media/virtual-machines-windows-sql-automated-patching/azure-sql-rm-patching-configuration.png)

Wanneer u klaar bent, klikt u op de knop **OK** op de onderkant van de **configuratie van SQL Server** -blade uw wijzigingen op te slaan.

Als u automatisch patches voor de eerste keer inschakelen wilt, configureert Azure SQL Server IaaS Agent op de achtergrond. Gedurende deze periode de Azure portal worden niet altijd weergegeven patches automatisch is geconfigureerd. Wacht enkele minuten voor de agent moet worden geïnstalleerd of geconfigureerd. Daarna de Azure portal komt overeen met de nieuwe instellingen.

>[AZURE.NOTE] U kunt ook automatisch herstellen met behulp van een sjabloon. Zie [Azure quickstart sjabloon voor automatisch herstellen](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-sql-existing-autopatching-update)voor meer informatie.

## <a name="configuration-with-powershell"></a>Configuratie met PowerShell

Na het inrichten van uw VM SQL gebruik PowerShell patches automatisch configureren.

In het volgende voorbeeld wordt de PowerShell patches automatisch te configureren op een bestaande SQL Server VM gebruikt. De opdracht **AzureRM.Compute\New AzureVMSqlServerAutoPatchingConfig** configureren een nieuw onderhoudsvenster voor automatische updates.

    $vmname = "vmname"
    $resourcegroupname = "resourcegroupname"
    $aps = AzureRM.Compute\New-AzureVMSqlServerAutoPatchingConfig -Enable -DayOfWeek "Thursday" -MaintenanceWindowStartingHour 11 -MaintenanceWindowDuration 120  -PatchCategory "Important"

    Set-AzureRmVMSqlServerExtension -AutoPatchingSettings $aps -VMName $vmname -ResourceGroupName $resourcegroupname

Op basis van het volgende voorbeeld wordt beschrijft de volgende tabel de praktische gevolgen voor het doel Azure VM:

|Parameter|Effect|
|---|---|
|**Dag van de week**|Patches geïnstalleerd elke donderdag.|
|**MaintenanceWindowStartingHour**|Begin updates om 11:00 uur.|
|**MaintenanceWindowsDuration**|Patches moeten worden geïnstalleerd binnen 120 minuten. Gebaseerd op de begintijd, moeten zij voltooien door 13:00 uur.|
|**PatchCategory**|De enige mogelijke instelling voor deze parameter is **Belangrijk**.|

Het kan enige tijd duren om te installeren en configureren van de IaaS SQL Server Agent.

Uitvoeren om automatisch herstellen uitschakelen, hetzelfde script zonder de **-inschakelen** -parameter voor de **AzureRM.Compute\New AzureVMSqlServerAutoPatchingConfig**. Het ontbreken van de **-inschakelen** parameter geeft de opdracht voor het uitschakelen van de functie.

## <a name="next-steps"></a>Volgende stappen

Zie voor informatie over andere beschikbare automatiseringstaken, [IaaS agentextensie voor SQL Server](virtual-machines-windows-sql-server-agent-extension.md).

Zie [SQL Server op de virtuele Machines van Azure-overzicht](virtual-machines-windows-sql-server-iaas-overview.md)voor meer informatie over het uitvoeren van SQL Server op Azure VMs.
