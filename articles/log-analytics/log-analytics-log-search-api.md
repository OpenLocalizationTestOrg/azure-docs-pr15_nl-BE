<properties
    pageTitle="Analytics logboek logboek zoeken REST API | Microsoft Azure"
    description="Deze handleiding biedt een eenvoudige zelfstudie met een beschrijving van hoe u kunt de zoekopdracht logboek Analytics REST API Operations Management Suite (OMS) en biedt voorbeelden die tonen u hoe u de opdrachten gebruikt."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="banders"/>


# <a name="log-analytics-log-search-rest-api"></a>Logboek Analytics zoekfunctie REST API

Deze handleiding biedt een eenvoudige zelfstudie met een beschrijving van hoe u kunt de REST logboek Analytics Zoek-API Operations Management Suite (OMS) en biedt voorbeelden die tonen u hoe u de opdrachten gebruikt. Sommige van de voorbeelden in dit artikel verwijst naar operationele inzichten, die de naam van de vorige versie van het logboek Analytics is.

## <a name="overview-of-the-log-search-rest-api"></a>Overzicht van het logboek zoeken REST API

De REST logboek Analytics Zoek-API RESTful is en toegankelijk is via de API Azure Resource Manager. In dit document vindt u voorbeelden waar de API is toegankelijk via de [ARMClient](https://github.com/projectkudu/ARMClient), een open-source opdrachtregelprogramma dat vereenvoudigt de bronnenbeheerder Azure API aanroepen. Het gebruik van ARMClient en PowerShell is een van de vele opties voor toegang tot het logboek Analytics Zoek-API. Een andere mogelijkheid is de module Azure PowerShell voor OperationalInsights waaronder de cmdlets voor toegang tot zoeken gebruiken. Met deze hulpmiddelen kunt u de RESTful Azure Resource Manager API-aanroepen van OMS werkruimten en uitvoeren van de opdrachten zoeken binnen deze gebruiken. De API uitvoer zoekresultaten aan u in JSON-indeling, zodat u kunt de zoekresultaten via programmering op veel verschillende manieren gebruiken.

De bronnenbeheerder Azure kan worden gebruikt via een [bibliotheek voor .NET](https://msdn.microsoft.com/library/azure/dn910477.aspx) zoals een [REST API](https://msdn.microsoft.com/library/azure/mt163658.aspx). Bekijk de gekoppelde webpagina's voor meer informatie.

## <a name="basic-log-analytics-search-rest-api-tutorial"></a>Basic logboek Analytics Search REST API-zelfstudie

### <a name="to-use-the-arm-client"></a>De ARM-Client gebruiken

1. Installeer [Chocolatey](https://chocolatey.org/), dat is een Open Source Package Manager voor Windows. Open een opdrachtprompt als administrator en voert u de volgende opdracht:

    ```
    @powershell -NoProfile -ExecutionPolicy unrestricted -Command "iex ((new-object net.webclient).DownloadString('https://chocolatey.org/install.ps1'))" && SET PATH=%PATH%;%ALLUSERSPROFILE%\chocolatey\bin
    ```

2. De ARMClient installeren door de volgende opdracht uit te voeren:

    ```
    choco install armclient
    ```

### <a name="to-perform-a-simple-search-using-the-armclient"></a>Voor het uitvoeren van een eenvoudige zoekactie met behulp van de ARMClient

1. Aanmelden bij uw account van Microsoft of OrgID:

    ```
    armclient login
    ```

    Een geslaagde aanmelding geeft een overzicht van alle abonnementen die zijn gekoppeld aan de opgegeven account:

    ```
    PS C:\Users\SampleUserName> armclient login
    Welcome YourEmail@ORG.com (Tenant: zzzzzzzz-zzzz-zzzz-zzzz-zzzzzzzzzzzz)
    User: YourEmail@ORG.com, Tenant: zzzzzzzz-zzzz-zzzz-zzzz-zzzzzzzzzzzz (Example org)
    There are 3 subscriptions
    Subscription xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx (Example Name 1)
    Subscription xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx (Example Name 2)
    Subscription xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx (Example Name 3)
    ```

2. Haal de werkruimten Operations Management Suite:

    ```
    armclient get /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces?api-version=2015-03-20
    ```

    Een geslaagde aanroep van Get zou alle werkruimten die zijn gekoppeld aan het abonnement uitvoer:

    ```
    {
    "value": [
    {
      "properties": {
    "source": "External",
    "customerId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "portalUrl": "https://eus.login.mms.microsoft.com/SignIn.aspx?returnUrl=https%3a%2f%2feus.mms.microsoft.com%2fMain.aspx%3fcid%xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
      },
      "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcegroups/oi-default-east-us/providers/microsoft.operationalinsights/workspaces/{WORKSPACE NAME}",
      "name": "{WORKSPACE NAME}",
      "type": "Microsoft.OperationalInsights/workspaces",
      "location": "East US"
       ]
    }
    ```
3. Maak uw variabele zoeken:

    ```
    $mySearch = "{ 'top':150, 'query':'Error'}";
    ```
4. Zoeken met behulp van uw nieuwe zoeken variabele:

    ```
    armclient post /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{WORKSPACE NAME}/search?api-version=2015-03-20 $mySearch
    ```

## <a name="log-analytics-search-rest-api-reference-examples"></a>Meld u Analytics Search REST API reference-voorbeelden
De volgende voorbeelden geven aan hoe u de zoek-API kunt gebruiken.

### <a name="search---actionread"></a>Zoeken - actie/lezen

**Voorbeeld-Url:**

```
    /subscriptions/{SubscriptionId}/resourceGroups/{ResourceGroupId}/providers/Microsoft.OperationalInsights/workspaces/{WorkspaceName}/search?api-version=2015-03-20
```

**Aanvraag:**

```
    $savedSearchParametersJson =
    {
      "top":150,
      "highlight":{
        "pre":"{[hl]}",
        "post":"{[/hl]}"
      },
      "query":"*",
      "start":"2015-02-04T21:03:29.231Z",
      "end":"2015-02-11T21:03:29.231Z"
    }
    armclient post /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace ID}/search?api-version=2015-03-20 $searchParametersJson
```
De volgende tabel beschrijft de eigenschappen die beschikbaar zijn.

|**Eigenschap**|**Beschrijving**|
|---|---|
|Boven|Het maximum aantal resultaten te retourneren.|
|markeren|Pre en post parameters, meestal gebruikt voor het markeren van de overeenkomende velden bevat|
|Pre|De opgegeven tekenreeks naar de overeenkomende velden als voorvoegsel toegevoegd.|
|Verzenden|Voegt de opgegeven tekenreeks naar de overeenkomende velden.|
|query|De query gebruikt om resultaten te verkrijgen.|
|Start|Het begin van de gewenste resultaten worden gevonden van venster.|
|einde|Het einde van de gewenste resultaten worden gevonden van venster.|


**Antwoord:**

```
    {
      "id" : "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
      "__metadata" : {
        "resultType" : "raw",
        "total" : 1455,
        "top" : 150,
        "StartTime" : "2015-02-11T21:09:07.0345815Z",
        "Status" : "Successful",
        "LastUpdated" : "2015-02-11T21:09:07.331463Z",
        "CoreResponses" : [],
        "sort" : [{
          "name" : "TimeGenerated",
          "order" : "desc"
        }],
        "requestTime" : 450
      },
      "value": [{
        "SourceSystem" : "OpsManager",
        "TimeGenerated" : "2015-02-07T14:07:33Z",
        "Source" : "SideBySide",
        "EventLog" : "Application",
        "Computer" : "BAMBAM",
        "EventCategory" : 0,
        "EventLevel" : 1,
        "EventLevelName" : "Error",
        "UserName" : "N/A",
        "EventID" : 78,
        "MG" : "00000000-0000-0000-0000-000000000001",
        "TimeCollected" : "2015-02-07T14:10:04.69Z",
        "ManagementGroupName" : "AOI-5bf9a37f-e841-462b-80d2-1d19cd97dc40",
        "id" : "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
        "Type" : "Event",
        "__metadata" : {
          "Type" : "Event",
          "TimeGenerated" : "2015-02-07T14:07:33Z",
          "highlighting" : {
          "EventLevelName" : ["{[hl]}Error{[/hl]}"]
        }
      }]
    ],
            "start" : "2015-02-04T21:03:29.231Z",
            "end" : "2015-02-11T21:03:29.231Z"
          }
        }
      }]
    }
```

### <a name="searchid---actionread"></a>Zoeken / {ID} - actie/lezen

**De inhoud van een opgeslagen zoekopdracht aanvragen:**

```
    armclient post /subscriptions/{SubId}/resourceGroups/{ResourceGroupId}/providers/Microsoft.OperationalInsights/workspaces/{WorkspaceName}/search/{SearchId}?api-version=2015-03-20
```

>[AZURE.NOTE] Als de zoekactie retourneert met de status 'In behandeling', kan vervolgens de bijgewerkte uitkomsten polling worden gedaan via deze API. Na 6 min, het resultaat van de zoekactie wordt verwijderd uit de cache en HTTP niet voltooid worden geretourneerd. Als de aanvankelijke zoekaanvraag onmiddellijk geretourneerd van de status 'Voltooid', wordt deze niet toegevoegd aan de cache waardoor deze API om terug te keren verdwenen HTTP als opgevraagd. De inhoud van het resultaat van een HTTP-200 worden weergegeven in dezelfde indeling als de aanvankelijke zoekaanvraag net met de bijgewerkte waarden.

### <a name="saved-searches---rest-only"></a>Opgeslagen zoekopdrachten - REST alleen

**Lijst met opgeslagen zoekopdrachten aanvragen:**

```
    armclient post /subscriptions/{SubId}/resourceGroups/{ResourceGroupId}/providers/Microsoft.OperationalInsights/workspaces/{WorkspaceName}/savedSearches?api-version=2015-03-20
```

Ondersteunde methoden: GET plaatsen verwijderen

Ondersteunde methoden: ophalen

De volgende tabel beschrijft de eigenschappen die beschikbaar zijn.

|Eigenschap|Beschrijving|
|---|---|
|ID|De unieke id.|
|ETag|**Vereist voor Patch**. Op elke schrijven door de server bijgewerkt. Waarde moet gelijk zijn aan de huidige opgeslagen waarde of ' *' bij te werken. 409 voor oude/ongeldige waarden geretourneerd.|
|Properties.query|**Vereist**. De zoekopdracht.|
|properties.displayName|**Vereist**. De gebruiker gedefinieerde weergavenaam van de query. Als gemodelleerd als een Azure-bron, zou dit een code zijn.|
|Properties.Category|**Vereist**. De door de gebruiker gedefinieerde categorie van de query. Als gemodelleerd als een bron van Azure dit een code is.|

>[AZURE.NOTE] Het logboek Analytics Zoek-API retourneert momenteel gebruiker gemaakte opgeslagen zoekopdrachten wanneer doorzocht op opgeslagen zoekopdrachten in een werkruimte. De API levert opgeslagen zoekopdrachten oplossingen die op dit moment niet. Deze functionaliteit wordt toegevoegd op een later tijdstip.

### <a name="create-saved-searches"></a>Opgeslagen zoekopdrachten maken

**Aanvraag:**

```
    $savedSearchParametersJson = "{'properties': { 'Category': 'myCategory', 'DisplayName':'myDisplayName', 'Query':'* | measure Count() by Source', 'Version':'1'  }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace ID}/savedSearches/thisIsMyId?api-version=2015-03-20 $savedSearchParametersJson
```

### <a name="delete-saved-searches"></a>Verwijdert u opgeslagen zoekopdrachten

**Aanvraag:**

```
    armclient delete /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace ID}/savedSearches/thisIsMyId?api-version=2015-03-20
```

### <a name="update-saved-searches"></a>Update opgeslagen zoekopdrachten

 **Aanvraag:**

```
    $savedSearchParametersJson = "{'etag': 'W/`"datetime\'2015-04-16T23%3A35%3A35.3182423Z\'`"', 'properties': { 'Category': 'myCategory', 'DisplayName':'myDisplayName', 'Query':'* | measure Count() by Source', 'Version':'1'  }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace ID}/savedSearches/thisIsMyId?api-version=2015-03-20 $savedSearchParametersJson
```

### <a name="metadata---json-only"></a>Metagegevens - alleen JSON

Hier is een manier om de velden voor alle typen van de logboekbestanden voor de verzamelde gegevens in uw werkruimte te bekijken. Bijvoorbeeld, als u dat u weten wilt of het type gebeurtenis een veld met de naam Computer heeft, is dit een manier om te zoeken en te bevestigen.

**Aanvraag voor velden:**

```
    armclient get /subscriptions/{SubId}/resourceGroups/{ResourceGroupId}/providers/Microsoft.OperationalInsights/workspaces/{WorkspaceName}/schema?api-version=2015-03-20
```

**Antwoord:**

```
    {
      "__metadata" : {
        "schema" : {
          "name" : "Example Name",
          "version" : 2
        },
        "resultType" : "schema",
        "requestTime" : 35
      },
      "value" : [{
          "name" : "MG",
          "displayName" : "MG",
          "type" : "Guid",
          "facetable" : true,
          "display" : false,
          "ownerType" : ["PerfHourly", "ProtectionStatus", "Capacity_SMBUtilizationByHost", "Capacity_ArrayUtilization", "Capacity_SMBShareUtilization", "SQLAssessmentRecommendation", "Event", "ConfigurationChange", "ConfigurationAlert", "ADAssessmentRecommendation", "ConfigurationObject", "ConfigurationObjectProperty"]
        }, {
          "name" : "ManagementGroupName",
          "displayName" : "ManagementGroupName",
          "type" : "String",
          "facetable" : true,
          "display" : true,
          "ownerType" : ["PerfHourly", "ProtectionStatus", "Event", "ConfigurationChange", "ConfigurationAlert", "W3CIISLog", "AlertHistory", "Recommendation", "Alert", "SecurityEvent", "UpdateAgent", "RequiredUpdate", "ConfigurationObject", "ConfigurationObjectProperty"]
        }
      ]
    }
```

De volgende tabel beschrijft de eigenschappen die beschikbaar zijn.

|**Eigenschap**|**Beschrijving**|
|---|---|
|naam|De naam van het veld.|
|displayName|De weergegeven naam van het veld.|
|type|Het Type van de waarde van het veld.|
|facetable|De combinatie van 'geïndexeerd' huidige ' opgeslagen ' en 'facet' Eigenschappen.|
|weergeven|Huidige 'weergeven' eigenschap. Deze eigenschap is True als het veld wordt weergegeven in de zoekresultaten.|
|voor ownerType|Beperkt tot alleen de soorten die behoren tot de onboarded IP.|


## <a name="optional-parameters"></a>Optionele parameters
De volgende informatie beschrijft de optionele parameters beschikbaar.

### <a name="highlighting"></a>Markering

De parameter 'Markeren' is een optionele parameter die u mag gebruiken voor het aanvragen van het subsysteem zoeken bevatten een reeks gegevensmarkeringen in een antwoord.

Deze markeringen geven het begin en einde gemarkeerde tekst die overeenkomt met de voorwaarden die in de zoekopdracht.
U kunt de begin- en eindmarkeringen die wordt gebruikt door de zoekfunctie om de gemarkeerde term terugloopt opgeven.

**Voorbeeld van zoekopdracht**

```
    $savedSearchParametersJson =
    {
      "top":150,
      "highlight":{
        "pre":"{[hl]}",
        "post":"{[/hl]}"
      },
      "query":"*",
      "start":"2015-02-04T21:03:29.231Z",
      "end":"2015-02-11T21:03:29.231Z"
    }
    armclient post /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace ID}/search?api-version=2015-03-20 $searchParametersJson
```

**Het resultaat van het voorbeeld:**

```
    {
        "TimeGenerated":
        "2015-05-18T23:55:59Z", "Source":
        "EventLog": "Application",
        "Computer": "smokedturkey.net",
        "EventCategory": 0,
        "EventLevel":1,
        "EventLevelName":
        "Error"
        "Manager ", "__metadata":
        {
            "Type": "Event",
            "TimeGenerated": "2015-05-18T23:55:59Z",
            "highlighting": {
                "EventLevelName":
                ["{[hl]}Error{[/hl]}"]
            }
        }
    }
```

Zoals u ziet het resultaat boven een foutbericht weergegeven dat is voorafgegaan en toegevoegd.

## <a name="computer-groups"></a>Computergroepen

Computergroepen zijn speciale opgeslagen zoekopdrachten die resulteren in een set computers.  Kunt u een computergroep in een andere query's om de resultaten naar de computers in de groep te beperken.  Een groep van de computer is geïmplementeerd als een opgeslagen zoekopdracht met een label van de groep met een waarde van de Computer.

Hieronder vindt u een antwoord van de steekproef voor een computergroep.

    "etag": "W/\"datetime'2016-04-01T13%3A38%3A04.7763203Z'\"",
    "properties": {
        "Category": "My Computer Groups",
        "DisplayName": "My Computer Group",
        "Query": "srv* | Distinct Computer",
        "Tags": [{
            "Name": "Group",
            "Value": "Computer"
        }],
    "Version": 1
    }

### <a name="retrieving-computer-groups"></a>Computergroepen ophalen

Gebruik de methode Get met de groeps-ID op te halen een computergroep.

```
armclient get /subscriptions/{Subscription ID}/resourceGroups/{Resource Group Name}/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Group ID}`?api-version=2015-03-20
```

### <a name="creating-or-updating-a-computer-group"></a>Maken of bijwerken van een computergroep

Gebruik de methode Put met een unieke-ID zoeken opgeslagen voor het maken van een nieuwe computergroep. Als u een bestaande computer-ID, wordt dat een worden gewijzigd. Wanneer u een computergroep in de console OMS maken, wordt de ID van de groep en de naam gemaakt.

De query die wordt gebruikt voor de groepsdefinitie moet een set computers voor de groep goed te retourneren.  Het wordt aanbevolen dat u de query met de *beëindigen | Afzonderlijke Computer* om ervoor te zorgen de juiste gegevens geretourneerd.

De definitie van de opgeslagen zoekopdracht moet een label met de naam groep met een waarde van de Computer voor de zoekactie worden geclassificeerd als een computergroep bevatten.

    $etag=Get-Date -Format yyyy-MM-ddThh:mm:ss.msZ
    $groupName="My Computer Group"
    $groupQuery = "Computer=srv* | Distinct Computer"
    $groupCategory="My Computer Groups"
    $groupID = "My Computer Groups | My Computer Group"

    $groupJson = "{'etag': 'W/`"datetime\'" + $etag + "\'`"', 'properties': { 'Category': '" + $groupCategory + "', 'DisplayName':'"  + $groupName + "', 'Query':'" + $groupQuery + "', 'Tags': [{'Name': 'Group', 'Value': 'Computer'}], 'Version':'1'  }"

    armclient put /subscriptions/{Subscription ID}/resourceGroups/{Resource Group Name}/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/$groupId`?api-version=2015-03-20 $groupJson

### <a name="deleting-computer-groups"></a>Computergroepen verwijderen

Gebruik de methode Delete met de groeps-ID een computergroep verwijderen.

```
armclient delete /subscriptions/{Subscription ID}/resourceGroups/{Resource Group Name}/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/$groupId`?api-version=2015-03-20
```


## <a name="next-steps"></a>Volgende stappen

- Informatie over het [logboek zoeken](log-analytics-log-searches.md) met aangepaste velden voor de criteria voor query's bouwen.
