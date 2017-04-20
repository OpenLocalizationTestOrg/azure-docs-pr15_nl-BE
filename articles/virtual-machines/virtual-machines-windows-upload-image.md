<properties
    pageTitle="Een Windows VHD voor Resource Manager uploaden | Microsoft Azure"
    description="Informatie voor het uploaden van een virtuele Windows-computer VHD in ruimten met Azure, het implementatiemodel Resource Manager gebruiken. U kunt een VHD van hetzij een algemene of een gespecialiseerde VM uploaden."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor="tysonn"
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="cynthn"/>

# <a name="upload-a-windows-vhd-from-an-on-premises-vm-to-azure"></a>Een Windows VHD vanuit een on-premises VM naar Azure uploaden 


In dit artikel wordt beschreven hoe u voor het maken en uploaden van een Windows virtuele harde schijf (VHD) moet worden gebruikt bij het maken van een Azure Vm. U kunt een VHD vanuit een gegeneraliseerde VM of een gespecialiseerde VM uploaden. 

Zie voor meer informatie over schijven en VHD in Azure [over schijven en VHD's voor virtuele machines](virtual-machines-linux-about-disks-vhds.md).


## <a name="prepare-the-vm"></a>Voorbereiden van de VM 

U kunt zowel algemene als gespecialiseerde VHD's uploaden naar Azure. Elk type, moet u de VM voorbereiden voordat u begint.

- **Generalized VHD** - een gegeneraliseerde VHD heeft al uw persoonlijke accountgegevens verwijderd met behulp van Sysprep. Als u van plan bent de VHD als een afbeelding met nieuwe VMs uit maakt, moet u:
    - [Voorbereiden van een Windows-VHD te uploaden naar Azure](virtual-machines-windows-prepare-for-upload-vhd-image.md). 
    - [Generaliseren de virtuele machine met behulp van Sysprep](virtual-machines-windows-generalize-vhd.md). 

- **Gespecialiseerde VHD** - een gespecialiseerde VHD onderhoudt de gebruikersaccounts, toepassingen en andere gegevens uit uw oorspronkelijke VM. Als u wilt gebruiken de VHD als-is het maken van een nieuwe VM, controleer dan de volgende stappen zijn voltooid. 
    - [Voorbereiden van een Windows-VHD te uploaden naar Azure](virtual-machines-windows-prepare-for-upload-vhd-image.md). **Niet** generaliseren VM met behulp van Sysprep.
    - Verwijder de Gast virtualisatie gereedschappen en agenten die zijn geïnstalleerd op de VM (dat wil zeggen VMware tools).
    - Controleer dat de VM om op te halen van het IP-adres en DNS-instellingen via DHCP is geconfigureerd. Dit zorgt ervoor dat de server een IP-adres binnen de VNet verkrijgt wanneer deze wordt gestart. 

## <a name="log-in-to-azure"></a>Log in op Azure

Als u versie 1.4 of hoger is geïnstalleerd, Lees [het installeren en configureren van Azure PowerShell](../powershell-install-configure.md)PowerShell nog geen hebt.

1. Azure PowerShell openen en inloggen bij uw account Azure. Een pop-upvenster opent u uw Azure accountreferenties opgeven.

    ```powershell
    Login-AzureRmAccount
    ```


2. De abonnement-id's ophalen voor uw abonnementen beschikbaar.

    ```powershell
    Get-AzureRmSubscription
    ```

3. Instellen van het juiste abonnement met de abonnement-ID. Vervangen `<subscriptionID>` met de ID van het juiste abonnement.

    ```powershell
    Select-AzureRmSubscription -SubscriptionId "<subscriptionID>"
    ```

## <a name="get-the-storage-account"></a>De opslag-account aanvragen

U moet een account voor opslag in Azure voor het opslaan van het geüploade image voor VM. Gebruik een bestaande account voor opslag of een nieuwe maken. 

Als u wilt weergeven op de rekeningen beschikbare opslagruimte, typt u:

```powershell
Get-AzureRmStorageAccount
```

Als u een bestaande opslag gebruikt, gaat u verder met de sectie [de VM afbeelding uploaden](#upload-the-vm-vhd-to-your-storage-account) .

Als u een account opslag maken, als volgt te werk:

1. U moet de naam van de resourcegroep waarop de opslag-account moet worden gemaakt. Als u wilt weten wat de resourcegroepen in uw abonnement, typt u:

    ```powershell
    Get-AzureRmResourceGroup
    ```

Een resourcegroep maken met de naam **myResourceGroup** in de regio **West VS** , type:

    ```powershell
    New-AzureRmResourceGroup -Name myResourceGroup -Location "West US"
    ```

2. Een opslag-account met de naam **mystorageaccount** in deze resourcegroep met behulp van de cmdlet [New-AzureRmStorageAccount](https://msdn.microsoft.com/library/mt607148.aspx) maken:

    ```powershell
    New-AzureRmStorageAccount -ResourceGroupName myResourceGroup -Name mystorageaccount -Location "West US" -SkuName "Standard_LRS" -Kind "Storage"
    ```
            
    Geldige waarden voor - SkuName zijn:

    - **Standard_LRS** - lokaal redundante opslag. 
    - **Standard_ZRS** - Zone redundante opslag.
    - **Standard_GRS** - Geo-redundante opslag. 
    - **Standard_RAGRS** - leestoegang geo-redundante opslag. 
    - **Premium_LRS** - Premium lokaal redundante opslag. 



## <a name="upload-the-vhd-to-your-storage-account"></a>De VHD uploaden naar je account opslag

Gebruik de cmdlet [Add-AzureRmVhd](https://msdn.microsoft.com/library/mt603554.aspx) voor het uploaden van de afbeelding naar een container in uw account voor opslag. In dit voorbeeld uploadt u het bestand **myVHD.vhd** uit `"C:\Users\Public\Documents\Virtual hard disks\"` een opslag account met de naam **mystorageaccount** in de resourcegroep van de **myResourceGroup** . Het bestand wordt geplaatst in de container met de naam **mycontainer** en de nieuwe bestandsnaam is **myUploadedVHD.vhd**.

```powershell
$rgName = "myResourceGroup"
$urlOfUploadedImageVhd = "https://mystorageaccount.blob.core.windows.net/mycontainer/myUploadedVHD.vhd"
Add-AzureRmVhd -ResourceGroupName $rgName -Destination $urlOfUploadedImageVhd -LocalFilePath "C:\Users\Public\Documents\Virtual hard disks\myVHD.vhd"
```


Als dit lukt, krijgt u een antwoord dat er ongeveer zo uitziet:

```
  C:\> Add-AzureRmVhd -ResourceGroupName myResourceGroup -Destination https://mystorageaccount.blob.core.windows.net/mycontainer/myUploadedVHD.vhd -LocalFilePath "C:\Users\Public\Documents\Virtual hard disks\myVHD.vhd"
  MD5 hash is being calculated for the file C:\Users\Public\Documents\Virtual hard disks\myVHD.vhd.
  MD5 hash calculation is completed.
  Elapsed time for the operation: 00:03:35
  Creating new page blob of size 53687091712...
  Elapsed time for upload: 01:12:49

  LocalFilePath           DestinationUri
  -------------           --------------
  C:\Users\Public\Doc...  https://mystorageaccount.blob.core.windows.net/mycontainer/myUploadedVHD.vhd
```

Afhankelijk van uw netwerkverbinding en de grootte van het VHD-bestand, deze opdracht kan even duren om te voltooien


## <a name="next-steps"></a>Volgende stappen

- [Een VM in Azure een gegeneraliseerde VHD maken](virtual-machines-windows-create-vm-generalized.md)
- [Een VM in Azure een gespecialiseerde VHD maken](virtual-machines-windows-create-vm-specialized.md) door te koppelen als een OS schijf bij het maken van een nieuwe VM.


