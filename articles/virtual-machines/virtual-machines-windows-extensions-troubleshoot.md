<properties
   pageTitle="Problemen met Windows VM extensie oplost | Microsoft Azure"
   description="Klik hier voor informatie over het oplossen van storingen in Azure Windows VM-extensie"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="kundanap"
   manager="timlt"
   editor=""
   tags="top-support-issue,azure-resource-manager"/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="03/29/2016"
   ms.author="kundanap"/>

# <a name="troubleshooting-azure-windows-vm-extension-failures"></a>Problemen met storingen in Azure Windows VM-extensie

[AZURE.INCLUDE [virtual-machines-common-extensions-troubleshoot](../../includes/virtual-machines-common-extensions-troubleshoot.md)]

## <a name="viewing-extension-status"></a>Status van extensie weergeven
Azure Resource Manager-sjablonen kunnen worden uitgevoerd vanuit Azure PowerShell. Zodra de sjabloon wordt uitgevoerd, kan de status van extensie van Azure Resource Explorer of het opdrachtregelprogramma's worden weergegeven.

Hier volgt een voorbeeld:

Azure PowerShell:

      Get-AzureRmVM -ResourceGroupName $RGName -Name $vmName -Status

Hier is een voorbeeld van de uitvoer:

      Extensions:  {
      "ExtensionType": "Microsoft.Compute.CustomScriptExtension",
      "Name": "myCustomScriptExtension",
      "SubStatuses": [
        {
          "Code": "ComponentStatus/StdOut/succeeded",
          "DisplayStatus": "Provisioning succeeded",
          "Level": "Info",
          "Message": "    Directory: C:\\temp\\n\\n\\nMode                LastWriteTime     Length Name
              \\n----                -------------     ------ ----                              \\n-a---          9/1/2015   2:03 AM         11
              test.txt                          \\n\\n",
                      "Time": null
          },
        {
          "Code": "ComponentStatus/StdErr/succeeded",
          "DisplayStatus": "Provisioning succeeded",
          "Level": "Info",
          "Message": "",
          "Time": null
        }
    }
  ]

## <a name="troubleshooting-extension-failures"></a>Extensie-fouten oplossen

### <a name="re-running-the-extension-on-the-vm"></a>De extensie opnieuw uit te voeren op de VM

Als u scripts op de VM via aangepast Script extensie, kan het soms ook uitvoeren in een fout wanneer VM is gemaakt, maar het script is mislukt. Onder deze voorwaarden is de aanbevolen manier om deze fout herstellen om de extensie te verwijderen en de sjabloon opnieuw opnieuw.
Opmerking: In het vervolg deze functionaliteit verbeterd zouden kunnen worden om de noodzaak voor het verwijderen van de extensie te verwijderen.


#### <a name="remove-the-extension-from-azure-powershell"></a>De uitbreiding van Azure PowerShell verwijderen

    Remove-AzureRmVMExtension -ResourceGroupName $RGName -VMName $vmName -Name "myCustomScriptExtension"

Zodra de extensie is verwijderd, kan de sjabloon opnieuw worden uitgevoerd om de scripts uitvoert op de VM.
