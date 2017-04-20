<properties
   pageTitle="Logica Apps Exception Handling | Microsoft Azure"
   description="Informatie over patronen voor de fout en met Azure logica Apps afhandeling van uitzonderingen"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="jeffhollan"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="10/18/2016"
   ms.author="jehollan"/>

# <a name="logic-apps-error-and-exception-handling"></a>Logische Apps fout en afhandeling van uitzonderingen

Logica Apps biedt een uitgebreide set hulpmiddelen en patronen om ervoor te zorgen dat uw integraties zijn krachtige en robuuste tegen storingen.  Een van de uitdagingen met een integratiearchitectuur is ervoor te zorgen dat uitval of problemen van afhankelijke systemen correct worden afgehandeld.  Apps logica wordt foutafhandeling een eerste-klas ervaring, zodat u de hulpprogramma's die u moet reageren op uitzonderingen en fouten in uw werkstromen.

## <a name="retry-policies"></a>Beleid opnieuw

De belangrijkste uitzondering en foutafhandeling is een beleid voor het opnieuw.  Dit beleid wordt gedefinieerd als de actie opnieuw proberen moet als eerste aanvraag een time-out of niet (elk verzoek dat heeft geresulteerd in een 429 of 5xx-antwoord).  Standaard opnieuw alle acties 4 extra keer gedurende 20 seconden intervallen.  Als de eerste aanvraag ontvangen een `500 Internal Server Error` reactie, workflow-engine wordt onderbroken voor 20 seconden en probeert de aanvraag opnieuw.  Als na alle pogingen het antwoord is nog steeds een uitzondering of storing, de werkstroom wordt voortgezet en de status van de actie als `Failed`.

U kunt beleid opnieuw configureren in de **invoer** van een bepaalde actie.  Een beleid voor het opnieuw kan worden geconfigureerd om te proberen maar liefst 4 keer meer dan 1 uur intervallen.  Alle informatie over de eigenschappen voor de invoer kunnen worden [gevonden op MSDN][retryPolicyMSDN].

```json
"retryPolicy" : {
      "type": "<type-of-retry-policy>",
      "interval": <retry-interval>,
      "count": <number-of-retry-attempts>
    }
```

Als u uw HTTP-actie 4 keer opnieuw proberen en wacht 10 minuten tussen elke poging hebt u de volgende definitie:

```json
"HTTP": 
{
    "inputs": {
        "method": "GET",
        "uri": "http://myAPIendpoint/api/action",
        "retryPolicy" : {
            "type": "fixed",
            "interval": "PT10M",
            "count": 4
        }
    },
    "runAfter": {},
    "type": "Http"
}
```

Voor meer informatie over de ondersteunde syntaxis, raadpleegt u de [sectie opnieuw beleid op MSDN][retryPolicyMSDN].

## <a name="runafter-property-to-catch-failures"></a>De eigenschap RunAfter om af te vangen fouten

Elke actie logica app verklaart welke acties moeten uitvoeren voordat de actie wordt gestart.  U kunt dit beschouwen als de volgorde van de stappen in de workflow.  Deze volgorde is bekend als de `runAfter` eigenschap in de definitie van de actie.  Het is een object dat beschrijft welke acties en de status van de actie de actie wilt uitvoeren.  Alle acties die worden toegevoegd via de ontwerpfunctie zijn standaard ingesteld op `runAfter` de vorige stap als de vorige stap is `Succeeded`.  U kunt deze waarde acties gestart wanneer vorige acties zijn echter aanpassen `Failed`, `Skipped`, of een mogelijke verzameling van deze waarden.  Als u een item wilt toevoegen aan een bepaalde Service Bus onderwerp na een specifieke actie `Insert_Row` mislukt, gebruikt u de volgende `runAfter` configuratie:

```json
"Send_message": {
    "inputs": {
        "body": {
            "ContentData": "@{encodeBase64(body('Insert_Row'))}",
            "ContentType": "{ \"content-type\" : \"application/json\" }"
        },
        "host": {
            "api": {
                "runtimeUrl": "https://logic-apis-westus.azure-apim.net/apim/servicebus"
            },
            "connection": {
                "name": "@parameters('$connections')['servicebus']['connectionId']"
            }
        },
        "method": "post",
        "path": "/@{encodeURIComponent('failures')}/messages"
    },
    "runAfter": {
        "Insert_Row": [
            "Failed"
        ]
    }
}
```

Aankondiging van de `runAfter` eigenschap is ingesteld op gestart als de `Insert_Row` is `Failed`.  De actie wordt uitgevoerd als de status van de actie `Succeeded`, `Failed`, of `Skipped` is de syntaxis:

```json
"runAfter": {
        "Insert_Row": [
            "Failed", "Succeeded", "Skipped"
        ]
    }
```

>[AZURE.TIP] Acties die worden uitgevoerd nadat een vorige actie is mislukt en wordt voltooid, wordt gemarkeerd als `Succeeded`.  Deze werking houdt in dat als u met succes werkelijk alle fouten in een werkstroom de uitvoering zelf gemarkeerd als `Succeeded`.

## <a name="scopes-and-results-to-evaluate-actions"></a>Scopes en resultaten te evalueren acties

Vergelijkbaar met hoe u kunt uitvoeren na afzonderlijke acties kunt u ook acties in de groep samen binnen een [scope](app-service-logic-loops-and-scopes.md) - die als een logische groepering van handelingen fungeren.  Scopes zijn handig voor het ordenen van uw app logica acties, zowel voor het uitvoeren van statistische evaluaties van de status van een scope.  De scope zelf ontvangt een status nadat alle acties binnen een scope hebt voltooid.  De status van de scope wordt bepaald met dezelfde criteria als een run--als de laatste actie in een vertakking van de uitvoering van `Failed` of `Aborted` de status `Failed`.

U kunt `runAfter` een scope is gemarkeerd `Failed` bij brand van specifieke acties voor eventuele fouten die zijn opgetreden in het kader.  Uitgevoerd nadat een scope is mislukt, kunt u één enkele actie catch storingen als niet *alle* acties binnen het bereik maken.

### <a name="getting-the-context-of-failures-with-results"></a>De context van fouten met resultaten ophalen

Afvangen van fouten van een scope is erg handig, maar u kunt ook de context te begrijpen precies welke acties is mislukt, en eventuele fouten of statuscodes die zijn geretourneerd.  De `@result()` functie van de werkstroom biedt context in het resultaat van alle acties binnen een scope.

`@result()`wordt een enkele parameter, de scopenaam en geeft als resultaat een matrix van alle actie resultaten van binnen dat bereik.  Deze actie-objecten bevatten dezelfde kenmerken als de `@actions()` object, met inbegrip van actie begintijd, eindtijd actie Actiestatus, actie ingangen, actie correlatie-id's en actie uitgevoerd.  U kunt eenvoudig combineren een `@result()` functioneren met een `runAfter` voor het verzenden van de context van alle acties die niet binnen een scope.

Als u wilt dat een actie *voor elke* actie uitvoeren in een scope die `Failed`, u kunt combineren `@result()` van de actie van een **[Array met filters](../connectors/connectors-native-query.md)** en een **[ForEach](app-service-logic-loops-and-scopes.md)** lus.  Hiermee kunt u de matrix met resultaten van acties die niet filteren.  U kunt de gefilterde resultaten matrix en een actie uitvoeren voor elke fout met de **ForEach** lus.  Hier volgt een voorbeeld hieronder, gevolgd door een gedetailleerde uitleg.  In het volgende voorbeeld stuurt een HTTP POST-aanvraag met de hoofdtekst van de reactie van alle acties die niet binnen het bereik `My_Scope`.

```json
"Filter_array": {
    "inputs": {
        "from": "@result('My_Scope')",
        "where": "@equals(item()['status'], 'Failed')"
    },
    "runAfter": {
        "My_Scope": [
            "Failed"
        ]
    },
    "type": "Query"
},
"For_each": {
    "actions": {
        "Log_Exception": {
            "inputs": {
                "body": "@item()['outputs']['body']",
                "method": "POST",
                "headers": {
                    "x-failed-action-name": "@item()['name']",
                    "x-failed-tracking-id": "@item()['clientTrackingId']"
                },
                "uri": "http://requestb.in/"
            },
            "runAfter": {},
            "type": "Http"
        }
    },
    "foreach": "@body('Filter_array')",
    "runAfter": {
        "Filter_array": [
            "Succeeded"
        ]
    },
    "type": "Foreach"
}
```

Hier is een gedetailleerd overzicht van wat er gebeurt:

1. Actie van de **Array met filters** voor het filteren van de `@result('My_Scope')` om het resultaat van alle acties binnen`My_Scope`
1. Voorwaarde van de **Array met filters** is een `@result()` item met de status is gelijk aan `Failed`.  Hiermee wordt de array van alle resultaten van de actie van filteren `My_Scope` voor een reeks mislukte actie resultaten.
1. Een **voor elke** actie uitvoeren op de uitgangen **Matrix gefilterd** .  Dit zal een actie *voor elk* mislukt actie resultaat we gefilterd hierboven uitvoeren.
    - Als er één handeling in het bereik dat is mislukt, de acties in het `foreach` slechts één keer uitgevoerd.  Veel mislukte acties veroorzaakt één actie per storing.
1. Verzenden van een HTTP POST op de `foreach` item response-instantie of `@item()['outputs']['body']`.  De `@result()` item shape is hetzelfde als de `@actions()` vorm en kan op dezelfde manier worden geparseerd.
1. Ook twee aangepaste headers met de naam van de mislukte actie opgenomen `@item()['name']` en de mislukte het uitvoeren van het volgnummer `@item()['clientTrackingId']`.

Ter referentie is hier een voorbeeld van een enkele `@result()` artikel.  U ziet de `name`, `body`, en `clientTrackingId` eigenschappen worden geparseerd in het bovenstaande voorbeeld.  Opgemerkt moet worden dat van buiten een `foreach`, `@result()` retourneert een matrix van deze objecten.

```json
{
    "name": "Example_Action_That_Failed",
    "inputs": {
        "uri": "https://myfailedaction.azurewebsites.net",
        "method": "POST"
    },
    "outputs": {
        "statusCode": 404,
        "headers": {
            "Date": "Thu, 11 Aug 2016 03:18:18 GMT",
            "Server": "Microsoft-IIS/8.0",
            "X-Powered-By": "ASP.NET",
            "Content-Length": "68",
            "Content-Type": "application/json"
        },
        "body": {
            "code": "ResourceNotFound",
            "message": "/docs/foo/bar does not exist"
        }
    },
    "startTime": "2016-08-11T03:18:19.7755341Z",
    "endTime": "2016-08-11T03:18:20.2598835Z",
    "trackingId": "bdd82e28-ba2c-4160-a700-e3a8f1a38e22",
    "clientTrackingId": "08587307213861835591296330354",
    "code": "NotFound",
    "status": "Failed"
}
```

U kunt de bovenstaande uitdrukkingen voor het uitvoeren van verschillende patronen de verwerking van uitzonderingen.  U kunt kiezen voor het uitvoeren van een enkele uitzondering verwerken actie buiten het bereik dat het volledige gefilterde matrix storingen en verwijderen accepteert de `foreach`.  U kunt ook andere nuttige eigenschappen van het `@result()` reactie hierboven.

## <a name="azure-diagnostics-and-telemetry"></a>Azure diagnostiek en telemetrie

De bovenstaande patronen fantastische manier om het afhandelen van fouten en uitzonderingen binnen zijn, maar u kunt ook onderkennen en fouten die onafhankelijk is van de groep zelf.  [Diagnostische gegevens van Azure](app-service-logic-monitor-your-logic-apps.md) biedt een eenvoudige manier alle workflowgebeurtenissen (inclusief alle uitvoeren en de actie status) verzenden naar een opslag van Azure-account of een gebeurtenis Azure Hub.  U kunt Logboeken en statistieken controleren of publiceer ze in elk gewenst uitvoeren statussen geëvalueerd controleprogramma.  Een mogelijke optie is voor het stroomsgewijs verzenden van de gebeurtenissen via Azure gebeurtenis Hub in [Analytics Stream](https://azure.microsoft.com/services/stream-analytics/).  In Analytics Stream kunt u live query's van eventuele anomalieën, gemiddelden of storingen van de diagnostische logboeken.  Stream Analytics kunt gemakkelijk aan andere gegevensbronnen zoals wachtrijen, onderwerpen, SQL, DocumentDB en Power BI uitvoeren.

## <a name="next-steps"></a>Volgende stappen
- [Zie hoe een klant gebouwd robuuste foutafhandeling met logica Apps](app-service-logic-scenario-error-and-exception-handling.md)
- [Zoeken naar meer logica Apps voorbeelden en scenario 's](app-service-logic-examples-and-scenarios.md)
- [Informatie over het maken van geautomatiseerde implementatie van logica apps](app-service-logic-create-deploy-template.md)
- [Ontwerpen en implementeren van logische apps vanuit Visual Studio](app-service-logic-deploy-from-vs.md)


<!-- References -->
[retryPolicyMSDN]: https://msdn.microsoft.com/library/azure/mt643939.aspx#Anchor_9