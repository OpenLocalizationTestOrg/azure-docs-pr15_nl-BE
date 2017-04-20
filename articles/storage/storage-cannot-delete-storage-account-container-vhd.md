<properties
    pageTitle="Problemen met Azure opslag rekeningen, in containers of VHD's in een klassieke implementatie verwijderen | Microsoft Azure"
    description="Problemen oplossen met Azure opslag rekeningen, in containers of VHD's in een klassieke installatie verwijderen"
    services="storage"
    documentationCenter=""
    authors="genlin"
    manager="felixwu"
    editor="tysonn"
    tags="storage"/>

<tags
    ms.service="storage"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="genli"/>

# <a name="troubleshoot-deleting-azure-storage-accounts-containers-or-vhds-in-a-classic-deployment"></a>Problemen oplossen met Azure opslag rekeningen, in containers of VHD's in een klassieke installatie verwijderen

[AZURE.INCLUDE [storage-selector-cannot-delete-storage-account-container-vhd](../../includes/storage-selector-cannot-delete-storage-account-container-vhd.md)]

Er kunnen fouten optreden wanneer u probeert te verwijderen van de account Azure opslag, de recipiënt of de VHD in de [Azure portal](https://portal.azure.com/) of de [Azure klassieke portal](https://manage.windowsazure.com/). De problemen kunnen worden veroorzaakt door de volgende omstandigheden:

-   Wanneer u een VM verwijdert, worden de schijf en VHD niet automatisch verwijderd. Is dit mogelijk de oorzaak van de fout van opslag account verwijderen. Verwijder de schijf niet wij zodat kunt u de schijf wilt koppelen op een andere VM.

-   Er is nog steeds een lease op een schijf of de label die is gekoppeld aan de schijf.

Als uw Azure niet in dit artikel besproken wordt, gaat u naar de Azure forums op [MSDN en de Stack Overflow](https://azure.microsoft.com/support/forums/). U kunt het probleem op deze forums of boeken @AzureSupport op Twitter. U kunt ook een Azure Ondersteuningsverzoek indienen door **ondersteuning** op de site [ondersteuning van Azure](https://azure.microsoft.com/support/options/) .

## <a name="symptoms"></a>Symptomen

De volgende sectie worden veelvoorkomende fouten die verschijnen kunnen wanneer u probeert de Azure opslag rekeningen, containers of VHD's verwijderen.

### <a name="scenario-1-unable-to-delete-a-storage-account"></a>Scenario 1: Kan niet verwijderen van een account voor opslag

Wanneer u naar de rekening van de opslag in de [Azure portal](https://portal.azure.com/) of [Azure klassieke portal](https://manage.windowsazure.com/) en selecteer **verwijderen navigeert**, ziet u het volgende foutbericht weergegeven:

*Opslag account StorageAccountName bevat VM afbeeldingen. Controleer dat deze VM-afbeeldingen worden verwijderd voordat u deze opslag account verwijdert.*

Deze fout kan ook worden weergegeven:

**Op Azure portal**:

*Opslag account < vm-opslag-account-naam > verwijderen is mislukt. Kan geen opslag account < vm-opslag-account-naam > verwijderen: ' opslag account < vm-opslag-account-name > heeft enkele actieve afbeelding(en) en/of schijven. Deze afbeelding(en) en/of schijven worden verwijderd voordat u deze opslag account verwijdert. ".*

**Klassieke portal op Azure**:

*Opslag account < vm-opslag-account-name > heeft enkele actieve afbeelding(en) en/of schijven, bijvoorbeeld xxxxxxxxx-xxxxxxxxx-O-209490240936090599. Controleer deze afbeelding(en) en/of schijven worden verwijderd voordat u deze opslag account verwijdert.*

Of

**Op Azure portal**:

*Opslag account < vm-opslag-account-name > heeft 1 container(s) die een actieve afbeelding en/of schijf artefacten. Controleer deze artefacten worden verwijderd uit de opslagplaats van de afbeelding voordat u deze opslag account verwijdert*.

**Klassieke portal op Azure**:

*Indienen is mislukt opslag account < vm-opslag-account-name > heeft 1 container(s) die een actieve afbeelding en/of schijf artefacten. Controleer dat deze artefacten worden verwijderd uit de opslagplaats van de afbeelding voordat u deze opslag account verwijdert. Wanneer u probeert te verwijderen van een account voor opslag en er nog steeds actief schijven is gekoppeld zijn, verschijnt een bericht dat er nog actieve schijven die moeten worden verwijderd*.

### <a name="scenario-2-unable-to-delete-a-container"></a>Scenario 2: Kan niet verwijderen van een container

Wanneer u de container voor de opslag te verwijderen, kan het volgende foutbericht weergegeven:

*Fout bij het verwijderen van opslag container <container name>. Fout: "Er is momenteel een lease-overeenkomst voor de container en geen lease-ID is opgegeven in de aanvraag*.

### <a name="scenario-3-unable-to-delete-a-vhd"></a>Scenario 3: Kan niet verwijderen van een VHD

Nadat u een VM te verwijderen en probeer de blobs voor de bijbehorende VHD's verwijderd, kan het volgende bericht worden weergegeven:

*Fout bij het verwijderen van blob ' pad/XXXXXX-XXXXXX-os-1447379084699.vhd'. Fout: "Er is momenteel een lease-overeenkomst op de blob en geen lease-ID is opgegeven in de aanvraag.*

## <a name="solution"></a>Oplossing
Probeer de meest voorkomende problemen oplossen door de volgende methode:

### <a name="step-1-delete-any-os-disks-that-are-preventing-deletion-of-the-storage-account-container-or-vhd"></a>Stap 1: OS schijven die verhinderen het verwijderen van de account van de opslag, de recipiënt of het VHD dat verwijderen

1. Schakel over naar de [Azure klassieke portal](https://manage.windowsazure.com/).
2. Selecteer de **virtuele MACHINE** > **schijven**.

    ![Afbeelding van de schijven op virtuele machines op Azure klassieke portal.](./media/storage-cannot-delete-storage-account-container-vhd/VMUI.png)

3. Zoek de diskettes die zijn gekoppeld aan de account van de opslag, container of VHD die u wilt verwijderen. Wanneer u de locatie van de schijf controleert, zult u de account gekoppeld opslag, de recipiënt of de VHD.

    ![Afbeelding waarin informatie over de locatie voor schijven op Azure klassieke portal](./media/storage-cannot-delete-storage-account-container-vhd/DiskLocation.png)

4. Controleer of er geen VM is opgenomen in het veld **gekoppeld aan** van de schijven en verwijder de schijven.

    > [AZURE.NOTE] Als een schijf is aangesloten op een VM, is het niet mogelijk om deze te verwijderen. Schijven worden losgekoppeld van een verwijderde VM asynchroon. Het duurt een paar minuten nadat de VM voor dit veld te ruimen is verwijderd.

### <a name="step-2-delete-any-vm-images-that-are-preventing-deletion-of-the-storage-account-or-container"></a>Stap 2: VM afbeeldingen die verhinderen het verwijderen van de opslag- of container dat verwijderen

1. Schakel over naar de [Azure klassieke portal](https://manage.windowsazure.com/).
2. Selecteer de **virtuele MACHINE** > **beelden**, en verwijder vervolgens de afbeeldingen die gekoppeld aan de account van de opslag, de recipiënt of het VHD zijn.

    Daarna probeer opnieuw te verwijderen de rekening voor opslag, de recipiënt of de VHD.

> [AZURE.WARNING] Zorg dat u een back-up alles wat die u opslaan wilt voordat u de account verwijdert. Het is niet mogelijk om een verwijderde opslag account herstellen of ophalen van de inhoud die deze vóór verwijdering bevatte. Dit ook geldt voor alle bronnen in de rekening: als u een VHD, blob, tabel, wachtrij of bestand verwijdert, wordt deze permanent verwijderd. Zorg ervoor dat de bron niet in gebruik.

## <a name="about-the-stopped-deallocated-status"></a>Over de status gestopt (opgeheven)

VMs die zijn gemaakt in het implementatiemodel klassiek en die zijn gehouden wordt de status **(opgeheven) gestopt** op de [Azure portal](https://portal.azure.com/) of [Azure klassieke portal](https://manage.windowsazure.com/)hebben.

**Azure klassieke portal**:

![Status (Deallocated) voor VMs op Azure portal gestopt.](./media/storage-cannot-delete-storage-account-container-vhd/moreinfo2.png)


**Azure portal**:

![Gestopt (vrijgegeven) de status van VMs in Azure klassieke portal.](./media/storage-cannot-delete-storage-account-container-vhd/moreinfo1.png)

De status 'Gestopt (opgeheven)' brengt de computerbronnen, zoals CPU, geheugen en netwerk. De schijven, blijven echter nog steeds behouden zodat u snel opnieuw kunt maken de VM indien nodig. Deze schijven worden op VHD's worden ondersteund door Azure opslag gemaakt. De opslag-account heeft deze VHD's en de schijven hebben een lease-overeenkomsten op de VHD's.

## <a name="next-steps"></a>Volgende stappen

- [Een opslag-account verwijderen](storage-create-storage-account.md#delete-a-storage-account)
- [Hoe u de vergrendelde lease van blob-opslag in Microsoft Azure (PowerShell)](https://gallery.technet.microsoft.com/scriptcenter/How-to-break-the-locked-c2cd6492)
