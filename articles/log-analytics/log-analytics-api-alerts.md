<properties
   pageTitle="Logboek Analytics Alert REST API"
   description="Logboek Analytics Alert REST API kunt u waarschuwingen in Operations Management Suite (OMS) maken en beheren.  Dit artikel bevat informatie over de API en verschillende voorbeelden voor het uitvoeren van verschillende bewerkingen."
   services="log-analytics"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags
   ms.service="log-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/18/2016"
   ms.author="bwren" />

# <a name="log-analytics-alert-rest-api"></a>Logboek Analytics alert REST API

Logboek Analytics Alert REST API kunt u waarschuwingen in Operations Management Suite (OMS) maken en beheren.  Dit artikel bevat informatie over de API en verschillende voorbeelden voor het uitvoeren van verschillende bewerkingen.

De REST logboek Analytics Zoek-API RESTful is en toegankelijk is via de Azure Resource Manager REST API. In dit document vindt u voorbeelden waarin de API is toegankelijk via een PowerShell-opdrachtregel met behulp van [ARMClient](https://github.com/projectkudu/ARMClient), een open-source opdrachtregelprogramma dat vereenvoudigt de bronnenbeheerder Azure API aanroepen. Het gebruik van ARMClient en PowerShell is een van de vele opties voor toegang tot het logboek Analytics Zoek-API. Met deze hulpmiddelen kunt u de RESTful Azure Resource Manager API-aanroepen van OMS werkruimten en uitvoeren van de opdrachten zoeken binnen deze gebruiken. De API uitvoer zoekresultaten aan u in JSON-indeling, zodat u kunt de zoekresultaten via programmering op veel verschillende manieren gebruiken.

## <a name="prerequisites"></a>Vereisten
Waarschuwingen kunnen momenteel alleen worden gemaakt met een opgeslagen zoekopdracht in logboek Analytics.  U kunt verwijzen naar het [Logboek zoeken REST API](log-analytics-log-search-api.md) voor meer informatie.

## <a name="schedules"></a>Schema 's
Een opgeslagen zoekactie kan een of meer schema's hebben. Het schema bepaalt hoe vaak de zoekopdracht uitvoeren en het tijdsinterval waarop de criteria wordt aangegeven.
Schema's hebben de eigenschappen in de volgende tabel.

| Eigenschap  | Beschrijving |
|:--|:--|
| Interval | Hoe vaak de zoekopdracht wordt uitgevoerd. Gemeten in minuten. |
| QueryTimeSpan | Het tijdsinterval waarop de criteria worden geëvalueerd. Moet gelijk zijn aan of groter is dan het Interval. Gemeten in minuten. |
| Versie | De API-versie wordt gebruikt.  Op dit moment moet dit altijd worden ingesteld op 1. |

Neem bijvoorbeeld een event-query met een Interval van 15 minuten en een interval van 30 minuten. In dit geval zou de query worden uitgevoerd om de 15 minuten en een waarschuwing zou worden gegeven als de criteria blijven omzetten in op true als u een reeks van 30 minuten.

### <a name="retrieving-schedules"></a>Ophalen van schema 's
Gebruik de methode Get alle schema's voor een opgeslagen zoekopdracht ophalen.

    armclient get /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search  ID}/schedules?api-version=2015-03-20

Gebruik de methode Get met een schema-ID op te halen een bepaald schema voor een opgeslagen zoekactie.

    armclient get /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Subscription ID}/schedules/{Schedule ID}?api-version=2015-03-20

Hieronder vindt u een antwoord voorbeeld voor een schema.

    {
        "id": "subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/MyWorkspace/savedSearches/0f0f4853-17f8-4ed1-9a03-8e888b0d16ec/schedules/a17b53ef-bd70-4ca4-9ead-83b00f2024a8",
        "etag": "W/\"datetime'2016-02-25T20%3A54%3A49.8074679Z'\"",
        "properties": {
        "Interval": 15,
        "QueryTimeSpan": 15
    }

### <a name="creating-a-schedule"></a>Een planning maken
Gebruik de methode Put met een unieke planning-ID voor het maken van een nieuwe planning.  Houd er rekening mee dat twee planningen kunnen niet dezelfde ID zelfs als ze gekoppeld aan andere zijn opgeslagen zoekopdrachten.  Als u een planning in de console OMS maakt, wordt een GUID gemaakt voor het schema.

    $scheduleJson = "{'properties': { 'Interval': 15, 'QueryTimeSpan':15, 'Active':'true' }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/mynewschedule?api-version=2015-03-20 $scheduleJson

### <a name="editing-a-schedule"></a>Een planning bewerken
Gebruik de methode Put met een bestaande schema-ID voor de opgeslagen zoekopdracht die hetzelfde schema wijzigen.  Het hoofdgedeelte van de aanvraag moet de etag van de planning opnemen.

    $scheduleJson = "{'etag': 'W/\"datetime'2016-02-25T20%3A54%3A49.8074679Z'\""','properties': { 'Interval': 15, 'QueryTimeSpan':15, 'Active':'true' }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/mynewschedule?api-version=2015-03-20 $scheduleJson


### <a name="deleting-schedules"></a>Schema's verwijderen
Gebruik de methode Delete met een schema-ID te verwijderen van een schema.

    armclient delete /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Subscription ID}/schedules/{Schedule ID}?api-version=2015-03-20


## <a name="actions"></a>Acties
Een schema kan meerdere acties hebben. Een actie kan een of meer processen uitvoeren zoals het verzenden van een e-mailbericht of vanaf een runbook definiëren, of het kan een drempel die bepaalt wanneer de resultaten van een zoekopdracht overeenkomen met bepaalde criteria definiëren.  Sommige acties definiëren beide zodat de processen worden uitgevoerd wanneer de drempel is bereikt.

Alle acties hebben de eigenschappen in de volgende tabel.  Verschillende soorten meldingen hebben verschillende extra eigenschappen die hieronder worden beschreven.

| Eigenschap | Beschrijving |
|:--|:--|
| Type | Type actie.  De mogelijke waarden zijn momenteel waarschuwing en Webhook. |
| Naam | De weergegeven naam voor de waarschuwing. |
| Versie | De API-versie wordt gebruikt.  Op dit moment moet dit altijd worden ingesteld op 1. |

### <a name="retrieving-actions"></a>Ophalen van acties
Gebruik de Get-methode voor het ophalen van alle acties voor een schema.

    armclient get /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search  ID}/schedules/{Schedule ID}/actions?api-version=2015-03-20

Gebruik de methode Get met actie-ID op te halen een bepaalde actie voor een schema.

    armclient get /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Subscription ID}/schedules/{Schedule ID}/actions/{Action ID}?api-version=2015-03-20

### <a name="creating-or-editing-actions"></a>Maken of bewerken van acties
Gebruik de methode Put met een actie-ID die uniek is voor de planning voor het maken van een nieuwe actie.  Wanneer u een actie in de console OMS maakt, wordt een GUID is voor de actie-ID.

Gebruik de methode Put met een bestaand actie-ID voor de opgeslagen zoekopdracht die hetzelfde schema wijzigen.  Het hoofdgedeelte van de aanvraag moet de etag van de planning opnemen.

De indeling voor het maken van een nieuwe actie verschilt per actietype, zodat deze voorbeelden zijn beschikbaar in de volgende secties.

### <a name="deleting-actions"></a>Acties verwijderen
Gebruik de methode Delete met de actie-ID te verwijderen van een actie.

    armclient delete /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Subscription ID}/schedules/{Schedule ID}/Actions/{Action ID}?api-version=2015-03-20

### <a name="alert-actions"></a>Acties bij waarschuwingen
Een schema kan slechts één waarschuwing actie moet hebben.  Acties bij waarschuwingen zijn een of meer van de secties in de volgende tabel.  Verder hieronder uitvoerig worden beschreven.

| Sectie | Beschrijving |
|:--|:--|
| Drempel | Criteria voor wanneer de actie wordt uitgevoerd. |  
| EmailNotification | E-mailberichten naar meerdere geadresseerden verzenden. |
| Herstel | Start een runbook in Azure automatisering te proberen op te lossen geconstateerde probleem. |

#### <a name="thresholds"></a>Drempels
Een Alert actie mag slechts één drempel hebben.  Wanneer de resultaten van een opgeslagen zoekopdracht overeenkomen met de drempel in een actie die is gekoppeld aan dat met de zoekactie, worden de andere processen in die actie uitvoeren.  Een actie kan ook alleen een drempelwaarde bevatten, zodat deze kan worden gebruikt met de acties van andere soorten die drempels niet bevatten.

Drempels hebben de eigenschappen in de volgende tabel.

| Eigenschap | Beschrijving |
|:--|:--|
| Operator | Operator voor de vergelijking van de drempel. <br> gt = groter dan <br> lt = minder dan |
| Waarde | De waarde voor de drempelwaarde. |

Neem bijvoorbeeld een event-query met een Interval van 15 minuten, een interval van 30 minuten en een drempel van meer dan 10. In dit geval zou de query worden uitgevoerd om de 15 minuten en een waarschuwing als het geretourneerde 10 gebeurtenissen die zijn gemaakt via een reeks van 30 minuten zou worden gegeven.

Hieronder vindt u een antwoord van de steekproef voor een actie met alleen een drempel.  

    "etag": "W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"",
    "properties": {
        "Type": "Alert",
        "Name": "My threshold action",
        "Threshold": {
            "Operator": "gt",
            "Value": 10
        },
        "Version": 1
    }

Gebruik de methode Put met een unieke actie-ID voor het maken van een nieuwe actie van de drempel voor een schema.  

    $thresholdJson = "{'properties': { 'Name': 'My Threshold', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 } }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mythreshold?api-version=2015-03-20 $thresholdJson

Gebruik de methode Put met een bestaand actie-ID voor het wijzigen van een actie van de drempel voor een schema.  Het hoofdgedeelte van de aanvraag moet bevatten de etag van de actie.

    $thresholdJson = "{'etag': 'W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"','properties': { 'Name': 'My Threshold', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 } }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mythreshold?api-version=2015-03-20 $thresholdJson

#### <a name="email-notification"></a>E-mailmeldingen
E-mail verzenden e-mailberichten naar een of meer geadresseerden.  Ze bevatten de eigenschappen in de volgende tabel.

| Eigenschap | Beschrijving |
|:--|:--|
| Geadresseerden | Lijst met e-mailadressen. |
| Onderwerp | Het onderwerp van het e-mailbericht. |
| Bijlage | Bijlagen worden momenteel niet ondersteund, dus dit altijd de waarde 'Geen heeft'. |

Hieronder vindt u een monster antwoord voor een actie e-mail bericht met een drempel.  

    "etag": "W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"",
    "properties": {
        "Type": "Alert",
        "Name": "My email action",
        "Threshold": {
            "Operator": "gt",
            "Value": 10
        },
        "EmailNotification": {
            "Recipients": [
                "recipient1@contoso.com",
                "recipient2@contoso.com"
            ],
            "Subject": "This is the subject",
            "Attachment": "None"
        },
        "Version": 1
    }

Gebruik de methode Put met een unieke actie-ID voor het maken van een nieuwe e-actie voor een schema.  In het volgende voorbeeld wordt een e-mailbericht gemaakt met een drempel, zodat het e-mailbericht wordt verzonden wanneer de resultaten van het opgeslagen zoekpad dat groter is dan de drempel.

    $emailJson = "{'properties': { 'Name': 'MyEmailAction', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 }, 'EmailNotification': {'Recipients': ['recipient1@contoso.com', 'recipient2@contoso.com'], 'Subject':'This is the subject', 'Attachment':'None'} }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myemailaction?api-version=2015-03-20 $ emailJson

Gebruik de methode Put met een bestaand actie-ID voor het wijzigen van een e-actie voor een schema.  Het hoofdgedeelte van de aanvraag moet bevatten de etag van de actie.

    $emailJson = "{'etag': 'W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"','properties': { 'Name': 'MyEmailAction', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 }, 'EmailNotification': {'Recipients': ['recipient1@contoso.com', 'recipient2@contoso.com'], 'Subject':'This is the subject', 'Attachment':'None'} }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myemailaction?api-version=2015-03-20 $ emailJson

#### <a name="remediation-actions"></a>Acties voor herstel
Een runbook start remediations in Azure automatisering op het probleem dat wordt aangeduid met de waarschuwing.  U moet een webhook voor de runbook die wordt gebruikt in een actie doorvoeren maken en geef vervolgens de URI in de eigenschap WebhookUri.  Als u deze actie met de OMS-console maakt, wordt automatisch een nieuwe webhook voor de runbook gemaakt.

Remediations bevatten de eigenschappen in de volgende tabel.

| Eigenschap | Beschrijving |
|:--|:--|
| RunbookName | De naam van de runbook. Deze moet overeenkomen met een gepubliceerde runbook in de automatisering-account geconfigureerd in de oplossing voor automatisering in uw werkruimte OMS. |
| WebhookUri | De URI van de webhook.
| Vervaldatum | De datum en tijd van de webhook.  Als de webhook niet een vervaldatum, kan dit een geldige datum in de toekomst zijn. |

Hieronder vindt u een monster antwoord voor de actie doorvoeren met een drempel.

    "etag": "W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"",
    "properties": {
        "Type": "Alert",
        "Name": "My remediation action",
        "Threshold": {
            "Operator": "gt",
            "Value": 10
        },
        "Remediation": {
            "RunbookName": "My-Runbook",
            "WebhookUri": "https://s1events.azure-automation.net/webhooks?token=4jCibOjO3w4W2Cfg%2b2NkjLYdafnusaG6i8tnP8h%2fNNg%3d",
            "Expiry": "2018-02-25T18:27:20"
            },
        "Version": 1
    }

Gebruik de methode Put met een unieke actie-ID voor het maken van een nieuwe actie doorvoeren voor een schema.  Het volgende voorbeeld wordt een oplossing voor problemen met een drempelwaarde zodat het runbook wordt gestart wanneer de resultaten van de zoekactie de drempel overschrijden.

    $remediateJson = "{'properties': { 'Type':'Alert', 'Name': 'My Remediation Action', 'Version':'1', 'Threshold': { 'Operator': 'gt', 'Value': 10 }, 'Remediation': {'RunbookName': 'My-Runbook', 'WebhookUri':'https://s1events.azure-automation.net/webhooks?token=4jCibOjO3w4W2Cfg%2b2NkjLYdafnusaG6i8tnP8h%2fNNg%3d', 'Expiry':'2018-02-25T18:27:20Z'} }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myremediationaction?api-version=2015-03-20 $remediateJson

Gebruik de methode Put met een bestaand actie-ID voor het wijzigen van een herstel-actie voor een schema.  Het hoofdgedeelte van de aanvraag moet bevatten de etag van de actie.

    $remediateJson = "{'etag': 'W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"','properties': { 'Type':'Alert', 'Name': 'My Remediation Action', 'Version':'1', 'Threshold': { 'Operator': 'gt', 'Value': 10 }, 'Remediation': {'RunbookName': 'My-Runbook', 'WebhookUri':'https://s1events.azure-automation.net/webhooks?token=4jCibOjO3w4W2Cfg%2b2NkjLYdafnusaG6i8tnP8h%2fNNg%3d', 'Expiry':'2018-02-25T18:27:20Z'} }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myremediationaction?api-version=2015-03-20 $remediateJson

#### <a name="example"></a>Voorbeeld
Hieronder ziet u een volledig voorbeeld voor het maken van een nieuwe e-mailwaarschuwing.  Hiermee maakt u een nieuwe planning met een actie met een drempel en e-mailadres.

    $subscriptionId = "3d56705e-5b26-5bcc-9368-dbc8d2fafbfc"
    $workspaceId    = "MyWorkspace"
    $searchId       = "51cf0bd9-5c74-6bcb-927e-d1e9080b934e"

    $scheduleJson = "{'properties': { 'Interval': 15, 'QueryTimeSpan':15, 'Active':'true' }"
    armclient put /subscriptions/$subscriptionId/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/$workspaceId/savedSearches/$searchId/schedules/myschedule?api-version=2015-03-20 $scheduleJson

    $thresholdJson = "{'properties': { 'Name': 'My Threshold', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 } }"
    armclient put /subscriptions/$subscriptionId/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/$workspaceId/savedSearches/$searchId/schedules/myschedule/actions/mythreshold?api-version=2015-03-20 $thresholdJson

    $emailJson = "{'properties': { 'Name': 'MyEmailAction', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 }, 'EmailNotification': {'Recipients': ['recipient1@contoso.com', 'recipient2@contoso.com'], 'Subject':'This is the subject', 'Attachment':'None'} }"
    armclient put /subscriptions/$subscriptionId/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/$workspaceId/savedSearches/$searchId/schedules/myschedule/actions/myemailaction?api-version=2015-03-20 $emailJson

### <a name="webhook-actions"></a>Webhook acties
Een proces starten Webhook acties door een URL aanroepen en optioneel bieden een nettolading moet worden verzonden.  Ze lijken op acties voor herstel maar ze zijn bedoeld voor webhooks die processen van Azure automatisering runbooks beroepen zich.  Ze bieden ook de extra mogelijkheid bieden een nettolading aan extern proces worden geleverd.

Webhook acties hoeft niet een drempel, maar in plaats daarvan moeten worden toegevoegd aan een schema met een waarschuwing actie met een drempel.  Meerdere Webhook acties die al worden uitgevoerd wanneer de drempel is bereikt, kunt u toevoegen.

Webhook-acties bevatten de eigenschappen in de volgende tabel.

| Eigenschap | Beschrijving |
|:--|:--|
| WebhookUri | Het onderwerp van het e-mailbericht. |
| CustomPayload | Aangepaste payload worden verzonden naar de webhook.  De indeling hangt af wat de webhook verwacht. |

Hieronder vindt u een monster antwoord voor de actie webhook en een bijbehorende waarschuwing aan een drempelwaarde.

    {
        "__metadata": {},
        "value": [
            {
                "id": "subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/bwren/savedSearches/2d1b30fb-7f48-4de5-9614-79ee244b52de/schedules/b80f5621-7217-4007-b32d-165d14377093/Actions/72884702-acf9-4653-bb67-f42436b342b4",
                "etag": "W/\"datetime'2016-02-26T20%3A25%3A00.6862124Z'\"",
                "properties": {
                    "Type": "Webhook",
                    "Name": "My Webhook Action",
                    "WebhookUri": "https://oaaswebhookdf.cloudapp.net/webhooks?token=VfkYTIlpk%2fc%2bJBP",
                    "CustomPayload": "{\"fielld1\":\"value1\",\"field2\":\"value2\"}",
                    "Version": 1
                }
            },
            {
                "id": "subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/bwren/savedSearches/2d1b30fb-7f48-4de5-9614-79ee244b52de/schedules/b80f5621-7217-4007-b32d-165d14377093/Actions/90a27cf8-71b7-4df2-b04f-54ed01f1e4b6",
                "etag": "W/\"datetime'2016-02-26T20%3A25%3A00.565204Z'\"",
                "properties": {
                    "Type": "Alert",
                    "Name": "Threshold for my webhook action",
                    "Threshold": {
                        "Operator": "gt",
                        "Value": 10
                    },
                    "Version": 1
                }
            }
        ]
    }

#### <a name="create-or-edit-a-webhook-action"></a>Maken of bewerken van een actie webhook
Gebruik de methode Put met een unieke actie-ID voor het maken van een nieuwe webhook-actie voor een schema.  Het volgende voorbeeld wordt de actie van een Webhook en een waarschuwing aan een drempelwaarde zodat het webhook wordt geactiveerd wanneer de resultaten van het opgeslagen zoekpad dat groter is dan de drempelwaarde.

    $thresholdAction = "{'properties': { 'Name': 'My Threshold', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 } }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mythreshold?api-version=2015-03-20 $thresholdAction

    $webhookAction = "{'properties': {'Type': 'Webhook', 'Name': 'My Webhook", 'WebhookUri': 'https://oaaswebhookdf.cloudapp.net/webhooks?token=VrkYTKlhk%2fc%2bKBP', 'CustomPayload': '{\"field1\":\"value1\",\"field2\":\"value2\"}', 'Version': 1 }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mywebhookaction?api-version=2015-03-20 $webhookAction

Gebruik de methode Put met een bestaand actie-ID voor het wijzigen van een webhook-actie voor een schema.  Het hoofdgedeelte van de aanvraag moet bevatten de etag van de actie.

    $webhookAction = "{'etag': 'W/\"datetime'2016-02-26T20%3A25%3A00.6862124Z'\"','properties': {'Type': 'Webhook', 'Name': 'My Webhook", 'WebhookUri': 'https://oaaswebhookdf.cloudapp.net/webhooks?token=VrkYTKlhk%2fc%2bKBP', 'CustomPayload': '{\"field1\":\"value1\",\"field2\":\"value2\"}', 'Version': 1 }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mywebhookaction?api-version=2015-03-20 $webhookAction

## <a name="next-steps"></a>Volgende stappen

- De [REST API om zoekopdrachten te log](log-analytics-log-search-api.md) in Log Analytics gebruiken.
