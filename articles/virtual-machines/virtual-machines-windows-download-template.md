<properties
    pageTitle="Een VM-afbeelding maken van een Azure VM | Microsoft Azure"
    description="Informatie over het maken van een algemene VM installatiekopie van een bestaande Azure VM gemaakt in het implementatiemodel Resource Manager"
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="cynthn"/>


# <a name="download-the-template-for-a-vm"></a>De sjabloon voor een VM downloaden

Wanneer u een VM in Azure via de portal of PowerShell maakt, wordt automatisch een sjabloon Resource Manager voor u gemaakt. Met deze sjabloon kunt u snel een implementatie te dupliceren. De sjabloon bevat informatie over alle resources in een resourcegroep. Voor een virtuele machine, betekent dit dat de sjabloon containers alles wat is gemaakt ter ondersteuning van de VM in die resourcegroep, met inbegrip van het netwerk bronnen.

## <a name="download-the-template-using-the-portal"></a>De sjabloon met behulp van de portal downloaden

1. Log in op de [Azure portal](https://portal.azure.com/).
2. Een hub het menu Selecteer **virtuele Machines**.
3. Selecteer de virtuele machine in de lijst.
5. Selecteer **script voor automatisering**.
6. Selecteer **downloaden** en sla het ZIP-bestand naar uw lokale computer.
7. Open het ZIP-bestand en pak de bestanden in een map. Het ZIP-bestand bevat:
    
    - Deploy.ps1
    - Deploy.sh 
    - deployer.RB
    - DeploymentHelper.cs
    - parameters.JSON
    - Template.JSON

Het bestand .json is de sjabloon.
    
## <a name="download-the-template-using-powershell"></a>De sjabloon met PowerShell downloaden

U kunt ook de .json-sjabloonbestand met de cmdlet [Export-AzureRMResourceGroup](https://msdn.microsoft.com/library/mt715427.aspx) downloaden. U kunt de `-path` -parameter voor de bestandsnaam en het pad voor het bestand .json bieden. In dit voorbeeld ziet hoe u de sjabloon voor de groep met de naam **myResourceGroup** naar de map **C:\users\public\downloads** op de lokale computer downloaden.

```powershell
    Export-AzureRmResourceGroup -ResourceGroupName "myResourceGroup" -Path "C:\users\public\downloads"
```

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het implementeren van bronnen met behulp van sjablonen, [Resource Manager sjabloon scenario](../resource-manager-template-walkthrough.md).