<properties
    pageTitle="Fouten oplossen wanneer u accounts Azure opslag, containers of VHD's in de implementatie van een Resource Manager verwijderen | Microsoft Azure"
    description="Fouten oplossen wanneer u accounts Azure opslag, containers of VHD's in een implementatie Resource Manager verwijderen"
    services="storage"
    documentationCenter=""
    authors="genlin"
    manager="felixwu"
    editor="na"
    tags="storage"/>

<tags
    ms.service="storage"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/17/2016"
    ms.author="genli"/>

# <a name="troubleshoot-errors-when-you-delete-azure-storage-accounts-containers-or-vhds-in-a-resource-manager-deployment"></a>Fouten oplossen wanneer u accounts Azure opslag, containers of VHD's in een implementatie Resource Manager verwijderen

[AZURE.INCLUDE [storage-selector-cannot-delete-storage-account-container-vhd](../../includes/storage-selector-cannot-delete-storage-account-container-vhd.md)]

Er kunnen fouten optreden wanneer u probeert te verwijderen van een account Azure opslag, de recipiënt of de virtuele harde schijf (VHD) in [Azure portal](https://portal.azure.com). Dit artikel bevat richtlijnen voor het oplossen van problemen helpen bij het oplossen van het probleem in een bronnenbeheerder Azure-implementatie.

In dit artikel niet van toepassing de Azure probleem, gaat u naar de Azure forums op [MSDN en Stack Overflow](https://azure.microsoft.com/support/forums/). U kunt het probleem op deze forums of boeken @AzureSupport op Twitter. U kunt ook een Azure Ondersteuningsverzoek indienen door **ondersteuning** op de site [ondersteuning van Azure](https://azure.microsoft.com/support/options/) .

## <a name="symptoms"></a>Symptomen

### <a name="scenario-1"></a>Scenario 1

Wanneer u een VHD in een rekening voor opslag in een implementatie Resource Manager te verwijderen probeert, wordt het volgende foutbericht weergegeven:

**Blob 'vhds/BlobName.vhd' te verwijderen is mislukt. Fout: Er is momenteel een lease-overeenkomst op de blob en geen lease-ID is opgegeven in de aanvraag.**

Dit probleem kan optreden omdat een virtuele machine (VM) een lease-overeenkomst op de VHD die u probeert is te verwijderen.

### <a name="scenario-2"></a>Scenario 2

Wanneer u een container op een rekening voor opslag in een implementatie Resource Manager, wordt het volgende foutbericht weergegeven:

**Opslag container 'VHD's ' te verwijderen is mislukt. Fout: Er is momenteel een lease-overeenkomst voor de container en geen lease-ID is opgegeven in de aanvraag.**

Dit probleem kan optreden omdat de container heeft een VHD die in de staat van de lease-overeenkomst is vergrendeld.

### <a name="scenario-3"></a>Scenario 3

Wanneer u een account voor opslag in een implementatie Resource Manager te verwijderen, wordt het volgende foutbericht weergegeven:

**Opslag account 'StorageAccountName' te verwijderen is mislukt. Fout: De account opslag kan niet worden verwijderd als gevolg van de artefacten die in gebruik.**

Dit probleem kan optreden omdat de opslag-account bevat een VHD met de status van de lease-overeenkomst.

## <a name="solution"></a>Oplossing

Om deze problemen oplossen, moet u de VHD die de fout veroorzaakt en de bijbehorende VM identificeren. Vervolgens los van de VHD van de VM (voor gegevensschijven) of de VM die van de VHD (voor OS schijven gebruikmaakt) verwijderen. Hiermee verwijdert de lease van de VHD en kan worden verwijderd.

### <a name="step-1-identify-the-problem-vhd-and-the-associated-vm"></a>Stap 1: Bepaal het probleem VHD en de bijbehorende VM


1. Log in om de [Azure portal](https://portal.azure.com).
2. Selecteer **alle resources**in het menu **Hub** . Ga naar de opslag-account die u wilt verwijderen en selecteer vervolgens de **BLOB's** > **VHD's**.

    ![Schermafbeelding van de portal met de opslag en de container 'VHD's ' is geselecteerd](./media/storage-resource-manager-cannot-delete-storage-account-container-vhd/opencontainer.png)

3. Controleer de eigenschappen van elk VHD in de container. Zoek de VHD in de **huurlijnen** staat. Vervolgens bepalen welke VM maakt gebruik van de VHD. Meestal kunt u bepalen welk VM de VHD bevat aan de hand van de naam van de VHD:

    - In het algemeen volgen deze naamgevingsconventie OS schijven: VMNameYYYYMMDDHHMMSS.vhd
    - In het algemeen volgen deze naamgevingsconventie gegevensschijven: JJJJMMDD-VMName-HHMMSS.vhd

    ![Schermafbeelding van de gegevens in de portal van de container met de naam van de VM, de status van de lease van "Vergrendeld" en de status van de lease van "Huurlijnen" gemarkeerd](./media/storage-resource-manager-cannot-delete-storage-account-container-vhd/locatevm.png)

### <a name="step-2-remove-the-lease-from-the-vhd"></a>Stap 2: De lease van de VHD verwijderen

De VM die van de VHD (voor OS schijven gebruikmaakt) verwijderen:

1.  Log in om de [Azure portal](https://portal.azure.com).
2.  Selecteer in het menu **Hub** **virtuele Machines**.
3.  Selecteer de VM die in het bezit van een lease-overeenkomst op de VHD.
4.  Zorg ervoor dat er niets is actief met behulp van de virtuele machine, en dat u niet langer de virtuele machine.
5.  Aan de bovenkant van het blad **VM details** selecteert u **verwijderen**en klik vervolgens op **Ja** om te bevestigen.
6.  De VM moet worden verwijderd, maar de VHD dient te worden gehandhaafd. De VHD moet niet langer een lease-overeenkomst hebben erop. Het kan enkele minuten duren voordat de lease-overeenkomst moet worden vrijgegeven. Om te controleren dat de lease-overeenkomst is vrijgegeven, gaat u naar **alle resources** > **Opslag accountnaam** > **BLOB's** > **VHD's**. In het deelvenster **Eigenschappen Blob** moet de **Status van de lease-overeenkomst** worden **ontgrendeld**.

Loskoppelen van de VHD van de VM die wordt gebruikt (voor gegevensschijven):

1.  Log in om de [Azure portal](https://portal.azure.com).
2.  Selecteer in het menu **Hub** **virtuele Machines**.
3.  Selecteer de VM die in het bezit van een lease-overeenkomst op de VHD.
4.  Selecteer de **schijven** op de bladeserver **VM details** .
5.  Selecteer de gegevensschijf die in het bezit van een lease-overeenkomst op de VHD. U kunt bepalen welke VHD is gekoppeld de schijf aan de hand van de URL van de VHD.
6.  Met zekerheid dat er niets is actief met behulp van de gegevens bepalen.
7.  Klik op **verbreken** op de **schijf gegevens** blade.
8.  De schijf moet nu worden losgekoppeld van de VM, en de VHD moet een lease-overeenkomst niet meer op deze. Het kan enkele minuten duren voordat de lease-overeenkomst moet worden vrijgegeven. Om te controleren dat de lease-overeenkomst is vrijgegeven, gaat u naar **alle resources** > **Opslag accountnaam** > **BLOB's** > **VHD's**. In het deelvenster **Eigenschappen Blob** moet de **Status van de lease-overeenkomst** worden **ontgrendeld**.

## <a name="what-is-a-lease"></a>Wat is een lease-overeenkomst?

Een lease is een vergrendeling die kan worden gebruikt voor toegang tot een blob (bijvoorbeeld een VHD). Wanneer een blob wordt geleasd, kan alleen de eigenaars van de lease-overeenkomst toegang krijgen tot de blob. Een lease is het belangrijk om de volgende redenen:

-   Als er meerdere eigenaren probeert te schrijven naar hetzelfde gedeelte van de blob tegelijkertijd kunnen gegevens beschadigd raken.
-   Het voorkomt dat de blob wordt verwijderd als er iets actief gebruikt wordt (bijvoorbeeld een VM).
-   Het voorkomt dat de opslag-account wordt verwijderd als er iets actief gebruikt wordt (bijvoorbeeld een VM).



## <a name="next-steps"></a>Volgende stappen

- [Een opslag-account verwijderen](storage-create-storage-account.md#delete-a-storage-account)
- [Hoe u de vergrendelde lease van blob-opslag in Microsoft Azure (PowerShell)](https://gallery.technet.microsoft.com/scriptcenter/How-to-break-the-locked-c2cd6492)
