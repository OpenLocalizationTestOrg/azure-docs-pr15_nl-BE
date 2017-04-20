<properties
    pageTitle="Het maken, beheren of verwijderen van een account voor opslag in de klassieke Portal Azure | Microsoft Azure"
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

Een Azure opslag account krijgt u toegang tot de services Azure Blob, wachtrij, tabel en bestand in Azure opslag. Uw account opslag biedt een unieke naamruimte voor uw opslag Azure-gegevensobjecten. De gegevens in uw account is standaard alleen beschikbaar voor u, de eigenaar van de account.

Er zijn twee soorten accounts opslag:

- Een standaard opslag-account bevat Blob, tabel, wachtrij en bestand opslag.
- Een opslag premium account ondersteunt momenteel alleen schijven Azure virtuele machine. Zie [Premium-opslag: krachtige opslag voor Azure VM werklasten](storage-premium-storage.md) voor een diepgaand overzicht van de premie opslag.

## <a name="storage-account-billing"></a>Opslag account facturering

U worden voor Azure opslag gebruik op basis van uw opslag account gefactureerd. Kosten voor opslag zijn gebaseerd op vier factoren: opslagcapaciteit, replicatie schema, opslag, transacties en gegevens egress.

- Opslagcapaciteit verwijst naar hoeveel van uw account opslag eenheden u wilt gebruiken voor het opslaan van gegevens. De kosten van het opslaan van gegevens gewoon wordt bepaald door de hoeveelheid gegevens die u opslaat, en hoe deze worden gerepliceerd.
- Replicatie wordt bepaald hoeveel exemplaren van uw gegevens tegelijk en op welke locaties worden bijgehouden.
- Transacties verwijzen naar alle lees- en schrijfbewerkingen naar de opslag van Azure.
- Gegevens egress verwijst naar gegevens die zijn verzonden uit een gebied met Azure. Wanneer de gegevens in uw account voor de opslag wordt geopend door een toepassing die niet wordt uitgevoerd in dezelfde regio, of die toepassing een cloud-service of een ander type van toepassing is, worden u gegevens egress belast. (Voor Azure services, u kunt maatregelen treffen om uw gegevens en services in de dezelfde datacenters te verminderen of te elimineren gegevens egress toeslagen.)  

De pagina [Azure opslag prijzen](https://azure.microsoft.com/pricing/details/storage) biedt gedetailleerde prijsinformatie voor de opslagcapaciteit, replicatie en transacties. De pagina [Details prijzen op overdrachten van gegevens](https://azure.microsoft.com/pricing/details/data-transfers/) biedt gedetailleerde prijsinformatie voor egress gegevens.

Zie voor meer informatie over capaciteit en prestaties doelen van opslag account [Azure opslag schaalbaarheid en prestaties, doelen](storage-scalability-targets.md).

> [AZURE.NOTE] Wanneer u een Azure virtuele machine maakt, een opslag account wordt u automatisch gemaakt op de locatie van de installatie als u nog een rekening voor opslag op die locatie. Het is dus niet nodig de stappen hieronder om een account voor opslag van de virtuele machine-schijven maken. De naam van de opslag zal worden gebaseerd op de naam van de virtuele machine. Zie de [documentatie van Azure virtuele Machines](https://azure.microsoft.com/documentation/services/virtual-machines/) voor meer informatie.

## <a name="create-a-storage-account"></a>Maak een account voor opslag

1. Aanmelden bij de [Azure klassieke Portal](https://manage.windowsazure.com).

2. Klik op **Nieuw** op de taakbalk onder aan de pagina. Kies **Data Services** | **opslag**, en klik vervolgens op **Snel maken**.

    ![NewStorageAccount](./media/storage-create-storage-account-classic-portal/storage_NewStorageAccount.png)

3. Voer een naam voor uw account voor opslag in de **URL**.

    > [AZURE.NOTE] Opslag-accountnamen moeten tussen 3 en 24 tekens bevatten en mag alleen cijfers en kleine letters bevatten.
    >  
    > De naam van uw opslag moet uniek zijn binnen de Azure. De klassieke Portal Azure wordt aangegeven als de naam van de opslag die u selecteert wordt al gebruikt.

    Zie [opslag account eindpunten](#storage-account-endpoints) hieronder voor meer informatie over hoe de naam van de opslag om uw objecten in Azure opslag worden gebruikt.

4. Selecteer een locatie voor uw account opslag die dicht bij u of uw klanten in **De groep locatie/affiniteit**. Als gegevens in uw account opslag worden benaderd vanuit een andere Azure service, zoals een Azure virtuele machine of cloud-service, kunt u een groep affiniteit selecteren uit de lijst om uw account opslag in hetzelfde datacenter met andere Azure services die u gebruikt voor het verbeteren van prestaties en lagere kosten.

    Opmerking u moet een groep affiniteit selecteren als uw account opslag wordt gemaakt. U kunt een bestaande account niet verplaatsen naar een groep affiniteit. Zie voor meer informatie over affiniteit groepen, [collocatie met een groep affiniteit Service](#service-co-location-with-an-affinity-group) hieronder.

    >[AZURE.IMPORTANT] Als u wilt bepalen welke locaties beschikbaar zijn voor uw abonnement, kunt u de bewerking voor [alle leveranciers van de resource lijst](https://msdn.microsoft.com/library/azure/dn790524.aspx) aanroepen. [Get AzureLocation](https://msdn.microsoft.com/library/azure/dn757693.aspx)aanroepen om providers van PowerShell te geven. Gebruik de [lijst](https://msdn.microsoft.com/library/azure/microsoft.azure.management.resources.provideroperationsextensions.list.aspx) methode van de klasse ProviderOperationsExtensions van .NET.
    >
    >Zie ook [Azure regio's](https://azure.microsoft.com/regions/#services) voor meer informatie over welke services in welke regio beschikbaar zijn.


5. Als u meer dan één Azure-abonnement hebt, wordt het veld **abonnement** weergegeven. Voer de Azure abonnement dat u wilt gebruikmaken van de account van de opslag met in- **abonnement**.

6. Selecteer het gewenste niveau van replicatie voor uw account opslag bij **replicatie**. De replicatieoptie aanbevolen is geo-redundante replicatie biedt maximale duurzaamheid voor uw gegevens. Zie voor meer informatie over replicatie Azure opslagopties, [Azure Storage replicatie](storage-redundancy.md).

6. Klik op **opslag-Account maken**.

    Het duurt een paar minuten om uw account opslag te maken. U kunt de status controleren, kunt u de mededelingen onder in de klassieke Portal Azure controleren. Nadat de opslag-account is gemaakt, wordt uw nieuwe account voor opslag status **on line** is en gereed is voor gebruik.

![StoragePage](./media/storage-create-storage-account-classic-portal/Storage_StoragePage.png)


### <a name="storage-account-endpoints"></a>Opslag account eindpunten

Elk object dat u in Azure opslag opslaat heeft een unieke URL-adres. De naam van de opslag vormt het subdomein van dat adres. De combinatie van subdomein en domein naam, die specifiek voor elke service is, vormt een *eindpunt* voor uw opslag-account.

Als uw account opslag met de naam *mystorageaccount*, vervolgens zijn de Standaardeindpunten voor de account van uw opslag bijvoorbeeld:

- BLOB-service: http://*mystorageaccount*. blob.core.windows.net

- Tabel service: http://*mystorageaccount*. table.core.windows.net

- Service in de wachtrij: http://*mystorageaccount*. queue.core.windows.net

- File-service: http://*mystorageaccount*. file.core.windows.net

Zodra de account is gemaakt, kunt u voor uw account opslag op het dashboard voor opslag in de [Klassieke Azure-Portal](https://manage.windowsazure.com) de eindpunten bekijken.

De URL voor toegang tot een object in een opslag-rekening wordt gebouwd door de locatie van het object in de opslag-account toe te voegen aan het eindpunt. Bijvoorbeeld deze indeling kan hebben in een blob-adres: http://*mystorageaccount*.blob.core.windows.net/*mycontainer*/*myblob*.

U kunt ook een aangepaste domeinnaam voor uw account opslag configureren. Zie [een aangepaste domeinnaam voor uw eindpunt blob-opslag configureren](storage-custom-domain-name.md) voor meer informatie.

### <a name="service-co-location-with-an-affinity-group"></a>Collocatie bij een groep affiniteit service

Een *groep affiniteit* is een geografische groepering van VMs en Azure services aan uw account Azure opslag. Een groep affiniteit Verbeter prestaties van de service door werkbelasting computer zoeken in het datacenter van dezelfde of in de buurt van de doelgroep van de gebruiker. Ook geen facturering kosten verbonden zijn egress wanneer gegevens in een opslag-account van een andere service die deel uitmaakt van dezelfde groep affiniteit wordt geopend.

> [AZURE.NOTE]  Een affiniteit als groep wilt maken, opent u het gebied <b>Instellingen</b> van de [Klassieke Portal Azure](https://manage.windowsazure.com), klik op <b>affiniteit groepen</b>en klik op <b>een groep affiniteit toevoegen</b> of de knop <b>toevoegen</b> . U kunt ook groepen maken en beheren affiniteit met behulp van de API-Azure Service Management. Zie <a href="http://msdn.microsoft.com/library/azure/ee460798.aspx">bewerkingen op affiniteit groepen</a> voor meer informatie.

## <a name="view-copy-and-regenerate-storage-access-keys"></a>De toegangstoetsen weergeven, kopiëren en opnieuw genereren van opslag

Wanneer u een opslag-account maakt, genereert Azure twee opslag 512 bits toegangstoetsen, die worden gebruikt voor verificatie als de opslag-account wordt geopend. Doordat twee opslag toegangstoetsen kunt Azure u opnieuw genereren van sleutels met geen onderbreking met uw opslagservice of toegang tot deze service.

> [AZURE.NOTE] Het is raadzaam om te voorkomen dat uw opslag toegangstoetsen delen met anderen. Om toegang te verlenen tot opslagbronnen zonder opgave van uw access-toetsen, kunt u een *gedeelde access-handtekening*. De handtekening van een gedeelde toegang biedt toegang tot een bron in uw account voor een interval dat u definieert en met de machtigingen die u opgeeft. [Met behulp van gedeelde toegang handtekeningen (SAS)](storage-dotnet-shared-access-signature-part-1.md) Zie voor meer informatie.

Gebruik in de [Klassieke Portal Azure](https://manage.windowsazure.com) **Sleutels beheren** op het dashboard of de **opslag** -pagina bekijken, kopiëren en opnieuw genereren van de toegangstoetsen opslag die worden gebruikt voor toegang tot de Blob-, tabel- en wachtrij-services.

### <a name="copy-a-storage-access-key"></a>Een toegangstoets opslag kopiëren  

**Sleutels beheren** kunt u een toegangstoets opslag te gebruiken in een verbindingsreeks kopiëren. De verbindingsreeks is de naam van de opslag en een sleutel voor verificatie vereist. Zie voor meer informatie over het configureren van verbindingsreeksen voor toegang tot Azure opslagservices [Azure opslag verbindingsreeksen configureren](storage-configure-connection-string.md).

1. In de [Klassieke Azure-Portal](https://manage.windowsazure.com)op **opslag**en klik vervolgens op de naam van de account van de opslag voor het openen van het dashboard.

2. Klik op **sleutels beheren**.

    **Toegangstoetsen beheren** wordt geopend.

    ![Managekeys](./media/storage-create-storage-account-classic-portal/Storage_ManageKeys.png)


3. Als een toegangstoets opslag kopiëren, selecteert u de tekst. Klik met de rechtermuisknop en klik op **kopiëren**.

### <a name="regenerate-storage-access-keys"></a>Toegangstoetsen opslag opnieuw genereren
Het is raadzaam dat u de toegangstoetsen wijzigen naar uw account opslag regelmatig om uw opslag verbindingen te beveiligen. Twee toegangstoetsen worden toegewezen, zodat u verbindingen naar de rekening voor opslag onderhouden kunt via een toegangstoets terwijl u de toegangstoets opnieuw genereren.

> [AZURE.WARNING] Opnieuw genereren van de toegangstoetsen kan invloed hebben op services in Azure, alsmede uw eigen toepassingen die afhankelijk van de opslag-account zijn. Alle clients die de toegangstoets gebruiken voor toegang tot de account van de opslag moeten worden bijgewerkt om de nieuwe sleutel te gebruiken.

**Mediaservices** - als u mediaservices die afhankelijk van uw opslag-account zijn hebt, u moet opnieuw synchroniseren de toegangstoetsen met uw media-service nadat u de sleutels opnieuw genereren.

**Toepassingen** : als u webtoepassingen of cloud-services die gebruikmaken van de account van de opslag, verliest u de verbindingen als u opnieuw genereren van sleutels, tenzij u uw sleutels herstellen. 

**Opslag Explorers** - als u een [explorer toepassingen worden opgeslagen](storage-explorers.md), waarschijnlijk moet u de sleutel opslag door deze toepassingen bijwerken.

Hier is het proces voor uw opslag toegangstoetsen draaien:

1. De tekenreeksen in de toepassingscode om te verwijzen naar de secundaire sneltoets van de opslag-account bijwerken.

2. De primaire toegangstoets voor uw opslag account opnieuw te genereren. Klik in de [Klassieke Azure-Portal](https://manage.windowsazure.com)van het dashboard of de pagina **configureren** **Sleutels beheren**. Klik op **toevoegen** onder de toegangssleutel van de primaire en klik vervolgens op **Ja** om te bevestigen dat u wilt een nieuwe sleutel genereren.

3. Werk de verbindingsreeksen in uw programmacode te verwijzen naar de nieuwe primaire toegangstoets.

4. De secundaire sneltoets opnieuw te genereren.

## <a name="delete-a-storage-account"></a>Een opslag-account verwijderen

Een opslag als account wilt verwijderen die u niet meer gebruikt, gebruik **verwijderen** op het dashboard of op de pagina **configureren** . **Verwijderen** , verwijdert u de account volledige opslag, met inbegrip van alle BLOB's, tabellen en wachtrijen op de rekening.

> [AZURE.WARNING] Het is niet mogelijk om een verwijderde opslag account herstellen of ophalen van de inhoud die deze vóór verwijdering bevatte. Zorg dat u een back-up alles wat die u opslaan wilt voordat u de account verwijdert. Dit ook geldt voor alle bronnen in de account, als u eenmaal een blob, tabel, wachtrij of bestand, wordt deze permanent verwijderd.
>
> Als uw account opslag een Azure VM VHD-bestanden bevat, moet u verwijderen alle afbeeldingen en schijven die de VHD-bestanden gebruiken voordat u de opslag-account kunt verwijderen. Eerst stopt de virtuele machine als het wordt uitgevoerd en vervolgens te verwijderen. Schijven verwijderen, gaat u naar het tabblad **schijven** en schijven er verwijderen. Afbeeldingen verwijderen, Ga naar het tabblad **afbeeldingen** en afbeeldingen die zijn opgeslagen in de account verwijderen.

1. Klik op **opslag**in de [Klassieke Azure-Portal](https://manage.windowsazure.com).

2. Klik ergens in de opslag, met uitzondering van de naam van de post en klik vervolgens op **verwijderen**.

     - Of -

    Klik op de naam van de account van de opslag voor het openen van het dashboard en klik vervolgens op **verwijderen**.

3. Klik op **Ja** om te bevestigen dat u wilt verwijderen van de account van de opslag.

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over Azure opslag, de [opslag Azure documentatie](https://azure.microsoft.com/documentation/services/storage/).
- Ga naar de [Azure Storage teamblog](http://blogs.msdn.com/b/windowsazurestorage/).
- [Overdracht van gegevens met het hulpprogramma AzCopy voor opdrachtregel](storage-use-azcopy.md)
