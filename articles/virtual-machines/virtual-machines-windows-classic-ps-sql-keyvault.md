<properties
    pageTitle="Integratie van de Azure sleutel kluis voor SQL Server configureren op Azure VMs (klassiek)"
    description="Informatie over het automatiseren van de configuratie van SQL Server-versleuteling voor gebruik met Azure sleutel kluis. In dit onderwerp wordt uitgelegd hoe Azure sleutel kluis integratie met SQL Server virtuele machines in het implementatiemodel klassiek maken gebruiken."
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
    ms.date="09/26/2016"
    ms.author="jroth"/>

# <a name="configure-azure-key-vault-integration-for-sql-server-on-azure-vms-classic"></a>Integratie van de Azure sleutel kluis voor SQL Server configureren op Azure VMs (klassiek)

> [AZURE.SELECTOR]
- [Resource Manager](virtual-machines-windows-ps-sql-keyvault.md)
- [Klassiek](virtual-machines-windows-classic-ps-sql-keyvault.md)

## <a name="overview"></a>Overzicht
Er zijn meerdere SQL Server coderingsfuncties zoals [transparante gegevenscodering (TDE)](https://msdn.microsoft.com/library/bb934049.aspx) [kolom niveau van codering (CLE)](https://msdn.microsoft.com/library/ms173744.aspx)en [back-codering](https://msdn.microsoft.com/library/dn449489.aspx). Deze vormen van codering, moet u de cryptografiesleutels die voor codering u opslaan en beheren. De Azure sleutel kluis (AKV)-service is ontworpen voor het verbeteren van de beveiliging en het beheer van deze sleutels in een veilige en hoge beschikbaarheid locatie. De [SQL Server-Connector](http://www.microsoft.com/download/details.aspx?id=45344) kunnen deze toetsen van Azure sleutel kluis worden gebruikt door SQL Server.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

Als u SQL Server voor gebouwen, machines uitvoert, zijn er [stappen die u kunt volgen om toegang tot Azure sleutel kluis van uw SQL Server-computer op gebouwen](https://msdn.microsoft.com/library/dn198405.aspx). Maar voor SQL Server in Azure VMs, kunt u tijd besparen met behulp van de functie *Integratie met Azure sleutel kluis* . Met een paar Azure PowerShell-cmdlets voor het inschakelen van deze functie, kunt u de configuratie nodig voor een SQL-VM toegang te krijgen tot uw sleutel kluis kunt automatiseren.

Wanneer deze functie is ingeschakeld, het automatisch de SQL Server-Connector installeert, configureert u de provider EKM toegang tot Azure sleutel kluis en maakt u de referenties waarmee u toegang tot uw kluis. Als u de stappen in de documentatie van de eerder genoemde op gebouwen bekeken, kunt u zien dat deze functie kunt u stap 2 en 3 automatiseren. Het enige wat dat nog moet u handmatig doen is de sleutel kluis en toetsen te maken. Daar is de instellingen van uw SQL-VM geautomatiseerd. Als deze functie kan deze setup is voltooid, kunt u de T-SQL-instructies om te beginnen met het coderen van uw databases of back-ups als normaal uitvoeren.

[AZURE.INCLUDE [AKV Integration Prepare](../../includes/virtual-machines-sql-server-akv-prepare.md)]

## <a name="configure-akv-integration"></a>AKV-integratie configureren
Gebruik PowerShell Azure sleutel kluis integratie configureren. De volgende secties vindt een overzicht van de vereiste parameters en vervolgens een voorbeeldscript met PowerShell.

### <a name="install-the-sql-server-iaas-extension"></a>De IaaS SQL Server Extensions installeren

Eerste, [de extensie IaaS van SQL Server te installeren](virtual-machines-windows-classic-sql-server-agent-extension.md).

### <a name="understand-the-input-parameters"></a>De invoerparameters begrijpen
De volgende tabel bevat de vereiste parameters voor het PowerShell script uitvoeren in de volgende sectie.

|Parameter|Beschrijving|Voorbeeld|
|---|---|---|
|**$akvURL**|**De URL van de sleutel kluis**|"https://contosokeyvault.vault.azure.net/"|
|**$spName**|**Service Principal-naam**|"fde2b411 - 33d 5-4e11-af04eb07b669ccf2"|
|**$spSecret**|**Service Principal geheim**|"9VTJSQwzlFepD8XODnzy8n2V01Jd8dAjwm/azF1XDKM ="|
|**$credName**|**Referentie-naam**: AKV integratie maakt een referentie binnen SQL Server, zodat de VM toegang hebben tot de sleutel kluis. Kies een naam voor deze referentie.|"mycred1"|
|**$vmName**|**Naam van de virtuele machine**: de naam van een eerder gemaakte SQL-VM.|"myvmname"|
|**$serviceName**|**Naam**: naam van de Cloud-Service die is gekoppeld aan de SQL VM.|"mycloudservicename"|

### <a name="enable-akv-integration-with-powershell"></a>Integratie met PowerShell AKV
De cmdlet **New-AzureVMSqlServerKeyVaultCredentialConfig** maakt een configuratie voor de functie Integratie met Azure sleutel kluis. De **Set AzureVMSqlServerExtension** configureert deze integratie met de parameter **KeyVaultCredentialSettings** . De volgende stappen laten zien hoe deze opdrachten gebruiken.

1. In Azure PowerShell, eerst de invoerparameters configureren met uw specifieke waarden zoals beschreven in de voorgaande secties in dit onderwerp. Het volgende script is een voorbeeld.

        $akvURL = "https://contosokeyvault.vault.azure.net/"
        $spName = "fde2b411-33d5-4e11-af04eb07b669ccf2"
        $spSecret = "9VTJSQwzlFepD8XODnzy8n2V01Jd8dAjwm/azF1XDKM="
        $credName = "mycred1"
        $vmName = "myvmname"
        $serviceName = "mycloudservicename"
2.  Gebruik vervolgens het volgende script configureren en inschakelen van AKV-integratie.

        $secureakv =  $spSecret | ConvertTo-SecureString -AsPlainText -Force
        $akvs = New-AzureVMSqlServerKeyVaultCredentialConfig -Enable -CredentialName $credname -AzureKeyVaultUrl $akvURL -ServicePrincipalName $spName -ServicePrincipalSecret $secureakv
        Get-AzureVM -ServiceName $serviceName -Name $vmName | Set-AzureVMSqlServerExtension -KeyVaultCredentialSettings $akvs | Update-AzureVM

De extensie SQL IaaS Agent zal de VM SQL bijwerken met deze nieuwe configuratie.

[AZURE.INCLUDE [AKV Integration Next Steps](../../includes/virtual-machines-sql-server-akv-next-steps.md)]
