<properties
   pageTitle="Met behulp van de gewenste State configuratie met virtuele Machine schaal Sets | Microsoft Azure"
   description="Schaal van de virtuele Machine wordt ingesteld met de Azure DSC-extensie"
   services="virtual-machine-scale-sets"
   documentationCenter=""
   authors="zjalexander"
   manager="timlt"
   editor=""
   tags="azure-service-management,azure-resource-manager"
   keywords=""/>

<tags
   ms.service="virtual-machine-scale-sets"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="na"
   ms.date="09/15/2016"
   ms.author="zachal"/>

# <a name="using-virtual-machine-scale-sets-with-the-azure-dsc-extension"></a>Schaal van de virtuele Machine wordt ingesteld met de Azure DSC-extensie

[Virtuele Machine schaal Sets (VMSS)](virtual-machine-scale-sets-overview.md) kan worden gebruikt met de extensie-handler [Azure gewenst staat configuratie (DSC)](../virtual-machines/virtual-machines-windows-extensions-dsc-overview.md) . VMSS biedt een manier om te implementeren en beheren van een groot aantal virtuele machines en elastically in en uit kunt schalen in reactie op laden. DSC wordt het VMs configureren als ze online komen, zodat deze worden uitgevoerd, de productie-software gebruikt.

## <a name="differences-between-deploying-to-vm-and-vmss"></a>Verschillen in de implementatie van de VM en VMSS

De onderliggende sjabloonstructuur voor VMSS is iets anders dan een enkele VM. Met name implementeert een enkele VM extensies onder het knooppunt 'virtualMachines'. Er is een vermelding van het type 'extensions' waar DSC is toegevoegd aan de sjabloon

```
"resources": [
          {
              "name": "Microsoft.Powershell.DSC",
              "type": "extensions",
              "location": "[resourceGroup().location]",
              "apiVersion": "2015-06-15",
              "dependsOn": [
                  "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
              ],
              "tags": {
                  "displayName": "dscExtension"
              },
              "properties": {
                  "publisher": "Microsoft.Powershell",
                  "type": "DSC",
                  "typeHandlerVersion": "2.20",
                  "autoUpgradeMinorVersion": false,
                  "forceUpdateTag": "[parameters('dscExtensionUpdateTagVersion')]",
                  "settings": {
                      "configuration": {
                          "url": "[concat(parameters('_artifactsLocation'), '/', variables('dscExtensionArchiveFolder'), '/', variables('dscExtensionArchiveFileName'))]",
                          "script": "DscExtension.ps1",
                          "function": "Main"
                      },
                      "configurationArguments": {
                          "nodeName": "[variables('vmName')]"
                      }
                  },
                  "protectedSettings": {
                      "configurationUrlSasToken": "[parameters('_artifactsLocationSasToken')]"
                  }
              }
          }
      ]
```

Een knooppunt VMSS heeft een sectie 'Eigenschappen' met de 'VirtualMachineProfile', 'extensionProfile'-kenmerk. DSC toegevoegd onder 'extensions'

```
"extensionProfile": {
            "extensions": [
                {
                    "name": "Microsoft.Powershell.DSC",
                    "properties": {
                        "publisher": "Microsoft.Powershell",
                        "type": "DSC",
                        "typeHandlerVersion": "2.20",
                        "autoUpgradeMinorVersion": false,
                        "forceUpdateTag": "[parameters('DscExtensionUpdateTagVersion')]",
                        "settings": {
                            "configuration": {
                                "url": "[concat(parameters('_artifactsLocation'), '/', variables('DscExtensionArchiveFolder'), '/', variables('DscExtensionArchiveFileName'))]",
                                "script": "DscExtension.ps1",
                                "function": "Main"
                            },
                            "configurationArguments": {
                                "nodeName": "localhost"
                            }
                        },
                        "protectedSettings": {
                            "configurationUrlSasToken": "[parameters('_artifactsLocationSasToken')]"
                        }
                    }
                }
            ]
```

## <a name="behavior-for-vmss"></a>Gedrag voor VMSS

Het gedrag van VMSS is gelijk aan het gedrag van een enkele VM. Wanneer een nieuwe VM wordt gemaakt, wordt deze automatisch met de extensie DSC ingericht. Als u een nieuwere versie van de WMF is vereist voor de uitbreiding, de VM opnieuw wordt opgestart voordat u on line wordt gebracht. Zodra u on line is, de DSC-configuratie .zip downloadt en het inrichten van de VM. Meer informatie vindt u in [Het overzicht Azure DSC-extensie](../virtual-machines/virtual-machines-windows-extensions-dsc-overview.md).

## <a name="next-steps"></a>Volgende stappen ##
Controleer de [sjabloon voor de DSC-extensie Azure Resource Manager](../virtual-machines/virtual-machines-windows-extensions-dsc-template.md).

Meer informatie over hoe de [DSC-extensie referenties veilig worden verwerkt](../virtual-machines/virtual-machines-windows-extensions-dsc-credentials.md). 

Zie voor meer informatie over de handler Azure DSC-extensie, [Inleiding tot de configuratie van Azure staat de gewenste extensie handler](../virtual-machines/virtual-machines-windows-extensions-dsc-overview.md). 

Voor meer informatie over PowerShell DSC, [gaat u naar het midden van de documentatie van PowerShell](https://msdn.microsoft.com/powershell/dsc/overview). 


