<properties
   pageTitle="Sjablonen met Linux VM-uitbreidingen schrijven | Microsoft Azure"
   description="Meer informatie over het ontwerpen van Azure Resource Manager sjablonen met extensies voor Linux VMs"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="kundanap"
   manager="timlt"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="03/29/2016"
   ms.author="kundanap"/>

# <a name="authoring-azure-resource-manager-templates-with-linux-vm-extensions"></a>Sjablonen met Linux VM extensies Azure Resource Manager ontwerpen

[AZURE.INCLUDE [virtual-machines-common-extensions-authoring-templates](../../includes/virtual-machines-common-extensions-authoring-templates.md)]

Uitvoeren van Azure CLI, de volgende verbindingsopdracht:

      Azure VM extension list

Met deze opdracht geeft als resultaat de naam van de uitgever, Extensienaam en de versie als volgt:

      Publisher                   : Microsoft.Azure.Extensions  
      ExtensionName               : DockerExtension
      Version                     : 1.0

Deze drie eigenschappen toewijzen aan 'publisher', 'type' en 'typeHandlerVersion', respectievelijk in het bovenstaande fragment van de sjabloon.

>[AZURE.NOTE]Het is altijd raadzaam de meest recente versie van de uitbreiding met de meest bijgewerkte functionaliteit.

## <a name="identifying-the-schema-for-the-extension-configuration-parameters"></a>Het schema voor de uitbreiding configuratieparameters bepalen

De volgende stap bij het ontwerpen van een sjabloon met extensie is de indeling voor het verstrekken van configuratieparameters te identificeren. Elke extensie ondersteunt een eigen set parameters.

Voorbeelden van configuraties voor Linux extensies bekijken, klikt u op de documentatie bij [Linux eExtensions monsters](virtual-machines-linux-extensions-configuration-samples.md)Zie.

Raadpleeg het volgende om een volledig complete sjabloon met de VM.

[Aangepast script extensie op een Linux VM](https://github.com/Azure/azure-quickstart-templates/blob/b1908e74259da56a92800cace97350af1f1fc32b/mongodb-on-ubuntu/azuredeploy.json/)

Na het ontwerpen van de sjabloon, kunt u met behulp van de CLI Azure implementeren.
