<properties
    pageTitle="Het formaat van een klassieke Windows VM | Microsoft Azure"
    description="Het formaat van een virtuele Windows-computer in het klassieke implementatiemodel met Azure Powershell gemaakt."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="Drewm3"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="na"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/19/2016"
    ms.author="drewm"/>


# <a name="resize-a-windows-vm-created-in-the-classic-deployment-model"></a>Het formaat van een Windows VM gemaakt in het implementatiemodel klassiek

In dit artikel wordt beschreven hoe u het formaat van een Windows VM, gemaakt in de klassieke implementatiemodel met Azure Powershell.

Wanneer u de mogelijkheid om het formaat van een VM, zijn er twee concepten die het bereik van de formaten die beschikbaar zijn voor de grootte van de virtuele machine. Het eerste concept is het gebied waarin uw VM wordt geïmplementeerd. De lijst van VM formaten beschikbaar in de regio is onder het tabblad Services van de webpagina Azure regio's. Het tweede concept is de fysieke hardware uw VM momenteel als host optreedt. De fysieke servers die als host fungeert voor VMs worden gegroepeerd in clusters van gemeenschappelijke fysieke hardware. De methode van het wijzigen van de grootte van een VM verschilt afhankelijk van als de gewenste grootte voor nieuwe VM wordt ondersteund door de hardware-cluster de VM momenteel als host optreedt.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]U kunt ook [het formaat van een VM gemaakt in het implementatiemodel Resource Manager](virtual-machines-windows-resize-vm.md).


## <a name="add-your-account"></a>Uw account toevoegen

Azure PowerShell om te werken met klassieke Azure bronnen, moet u configureren. De volgende stappen voor het configureren van Azure PowerShell klassieke bronnen beheren.

1. Bij de PowerShell-prompt, typ `Add-AzureAccount` en druk op **Enter**. 
2. Typ in het e-mailadres dat is gekoppeld aan uw abonnement Azure en klik op **Doorgaan**. 
3. Typ het wachtwoord voor uw account. 
4. Klik op **aanmelden**. 


## <a name="resize-in-the-same-hardware-cluster"></a>Formaat in het cluster met dezelfde hardware

Als u een VM tot een formaat dat beschikbaar is in de hardware-cluster waarop de VM, kunt u de volgende stappen uitvoeren.

1. Voer de volgende PowerShell-opdracht om een lijst van de VM-formaten beschikbaar zijn in de hardware-cluster host wolk waarin de VM.

    ```powershell
    Get-AzureService | where {$_.ServiceName -eq "<cloudServiceName>"}
    ```

2. Voer de volgende opdrachten om het formaat van de VM.

    ```powershell
    Get-AzureVM -ServiceName <cloudServiceName> -Name <vmName> | Set-AzureVMSize -InstanceSize <newVMSize> | Update-AzureVM
    ```

## <a name="resize-on-a-new-hardware-cluster"></a>Formaat van een cluster met nieuwe hardware

Het formaat van een VM op een grootte die niet beschikbaar zijn in de hardware cluster VM de cloud hosting service en alle VMs in de cloud-service moeten opnieuw worden gemaakt. Elke cloud-service wordt gehost op een cluster met één hardware zodat alle VMs in de cloud-service moet een grootte die op een cluster hardware wordt ondersteund. De volgende stappen wordt beschreven hoe het formaat van een VM door te verwijderen en opnieuw maken van de cloud-service.

1. Voer de volgende opdracht PowerShell om de VM te selecteren in de regio. 

    ```powershell
    Get-AzureLocation | where {$_.Name -eq "<locationName>"}
    ```

2. Noteer alle configuratie-instellingen voor elke VM in de cloud-service bevat de VM worden aangepast. 
3. Verwijder alle VMs in de cloud-service als u de optie om de schijven te bewaren voor elke VM.
4. Maak opnieuw de VM worden aangepast met behulp van de gewenste grootte van de VM.
5. Andere VMs die waren in de cloud-service met behulp van een VM-formaat beschikbaar zijn in de hardware cluster hosting nu de cloud-service opnieuw.

Een voorbeeld van een script voor het verwijderen en opnieuw maken van een cloud-service met behulp van een nieuwe VM-grootte vindt u [hier](https://github.com/Azure/azure-vm-scripts). 


## <a name="next-steps"></a>Volgende stappen

- [Het formaat van een VM gemaakt in het implementatiemodel Resource Manager](virtual-machines-windows-resize-vm.md).