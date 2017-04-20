<properties 
    pageTitle="Pijpleidingen maken/plannen, de keten van activiteiten in de fabriek gegevens | Microsoft Azure" 
    description="Informatie over een pijplijn data in Azure Data Factory te verplaatsen en transformeren van gegevens maken. Een werkstroom door gegevens gestuurde productie klaar om informatie te gebruiken maken." 
    keywords="gegevens pipeline, gegevensgestuurde workflow"
    services="data-factory" 
    documentationCenter="" 
    authors="sharonlo101" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article"
    ms.date="09/12/2016" 
    ms.author="shlo"/>

# <a name="pipelines-and-activities-in-azure-data-factory"></a>Pijpleidingen en activiteiten in Azure Data Factory
In dit artikel helpt u bij het begrijpen van pijpleidingen en activiteiten in Azure Data Factory en deze gebruiken om end-to-end gegevensgestuurde werkstromen voor de verplaatsing van gegevens en gegevens verwerken scenario's samen te stellen.  

> [AZURE.NOTE] In dit artikel wordt ervan uitgegaan dat u via [Azure Data Factory-inleiding](data-factory-introduction.md)hebben voltooid. Als u nog geen hands-op-ervaring met het maken van gegevens via zelfstudie voor het [maken van uw eerste data factory](data-factory-build-your-first-pipeline.md) fabrieken zou helpen begrijpt u dit artikel beter.  

## <a name="what-is-a-data-pipeline"></a>Wat is een pijpleiding gegevens?
**Pijplijn** is een groep logisch samenhangende **activiteiten**. Het wordt gebruikt bij groepsactiviteiten in een eenheid waarmee een taak wordt uitgevoerd. Om pijpleidingen beter te begrijpen, moet u eerst begrijpen van een activiteit. 

## <a name="what-is-an-activity"></a>Wat is een activiteit?
Activiteiten definiëren de acties uit te voeren op de gegevens. Elke activiteit heeft nul of meer [gegevenssets](data-factory-create-datasets.md) als invoer en een of meer gegevenssets als uitvoer produceert. 

U kunt bijvoorbeeld een activiteit kopie kopiëren van gegevens uit een gegevensarchief naar een ander gegevensarchief goedkeuringen. U kunt ook een component HDInsight activiteit een component-query uitvoeren op een cluster Azure HDInsight om uw gegevens te transformeren. Azure Data Factory biedt een breed scala van [gegevenstransformatie](data-factory-data-transformation-activities.md)en activiteiten van de [verplaatsing van gegevens](data-factory-data-movement-activities.md) . U kunt voor het maken van een aangepaste activiteit .NET om uw eigen code uitvoert. 

## <a name="sample-copy-pipeline"></a>Voorbeeld kopiëren pijpleiding
In het volgende voorbeeld pipeline is er een activiteit van het type- **exemplaar** in de sectie **activiteiten** . In dit voorbeeld wordt kopieert de [activiteit kopiëren](data-factory-data-movement-activities.md) gegevens van een Azure Blob-opslag met een Azure SQL-database. 

    {
      "name": "CopyPipeline",
      "properties": {
        "description": "Copy data from a blob to Azure SQL table",
        "activities": [
          {
            "name": "CopyFromBlobToSQL",
            "type": "Copy",
            "inputs": [
              {
                "name": "InputDataset"
              }
            ],
            "outputs": [
              {
                "name": "OutputDataset"
              }
            ],
            "typeProperties": {
              "source": {
                "type": "BlobSource"
              },
              "sink": {
                "type": "SqlSink",
                "writeBatchSize": 10000,
                "writeBatchTimeout": "60:00:00"
              }
            },
            "Policy": {
              "concurrency": 1,
              "executionPriorityOrder": "NewestFirst",
              "retry": 0,
              "timeout": "01:00:00"
            }
          }
        ],
        "start": "2016-07-12T00:00:00Z",
        "end": "2016-07-13T00:00:00Z"
      }
    } 

Houd rekening met de volgende punten:

- In de sectie activiteiten is er slechts één activiteit waarvan het **type** is ingesteld op **kopiëren**.
- Input voor de activiteit is ingesteld op **InputDataset** en uitvoer voor de activiteit is ingesteld op **OutputDataset**.
- **BlobSource** is opgegeven als het bronsoort en **SqlSink** als het type sink is opgegeven in de sectie **typeProperties** .

Zie voor een compleet overzicht van het maken van deze pijpleiding [Zelfstudie: gegevens van Blob-opslag kopiëren naar de SQL-Database](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md). 

## <a name="sample-transformation-pipeline"></a>Monster transformatie pijplijn
In het volgende voorbeeld pipeline is er een activiteit van het type **HDInsightHive** in de sectie **activiteiten** . In dit voorbeeld wordt de [activiteit van de component HDInsight](data-factory-hive-activity.md) gegevens uit een Azure Blob-opslag getransformeerd door het uitvoeren van een scriptbestand component op een cluster Azure HDInsight Hadoop. 

    {
        "name": "TransformPipeline",
        "properties": {
            "description": "My first Azure Data Factory pipeline",
            "activities": [
                {
                    "type": "HDInsightHive",
                    "typeProperties": {
                        "scriptPath": "adfgetstarted/script/partitionweblogs.hql",
                        "scriptLinkedService": "AzureStorageLinkedService",
                        "defines": {
                            "inputtable": "wasb://adfgetstarted@<storageaccountname>.blob.core.windows.net/inputdata",
                            "partitionedtable": "wasb://adfgetstarted@<storageaccountname>.blob.core.windows.net/partitioneddata"
                        }
                    },
                    "inputs": [
                        {
                            "name": "AzureBlobInput"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "AzureBlobOutput"
                        }
                    ],
                    "policy": {
                        "concurrency": 1,
                        "retry": 3
                    },
                    "scheduler": {
                        "frequency": "Month",
                        "interval": 1
                    },
                    "name": "RunSampleHiveActivity",
                    "linkedServiceName": "HDInsightOnDemandLinkedService"
                }
            ],
            "start": "2016-04-01T00:00:00Z",
            "end": "2016-04-02T00:00:00Z",
            "isPaused": false
        }
    }

Houd rekening met de volgende punten: 

- In de sectie activiteiten is er slechts één activiteit waarvan het **type** is ingesteld op **HDInsightHive**.
- Het scriptbestand component, **partitionweblogs.hql**, wordt opgeslagen in de Azure opslag account (zoals opgegeven door de scriptLinkedService, **AzureStorageLinkedService**genoemd) en in de map **scripts** in de container **adfgetstarted**.
- De sectie **definieert** wordt gebruikt om de runtime instellingen die worden doorgegeven aan het script component als component configuratiewaarden (bijvoorbeeld ${hiveconf: inputtable}, {hiveconf:partitionedtable} $).

Zie voor een compleet overzicht van het maken van deze pijpleiding [Zelfstudie: maken van uw eerste pijpleiding om gegevens te verwerken met behulp van Hadoop-cluster](data-factory-build-your-first-pipeline.md). 

## <a name="chaining-activities"></a>Chaining activiteiten
Als er meerdere activiteiten in een pijpleiding en de uitvoer van een activiteit niet de input van een andere activiteit is, kunnen de activiteiten parallel uitgevoerd als invoergegevens segmenten voor de werkzaamheden gereed zijn. 

U kunt twee activiteiten koppelen door de dataset uitvoer van een activiteit als de dataset input van de andere activiteit. De activiteiten kunnen zich in dezelfde pijpleiding of andere pijpleidingen. De tweede activiteit wordt uitgevoerd alleen als eerste is voltooid. 

Neem bijvoorbeeld het volgende geval:
 
1.  Pijpleiding P1 heeft activiteit A1 die externe invoer DataSet-objecten nodig D1 en produceert **uitvoer** dataset **D2**.
2.  Pijpleiding P2 heeft activiteit A2 die **invoer** uit **D2**dataset is vereist, en produceert uitvoer dataset D3.
 
In dit scenario wordt de activiteit A1 wordt uitgevoerd als de externe gegevens beschikbaar zijn en de frequentie van de geplande beschikbaarheid wordt bereikt.  De activiteit A2 wordt uitgevoerd wanneer de geplande segmenten van D2 beschikbaar komen en de frequentie van de geplande beschikbaarheid wordt bereikt. Als er een fout in een van de segmenten in de dataset D2, A2 voor dat segment pas uitgevoerd dit beschikbaar komt.

De weergave Netwerkdiagram:

![Chaining activiteiten in twee pijpleidingen](./media/data-factory-create-pipelines/chaining-two-pipelines.png)

De weergave Netwerkdiagram met beide activiteiten in dezelfde pijpleiding: 

![Activiteiten in dezelfde pijpleiding Chaining](./media/data-factory-create-pipelines/chaining-one-pipeline.png)

Zie voor meer informatie, [planning en uitvoering](#chaining-activities). 

## <a name="scheduling-and-execution"></a>Planning en uitvoering
Tot nu toe hebt u wat pijpleidingen en activiteiten zijn begrepen. U hebt ook gekeken hoe gedefinieerd en een hoog niveau zijn overzicht van de activiteiten in Azure Data Factory. Nu laten we eens kijken hoe ze worden uitgevoerd. 

Een pijpleiding is actief tussen de begin- en eindtijd. Niet wordt uitgevoerd vóór de begintijd of de eindtijd. Als de pijpleiding wordt onderbroken, wordt dat niet uitgevoerd ongeacht de tijd van de begin- en einddatum. Voor een pijpleiding worden uitgevoerd, moet deze niet worden onderbroken. In feite is het niet de pijpleiding die wordt uitgevoerd. Het is van de activiteiten die worden uitgevoerd in de pijplijn. Maar ze doen in de context van de pijpleiding. 

Zie [planning en uitvoering](data-factory-scheduling-and-execution.md) te begrijpen hoe de planning en uitvoering in Azure Data Factory werkt.

## <a name="create-pipelines"></a>Pijpleidingen maken
Azure Data Factory biedt verschillende mechanismen voor het ontwerpen en implementeren van pijpleidingen (die op zijn beurt een of meer activiteiten in deze bevatten). 

### <a name="using-azure-portal"></a>Met behulp van Azure portal
U kunt Data Factory-editor in Azure portal voor het maken van een pijpleiding. Zie [aan de slag met Azure Data Factory (Data Factory-Editor)](data-factory-build-your-first-pipeline-using-editor.md) voor een overzicht van de end-to-end. 

### <a name="using-visual-studio"></a>Met behulp van Visual Studio 
U kunt Visual Studio ontwerpen en implementeren van pijpleidingen voor Azure Data Factory. Zie [aan de slag met Azure Data Factory (Visual Studio)](data-factory-build-your-first-pipeline-using-vs.md) voor een overzicht van de end-to-end. 

### <a name="using-azure-powershell"></a>Azure PowerShell gebruiken
U kunt de Azure PowerShell pijpleidingen maken in Azure Data Factory. U hebt de pijpleiding JSON zeggen gedefinieerd in een bestand op c:\DPWikisample.json. U kunt deze uploaden naar uw exemplaar Azure Data Factory zoals in het volgende voorbeeld wordt getoond:

    New-AzureRmDataFactoryPipeline -ResourceGroupName ADF -Name DPWikisample -DataFactoryName wikiADF -File c:\DPWikisample.json

Zie [aan de slag met Azure Data Factory (Azure PowerShell)](data-factory-build-your-first-pipeline-using-powershell.md) voor een end-to-end Stapsgewijze instructies voor het maken van een fabriek voor gegevens met een pijpleiding. 

### <a name="using-net-sdk"></a>Met behulp van .NET SDK
U kunt maken en pijpleiding via .NET SDK te implementeren. Dit mechanisme kan worden gebruikt voor pijpleidingen maken via programmacode. Zie voor meer informatie [maken, beheren, en fabrieken van gegevens via programmacode te controleren](data-factory-create-data-factories-programmatically.md). 


### <a name="using-azure-resource-manager-template"></a>Met behulp van de sjabloon Azure Resource Manager
U kunt maken en implementeren met behulp van een sjabloon Azure Resource Manager pijpleiding. Zie [aan de slag met Azure Data Factory (Azure Resource Manager)](data-factory-build-your-first-pipeline-using-arm.md)voor meer informatie. 

### <a name="using-rest-api"></a>REST API gebruiken
U kunt maken en implementeren van pijpleiding REST API's te gebruiken. Dit mechanisme kan worden gebruikt voor pijpleidingen maken via programmacode. Zie voor meer informatie [maken of bijwerken van een pijpleiding](https://msdn.microsoft.com/library/azure/dn906741.aspx). 


## <a name="monitor-and-manage-pipelines"></a>Bewaken en beheren van pijpleidingen  
Zodra een pijpleiding is geïmplementeerd, kunt u beheren en controleren van de pijpleidingen, segmenten en wordt uitgevoerd. Lees hier meer over: [controleren en beheren van pijpleidingen](data-factory-monitor-manage-pipelines.md).


## <a name="pipeline-json"></a>Pijpleiding JSON   
Laat het ons Kijk eens op hoe een pijpleiding in JSON-indeling wordt gedefinieerd. De algemene structuur voor een pijpleiding ziet er als volgt uit:

    {
        "name": "PipelineName",
        "properties": 
        {
            "description" : "pipeline description",
            "activities":
            [
    
            ],
            "start": "<start date-time>",
            "end": "<end date-time>"
        }
    }

De sectie **activiteiten** kan een of meer activiteiten zoals gedefinieerd in het hebben. Elke activiteit heeft de volgende structuur op het hoogste niveau:

    {
        "name": "ActivityName",
        "description": "description", 
        "type": "<ActivityType>",
        "inputs":  "[]",
        "outputs":  "[]",
        "linkedServiceName": "MyLinkedService",
        "typeProperties":
        {
    
        },
        "policy":
        {
        }
        "scheduler":
        {
        }
    }

Na de tabel beschrijven de eigenschappen van de activiteit en de pijpleiding JSON-definities:

Label | Beschrijving | Vereist
--- | ----------- | --------
naam | De naam van de activiteit of de pijplijn. Geef een naam voor de actie die op dat de activiteit of de pijpleiding is geconfigureerd<br/><ul><li>Maximum aantal tekens: 260</li><li>Moet beginnen met het nummer van een letter of een onderstrepingsteken (_)</li><li>Volgende tekens zijn niet toegestaan: ".", "+", "?", "/", "<",">", "*", "%", "&", ":","\\"</li></ul> | Ja
Beschrijving | Tekst met een beschrijving van wat de activiteit of de pijpleiding wordt gebruikt voor | Ja
type | Hiermee geeft u het type van de activiteit. Zie de artikelen [Gegevensverplaatsing activiteiten](data-factory-data-movement-activities.md) en [Data Transformation activiteiten](data-factory-data-transformation-activities.md) voor verschillende soorten activiteiten. | Ja
ingangen | Gebruikt door de activiteit invoertabellen<br/><br/>een invoeren tabel<br/>"invoer": [{"name": "inputtable1"}],<br/><br/>twee invoertabellen <br/>"inputs": [{"name": "inputtable1"}, {"name": "inputtable2"}], | Ja
uitgangen | Output-tabellen die worden gebruikt door de activity.// een uitvoertabel<br/>"uitvoer": [{"name": "outputtable1"}],<br/><br/>twee tabellen<br/>"uitvoer": [{"name": "outputtable1"}, {"name": "outputtable2"}], | Ja
linkedServiceName | De naam van de gekoppelde service wordt gebruikt door de activiteit. <br/><br/>Een activiteit is mogelijk dat u de gekoppelde service die is gekoppeld aan het milieu vereiste berekenen opgeeft. | Ja voor HDInsight activiteiten en Azure Machine leren scoren activiteit Batch <br/><br/>Nee voor alle andere
typeProperties | Eigenschappen in de sectie typeProperties afhankelijk van de activiteit. | Nee
beleid | Het beleid dat van invloed zijn op de run-time gedrag van de activiteit. Als dit niet is opgegeven, wordt standaardbeleid worden gebruikt. | Nee
Start | Begindatum-tijd voor de pijplijn. Moet in de [ISO-notatie](http://en.wikipedia.org/wiki/ISO_8601). Bijvoorbeeld: 2014-10-14T16:32:41Z. <br/><br/>Het is mogelijk om op te geven van een plaatselijke tijd, bijvoorbeeld een geschatte tijd. Hier volgt een voorbeeld: ' 2016-02-27T06:00:00**-05: 00**", die is 6 AM EST.<br/><br/>De eigenschappen van de begin- en einddatum opgeven samen actieve periode voor de pijplijn. Segmenten van de uitvoer zijn in deze periode actief alleen met geproduceerd. | Nee<br/><br/>Als u een waarde voor de eigenschap end opgeeft, moet u de waarde voor de eigenschap start.<br/><br/>De begin- en eindtijden kunnen beide leeg zijn voor het maken van een pijpleiding. Geef beide waarden in te stellen een actieve periode voor de pijpleiding worden uitgevoerd. Als u geen begin- en eindtijden opgeeft bij het maken van een pijpleiding, kunt u deze later met behulp van de cmdlet Set-AzureRmDataFactoryPipelineActivePeriod instellen.
einde | Datum-eindtijd voor de pijplijn. Als in de ISO-notatie moet worden opgegeven. Bijvoorbeeld: 2014-10-14T17:32:41Z <br/><br/>Het is mogelijk om op te geven van een plaatselijke tijd, bijvoorbeeld een geschatte tijd. Hier volgt een voorbeeld: ' 2016-02-27T06:00:00**-05: 00**", die is 6 AM EST.<br/><br/>Geef de pijplijn voor onbepaalde tijd uitvoeren, 9999-09-09 als de waarde voor de eigenschap end. | Nee <br/><br/>Als u een waarde voor de eigenschap start opgeeft, moet u de waarde voor de eigenschap end.<br/><br/>Zie Opmerkingen voor de eigenschap **start** .
isPaused | Als is ingesteld op true, de pijpleiding wordt niet uitgevoerd. Standaardwaarde = false. U kunt deze eigenschap in- of uitschakelen. | Nee 
Scheduler | "planner" eigenschap wordt gebruikt voor het definiëren van de gewenste planning voor de activiteit. Bijbehorende subeigenschappen zijn dezelfde als die in de [beschikbaarheid van de eigenschap in een dataset](data-factory-create-datasets.md#Availability). | Nee |   
| pipelineMode | De methode voor het plannen van de pijpleiding wordt uitgevoerd. Toegestane waarden zijn: (standaard), geplande eenmalige.<br/><br/>'Gepland' geeft aan dat de pijpleiding wordt uitgevoerd op een opgegeven tijdsinterval op basis van de actieve periode (begin- en tijd). 'Eenmalige' geeft aan dat de pijpleiding wordt slechts eenmaal uitgevoerd. Eenmalige pijpleidingen eenmaal gemaakt kunnen niet worden gewijzigd/bijgewerkt op dit moment. Zie [Onetime pipeline](data-factory-scheduling-and-execution.md#onetime-pipeline) voor meer informatie over het instellen van eenmalige. | Nee | 
| expirationTime | De duur van tijd na het maken van dat de pijpleiding is geldig en moet worden ingericht. Als er geen eventuele actieve mislukt, of wordt uitgevoerd, in afwachting van de pijpleiding wordt automatisch verwijderd wanneer de verlooptijd is bereikt. | Nee | 
| DataSets | Lijst van datasets zijn voor activiteiten zoals gedefinieerd in de pijplijn. Deze eigenschap kan worden gebruikt voor het definiëren van datasets die specifiek zijn voor deze pijpleiding en niet gedefinieerd in de fabriek van gegevens. DataSets gedefinieerd binnen deze pijpleiding kan alleen worden gebruikt door deze pijpleiding en kan niet worden gedeeld. [Binnen bereik van datasets](data-factory-create-datasets.md#scoped-datasets) Zie voor meer informatie.| Nee |  
 

### <a name="policies"></a>Beleid
Beleid van invloed zijn op de run-time gedrag van een activiteit, met name wanneer het segment van een tabel wordt verwerkt. De volgende tabel vindt u de details.

Eigenschap | Toegestane waarden | Standaardwaarde | Beschrijving
-------- | ----------- | -------------- | ---------------
gelijktijdigheid | Geheel getal <br/><br/>De maximale waarde: 10 | 1 | Het aantal gelijktijdige uitvoering van de activiteit.<br/><br/>Bepaalt het aantal parallelle activiteit uitvoeringen die op verschillende segmenten gebeuren kan. Bijvoorbeeld als een activiteit moet doorlopen versnelt een grote verzameling van beschikbare gegevens, met een grotere waarde voor gelijktijdigheid de verwerking van gegevens. 
executionPriorityOrder | NewestFirst<br/><br/>OldestFirst | OldestFirst | Hiermee bepaalt u de volgorde van de segmenten voor gegevens die worden verwerkt.<br/><br/>Als er 2 segmenten (één gebeurt om 16: 00 en een andere om 5 uur) en zijn in afwachting van uitvoering. Als u de executionPriorityOrder te NewestFirst zijn ingesteld, wordt het segment om 17.00 eerst verwerkt. Op dezelfde manier als u de executionPriorityORder te OldestFIrst zijn ingesteld, is worden het segment om 4 uur verwerkt. 
Probeer het opnieuw | Geheel getal<br/><br/>De maximale waarde is 10 | 3 | Het aantal pogingen voordat de verwerking van gegevens voor het segment wordt gemarkeerd als een fout. Uitvoering van de activiteit voor een segment is maximaal het aantal opgegeven opnieuw geprobeerd. De poging is gedaan zo spoedig mogelijk na de storing.
time-out | TimeSpan | 00:00:00 | Time-out voor de activiteit. Bijvoorbeeld: 00:10:00 (impliceert timeout 10 minuten)<br/><br/>Als een waarde niet is opgegeven of is ingesteld op 0, wordt de time-out is oneindig.<br/><br/>Als de verwerkingstijd van een segment de time-outwaarde overschrijdt, deze is geannuleerd en wordt geprobeerd het systeem opnieuw de verwerking uit te voeren. Het aantal nieuwe pogingen is afhankelijk van de eigenschap opnieuw. Wanneer een time-out optreedt, wordt de status ingesteld op time-out.
vertraging | TimeSpan | 00:00:00 | Geef de vertraging voordat de verwerking van het segment wordt gestart.<br/><br/>De uitvoering van de activiteit voor een segment wordt gestart nadat u de vertraging is voorbij de verwachte tijd voor uitvoering.<br/><br/>Bijvoorbeeld: 00:10:00 (impliceert vertraging van 10 minuten)
longRetry | Geheel getal<br/><br/>De maximale waarde: 10 | 1 | Het aantal herhalingspogingen lang voordat de uitvoering van het segment is mislukt.<br/><br/>longRetry pogingen worden verdeeld door longRetryInterval. Als u een tijd tussen nieuwe pogingen opgeven, dus longRetry gebruiken. Als zowel opnieuw proberen en longRetry zijn opgegeven, elke poging longRetry omvat nieuwe pogingen en het maximum aantal pogingen is opnieuw * longRetry.<br/><br/>Als bijvoorbeeld hebben we de volgende instellingen in het beleid van de activiteit:<br/>Probeer het opnieuw: 3<br/>longRetry: 2<br/>longRetryInterval: 01:00:00<br/><br/>Er wordt slechts één segment uit te voeren (status Waiting) en de uitvoering van de activiteit niet elke keer. In eerste instantie zou er 3 uitvoering opeenvolgende pogingen. Na elke poging is de status van het segment opnieuw. Nadat de eerste 3 pogingen zijn, zou de status segment LongRetry zijn.<br/><br/>Na een uur (dat wil zeggen de waarde van longRetryInteval), zou er een andere set van 3 uitvoering opeenvolgende pogingen. De status van het segment zou worden niet, en geen pogingen meer zou worden uitgevoerd. Dus zijn over het algemeen 6 pogingen aangebracht.<br/><br/>Als de uitvoering is voltooid, zou de status van het segment klaar is en geen pogingen meer worden geprobeerd.<br/><br/>longRetry kunnen worden gebruikt in situaties waarbij afhankelijke gegevens bij niet-deterministisch tijden aankomen of het algemene klimaat flaky onder welke verwerking plaatsvindt. In dergelijke gevallen pogingen achter elkaar niet kunt doen, en dit na een interval van tijd, leidt tot de gewenste uitvoer.<br/><br/>Waarschuwing: geen hoge waarden voor longRetry of longRetryInterval instelt. Hogere waarden impliceren gewoonlijk andere systemische problemen. 
longRetryInterval | TimeSpan | 00:00:00 | De vertraging tussen de lange nieuwe pogingen 


## <a name="next-steps"></a>Volgende stappen

- Inzicht in [de planning en uitvoering in Azure Data Factory](data-factory-scheduling-and-execution.md).  
- Meer informatie over het [verplaatsen van gegevens](data-factory-data-movement-activities.md) en [voorzieningen voor transformatie](data-factory-data-transformation-activities.md) in Azure Data Factory
- [Beheer en bewaking in Azure Data Factory](data-factory-monitor-manage-pipelines.md)begrijpen.
- [Bouw en implementeer de pipeline fist](data-factory-build-your-first-pipeline.md). 
