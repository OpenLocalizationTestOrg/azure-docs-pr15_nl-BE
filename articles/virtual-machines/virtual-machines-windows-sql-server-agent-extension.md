<properties
    pageTitle="SQL Server Agent-extensie voor SQL Server VMs (Resource Manager) | Microsoft Azure"
    description="In dit onderwerp wordt beschreven hoe u voor het beheren van de SQL Server agent-extensie die specifieke SQL Server-beheertaken worden geautomatiseerd. Deze omvatten automatische back-up automatisch patches en Azure sleutel kluis integratie. In dit onderwerp maakt gebruik van de Resource Manager distributie-modus."
    services="virtual-machines-windows"
    documentationCenter=""
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
    ms.date="10/27/2016"
    ms.author="jroth"/>

# <a name="sql-server-agent-extension-for-sql-server-vms-resource-manager"></a>SQL Server Agent-extensie voor SQL Server VMs (Resource Manager)

> [AZURE.SELECTOR]
- [Resource Manager](virtual-machines-windows-sql-server-agent-extension.md)
- [Klassiek](virtual-machines-windows-classic-sql-server-agent-extension.md)

De extensie in SQL Server IaaS Agent (SQLIaaSExtension) wordt uitgevoerd op Azure virtuele machines om beheertaken te automatiseren. Dit onderwerp bevat een overzicht van de services die worden ondersteund door de extensie als-instructies voor de installatie, status en verwijderen.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]klassieke implementatiemodel. Zie [SQL Server Agent-extensie voor SQL Server VMs Classic](virtual-machines-windows-classic-sql-server-agent-extension.md)de light versie van dit artikel.

## <a name="supported-services"></a>Ondersteunde services

De SQL Server IaaS agentextensie ondersteunt de volgende beheertaken:

| De beheerfunctie | Beschrijving |
|---------------------|-------------------------------|
| **Geautomatiseerde back-ups van SQL** | Automatiseert het plannen van back-ups voor alle databases voor de standaardinstantie van SQL Server in de VM. Zie [Automatische back-up van SQL Server in Azure virtuele Machines (Resource Manager)](virtual-machines-windows-sql-automated-backup.md)voor meer informatie.|
| **SQL automatisch herstellen** | Hiermee configureert u een onderhoudsvenster gedurende welke updates voor uw VM plaatsvinden kunnen, zodat u dat updates tijdens piekuren voor uw werkbelasting voorkomen kunt. Zie voor meer informatie [automatisch patches voor SQL Server in Microsoft Azure virtuele Machines (Resource Manager)](virtual-machines-windows-sql-automated-patching.md).|
| **Integratie van de Azure sleutel kluis** | Hiermee kunt u het automatisch installeren en configureren van Azure sleutel kluis op uw SQL Server-VM. Voor meer informatie Zie [Azure sleutel kluis integratie configureren voor SQL Server op Azure VMs (Resource Manager)](virtual-machines-windows-ps-sql-keyvault.md).|

## <a name="prerequisites"></a>Vereisten

Vereisten voor het gebruik van de SQL Server IaaS agentextensie op uw VM:

**Besturingssysteem**:

- Windows Server 2012
- Windows Server 2012 R2

**Versies van SQL Server**:

- SQL Server 2012
- SQL Server 2014
- SQL Server 2016

**Azure PowerShell**:

- [Downloaden en de meest recente Azure PowerShell-opdrachten configureren](../powershell-install-configure.md)

## <a name="installation"></a>Installatie

De SQL Server IaaS agentextensie wordt automatisch geïnstalleerd wanneer u een van de SQL Server virtuele machine-afbeeldingen creëren.

Als u een alleen-besturingssysteem Windows Server virtuele machine hebt gemaakt, kunt u de extensie handmatig installeren via de PowerShell-cmdlet **Set-AzureVMSqlServerExtension** . De volgende opdracht wordt bijvoorbeeld de extensie installeert op een VM OS alleen Windows Server en namen van "SQLIaaSExtension".

    Set-AzureRmVMSqlServerExtension -ResourceGroupName "resourcegroupname" -VMName "vmname" -Name "SQLIaasExtension" -Version "1.2"

Als u hebt bijgewerkt naar de nieuwste versie van de SQL-extensie IaaS Agent, moet u uw virtuele machine opnieuw na het bijwerken van de uitbreiding.

>[AZURE.NOTE] Als u de extensie SQL Server IaaS Agent handmatig op een Windows Server-VM installeert, moet u gebruiken en beheren van de functies met PowerShell-opdrachten. De portal-interface is alleen beschikbaar voor SQL Server-afbeeldingen.

## <a name="status"></a>Status

Agent status weergeven in de Portal Azure is een manier om te controleren of de extensie is geïnstalleerd. **Alle instellingen** te selecteren in de blade virtuele machine en klik op **extensies**. Hier ziet u de **SQLIaaSExtension** -extensie weergegeven.

![SQL Server Agent IaaS extensie in Azure Portal](./media/virtual-machines-windows-sql-server-agent-extension/azure-rm-sql-server-iaas-agent-portal.png)

U kunt ook de cmdlet **Get-AzureVMSqlServerExtension** Azure Powershell gebruiken.

    Get-AzureRmVMSqlServerExtension -VMName "vmname" -ResourceGroupName "resourcegroupname"

De vorige opdracht bevestigt de agent wordt geïnstalleerd en biedt algemene informatie over. U kunt ook specifieke statusinformatie over geautomatiseerde back-up en Patching met de volgende opdrachten opvragen.

    $sqlext = Get-AzureRmVMSqlServerExtension -VMName "vmname" -ResourceGroupName "resourcegroupname"
    $sqlext.AutoPatchingSettings
    $sqlext.AutoBackupSettings

## <a name="removal"></a>Verwijderen   

U kunt de extensie in de Portal Azure verwijderen door te klikken op de drie puntjes op het blad **uitbreidingen** van de eigenschappen van de virtuele machine. Klik vervolgens op **verwijderen**.

![De SQL Server Agent IaaS uitbreiding in Azure Portal verwijderen](./media/virtual-machines-windows-sql-server-agent-extension/azure-rm-sql-server-iaas-agent-uninstall.png)

U kunt ook de Powershell-cmdlet **Verwijderen AzureRmVMSqlServerExtension** .

    Remove-AzureRmVMSqlServerExtension -ResourceGroupName "resourcegroupname" -VMName "vmname" -Name "SQLIaasExtension"

## <a name="next-steps"></a>Volgende stappen

Beginnen met een van de services die worden ondersteund door de extensie. Zie voor meer informatie de onderwerpen waarnaar wordt verwezen in de sectie [services ondersteund](#supported-services) .

Zie [SQL Server op de virtuele Machines van Azure-overzicht](virtual-machines-windows-sql-server-iaas-overview.md)voor meer informatie over het uitvoeren van SQL Server op Azure virtuele Machines.
