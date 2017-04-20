<properties
    pageTitle="SQL Server Agent-extensie voor SQL Server VMs (klassiek) | Microsoft Azure"
    description="In dit onderwerp wordt beschreven hoe u voor het beheren van de SQL Server agent-extensie die specifieke SQL Server-beheertaken worden geautomatiseerd. Deze omvatten automatische back-up automatisch patches en Azure sleutel kluis integratie. In dit onderwerp wordt de implementatie van de klassieke modus gebruikt."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="rothja"
    manager="jhubbard"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="10/27/2016"
    ms.author="jroth"/>

# <a name="sql-server-agent-extension-for-sql-server-vms-classic"></a>SQL Server Agent-extensie voor SQL Server VMs (klassiek)

> [AZURE.SELECTOR]
- [Resource Manager](virtual-machines-windows-sql-server-agent-extension.md)
- [Klassiek](virtual-machines-windows-classic-sql-server-agent-extension.md)

De extensie in SQL Server IaaS Agent (SQLIaaSAgent) wordt uitgevoerd op Azure virtuele machines om beheertaken te automatiseren. Dit onderwerp bevat een overzicht van de services die worden ondersteund door de extensie als-instructies voor de installatie, status en verwijderen.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Zie [SQL Server Agent-extensie voor SQL Server VMs Resource Manager](virtual-machines-windows-sql-server-agent-extension.md)de Resource Manager versie van dit artikel.

## <a name="supported-services"></a>Ondersteunde services

De SQL Server IaaS agentextensie ondersteunt de volgende beheertaken:

| De beheerfunctie | Beschrijving |
|---------------------|-------------------------------|
| **Geautomatiseerde back-ups van SQL** | Automatiseert het plannen van back-ups voor alle databases voor de standaardinstantie van SQL Server in de VM. Zie [Automatische back-up van SQL Server in Azure virtuele Machines (klassiek)](virtual-machines-windows-classic-sql-automated-backup.md)voor meer informatie.|
| **SQL automatisch herstellen** | Hiermee configureert u een onderhoudsvenster gedurende welke updates voor uw VM plaatsvinden kunnen, zodat u dat updates tijdens piekuren voor uw werkbelasting voorkomen kunt. Zie [automatisch patches voor SQL Server in Microsoft Azure virtuele Machines (klassiek)](virtual-machines-windows-classic-sql-automated-patching.md)voor meer informatie.|
| **Integratie van de Azure sleutel kluis** | Hiermee kunt u het automatisch installeren en configureren van Azure sleutel kluis op uw SQL Server-VM. Zie [Azure sleutel kluis integratie configureren voor SQL Server op Azure VMs (klassiek)](virtual-machines-windows-classic-ps-sql-keyvault.md)voor meer informatie.|

## <a name="prerequisites"></a>Vereisten

Vereisten voor het gebruik van de SQL Server IaaS agentextensie op uw VM:

### <a name="operating-system"></a>Besturingssysteem:

- Windows Server 2012
- Windows Server 2012 R2

### <a name="sql-server-versions"></a>Versies van SQL Server:

- SQL Server 2012
- SQL Server 2014
- SQL Server 2016

### <a name="azure-powershell"></a>Azure PowerShell:

[Downloaden en de meest recente Azure PowerShell-opdrachten configureren](../powershell-install-configure.md).

Start Windows PowerShell en verbinding maken met uw Azure-abonnement met de opdracht **Add-AzureAccount** .

    Add-AzureAccount

Als er meerdere abonnementen, **Selecteer AzureSubscription** gebruiken te selecteren van het abonnement met het doel-klassieke VM.

    Select-AzureSubscription -SubscriptionName <subscriptionname>

Op dit punt kunt u een lijst met de klassieke virtuele machines en de namen van de bijbehorende service met de opdracht **Get-AzureVM** .

    Get-AzureVM

## <a name="installation"></a>Installatie

Voor klassieke VMs, moet u PowerShell gebruiken de extensie SQL Server IaaS Agent installeren en configureren van de gekoppelde services. De PowerShell-cmdlet **Set-AzureVMSqlServerExtension** gebruikt de extensie te installeren. De volgende opdracht wordt bijvoorbeeld de extensie installeert op een Windows Server VM (klassiek) en namen van "SQLIaaSExtension".

    Get-AzureVM -ServiceName <vmservicename> -Name <vmname> | Set-AzureVMSqlServerExtension -ReferenceName "SQLIaasExtension" -Version "1.2" | Update-AzureVM

Als u hebt bijgewerkt naar de nieuwste versie van de SQL-extensie IaaS Agent, moet u uw virtuele machine opnieuw na het bijwerken van de uitbreiding.

>[AZURE.NOTE] Klassieke virtuele machines beschikt niet over een optie voor het installeren en configureren van de SQL Agent-extensie voor IaaS via de portal.

## <a name="status"></a>Status

Agent status weergeven in de Portal Azure is een manier om te controleren of de extensie is geïnstalleerd. **Alle instellingen** te selecteren in de blade virtuele machine en klik op **extensies**. Hier ziet u de **SQLIaaSAgent** -extensie weergegeven.

![SQL Server Agent IaaS extensie in Azure Portal](./media/virtual-machines-windows-classic-sql-server-agent-extension/azure-sql-server-iaas-agent-portal.png)

U kunt ook de cmdlet **Get-AzureVMSqlServerExtension** Azure Powershell gebruiken.

    Get-AzureVM –ServiceName "service" –Name "vmname" | Get-AzureVMSqlServerExtension

## <a name="removal"></a>Verwijderen   

U kunt de extensie in de Portal Azure verwijderen door te klikken op de drie puntjes op het blad **uitbreidingen** van de eigenschappen van de virtuele machine. Klik vervolgens op **verwijderen**.

![De SQL Server Agent IaaS uitbreiding in Azure Portal verwijderen](./media/virtual-machines-windows-classic-sql-server-agent-extension/azure-sql-server-iaas-agent-uninstall.png)

U kunt ook de Powershell-cmdlet **Verwijderen AzureVMSqlServerExtension** .

    Get-AzureVM –ServiceName "service" –Name "vmname" | Remove-AzureVMSqlServerExtension | Update-AzureVM

## <a name="next-steps"></a>Volgende stappen

Beginnen met een van de services die worden ondersteund door de extensie. Zie voor meer informatie de onderwerpen waarnaar wordt verwezen in de sectie [services ondersteund](#supported-services) .

Zie [SQL Server op de virtuele Machines van Azure-overzicht](virtual-machines-windows-sql-server-iaas-overview.md)voor meer informatie over het uitvoeren van SQL Server op Azure virtuele Machines.
