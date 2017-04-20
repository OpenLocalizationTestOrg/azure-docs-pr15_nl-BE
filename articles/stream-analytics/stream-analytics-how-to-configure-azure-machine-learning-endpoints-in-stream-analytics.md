<properties 
    pageTitle="Azure Machine Learning eindpunten configureren in Analytics Stream | Microsoft Azure" 
    description="Machine Language door gebruiker gedefinieerde functies in Analytics Stream"
    keywords=""
    documentationCenter=""
    services="stream-analytics"
    authors="jeffstokes72" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="stream-analytics" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="data-services" 
    ms.date="09/26/2016" 
    ms.author="jeffstok"
/>

# <a name="machine-learning-integration-in-stream-analytics"></a>Machine Learning integratie in Analytics Stream

Stream Analytics ondersteunt door de gebruiker gedefinieerde functies naar Azure Machine Learning eindpunten aanroepen. REST API-ondersteuning voor deze functie wordt beschreven in de [bibliotheek Stream Analytics REST API](https://msdn.microsoft.com/library/azure/dn835031.aspx). Dit artikel bevat aanvullende informatie die nodig is voor een succesvolle implementatie van deze mogelijkheid in Analytics Stream. Een zelfstudie is ook geboekt en is beschikbaar [hier](stream-analytics-machine-learning-integration-tutorial.md).

## <a name="overview-azure-machine-learning-terminology"></a>Overzicht: Azure Machine Learning-terminologie

Microsoft Azure Machine Learning biedt een hulpprogramma voor samenwerking, slepen en neerzetten die kunt u bouwen, testen en implementeren van anticiperende analytische oplossingen voor uw gegevens. Dit programma heet de *Azure Machine Learning Studio*. De studio wordt gebruikt om te communiceren met de Machine trainingsmaterialen eenvoudig bouwen, testen en van uw ontwerp doorlopen. Deze bronnen en de bijbehorende definities staan hieronder.

- **Werkruimte**: de *werkruimte* is een container met alle andere Machine trainingsmaterialen samen in een container voor beheer en controle.
- **Experiment**: *proeven* worden gemaakt door wetenschappers van gegevens gebruikmaken van datasets en trainen van een model van machine learning.
- **Eindpunt**: *eindpunten* zijn de Azure Machine Learning object gebruikt voor functies als invoer neemt, een opgegeven computer leren model toepassen en gescoord uitvoer retourneren.
- **Webservice scoren**: een *webservice scoren* is een verzameling van eindpunten zoals hierboven vermeld.

Elk eindpunt heeft de API's voor het uitvoeren van batch en synchrone uitvoering. Stream Analytics synchrone uitvoering gebruikt. De specifieke service met de naam van een [Service aanvraag en respons](../machine-learning/machine-learning-consume-web-services.md#request-response-service-rrs) in AzureML studio.

## <a name="machine-learning-resources-needed-for-stream-analytics-jobs"></a>Trainingsmaterialen die nodig zijn voor taken Stream Analytics machine

Voor de toepassing van de Stream Analytics verwerking van een taak, een aanvraag/reactie-eindpunt, een [apikey](../machine-learning/machine-learning-connect-to-azure-machine-learning-web-service.md#get-an-azure-machine-learning-authorization-key)en een definitie van swagger alle noodzakelijk zijn voor een geslaagde uitvoering. Analytics Stream heeft een extra eindpunt dat wordt de url van het eindpunt van de swagger, zoekt de interface en een standaarddefinitie UDF aan de gebruiker als resultaat gegeven.

## <a name="configure-a-stream-analytics-and-machine-learning-udf-via-rest-api"></a>Een Stream Analytics en UDF via REST API leren Machine configureren

Met behulp van REST API's kunt u uw taak om Azure Machine Language-functies configureren. De stappen zijn als volgt:

1. Maak een taak Stream Analytics
2. Input definiëren
3. Definieer een output
4. Maken van een door de gebruiker gedefinieerde functie (UDF)
5. Een transformatie Stream Analytics die de UDF roept schrijven
6. De taak starten

## <a name="creating-a-udf-with-basic-properties"></a>Een UDF maken met basiseigenschappen

De volgende code maakt bijvoorbeeld een scalaire UDF met de naam *newudf* aan een eindpunt Azure Machine Learning koppelt. Merk op dat het *eindpunt* (service-URI) u op de help-pagina voor de API voor de gekozen service en de *apiKey vinden kunt* vindt u op de hoofdpagina van Services.

````
    PUT : /subscriptions/<subscriptionId>/resourceGroups/<resourceGroup>/providers/Microsoft.StreamAnalytics/streamingjobs/<streamingjobName>/functions/<udfName>?api-version=<apiVersion>  
````

Voorbeeld hoofdgedeelte van de aanvraag:  

````
    {
        "name": "newudf",
        "properties": {
            "type": "Scalar",
            "properties": {
                "binding": {
                    "type": "Microsoft.MachineLearning/WebService",
                    "properties": {
                        "endpoint": "https://ussouthcentral.services.azureml.net/workspaces/f80d5d7a77fb4b46bf2a30c63c078dca/services/b7be5e40fd194258796fb402c1958eaf/execute ",
                        "apiKey": "replacekeyhere"
                    }
                }
            }
        }
    }
````

## <a name="call-retrievedefaultdefinition-endpoint-for-default-udf"></a>Oproep RetrieveDefaultDefinition eindpunt voor UDF-standaard

De volledige definitie van de UDF is nodig zodra het skelet UDF is gemaakt. Het eindpunt van de RetreiveDefaultDefinition kunt u de standaard-definitie voor een scalaire functie die is gekoppeld aan een eindpunt Azure Machine Learning. De onderstaande nettolading, moet u de standaard UDF-definitie voor een scalaire functie die is gekoppeld aan een eindpunt Azure Machine Learning. De werkelijke eindpunt wordt niet gedefinieerd als reeds is verstrekt tijdens de opslag. Stream Analytics roept het eindpunt dat is opgegeven in de aanvraag als deze expliciet wordt opgegeven. Als deze wordt gebruikt met een oorspronkelijk waarnaar wordt verwezen. Hier de UDF neemt één parameter (zin) en geeft een enkele uitvoer van het type tekenreeks de tekenreeks die aangeeft dat het label "sentiment" voor die zin.

````
POST : /subscriptions/<subscriptionId>/resourceGroups/<resourceGroup>/providers/Microsoft.StreamAnalytics/streamingjobs/<streamingjobName>/functions/<udfName>/RetrieveDefaultDefinition?api-version=<apiVersion>
````

Voorbeeld hoofdgedeelte van de aanvraag:  

````
    {
        "bindingType": "Microsoft.MachineLearning/WebService",
        "bindingRetrievalProperties": {
            "executeEndpoint": null,
            "udfType": "Scalar"
        }
    }
````

Een voorbeeld van een uitvoer van deze zoekt iets graag hieronder.  

````
    {
        "name": "newudf",
        "properties": {
            "type": "Scalar",
            "properties": {
                "inputs": [{
                    "dataType": "nvarchar(max)",
                    "isConfigurationParameter": null
                }],
                "output": {
                    "dataType": "nvarchar(max)"
                },
                "binding": {
                    "type": "Microsoft.MachineLearning/WebService",
                    "properties": {
                        "endpoint": "https://ussouthcentral.services.azureml.net/workspaces/f80d5d7a77ga4a4bbf2a30c63c078dca/services/b7be5e40fd194258896fb602c1858eaf/execute",
                        "apiKey": null,
                        "inputs": {
                            "name": "input1",
                            "columnNames": [{
                                "name": "tweet",
                                "dataType": "string",
                                "mapTo": 0
                            }]
                        },
                        "outputs": [{
                            "name": "Sentiment",
                            "dataType": "string"
                        }],
                        "batchSize": 10
                    }
                }
            }
        }
    }
````

## <a name="patch-udf-with-the-response"></a>UDF patch met het antwoord 

De UDF moet nu worden gerepareerd met het vorige antwoord, zoals hieronder wordt weergegeven.

````
PATCH : /subscriptions/<subscriptionId>/resourceGroups/<resourceGroup>/providers/Microsoft.StreamAnalytics/streamingjobs/<streamingjobName>/functions/<udfName>?api-version=<apiVersion>
````

Hoofdgedeelte van de aanvraag (Output van RetrieveDefaultDefinition):

````
    {
        "name": "newudf",
        "properties": {
            "type": "Scalar",
            "properties": {
                "inputs": [{
                    "dataType": "nvarchar(max)",
                    "isConfigurationParameter": null
                }],
                "output": {
                    "dataType": "nvarchar(max)"
                },
                "binding": {
                    "type": "Microsoft.MachineLearning/WebService",
                    "properties": {
                        "endpoint": "https://ussouthcentral.services.azureml.net/workspaces/f80d5d7a77ga4a4bbf2a30c63c078dca/services/b7be5e40fd194258896fb602c1858eaf/execute",
                        "apiKey": null,
                        "inputs": {
                            "name": "input1",
                            "columnNames": [{
                                "name": "tweet",
                                "dataType": "string",
                                "mapTo": 0
                            }]
                        },
                        "outputs": [{
                            "name": "Sentiment",
                            "dataType": "string"
                        }],
                        "batchSize": 10
                    }
                }
            }
        }
    }
````

## <a name="implement-stream-analytics-transformation-to-call-the-udf"></a>Stream Analytics transformatie aan te roepen van de UDF implementeren

Nu de UDF (hier scoreTweet genoemd) voor elke invoer gebeurtenis query en schrijft een antwoord voor die gebeurtenis aan een uitvoer.  

````
    {
        "name": "transformation",
        "properties": {
            "streamingUnits": null,
            "query": "select *,scoreTweet(Tweet) TweetSentiment into blobOutput from blobInput"
        }
    }
````


## <a name="get-help"></a>Help-informatie opvragen
Probeer onze [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics) voor verdere ondersteuning

## <a name="next-steps"></a>Volgende stappen

- [Inleiding tot Analytics Azure Stream](stream-analytics-introduction.md)
- [Aan de slag met Azure Stream Analytics](stream-analytics-get-started.md)
- [Schaal Azure Stream Analytics taken](stream-analytics-scale-jobs.md)
- [Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure Stream Analytics beheer REST API: naslag](https://msdn.microsoft.com/library/azure/dn835031.aspx)
