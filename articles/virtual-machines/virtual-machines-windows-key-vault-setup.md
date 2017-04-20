<properties
    pageTitle="Sleutel kluis instellen voor virtuele machines in Azure Resource Manager | Microsoft Azure"
    description="Het instellen van de sleutel kluis voor gebruik met een virtuele machine van Azure Resource Manager."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="singhkays"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="05/31/2016"
    ms.author="singhkay"/>

# <a name="set-up-key-vault-for-virtual-machines-in-azure-resource-manager"></a>Sleutel kluis instellen voor virtuele machines in Azure Resource Manager

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]klassieke implementatiemodel.

In Azure Resource Manager-stack worden geheimen/certificaten gemodelleerd als bronnen die worden geleverd door de aanbieder van de resource van sleutel kluis. Zie voor meer informatie over de sleutel kluis, [Wat is Azure sleutel kluis?](../key-vault/key-vault-whatis.md)

>[AZURE.NOTE] 
>
>1. Sleutel kluis met Azure Resource Manager virtuele machines worden gebruikt om de eigenschap **EnabledForDeployment** op sleutel kluis moet worden ingesteld op true. U kunt dit doen in verschillende clients.
>
>2. De sleutel kluis moet worden gemaakt in dezelfde locatie als de virtuele Machine en abonnement.

## <a name="use-powershell-to-set-up-key-vault"></a>PowerShell gebruiken voor het instellen van een sleutel kluis
Zie voor informatie over het maken van een sleutel kluis met PowerShell [aan de slag met Azure sleutel kluis](../key-vault/key-vault-get-started.md#vault).

Voor de nieuwe sleutel kluizen, kunt u deze PowerShell-cmdlet gebruiken:

    New-AzureRmKeyVault -VaultName 'ContosoKeyVault' -ResourceGroupName 'ContosoResourceGroup' -Location 'East Asia' -EnabledForDeployment

Voor bestaande sleutels kluizen, kunt u deze PowerShell-cmdlet gebruiken:

    Set-AzureRmKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -EnabledForDeployment

## <a name="us-cli-to-set-up-key-vault"></a>Ons CLI sleutel kluis instellen
Zie voor informatie over het maken van een sleutel kluis via de opdrachtregelinterface (CLI) [sleutel kluis beheren met behulp van CLI](../key-vault/key-vault-manage-with-cli.md#create-a-key-vault).

Voor CLI moet u de sleutel kluis maken voordat u het beleid toewijzen. U kunt dit doen door de volgende opdracht:

    azure keyvault set-policy ContosoKeyVault –enabled-for-deployment true

## <a name="use-templates-to-set-up-key-vault"></a>Sjablonen gebruiken voor het instellen van een sleutel kluis
Terwijl u een sjabloon gebruikt, moet u de `enabledForDeployment` eigenschap `true` voor de resource sleutel kluis.

    {
      "type": "Microsoft.KeyVault/vaults",
      "name": "ContosoKeyVault",
      "apiVersion": "2015-06-01",
      "location": "<location-of-key-vault>",
      "properties": {
        "enabledForDeployment": "true",
        ....
        ....
      }
    }

Zie voor andere opties die u configureren kunt wanneer u een sleutel kluis maakt met behulp van sjablonen, [maken een sleutel kluis](https://azure.microsoft.com/documentation/templates/101-key-vault-create/).
