<properties 
   pageTitle="Aan de slag met Data Lake Analytics met REST API | Microsoft Azure" 
   description="WebHDFS REST API's gebruiken voor het uitvoeren van bewerkingen op de gegevens Lake Analytics" 
   services="data-lake-analytics" 
   documentationCenter="" 
   authors="mumian" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="10/19/2016"
   ms.author="jgao"/>

# <a name="get-started-with-azure-data-lake-analytics-using-rest-apis"></a>Aan de slag met Azure gegevens Lake Analytics met REST API 's

[AZURE.INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]

Informatie over het WebHDFS REST API's en gegevens Lake Analytics REST API's gebruiken voor het beheren van accounts, taken en catalogus gegevens Lake Analytics. 

## <a name="prerequisites"></a>Vereisten

- **Azure een abonnement**. Zie [Azure krijg gratis proefperiode](https://azure.microsoft.com/pricing/free-trial/).

- **Een Azure Active Directory-toepassing maken**. Met de Azure AD-toepassing kunt u de toepassing gegevens Lake Analytics met Azure Active Directory te verifiëren. Er zijn verschillende benaderingen voor de verificatie van AD Azure, **eindgebruikers** of **verificatie services zijn**. Zie voor instructies en meer informatie over het verifiëren [verifiëren met gegevens Lake Analytics met Azure Active Directory](../data-lake-store/data-lake-store-authenticate-using-active-directory.md).

- [cURL](http://curl.haxx.se/). In dit artikel gebruikt cURL om aan te tonen hoe u de REST API-aanroepen tegen een account gegevens Lake Analytics.

## <a name="authenticate-with-azure-active-directory"></a>Verifiëren met Azure Active Directory

Er zijn twee methoden voor het verifiëren van Azure Active Directory.

### <a name="end-user-authentication-interactive"></a>Eindgebruiker-verificatie (interactief)

Met deze methode wordt gevraagd of de gebruiker zich aanmeldt en alle bewerkingen worden uitgevoerd in de context van de gebruiker. 

Volg deze stappen voor de interactieve verificatie:

1. Door de toepassing, moet u de gebruiker omleiden naar de volgende URL:

        https://login.microsoftonline.com/<TENANT-ID>/oauth2/authorize?client_id=<CLIENT-ID>&response_type=code&redirect_uri=<REDIRECT-URI>

    >[AZURE.NOTE] \<REDIRECT URI > moet worden gecodeerd voor gebruik in een URL. Gebruik dus voor https://localhost, `https%3A%2F%2Flocalhost`)

    U kunt voor de toepassing van deze zelfstudie, vervangt u de tijdelijke aanduiding voor waarden in de URL hierboven en plak deze in de adresbalk van de webbrowser. U wordt omgeleid om te verifiëren met behulp van uw aanmelding Azure. Zodra u met succes bent ingelogd, wordt het antwoord weergegeven in de adresbalk van de browser. Het antwoord niet in de volgende notatie:
        
        http://localhost/?code=<AUTHORIZATION-CODE>&session_state=<GUID>

2. Vastleggen van de code van de vergunning van het antwoord. U kunt voor deze zelfstudie, Kopieer de code van de vergunning uit de adresbalk van de webbrowser en doorgegeven in de POST-aanvraag aan het token eindpunt, zoals hieronder wordt weergegeven:

        curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token \
        -F redirect_uri=<REDIRECT-URI> \
        -F grant_type=authorization_code \
        -F resource=https://management.core.windows.net/ \
        -F client_id=<CLIENT-ID> \
        -F code=<AUTHORIZATION-CODE>

    >[AZURE.NOTE] In dit geval de \<REDIRECT URI > moet niet worden gecodeerd.

3. Het antwoord is een JSON-object dat een toegangstoken bevat (bijv. `"access_token": "<ACCESS_TOKEN>"`) en een token vernieuwen (bijv. `"refresh_token": "<REFRESH_TOKEN>"`). De toepassing gebruikt het toegangstoken Azure Lake gegevensopslag en de token vernieuwd naar een andere access-token ophalen als een toegangstoken verloopt.

        {"token_type":"Bearer","scope":"user_impersonation","expires_in":"3599","expires_on":"1461865782","not_before": "1461861882","resource":"https://management.core.windows.net/","access_token":"<REDACTED>","refresh_token":"<REDACTED>","id_token":"<REDACTED>"}

4.  Wanneer het toegangstoken is verlopen, kunt u een nieuwe access token met behulp van het token vernieuwen aanvragen, zoals hieronder wordt weergegeven:

         curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token  \
            -F grant_type=refresh_token \
            -F resource=https://management.core.windows.net/ \
            -F client_id=<CLIENT-ID> \
            -F refresh_token=<REFRESH-TOKEN>
 
Zie voor meer informatie over interactieve gebruikersverificatie [autorisatiecode stroom te verlenen](https://msdn.microsoft.com/library/azure/dn645542.aspx).

### <a name="service-to-service-authentication-non-interactive"></a>Services verificatie (niet-interactieve)

Met deze methode wordt de toepassing biedt eigen referenties voor het uitvoeren van bewerkingen. Hiervoor moet u een POST-aanvraag, zoals in de onderstaande afgeven: 

    curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token  \
      -F grant_type=client_credentials \
      -F resource=https://management.core.windows.net/ \
      -F client_id=<CLIENT-ID> \
      -F client_secret=<AUTH-KEY>

De uitvoer van deze aanvraag omvat Autorisatietoken (aangeduid met `access-token` in de uitvoer hieronder) die u vervolgens met de REST API-aanroepen worden doorgegeven. Deze verificatietoken opslaat in een tekstbestand; Dit moet u verderop in dit artikel.

    {"token_type":"Bearer","expires_in":"3599","expires_on":"1458245447","not_before":"1458241547","resource":"https://management.core.windows.net/","access_token":"<REDACTED>"}

In dit artikel wordt de **niet-interactieve** benadering. Zie voor meer informatie over niet-interactieve (service-service-oproepen) [-Service met de referenties van serviceoproepen](https://msdn.microsoft.com/library/azure/dn645543.aspx).
## <a name="create-a-data-lake-analytics-account"></a>Maak een account gegevens Lake Analytics

Moet u een resourcegroep Azure en een gegevensarchief Lake account voordat u een account gegevens Lake Analytics kunt maken.  Zie [een gegevensarchief Lake-account maken](../data-lake-store/data-lake-store-get-started-rest-api.md#create-a-data-lake-store-account).

De volgende Curl-opdracht ziet u hoe u een account maken:

    curl -i -X PUT -H "Authorization: Bearer <REDACTED>" -H "Content-Type: application/json" https://management.azure.com/subscriptions/<AzureSubscriptionID>/resourceGroups/<AzureResourceGroupName>/providers/Microsoft.DataLakeAnalytics/accounts/<NewAzureDataLakeAnalyticsAccountName>?api-version=2016-11-01 -d@"C:\tutorials\adla\CreateDataLakeAnalyticsAccountRequest.json"

Vervangen \< `REDACTED` \> met de Autorisatietoken \< `AzureSubscriptionID` \> met uw abonnement-ID, \< `AzureResourceGroupName` \> met de naam van een bestaande resourcegroep Azure, en \< `NewAzureDataLakeAnalyticsAccountName` \> met een nieuwe naam voor de gegevens Lake Analytics-Account. De nettolading van de aanvraag voor deze opdracht bevindt zich in het **CreateDatalakeAnalyticsAccountRequest.json** -bestand dat is opgegeven voor de `-d` parameter hierboven. De inhoud van het bestand input.json als de volgende:

    {  
        "location": "East US 2",  
        "name": "myadla1004",  
        "tags": {},  
        "properties": {  
            "defaultDataLakeStoreAccount": "my1004store",  
            "dataLakeStoreAccounts": [  
                {  
                    "name": "my1004store"  
                }     
            ]
        }  
    }  


## <a name="list-data-lake-analytics-accounts-in-a-subscription"></a>Lijst met gegevens Lake Analytics-accounts in een abonnement

De volgende Curl-opdracht ziet u hoe u accounts in een abonnement:

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" https://management.azure.com/subscriptions/<AzureSubscriptionID>/providers/Microsoft.DataLakeAnalytics/Accounts?api-version=2016-11-01

Vervangen \< `REDACTED` \> met de Autorisatietoken \< `AzureSubscriptionID` \> met uw abonnement-ID. De uitvoer is vergelijkbaar met:

    {
        "value": [
            {
            "properties": {
                "provisioningState": "Succeeded",
                "state": "Active",
                "endpoint": "myadla0831.azuredatalakeanalytics.net",
                "accountId": "21e74660-0941-4880-ae72-b143c2615ea9",
                "creationTime": "2016-09-01T12:49:12.7451428Z",
                "lastModifiedTime": "2016-09-01T12:49:12.7451428Z"
            },
            "location": "East US 2",
            "tags": {},
            "id": "/subscriptions/65a1016d-0f67-45d2-b838-b8f373d6d52e/resourceGroups/myadla0831rg/providers/Microsoft.DataLakeAnalytics/accounts/myadla0831",
            "name": "myadla0831",
            "type": "Microsoft.DataLakeAnalytics/accounts"
            },
            {
            "properties": {
                "provisioningState": "Succeeded",
                "state": "Active",
                "endpoint": "myadla1004.azuredatalakeanalytics.net",
                "accountId": "3ff9b93b-11c4-43c6-83cc-276292eeb350",
                "creationTime": "2016-10-04T20:46:42.287147Z",
                "lastModifiedTime": "2016-10-04T20:46:42.287147Z"
            },
            "location": "East US 2",
            "tags": {},
            "id": "/subscriptions/65a1016d-0f67-45d2-b838-b8f373d6d52e/resourceGroups/myadla1004rg/providers/Microsoft.DataLakeAnalytics/accounts/myadla1004",
            "name": "myadla1004",
            "type": "Microsoft.DataLakeAnalytics/accounts"
            }
        ]
    }

## <a name="get-information-about-a-data-lake-analytics-account"></a>Informatie over een account gegevens Lake Analytics

De volgende Curl-opdracht ziet u hoe u een account-informatie:

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" https://management.azure.com/subscriptions/<AzureSubscriptionID>/resourceGroups/<AzureResourceGroupName>/providers/Microsoft.DataLakeAnalytics/accounts/<DataLakeAnalyticsAccountName>?api-version=2015-11-01

Vervangen \< `REDACTED` \> met de Autorisatietoken \< `AzureSubscriptionID` \> met uw abonnement-ID, \< `AzureResourceGroupName` \> met de naam van een bestaande resourcegroep Azure, en \< `DataLakeAnalyticsAccountName` \> met de naam van een bestaande gegevens Lake Analytics-Account. De uitvoer is vergelijkbaar met:

    {
        "properties": {
            "defaultDataLakeStoreAccount": "my1004store",
            "dataLakeStoreAccounts": [
            {
                "properties": {
                "suffix": "azuredatalakestore.net"
                },
                "name": "my1004store"
            }
            ],
            "provisioningState": "Creating",
            "state": null,
            "endpoint": null,
            "accountId": "3ff9b93b-11c4-43c6-83cc-276292eeb350",
            "creationTime": null,
            "lastModifiedTime": null
        },
        "location": "East US 2",
        "tags": {},
        "id": "/subscriptions/65a1016d-0f67-45d2-b838-b8f373d6d52e/resourceGroups/myadla1004rg/providers/Microsoft.DataLakeAnalytics/accounts/myadla1004",
        "name": "myadla1004",
        "type": "Microsoft.DataLakeAnalytics/accounts"
    }

## <a name="list-data-lake-stores-of-a-data-lake-analytics-account"></a>Lijst gegevensarchieven Lake van een account gegevens Lake Analytics

De volgende Curl-opdracht ziet u hoe u meer opgeslagen gegevens van een account:

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" https://management.azure.com/subscriptions/<AzureSubscriptionID>/resourceGroups/<AzureResourceGroupName>/providers/Microsoft.DataLakeAnalytics/accounts/<DataLakeAnalyticsAccountName>/DataLakeStoreAccounts/?api-version=2016-11-01

Vervangen \< `REDACTED` \> met de Autorisatietoken \< `AzureSubscriptionID` \> met uw abonnement-ID, \< `AzureResourceGroupName` \> met de naam van een bestaande resourcegroep Azure, en \< `DataLakeAnalyticsAccountName` \> met de naam van een bestaande gegevens Lake Analytics-Account. De uitvoer is vergelijkbaar met:

    {
        "value": [
            {
            "properties": {
                "suffix": "azuredatalakestore.net"
            },
            "id": "/subscriptions/65a1016d-0f67-45d2-b838-b8f373d6d52e/resourceGroups/myadla1004rg/providers/Microsoft.DataLakeAnalytics/accounts/myadla1004/dataLakeStoreAccounts/my1004store",
            "name": "my1004store",
            "type": "Microsoft.DataLakeAnalytics/accounts/dataLakeStoreAccounts"
            }
        ]
    }

## <a name="submit-u-sql-jobs"></a>U SQL-taken

De volgende Curl-opdracht ziet u hoe een U-SQL-taak verzenden:

    curl -i -X PUT -H "Authorization: Bearer <REDACTED>" https://<DataLakeAnalyticsAccountName>.azuredatalakeanalytics.net/Jobs/<NewGUID>?api-version=2016-03-20-preview -d@"C:\tutorials\adla\SubmitADLAJob.json"

Vervangen \< `REDACTED` \> met de Autorisatietoken \< `DataLakeAnalyticsAccountName` \> met de naam van een bestaande gegevens Lake Analytics-Account. De nettolading van de aanvraag voor deze opdracht bevindt zich in het **SubmitADLAJob.json** -bestand dat is opgegeven voor de `-d` parameter hierboven. De inhoud van het bestand input.json als de volgende:

    {
        "jobId": "8f8ebf8c-4b63-428a-ab46-a03d2cc5b65a",
        "name": "convertTSVtoCSV",
        "type": "USql",
        "degreeOfParallelism": 1,
        "priority": 1000,
        "properties": {
            "type": "USql",
            "script": "@searchlog =\n    EXTRACT UserId          int,\n            Start           DateTime,\n            Region          string,\n            Query          
        string,\n            Duration        int?,\n            Urls            string,\n            ClickedUrls     string\n    FROM \"/Samples/Data/SearchLog.tsv\"\n    US
        ING Extractors.Tsv();\n\nOUTPUT @searchlog   \n    TO \"/Output/SearchLog-from-Data-Lake.csv\"\nUSING Outputters.Csv();"
        }
    }

De uitvoer is vergelijkbaar met:

    {
        "jobId": "8f8ebf8c-4b63-428a-ab46-a03d2cc5b65a",
        "name": "convertTSVtoCSV",
        "type": "USql",
        "submitter": "myadl@SPI",
        "degreeOfParallelism": 1,
        "priority": 1000,
        "submitTime": "2016-10-05T13:54:59.9871859+00:00",
        "state": "Compiling",
        "result": "Succeeded",
        "stateAuditRecords": [
            {
            "newState": "New",
            "timeStamp": "2016-10-05T13:54:59.9871859+00:00",
            "details": "userName:myadl@SPI;submitMachine:N/A"
            }
        ],
        "properties": {
            "owner": "myadl@SPI",
            "resources": [],
            "runtimeVersion": "default",
            "rootProcessNodeId": "00000000-0000-0000-0000-000000000000",
            "algebraFilePath": "adl://myadls0831.azuredatalakestore.net/system/jobservice/jobs/Usql/2016/10/05/13/54/8f8ebf8c-4b63-428a-ab46-a03d2cc5b65a/algebra.xml",
            "compileMode": "Semantic",
            "errorSource": "Unknown",
            "totalCompilationTime": "PT0S",
            "totalPausedTime": "PT0S",
            "totalQueuedTime": "PT0S",
            "totalRunningTime": "PT0S",
            "type": "USql"
        }
    }


## <a name="list-u-sql-jobs"></a>U SQL-taken weergeven

De volgende Curl-opdracht ziet u hoe U SQL-taken:

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" https://<DataLakeAnalyticsAccountName>.azuredatalakeanalytics.net/Jobs?api-version=2016-11-01 

Vervangen \< `REDACTED` \> met de Autorisatietoken en \< `DataLakeAnalyticsAccountName` \> met de naam van een bestaande gegevens Lake Analytics-Account. 


De uitvoer is vergelijkbaar met:

    {
    "value": [
        {
        "jobId": "65cf1691-9dbe-43cd-90ed-1cafbfb406fb",
        "name": "convertTSVtoCSV",
        "type": "USql",
        "submitter": "someone@microsoft.com",
        "account": null,
        "degreeOfParallelism": 1,
        "priority": 1000,
        "submitTime": "Wed, 05 Oct 2016 13:46:53 GMT",
        "startTime": "Wed, 05 Oct 2016 13:47:33 GMT",
        "endTime": "Wed, 05 Oct 2016 13:48:07 GMT",
        "state": "Ended",
        "result": "Succeeded",
        "errorMessage": null,
        "storageAccounts": null,
        "stateAuditRecords": null,
        "logFilePatterns": null,
        "properties": null
        },
        {
        "jobId": "8f8ebf8c-4b63-428a-ab46-a03d2cc5b65a",
        "name": "convertTSVtoCSV",
        "type": "USql",
        "submitter": "someoneadl@SPI",
        "account": null,
        "degreeOfParallelism": 1,
        "priority": 1000,
        "submitTime": "Wed, 05 Oct 2016 13:54:59 GMT",
        "startTime": "Wed, 05 Oct 2016 13:55:43 GMT",
        "endTime": "Wed, 05 Oct 2016 13:56:11 GMT",
        "state": "Ended",
        "result": "Succeeded",
        "errorMessage": null,
        "storageAccounts": null,
        "stateAuditRecords": null,
        "logFilePatterns": null,
        "properties": null
        }
    ],
    "nextLink": null,
    "count": null
    }


## <a name="get-catalog-items"></a>Catalogus artikelen

De volgende Curl-opdracht ziet u hoe u de databases van de catalogus:

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" https://<DataLakeAnalyticsAccountName>.azuredatalakeanalytics.net/catalog/usql/databases?api-version=2016-11-01

De uitvoer is vergelijkbaar met:

    {
    "@odata.context":"https://myadla0831.azuredatalakeanalytics.net/sqlip/$metadata#databases","value":[
        {
        "computeAccountName":"myadla0831","databaseName":"mytest","version":"f6956327-90b8-4648-ad8b-de3ff09274ea"
        },{
        "computeAccountName":"myadla0831","databaseName":"master","version":"e8bca908-cc73-41a3-9564-e9bcfaa21f4e"
        }
    ]
    }

## <a name="see-also"></a>Zie ook

- Een complexe query's, Zie [Logboeken van Website analyseren met behulp van Azure gegevens Lake Analytics](data-lake-analytics-analyze-weblogs.md).
- Zie [ontwikkelt U SQL - scripts met behulp van hulpmiddelen voor gegevens Lake voor Visual Studio](data-lake-analytics-data-lake-tools-get-started.md)om te beginnen U SQL-toepassingen ontwikkelen.
- Zie informatie over U SQL, [aan de slag met Azure gegevens Lake Analytics U SQL - taal](data-lake-analytics-u-sql-get-started.md).
- Zie voor beheertaken, [Azure gegevens Lake Analytics beheren met behulp van Azure portal](data-lake-analytics-manage-use-portal.md).
- Zie voor een overzicht van gegevens Lake Analytics, [Azure gegevens Lake Analytics overzicht](data-lake-analytics-overview.md).
- Klik op het tabblad kiezers boven aan de pagina overzicht dezelfde zelfstudie met behulp van andere hulpprogramma's.
- Zie [toegang tot diagnostische logboeken voor Azure gegevens Lake Analytics](data-lake-analytics-diagnostic-logs.md) om diagnostische gegevens vastleggen,
