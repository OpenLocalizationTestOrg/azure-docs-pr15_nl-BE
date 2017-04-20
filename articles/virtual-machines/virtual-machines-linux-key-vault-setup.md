<properties
    pageTitle="Sleutel kluis instellen voor virtuele machines in Azure Resource Manager | Microsoft Azure"
    description="Het instellen van de sleutel kluis voor gebruik met een virtuele machine van Azure Resource Manager."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="singhkays"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="05/31/2016"
    ms.author="singhkay"/>

# <a name="set-up-key-vault-for-virtual-machines-in-azure-resource-manager"></a>Sleutel kluis instellen voor virtuele machines in Azure Resource Manager

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]klassieke implementatiemodel.

In Azure Resource Manager-stack worden geheimen/certificaten gemodelleerd als bronnen die worden geleverd door de aanbieder van de resource van sleutel kluis. Zie voor meer informatie over Azure sleutel kluis, [Wat is Azure sleutel kluis?](../key-vault/key-vault-whatis.md)

Sleutel kluis met Azure Resource Manager virtuele machines worden gebruikt om de eigenschap *EnabledForDeployment* op sleutel kluis moet worden ingesteld op true. U kunt dit doen in verschillende clients."

## <a name="use-cli-to-set-up-key-vault"></a>CLI gebruiken voor het instellen van een sleutel kluis
Zie voor informatie over het maken van een sleutel kluis via de opdrachtregelinterface (CLI) [sleutel kluis beheren met behulp van CLI](../key-vault/key-vault-manage-with-cli.md#create-a-key-vault).

Voor CLI moet u de sleutel kluis maken voordat u het beleid toewijzen. U kunt dit doen door de volgende opdracht:

    azure keyvault set-policy ContosoKeyVault –enabled-for-deployment true

## <a name="use-templates-to-set-up-key-vault"></a>Sjablonen gebruiken voor het instellen van een sleutel kluis
Wanneer u een sjabloon gebruikt, moet u de `enabledForDeployment` eigenschap `true` voor de resource sleutel kluis.

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
