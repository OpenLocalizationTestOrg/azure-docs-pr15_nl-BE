<properties
   pageTitle="Logica Apps lussen, Scopes en Debatching | Microsoft Azure"
   description="Logica App lus, het bereik en debatching concepten"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="jeffhollan"
   manager="dwrede"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="05/14/2016"
   ms.author="jehollan"/>
   
# <a name="logic-apps-loops-scopes-and-debatching"></a>Logica Apps lussen, Scopes en Debatching
  
>[AZURE.NOTE] Deze versie van het artikel is van toepassing op logica Apps 2016 04-01-Bekijk schema en hoger.  Concepten zijn vergelijkbaar voor oudere schema's, maar bereiken zijn alleen beschikbaar voor dit schema en hoger.
  
## <a name="foreach-loop-and-arrays"></a>ForEach lus en Arrays
  
Logica Apps kunt u een set gegevens doorlopen en een actie uitvoeren voor elk artikel.  Dit is mogelijk via de `foreach` actie.  In het ontwerp, kunt u opgeven als u wilt toevoegen een voor elke lus.  Na het selecteren van de matrix die u wilt doorlopen, kunt u beginnen acties toe te voegen.  Momenteel zijn beperkt tot één actie per foreach lus, maar deze beperking wordt in de komende weken worden opgeheven.  Eenmaal binnen de lus kunt u opgeven wat er moet gebeuren op elke waarde in de matrix.

Als de weergave code gebruikt, kunt u een voor elke lus zoals hieronder.  Dit is een voorbeeld van een voor elke lus die stuurt een e-mailbericht voor elk e-mailadres met 'microsoft.com':

```
{
    "email_filter": {
        "type": "query",
        "inputs": {
            "from": "@triggerBody()['emails']",
            "where": "@contains(item()['email'], 'microsoft.com')
        }
    },
    "forEach_email": {
        "type": "foreach",
        "foreach": "@body('email_filter')",
        "actions": {
            "send_email": {
                "type": "ApiConnection",
                "inputs": {
                "body": {
                    "to": "@item()",
                    "from": "me@contoso.com",
                    "message": "Hello, thank you for ordering"
                }
                "host": {
                    "connection": {
                        "id": "@parameters('$connections')['office365']['connection']['id']"
                    }
                }
                }
            }
        },
        "runAfter":{
            "email_filter": [ "Succeeded" ]
        }
    }
}
```
  
  A `foreach` actie over maximaal 5.000 rijen matrices kunt doorlopen.  Elke herhaling kunt uitvoeren in parallel, zodat berichten naar een wachtrij toevoegen datatransportbesturing eventueel nodig kan zijn.
  
## <a name="until-loop"></a>Tot en met lus
  
  U kunt een actie of een reeks acties uitvoeren totdat er een voorwaarde is voldaan.  Het meest gangbare scenario voor deze oproep een eindpunt totdat u het antwoord dat u zoekt.  In het ontwerp, kunt u opgeven als u wilt toevoegen een tot lus.  Na het toevoegen van acties binnen de lus, kunt u instellen de voorwaarde voor afsluiten als de lus grenzen.  Er is een vertraging van 1 minuut tussen lus cycli.
  
  Als de weergave code gebruikt, kunt u een pas lus, zoals hieronder.  Dit is een voorbeeld van het aanroepen van een HTTP-eindpunt totdat het hoofdgedeelte van de response heeft de waarde "Voltooid".  Er wordt uitgevoerd wanneer beide 
  
  * HTTP-antwoord heeft de status 'Voltooid'
  * Zij heeft getracht, voor 1 uur
  * Het is 100 keer herhaald
  
  ```
  {
      "until_successful":{
        "type": "until",
        "expression": "@equals(actions('http')['status'], 'Completed'),
        "limit": {
            "count": 100,
            "timeout": "PT1H"
        },
        "actions": {
            "create_resource": {
                "type": "http",
                "inputs": {
                    "url": "http://provisionRseource.com",
                    "body": {
                        "resourceId": "@triggerBody()"
                    }
                }
            }
        }
      }
  }
  ```
  
## <a name="spliton-and-debatching"></a>SplitOn en Debatching

Een trigger wordt soms ook een matrix van items die u wilt debatch en start een werkstroom per artikel.  Dit kunt u doen de `spliton` opdracht.  Als uw trigger swagger een nettolading die een matrix, geeft een `spliton` wordt toegevoegd en start een run per artikel.  SplitOn kunnen alleen worden toegevoegd aan een trigger.  Dit kan handmatig worden geconfigureerd of in de codeweergave definitie opgeheven.  Momenteel SplitOn kan debatch tot 5000 artikelen matrices.  U kunt niet hebben een `spliton` en ook het antwoord syncronous patroon te implementeren.  Een werkstroom die genoemd is een `response` actie in aanvulling op `spliton` wordt asyncronously uitgevoerd en verzend een onmiddellijke `202 Accepted` reactie.  

SplitOn kan worden opgegeven in de weergave code als in het volgende voorbeeld.  Deze krijgen een matrix van items en debatches op elke rij.

```
{
    "myDebatchTrigger": {
        "type": "Http",
        "inputs": {
            "url": "http://getNewCustomers",
        },
        "recurrence": {
            "frequency": "Second",
            "interval": 15
        },
        "spliton": "@triggerBody()['rows']"
    }
}
```

## <a name="scopes"></a>Scopes

Het is mogelijk om een reeks acties maken met behulp van een scope.  Dit is vooral handig voor de uitvoering van de verwerking van uitzonderingen.  U kunt in de ontwerpfunctie voor een nieuwe scope toevoegen en beginnen acties erin toe te voegen.  U kunt bereiken definiëren in de codeweergave als volgt uit:


```
{
    "myScope": {
        "type": "scope",
        "actions": {
            "call_bing": {
                "type": "http",
                "inputs": {
                    "url": "http://www.bing.com"
                }
            }
        }
    }
}
```
