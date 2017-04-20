<properties
    pageTitle="Informatie over afbeeldingen voor Windows virtuele machines | Microsoft Azure"
    description="Meer informatie over de manier waarop afbeeldingen worden gebruikt met Windows virtuele machines in Azure."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/21/2016"
    ms.author="cynthn"/>

# <a name="about-images-for-windows-virtual-machines"></a>Informatie over afbeeldingen voor Windows virtuele machines

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

[AZURE.INCLUDE [virtual-machines-common-classic-about-images](../../includes/virtual-machines-common-classic-about-images.md)]



## <a name="working-with-images"></a>Werken met afbeeldingen

U kunt de Azure PowerShell-module voor het beheren van de afbeeldingen beschikbaar om uw abonnement op Azure. Ook kunt u de klassieke Azure portal voor bepaalde taken van de afbeelding, maar de opdrachtregel biedt u meer opties.


-   **Alle afbeeldingen ophalen**:`Get-AzureVMImage`resulteert in een lijst van alle afbeeldingen die beschikbaar zijn in uw huidige abonnement: afbeeldingen, alsmede die welke door Azure of partners. Dit betekent bijvoorbeeld dat u een grote lijst. De volgende voorbeelden laten zien hoe een kortere lijst.
-   **Afbeelding families krijgen**:`Get-AzureVMImage | select ImageFamily` haalt u een lijst van families van de afbeelding door tekenreeksen **ImageFamily** eigenschap weer te geven.
-   **Alle afbeeldingen in een bepaalde familie ophalen**:`Get-AzureVMImage | Where-Object {$_.ImageFamily -eq $family}`
-   **VM afbeeldingen vinden**: `Get-AzureVMImage | where {(gm –InputObject $_ -Name DataDiskConfigurations) -ne $null} | Select -Property Label, ImageName` dit werkt door het filteren van de eigenschap DataDiskConfiguration, die alleen van toepassing op afbeeldingen in VM. In dit voorbeeld wordt ook de uitvoer met alleen de naam label en image filters.
-   **Een algemene afbeelding opslaan**:`Save-AzureVMImage –ServiceName "myServiceName" –Name "MyVMtoCapture" –OSState "Generalized" –ImageName "MyVmImage" –ImageLabel "This is my generalized image"`
-   **Een speciale afbeelding opslaan**:`Save-AzureVMImage –ServiceName "mySvc2" –Name "MyVMToCapture2" –ImageName "myFirstVMImageSP" –OSState "Specialized" -Verbose`
>[Azure.Tip] De OSState-parameter is vereist als u wilt dat een VM-installatiekopie, inclusief data schijven als de schijf maken. Als u de parameter niet gebruikt, maakt de cmdlet een installatiekopie van het besturingssysteem. De waarde van de parameter geeft aan of de afbeelding is algemene of gespecialiseerde, op basis van de vraag of de schijf is voorbereid voor hergebruik.
-   **Een afbeelding verwijderen**:`Remove-AzureVMImage –ImageName "MyOldVmImage"`


## <a name="next-steps"></a>Volgende stappen

U kunt ook [een Windows-computer met behulp van de klassieke portal maken](virtual-machines-windows-classic-tutorial.md)

