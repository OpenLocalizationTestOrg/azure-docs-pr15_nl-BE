<properties
   pageTitle="Sjablonen met Windows VM-uitbreidingen schrijven | Microsoft Azure"
   description="Meer informatie over het ontwerpen van Azure Resource Manager sjablonen met extensies voor Windows VMs"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="kundanap"
   manager="timlt"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="03/29/2016"
   ms.author="kundanap"/>

# <a name="authoring-azure-resource-manager-templates-with-windows-vm-extensions"></a>Sjablonen met Windows VM extensies Azure Resource Manager ontwerpen

[AZURE.INCLUDE [virtual-machines-common-extensions-authoring-templates](../../includes/virtual-machines-common-extensions-authoring-templates.md)]

Van Azure PowerShell de volgende Azure PowerShell-cmdlet uitvoert:

      Get-AzureVMAvailableExtension


Deze cmdlet retourneert de naam van de uitgever, naam en versie als volgt:

      Publisher                   : Microsoft.Azure.Extensions  
      ExtensionName               : DockerExtension
      Version                     : 1.0

Deze drie eigenschappen toewijzen aan 'publisher', 'type' en 'typeHandlerVersion', respectievelijk in het bovenstaande fragment van de sjabloon.

>[AZURE.NOTE]Het is altijd raadzaam de meest recente versie van de uitbreiding met de meest bijgewerkte functionaliteit.

## <a name="identifying-the-schema-for-the-extension-configuration-parameters"></a>Het schema voor de uitbreiding configuratieparameters bepalen

De volgende stap bij het ontwerpen van een sjabloon met extensie is de indeling voor het verstrekken van configuratieparameters te identificeren. Elke extensie ondersteunt een eigen set parameters.

Zie als u wilt voorbeelden van configuraties voor uitbreidingen voor Windows, [Windows extensions monsters](virtual-machines-windows-extensions-configuration-samples.md).


Raadpleeg het volgende om een sjabloon volledig voltooid met de VM-extensies.

[Aangepast Script extensie op een Windows VM](https://github.com/Azure/azure-quickstart-templates/blob/b1908e74259da56a92800cace97350af1f1fc32b/201-list-storage-keys-windows-vm/azuredeploy.json/)


Na het ontwerpen van de sjabloon, kunt u met behulp van Azure PowerShell implementeren.
