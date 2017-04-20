<properties
    pageTitle="Maak een account Azure Batch | Microsoft Azure"
    description="Informatie over het maken van een account Azure Batch in Azure portal naar grootschalige parallelle werklasten worden uitgevoerd in de cloud"
    services="batch"
    documentationCenter=""
    authors="mmacy"
    manager="timlt"
    editor=""/>

<tags
    ms.service="batch"
    ms.workload="big-compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/21/2016"
    ms.author="marsma"/>

# <a name="create-an-azure-batch-account-using-the-azure-portal"></a>Een Batch Azure account maken met de Azure portal

> [AZURE.SELECTOR]
- [Azure portal](batch-account-create-portal.md)
- [Batchbeheer .NET](batch-management-dotnet.md)

Informatie over het maken van een account Azure Batch in [Azure portal][azure_portal], en waar u wilt zoeken naar belangrijke eigenschappen, zoals de toegangstoetsen en URL's rekening. Bespreken we ook Batch prijzen en een Azure opslag account koppelen aan je account Batch zodat kunt u [toepassingspakketten](batch-application-packages.md) en [Project en uitvoer blijven behouden](batch-task-output.md).

## <a name="create-a-batch-account"></a>Een Batch-account maken

1. Aanmelden bij de [Azure portal][azure_portal].

2. Klik op **nieuwe** > **berekenen** > **Service Batch**.

    ![Batch in de markt][marketplace_portal]

3. De blade **Nieuwe Batch-Account** wordt weergegeven. Zie de artikelen *een* tot en met *e* hieronder voor een beschrijving van elke blade-element.

    ![Een Batch-account maken][account_portal]

    een. **Accountnaam**: een unieke naam voor uw Batch-account. Deze naam moet uniek zijn binnen de Azure regio die de account wordt gemaakt (Zie hieronder de *locatie* ). Het mag alleen letters, cijfers, en 3-24 tekens lang moet zijn.

    b. **Abonnement**: een abonnement voor het maken van de Batch-account. Als u slechts één abonnement hebt, is standaard ingeschakeld.

    c. **Resourcegroep**: een bestaand middel voor uw nieuwe Batch account groeperen of eventueel een nieuwe maken.

    d. **Locatie**: regio een Azure waarin de Batch-account te maken. Alleen de regio's die worden ondersteund door uw abonnement en de resourcegroep worden als opties weergegeven.

    e. **Opslag Account** (optioneel): een **algemene** opslag account u (koppeling) naar uw nieuwe Batch-account koppelen. Zie [gekoppelde Azure opslag account](#linked-azure-storage-account) hieronder voor meer informatie.

4. Klik op **maken** om de account te maken.

  De portal geeft aan dat het **implementeren** van de account is voltooid, een melding **implementaties is voltooid** wordt weergegeven in de *meldingen*.

## <a name="view-batch-account-properties"></a>Batch-eigenschappen weergeven

Zodra de account is gemaakt, kunt u de **batchverwerking account blade** voor toegang tot de eigenschappen en instellingen te openen. Toegankelijk alle eigenschappen en instellingen via het menu links van de bladeserver Batch-account.

![Batch account blade in Azure portal][account_blade]

* **Batch account URL**: toepassingen die u met de [ontwikkeling van de Batch API's maakt](batch-technical-overview.md#batch-development-apis) moeten een account URL netwerkbronnen beheren en uitvoeren van taken in de account. Een Batch account URL heeft de volgende indeling:

    `https://<account_name>.<region>.batch.azure.com`

![Batch account URL in de portal][account_url]

* **Toegangstoetsen**: uw toepassingen moeten ook een toegangstoets als u werkt met resources in uw account voor de Batch. Invoeren om te bekijken of opnieuw genereren van sleutels voor uw Batch-account toegang, `keys` in het linkermenu **Zoeken** op de account Batch blade, selecteer **sleutels**.

    ![Batch account sleutels in Azure portal][account_keys]

## <a name="pricing"></a>Prijzen

Batchaccounts worden alleen in een "vrij laag," wat betekent dat u niet worden aangerekend voor de Batch account zelf aangeboden. U in rekening worden gebracht voor de onderliggende Azure compute bronnen verbruiken voor uw Batch-oplossingen en de bronnen worden gebruikt door andere services, wanneer uw werklasten worden uitgevoerd. Zoals u in rekening gebracht voor de compute nodes in uw toepassingen en voor de gegevens die u opslaat in Azure opslag als input of output voor uw taken. Op dezelfde manier als u de functie voor [toepassingspakketten](batch-application-packages.md) batch, u in rekening worden gebracht voor de Azure opslagbronnen gebruikt voor het opslaan van uw toepassingspakketten. [Batch prijzen] Zie[ batch_pricing] voor meer informatie.

## <a name="linked-azure-storage-account"></a>Gekoppelde Azure opslag account

Zoals eerder vermeld, kunt u (optioneel) een **algemene** opslag-account koppelen aan uw account voor de Batch. De functie voor [toepassingspakketten](batch-application-packages.md) batch gebruikt blob-opslag in een gekoppelde algemene opslag-account de [Batch bestand verdragen .NET](batch-task-output.md) library. Deze optionele functies helpt u bij het persistent maken van de gegevens die zij produceren en distribueren van de toepassingen die de batchverwerking taken uitgevoerd.

Momenteel ondersteunt *alleen* de **algemene** rekening opslagtype batch zoals beschreven in stap 5, [een opslag-account maken](../storage/storage-create-storage-account.md#create-a-storage-account), in [Azure over opslag rekeningen](../storage/storage-create-storage-account.md). Als u een account Azure opslag voor uw Batch-account koppelt, of koppeling *alleen* een **algemene** opslag-account zijn.

![Een "Algemene doeleinden" opslag-account maken][storage_account]

Het is raadzaam dat u een account opslag voor exclusief gebruik door uw Batch-account maken.

>[AZURE.WARNING] Wees voorzichtig bij het opnieuw genereren van de toegangstoets van een gekoppelde account voor opslag. Slechts één opslag account sleutel opnieuw genereren en klik op **Sync sleutels** in het gekoppelde account blade voor opslag. Wacht vijf minuten om de sleutels doorgeven aan de compute nodes in uw toepassingen, en vervolgens opnieuw genereren en synchroniseren van de andere sleutel indien nodig. Als u beide toetsen op hetzelfde moment, uw computerknooppunten niet mogelijk om te synchroniseren van een sleutel en zij geen toegang meer naar de rekening voor opslag.

  ![Opnieuw genereren van sleutels voor opslag account][4]

## <a name="batch-service-quotas-and-limits"></a>Batch service quota's en beperkingen

Zorg ervoor dat bij uw abonnement Azure en andere Azure diensten, bepaalde [quota's en beperkingen](batch-quota-limit.md) van toepassing op Batchaccounts. Huidige quota voor een Batch-account worden weergegeven in de portal in de **Eigenschappen**van de account.

![Batch account quota's in Azure portal][quotas]

Houd rekening met deze quota ontwerpen en schalen van uw werkbelasting Batch. Bijvoorbeeld als uw groep is niet van het doel aantal computerknooppunten die u hebt opgegeven bereiken, u mogelijk hebt bereikt de quotumlimiet core voor uw Batch-account.

Vergeet niet dat u niet tot één Batch-account voor uw abonnement op Azure beperkt zijn. U kunt meerdere Batch werklasten in één Batch-account uitvoeren of distribueren van uw taken onder Batch-accounts in het abonnement hetzelfde, maar in verschillende gebieden van Azure.

Veel van deze contingenten worden verhoogd met een gratis product support aanvraag ingediend in de portal Azure. Zie [quota's en beperkingen voor de Batch Azure-service](batch-quota-limit.md) voor meer informatie over het aanvragen van quota-verhogingen.

## <a name="other-batch-account-management-options"></a>Andere opties voor batchverwerking account management

De Azure portal gebruiken, kunt u ook maken en beheren van accounts met de volgende Batch:

* [Batch PowerShell-cmdlets](batch-powershell-cmdlets-get-started.md)
* [Azure CLI](../xplat-cli-install.md)
* [Batchbeheer .NET](batch-management-dotnet.md)

## <a name="next-steps"></a>Volgende stappen

* Zie voor meer informatie over functies en Batch-service-begrippen [Functieoverzicht Azure Batch](batch-api-basics.md) . Het artikel bespreekt de primaire Batch-bronnen, zoals toepassingen, compute nodes, projecten en taken en biedt een overzicht van de functies van de service waarmee de uitvoering van grootschalige compute werkbelasting.

* De basisbegrippen van het ontwikkelen van een Batch-toepassing met behulp van de [Batch .NET client library](batch-dotnet-get-started.md). De [inleidende artikel](batch-dotnet-get-started.md) helpt u bij een werken-toepassing die u gebruikt de batchverwerking om een werkbelasting uitvoeren op meerdere computerknooppunten en gebruik van Azure opslag voor werkbelasting bestand klaarzetten en ophalen.

[api_net]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[api_rest]: https://msdn.microsoft.com/library/azure/Dn820158.aspx

[azure_portal]: https://portal.azure.com
[batch_pricing]: https://azure.microsoft.com/pricing/details/batch/

[4]: ./media/batch-account-create-portal/batch_acct_04.png "Opnieuw genereren van sleutels voor opslag account"
[marketplace_portal]: ./media/batch-account-create-portal/marketplace_batch.PNG
[account_blade]: ./media/batch-account-create-portal/batch_blade.png
[account_portal]: ./media/batch-account-create-portal/batch_acct_portal.png
[account_keys]: ./media/batch-account-create-portal/account_keys.PNG
[account_url]: ./media/batch-account-create-portal/account_url.png
[storage_account]: ./media/batch-account-create-portal/storage_account.png
[quotas]: ./media/batch-account-create-portal/quotas.png
