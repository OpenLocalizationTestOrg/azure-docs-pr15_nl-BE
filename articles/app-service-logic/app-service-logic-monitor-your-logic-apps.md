<properties 
    pageTitle="Uw apps logica in Azure App-Service controleren | Microsoft Azure" 
    description="Hoe kunt u zien wat uw logica apps hebt gedaan" 
    authors="jeffhollan" 
    manager="erikre" 
    editor="" 
    services="logic-apps" 
    documentationCenter=""/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="jehollan"/>

# <a name="monitor-your-logic-apps"></a>De logica apps controleren

Nadat u [een logica app maken](app-service-logic-create-a-logic-app.md)ziet u de volledige geschiedenis van de uitvoering ervan in de portal Azure.  U kunt services zoals Azure diagnostiek en Azure waarschuwingen instellen om realtime gebeurtenissen te controleren en alert voor gebeurtenissen zoals 'wanneer u meer dan 5 wordt uitgevoerd niet binnen een uur."

## <a name="monitor-in-the-azure-portal"></a>Monitor in Azure Portal

De geschiedenis weergeven, selecteer **Bladeren**en **Logica Apps**selecteren. Een lijst van alle apps in de logica in uw abonnement wordt weergegeven.  Selecteer de logica app die u wilt controleren.  U ziet een lijst van alle acties en triggers die voor deze toepassing logica hebben plaatsgevonden.

![Overzicht](./media/app-service-logic-monitor-your-logic-apps/overview.png)

Er zijn een paar secties op deze blade die handig zijn:

- **Samenvatting** geeft een overzicht van **alle wordt uitgevoerd** en de **Geschiedenis van de Trigger**
    - Lijst met **alle wordt uitgevoerd** de nieuwste logica app wordt uitgevoerd.  U kunt op elke rij voor meer informatie over het uitvoeren of klikt u op de tegel om meer.
    - **Geschiedenis van de trigger** worden alle activiteiten van de trigger voor de toepassing van deze logica.  Activiteit van de trigger kan zijn van een 'Overgeslagen' controleren op nieuwe gegevens (bv. op zoek om te zien als een nieuw bestand is toegevoegd aan de FTP), "Geslaagd" wat betekent dat gegevens geretourneerd bij brand een app logica of 'Mislukt' is een fout in de configuratie overeenkomt.
- **Diagnostische gegevens** kunt u runtime-gegevens en gebeurtenissen bekijken en zich abonneren op [Waarschuwingen Azure](#adding-azure-alerts)

>[AZURE.NOTE] Alle runtime-gegevens en gebeurtenissen worden gecodeerd in rust binnen de logica App-service. Deze worden alleen ontsleuteld op verzoek van een gebruiker weergeven. Toegang tot deze gebeurtenissen kan ook worden beheerd door Azure Role-Based Access Control RBAC ().

### <a name="view-the-run-details"></a>De uitvoering details weergeven

Deze lijst van reeksen toont de **Status**, de **Begintijd**en de **duur** van de betreffende uitvoeren. Selecteer een rij voor details op die worden uitgevoerd.

De monitoring weergave ziet u elke stap van de uitvoering, de ingangen en uitgangen en eventuele foutberichten die occurre kunnen hebben.

![En acties uitvoeren](./media/app-service-logic-monitor-your-logic-apps/monitor-view.png)

Als u aanvullende details zoals het uitvoeren van de **Correlatie-ID** (die kan worden gebruikt voor de REST API), kunt u de knop **Details worden uitgevoerd** .  Dit omvat alle stappen, status en inputs/outputs voor het uitvoeren.

## <a name="azure-diagnostics-and-alerts"></a>Azure diagnostische gegevens en signalen

Naast de informatie van de Portal Azure en REST API hierboven, kunt u uw app logica voor het gebruik van Azure diagnostische gegevens voor meer uitgebreide informatie en foutopsporing.

1. Klik op de sectie **Diagnostische gegevens** van de logica app blade
1. Klik op **Diagnostische instellingen** configureren
1. Configureer een gebeurtenis Hub of opslag Account om gegevens te verzenden

    ![Azure diagnostische instellingen](./media/app-service-logic-monitor-your-logic-apps/diagnostics.png)

### <a name="adding-azure-alerts"></a>Azure waarschuwingen toevoegen

Nadat Diagnostische gegevens zijn geconfigureerd, kunt u toevoegen Azure-waarschuwingen moeten worden gestart wanneer bepaalde drempels worden overschreden.  Selecteer de tegel **waarschuwingen** en **melding toevoegen**in de blade **Diagnostische gegevens** .  Dit helpt u bij het configureren van een waarschuwing op basis van een aantal drempels en statistieken.

![Azure Alert statistieken](./media/app-service-logic-monitor-your-logic-apps/alerts.png)

U kunt de **voorwaarde**, **drempelwaarde**en **periode** naar wens configureren.  Ten slotte kunt u een e-mailadres voor het verzenden van een bericht dat u wilt configureren of een webhook configureren.  Kunt u de [aanvraag trigger](../connectors/connectors-native-reqres.md) in een app logica voor het uitvoeren van een waarschuwing ook (om dingen te doen zoals [vertraging boeken](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-slack-with-logic-app), [een tekst te verzenden](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-text-message-with-logic-app)of [een bericht naar een wachtrij toevoegen](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-queue-with-logic-app)).

### <a name="azure-diagnostics-settings"></a>Azure diagnostische instellingen

Elk van deze gebeurtenissen bevat details over de logica app en gebeurtenis als status.  Hier volgt een voorbeeld van een gebeurtenis *ActionCompleted* :

```javascript
{
            "time": "2016-07-09T17:09:54.4773148Z",
            "workflowId": "/SUBSCRIPTIONS/80D4FE69-ABCD-EFGH-A938-9250F1C8AB03/RESOURCEGROUPS/MYRESOURCEGROUP/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/MYLOGICAPP",
            "resourceId": "/SUBSCRIPTIONS/80D4FE69-ABCD-EFGH-A938-9250F1C8AB03/RESOURCEGROUPS/MYRESOURCEGROUP/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/MYLOGICAPP/RUNS/08587361146922712057/ACTIONS/HTTP",
            "category": "WorkflowRuntime",
            "level": "Information",
            "operationName": "Microsoft.Logic/workflows/workflowActionCompleted",
            "properties": {
                "$schema": "2016-06-01",
                "startTime": "2016-07-09T17:09:53.4336305Z",
                "endTime": "2016-07-09T17:09:53.5430281Z",
                "status": "Succeeded",
                "code": "OK",
                "resource": {
                    "subscriptionId": "80d4fe69-ABCD-EFGH-a938-9250f1c8ab03",
                    "resourceGroupName": "MyResourceGroup",
                    "workflowId": "cff00d5458f944d5a766f2f9ad142553",
                    "workflowName": "MyLogicApp",
                    "runId": "08587361146922712057",
                    "location": "eastus",
                    "actionName": "Http"
                },
                "correlation": {
                    "actionTrackingId": "e1931543-906d-4d1d-baed-dee72ddf1047",
                    "clientTrackingId": "my-custom-tracking-id"
                },
                "trackedProperties": {
                    "myProperty": "<value>"
                }
            }
        }
```

De twee eigenschappen vooral handig zijn voor het bijhouden en controleren van zijn *clientTrackingId* en *trackedProperties*.  

#### <a name="client-tracking-id"></a>Tracerings-ID van client

De client het volgnummer is een waarde die wordt correleren van gebeurtenissen in een logica app uitvoeren, met inbegrip van eventuele geneste werkstromen genoemd als onderdeel van een app logica.  Deze ID wordt automatisch gegenereerd als het niet beschikbaar, maar u kunt handmatig opgeven met de client die het volgnummer van een trigger door een `x-ms-client-tracking-id` kop met de id-waarde in de trigger-aanvraag (aanvraag trigger, HTTP-trigger of webhook trigger).

#### <a name="tracked-properties"></a>Bijgehouden eigenschappen

Naar acties in de workflowdefinitie voor het bijhouden van invoer of uitvoer in diagnostische gegevens worden bijgehouden eigenschappen toegevoegd.  Dit is handig als u wilt bijhouden van gegevens, net als een 'order-ID' in uw telemetrie.  Als een eigenschap bijgehouden toevoegt, moet u de `trackedProperties` -eigenschap op een actie.  Bijgehouden eigenschappen kan alleen bijhouden een afzonderlijke acties ingangen en uitgangen, maar u kunt de `correlation` eigenschappen van de gebeurtenissen in het correleren van verschillende acties in een run.

```javascript
{
    "myAction": {
        "type": "http",
        "inputs": {
            "uri": "http://uri",
            "headers": {
                "Content-Type": "application/json"
            },
            "body": "@triggerBody()"
        },
        "trackedProperties":{
            "myActionHTTPStatusCode": "@action()['outputs']['statusCode']",
            "myActionHTTPValue": "@action()['outputs']['body']['foo']",
            "transactionId": "@action()['inputs']['body']['bar']"
        }
    }
}
```

### <a name="extending-your-solutions"></a>Uitbreiden van uw oplossingen

U kunt gebruikmaken van deze telemetrie van de gebeurtenis Hub of opslag in andere services, zoals [Operations Management Suite](https://www.microsoft.com/cloud-platform/operations-management-suite) [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/)en [Power BI](https://powerbi.com) real-time bewaking van uw workflows integratie hebben.

## <a name="next-steps"></a>Volgende stappen
- [Algemene voorbeelden en scenario's voor apps logica](app-service-logic-examples-and-scenarios.md)
- [Een sjabloon voor de implementatie logica App maken](app-service-logic-create-deploy-template.md)
- [Enterprise integration-functies](app-service-logic-enterprise-integration-overview.md)
