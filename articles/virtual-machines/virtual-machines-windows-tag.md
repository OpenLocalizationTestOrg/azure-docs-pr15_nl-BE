<properties
   pageTitle="Het label een VM | Microsoft Azure"
   description="Meer informatie over het coderen van een virtuele Windows-computer in het implementatiemodel Resource Manager met Azure gemaakt"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="mmccrory"
   manager="timlt"
   editor="tysonn"
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="07/05/2016"
   ms.author="memccror"/>

# <a name="how-to-tag-a-windows-virtual-machine-in-azure"></a>Het label van een virtuele Windows-computer in Azure


Dit artikel beschrijft verschillende manieren een virtuele Windows-computer een label in Azure via het implementatiemodel Resource Manager. Tags zijn door de gebruiker gedefinieerde sleutel/waarde-paren die rechtstreeks op een resource of resourcegroep kunnen worden geplaatst. Azure ondersteunt momenteel maximaal 15 tags per resource en de resourcegroep. Codes is geplaatst op een bron op het moment van het maken van of toegevoegd aan een bestaande bron. Houd er rekening mee dat tags worden ondersteund voor bronnen die zijn gemaakt via de Resource Manager alleen implementatiemodel. Als u labelen van een virtuele Linux machine wilt, Zie [een Linux virtuele machine in Azure labelen](virtual-machines-linux-tag.md).

[AZURE.INCLUDE [virtual-machines-common-tag](../../includes/virtual-machines-common-tag.md)]

## <a name="tagging-with-powershell"></a>Gelabeld met PowerShell

Maken, toevoegen en verwijderen van codes via PowerShell, moet u eerst uw [omgeving PowerShell Azure Resource Manager][]instellen. Als u de installatie hebt voltooid, kunt u de tags plaatsen Compute-, netwerk- en bronnen te maken, of nadat de bron is gemaakt via PowerShell. In dit artikel zal zich concentreren op weergeven/bewerken tags geplaatst op virtuele Machines.

Eerst, Ga naar een virtuele Machine via de `Get-AzureRmVM` cmdlet.

        PS C:\> Get-AzureRmVM -ResourceGroupName "MyResourceGroup" -Name "MyTestVM"

Als uw virtuele Machine al codes bevat, ziet u vervolgens alle codes van de bron:

        Tags : {
                "Application": "MyApp1",
                "Created By": "MyName",
                "Department": "MyDepartment",
                "Environment": "Production"
               }

Als u toevoegen van codes via PowerShell wilt, kunt u de `Set-AzureRmResource` opdracht. Houd er rekening mee bij het bijwerken van codes via PowerShell, labels als geheel worden bijgewerkt. Dus als u één code aan een resource die al van codes toevoegt, moet u opnemen de tags die u wilt worden geplaatst op de bron. Hieronder volgt een voorbeeld van extra labels toevoegen aan een bron via PowerShell-Cmdlets.

Deze eerste cmdlet alle codes op *MyTestVM* geplaatst aan de variabele *$tags* ingesteld met behulp van de `Get-AzureRmResource` en `Tags` eigenschap.

        PS C:\> $tags = (Get-AzureRmResource -ResourceGroupName MyResourceGroup -Name MyTestVM).Tags

De tweede opdracht geeft de labels voor de opgegeven variabele.

        PS C:\> $tags

        Name        Value
        ----                           -----
        Value       MyDepartment
        Name        Department
        Value       MyApp1
        Name        Application
        Value       MyName
        Name        Created By
        Value       Production
        Name        Environment

De derde opdracht wordt een extra code toegevoegd aan de variabele *$tags* . Let op het gebruik van de **+=** de nieuwe sleutel/waarde-paar toevoegen aan de lijst met *$tags* .

        PS C:\> $tags += @{Name="Location";Value="MyLocation"}

De vierde opdracht stelt alle codes die zijn gedefinieerd in de variabele *$tags* de gegeven resource. In dit geval is het MyTestVM.

        PS C:\> Set-AzureRmResource -ResourceGroupName MyResourceGroup -Name MyTestVM -ResourceType "Microsoft.Compute/VirtualMachines" -Tag $tags

De vijfde opdracht geeft alle codes van de bron. Zoals u ziet, wordt *locatie* nu gedefinieerd als een tag met *MyLocation* als de waarde.

        PS C:\> (Get-AzureRmResource -ResourceGroupName MyResourceGroup -Name MyTestVM).Tags

        Name        Value
        ----                           -----
        Value       MyDepartment
        Name        Department
        Value       MyApp1
        Name        Application
        Value       MyName
        Name        Created By
        Value       Production
        Name        Environment
        Value       MyLocation
        Name        Location

Bekijk voor meer informatie over codering via PowerShell, de [Resource-Cmdlets Azure][].

[AZURE.INCLUDE [virtual-machines-common-tag-usage](../../includes/virtual-machines-common-tag-usage.md)]

## <a name="next-steps"></a>Volgende stappen

* Zie voor meer informatie over het coderen van uw resources Azure, [Azure Resource Manager-overzicht][] en [Labels gebruiken voor het ordenen van uw Resources Azure][].
* Hoe labels kunt u uw gebruik van Azure resources beheren, Zie [Wat zijn uw factuur Azure][] en [inzicht in uw verbruik van Microsoft Azure krijgen][].

[PowerShell omgeving met Azure Resource Manager]: ../powershell-azure-resource-manager.md
[Azure Resource Cmdlets]: https://msdn.microsoft.com/library/azure/dn757692.aspx
[Azure Resource Manager-overzicht]: ../azure-resource-manager/resource-group-overview.md
[Met behulp van labels voor het ordenen van uw Resources Azure]: ../resource-group-using-tags.md
[Wat is uw Azure-rekening?]: ../billing/billing-understand-your-bill.md
[Inzicht in uw verbruik van Microsoft Azure krijgen]: ../billing-usage-rate-card-overview.md
