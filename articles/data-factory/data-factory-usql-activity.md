<properties 
    pageTitle="U SQL-script uitvoeren op Azure gegevens Lake Analytics van Azure Data Factory" 
    description="Informatie over het verwerken van gegevens door U SQL-scripts op Azure gegevens Lake Analytics compute-service wordt uitgevoerd." 
    services="data-factory" 
    documentationCenter="" 
    authors="spelluru" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/12/2016" 
    ms.author="spelluru"/>

# <a name="run-u-sql-script-on-azure-data-lake-analytics-from-azure-data-factory"></a>U SQL-script uitvoeren op Azure gegevens Lake Analytics van Azure Data Factory
> [AZURE.SELECTOR]
[Component](data-factory-hive-activity.md)  
[Varken](data-factory-pig-activity.md)  
[MapReduce](data-factory-map-reduce.md)  
[Streaming Hadoop](data-factory-hadoop-streaming-activity.md)
[Machine Learning](data-factory-azure-ml-batch-execution-activity.md) 
[Opgeslagen Procedure](data-factory-stored-proc-activity.md)
[Gegevens Lake Analytics U SQL](data-factory-usql-activity.md)
[.NET aangepaste](data-factory-use-custom-activities.md)
 
Gegevens in gekoppelde storage-services verwerkt een pijpleiding in een fabriek Azure gegevens door middel van gekoppelde compute-services. Het bevat een reeks activiteiten waarbij elke activiteit een bepaalde verwerking uitvoert. Dit artikel beschrijft de **Gegevens Lake Analytics U SQL activiteit** die een **U SQL** -script wordt uitgevoerd op een gekoppeld compute **Azure gegevens Lake Analytics** service. 

> [AZURE.NOTE] 
> Maak een Azure gegevens Lake Analytics account voordat u een pijpleiding met een activiteit gegevens Lake Analytics U SQL. Zie informatie over Azure gegevens Lake Analytics, [aan de slag met Azure gegevens Lake Analytics](../data-lake-analytics/data-lake-analytics-get-started-portal.md).
>  
> Bekijk het [maken van uw eerste pipeline-zelfstudie](data-factory-build-your-first-pipeline.md) voor gedetailleerde stappen om te maken van een data factory, gekoppelde services datasets en een pijpleiding. JSON-fragmenten gebruiken met Data Factory-Editor of Visual Studio of Azure PowerShell Data Factory-entiteiten maken.

## <a name="azure-data-lake-analytics-linked-service"></a>Azure gegevens Lake Analytics Service gekoppeld
Maakt u een service **Azure gegevens Lake Analytics** gekoppeld een compute Azure gegevens Lake Analytics service als u wilt koppelen een Azure data factory genoemd. De gegevens Lake Analytics U SQL-activiteit in de pijplijn verwijst naar deze gekoppelde service. 

In het volgende voorbeeld bevat de JSON-definitie voor een service Azure gegevens Lake Analytics gekoppeld. 

    {
        "name": "AzureDataLakeAnalyticsLinkedService",
        "properties": {
            "type": "AzureDataLakeAnalytics",
            "typeProperties": {
                "accountName": "adftestaccount",
                "dataLakeAnalyticsUri": "datalakeanalyticscompute.net",
                "authorization": "<authcode>",
                "sessionId": "<session ID>", 
                "subscriptionId": "<subscription id>",
                "resourceGroupName": "<resource group name>"
            }
        }
    }


In de volgende tabel vindt u beschrijvingen van de eigenschappen die worden gebruikt in de definitie van JSON. 

Eigenschap | Beschrijving | Vereist
-------- | ----------- | --------
Type | De eigenschap type moet worden ingesteld op: **AzureDataLakeAnalytics**. | Ja
Accountnaam | Azure gegevens Lake Analytics accountnaam. | Ja
dataLakeAnalyticsUri | Azure Data Analytics Lake URI. |  Nee 
autorisatie | Autorisatiecode wordt automatisch opgehaald na **autoriseren** te klikken in de Editor voor Factory en de OAuth-aanmelding is voltooid.  | Ja 
subscriptionId | Azure abonnements-id | Nee (indien niet opgegeven, abonnement van de fabriek van gegevens wordt gebruikt). 
resourceGroupName | Naam Azure resourcegroep |  Nee (indien niet opgegeven, de resourcegroep van de fabriek van gegevens wordt gebruikt).
sessie-id | SessionID van de sessie OAuth vergunning. Elke sessie-id is uniek en kan slechts eenmaal worden gebruikt. De sessie-Id is automatisch gegenereerd in de Editor voor Factory. | Ja

De autorisatiecode die u hebt gegenereerd met de knop **machtigen** verloopt later opnieuw. Zie de volgende tabel voor de verlooptermijn voor verschillende typen gebruikersaccounts. Ziet u het volgende foutbericht weergegeven wanneer de verificatie- **verificatietoken verloopt**: referentie-bewerking fout: invalid_grant - AADSTS70002: fout bij het valideren van de referenties. AADSTS70008: De opgegeven access-subsidie is verlopen of ingetrokken. Trace-ID: d18629e8-af88-43c5-88e3-d8419eb1fca1 correlatie-ID: tijdstempel fac30a0c-6be6-4e02-8d69-a776d2ffefd7: 2015-12-15 21:09:31Z

 
| Gebruikerstype | Verloopt na |
| :-------- | :----------- | 
| Gebruikersaccounts die niet worden beheerd door Active Directory Azure (@hotmail.com, @live.com, enz.) | 12 uur |
| Gebruikersaccounts beheerd door Azure Active Directory (AAD) | 14 dagen na het laatste segment wordt uitgevoerd. <br/><br/>90 dagen, als een segment op basis van de gekoppelde service OAuth gebaseerde ten minste eenmaal per 14 dagen wordt uitgevoerd. |

Om deze fout te voorkomen/oplossen, autoriseren met behulp van de **machtigen** knop wanneer de **verificatietoken verloopt** en in dat geval de gekoppelde service. U kunt ook waarden voor eigenschappen van **sessie-id** en **autorisatie** via programmacode in het volgende gedeelte met code genereren. 

  
### <a name="to-programmatically-generate-sessionid-and-authorization-values"></a>Programmatisch sessie-id en -autorisatie om waarden te genereren 

    if (linkedService.Properties.TypeProperties is AzureDataLakeStoreLinkedService ||
        linkedService.Properties.TypeProperties is AzureDataLakeAnalyticsLinkedService)
    {
        AuthorizationSessionGetResponse authorizationSession = this.Client.OAuth.Get(this.ResourceGroupName, this.DataFactoryName, linkedService.Properties.Type);

        WindowsFormsWebAuthenticationDialog authenticationDialog = new WindowsFormsWebAuthenticationDialog(null);
        string authorization = authenticationDialog.AuthenticateAAD(authorizationSession.AuthorizationSession.Endpoint, new Uri("urn:ietf:wg:oauth:2.0:oob"));

        AzureDataLakeStoreLinkedService azureDataLakeStoreProperties = linkedService.Properties.TypeProperties as AzureDataLakeStoreLinkedService;
        if (azureDataLakeStoreProperties != null)
        {
            azureDataLakeStoreProperties.SessionId = authorizationSession.AuthorizationSession.SessionId;
            azureDataLakeStoreProperties.Authorization = authorization;
        }

        AzureDataLakeAnalyticsLinkedService azureDataLakeAnalyticsProperties = linkedService.Properties.TypeProperties as AzureDataLakeAnalyticsLinkedService;
        if (azureDataLakeAnalyticsProperties != null)
        {
            azureDataLakeAnalyticsProperties.SessionId = authorizationSession.AuthorizationSession.SessionId;
            azureDataLakeAnalyticsProperties.Authorization = authorization;
        }
    }

Zie de [Klasse AzureDataLakeStoreLinkedService](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestorelinkedservice.aspx), [AzureDataLakeAnalyticsLinkedService klasse](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakeanalyticslinkedservice.aspx)en [De klasse AuthorizationSessionGetResponse](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.authorizationsessiongetresponse.aspx) onderwerpen voor meer informatie over de Data Factory klassen gebruikt in de code. Voeg een verwijzing naar: Microsoft.IdentityModel.Clients.ActiveDirectory.WindowsForms.dll voor de klasse WindowsFormsWebAuthenticationDialog. 
 
 
## <a name="data-lake-analytics-u-sql-activity"></a>Data Analytics Lake U SQL-activiteit 

In het volgende fragment van JSON definieert een pijpleiding aan een activiteit gegevens Lake Analytics U SQL. De definitie van de activiteit is een verwijzing naar de Azure gegevens Lake Analytics gekoppeld service die u eerder hebt gemaakt.   
  

    {
        "name": "ComputeEventsByRegionPipeline",
        "properties": {
            "description": "This is a pipeline to compute events for en-gb locale and date less than 2012/02/19.",
            "activities": 
            [
                {
                    "type": "DataLakeAnalyticsU-SQL",
                    "typeProperties": {
                        "scriptPath": "scripts\\kona\\SearchLogProcessing.txt",
                        "scriptLinkedService": "StorageLinkedService",
                        "degreeOfParallelism": 3,
                        "priority": 100,
                        "parameters": {
                            "in": "/datalake/input/SearchLog.tsv",
                            "out": "/datalake/output/Result.tsv"
                        }
                    },
                    "inputs": [
                        {
                            "name": "DataLakeTable"
                        }
                    ],
                    "outputs": 
                    [
                        {
                            "name": "EventsByRegionTable"
                        }
                    ],
                    "policy": {
                        "timeout": "06:00:00",
                        "concurrency": 1,
                        "executionPriorityOrder": "NewestFirst",
                        "retry": 1
                    },
                    "scheduler": {
                        "frequency": "Day",
                        "interval": 1
                    },
                    "name": "EventsByRegion",
                    "linkedServiceName": "AzureDataLakeAnalyticsLinkedService"
                }
            ],
            "start": "2015-08-08T00:00:00Z",
            "end": "2015-08-08T01:00:00Z",
            "isPaused": false
        }
    }


De volgende tabel beschrijft de namen en beschrijvingen van eigenschappen die specifiek voor deze activiteit zijn. 

Eigenschap | Beschrijving | Vereist
:-------- | :----------- | :--------
type | De eigenschap type moet worden ingesteld op **DataLakeAnalyticsU SQL**. | Ja
scriptPath | Het pad naar de map waarin het U SQL-script. Naam van het bestand is hoofdlettergevoelig. | Nee (als u een script gebruiken)
scriptLinkedService | Gekoppelde service die de opslag met het script aan de fabriek van de gegevens is gekoppeld | Nee (als u een script gebruiken)
script | Inline-script in plaats van scriptPath en scriptLinkedService opgeven. Bijvoorbeeld: 'script': 'CREATE databasetest'. | Nee (als u scriptPath en scriptLinkedService)
degreeOfParallelism | Het maximum aantal knooppunten die gelijktijdig wordt gebruikt voor het uitvoeren van de taak. | Nee
prioriteit | Bepaalt welke taken bij alle in de wachtrij moeten eerst worden geselecteerd. Hoe lager het getal, hoe hoger de prioriteit. | Nee 
parameters | Parameters voor het script dat U SQL | Nee 

Zie de [Definitie van SearchLogProcessing.txt-Script](#script-definition) voor de definitie van het script. 

## <a name="sample-input-and-output-datasets"></a>Voorbeeld van invoer en uitvoer datasets

### <a name="input-dataset"></a>Invoer dataset
In dit voorbeeld wordt de ingevoerde gegevens opgeslagen in een winkel in Azure gegevens Lake (SearchLog.tsv-bestand in de map datalake/input). 

    {
        "name": "DataLakeTable",
        "properties": {
            "type": "AzureDataLakeStore",
            "linkedServiceName": "AzureDataLakeStoreLinkedService",
            "typeProperties": {
                "folderPath": "datalake/input/",
                "fileName": "SearchLog.tsv",
                "format": {
                    "type": "TextFormat",
                    "rowDelimiter": "\n",
                    "columnDelimiter": "\t"
                }
            },
            "availability": {
                "frequency": "Day",
                "interval": 1
            }
        }
    }   

### <a name="output-dataset"></a>Dataset-uitvoer
In dit voorbeeld wordt geproduceerd door het script U SQL outputgegevens opgeslagen in een Azure gegevens Lake archief (datalake/output map). 

    {
        "name": "EventsByRegionTable",
        "properties": {
            "type": "AzureDataLakeStore",
            "linkedServiceName": "AzureDataLakeStoreLinkedService",
            "typeProperties": {
                "folderPath": "datalake/output/"
            },
            "availability": {
                "frequency": "Day",
                "interval": 1
            }
        }
    }

### <a name="sample-data-lake-store-linked-service"></a>Monster Lake gegevensarchief gekoppeld Service
Hier is de definitie van het monster dat Azure Lake gegevensarchief service wordt gebruikt door het i/o-datasets gekoppeld. 

    {
        "name": "AzureDataLakeStoreLinkedService",
        "properties": {
            "type": "AzureDataLakeStore",
            "typeProperties": {
                "dataLakeUri": "https://<accountname>.azuredatalakestore.net/webhdfs/v1",
                "sessionId": "<session ID>",
                "authorization": "<authorization URL>"
            }
        }
    }

Zie artikel [verplaatsen van gegevens van en naar Azure Lake gegevensarchief](data-factory-azure-datalake-connector.md) voor een beschrijving van JSON-eigenschappen. 

## <a name="sample-u-sql-script"></a>Voorbeeld U SQL-Script 

    @searchlog =
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM @in
        USING Extractors.Tsv(nullEscape:"#NULL#");
    
    @rs1 =
        SELECT Start, Region, Duration
        FROM @searchlog
    WHERE Region == "en-gb";
    
    @rs1 =
        SELECT Start, Region, Duration
        FROM @rs1
        WHERE Start <= DateTime.Parse("2012/02/19");
    
    OUTPUT @rs1   
        TO @out
          USING Outputters.Tsv(quoting:false, dateTimeFormat:null);

De waarden voor **@in** en **@out** parameters in het U SQL-script worden doorgegeven dynamisch door de ADF met behulp van de sectie 'parameters'. Zie de sectie 'parameters' in de definitie van de pijpleiding.

Kunt u andere eigenschappen zoals degreeOfParallelism en prioriteit ook in de definitie van de pijplijn voor de taken die worden uitgevoerd op de Azure gegevens Lake Analytics-service.

## <a name="dynamic-parameters"></a>Dynamische parameters
In de definitie van de pijpleiding monster worden in-en uitchecken parameters toegewezen met concrete waarden. 

    "parameters": {
        "in": "/datalake/input/SearchLog.tsv",
        "out": "/datalake/output/Result.tsv"
    }

Het is mogelijk om dynamische parameters gebruiken. Bijvoorbeeld: 

    "parameters": {
        "in": "$$Text.Format('/datalake/input/{0:yyyy-MM-dd HH:mm:ss}.tsv', SliceStart)",
        "out": "$$Text.Format('/datalake/output/{0:yyyy-MM-dd HH:mm:ss}.tsv', SliceStart)"
    }

In dit geval invoerbestanden worden nog opgehaald uit de map /datalake/input en output-bestanden worden gegenereerd in de map /datalake/output. De bestandsnamen zijn dynamisch, gebaseerd op de begintijd van het segment.  