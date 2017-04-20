<properties 
    pageTitle="Auteur definities logica App | Microsoft Azure" 
    description="Meer informatie over het schrijven de JSON-definitie voor logica apps" 
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
    ms.date="07/25/2016"
    ms.author="jehollan"/>
    
# <a name="author-logic-app-definitions"></a>Auteur logica App definities
In dit onderwerp wordt beschreven hoe [Azure logica Apps](app-service-logic-what-are-logic-apps.md) -definities, is een eenvoudige, declaratieve JSON-taal. Als u dit nog niet hebt gedaan, over [het maken van een nieuwe logica app](app-service-logic-create-a-logic-app.md) eerst controleren. Ook vindt u de [volledige verwijzing materiaal van de definition language op MSDN](http://aka.ms/logicappsdocs).

## <a name="several-steps-that-repeat-over-a-list"></a>Verschillende stappen herhalen in een lijst

U kunt gebruikmaken van de [foreach-type](app-service-logic-loops-and-scopes.md) voor het herhalen van een matrix met maximaal 10 k en een actie uitvoeren voor elk.

## <a name="a-failure-handling-step-if-something-goes-wrong"></a>Een stap fout afhandeling als er iets misgaat

Meestal willen kunnen schrijven een *stap herstel* , bepaalde logica die wordt uitgevoerd als **en slechts als**, een of meer van uw gesprekken is mislukt. We bij het ophalen van gegevens uit een groot aantal plaatsen in het volgende voorbeeld, maar als de aanroep mislukt, ik wil een bericht ergens zodat ik dat later kunt opsporen:  

```
{
    "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
    },
    "triggers": {
        "manual": {
            "type": "manual"
        }
    },
    "actions": {
        "readData": {
            "type": "Http",
            "inputs": {
                "method": "GET",
                "uri": "http://myurl"
            }
        },
        "postToErrorMessageQueue": {
            "type": "ApiConnection",
            "inputs": "...",
            "runAfter": {
                "readData": ["Failed"]
            }
        }
    },
    "outputs": {}
}
```

Kunt u het gebruik van de `runAfter` eigenschap om de `postToErrorMessageQueue` wordt alleen uitgevoerd na `readData` is **mislukt**.  Dit kan ook een lijst van mogelijke waarden, dus zijn `runAfter` kan worden `["Succeeded", "Failed"]`.

Ten slotte, omdat u nu de fout verwerkt hebt, wij niet meer markeren de uitvoeren als **mislukt**. Zoals u hier zien kunt, is het uitvoeren **geslaagd** , hoewel een stap mislukt, omdat ik de stap voor het verwerken van deze fout geschreven.

## <a name="two-or-more-steps-that-execute-in-parallel"></a>Twee (of meer) werk dat parallel worden uitgevoerd.

Als u meerdere acties worden uitgevoerd in parallel, de `runAfter` eigenschap tijdens runtime gelijkwaardig moet zijn. 

```
{
    "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {},
    "triggers": {
        "manual": {
            "type": "manual"
        }
    },
    "actions": {
        "readData": {
            "type": "Http",
            "inputs": {
                "method": "GET",
                "uri": "http://myurl"
            }
        },
        "branch1": {
            "type": "Http",
            "inputs": "...",
            "runAfter": {
                "readData": ["Succeeded"]
            }
        },
        "branch2": {
            "type": "Http",
            "inputs": "...",
            "runAfter": {
                "readData": ["Succeeded"]
            }
        }
    },
    "outputs": {}
}
```

Zoals u in het voorbeeld hierboven, beide zien kunt `branch1` en `branch2` zijn ingesteld op uitvoeren na `readData`. Als gevolg hiervan worden beide van deze bijkantoren in parallel uitgevoerd:

![Parallel](./media/app-service-logic-author-definitions/parallel.png)

Ziet u dat het tijdstempel voor beide takken identiek is. 

## <a name="join-two-parallel-branches"></a>Lid worden van twee parallelle vertakkingen

U kunt deelnemen aan twee acties die zijn ingesteld in parallel uitvoeren door het toevoegen van items aan de `runAfter` eigenschap vergelijkbaar naar boven.

```
{
    "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-04-01-preview/workflowdefinition.json#",
    "actions": {
        "readData": {
            "inputs": {
                "method": "GET",
                "uri": "http://myurl"
            },
            "runAfter": {},
            "type": "Http"
        },
        "branch1": {
            "inputs": {
                "method": "GET",
                "uri": "http://myurl"
            },
            "runAfter": {
                "readData": [
                    "Succeeded"
                ]
            },
            "type": "Http"
        },
        "branch2": {
            "inputs": {
                "method": "GET",
                "uri": "http://myurl"
            },
            "runAfter": {
                "readData": [
                    "Succeeded"
                ]
            },
            "type": "Http"
        },
        "join": {
            "inputs": {
                "method": "GET",
                "uri": "http://myurl"
            },
            "runAfter": {
                "branch1": [
                    "Succeeded"
                ],
                "branch2": [
                    "Succeeded"
                ]
            },
            "type": "Http"
        }
    },
    "contentVersion": "1.0.0.0",
    "outputs": {},
    "parameters": {},
    "triggers": {
        "manual": {
            "inputs": {
                "schema": {}
            },
            "kind": "Http",
            "type": "Request"
        }
    }
}
```

![Parallel](./media/app-service-logic-author-definitions/join.png)

## <a name="mapping-items-in-a-list-to-some-different-configuration"></a>Items in een lijst toewijzen aan een andere configuratie

Volgende, Stel dat we willen volledig verschillende inhoud afhankelijk van de waarde van een eigenschap ophalen. Wij kunnen een kaart van waarden naar bestemmingen als parameter maken:  

```
{
    "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "specialCategories": {
            "defaultValue": ["science", "google", "microsoft", "robots", "NSA"],
            "type": "Array"
        },
        "destinationMap": {
            "defaultValue": {
                "science": "http://www.nasa.gov",
                "microsoft": "https://www.microsoft.com/en-us/default.aspx",
                "google": "https://www.google.com",
                "robots": "https://en.wikipedia.org/wiki/Robot",
                "NSA": "https://www.nsa.gov/"
            },
            "type": "Object"
        }
    },
    "triggers": {
        "manual": {
            "type": "manual"
        }
    },
    "actions": {
        "getArticles": {
            "type": "Http",
            "inputs": {
                "method": "GET",
                "uri": "https://ajax.googleapis.com/ajax/services/feed/load?v=1.0&q=http://feeds.wired.com/wired/index"
            },
            "conditions": []
        },
        "getSpecialPage": {
            "type": "Http",
            "inputs": {
                "method": "GET",
                "uri": "@parameters('destinationMap')[first(intersection(item().categories, parameters('specialCategories')))]"
            },
            "conditions": [{
                "expression": "@greater(length(intersection(item().categories, parameters('specialCategories'))), 0)"
            }],
            "forEach": "@body('getArticles').responseData.feed.entries"
        }
    }
}
```

In dat geval krijgen we eerst een lijst met artikelen en vervolgens de tweede stap zoekt in een map, op basis van de categorie die is gedefinieerd als parameter, welke URL de inhoud uit. 

Twee items hier aandacht te besteden: de [`intersection()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#intersection) gebruikt om te controleren of de categorie overeenkomt met een van de bekende categorieën die zijn gedefinieerd. Zodra we u de categorie, kunnen we ten tweede het item van een kaart met vierkante haken pull: `parameters[...]`. 

## <a name="working-with-strings"></a>Werken met tekenreeksen

Er zijn diverse functies die kunnen worden gebruikt om een tekenreeks te manipuleren. We nemen een voorbeeld hebben we een tekenreeks die we moeten worden doorgegeven aan een systeem waarbij we niet zeker dat tekencodering wordt uitgevoerd correct zijn. Kunt base64 coderen deze tekenreeks. Echter, om te voorkomen dat niet zijn toegestaan in een URL gaan we enkele tekens wilt vervangen. 

Verder willen we een subtekenreeks van de van de order naam omdat de eerste 5 tekens niet worden gebruikt.

```
{
    "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "order": {
            "defaultValue": {
                "quantity": 10,
                "id": "myorder1",
                "orderer": "NAME=Stèphén__Šīçiłianö"
            },
            "type": "Object"
        }
    },
    "triggers": {
        "manual": {
            "type": "manual"
        }
    },
    "actions": {
        "order": {
            "type": "Http",
            "inputs": {
                "method": "GET",
                "uri": "http://www.example.com/?id=@{replace(replace(base64(substring(parameters('order').orderer,5,sub(length(parameters('order').orderer), 5) )),'+','-') ,'/' ,'_' )}"
            }
        }
    },
    "outputs": {}
}
```

Werken van binnenuit:

1. Krijgen de [`length()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#length) van de naam van de besteller, dit geeft weer het totale aantal tekens

2. 5 aftrekken (omdat we een kortere tekenreeks zult)

3. Pas de [`substring()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#substring) . We beginnen bij index `5` en de rest van de tekenreeks.

4. Deze reeks die u wilt converteren een [`base64()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#base64) string

5. [`replace()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#replace)alle van de `+` met tekens`-`

6. [`replace()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#replace)alle van de `/` met tekens`_`

## <a name="working-with-date-times"></a>Werken met datums en tijden

Datums en tijden is handig, vooral wanneer u probeert om gegevens op te halen uit een gegevensbron die **Triggers**natuurlijk niet ondersteunt.  U kunt ook datums en tijden gebruiken om erachter te komen hoe lang de verschillende stappen worden genomen. 

```
{
    "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "order": {
            "defaultValue": {
                "quantity": 10,
                "id": "myorder1"
            },
            "type": "Object"
        }
    },
    "triggers": {
        "manual": {
            "type": "manual"
        }
    },
    "actions": {
        "order": {
            "type": "Http",
            "inputs": {
                "method": "GET",
                "uri": "http://www.example.com/?id=@{parameters('order').id}"
            }
        },
        "timingWarning": {
            "actions" {
                "type": "Http",
                "inputs": {
                    "method": "GET",
                    "uri": "http://www.example.com/?recordLongOrderTime=@{parameters('order').id}&currentTime=@{utcNow('r')}"
                },
                "runAfter": {}
            }
            "expression": "@less(actions('order').startTime,addseconds(utcNow(),-1))"
        }
    },
    "outputs": {}
}
```

In dit voorbeeld zijn we uitpakken de `startTime` van de vorige stap. Vervolgens zijn we ophalen van de huidige tijd en één seconde af te trekken:[`addseconds(..., -1)`](https://msdn.microsoft.com/library/azure/mt643789.aspx#addseconds) (kunt u andere eenheden van tijd, zoals `minutes` of `hours`). Ten slotte kunnen we deze twee waarden vergelijken. Als de eerste lager is dan de tweede, en dat betekent meer dan één seconde is verstreken sinds de eerste is geplaatst. 

Ook de opmerking dat we formatters string gebruiken kunt voor het opmaken van datums: in de queryreeks die ik gebruik [`utcnow('r')`](https://msdn.microsoft.com/library/azure/mt643789.aspx#utcnow) om de RFC1123. Alle datum opmaak [wordt beschreven op MSDN](https://msdn.microsoft.com/library/azure/mt643789.aspx#utcnow). 

## <a name="using-deployment-time-parameters-for-different-environments"></a>Tijd voor serverimplementaties parameters gebruiken voor verschillende omgevingen

Het is gebruikelijk om de levensduur van een implementatie waar u hebt een ontwikkelomgeving, een testomgeving en een productie-omgeving. In al deze u kunt dezelfde definitie, maar gebruik bijvoorbeeld verschillende databases. U kunt ook dezelfde definitie tussen de vele verschillende regio's gebruiken voor hoge beschikbaarheid, maar elk exemplaar van de app logica om te communiceren met de database van de regio. 

Houd er rekening mee dat dit verschilt van de verschillende parameters in *runtime*, waarbij voor dat moet u de `trigger()` zoals hierboven wordt genoemd. 

U kunt beginnen met de definitie van een erg eenvoudig zoals dit:

```
{
    "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "uri": {
            "type": "string"
        }
    },
    "triggers": {
        "manual": {
            "type": "manual"
        }
    },
    "actions": {
        "readData": {
            "type": "Http",
            "inputs": {
                "method": "GET",
                "uri": "@parameters('uri')"
            }
        }
    },
    "outputs": {}
}
```

Klik in de werkelijke `PUT` een aanvraag voor de logica app kunt u opgeven dat de parameter `uri`. Opmerking Als er is niet langer een standaardwaarde die deze parameter is vereist in de nettolading van de app logica:

```
{
    "properties": {},
        "definition": {
          // Use the definition from above here
        },
        "parameters": {
            "connection": {
                "value": "https://my.connection.that.is.per.enviornment"
            }
        }
    },
    "location": "westus"
}
``` 

Op elke omgeving bieden u vervolgens een andere waarde voor de `connection` parameter. 

Zie de [REST API-documentatie](https://msdn.microsoft.com/library/azure/mt643787.aspx) voor alle opties voor het maken en beheren van apps logica. 
