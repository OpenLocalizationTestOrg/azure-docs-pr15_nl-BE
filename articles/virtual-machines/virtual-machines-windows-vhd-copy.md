<properties
    pageTitle="Een kopie maken van een gespecialiseerde VM in Azure | Microsoft Azure"
    description="Informatie over het maken van een kopie van een speciale Windows VM in Azure, in het implementatiemodel Resource Manager wordt uitgevoerd."
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
    ms.date="10/20/2016"
    ms.author="cynthn"/>
    
    
    
# <a name="create-a-copy-of-a-specialized-windows-vm-running-in-azure"></a>Een kopie maken van een speciale Windows VM in Azure uitgevoerd 

In dit artikel wordt beschreven hoe u het hulpprogramma AZCopy gebruiken voor het maken van een kopie van de VHD vanuit een speciale Windows VM in Azure met. Vervolgens kunt u de kopie van de VHD maken van een nieuwe VM. 

- Als wilt kopiëren van een algemene VM, Zie [het maken van een afbeelding een bestaande algemene Azure VM VM](virtual-machines-windows-capture-image.md).

- Als u een VHD vanuit een VM op ruimten, zoals een gemaakt met behulp van Hyper-V, het [uploaden van een VHD Windows in een VM op ruimten naar Azure](virtual-machines-windows-upload-image.md)Zie uploaden wilt.


## <a name="before-you-begin"></a>Voordat u begint

Zorg ervoor dat u:

- Informatie over de **bron- en doeladressen opslag accounts**hebben. Voor de bron VM moet u de namen en de container opslag. De containernaam van de worden meestal **VHD's**. U moet ook een bestemming opslag-account hebt. Als u dit niet al hebt, kunt u met de beide portal (**Meer Services** > opslag rekeningen > toevoegen) of met de cmdlet [New-AzureRmStorageAccount](https://msdn.microsoft.com/library/mt607148.aspx) . 

- Azure [PowerShell 1.0](../powershell-install-configure.md) is (of hoger) geïnstalleerd.

- Hebt gedownload en geïnstalleerd het [programma AzCopy](../storage/storage-use-azcopy.md). 


## <a name="deallocate-the-vm"></a>Toewijzing van de VM

Deallocate VM, de VHD maakt te kopiëren. 

- **Portal**: klik op **virtuele machines** > **myVM** > stoppen
- **PowerShell**: `Stop-AzureRmVM -ResourceGroupName myResourceGroup -Name myVM` deallocates met de naam **myVM** in de resource groep **myResourceGroup**VM.

De **Status** van de VM in Azure portal verandert **gestopt** **gestopt (opgeheven)**.


## <a name="get-the-storage-account-urls"></a>De opslag account URL's ophalen

U moet de URL van de bron- en doeladressen opslag rekeningen. De URL's eruit: `https://<storageaccount>.blob.core.windows.net/<containerName>/`. Als u de naam van de opslag en de container al weet, kunt u alleen de informatie tussen de haken voor het maken van uw URL vervangen. 

U kunt de Azure portal of Azure Powershell ophalen van de URL:

- **Portal**: klik op **meer services** > **opslag rekeningen**  >  <storage account> **BLOB's** en de VHD bestand is waarschijnlijk in de container **VHD's** . Klik op **Eigenschappen** voor de container en de tekst met de naam **URL**kopiëren. U moet de URL van de bron- en doel-containers. 

- **PowerShell**: `Get-AzureRmVM -ResourceGroupName "myResourceGroup" -Name "myVM"` wordt de informatie voor VM met de naam **myVM** in de resource groep **myResourceGroup**. Zoek in de resultaten van de in de sectie **opslag profiel** voor de **Vhd-Uri**. Het eerste deel van de Uri is de URL van de container en het laatste deel is de naam van de OS VHD voor VM.

## <a name="get-the-storage-access-keys"></a>De toegangstoetsen opslag ophalen

De toegangstoetsen voor de bron- en doeladressen accounts opslag vinden. Zie voor meer informatie over toegangstoetsen [over Azure opslag rekeningen](../storage/storage-create-storage-account.md).

- **Portal**: klik op **meer services** > **opslag rekeningen**  >  <storage account> **Alle instellingen** > **toegangstoetsen**. De sleutel is aangeduid als **key1**kopiëren.
- **PowerShell**: `Get-AzureRmStorageAccountKey -Name mystorageaccount -ResourceGroupName myResourceGroup` krijgt de sleutel van de opslag voor de opslag rekening **mystorageaccount** in de resource groep **myResourceGroup**. De sleutel is aangeduid als **key1**kopiëren.


## <a name="copy-the-vhd"></a>Kopieer de VHD 

U kunt bestanden kopiëren tussen opslag accounts met AzCopy. Voor de doelcontainer als de opgegeven container niet bestaat, zal deze worden gemaakt. 

AzCopy, open een opdrachtprompt op de lokale computer en Ga naar de map waarin AzCopy is geïnstalleerd. Dit is vergelijkbaar met *C:\Program Files (x86) \Microsoft SDKs\Azure\AzCopy*. 

Alle bestanden in de container wilt kopiëren, moet u de schakeloptie **/S** gebruikt. Dit kan worden gebruikt voor het kopiëren van de VHD OS en alle gegevensschijven als deze zich in dezelfde container. In dit voorbeeld ziet hoe u alle bestanden in de container **mysourcecontainer** in opslag account **mysourcestorageaccount** te kopiëren naar de container **mydestinationcontainer** in de opslag **mydestinationstorageaccount** account. De namen van de accounts van opslag en containers vervangen door uw eigen. Vervangen `<sourceStorageAccountKey1>` en `<destinationStorageAccountKey1>` met uw eigen sleutels.

```
    AzCopy /Source:https://mysourcestorageaccount.blob.core.windows.net/mysourcecontainer `
        /Dest:https://mydestinationatorageaccount.blob.core.windows.net/mydestinationcontainer `
        /SourceKey:<sourceStorageAccountKey1> /DestKey:<destinationStorageAccountKey1> /S
```

Als u alleen een specifieke VHD in een container met meerdere bestanden kopiëren, kunt u ook de naam van het bestand met de schakeloptie /Pattern. In dit voorbeeld wordt alleen het bestand met de naam **myFileName.vhd** gekopieerd.

```
    AzCopy /Source:https://mysourcestorageaccount.blob.core.windows.net/mysourcecontainer `
        /Dest:https://mydestinationatorageaccount.blob.core.windows.net/mydestinationcontainer `
        /SourceKey:<sourceStorageAccountKey1> /DestKey:<destinationStorageAccountKey1> `
        /Pattern:myFileName.vhd
```


Wanneer deze is voltooid, ontvangt u een bericht dat er ongeveer als volgt:

```
  Finished 2 of total 2 file(s).
  [2016/10/07 17:37:41] Transfer summary:
  -----------------
  Total files transferred: 2
  Transfer successfully:   2
  Transfer skipped:        0
  Transfer failed:         0
  Elapsed time:            00.00:13:07
```

## <a name="troubleshooting"></a>Het oplossen van problemen

- Wanneer u de AZCopy, gebruikt als u het foutbericht 'Server kan niet worden geverifieerd verzoek. Zorg ervoor dat de waarde van verificatieheader goed met inbegrip van de handtekening wordt gevormd." en u gebruikt de sleutel 2 of voor de secundaire opslag, probeert u het met de opslag van primaire of 1e sleutel.


## <a name="next-steps"></a>Volgende stappen

- U kunt een nieuwe VM maken door [de kopie van de VHD naar een VM als een OS schijf koppelen](virtual-machines-windows-create-vm-specialized.md).












