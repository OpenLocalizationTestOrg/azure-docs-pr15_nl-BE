<properties 
    pageTitle="Nieuwe schema versie 2015-08-01-voorbeeld" 
    description="Meer informatie over het schrijven de JSON-definitie voor de nieuwste versie van Logic-apps" 
    authors="stepsic-microsoft-com" 
    manager="dwrede" 
    editor="" 
    services="logic-apps" 
    documentationCenter=""/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="05/31/2016"
    ms.author="stepsic"/>
    
# <a name="new-schema-version-2015-08-01-preview"></a>Nieuwe schema versie 2015-08-01-voorbeeld

Het nieuwe schema en de API voor logica apps-versie heeft een aantal verbeteringen waarmee u de betrouwbaarheid verbeteren en het gemak van gebruik van logica apps. Er zijn 4 belangrijke verschillen:

1. Het type **APIApp** actie is bijgewerkt naar een nieuw type **APIConnection** .
2. **Herhaal** heeft voor **Foreach**gekregen.
3. De **Http-Listener** API app is niet langer vereist.
4. Het aanroepen van onderliggende workflows maakt gebruik van een nieuw schema.

## <a name="1-moving-to-api-connections"></a>1. verplaatsen API verbindingen

De grootste verandering is dat u niet langer API apps implementeren in uw abonnement Azure-API's te gebruiken. Er zijn 2 manieren u API's kunt gebruiken:
* Beheerde API
* Uw aangepaste Web-API's

Elk van deze is iets anders behandeld omdat het beheer en hosting modellen verschillend zijn. Een voordeel van dit model is dat u bent niet langer beperkt tot de bronnen die zijn geïmplementeerd in de resourcegroep. 

### <a name="managed-apis"></a>Beheerde API 's

Er zijn een aantal API's die door Microsoft worden beheerd voor jou, zoals Office 365, televergaderingen, Twitter, FTP, enz... Sommige van deze beheerde API kan worden gebruikt als-is, zoals Bing vertalen, terwijl anderen moeten worden geconfigureerd. Deze configuratie is een *verbinding*genoemd.

Bijvoorbeeld wanneer u Office 365 gebruikt, moet u een verbinding tot stand met uw Office 365-in token. Deze token wordt veilig opgeslagen en vernieuwd zodat uw logica app kunt altijd de Office 365-API. Ook als u verbinding met de SQL- of FTP-server wilt, moet u een verbinding tot stand met de verbindingsreeks. 

In de definitie van deze acties worden genoemd `APIConnection`. Hier volgt een voorbeeld van een verbinding die Office 365 voor het verzenden van een e-mailbericht:

```
{
    "actions": {
        "Send_Email": {
            "type": "ApiConnection",
            "inputs": {
                "host": {
                    "api": {
                        "runtimeUrl": "https://msmanaged-na.azure-apim.net/apim/office365"
                    },
                    "connection": {
                        "name": "@parameters('$connections')['shared_office365']['connectionId']"
                    }
                },
                "method": "post",
                "body": {
                    "Subject": "Reminder",
                    "Body": "Don't forget!",
                    "To": "me@contoso.com"
                },
                "path": "/Mail"
            }
        }
    }
}
```

Het gedeelte van de "inputs" die uniek is voor verbindingen van de API is de `host` object. Dit bestaat uit twee delen: `api` en `connection`.

De `api` is de URL van waar die API beheerd wordt gehost runtime. U ziet alle van de beschikbare beheerde API's voor u door het aanroepen van `GET https://management.azure.com/subscriptions/{subid}/providers/Microsoft.Web/managedApis/?api-version=2015-08-01-preview`.

Wanneer u een API, kan of geen **verbindingsparameters** gedefinieerd. Als dat niet het geval zijn er geen **verbinding** is vereist. Als dit het geval is, hebt u een verbinding maakt. Wanneer u die het de naam die u hebt verbinding maken en vervolgens verwijzen die in de `connection` object binnen de `host` object. Een verbinding wilt maken in een groep, bel:

```
PUT https://management.azure.com/subscriptions/{subid}/resourceGroups/{rgname}/providers/Microsoft.Web/connections/{name}?api-version=2015-08-01-preview
```

Met de volgende tekst:


```
{
  "properties": {
    "api": {
      "id": "/subscriptions/{subid}/providers/Microsoft.Web/managedApis/azureblob"
    },
    "parameterValues" : {
        "accountName" : "{The name of the storage account -- the set of parameters is different for each API}"
    }
  },
  "location" : "{Logic app's location}"
}
```

### <a name="deploying-managed-apis-in-an-azure-resource-manager-template"></a>Implementeren van API's beheren in een sjabloon Azure Resource manager

U kunt een volledige toepassing maken in een sjabloon ARM als interactief aanmelden niet nodig. Als het nodig is-in, u kunt die alles met de ARM sjabloon, maar nog steeds gaat u naar de portal voor het machtigen van de verbindingen. 

```
    "resources": [{
        "apiVersion": "2015-08-01-preview",
        "name": "azureblob",
        "type": "Microsoft.Web/connections",
        "location": "[resourceGroup().location]",
        "properties": {
            "api": {
                "id": "[concat(subscription().id,'/providers/Microsoft.Web/locations/westus/managedApis/azureblob')]"
            },
            "parameterValues": {
                "accountName": "[parameters('storageAccountName')]",
                "accessKey": "[parameters('storageAccountKey')]"
            }
        }
    }, {
        "type": "Microsoft.Logic/workflows",
        "apiVersion": "2015-08-01-preview",
        "name": "[parameters('logicAppName')]",
        "location": "[resourceGroup().location]",
        "dependsOn": [
            "[resourceId('Microsoft.Web/connections', 'azureblob')]"
        ],
        "properties": {
            "sku": {
                "name": "[parameters('sku')]",
                "plan": {
                    "id": "[concat(resourceGroup().id, '/providers/Microsoft.Web/serverfarms/',parameters('svcPlanName'))]"
                }
            },
            "definition": {
                "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2015-08-01-preview/workflowdefinition.json#",
                "actions": {
                    "Create_file": {
                        "type": "apiconnection",
                        "inputs": {
                            "host": {
                                "api": {
                                    "runtimeUrl": "https://logic-apis-westus.azure-apim.net/apim/azureblob"
                                },
                                "connection": {
                                    "name": "@parameters('$connections')['azureblob']['connectionId']"
                                }
                            },
                            "method": "post",
                            "queries": {
                                "folderPath": "[concat('/',parameters('containerName'))]",
                                "name": "helloworld.txt"
                            },
                            "body": "@decodeDataUri('data:,Hello+world!')",
                            "path": "/datasets/default/files"
                        },
                        "conditions": []
                    }
                },
                "contentVersion": "1.0.0.0",
                "outputs": {},
                "parameters": {
                    "$connections": {
                        "defaultValue": {},
                        "type": "Object"
                    }
                },
                "triggers": {
                    "recurrence": {
                        "type": "Recurrence",
                        "recurrence": {
                            "frequency": "Day",
                            "interval": 1
                        }
                    }
                }
            },
            "parameters": {
                "$connections": {
                    "value": {
                        "azureblob": {
                            "connectionId": "[concat(resourceGroup().id,'/providers/Microsoft.Web/connections/azureblob')]",
                            "connectionName": "azureblob",
                            "id": "[concat(subscription().id,'/providers/Microsoft.Web/locations/westus/managedApis/azureblob')]"
                        }

                    }
                }
            }
        }
    }]
```

U kunt zien in dit voorbeeld is dat de verbindingen zijn gewone bronnen die in de resourcegroep wonen. Ze verwijzen naar de managedAPIs die beschikbaar zijn in uw abonnement.

### <a name="your-custom-web-apis"></a>Uw aangepaste Web-API 's

Als u uw eigen API's (met name niet Microsoft beheerd die) gebruikt, moet u de ingebouwde **HTTP** -actie aan te roepen ze gebruiken. Om een ideale ervaring hebt, moet u een eindpunt swagger voor uw API worden blootgesteld. Hierdoor kan de ontwerper van de app logica om invoer en uitvoer voor uw API weer te geven. Zonder een swagger, is de ontwerper alleen mogelijk in- en uitgangen als dekkend JSON-objecten weergeven.

Hier volgt een voorbeeld weergeven van de nieuwe `metadata.apiDefinitionUrl` eigenschap:
```
{
   "actions": {
        "mycustomAPI": {
            "type": "http",
            "metadata" : {
              "apiDefinitionUrl" : "https://mysite.azurewebsites.net/api/apidef/"  
            },
            "inputs": {
                "uri": "https://mysite.azurewebsites.net/api/getsomedata",
                "method" : "GET"
            }
        }
    }
}
```

Als u uw Web API **App** service host en vervolgens automatisch wordt weergegeven in de lijst met acties die beschikbaar zijn in de ontwerpfunctie. Als dit niet het geval is, hebt u direct in de URL te plakken. Het eindpunt swagger moet niet worden-om bruikbaar in de ontwerpfunctie voor logica apps (Hoewel u de API zelf met welke methoden worden ondersteund in de Swagger beveiligen kan).

### <a name="using-your-already-deployed-api-apps-with-2015-08-01-preview"></a>Met behulp van uw reeds geïmplementeerde API apps met 2015-08-01-voorbeeld

Als u een API-app hebt geïmplementeerd, kunt u deze aanroepen via de **HTTP** -actie.

Bijvoorbeeld, als u bestanden van de lijst met Dropbox, wellicht iets dergelijks in uw versie van **2014-12-01-voorbeeld** schemadefinitie:

```
{
    "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2014-12-01-preview/workflowdefinition.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "/subscriptions/423db32d-...-b59f14c962f1/resourcegroups/avdemo/providers/Microsoft.AppService/apiapps/dropboxconnector/token": {
            "defaultValue": "eyJ0eX...wCn90",
            "type": "String",
            "metadata": {
                "token": {
                    "name": "/subscriptions/423db32d-...-b59f14c962f1/resourcegroups/avdemo/providers/Microsoft.AppService/apiapps/dropboxconnector/token"
                }
            }
        }
    },
    "actions": {
        "dropboxconnector": {
            "type": "ApiApp",
            "inputs": {
                "apiVersion": "2015-01-14",
                "host": {
                    "id": "/subscriptions/423db32d-...-b59f14c962f1/resourcegroups/avdemo/providers/Microsoft.AppService/apiapps/dropboxconnector",
                    "gateway": "https://avdemo.azurewebsites.net"
                },
                "operation": "ListFiles",
                "parameters": {
                    "FolderPath": "/myfolder"
                },
                "authentication": {
                    "type": "Raw",
                    "scheme": "Zumo",
                    "parameter": "@parameters('/subscriptions/423db32d-...-b59f14c962f1/resourcegroups/avdemo/providers/Microsoft.AppService/apiapps/dropboxconnector/token')"
                }
            }
        }
    }
}
```

U kunt de equivalente bewerkingen van HTTP zoals hieronder (de sectie parameters van de logica app definitie blijft ongewijzigd) maken:

```
{
    "actions": {
        "dropboxconnector": {
            "type": "Http",
            "metadata" : {
              "apiDefinitionUrl" : "https://avdemo.azurewebsites.net/api/service/apidef/dropboxconnector/?api-version=2015-01-14&format=swagger-2.0-standard"  
            },
            "inputs": {
                "uri": "https://avdemo.azurewebsites.net/api/service/invoke/dropboxconnector/ListFiles?api-version=2015-01-14",
                "method" : "POST",
                "body": {
                    "FolderPath": "/myfolder"
                },
                "authentication": {
                    "type": "Raw",
                    "scheme": "Zumo",
                    "parameter": "@parameters('/subscriptions/423db32d-...-b59f14c962f1/resourcegroups/avdemo/providers/Microsoft.AppService/apiapps/dropboxconnector/token')"
                }
            }
        }
    }
}
```

Stap voor stap één voor één van deze eigenschappen:

| Eigenschap Action |  Beschrijving |
| --------------- | -----------  |
| `type` | `Http`In plaats van`APIapp` |
| `metadata.apiDefinitionUrl` | Als u deze actie gebruiken in de logica apps designer wilt, zult u het eindpunt van de metagegevens bevatten. Dit is samengesteld uit:`{api app host.gateway}/api/service/apidef/{last segment of the api app host.id}/?api-version=2015-01-14&format=swagger-2.0-standard` |
| `inputs.uri` | Dit is samengesteld uit:`{api app host.gateway}/api/service/invoke/{last segment of the api app host.id}/{api app operation}?api-version=2015-01-14` |
| `inputs.method` | Altijd`POST` |
| `inputs.body` | Identiek aan de api app-parameters | 
| `inputs.authentication` | Identiek aan de api app-verificatie |

Deze benadering moet voor alle API app acties werken. Maar houd er rekening mee dat deze eerdere API apps worden niet langer ondersteund en u moet verplaatsen naar een van de twee andere opties boven (een beheerde API of hosten van uw aangepaste Web API).

## <a name="2-repeat-renamed-to-foreach"></a>2. Herhaal de naam gewijzigd in Foreach

Voor de vorige versie we ontvangen veel feedback van klanten die **Herhaal** was onduidelijk en niet goed vast te leggen was echt een voor elke lus. Als gevolg hiervan hebben we deze gewijzigd in **Foreach**. Bijvoorbeeld:

```
{
    "actions": {
        "pingBing": {
            "type": "Http",
            "repeat": "@range(0,2)",
            "inputs": {
                "method": "GET",
                "uri": "https://www.bing.com/search?q=@{repeatItem()}"
            }
        }
    }
}
```

Nu zou worden geschreven als:

```
{
    "actions": {
        "pingBing": {
            "type": "Http",
            "foreach": "@range(0,2)",
            "inputs": {
                "method": "GET",
                "uri": "https://www.bing.com/search?q=@{item()}"
            }
        }
    }
}
```

Eerder de functie `@repeatItem()` is gebruikt om te verwijzen naar het huidige item wordt herhaald. Dit is vereenvoudigd NET `@item()`. 

### <a name="referencing-the-outputs-of-the-foreach"></a>Verwijzen naar de uitgangen van de Foreach
Verder vereenvoudigen, wordt de uitvoer van **Foreach** acties worden verpakt in een object met de naam **repeatItems**. Dit betekent, dat de uitgangen van de bovenstaande herhalen zijn:

```
{
    "repeatItems": [
        {
            "name": "pingBing",
            "inputs": {
                "uri": "https://www.bing.com/search?q=0",
                "method": "GET"
            },
            "outputs": {
                "headers": { },
                "body": "<!DOCTYPE html><html lang=\"en\" xml:lang=\"en\" xmlns=\"http://www.w3.org/1999/xhtml\" xmlns:Web=\"http://schemas.live.com/Web/\">...</html>"
            }
            "status": "Succeeded"
        }
    ]
}
```

Nu dient:

```
[
    {
        "name": "pingBing",
        "inputs": {
            "uri": "https://www.bing.com/search?q=0",
            "method": "GET"
        },
        "outputs": {
            "headers": { },
            "body": "<!DOCTYPE html><html lang=\"en\" xml:lang=\"en\" xmlns=\"http://www.w3.org/1999/xhtml\" xmlns:Web=\"http://schemas.live.com/Web/\">...</html>"
        }
        "status": "Succeeded"
    }
]
```

Wanneer wordt verwezen naar deze uitgangen, als u naar de hoofdtekst van de actie moet u doen:

```
{
    "actions": {
        "secondAction" : {
            "type" : "Http",
            "repeat" : "@outputs('pingBing').repeatItems",
            "inputs" : {
                "method" : "POST",
                "uri" : "http://www.example.com",
                "body" : "@repeatItem().outputs.body"
            }
        }
    }
}
```

U kunt nu in plaats daarvan:

```
{
    "actions": {
        "secondAction" : {
            "type" : "Http",
            "foreach" : "@outputs('pingBing')",
            "inputs" : {
                "method" : "POST",
                "uri" : "http://www.example.com",
                "body" : "@item().outputs.body"
            }
        }
    }
}
```

Met deze wijzigingen, de functies `@repeatItem()`, `@repeatBody()` en `@repeatOutputs()` worden verwijderd.

## <a name="3-native-http-listener"></a>3. ingebouwde HTTP-listener 
De HTTP-Listener-mogelijkheden zijn nu ingebouwd, zodat u niet langer voor de implementatie van een HTTP-Listener API app. Meer informatie over [de volledige details voor het maken van uw logica app eindpunt callable hier](app-service-logic-http-endpoint.md). 

Met deze wijzigingen, de functie `@accessKeys()` wordt verwijderd en is vervangen door de `@listCallbackURL()` functie ten behoeve van het verkrijgen van het eindpunt (indien nodig). Ook moet nu u ten minste één trigger in uw logica app nu. Als u wilt `/run` de werkstroom, moet u een van een `manual`, `apiConnectionWebhook` of `httpWebhook` triggers. 

## <a name="4-calling-child-workflows"></a>4. calling onderliggende workflows

Het aanroepen van onderliggende workflows vereist was, gaat die werkstroom, het toegangstoken opgehaald en plak die in de definitie van de logica app die u wilt bellen van het kind. Met de nieuwe schemaversie zorgt de logica apps engine automatisch voor een SAS tijdens runtime voor de onderliggende werkstroom, wat betekent dat u niet hoeft te plakken geen geheimen in de definitie.  Hier volgt een voorbeeld:

```
"mynestedwf" : {
    "type" : "workflow",
    "inputs" : {
        "host" : {
            "id" : "/subscriptions/xxxxyyyyzzz/resourceGroups/rg001/providers/Microsoft.Logic/mywf001",
            "triggerName" : "myendpointtrigger"
        },
        "queries" : {
            "extrafield" : "specialValue"
        },
        "headers" : {
            "x-ms-date" : "@utcnow()",
            "Content-type" : "application/json"
        },
        "body" : {
            "contentFieldOne" : "value100",
            "anotherField" : 10.001
        }
    },
    "conditions" : []
}
```

Een tweede verbetering is dat we zullen worden de onderliggende workflows volledige toegang geven tot de inkomende aanvraag. Dit betekent dat u kunt parameters doorgeven in het gedeelte *query's* en in de *kop* -object en kunt u volledig de volledige instantie definiëren.

Ten slotte, zijn er de vereiste wijzigingen in de onderliggende werkstroom. Overwegende dat voor u alleen onderliggende werkstroom rechtstreeks aanroepen kan; nu moet u een eindpunt trigger definiëren in de workflow voor het bovenliggende object aan te roepen. Dit betekent in het algemeen zult u een trigger van het type **handmatig** toevoegen en gebruiken die in de definitie van de bovenliggende. Houd er rekening mee dat de `host` eigenschap name is een `triggerName`, omdat het altijd moet u opgeven welke trigger die u aanroept.

## <a name="other-changes"></a>Andere wijzigingen

### <a name="new-queries-property"></a>Nieuwe query's, eigenschap
Alle actietypen ondersteunen nu een nieuwe input **query's**genoemd. Dit is een gestructureerde object in plaats van u hebben voor het samenstellen van de tekenreeks met de hand.

### <a name="parse-function-renamed"></a>Parse() functie gewijzigd
Als we binnenkort meer inhoudstypen wilt toevoegen de `parse()` functie is gewijzigd in `json()`.

## <a name="coming-soon-enterprise-integration-apis"></a>Binnenkort: Enterprise integratie API's
Op dit moment, we nog niet hebben versies van de Enterprise Integration beschikbare API's (zoals AS2) beheerd. Deze zal worden binnenkort in de [Routekaart](http://www.zdnet.com/article/microsoft-outlines-its-cloud-and-server-integration-roadmap-for-2016/)wordt gedekt. Ondertussen, kunt u uw bestaande BizTalk-APIs geïmplementeerd via de HTTP-actie, zoals hierboven in 'Gebruik uw reeds geïmplementeerde API apps."
