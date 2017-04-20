<properties
    pageTitle="Veelgestelde vragen over Stack Azure | Microsoft Azure"
    description="Azure Stack Veelgestelde vragen."
    services="azure-stack"
    documentationCenter=""
    authors="HeathL17"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/13/2016"
    ms.author="helaw"/>

# <a name="frequently-asked-questions-for-azure-stack"></a>Veelgestelde vragen over Stack Azure

## <a name="deployment"></a>Implementatie

### <a name="do-i-need-to-format-my-data-disks-before-starting-or-restarting-an-installation"></a>Moet ik mijn gegevensschijven opmaken voordat starten of een installatie opnieuw starten?

Schijven moeten worden in de indeling raw. Als u het besturingssysteem opnieuw installeert, moet u controleren of de opslaggroep met oude nog steeds aanwezig is en verwijderen met behulp van de volgende stappen uit:

1. Open Serverbeheer.
2. Selecteer opslagpools.
3. Zie het als een groep wordt weergegeven.
4. Met de rechtermuisknop op de **groep** als vermeld en, lezen inschakelen / schrijven.
5. Klik met de rechtermuisknop op **virtuele harde schijf** (linksonder) en selecteert u verwijderen.
6. Met de rechtermuisknop op de **Groep** en klik op verwijderen.
7. Azure Stack script opnieuw starten en controleer of dat de schijf controle wordt doorgegeven.

Eventueel kan het volgende script worden gebruikt:

```PowerShell
$pools = Get-StoragePool -IsPrimordial $False -ErrorVariable Err -ErrorAction SilentlyContinue
if ($pools -ne $null) {
  $pools| Set-StoragePool -IsReadOnly $False -ErrorVariable Err -ErrorAction SilentlyContinue
  $pools = Get-StoragePool -IsPrimordial $False -ErrorVariable Err -ErrorAction SilentlyContinue
  $pools | Get-VirtualDisk | Remove-VirtualDisk -Confirm:$False
  $pools | Remove-StoragePool -Confirm:$False
}
```

### <a name="can-i-use-all-ssd-disks-for-the-storage-pool-in-the-poc-installation"></a>Kan ik alle SSD-schijven gebruiken voor de opslag van toepassingen in de Haalbaarheidstest installatie?

Deze configuratie wordt niet ondersteund in deze release.  Zie voor meer informatie de [handleiding eisen](azure-stack-deploy.md) voor meer informatie.

### <a name="can-i-use-nvme-data-disks-for-the-microsoft-azure-stack-poc"></a>Kan ik voor de Microsoft Azure Stack Implementatiemodel gegevensschijven NVMe gebruiken?

Terwijl opslag spaties directe NVMe-schijven ondersteunt, ondersteunt Azure Stack alleen een subset van de mogelijke stationstypen en mogelijke combinaties voor opslag spaties Direct. 

### <a name="how-can-i-reinstall-azure-stack"></a>Hoe kan ik Azure Stack opnieuw installeren?
U kunt de stappen in de [handleiding Microsoft CRM opnieuw installeren](azure-stack-redeploy.md).  

## <a name="tenant"></a>Huurder

### <a name="can-i-deploy-my-own-images-as-a-tenant"></a>Kan ik mijn eigen afbeeldingen als een huurder implementeren?

Ja, net als in Azure, een huurder kunt uploaden afbeeldingen in Azure Stack, naast het gebruik van de beelden van de servicebeheerder. Zie voor een overzicht, het [toevoegen van een afbeelding VM](azure-stack-add-vm-image.md). 

## <a name="testing"></a>Testen

### <a name="can-i-use-nested-virtualization-to-test-the-microsoft-azure-stack-poc"></a>Kan ik geneste virtualisatie gebruiken voor het testen van de Microsoft Azure Stack Implementatiemodel?

Geneste virtualisatie wordt niet ondersteund of getest met Azure Stack technische Preview 2.

## <a name="virtual-machines"></a>Virtuele machines

### <a name="does-azure-stack-support-dynamic-disks-for-virtual-machines"></a>Ondersteunt de Stack Azure dynamische schijven voor virtuele machines?

Azure-Stack biedt geen ondersteuning voor dynamische schijven.

## <a name="next-steps"></a>Volgende stappen

[Het oplossen van problemen](azure-stack-troubleshooting.md)
