<properties
   pageTitle="Beheren van uw virtuele machines met Azure PowerShell | Microsoft Azure"
   description="Informatie over de opdrachten die u gebruiken kunt voor het automatiseren van taken in het beheer van uw virtuele machines."
   services="virtual-machines-windows"
   documentationCenter="windows"
   authors="singhkays"
   manager="timlt"
   editor=""
   tags="azure-service-management"/>

   <tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="10/12/2016"
   ms.author="kasing"/>

# <a name="manage-your-virtual-machines-by-using-azure-powershell"></a>De virtuele machines met Azure PowerShell beheren

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


Veel taken die u elke dag voor het beheren van uw VMs kunnen worden geautomatiseerd met Azure PowerShell-cmdlets. In dit artikel kunt u voorbeeld opdrachten voor eenvoudigere taken en koppelingen naar artikelen waarin de opdrachten voor complexe taken.

>[AZURE.NOTE] Als u nog niet hebt geïnstalleerd en geconfigureerd Azure PowerShell nog, krijgt u instructies in het artikel in [het installeren en configureren van Azure PowerShell](../powershell-install-configure.md).

## <a name="how-to-use-the-example-commands"></a>Het gebruik van de voorbeeld-opdrachten
U moet een deel van de tekst in de opdrachten worden vervangen door tekst die geschikt is voor uw omgeving. De < en > symbolen geven aan de tekst die u wilt vervangen. Wanneer u de tekst wilt vervangen, verwijdert u de symbolen, maar laat de aanhalingstekens in plaats.

## <a name="get-a-vm"></a>Krijg een VM
Dit is een eenvoudige taak die u vaak gebruikt. Deze gebruiken om informatie over een VM, taken uitvoeren op een VM of u uitvoer op te slaan in een variabele.

Als u informatie over de VM, uitvoeren van deze opdracht, vervangt alle aspecten van de offertes, met inbegrip van de < en > tekens:

     Get-AzureVM -ServiceName "<cloud service name>" -Name "<virtual machine name>"

Als u wilt het resultaat opslaan in een variabele $vm, worden uitgevoerd:

    $vm = Get-AzureVM -ServiceName "<cloud service name>" -Name "<virtual machine name>"

## <a name="log-on-to-a-windows-based-vm"></a>Meld u aan bij een Windows-gebaseerde VM

Deze opdrachten uitvoeren:

>[AZURE.NOTE] De naam van de virtuele machine en cloud service krijgt u van de weergave van de opdracht **Get-AzureVM** .
>
    $svcName = "<cloud service name>"
    $vmName = "<virtual machine name>"
    $localPath = "<drive and folder location to store the downloaded RDP file, example: c:\temp >"
    $localFile = $localPath + "\" + $vmname + ".rdp"
    Get-AzureRemoteDesktopFile -ServiceName $svcName -Name $vmName -LocalPath $localFile -Launch

## <a name="stop-a-vm"></a>Stop een VM

Deze opdracht uitvoeren:

    Stop-AzureVM -ServiceName "<cloud service name>" -Name "<virtual machine name>"

>[AZURE.IMPORTANT]Deze parameter gebruiken om de virtuele IP-(VIP) van de cloud-service voor het geval is het de laatste VM in die service cloud. <br><br> Als u de parameter StayProvisioned, zult u nog steeds worden gebruikt voor de VM.

## <a name="start-a-vm"></a>Start een VM

Deze opdracht uitvoeren:

    Start-AzureVM -ServiceName "<cloud service name>" -Name "<virtual machine name>"

## <a name="attach-a-data-disk"></a>Een schijf met gegevens koppelen
Deze taak vereist een paar stappen. Gebruik eerst de *** toevoegen-cmdlet AzureDataDisk *** aan de schijf toevoegen aan het $vm-object. Vervolgens gebruikt u de cmdlet **Update-AzureVM** voor het bijwerken van de configuratie van de VM.

Ook moet u beslissen of u wilt toevoegen van een nieuwe schijf of die gegevens bevat. Voor een nieuwe schijf de opdracht het VHD-bestand wordt gemaakt en gekoppeld.

Als u wilt een nieuwe schijf koppelt, kunt u deze opdracht uitvoert:

    Add-AzureDataDisk -CreateNew -DiskSizeInGB 128 -DiskLabel "<main>" -LUN <0> -VM $vm | Update-AzureVM

Als u wilt koppelen een schijf met bestaande gegevens, kunt u deze opdracht uitvoert:

    Add-AzureDataDisk -Import -DiskName "<MyExistingDisk>" -LUN <0> | Update-AzureVM

Om gegevensschijven uit een bestaande VHD-bestand in een blob-opslag koppelt, kunt u deze opdracht uitvoert:

    Add-AzureDataDisk -ImportFrom -MediaLocation `
              "<https://mystorage.blob.core.windows.net/mycontainer/MyExistingDisk.vhd>" `
              -DiskLabel "<main>" -LUN <0> |
              Update-AzureVM

## <a name="create-a-windows-based-vm"></a>Maak een Windows-gebaseerde VM

U maakt een nieuwe virtuele machine onder Windows in Azure, gebruik de instructies in [Azure PowerShell gebruiken voor het maken en vooraf configureren van Windows-gebaseerde virtuele machines](virtual-machines-windows-classic-create-powershell.md). Dit onderwerp stappen maakt u stapsgewijs bij het maken van een Azure PowerShell opdracht ingesteld dat een op Windows gebaseerde VM die kan vooraf worden geconfigureerd:

- Met het lidmaatschap van Active Directory-domein.
- Met extra schijven.
- Als lid van een bestaande taakverdeling instellen.
- Met een statisch IP-adres.
