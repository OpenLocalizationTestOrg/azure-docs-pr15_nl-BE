<properties
    pageTitle="Het maken, beheren of verwijderen van een account voor opslag in de Portal Azure | Microsoft Azure"
    description="Maak een nieuwe account voor de opslag, de toegangstoetsen account beheren of verwijderen van een account voor opslag in de Portal Azure. Informatie over opslag standard en premium-accounts."
    services="storage"
    documentationCenter=""
    authors="robinsh"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="07/26/2016"
    ms.author="robinsh"/>


# <a name="about-azure-storage-accounts"></a>Over Azure opslag accounts

[AZURE.INCLUDE [storage-selector-portal-create-storage-account](../../includes/storage-selector-portal-create-storage-account.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools](../../includes/storage-try-azure-tools.md)]

## <a name="overview"></a>Overzicht

Een account Azure opslag biedt een unieke naamruimte voor het opslaan en toegang tot uw gegevens opslag Azure objecten. Alle objecten in een opslag-account worden in rekening gebracht samen als een groep. De gegevens in uw account is standaard alleen beschikbaar voor u, de eigenaar van de account.

[AZURE.INCLUDE [storage-account-types-include](../../includes/storage-account-types-include.md)]

## <a name="storage-account-billing"></a>Opslag account facturering

[AZURE.INCLUDE [storage-account-billing-include](../../includes/storage-account-billing-include.md)]

> [AZURE.NOTE] Wanneer u een Azure virtuele machine maakt, een opslag account wordt u automatisch gemaakt op de locatie van de installatie als u nog een rekening voor opslag op die locatie. Het is dus niet nodig de stappen hieronder om een account voor opslag van de virtuele machine-schijven maken. De naam van de opslag zal worden gebaseerd op de naam van de virtuele machine. Zie de [documentatie van Azure virtuele Machines](https://azure.microsoft.com/documentation/services/virtual-machines/) voor meer informatie.

## <a name="storage-account-endpoints"></a>Opslag account eindpunten

Elk object dat u in Azure opslag opslaat heeft een unieke URL-adres. De naam van de opslag vormt het subdomein van dat adres. De combinatie van subdomein en domein naam, die specifiek voor elke service is, vormt een *eindpunt* voor uw opslag-account.

Als uw account opslag met de naam *mystorageaccount*, vervolgens zijn de Standaardeindpunten voor de account van uw opslag bijvoorbeeld:

- BLOB-service: http://*mystorageaccount*. blob.core.windows.net

- Tabel service: http://*mystorageaccount*. table.core.windows.net

- Service in de wachtrij: http://*mystorageaccount*. queue.core.windows.net

- File-service: http://*mystorageaccount*. file.core.windows.net

> [AZURE.NOTE] Een Blob storage rekening stelt alleen de Blob-service-eindpunt.

De URL voor toegang tot een object in een opslag-rekening wordt gebouwd door de locatie van het object in de opslag-account toe te voegen aan het eindpunt. Bijvoorbeeld deze indeling kan hebben in een blob-adres: http://*mystorageaccount*.blob.core.windows.net/*mycontainer*/*myblob*.

U kunt ook een aangepaste domeinnaam voor uw account opslag configureren. Zie voor klassieke opslag rekeningen, [een aangepaste naam voor uw eindpunt Blob Storage-domein configureren](storage-custom-domain-name.md) voor meer informatie. Deze functionaliteit is nog niet toegevoegd aan de [portal Azure](https://portal.azure.com) voor Resource Manager opslag, maar kunt u deze configureren met PowerShell. Zie voor meer informatie de cmdlet [Set-AzureRmStorageAccount](https://msdn.microsoft.com/library/mt607146.aspx) .  

## <a name="create-a-storage-account"></a>Maak een account voor opslag

1. Log in om de [Azure portal](https://portal.azure.com).

2. Selecteer **Nieuw**in het menu Hub -> **Data- +** -> **opslag account**.

3. Voer een naam voor de account van uw opslag. Zie [opslag account eindpunten](#storage-account-endpoints) voor meer informatie over hoe de naam van de opslag om uw objecten in Azure opslag worden gebruikt.

    > [AZURE.NOTE] Opslag-accountnamen moeten tussen 3 en 24 tekens bevatten en mag alleen cijfers en kleine letters bevatten.
    >  
    > De naam van uw opslag moet uniek zijn binnen de Azure. De Azure portal wordt aangegeven als de naam van de opslag die u selecteert al gebruikt wordt.

4. Geef het implementatiemodel wordt gebruikt: **Resource Manager** of **Klassiek**. **Bronbeheer** is het aanbevolen implementatiemodel. Zie [Resource Manager voor informatie over implementatie en klassieke implementatie](../resource-manager-deployment-model.md)voor meer informatie.

    > [AZURE.NOTE] BLOB storage accounts kunnen alleen worden gemaakt met behulp van het implementatiemodel Resource Manager.

5. Selecteer het type opslag account: **algemene doeleinden** of **Blob-opslag**. **Algemeen doel** is de standaardinstelling.

    Als **Algemeen doel** is geselecteerd, geeft u het niveau van de prestaties: **Standard** of **Premium**. De standaardinstelling is **standaard**. Zie [Inleiding tot Microsoft Azure opslag](storage-introduction.md) voor meer informatie over opslag standard en premium accounts, en [Premium-opslag: krachtige opslag voor een standaardwerkbelasting Azure Virtual Machine](storage-premium-storage.md).

    Als **Blob-opslag** is geselecteerd, geeft u de access-laag: **warm** of **koel**. De standaardwaarde is **Hot**. Zie [Azure Blob-opslag: afkoelen en lagen Hot](storage-blob-storage-tiers.md) voor meer informatie.

6. Selecteer de replicatieoptie voor de opslag: **LRS**, **GRS**, **RA GRS**of **ZRS**. De standaardwaarde is **RA GRS**. Zie voor meer informatie over replicatie Azure opslagopties, [Azure Storage replicatie](storage-redundancy.md).

7. Selecteer het abonnement waar u wilt de nieuwe opslag-account maken.

8. Een nieuwe resourcegroep of Selecteer een bestaande brongroep. Zie voor meer informatie over resourcegroepen, [Azure Resource Manager-overzicht](../azure-resource-manager/resource-group-overview.md).

9. Selecteer de geografische locatie voor uw opslag-account. Zie [Azure regio's](https://azure.microsoft.com/regions/#services) voor meer informatie over welke services in welke regio beschikbaar zijn.

10. Klik op **maken** om de opslag-account te maken.

## <a name="manage-your-storage-account"></a>Uw account opslag beheren

### <a name="change-your-account-configuration"></a>Configuratie van de account wijzigen

Nadat u uw account opslag hebt gemaakt, kunt u de configuratie ervan, zoals het wijzigen van de replicatie-optie die wordt gebruikt voor de account of de laag toegang voor een Blob storage-account wijzigen. Navigeer naar de account van uw opslag in [Azure portal](https://portal.azure.com), klikt u op **alle instellingen** en klik op **configuratie** te bekijken en/of de accountconfiguratie wijzigen.

> [AZURE.NOTE] Afhankelijk van de laag van prestaties die u hebt gekozen bij het maken van de account van de opslag, replicatieopties mogelijk niet beschikbaar.

Wijzigen van de replicatieoptie verandert uw prijzen. Voor meer informatie, Zie [Azure opslag prijzen](https://azure.microsoft.com/pricing/details/storage/) pagina.

Blob storage accounts kan wijzigen van de laag toegang gelden de tarieven voor de wijziging wijzigen van uw prijzen. Zie de [Blob storage accounts - prijzen en facturering](storage-blob-storage-tiers.md#pricing-and-billing) voor meer informatie.

### <a name="manage-your-storage-access-keys"></a>De toegangstoetsen opslag beheren

Wanneer u een opslag-account maakt, genereert Azure twee opslag 512 bits toegangstoetsen, die worden gebruikt voor verificatie als de opslag-account wordt geopend. Doordat twee opslag toegangstoetsen kunt Azure u opnieuw genereren van sleutels met geen onderbreking met uw opslagservice of toegang tot deze service.

> [AZURE.NOTE] Het is raadzaam om te voorkomen dat uw opslag toegangstoetsen delen met anderen. Om toegang te verlenen tot opslagbronnen zonder opgave van uw access-toetsen, kunt u een *gedeelde access-handtekening*. De handtekening van een gedeelde toegang biedt toegang tot een bron in uw account voor een interval dat u definieert en met de machtigingen die u opgeeft. [Met behulp van gedeelde toegang handtekeningen (SAS)](storage-dotnet-shared-access-signature-part-1.md) Zie voor meer informatie.

#### <a name="view-and-copy-storage-access-keys"></a>Bekijken en toegangstoetsen opslag kopiëren

Navigeer naar de account van uw opslag in [Azure portal](https://portal.azure.com), klikt u op **alle instellingen** en klik op **Access-toetsen** om te bekijken, kopiëren en de toegangstoetsen voor uw account opnieuw genereren. De **Toegangstoetsen** blade bevat ook vooraf geconfigureerde verbindingsreeksen met behulp van de primaire en secundaire sleutels die u kopiëren kunt als u wilt gebruiken in uw toepassingen.

#### <a name="regenerate-storage-access-keys"></a>Toegangstoetsen opslag opnieuw genereren

Het is raadzaam dat u de toegangstoetsen wijzigen naar uw account opslag regelmatig om uw opslag verbindingen te beveiligen. Twee toegangstoetsen worden toegewezen, zodat u verbindingen naar de rekening voor opslag onderhouden kunt via een toegangstoets terwijl u de toegangstoets opnieuw genereren.

> [AZURE.WARNING] Opnieuw genereren van de toegangstoetsen kan invloed hebben op services in Azure, alsmede uw eigen toepassingen die afhankelijk van de opslag-account zijn. Alle clients die de toegangstoets gebruiken voor toegang tot de account van de opslag moeten worden bijgewerkt om de nieuwe sleutel te gebruiken.

**Mediaservices** - als u mediaservices die afhankelijk van uw opslag-account zijn hebt, u moet opnieuw synchroniseren de toegangstoetsen met uw media-service nadat u de sleutels opnieuw genereren.

**Toepassingen** : als u webtoepassingen of cloud-services die gebruikmaken van de account van de opslag, verliest u de verbindingen als u opnieuw genereren van sleutels, tenzij u uw sleutels herstellen.

**Opslag Explorers** - als u een [explorer toepassingen worden opgeslagen](storage-explorers.md), waarschijnlijk moet u de sleutel opslag door deze toepassingen bijwerken.

Hier is het proces voor uw opslag toegangstoetsen draaien:

1. De tekenreeksen in de toepassingscode om te verwijzen naar de secundaire sneltoets van de opslag-account bijwerken.

2. De primaire toegangstoets voor uw opslag account opnieuw te genereren. Op het blad **Toegangstoetsen** **Key1 genereren**op en klik vervolgens op **Ja** om te bevestigen dat u wilt een nieuwe sleutel genereren.

3. Werk de verbindingsreeksen in uw programmacode te verwijzen naar de nieuwe primaire toegangstoets.

4. Secundaire sneltoets toevoegen op dezelfde manier.

## <a name="delete-a-storage-account"></a>Een opslag-account verwijderen

Navigeer naar de account van de opslag in de [portal Azure](https://portal.azure.com)een opslag als account wilt verwijderen die u niet meer gebruikt, en klikt u op **verwijderen**. Een opslag-account verwijdert, wordt de gehele rekening, met inbegrip van alle gegevens in de account.

> [AZURE.WARNING] Het is niet mogelijk om een verwijderde opslag account herstellen of ophalen van de inhoud die deze vóór verwijdering bevatte. Zorg dat u een back-up alles wat die u opslaan wilt voordat u de account verwijdert. Dit ook geldt voor alle bronnen in de account, als u eenmaal een blob, tabel, wachtrij of bestand, wordt deze permanent verwijderd.

U verwijdert een opslag-account die is gekoppeld aan een Azure virtuele machine, moet u eerst controleren dat de virtuele machine-schijven zijn verwijderd. Als u de virtuele machine-schijven niet eerst verwijdert, vervolgens verschijnt wanneer u wilt uw opslag account verwijderen, een foutbericht dat lijkt op:

    Failed to delete storage account <vm-storage-account-name>. Unable to delete storage account <vm-storage-account-name>: 'Storage account <vm-storage-account-name> has some active image(s) and/or disk(s). Ensure these image(s) and/or disk(s) are removed before deleting this storage account.'.

Als de account voor de opslag wordt het model Klassiek implementatie gebruikt, kunt u de virtuele machine schijf verwijderen via de volgende stappen in de [Azure portal](https://manage.windowsazure.com):

1. Ga naar de [klassieke Azure portal](https://manage.windowsazure.com).
2. Ga naar het tabblad virtuele Machines.
3. Klik op het tabblad schijven.
4. Selecteer de gegevensschijf en klik vervolgens op schijf verwijderen.
5. U verwijdert schijfkopieën, Ga naar het tabblad afbeeldingen en afbeeldingen die zijn opgeslagen in de account verwijderen.

Zie voor meer informatie de [documentatie van Azure Virtual Machine](http://azure.microsoft.com/documentation/services/virtual-machines/).

## <a name="next-steps"></a>Volgende stappen

- [Azure Blob-opslag: Koel en warm lagen](storage-blob-storage-tiers.md)
- [Azure opslag, replicatie](storage-redundancy.md)
- [Verbindingsreeksen Azure opslag configureren](storage-configure-connection-string.md)
- [Overdracht van gegevens met het hulpprogramma AzCopy voor opdrachtregel](storage-use-azcopy.md)
- Ga naar de [Azure Storage teamblog](http://blogs.msdn.com/b/windowsazurestorage/).
