<properties 
   pageTitle="Aan de slag met Lake gegevensarchief met REST API | Microsoft Azure" 
   description="WebHDFS REST API's gebruiken voor het uitvoeren van bewerkingen op Lake gegevensarchief" 
   services="data-lake-store" 
   documentationCenter="" 
   authors="nitinme" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="09/27/2016"
   ms.author="nitinme"/>

# <a name="get-started-with-azure-data-lake-store-using-rest-apis"></a>Aan de slag met Azure Lake gegevensarchief met REST API 's

> [AZURE.SELECTOR]
- [Portal](data-lake-store-get-started-portal.md)
- [PowerShell](data-lake-store-get-started-powershell.md)
- [.NET SDK](data-lake-store-get-started-net-sdk.md)
- [Java SDK](data-lake-store-get-started-java-sdk.md)
- [REST-API](data-lake-store-get-started-rest-api.md)
- [Azure CLI](data-lake-store-get-started-cli.md)
- [Node.js](data-lake-store-manage-use-nodejs.md)

In dit artikel leert u hoe u WebHDFS REST API's en API's voor gegevens Lake winkel REST accountbeheer als bestandssysteem bewerkingen op Azure Lake gegevensarchief. Azure Lake gegevensarchief stelt zijn eigen REST API's van het account beheer. Echter omdat het gegevensarchief Lake compatibel is met HDFS en Hadoop-ecosysteem, ondersteunt WebHDFS REST API's gebruiken filesystem bewerkingen.

>[AZURE.NOTE] Ondersteuning voor gegevensopslag Lake, [Azure Data Lake winkel REST API Reference](https://msdn.microsoft.com/library/mt693424.aspx)Zie voor gedetailleerde informatie over de REST API.

## <a name="prerequisites"></a>Vereisten

- **Azure een abonnement**. Zie [Azure krijg gratis proefperiode](https://azure.microsoft.com/pricing/free-trial/).

- **Een Azure Active Directory-toepassing maken**. De Azure AD-toepassing kunt u de toepassing van het gegevensarchief Lake met Azure Active Directory te verifiëren. Er zijn verschillende benaderingen voor de verificatie van AD Azure, **eindgebruikers** of **verificatie services zijn**. Zie voor instructies en meer informatie over het verifiëren [verifiëren met Lake gegevensarchief Azure Active Directory](data-lake-store-authenticate-using-active-directory.md).

- [cURL](http://curl.haxx.se/). In dit artikel wordt de krul REST API-aanroepen tegen de account van een gegevensarchief voor meer voorbeelden.

## <a name="how-do-i-authenticate-using-azure-active-directory"></a>Hoe u verifiëren met Azure Active Directory?

U kunt op twee manieren verifiëren met behulp van Azure Active Directory.

### <a name="end-user-authentication-interactive"></a>Eindgebruiker-verificatie (interactief)

In dit scenario wordt de toepassing vraagt de gebruiker aan te melden en alle bewerkingen worden uitgevoerd in de context van de gebruiker. De volgende stappen uit voor interactieve verificatie uitvoeren.

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

In dit scenario wordt het de toepassing biedt een eigen referenties voor het uitvoeren van bewerkingen. Hiervoor moet u een POST-aanvraag, zoals in de onderstaande uitgeven. 

    curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token  \
      -F grant_type=client_credentials \
      -F resource=https://management.core.windows.net/ \
      -F client_id=<CLIENT-ID> \
      -F client_secret=<AUTH-KEY>

De uitvoer van deze aanvraag omvat Autorisatietoken (aangeduid met `access-token` in de uitvoer hieronder) die u vervolgens met de REST API-aanroepen worden doorgegeven. Deze verificatietoken opslaat in een tekstbestand; Dit moet u verderop in dit artikel.

    {"token_type":"Bearer","expires_in":"3599","expires_on":"1458245447","not_before":"1458241547","resource":"https://management.core.windows.net/","access_token":"<REDACTED>"}

In dit artikel wordt de **niet-interactieve** benadering. Zie voor meer informatie over niet-interactieve (service-service-oproepen) [-Service met de referenties van serviceoproepen](https://msdn.microsoft.com/library/azure/dn645543.aspx).

## <a name="create-a-data-lake-store-account"></a>Een gegevensarchief Lake-account maken

Deze bewerking is gebaseerd op de REST API call gedefinieerd [hier](https://msdn.microsoft.com/library/mt694078.aspx).

Gebruik de volgende opdracht in de krul. Vervangen ** \<yourstorename >** met de naam van het gegevensarchief Lake.

    curl -i -X PUT -H "Authorization: Bearer <REDACTED>" -H "Content-Type: application/json" https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.DataLakeStore/accounts/<yourstorename>?api-version=2015-10-01-preview -d@"C:\temp\input.json"

Vervang in de bovenstaande opdracht \< `REDACTED` \> met de Autorisatietoken opgehaald u eerder. De nettolading van de aanvraag voor deze opdracht bevindt zich in het **input.json** -bestand dat is opgegeven voor de `-d` parameter hierboven. De inhoud van het bestand input.json als de volgende:

    {
    "location": "eastus2",
    "tags": {
        "department": "finance"
        },
    "properties": {}
    }   

## <a name="create-folders-in-a-data-lake-store-account"></a>Mappen maken in een gegevensarchief Lake-account

Deze bewerking is gebaseerd op de API WebHDFS REST gedefinieerd oproep [hier](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Make_a_Directory).

Gebruik de volgende opdracht in de krul. Vervangen ** \<yourstorename >** met de naam van het gegevensarchief Lake.

    curl -i -X PUT -H "Authorization: Bearer <REDACTED>" -d "" https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/?op=MKDIRS

Vervang in de bovenstaande opdracht \< `REDACTED` \> met de Autorisatietoken opgehaald u eerder. Deze opdracht maakt een map met de naam **mytempdir** in de hoofdmap van uw account Lake gegevensarchief.

Als de bewerking met succes weergegeven een antwoord als volgt:

    {"boolean":true}

## <a name="list-folders-in-a-data-lake-store-account"></a>Lijst van mappen in een gegevensarchief Lake-account

Deze bewerking is gebaseerd op de API WebHDFS REST gedefinieerd oproep [hier](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#List_a_Directory).

Gebruik de volgende opdracht in de krul. Vervangen ** \<yourstorename >** met de naam van het gegevensarchief Lake.

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/?op=LISTSTATUS

Vervang in de bovenstaande opdracht \< `REDACTED` \> met de Autorisatietoken opgehaald u eerder.

Als de bewerking met succes weergegeven een antwoord als volgt:

    {
    "FileStatuses": {
        "FileStatus": [{
            "length": 0,
            "pathSuffix": "mytempdir",
            "type": "DIRECTORY",
            "blockSize": 268435456,
            "accessTime": 1458324719512,
            "modificationTime": 1458324719512,
            "replication": 0,
            "permission": "777",
            "owner": "NotSupportYet",
            "group": "NotSupportYet"
        }]
    }
    }

## <a name="upload-data-into-a-data-lake-store-account"></a>Gegevens uploaden rekening gegevensarchief Lake

Deze bewerking is gebaseerd op de API WebHDFS REST gedefinieerd oproep [hier](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Create_and_Write_to_a_File).

Uploaden van gegevens met behulp van de API WebHDFS REST is een proces, zoals hieronder uitgelegd.

1. PLAATSEN van HTTP-verzoek verzenden zonder het verzenden van gegevens uit het bestand te uploaden. Vervang in de opdracht ** \<yourstorename >** met de naam van het gegevensarchief Lake.

        curl -i -X PUT -H "Authorization: Bearer <REDACTED>" -d "" https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/?op=CREATE

    De uitvoer van deze opdracht worden bevat een tijdelijke omleiding URL, zoals een hieronder weergegeven.

        HTTP/1.1 100 Continue

        HTTP/1.1 307 Temporary Redirect
        ...
        ...
        Location: https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/somerandomfile.txt?op=CREATE&write=true
        ...
        ...

2. U moet een andere plaatsen van HTTP-verzoek aan de URL die wordt weergegeven voor de eigenschap **Location** in het antwoord nu verzenden. Vervangen ** \<yourstorename >** met de naam van het gegevensarchief Lake.

        curl -i -X PUT -T myinputfile.txt -H "Authorization: Bearer <REDACTED>" https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=CREATE&write=true

    De uitvoer is vergelijkbaar met het volgende:

        HTTP/1.1 100 Continue

        HTTP/1.1 201 Created
        ...
        ...

## <a name="read-data-from-a-data-lake-store-account"></a>Gegevens uit een gegevensarchief Lake account lezen

Deze bewerking is gebaseerd op de API WebHDFS REST gedefinieerd oproep [hier](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Open_and_Read_a_File).

Lezen van gegevens uit een gegevensarchief Lake is account een proces.

* Indienen van een GET-verzoek ten opzichte van het eindpunt `https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=OPEN`. Hiermee herstelt u een locatie voor de volgende GET-aanvraag te verzenden.
* U ten opzichte van het eindpunt van het GET-verzoek indienen `https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=OPEN&read=true`. Hiermee wordt de inhoud van het bestand weergegeven.

Echter, omdat er geen verschil is tussen de eerste en de tweede stap in de invoerparameters, kunt u de `-L` -parameter voor de eerste aanvraag indienen. `-L`optie zal in feite twee aanvragen combineert in één en krul de aanvraag op de nieuwe locatie opnieuw. Ten slotte, de uitvoer van alle oproepen van de aanvraag wordt weergegeven, zoals hieronder wordt weergegeven. Vervangen ** \<yourstorename >** met de naam van het gegevensarchief Lake.

    curl -i -L GET -H "Authorization: Bearer <REDACTED>" https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=OPEN

Een uitvoer van de volgende strekking worden weergegeven:

    HTTP/1.1 307 Temporary Redirect
    ...
    Location: https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/somerandomfile.txt?op=OPEN&read=true
    ...
    
    HTTP/1.1 200 OK
    ...
    
    Hello, Data Lake Store user!

## <a name="rename-a-file-in-a-data-lake-store-account"></a>De naam van een bestand in een gegevensarchief Lake-account

Deze bewerking is gebaseerd op de API WebHDFS REST gedefinieerd oproep [hier](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Rename_a_FileDirectory).

Gebruik de volgende cURL-opdracht om een bestand te wijzigen. Vervangen ** \<yourstorename >** met de naam van het gegevensarchief Lake.

    curl -i -X PUT -H "Authorization: Bearer <REDACTED>" -d "" https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=RENAME&destination=/mytempdir/myinputfile1.txt

Een uitvoer van de volgende strekking worden weergegeven:

    HTTP/1.1 200 OK
    ...
    
    {"boolean":true}

## <a name="delete-a-file-from-a-data-lake-store-account"></a>Een bestand uit een gegevensarchief Lake account verwijderen

Deze bewerking is gebaseerd op de API WebHDFS REST gedefinieerd oproep [hier](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Delete_a_FileDirectory).

Gebruik de volgende cURL-opdracht om een bestand te verwijderen. Vervangen ** \<yourstorename >** met de naam van het gegevensarchief Lake.

    curl -i -X DELETE -H "Authorization: Bearer <REDACTED>" https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile1.txt?op=DELETE

Hier ziet u de volgende uitvoer:

    HTTP/1.1 200 OK
    ...
    
    {"boolean":true}

## <a name="delete-a-data-lake-store-account"></a>Een gegevensarchief Lake account verwijderen

Deze bewerking is gebaseerd op de REST API call gedefinieerd [hier](https://msdn.microsoft.com/library/mt694075.aspx).

Gebruik de volgende opdracht in de krul een gegevensarchief Lake account verwijderen. Vervangen ** \<yourstorename >** met de naam van het gegevensarchief Lake.

    curl -i -X DELETE -H "Authorization: Bearer <REDACTED>" https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.DataLakeStore/accounts/<yourstorename>?api-version=2015-10-01-preview

Hier ziet u de volgende uitvoer:

    HTTP/1.1 200 OK
    ...
    ...

## <a name="see-also"></a>Zie ook

- [Brongegevens voor grote toepassingen compatibel zijn met Azure Lake gegevensarchief](data-lake-store-compatible-oss-other-applications.md)
 
