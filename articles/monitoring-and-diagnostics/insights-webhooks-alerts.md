<properties
    pageTitle="Webhooks op Azure metrische waarschuwingen configureren | Microsoft Azure"
    description="Azure waarschuwingen bij andere systemen niet Azure omleiden."
    authors="kamathashwin"
    manager="carolz"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/15/2016"
    ms.author="ashwink"/>

# <a name="configure-a-webhook-on-an-azure-metric-alert"></a>Een webhook op een metrische Azure waarschuwing configureren

Webhooks kunt u een waarschuwingsbericht Azure doorsturen naar andere systemen voor naverwerkingsprogramma of aangepaste acties. U kunt een webhook van een waarschuwing rondsturen naar services die SMS-bericht verzenden, meld bugs, kennis van een team via chat/messaging services of een willekeurig aantal andere acties uitvoeren. In dit artikel wordt beschreven hoe een webhook instellen voor een metrische Azure waarschuwing en de nettolading van de HTTP POST naar een webhook ziet eruit als. Waarschuwing voor informatie over de installatie en het schema voor een logboek met faxactiviteit Azure (melding van gebeurtenissen) [in plaats daarvan op deze pagina vindt](./insights-auditlog-to-webhook-email.md).

Azure waarschuwingen HTTP POST de inhoud van de waarschuwing in JSON opmaken voor een webhook URI die u opgeeft bij het maken van de waarschuwing hieronder gedefinieerd schema. Deze URI moet een geldige HTTP- of HTTPS-eindpunt. Azure geboekt één post per aanvraag als een waarschuwing wordt geactiveerd.

## <a name="configuring-webhooks-via-the-portal"></a>Webhooks via de portal configureren

U kunt toevoegen of bijwerken van de URI-webhook in het scherm waarschuwingen maken/bijwerken in de [portal](https://portal.azure.com/).

![Een waarschuwingsregel toevoegen](./media/insights-webhooks-alerts/Alertwebhook.png)

U kunt ook een waarschuwing voor het boeken van een webhook URI [Azure PowerShell Cmdlets](./insights-powershell-samples.md#create-alert-rules), [Cross-Platform CLI](./insights-cli-samples.md#work-with-alerts)of [Azure Monitor REST API](https://msdn.microsoft.com/library/azure/dn933805.aspx).

## <a name="authenticating-the-webhook"></a>Verificatie van de webhook

De webhook kan worden geverifieerd met behulp van deze methoden:

1. **Verificatie op basis van het token** - URI van de webhook wordt opgeslagen met een token-ID, bv. `https://mysamplealert/webcallback?tokenid=sometokenid&someparameter=somevalue`
2.  **Basisverificatie** - URI van de webhook wordt opgeslagen met een gebruikersnaam en wachtwoord, bv. `https://userid:password@mysamplealert/webcallback?someparamater=somevalue&foo=bar`

## <a name="payload-schema"></a>Schema Payload

De POST-bewerking bevat de volgende JSON-nettolading en het schema voor alle waarschuwingen op basis van een metric.

```JSON
{
"status": "Activated",
"context": {
            "timestamp": "2015-08-14T22:26:41.9975398Z",
            "id": "/subscriptions/s1/resourceGroups/useast/providers/microsoft.insights/alertrules/ruleName1",
            "name": "ruleName1",
            "description": "some description",
            "conditionType": "Metric",
            "condition": {
                        "metricName": "Requests",
                        "metricUnit": "Count",
                        "metricValue": "10",
                        "threshold": "10",
                        "windowSize": "15",
                        "timeAggregation": "Average",
                        "operator": "GreaterThanOrEqual"
                },
            "subscriptionId": "s1",
            "resourceGroupName": "useast",                                
            "resourceName": "mysite1",
            "resourceType": "microsoft.foo/sites",
            "resourceId": "/subscriptions/s1/resourceGroups/useast/providers/microsoft.foo/sites/mysite1",
            "resourceRegion": "centralus",
            "portalLink": "https://portal.azure.com/#resource/subscriptions/s1/resourceGroups/useast/providers/microsoft.foo/sites/mysite1"
},
"properties": {
              "key1": "value1",
              "key2": "value2"
              }
}
```


| Veld | Verplicht | Vaste Set van waarden | Notities |
| :-------------| :-------------   | :-------------   | :-------------   |
|status|Y|'Geactiveerd', 'Opgelost'|Status van de waarschuwing op basis van de voorwaarden die u hebt ingesteld.|
|context| Y | | De context van de waarschuwing.|
|tijdstempel| Y | | Het tijdstip waarop de waarschuwing is geactiveerd.|
|ID | Y | | Elke waarschuwingsregel heeft een unieke id.|
|naam               |Y                  |                   | De naam van de waarschuwing.|
|Beschrijving        |Y                  |                           |Beschrijving van de waarschuwing.|
|conditionType      |Y                  |"Metrisch", "Event"          |Twee soorten meldingen worden ondersteund. Op basis van een voorwaarde metrische en de andere op basis van een gebeurtenis in het logboek voor faxactiviteit. Gebruik deze waarde om te controleren als de waarschuwing wordt gebaseerd op metrisch of gebeurtenis.|
|voorwaarde          |Y                  |                           | De specifieke velden die u wilt controleren op basis van de conditionType.|
|metricName         |voor metrische waarschuwingen  |                           |De naam van de metric die wordt gedefinieerd wat de regel gecontroleerd.|
|metricUnit         |voor metrische waarschuwingen  |"Bytes", "BytesPerSecond", "Aantal", "CountPerSecond", "Percent", "Seconden"|     De eenheid die is toegestaan in de metric. [Toegestane waarden vindt u hier](https://msdn.microsoft.com/library/microsoft.azure.insights.models.unit.aspx).|
|metricValue        |voor metrische waarschuwingen  |                           |De werkelijke waarde van de metric die de waarschuwing hebben veroorzaakt.|
|drempel          |voor metrische waarschuwingen  |                           |De drempelwaarde waarbij de waarschuwing wordt geactiveerd.|
|WindowSize         |voor metrische waarschuwingen  |                           |De periode die wordt gebruikt voor het controleren van meldingen activiteit op basis van de drempel. Moet tussen 5 minuten en 1 dag. ISO 8601 duurnotatie.|
|TimeAggregation van    |voor metrische waarschuwingen  |"Gemiddelde", "Laatst", "Maximum", "Minimum", "None", "Totaal" | Hoe moeten de gegevens die worden verzameld worden gecombineerd in de tijd. De standaardwaarde is de gemiddelde. [Toegestane waarden vindt u hier](https://msdn.microsoft.com/library/microsoft.azure.insights.models.aggregationtype.aspx).|
|operator           |voor metrische waarschuwingen  |                           |De operator die wordt gebruikt om de huidige metrische gegevens vergelijken met de ingestelde drempelwaarde.|
|subscriptionId     |Y                  |                           |Azure abonnement-ID.|
|resourceGroupName  |Y                  |                           |De naam van de bronnengroep voor de bron van het risico.|
|resourceName       |Y                  |                           |De naam van de resource van de bron van het risico.|
|Brontype       |Y                  |                           |Het brontype van de bron van het risico.|
|resourceId         |Y                  |                           |Bron-ID van de resource die risico.|
|resourceRegion     |Y                  |                           |Regio of locatie van de bron van het risico.|
|portalLink         |Y                  |                           |Directe koppeling naar de pagina portal resource samenvatting.|
|Eigenschappen         |N                  |Optioneel                   |Instellen van `<Key, Value>` paren (dat wil zeggen `Dictionary<String, String>`) die informatie over de gebeurtenis bevat. Eigenschappen van dit veld is optioneel. In een aangepaste gebruikersinterface of logica app workflow, kunnen gebruikers die kan worden doorgegeven via de nettolading sleutel/waarden invoeren. De alternatieve manier is om aangepaste eigenschappen doorgeven aan de webhook via de webhook uri zelf (als de query-parameters)|


>[AZURE.NOTE] Het veld eigenschappen kan alleen worden ingesteld met de [Azure Monitor REST API](https://msdn.microsoft.com/library/azure/dn933805.aspx).

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over waarschuwingen Azure en webhooks in de video [Azure waarschuwingen integreren met PagerDuty](http://go.microsoft.com/fwlink/?LinkId=627080)
- [Azure automatiseringsscripts (Runbooks) uitvoeren op Azure waarschuwingen](http://go.microsoft.com/fwlink/?LinkId=627081)
- [Gebruik logica App via Twilio een SMS-bericht verzenden vanuit een Azure waarschuwing](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-text-message-with-logic-app)
- [Gebruik logica App een bericht te verzenden hoeveelheid toegestane vertraging van een waarschuwing Azure](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-slack-with-logic-app)
- [Gebruik logica App een bericht verzenden naar een wachtrij Azure uit een Azure waarschuwing](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-queue-with-logic-app)
