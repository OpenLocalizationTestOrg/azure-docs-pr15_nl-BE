<properties 
   pageTitle="Diagnostische logboeken voor Azure gegevens Lake winkel bekijken | Microsoft Azure" 
   description="Weten hoe u kunt instellen en de toegang tot diagnostische logboeken voor Azure Lake gegevensarchief " 
   services="data-lake-store" 
   documentationCenter="" 
   authors="nitinme" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="10/05/2016"
   ms.author="nitinme"/>

# <a name="accessing-diagnostic-logs-for-azure-data-lake-store"></a>Toegang tot diagnostische logboeken voor Azure Lake gegevensarchief

Meer informatie over het vastleggen van diagnostische gegevens voor uw account Lake gegevensarchief inschakelen en het bekijken van de logboeken die zijn verzameld voor uw account.

Organisaties kunnen diagnostische logboekregistratie inschakelen voor hun account Azure Lake gegevensarchief audittrails met access-gegevens verzamelen met informatie zoals de lijst van gebruikers met toegang tot de gegevens, hoe vaak de gegevens worden geopend, hoeveel gegevens worden opgeslagen in de account, enz.

## <a name="prerequisites"></a>Vereisten

- **Azure een abonnement**. Zie [Azure krijg gratis proefperiode](https://azure.microsoft.com/pricing/free-trial/).

- **Azure Lake gegevensarchief account**. Volg de instructies op de [aan de slag met Azure Lake gegevensarchief maken met de Portal Azure](data-lake-store-get-started-portal.md).

## <a name="enable-diagnostic-logging-for-your-data-lake-store-account"></a>Diagnostische logboekregistratie inschakelen voor uw account gegevensarchief Lake

1. Aanmelden voor de nieuwe [Portal Azure](https://portal.azure.com).

2. Open uw account Lake gegevensopslag en uit uw account Lake gegevensarchief blade, klikt u op **Instellingen**en klik op **Diagnostische instellingen**.

3. Breng de volgende wijzigingen voor het vastleggen van diagnostische gegevens configureren in de **Diagnostische** blade.

    ![Diagnostische logboekregistratie inschakelen] (./media/data-lake-store-diagnostic-logs/enable-diagnostic-logs.png "Diagnostische logboeken inschakelen")

    * Stel de **Status** **op** Diagnostische logboekregistratie inschakelen.
    * U kunt verwerken/winkel de gegevens op twee verschillende manieren.
        * Selecteer de optie exporteren **naar een gebeurtenis Hub** stream logboek gegevens naar een gebeurtenis Azure Hub. Waarschijnlijk gebruikt u deze optie als u een pijpleiding downstream-verwerkingen voor het analyseren van Logboeken voor binnenkomende in real-time. Als u deze optie selecteert, moet u de details opgeven voor de Azure gebeurtenis Hub die u wilt gebruiken.
        * Selecteer de optie exporteren **naar opslag Account** voor het opslaan van Logboeken op een rekening Azure opslag. U kunt deze optie gebruiken als u wilt dat de gegevens die batch verwerkt op een later tijdstip zijn. Als u deze optie selecteert, moet u een account Azure opslag om de logboeken te slaan opgeven.
    * Geef op of u wilt ophalen van controlelogboeken aanvraag logboeken en/of.
    * Geef het aantal dagen waarvoor de gegevens moeten worden bewaard.
    * Klik op **Opslaan**.

Nadat u de diagnostische instellingen hebt ingeschakeld, kunt u de logboeken op het tabblad **Diagnostische logboeken** bekijken.

## <a name="view-diagnostic-logs-for-your-data-lake-store-account"></a>Diagnostische logboeken weergeven voor uw account gegevensarchief Lake

Er zijn twee manieren om gegevens voor uw account Lake gegevensarchief weer te geven.

* Uit het gegevensarchief Lake account weergeven instellingen
* Van de opslag van Azure rekening waarop de gegevens worden opgeslagen

### <a name="using-the-data-lake-store-settings-view"></a>Met behulp van de weergave gegevens Lake winkelinstellingen

1. Klik op **Diagnostische logboeken**uit het gegevensarchief Lake account **Instellingen** -blade.

    ![Registratie van diagnostische weergeven] (./media/data-lake-store-diagnostic-logs/view-diagnostic-logs.png "Diagnostische logboeken weergeven") 

2. De bladeserver **Diagnostische logboeken** ziet u de logboeken van **Controlelogboeken** en **Logboeken vragen**in categorieën onderverdeeld.
    * Aanvraag logboeken vastleggen elk verzoek API op de rekening Lake gegevensarchief.
    * Controlelogboeken zijn vergelijkbaar met het aanvragen van logboek maar biedt een meer gedetailleerde uitsplitsing van de bewerkingen op de rekening van het gegevensarchief Lake. Een API-aanroep één upload in Logboeken van de aanvraag kan bijvoorbeeld resulteren in meerdere 'Append' bewerkingen in de controlelogboeken.

3. Klik op de koppeling **downloaden** tegen elke logboekvermelding te downloaden van de logboeken.

### <a name="from-the-azure-storage-account-that-contains-log-data"></a>Vanuit de Azure opslag bevat dat logboekgegevens

1. Open de opslag Azure account blade Lake gegevensarchief voor logboekregistratie gekoppeld en klik vervolgens op BLOB's. De blade **Blob-service** bevat twee containers.

    ![Registratie van diagnostische weergeven] (./media/data-lake-store-diagnostic-logs/view-diagnostic-logs-storage-account.png "Diagnostische logboeken weergeven")

    * De container **inzichten-logboeken-audit** bevat de controlelogboeken.
    * De container **inzichten-logboeken-aanvragen** bevat de aanvraag Logboeken.

2. Binnen deze containers, worden de logboekbestanden opgeslagen onder de volgende structuur.

    ![Registratie van diagnostische weergeven] (./media/data-lake-store-diagnostic-logs/view-diagnostic-logs-storage-account-structure.png "Diagnostische logboeken weergeven")

    Als u bijvoorbeeld kan het volledige pad naar een controlelogboek worden`https://adllogs.blob.core.windows.net/insights-logs-audit/resourceId=/SUBSCRIPTIONS/<sub-id>/RESOURCEGROUPS/myresourcegroup/PROVIDERS/MICROSOFT.DATALAKESTORE/ACCOUNTS/mydatalakestore/y=2016/m=07/d=18/h=04/m=00/PT1H.json`

    Similary, het volledige pad naar een logboek voor aanvraag kan worden`https://adllogs.blob.core.windows.net/insights-logs-requests/resourceId=/SUBSCRIPTIONS/<sub-id>/RESOURCEGROUPS/myresourcegroup/PROVIDERS/MICROSOFT.DATALAKESTORE/ACCOUNTS/mydatalakestore/y=2016/m=07/d=18/h=14/m=00/PT1H.json`

## <a name="understand-the-structure-of-the-log-data"></a>Inzicht in de structuur van de logboekgegevens

De logboeken controleren en aanvragen zijn in JSON-indeling. In deze sectie, we de structuur van een JSON voor aanvraag bekijken en controlelogboekbestanden.

### <a name="request-logs"></a>Aanvragen van logboek

Hier is een voorbeeld-vermelding in het logboek voor aanvraag JSON-indeling. Elke blob heeft een hoofdobject **records** met een array van objecten in logboek genoemd.

    {
    "records": 
      [     
        . . . .
        ,
        {
             "time": "2016-07-07T21:02:53.456Z",
             "resourceId": "/SUBSCRIPTIONS/<subscription_id>/RESOURCEGROUPS/<resource_group_name>/PROVIDERS/MICROSOFT.DATALAKESTORE/ACCOUNTS/<data_lake_store_account_name>",
             "category": "Requests",
             "operationName": "GETCustomerIngressEgress",
             "resultType": "200",
             "callerIpAddress": "::ffff:1.1.1.1",
             "correlationId": "4a11c709-05f5-417c-a98d-6e81b3e29c58",
             "identity": "1808bd5f-62af-45f4-89d8-03c5e81bac30",
             "properties": {"HttpMethod":"GET","Path":"/webhdfs/v1/Samples/Outputs/Drivers.csv","RequestContentLength":0,"ClientRequestId":"3b7adbd9-3519-4f28-a61c-bd89506163b8","StartTime":"2016-07-07T21:02:52.472Z","EndTime":"2016-07-07T21:02:53.456Z"}
        }
        ,
        . . . .
      ]
    }

#### <a name="request-log-schema"></a>Schema voor logboek aanvragen

| Naam            | Type   | Beschrijving                                                                    |
|-----------------|--------|--------------------------------------------------------------------------------|
| tijd            | Tekenreeks | De tijdstempel (in UTC) van het logboek                                              |
| resourceId      | Tekenreeks | De ID van de resource die werd op plaatsen                            |
| categorie        | Tekenreeks | De categorie aanmelden. Bijvoorbeeld **aanvragen**.                                   |
| operationName   | Tekenreeks | De naam van de bewerking die wordt geregistreerd. Bijvoorbeeld getfilestatus.              |
| resultType      | Tekenreeks | De status van de bewerking, bijvoorbeeld 200.                                 |
| callerIpAddress | Tekenreeks | Het IP-adres van de client die de aanvraag doet                                |
| correlationId   | Tekenreeks | De id van het logboek dat kan worden gebruikt een reeks verwante logboekvermeldingen groeperen |
| identiteit        | Object | De identiteit die in het logboek is gegenereerd                                            |
| Eigenschappen      | JSON   | Zie hieronder voor details                                                          |

#### <a name="request-log-properties-schema"></a>Schema voor logboek eigenschappen aanvragen

| Naam                 | Type   | Beschrijving                                               |
|----------------------|--------|-----------------------------------------------------------|
| HttpMethod           | Tekenreeks | De HTTP-methode gebruikt voor de bewerking. Zo krijgen. |
| Pad                 | Tekenreeks | Het pad van de bewerking is uitgevoerd op                   |
| RequestContentLength | int    | De lengte van de inhoud van de HTTP-aanvraag                    |
| ClientRequestId      | Tekenreeks | De Id die een unieke aanduiding voor deze aanvraag              |
| Starttijd            | Tekenreeks | De tijd waarop de server de aanvraag ontvangen         |
| Eindtijd              | Tekenreeks | De tijd waarop de server een antwoord verzonden              |

### <a name="audit-logs"></a>Controlelogboeken

Hier is een voorbeeld-item in het controlelogboek met JSON-indeling. Elke blob is een basis-object met de naam **records** met een array van objecten in logboek

    {
    "records": 
      [     
        . . . .
        ,
        {
             "time": "2016-07-08T19:08:59.359Z",
             "resourceId": "/SUBSCRIPTIONS/<subscription_id>/RESOURCEGROUPS/<resource_group_name>/PROVIDERS/MICROSOFT.DATALAKESTORE/ACCOUNTS/<data_lake_store_account_name>",
             "category": "Audit",
             "operationName": "SeOpenStream",
             "resultType": "0",
             "correlationId": "381110fc03534e1cb99ec52376ceebdf;Append_BrEKAmg;25.66.9.145",
             "identity": "A9DAFFAF-FFEE-4BB5-A4A0-1B6CBBF24355",
             "properties": {"StreamName":"adl://<data_lake_store_account_name>.azuredatalakestore.net/logs.csv"}
        }
        ,
        . . . .
      ]
    }

#### <a name="audit-log-schema"></a>Schema voor logboek met controle

| Naam            | Type   | Beschrijving                                                                    |
|-----------------|--------|--------------------------------------------------------------------------------|
| tijd            | Tekenreeks | De tijdstempel (in UTC) van het logboek                                              |
| resourceId      | Tekenreeks | De ID van de resource die werd op plaatsen                            |
| categorie        | Tekenreeks | De categorie aanmelden. Bijvoorbeeld **controle**.                                      |
| operationName   | Tekenreeks | De naam van de bewerking die wordt geregistreerd. Bijvoorbeeld getfilestatus.              |
| resultType      | Tekenreeks | De status van de bewerking, bijvoorbeeld 200.                                 |
| correlationId   | Tekenreeks | De id van het logboek dat kan worden gebruikt een reeks verwante logboekvermeldingen groeperen |
| identiteit        | Object | De identiteit die in het logboek is gegenereerd                                            |
| Eigenschappen      | JSON   | Zie hieronder voor details                                                          |

#### <a name="audit-log-properties-schema"></a>Schema voor logboek eigenschappen audit

| Naam       | Type   | Beschrijving                              |
|------------|--------|------------------------------------------|
| StreamName | Tekenreeks | Het pad van de bewerking is uitgevoerd op  |


## <a name="samples-to-process-the-log-data"></a>Voorbeelden voor het verwerken van de logboekgegevens

Azure Lake gegevensarchief hier een voorbeeld hoe te verwerken en analyseren van gegevens. In het voorbeeld kunt u vinden op [https://github.com/Azure/AzureDataLake/tree/master/Samples/AzureDiagnosticsSample](https://github.com/Azure/AzureDataLake/tree/master/Samples/AzureDiagnosticsSample). 


## <a name="see-also"></a>Zie ook

- [Overzicht van Azure Lake gegevensarchief](data-lake-store-overview.md)
- [Beveiliging van gegevens in het gegevensarchief Lake](data-lake-store-secure-data.md)

