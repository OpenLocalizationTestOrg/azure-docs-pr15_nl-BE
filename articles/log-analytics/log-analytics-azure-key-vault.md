<properties
    pageTitle="Azure sleutel kluis oplossing in logboek Analytics | Microsoft Azure"
    description="U kunt de Azure sleutel kluis oplossing in logboek Analytics Azure sleutel kluis logboeken bekijken."
    services="log-analytics"
    documentationCenter=""
    authors="richrundmsft"
    manager="jochan"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/12/2016"
    ms.author="richrund"/>

# <a name="azure-key-vault-preview-solution-in-log-analytics"></a>Azure sleutel kluis (voorbeeld) oplossing in logboek Analytics

>[AZURE.NOTE] Dit is een [voorbeeld van oplossing](log-analytics-add-solutions.md#log-analytics-preview-solutions-and-features).

U kunt de Azure sleutel kluis oplossing in logboek Analytics Azure sleutel kluis AuditEvent logboeken bekijken.

U kunt de logboekregistratie van gebeurtenissen controleren voor Azure sleutel kluis kunt inschakelen. Deze logboekbestanden worden geschreven naar Azure Blob-opslag waar ze kunnen vervolgens worden geïndexeerd door logboek Analytics voor zoek- en analyse.

## <a name="install-and-configure-the-solution"></a>Installeren en configureren van de oplossing

Gebruik de volgende instructies te installeren en configureren van de oplossing Azure sleutel kluis:

1.  [Diagnostische logboekregistratie voor sleutel kluis](../key-vault/key-vault-logging.md) bronnen die u wilt controleren inschakelen
2.  Logboek Analytics om te lezen van de logboeken van blob-opslag met behulp van het proces beschreven in [JSON bestanden in de blobopslag](../log-analytics/log-analytics-azure-storage-json.md)configureren.
3.  De oplossing Azure sleutel kluis inschakelen met behulp van het proces beschreven in [oplossingen logboek Analytics toevoegen uit de galerie met oplossingen](log-analytics-add-solutions.md).  

## <a name="review-azure-key-vault-data-collection-details"></a>Details van deze collectie Azure sleutel kluis gegevens

Azure sleutel kluis oplossing verzamelt diagnostische logboeken van Azure blob-opslag voor Azure sleutel kluis.
Geen agent is vereist voor het verzamelen van gegevens.

De volgende tabel bevat methoden van de collectie gegevens en andere informatie over de manier waarop gegevens worden verzameld voor Azure sleutel kluis.

| Platform | Directe agent | Systems Center Operations Manager (SCOM)-agent | Azure opslag | SCOM nodig? | SCOM agent gegevens verzonden via management groep | Frequentie van verzameling |
|---|---|---|---|---|---|---|
|Azure|![Nee](./media/log-analytics-azure-keyvault/oms-bullet-red.png)|![Nee](./media/log-analytics-azure-keyvault/oms-bullet-red.png)|![Ja](./media/log-analytics-azure-keyvault/oms-bullet-green.png)|            ![Nee](./media/log-analytics-azure-keyvault/oms-bullet-red.png)|![Nee](./media/log-analytics-azure-keyvault/oms-bullet-red.png)| 10 minuten|

## <a name="use-azure-key-vault"></a>Azure sleutel kluis gebruiken

Na de installatie van de oplossing, kunt u de samenvatting van de aanvraag statussen na verloop van tijd voor uw gecontroleerde sleutel kluizen met behulp van de **Azure sleutel kluis** naast elkaar kunt weergeven op de pagina **Overzicht** van het logboek Analytics.

![afbeelding van de tegel Azure sleutel kluis](./media/log-analytics-azure-keyvault/log-analytics-keyvault-tile.png)

Nadat u op de tegel **Overzicht** , kunt u de samenvattingen van de logboeken weergeven en vervolgens analyseren voor de volgende categorieën:

- Volume van alle belangrijke kluis bewerkingen na verloop van tijd
- Volumes bewerking mislukt na verloop van tijd
- Gemiddelde latentie van operationele per bewerking
- Kwaliteit van de service voor bewerkingen met het aantal bewerkingen die meer dan 1000 ms en een lijst van bewerkingen die meer dan 1000 ms

![afbeelding van Azure sleutel kluis dashboard](./media/log-analytics-azure-keyvault/log-analytics-keyvault01.png)

![afbeelding van Azure sleutel kluis dashboard](./media/log-analytics-azure-keyvault/log-analytics-keyvault02.png)

### <a name="to-view-details-for-any-operation"></a>Om voor elke bewerking te bekijken

1. Klik op de pagina **Overzicht** op de tegel **Azure sleutel kluis** .
2. Neem de samenvattingsinformatie in een van de bladen op het dashboard **Azure sleutel kluis** , en klik op een om gedetailleerde informatie weergeven over het op de pagina aanmelden.

    U kunt de resultaten door de tijd, de gedetailleerde resultaten en de zoekgeschiedenis logboek bekijken op een logboek zoekpagina's. U kunt ook filteren op facetten om de resultaten te beperken.

## <a name="log-analytics-records"></a>Logboekrecords Analytics

De oplossing Azure sleutel kluis analyseert records met een soort **KeyVaults** die zijn verzameld via [AuditEvent logs](../key-vault/key-vault-logging.md) in Azure diagnostische gegevens.  Eigenschappen van deze records zijn in de volgende tabel.  

| Eigenschap | Beschrijving |
|:--|:--|
| Type | *KeyVaults* |
| SourceSystem | *AzureStorage* |
| CallerIpAddress | IP-adres van de client die de aanvraag hebben gedaan |
| Categorie      | Voor logboeken sleutel kluis is AuditEvent de waarde één, beschikbaar.|
| CorrelationId | Een optionele GUID die de client doorgeven kunt voor de bijbehorende client-side logboeken met Logboeken voor service-side (sleutel kluis). |
| DurationMs | Benodigde tijd om de REST API-aanvraag in milliseconden. Dit omvat geen netwerkvertraging, zodat de tijd die u op de client meet mogelijk niet overeen met deze tijd. |
| HttpStatusCode_d | HTTP-statuscode die wordt geretourneerd door de aanvraag |
| Id_s       | De unieke ID van de aanvraag |
| Identity_o | De identiteit van het token dat bij de REST API-aanvraag is ingediend. Dit is meestal een "gebruiker", 'service principal' of een combinatie 'gebruiker + appId' zoals in het geval van een aanvraag die voortvloeien uit een Azure PowerShell-cmdlet. |
| OperationName      | Naam van de bewerking, zoals beschreven in [Logboekregistratie van Azure sleutel kluis](../key-vault/key-vault-logging.md)|
| OperationVersion      | REST API versie door de client aangevraagde|
| RemoteIPLatitude | Latitude van de client die de aanvraag hebben gedaan |
| RemoteIPLongitude | Lengte van de client die de aanvraag hebben gedaan |
| RemoteIPCountry | Land van de client die de aanvraag hebben gedaan  |
| RequestUri_s | De URI van de aanvraag |
| Resource   | Naam van de sleutel kluis |
| ResourceGroup | De resourcegroep van de sleutel kluis |
| ResourceId | Azure Resource Manager Resource-ID. Voor logboeken sleutel kluis is dit altijd de sleutel kluis resource-ID. |
| ResourceProvider | *MICROSOFT. KEYVAULT* |
| ResultSignature  | HTTP-status|
| ResultType      | Resultaat van de REST-API|
| SubscriptionId | Azure abonnements-ID van het abonnement met de sleutel kluis |


## <a name="next-steps"></a>Volgende stappen

- Met [zoekacties in Analytics logboek logboek](log-analytics-log-searches.md) kunt u gedetailleerde gegevens van Azure sleutel kluis.
