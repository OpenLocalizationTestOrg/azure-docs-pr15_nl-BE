<properties
    pageTitle="Automatische back-up voor SQL Server virtuele Machines (klassiek) | Microsoft Azure"
    description="Wordt de functie voor automatische back-up beschreven voor SQL Server wordt uitgevoerd in Azure virtuele Machines met behulp van bronbeheer. "
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

# <a name="automated-backup-for-sql-server-in-azure-virtual-machines-classic"></a>Automatische back-up van SQL Server in Azure virtuele Machines (klassiek)

> [AZURE.SELECTOR]
- [Resource Manager](virtual-machines-windows-sql-automated-backup.md)
- [Klassiek](virtual-machines-windows-classic-sql-automated-backup.md)

Automatische back-up configureert automatisch [Back-ups beheerd met Microsoft Azure](https://msdn.microsoft.com/library/dn449496.aspx) voor alle bestaande en nieuwe databases op een Azure VM met SQL Server 2014 Standard of Enterprise. Hiermee kunt u regelmatig back-ups die gebruikmaken van duurzame Azure blob-opslag configureren. Automatische back-up is afhankelijk van de [SQL Server IaaS agentextensie](virtual-machines-windows-classic-sql-server-agent-extension.md).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Zie [Geautomatiseerde back-up van SQL Server in Azure virtuele Machines Resource Manager](virtual-machines-windows-sql-automated-backup.md)de Resource Manager versie van dit artikel.

## <a name="prerequisites"></a>Vereisten

Voor het gebruik van geautomatiseerde back-up kunt u de volgende vereisten:

**Besturingssysteem**:

- Windows Server 2012
- Windows Server 2012 R2

**Versie/editie van SQL Server**:

- SQL Server 2014 Standard
- SQL Server 2014 Enterprise

>[AZURE.NOTE] SQL Server 2016 wordt nog niet ondersteund voor geautomatiseerde back-up.

**Configuratie van de database**:

- Doeldatabases moeten het model voor volledig herstel gebruiken.

**Azure PowerShell**:

- [Installeer de meest recente Azure PowerShell-opdrachten](../powershell-install-configure.md).

**IaaS extensie van SQL Server**:

- [De extensie IaaS van SQL Server te installeren](virtual-machines-windows-classic-sql-server-agent-extension.md).

## <a name="settings"></a>Instellingen

De volgende tabel beschrijft de opties die kunnen worden geconfigureerd voor automatische back-up. U moet voor klassieke VMs, PowerShell gebruiken om deze instellingen te configureren.

|Instelling|Bereik (standaard)|Beschrijving|
|---|---|---|
|**Automatische back-up**|Uitschakelen (uitgeschakeld)|Hiermee schakelt automatische back-up voor een Azure VM met SQL Server 2014 Standard of Enterprise of.|
|**Bewaartermijn**|1-30 dagen (30 dagen)|Het aantal dagen te bewaren van een back-up.|
|**Opslag Account**|Azure opslag account (de account opslag gemaakt voor de opgegeven VM)|Een account Azure opslag wilt gebruiken voor het opslaan van bestanden voor automatische back-up in blob-opslag. Een container wordt gemaakt op deze locatie voor het opslaan van alle back-upbestanden. De naamgevingsconventie van het back-upbestand bevat de datum, de tijd en de naam van de computer.|
|**Codering**|Uitschakelen (uitgeschakeld)|Hiermee schakelt u of codering. Als codering is ingeschakeld, worden de certificaten gebruikt om de back-up terugzetten bevinden zich in de opslag opgegeven account in dezelfde automaticbackup-container met behulp van de naamgevingsconventie voor dezelfde. Als het wachtwoord wordt gewijzigd, wordt een nieuw certificaat met dat wachtwoord wordt gegenereerd, maar het oude certificaat blijft om eerdere back-ups terugzetten.|
|**Wachtwoord**|Wachtwoord tekst (geen)|Een wachtwoord voor coderingssleutels. Dit is alleen vereist als codering is ingeschakeld. Om een gecodeerde back-up terugzetten, hebt u het juiste wachtwoord en het bijbehorende certificaat dat is gebruikt op het moment dat de back-up is gemaakt.|

## <a name="configuration-with-powershell"></a>Configuratie met PowerShell

In het volgende voorbeeld PowerShell, is geautomatiseerde back-up geconfigureerd voor een bestaande SQL Server 2014 VM. De opdracht **Nieuw AzureVMSqlServerAutoBackupConfig** configureert de instellingen voor automatische back-up voor het opslaan van back-ups in de opslag Azure-account die is opgegeven door de variabele $storageaccount. Deze back-ups voor 10 dagen bewaard. De opdracht **Set-AzureVMSqlServerExtension** werkt het opgegeven Azure VM met deze instellingen.

    $storageaccount = "<storageaccountname>"
    $storageaccountkey = (Get-AzureStorageKey -StorageAccountName $storageaccount).Primary
    $storagecontext = New-AzureStorageContext -StorageAccountName $storageaccount -StorageAccountKey $storageaccountkey
    $autobackupconfig = New-AzureVMSqlServerAutoBackupConfig -StorageContext $storagecontext -Enable -RetentionPeriod 10

    Get-AzureVM -ServiceName <vmservicename> -Name <vmname> | Set-AzureVMSqlServerExtension -AutoBackupSettings $autobackupconfig | Update-AzureVM

Het kan enige tijd duren om te installeren en configureren van de IaaS SQL Server Agent.

Schakel codering door het vorige script om te slagen voor de parameter EnableEncryption samen met een wachtwoord (beveiligde string) voor de parameter CertificatePassword te wijzigen. Het volgende script kunt de instellingen voor automatische back-up in het vorige voorbeeld en codering wordt toegevoegd.

    $storageaccount = "<storageaccountname>"
    $storageaccountkey = (Get-AzureStorageKey -StorageAccountName $storageaccount).Primary
    $storagecontext = New-AzureStorageContext -StorageAccountName $storageaccount -StorageAccountKey $storageaccountkey
    $password = "P@ssw0rd"
    $encryptionpassword = $password | ConvertTo-SecureString -AsPlainText -Force  
    $autobackupconfig = New-AzureVMSqlServerAutoBackupConfig -StorageContext $storagecontext -Enable -RetentionPeriod 10 -EnableEncryption -CertificatePassword $encryptionpassword

    Get-AzureVM -ServiceName <vmservicename> -Name <vmname> | Set-AzureVMSqlServerExtension -AutoBackupSettings $autobackupconfig | Update-AzureVM

Schakel automatische back-up uitvoeren van hetzelfde script zonder de **-inschakelen** -parameter voor de **Nieuwe AzureVMSqlServerAutoBackupConfig**. Net als bij de installatie, kan het uitschakelen van automatische back-up van enkele minuten duren.

>[AZURE.NOTE] Uitschakelen en het verwijderen van de IaaS SQL Server Agent wordt niet verwijderd van de eerder geconfigureerde instellingen voor back-up beheerd. Schakel automatische back-up voordat de IaaS SQL Server Agent verwijderen of uitschakelen.

## <a name="next-steps"></a>Volgende stappen

Managed Backup configureren automatische back-up op Azure VMs. Het is dus belangrijk dat u [de documentatie voor het beheren van back-up](https://msdn.microsoft.com/library/dn449496.aspx) te begrijpen van het gedrag en de gevolgen.

U kunt extra back-up vinden en herstellen van richtlijnen voor SQL Server op Azure VMs in het volgende onderwerp: [back-up en herstel voor SQL Server in Azure virtuele Machines](virtual-machines-windows-sql-backup-recovery.md).

Zie voor informatie over andere beschikbare automatiseringstaken, [IaaS agentextensie voor SQL Server](virtual-machines-windows-classic-sql-server-agent-extension.md).

Zie [SQL Server op de virtuele Machines van Azure-overzicht](virtual-machines-windows-sql-server-iaas-overview.md)voor meer informatie over het uitvoeren van SQL Server op Azure VMs.
