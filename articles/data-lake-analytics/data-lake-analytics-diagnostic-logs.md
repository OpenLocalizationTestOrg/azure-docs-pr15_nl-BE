<properties 
   pageTitle="Diagnostische logboeken bekijken voor Azure gegevens Lake Analytics | Microsoft Azure" 
   description="Instelling en toegang tot diagnostische logboeken voor Azure gegevens Lake analytics begrijpen " 
   services="data-lake-analytics" 
   documentationCenter="" 
   authors="Blackmist" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="08/11/2016"
   ms.author="larryfr"/>

# <a name="accessing-diagnostic-logs-for-azure-data-lake-analytics"></a>Diagnostische logboeken openen voor Azure gegevens Lake Analytics

Meer informatie over het vastleggen van diagnostische gegevens voor uw account gegevens Lake Analytics inschakelen en het bekijken van de logboeken die zijn verzameld voor uw account.

Organisaties kunnen diagnostische logboekregistratie voor hun rekening Azure gegevens Lake Analytics voor het verzamelen van data access audittrails inschakelen. Deze logboeken geven informatie, zoals:

* Een lijst met gebruikers die toegang krijgen de gegevens tot.
* Hoe vaak de gegevens worden geopend.
* Hoeveel gegevens worden opgeslagen in de account.

## <a name="prerequisites"></a>Vereisten

- **Azure een abonnement**. Zie [Azure krijg gratis proefperiode](https://azure.microsoft.com/pricing/free-trial/).
- **Uw abonnement op Azure inschakelen** voor gegevens Lake Analytics Public Preview. Zie de [instructies](data-lake-analytics-get-started-portal.md#signup).
- **Azure gegevens Lake Analytics-account**. Volg de instructies op [aan de slag met Azure gegevens Lake Analytics met behulp van de portal Azure](data-lake-analytics-get-started-portal.md).

## <a name="enable-logging"></a>Logboekregistratie inschakelen

1. Aanmelden voor de nieuwe [portal Azure](https://portal.azure.com).

2. Open uw account gegevens Lake Analytics en uw account gegevens Lake Analytics blade, klik op **Instellingen**en klik op **Diagnostische instellingen**.

3. Breng de volgende wijzigingen voor het vastleggen van diagnostische gegevens configureren in de **Diagnostische** blade.

    ![Diagnostische logboekregistratie inschakelen] (./media/data-lake-analytics-diagnostic-logs/enable-diagnostic-logs.png "Diagnostische logboeken inschakelen")

    * Stel de **Status** **op** Diagnostische logboekregistratie inschakelen.
    * U kunt verwerken/winkel de gegevens op twee verschillende manieren.
        * Selecteer **exporteren naar een gebeurtenis Hub** stream logboekgegevens op een gebeurtenis Azure Hub. Gebruik deze optie als u een pijpleiding downstream-verwerkingen voor het analyseren van Logboeken voor binnenkomende in real-time. Als u deze optie selecteert, moet u de details opgeven voor de Azure gebeurtenis Hub die u wilt gebruiken.
        * Selecteer **exporteren naar opslag Account** voor het opslaan van Logboeken op een rekening Azure opslag. Gebruik deze optie als u wilt de gegevens archiveren. Als u deze optie selecteert, dient u een account Azure opslag om de logboekbestanden wilt opslaan.
    * Geef op of u wilt ophalen van controlelogboeken aanvraag logboeken en/of.
    * Geef het aantal dagen waarvoor de gegevens moeten worden bewaard.
    * Klik op **Opslaan**.

Nadat u de diagnostische instellingen hebt ingeschakeld, kunt u de logboeken op het tabblad **Diagnostische logboeken** bekijken.

## <a name="view-logs"></a>Logboeken weergeven

Er zijn twee manieren om de gegevens voor uw account gegevens Lake Analytics weer te geven.

* Uit de gegevens Lake Analytics Accountinstellingen
* Van de opslag van Azure rekening waarop de gegevens worden opgeslagen

### <a name="using-the-data-lake-analytics-settings-view"></a>Met behulp van de instellingen voor Lake Analytics-gegevens weergeven

1. Klik op **Diagnostische logboeken**uit uw gegevens Lake Analytics account **Instellingen** -blade.

    ![Registratie van diagnostische weergeven] (./media/data-lake-analytics-diagnostic-logs/view-diagnostic-logs.png "Diagnostische logboeken weergeven") 

2. De bladeserver **Diagnostische logboeken** ziet u de logboeken van **Controlelogboeken** en **Logboeken vragen**in categorieën onderverdeeld.
    * Aanvraag logboeken vastleggen voor elk verzoek op de account gegevens Lake Analytics API.
    * Controlelogboeken zijn vergelijkbaar met het aanvragen van logboek maar biedt een meer gedetailleerde uitsplitsing van de bewerkingen op de gegevens Lake Analytics-account. Een API-aanroep één upload in Logboeken van de aanvraag kan bijvoorbeeld resulteren in meerdere 'Append' bewerkingen in de controlelogboeken.

3. Klik op de koppeling **downloaden** voor het logboek een vermelding voor het downloaden van de logboeken.

### <a name="from-the-azure-storage-account-that-contains-log-data"></a>Vanuit de Azure opslag bevat dat logboekgegevens

1. De Azure opslag account bladeserver die is gekoppeld aan gegevens Lake Analytics voor logboekregistratie te openen en klik vervolgens op BLOB's. De blade **Blob-service** bevat twee containers.

    ![Registratie van diagnostische weergeven] (./media/data-lake-analytics-diagnostic-logs/view-diagnostic-logs-storage-account.png "Diagnostische logboeken weergeven")

    * De container **inzichten-logboeken-audit** bevat de controlelogboeken.
    * De container **inzichten-logboeken-aanvragen** bevat de aanvraag Logboeken.

2. Binnen deze containers, worden de logboekbestanden opgeslagen onder de volgende structuur.

        resourceId=/
          SUBSCRIPTIONS/
            <<SUBSCRIPTION_ID>>/
              RESOURCEGROUPS/
                <<RESOURCE_GRP_NAME>>/
                  PROVIDERS/
                    MICROSOFT.DATALAKEANALYTICS/
                      ACCOUNTS/
                        <DATA_LAKE_ANALYTICS_NAME>>/
                          y=####/
                            m=##/
                              d=##/
                                h=##/
                                  m=00/
                                    PT1H.json
    
    > [AZURE.NOTE] De `##` items in het pad bevatten het jaar, maand, dag en uur waarop het logboek is gemaakt. Gegevens Lake Analytics maakt een bestand elk uur, dus `m=` bevat altijd een waarde van `00`.

    Het volledige pad naar een controlelogboek kan bijvoorbeeld zijn:
    
        https://adllogs.blob.core.windows.net/insights-logs-audit/resourceId=/SUBSCRIPTIONS/<sub-id>/RESOURCEGROUPS/myresourcegroup/PROVIDERS/MICROSOFT.DATALAKEANALYTICS/ACCOUNTS/mydatalakeanalytics/y=2016/m=07/d=18/h=04/m=00/PT1H.json

    Ook kan het volledige pad naar een logboek voor aanvraag zijn:
    
        https://adllogs.blob.core.windows.net/insights-logs-requests/resourceId=/SUBSCRIPTIONS/<sub-id>/RESOURCEGROUPS/myresourcegroup/PROVIDERS/MICROSOFT.DATALAKEANALYTICS/ACCOUNTS/mydatalakeanalytics/y=2016/m=07/d=18/h=14/m=00/PT1H.json

## <a name="log-structure"></a>Logboek-structuur

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
             "resourceId": "/SUBSCRIPTIONS/<subscription_id>/RESOURCEGROUPS/<resource_group_name>/PROVIDERS/MICROSOFT.DATALAKEANALYTICS/ACCOUNTS/<data_lake_analytics_account_name>",
             "category": "Requests",
             "operationName": "GetAggregatedJobHistory",
             "resultType": "200",
             "callerIpAddress": "::ffff:1.1.1.1",
             "correlationId": "4a11c709-05f5-417c-a98d-6e81b3e29c58",
             "identity": "1808bd5f-62af-45f4-89d8-03c5e81bac30",
             "properties": {
                 "HttpMethod":"POST",
                 "Path":"/JobAggregatedHistory",
                 "RequestContentLength":122,
                 "ClientRequestId":"3b7adbd9-3519-4f28-a61c-bd89506163b8",
                 "StartTime":"2016-07-07T21:02:52.472Z",
                 "EndTime":"2016-07-07T21:02:53.456Z"
                 }
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
| operationName   | Tekenreeks | De naam van de bewerking die wordt geregistreerd. Bijvoorbeeld GetAggregatedJobHistory.              |
| resultType      | Tekenreeks | De status van de bewerking, bijvoorbeeld 200.                                 |
| callerIpAddress | Tekenreeks | Het IP-adres van de client die de aanvraag doet                                |
| correlationId   | Tekenreeks | De id van het logboek. Deze waarde kan worden gebruikt voor een aantal verwante logboekvermeldingen groeperen |
| identiteit        | Object | De identiteit die in het logboek is gegenereerd                                            |
| Eigenschappen      | JSON   | Zie voor meer informatie het volgende gedeelte (aanvraag logboek eigenschappen schema) |

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
             "time": "2016-07-28T19:15:16.245Z",
             "resourceId": "/SUBSCRIPTIONS/<subscription_id>/RESOURCEGROUPS/<resource_group_name>/PROVIDERS/MICROSOFT.DATALAKEANALYTICS/ACCOUNTS/<data_lake_ANALYTICS_account_name>",
             "category": "Audit",
             "operationName": "JobSubmitted",
             "identity": "user@somewhere.com",
             "properties": {
                 "JobId":"D74B928F-5194-4E6C-971F-C27026C290E6",
                 "JobName": "New Job", 
                 "JobRuntimeName": "default",
                 "SubmitTime": "7/28/2016 7:14:57 PM"
                 }
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
| operationName   | Tekenreeks | De naam van de bewerking die wordt geregistreerd. Bijvoorbeeld JobSubmitted.              |
| resultType | Tekenreeks | Een substatus voor de status van de (operationName). |
| resultSignature | Tekenreeks | Meer informatie over de status van de taak (operationName). |
| identiteit      | Tekenreeks | De gebruiker die de bewerking aangevraagd. Bijvoorbeeld susan@contoso.com.                                 |
| Eigenschappen      | JSON   | Zie voor meer informatie het volgende gedeelte (Audit log eigenschappen schema) |

> [AZURE.NOTE] __resultType__ en __resultSignature__ bevatten informatie over het resultaat van een bewerking, en alleen een waarde bevatten als een bewerking is voltooid. Ze bevatten bijvoorbeeld een waarde wanneer __operationName__ een waarde van __JobStarted__ of __JobEnded__bevat.

#### <a name="audit-log-properties-schema"></a>Schema voor logboek eigenschappen audit

| Naam       | Type   | Beschrijving                              |
|------------|--------|------------------------------------------|
| Taak-id | Tekenreeks | De ID die is toegewezen aan het project  |
| Taaknaam | Tekenreeks | De naam die is opgegeven voor het project |
| JobRunTime | Tekenreeks | De runtime gebruikt voor het verwerken van de taak |
| SubmitTime | Tekenreeks | De UTC-tijd () dat de taak is ingediend. |
| Starttijd | Tekenreeks | De tijd dat de taak is gestart na ontvangst (UTC). |
| Eindtijd | Tekenreeks | Het tijdstip waarop de taak is beëindigd. |
| Parallellisme | Tekenreeks | Het aantal voor deze taak gevraagd tijdens het indienen van gegevens Lake Analytics-eenheden. |

> [AZURE.NOTE] __SubmitTime__, __Begintijd__, __eindtijd__ en __parallellisme__ bevatten informatie voor een bewerking, en alleen een waarde bevatten als een bewerking heeft gestart of voltooid. __SubmitTime__ bevat bijvoorbeeld een waarde na __operationName__ __JobSubmitted__geeft.

## <a name="process-the-log-data"></a>De logboekgegevens worden verwerkt

Azure gegevens Lake Analytics biedt een voorbeeld voor het verwerken en analyseren van gegevens. In het voorbeeld kunt u vinden op [https://github.com/Azure/AzureDataLake/tree/master/Samples/AzureDiagnosticsSample](https://github.com/Azure/AzureDataLake/tree/master/Samples/AzureDiagnosticsSample). 


## <a name="next-steps"></a>Volgende stappen

- [Overzicht van Azure gegevens Lake Analytics](data-lake-analytics-overview.md)


