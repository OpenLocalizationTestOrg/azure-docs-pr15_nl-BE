<properties
    pageTitle="Automatische back-up voor SQL Server virtuele Machines (Resource Manager) | Microsoft Azure"
    description="Wordt de functie voor automatische back-up beschreven voor SQL Server wordt uitgevoerd in Azure virtuele Machines met behulp van bronbeheer. "
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
    ms.date="07/14/2016"
    ms.author="jroth" />

# <a name="automated-backup-for-sql-server-in-azure-virtual-machines-resource-manager"></a>Automatische back-up voor SQL Server in Azure virtuele Machines (Resource Manager)

> [AZURE.SELECTOR]
- [Resource Manager](virtual-machines-windows-sql-automated-backup.md)
- [Klassiek](virtual-machines-windows-classic-sql-automated-backup.md)

Automatische back-up configureert automatisch [Back-ups beheerd met Microsoft Azure](https://msdn.microsoft.com/library/dn449496.aspx) voor alle bestaande en nieuwe databases op een Azure VM met SQL Server 2014 Standard of Enterprise. Hiermee kunt u regelmatig back-ups die gebruikmaken van duurzame Azure blob-opslag configureren. Automatische back-up is afhankelijk van de [SQL Server IaaS agentextensie](virtual-machines-windows-sql-server-agent-extension.md).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]klassieke implementatiemodel. De klassieke versie van dit artikel, Zie [Back-up automatisch voor SQL Server in Azure klassieke voor virtuele Machines](virtual-machines-windows-classic-sql-automated-backup.md).

## <a name="prerequisites"></a>Vereisten

Voor het gebruik van geautomatiseerde back-up kunt u de volgende vereisten:

**Besturingssysteem**:

- Windows Server 2012
- Windows Server 2012 R2

**Versie/editie van SQL Server**:

- SQL Server 2014 Standard
- SQL Server 2014 Enterprise

**Configuratie van de database**:

- Doeldatabases moeten het model voor volledig herstel gebruiken

**Azure PowerShell**:

- [Installeer de meest recente Azure PowerShell-opdrachten](../powershell-install-configure.md) als u automatische back-up configureren met PowerShell.

>[AZURE.NOTE] Automatische back-up is afhankelijk van de SQL Server IaaS Agent. Deze extensie standaard de huidige SQL-virtuele machine afbeeldingen toegevoegd. Zie [SQL Server IaaS Agent Extensions](virtual-machines-windows-sql-server-agent-extension.md)voor meer informatie.

## <a name="settings"></a>Instellingen

De volgende tabel beschrijft de opties die kunnen worden geconfigureerd voor automatische back-up. De werkelijke configuratiestappen zijn afhankelijk van of u de Azure portal of Azure Windows PowerShell-opdrachten gebruiken.

|Instelling|Bereik (standaard)|Beschrijving|
|---|---|---|
|**Automatische back-up**|Uitschakelen (uitgeschakeld)|Hiermee schakelt automatische back-up voor een Azure VM met SQL Server 2014 Standard of Enterprise of.|
|**Bewaartermijn**|1-30 dagen (30 dagen)|Het aantal dagen te bewaren van een back-up.|
|**Opslag Account**|Azure opslag account (de account opslag gemaakt voor de opgegeven VM)|Een account Azure opslag wilt gebruiken voor het opslaan van bestanden voor automatische back-up in blob-opslag. Een container wordt gemaakt op deze locatie voor het opslaan van alle back-upbestanden. De naamgevingsconventie van het back-upbestand bevat de datum, de tijd en de naam van de computer.|
|**Codering**|Uitschakelen (uitgeschakeld)|Hiermee schakelt u of codering. Als codering is ingeschakeld, worden de certificaten gebruikt om de back-up terugzetten bevinden zich in de opslag opgegeven account in dezelfde automaticbackup-container met behulp van de naamgevingsconventie voor dezelfde. Als het wachtwoord wordt gewijzigd, wordt een nieuw certificaat met dat wachtwoord wordt gegenereerd, maar het oude certificaat blijft om eerdere back-ups terugzetten.|
|**Wachtwoord**|Wachtwoord tekst (geen)|Een wachtwoord voor coderingssleutels. Dit is alleen vereist als codering is ingeschakeld. Om een gecodeerde back-up terugzetten, hebt u het juiste wachtwoord en het bijbehorende certificaat dat is gebruikt op het moment dat de back-up is gemaakt.|

## <a name="configuration-in-the-portal"></a>Configuratie in de Portal
U kunt de Portal Azure geautomatiseerde back-up configureren tijdens het inrichten of voor bestaande VMs.

### <a name="new-vms"></a>Nieuwe VMs
Via de Portal Azure geautomatiseerde back-up configureren wanneer u een nieuwe virtuele Machine voor SQL Server 2014 in het implementatiemodel Resource Manager.

Selecteer in de **instellingen van de SQL Server** -blade **geautomatiseerde back-ups**. De volgende Azure portal screenshot toont de blade **SQL geautomatiseerde back-up** .

![SQL geautomatiseerde back-up van de configuratie in Azure portal](./media/virtual-machines-windows-sql-automated-backup/azure-sql-arm-autobackup.png)

Zie het onderwerp volledig op het [inrichten van een SQL Server virtuele machine in Azure](virtual-machines-windows-portal-sql-server-provision.md)voor context.

### <a name="existing-vms"></a>Bestaande VMs
Selecteer uw SQL Server virtuele machine voor bestaande SQL Server virtuele machines. Selecteer de sectie **SQL Server-configuratie** van de **Instellingen voor** blade.

![SQL geautomatiseerde back-up voor bestaande VMs](./media/virtual-machines-windows-sql-automated-backup/azure-sql-rm-autobackup-existing-vms.png)

Klik op de knop **bewerken** in de automatische back-sectie in de **configuratie van SQL Server** -blade.

![SQL geautomatiseerde back-up configureren voor bestaande VMs](./media/virtual-machines-windows-sql-automated-backup/azure-sql-rm-autobackup-configuration.png)

Wanneer u klaar bent, klikt u op de knop **OK** op de onderkant van de **configuratie van SQL Server** -blade uw wijzigingen op te slaan.

Als u automatische back-up voor de eerste keer inschakelt, configureert Azure SQL Server IaaS Agent op de achtergrond. Gedurende deze periode de Azure portal worden niet altijd weergegeven dat automatisch back-up is geconfigureerd. Wacht enkele minuten voor de agent moet worden geïnstalleerd of geconfigureerd. Daarna wordt de Azure portal overeenkomen met de nieuwe instellingen.

>[AZURE.NOTE] U kunt ook automatische back-up van een sjabloon gebruiken. Zie voor meer informatie [Azure quickstart-sjabloon voor automatische back-up](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-sql-existing-autobackup-update).

## <a name="configuration-with-powershell"></a>Configuratie met PowerShell

Na het inrichten van uw VM SQL PowerShell te gebruiken voor het configureren van automatische back-up.

In het volgende voorbeeld PowerShell, is geautomatiseerde back-up geconfigureerd voor een bestaande SQL Server 2014 VM. De opdracht **AzureRM.Compute\New AzureVMSqlServerAutoBackupConfig** configureert de instellingen voor automatische back-up voor het opslaan van back-ups in de opslag Azure-account die is gekoppeld aan de virtuele machine. Deze back-ups voor 10 dagen bewaard. De opdracht **Set-AzureRmVMSqlServerExtension** werkt het opgegeven Azure VM met deze instellingen.

    $vmname = "vmname"
    $resourcegroupname = "resourcegroupname"
    $autobackupconfig = AzureRM.Compute\New-AzureVMSqlServerAutoBackupConfig -Enable -RetentionPeriodInDays 10 -ResourceGroupName $resourcegroupname

    Set-AzureRmVMSqlServerExtension -AutoBackupSettings $autobackupconfig -VMName $vmname -ResourceGroupName $resourcegroupname

Het kan enige tijd duren om te installeren en configureren van de IaaS SQL Server Agent.

Schakel codering door het vorige script om te slagen voor de parameter **EnableEncryption** samen met een wachtwoord (beveiligde string) voor de parameter **CertificatePassword** te wijzigen. Het volgende script kunt de instellingen voor automatische back-up in het vorige voorbeeld en codering wordt toegevoegd.

    $vmname = "vmname"
    $resourcegroupname = "resourcegroupname"
    $password = "P@ssw0rd"
    $encryptionpassword = $password | ConvertTo-SecureString -AsPlainText -Force  
    $autobackupconfig = AzureRM.Compute\New-AzureVMSqlServerAutoBackupConfig -Enable -RetentionPeriod 10 -EnableEncryption -CertificatePassword $encryptionpassword -ResourceGroupName $resourcegroupname

    Set-AzureRmVMSqlServerExtension -AutoBackupSettings $autobackupconfig -VMName $vmname -ResourceGroupName $resourcegroupname

Schakel automatische back-up uitvoeren van hetzelfde script zonder de **-inschakelen** -parameter voor de opdracht **AzureRM.Compute\New AzureVMSqlServerAutoBackupConfig** . Het ontbreken van de **-inschakelen** parameter geeft de opdracht voor het uitschakelen van de functie. Net als bij de installatie, kan het uitschakelen van automatische back-up van enkele minuten duren.

>[AZURE.NOTE] De IaaS SQL Server Agent worden niet verwijderd de eerder geconfigureerde instellingen voor automatische back-up. Schakel automatische back-up voordat de IaaS SQL Server Agent verwijderen of uitschakelen.

## <a name="next-steps"></a>Volgende stappen

Managed Backup configureren automatische back-up op Azure VMs. Het is dus belangrijk dat u [de documentatie voor het beheren van back-up](https://msdn.microsoft.com/library/dn449496.aspx) te begrijpen van het gedrag en de gevolgen.

U kunt extra back-up vinden en herstellen van richtlijnen voor SQL Server op Azure VMs in het volgende onderwerp: [back-up en herstel voor SQL Server in Azure virtuele Machines](virtual-machines-windows-sql-backup-recovery.md).

Zie voor informatie over andere beschikbare automatiseringstaken, [IaaS agentextensie voor SQL Server](virtual-machines-windows-sql-server-agent-extension.md).

Zie [SQL Server op de virtuele Machines van Azure-overzicht](virtual-machines-windows-sql-server-iaas-overview.md)voor meer informatie over het uitvoeren van SQL Server op Azure VMs.
