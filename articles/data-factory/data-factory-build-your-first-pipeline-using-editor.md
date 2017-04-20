<properties
    pageTitle="Bouwen van de eerste data factory (Azure portal) | Microsoft Azure"
    description="In deze zelfstudie maakt u een monster Azure Data Factory pijpleiding met Data Factory-Editor in de portal Azure."
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
    ms.topic="hero-article" 
    ms.date="09/14/2016"
    ms.author="spelluru"/>

# <a name="tutorial-build-your-first-azure-data-factory-using-azure-portal"></a>Zelfstudie: Uw eerste Azure data factory met Azure portal maken
> [AZURE.SELECTOR]
- [Overzicht en vereisten](data-factory-build-your-first-pipeline.md)
- [Azure portal](data-factory-build-your-first-pipeline-using-editor.md)
- [Visual Studio](data-factory-build-your-first-pipeline-using-vs.md)
- [PowerShell](data-factory-build-your-first-pipeline-using-powershell.md)
- [Sjabloon voor bronbeheer](data-factory-build-your-first-pipeline-using-arm.md)
- [REST-API](data-factory-build-your-first-pipeline-using-rest-api.md)

In dit artikel leert u hoe de [Azure portal](https://portal.azure.com/) gebruiken voor het maken van uw eerste Azure data factory genoemd. 

## <a name="prerequisites"></a>Vereisten        
1. Lees artikel [Overzicht zelfstudie](data-factory-build-your-first-pipeline.md) en de **vereiste** stappen.
2. Dit artikel biedt een overzicht van de Azure Data Factory-service niet. Het is raadzaam u doorlopen [Inleiding tot Azure Data Factory](data-factory-introduction.md) -artikel voor een gedetailleerd overzicht van de service.  

## <a name="create-data-factory"></a>Data factory maken
Een data factory genoemd kan een of meer pijpleidingen hebben. Een pijpleiding kan één of meer activiteiten erin hebben. Bijvoorbeeld, uitvoeren een kopie om gegevens te kopiëren van een bron naar een bestemming gegevensopslag en een HDInsight component activiteit component script transformeren invoergegevens product uit te voeren gegevens. Laten we beginnen met het maken van de fabriek van de gegevens in deze stap. 

1.  Log in op de [Azure portal](https://portal.azure.com/).
2.  Klik op **Nieuw** in het linkermenu, klikt u op **gegevens + Analytics**en **Data Factory**op.
        
    ![Blade maken](./media/data-factory-build-your-first-pipeline-using-editor/create-blade.png)

2.  Voer **GetStartedDF** voor de naam in de **nieuwe data factory** -blade.

    ![Nieuwe data factory blade](./media/data-factory-build-your-first-pipeline-using-editor/new-data-factory-blade.png)

    > [AZURE.IMPORTANT] 
    > De naam van de fabriek Azure gegevens moet **uniek**zijn. Als de fout: **naam van de Data factory "GetStartedDF" is niet beschikbaar**. Wijzig de naam van de fabriek van gegevens (bijvoorbeeld yournameGetStartedDF) en probeer het opnieuw maken. Zie [Data Factory - regels voor naamgeving van](data-factory-naming-rules.md) onderwerp voor naamgevingsregels voor Data Factory artefacten.
    > 
    > De naam van de fabriek gegevens kan worden geregistreerd als een **DNS-** naam in de toekomst en dus zichtbaar onlinebronnen.

3.  Selecteer het **abonnement Azure** waar u de gegevens fabriek worden gemaakt. 
4.  Selecteer bestaande **bronnengroep** of een resourcegroep te maken. Maak een groep met de naam voor de zelfstudie: **ADFGetStartedRG**. 
5.  Klik op **maken** op de **nieuwe data factory** -blade.

    > [AZURE.IMPORTANT] Data Factory om instanties te maken, moet u lid zijn van de rol van [Inzender fabriek](../active-directory/role-based-access-built-in-roles.md/#data-factory-contributor) op groepsniveau abonnement of een resource. 
6.  Ziet u de data factory wordt gemaakt in de **Startboard** van de portal voor Azure als volgt:   

    ![Status van de fabriek gegevens maken](./media/data-factory-build-your-first-pipeline-using-editor/creating-data-factory-image.png)
7. Gefeliciteerd! De eerste fabriek van gegevens hebt gemaakt. Als de data factory is gemaakt, ziet u de data factory-pagina waarin u de inhoud van de data factory genoemd.   

    ![Data Factory-blade](./media/data-factory-build-your-first-pipeline-using-editor/data-factory-blade.png)

Voordat u een pijpleiding in de fabriek van gegevens maakt, moet u eerst een paar Data Factory-entiteiten maken. U maakt eerst een gekoppelde services winkels/berekent de gegevens naar uw winkel gegevens koppelen, definiëren de input en output datasets invoer/uitvoer-gegevens in gekoppelde gegevens winkels vertegenwoordigen en maak de pijpleiding aan een activiteit die gebruikmaakt van deze datasets. 

## <a name="create-linked-services"></a>Gekoppelde services maken
In deze stap koppelt u uw account Azure opslag en een cluster op verzoek Azure HDInsight aan de fabriek van uw gegevens. De opslag van Azure-account bevat de gegevens invoer en uitvoer voor de pijplijn in dit voorbeeld. De service HDInsight gekoppeld wordt gebruikt in de activiteit van de pijpleiding die in dit voorbeeld opgegeven component-script uit te voeren. Bepalen welke [gegevens opslaan](data-factory-data-movement-activities.md)/[berekenen services](data-factory-compute-linked-services.md) in uw scenario worden gebruikt en die diensten koppelen aan de fabriek van gegevens door het maken van gekoppelde services.  

### <a name="create-azure-storage-linked-service"></a>Azure opslag gekoppeld service maken
In deze stap kunt u uw Azure opslag account koppelen aan uw data factory genoemd. In deze zelfstudie kunt u de Azure opslag rekening gebruiken voor het opslaan van gegevens voor invoer/uitvoer en het scriptbestand HQL. 

1.  Klik op **auteur en implementeren van** op de **DATA FACTORY** -blade voor **GetStartedDF**. Hier ziet u de Data Factory-Editor. 
     
    ![Ontwerpen en implementeren van tegel](./media/data-factory-build-your-first-pipeline-using-editor/data-factory-author-deploy.png)
2.  Klik op **nieuwe gegevens opslaan** en kies **Azure opslag**.

    ![Nieuwe gegevens worden opgeslagen - Storage Azure - menu](./media/data-factory-build-your-first-pipeline-using-editor/new-data-store-azure-storage-menu.png)

3.  U ziet de JSON-script voor het maken van een service gekoppeld Azure-opslag in de editor. 
    
    ![Azure gekoppeld opslagservice](./media/data-factory-build-your-first-pipeline-using-editor/azure-storage-linked-service.png)
     
4. **De naam** vervangen door de naam van uw account Azure opslag en **sleutel rekening** met de toegangssleutel van de account Azure opslag. Zie informatie over het ophalen van de sleutel van de toegang tot opslag, [weergeven, kopiëren en opnieuw genereren opslag toegangstoetsen](../storage/storage-create-storage-account.md#view-copy-and-regenerate-storage-access-keys)
5. Klik op **Deploy** op de opdrachtbalk aan de gekoppelde service implementeren.

    ![Knop implementeren](./media/data-factory-build-your-first-pipeline-using-editor/deploy-button.png)

   Nadat de gekoppelde service geïmplementeerd, verdwijnt het venster **concept-1** en **AzureStorageLinkedService** weergegeven in de boomstructuur aan de linkerkant. 
    ![Gekoppelde Storage-Service in het menu](./media/data-factory-build-your-first-pipeline-using-editor/StorageLinkedServiceInTree.png)   

 
### <a name="create-azure-hdinsight-linked-service"></a>Azure HDInsight gekoppeld service maken
In deze stap koppelt u een cluster op vraag HDInsight aan uw data factory genoemd. Het cluster HDInsight wordt automatisch gemaakt tijdens runtime en is verwijderd nadat het is verwerkt en niet-actief voor de opgegeven tijdsduur. 

1. Klik in de **Editor voor Data Factory**, **... Meer**en klik op **Nieuw compute** **cluster On-demand HDInsight**selecteren.

    ![Nieuwe berekenen](./media/data-factory-build-your-first-pipeline-using-editor/new-compute-menu.png)
2. Kopieer en plak het volgende stukje naar het venster **concept-1** . Het fragment JSON beschrijft de eigenschappen die worden gebruikt voor het maken van de HDInsight cluster op aanvraag. 

        {
          "name": "HDInsightOnDemandLinkedService",
          "properties": {
            "type": "HDInsightOnDemand",
            "typeProperties": {
              "version": "3.2",
              "clusterSize": 1,
              "timeToLive": "00:30:00",
              "linkedServiceName": "AzureStorageLinkedService"
            }
          }
        }
    
    In de volgende tabel vindt u beschrijvingen van de JSON-eigenschappen in het fragment gebruikt:
    
  	| Eigenschap | Beschrijving |
  	| :------- | :---------- |
  	| Versie | Hiermee geeft u aan dat de versie van de HDInsight 3.2 worden gemaakt. | 
  	| ClusterSize | Hiermee geeft u de grootte van de cluster HDInsight. | 
  	| TimeToLive | Geeft aan dat de niet-actieve tijd voor het cluster HDInsight voordat deze wordt verwijderd. |
  	| linkedServiceName | Hiermee geeft u de opslag-account die wordt gebruikt voor het opslaan van de logboekbestanden die worden gegenereerd door HDInsight. |

    Houd rekening met de volgende punten: 
    
    - De Data Factory gemaakt een HDInsight **Windows-gebaseerde** clusters met de JSON. U kunt ook het maken van een HDInsight **op basis van Linux** cluster hebben. Zie [Gekoppelde HDInsight-Service op verzoek](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) voor meer informatie. 
    - In plaats van een cluster op verzoek HDInsight kunt u **uw eigen cluster HDInsight** . Zie [Gekoppelde HDInsight-Service](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) voor meer informatie.
    - Het cluster HDInsight maakt een **standaardcontainer** in de blobopslag die u hebt opgegeven in de JSON (**linkedServiceName**). HDInsight, verwijdert deze container wanneer het cluster wordt verwijderd. Dit gedrag is inherent aan het ontwerp. Op verzoek HDInsight gekoppeld service, een HDInsight cluster gemaakt telkens wanneer een segment wordt verwerkt, tenzij er een live bestaand cluster (**timeToLive**). Het cluster wordt automatisch verwijderd wanneer de verwerking is voltooid.
    
        Als u meer segmenten worden verwerkt, ziet u veel containers in Azure blobopslag. Als u niet hen hoeft voor het oplossen van de taken, kunt u deze om de kosten voor opslag te verwijderen. De namen van deze containers een patroon volgen: "adf**yourdatafactoryname**-**linkedservicename**- datetimestamp". Met hulpmiddelen zoals [Microsoft Opslagverkenner](http://storageexplorer.com/) containers in Azure blobopslag verwijderen.

    Zie [Gekoppelde HDInsight-Service op verzoek](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) voor meer informatie.
3. Klik op **Deploy** op de opdrachtbalk aan de gekoppelde service implementeren. 

    ![Op verzoek HDInsight gekoppeld service implementeren](./media/data-factory-build-your-first-pipeline-using-editor/ondemand-hdinsight-deploy.png)

4. Bevestig dat u zowel **AzureStorageLinkedService** als **HDInsightOnDemandLinkedService** in de structuur weergeven aan de linkerkant zien.

    ![Boomstructuur met gekoppelde services](./media/data-factory-build-your-first-pipeline-using-editor/tree-view-linked-services.png)

## <a name="create-datasets"></a>Gegevenssets maken
In deze stap maakt u gegevenssets te vertegenwoordigen van de invoer en uitvoer van gegevens voor verwerking van de component. Deze datasets verwijzen naar de **AzureStorageLinkedService** die u eerder in deze handleiding hebt gemaakt. De gekoppelde service verwijst naar een Azure opslag- en datasets container, map, bestandsnaam opgeven in de opslag die in het bezit van de invoer en uitvoer van gegevens.   

### <a name="create-input-dataset"></a>Invoer gegevensset maken

1. Klik in de **Editor voor Data Factory**, **... Meer** op de opdrachtbalk en klik op **nieuwe gegevensset**en selecteer **Azure Blob-opslag**.

    ![Nieuwe dataset](./media/data-factory-build-your-first-pipeline-using-editor/new-data-set.png)
2. Kopieer en plak het volgende stukje naar het venster concept-1. In het fragment JSON maakt u een dataset **AzureBlobInput** met gegevens die zijn ingevoerd voor een activiteit in de pijplijn genoemd. U ook opgeven dat de ingevoerde gegevens bevindt zich in de blob zogenaamd **adfgetstarted** en de map met de naam **inputdata**.
        
        {
            "name": "AzureBlobInput",
            "properties": {
                "type": "AzureBlob",
                "linkedServiceName": "AzureStorageLinkedService",
                "typeProperties": {
                    "fileName": "input.log",
                    "folderPath": "adfgetstarted/inputdata",
                    "format": {
                        "type": "TextFormat",
                        "columnDelimiter": ","
                    }
                },
                "availability": {
                    "frequency": "Month",
                    "interval": 1
                },
                "external": true,
                "policy": {}
            }
        } 

    In de volgende tabel vindt u beschrijvingen van de JSON-eigenschappen in het fragment gebruikt:

  	| Eigenschap | Beschrijving |
  	| :------- | :---------- |
  	| type | De eigenschap type is ingesteld op AzureBlob omdat de gegevens bevinden zich in Azure blob-opslag. |  
  	| linkedServiceName | verwijst naar de AzureStorageLinkedService die u eerder hebt gemaakt. |
  	| Bestandsnaam | Deze eigenschap is optioneel. Als u deze eigenschap niet opgeeft, worden alle bestanden uit de folderPath opgenomen. In dit geval wordt alleen de input.log verwerkt. |
  	| type | De logboekbestanden worden in tekstindeling, zodat we de tekstopmaak gebruiken. | 
  	| columnDelimiter | kolommen in de logboekbestanden worden gescheiden door de tekens door komma's () |
  	| frequentie-interval | frequentie ingesteld op maand en -interval is 1, wat betekent dat de invoer segmenten maandelijks beschikbaar zijn. | 
  	| externe | Deze eigenschap is ingesteld op true als de ingevoerde gegevens niet door de service Data Factory wordt gegenereerd. | 
        
3. Klik op **Deploy** op de werkbalk voor de implementatie van de nieuwe gegevensset. Hier ziet u de dataset in de boomstructuur aan de linkerkant. 


### <a name="create-output-dataset"></a>Uitvoer-gegevensset maken
Maak nu de dataset uitvoer voor de opgeslagen uitvoergegevens in de Azure Blob-opslag. 

1. Klik in de **Editor voor Data Factory**, **... Meer** op de opdrachtbalk en klik op **nieuwe gegevensset**en selecteer **Azure Blob-opslag**.  
2. Kopieer en plak het volgende stukje naar het venster concept-1. In het fragment JSON maakt u een dataset genaamd **AzureBlobOutput**en de structuur van de gegevens die wordt geproduceerd door het onderdeel script op te geven. Bovendien is het u dat de resultaten worden opgeslagen in de blob-container **adfgetstarted** genoemd en de map met de naam **partitioneddata**. De sectie **beschikbaarheid** geeft aan dat de uitvoer dataset is geproduceerd op een maandelijkse basis.
    
        {
          "name": "AzureBlobOutput",
          "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
              "folderPath": "adfgetstarted/partitioneddata",
              "format": {
                "type": "TextFormat",
                "columnDelimiter": ","
              }
            },
            "availability": {
              "frequency": "Month",
              "interval": 1
            }
          }
        }

    Zie sectie **invoer dataset maken** voor een beschrijving van deze eigenschappen. U instelt niet de externe eigenschap op een dataset uitvoer zoals de dataset wordt geproduceerd door de service Data Factory.
3. Klik op **Deploy** op de werkbalk voor de implementatie van de nieuwe gegevensset.
4. Controleer of de dataset is gemaakt.

    ![Boomstructuur met gekoppelde services](./media/data-factory-build-your-first-pipeline-using-editor/tree-view-data-set.png)

## <a name="create-pipeline"></a>Pipeline maken
In deze stap maakt u uw eerste pijpleiding met een **HDInsightHive** activiteit. Invoer slice maandelijks beschikbaar is (frequentie: maand, interval: 1), output segment maandelijks wordt geproduceerd en de eigenschap planner voor de activiteit wordt ook ingesteld op maandelijks. De instellingen voor de gegevensset uitvoer en de scheduler activiteit moeten overeenkomen. Uitvoer dataset is momenteel wat de planning, stations, dus u een output-gegevensset maken moet zelfs als de activiteit niet er geen uitvoer geproduceerd. Als de activiteit niet elke input, kunt u het maken van de dataset invoer overslaan. De eigenschappen die worden gebruikt in de volgende JSON worden aan het einde van deze sectie beschreven. 

1. Klik in de **Editor voor Data Factory** **weglatingsteken (...) Meer opdrachten** en klik vervolgens op **nieuwe pijpleiding**.
    
    ![knop Nieuwe pijpleiding](./media/data-factory-build-your-first-pipeline-using-editor/new-pipeline-button.png)
2. Kopieer en plak het volgende stukje naar het venster concept-1.

    > [AZURE.IMPORTANT] **Storageaccountname** vervangen door de naam van uw account voor opslag in de JSON.
        
        {
            "name": "MyFirstPipeline",
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
 
    In het fragment JSON maakt u een pijpleiding die bestaat uit één activiteit die gebruikmaakt van component te verwerken gegevens op een cluster van HDInsight.
    
    Het scriptbestand component, **partitionweblogs.hql**, wordt opgeslagen in de Azure opslag account (zoals opgegeven door de scriptLinkedService, **AzureStorageLinkedService**genoemd) en in de map **scripts** in de container **adfgetstarted**.

    De sectie **definieert** wordt gebruikt om de runtime instellingen die worden doorgegeven aan het script component als component configuratiewaarden (bijvoorbeeld ${hiveconf: inputtable}, {hiveconf:partitionedtable} $).

    De eigenschappen van het **begin** en **einde** van de pijpleiding geeft de actieve periode van de pijpleiding.

    In de activiteit JSON, kunt u opgeven dat het onderdeel script wordt uitgevoerd op de compute opgegeven door de **linkedServiceName** - **HDInsightOnDemandLinkedService**.

    > [AZURE.NOTE] Zie [de anatomie van een pijpleiding](data-factory-create-pipelines.md#anatomy-of-a-pipeline) voor informatie over de JSON-eigenschappen die in het voorbeeld worden gebruikt. 

3. Controleer het volgende: 
    1. **Input.log** bestand bestaat in de map **inputdata** van de container **adfgetstarted** in de Azure blobopslag
    2. **partitionweblogs.hql** bestand bestaat in de map **scripts** van de container **adfgetstarted** in de Azure blobopslag. Volledige de vereiste stappen in de [Zelfstudie overzicht](data-factory-build-your-first-pipeline.md) als u deze bestanden niet weergegeven. 
    3. Bevestig dat u de **storageaccountname** door de naam van uw account voor opslag in de pijplijn JSON vervangen. 
2. Klik op **Deploy** op de werkbalk voor de implementatie van de pijpleiding. Sinds de ** **start** - en** eindtijden in het verleden zijn ingesteld en **isPaused** is ingesteld op false, wordt de pijpleiding (activiteit van de pijpleiding) uitgevoerd onmiddellijk nadat u hebt geïmplementeerd. 
4. Bevestig dat u ziet dat de pijpleiding in de structuurweergave.

    ![Boomstructuur met pipeline](./media/data-factory-build-your-first-pipeline-using-editor/tree-view-pipeline.png)
5. Gefeliciteerd, u hebt uw eerste pijpleiding gemaakt!

## <a name="monitor-pipeline"></a>Monitor-pipeline

### <a name="monitor-pipeline-using-diagram-view"></a>Monitor pijpleiding met behulp van de weergave Netwerkdiagram

6. **X** -blades Data Factory-Editor te sluiten en terug te keren naar het blade Data Factory, en klik op **Diagram**.
  
    ![Diagram naast elkaar](./media/data-factory-build-your-first-pipeline-using-editor/diagram-tile.png)
7. In de weergave Netwerkdiagram ziet u een overzicht van de pijpleidingen en datasets die in deze zelfstudie wordt gebruikt.
    
    ![Diagram weergeven](./media/data-factory-build-your-first-pipeline-using-editor/diagram-view-2.png) 
8. Alle activiteiten in de pijplijn, met de rechtermuisknop op de pijpleiding die in het diagram en klik op Open pijpleiding. 

    ![Menu openen pijpleiding](./media/data-factory-build-your-first-pipeline-using-editor/open-pipeline-menu.png)
9. Bevestig dat u ziet dat de HDInsightHive activiteit in de pijplijn. 
  
    ![Open pijpleiding weergeven](./media/data-factory-build-your-first-pipeline-using-editor/open-pipeline-view.png)

    Als u wilt navigeren naar de vorige weergave, klikt u op **Data factory genoemd** in de ' breadcrumb ' op het menu boven. 
10. Dubbelklik op de dataset **AzureBlobInput**in de **Weergave Netwerkdiagram**. Controleer of het segment in de status **Gereed** is. Het duurt enkele minuten voor het segment in de status gereed weergegeven. Als deze niet voordoet nadat u ergens wacht, zien als u het invoerbestand (input.log) geplaatst in de juiste container (adfgetstarted) en de map (inputdata).

    ![Invoer segment in de status gereed](./media/data-factory-build-your-first-pipeline-using-editor/input-slice-ready.png)
11. Klik op **X** om te sluiten **AzureBlobInput** blade. 
12. Dubbelklik op de dataset **AzureBlobOutput**in de **Weergave Netwerkdiagram**. U ziet dat het segment dat momenteel wordt verwerkt.

    ![DataSet](./media/data-factory-build-your-first-pipeline-using-editor/dataset-blade.png)
9. Wanneer de verwerking is voltooid, ziet u het segment in de status **Gereed** .
    
>[AZURE.IMPORTANT] Maken van een cluster op verzoek HDInsight duurt meestal ergens (ongeveer 20 minuten). Daarom verwachten de pijpleiding om **ongeveer 30 minuten** voor het verwerken van het segment.    

    ![Dataset](./media/data-factory-build-your-first-pipeline-using-editor/dataset-slice-ready.png) 
    
10. Als het segment **gereed is** , controleert u de map **partitioneddata** in de container **adfgetstarted** in de blob-opslag voor de uitvoergegevens.  
 
    ![uitvoergegevens](./media/data-factory-build-your-first-pipeline-using-editor/three-ouptut-files.png)
11. Klik op het segment details over het in een **segment** blade.

    ![Gegevens segment details](./media/data-factory-build-your-first-pipeline-using-editor/data-slice-details.png)  
12. Klik op een activiteit uitvoeren in de **lijst met activiteiten uitgevoerd** details over een activiteit (Component activiteit in ons scenario) uitgevoerd in een venster **uitgevoerd details activiteit** .   

    ![Activiteit uitvoeren details](./media/data-factory-build-your-first-pipeline-using-editor/activity-window-blade.png)  
    
    Uit de logboekbestanden kunt u de component query die is uitgevoerd en de statusinformatie bekijken. Deze logboeken zijn handig voor het oplossen van problemen.
Zie [controleren en beheren van pijpleidingen met Azure portal blades](data-factory-monitor-manage-pipelines.md) artikel voor meer informatie. 

> [AZURE.IMPORTANT] Het invoerbestand wordt verwijderd wanneer het segment wordt verwerkt. Dus als u wilt dat het segment opnieuw uitvoeren of de zelfstudie opnieuw doen, upload het invoerbestand (input.log) naar de map inputdata van de container adfgetstarted.

### <a name="monitor-pipeline-using-monitor--manage-app"></a>Monitor Monitor & App beheren via pijpleiding
Beeldscherm & toepassing voor het controleren van de pijpleidingen te beheren. Zie voor gedetailleerde informatie over het gebruik van deze toepassing [controleren en beheren van Azure Data Factory pijpleidingen met Monitoring en beheer App](data-factory-monitor-manage-app.md).

1. Klik op de tegel **Monitor & beheren** op de startpagina voor uw data factory genoemd.

    ![Bewaken en beheren van tegel](./media/data-factory-build-your-first-pipeline-using-editor/monitor-and-manage-tile.png) 
2. **Monitor & beheren toepassing**te zien. Wijzig de **Begintijd** en **eindtijd** aan start (01-04-2016 12:00 uur)- en eindtijden (04-02-2016 12:00 uur) van de pijpleiding, en klik op **toepassen**.

    ![Bewaken en beheren van App](./media/data-factory-build-your-first-pipeline-using-editor/monitor-and-manage-app.png) 
3. Een venster van de activiteit in de lijst met **Activiteiten Windows** voor informatie over het selecteren. 
    ![Venster details van activiteit](./media/data-factory-build-your-first-pipeline-using-editor/activity-window-details.png)


## <a name="summary"></a>Samenvatting 
In deze zelfstudie kunt u een Azure data factory om gegevens te verwerken met een component-script op een cluster HDInsight hadoop gemaakt. U gebruikt de Editor Data Factory in Azure portal doen de volgende stappen uit:  

1.  Een Azure **data factory**gemaakt.
2.  Twee **gekoppelde services**gemaakt:
    1.  **Azure Storage** service voor het koppelen van uw Azure blob-opslag met input/output-bestanden naar de fabriek gegevens gekoppeld.
    2.  **Azure HDInsight** service op verzoek gekoppelde een cluster op verzoek HDInsight Hadoop koppelen aan de data factory genoemd. Azure Data Factory maakt een HDInsight Hadoop cluster just-in-time om te verwerken invoergegevens en uitvoergegevens produceren. 
3.  Twee **datasets**, wordt gemaakt waarin de invoer en uitvoer van gegevens voor de component HDInsight activiteit in de pijplijn. 
4.  Een **pijpleiding** met een **HDInsight component** activiteit gemaakt. 

## <a name="next-steps"></a>Volgende stappen
In dit artikel, kunt u een pijpleiding hebt gemaakt met een transformatie activiteit (HDInsight) die een component-script wordt uitgevoerd op een cluster van de HDInsight op verzoek. Als u wilt zien hoe u een activiteit kopiëren gebruiken om gegevens te kopiëren van een Blob Azure SQL Azure, Zie [Zelfstudie: blob-gegevens kopiëren van een Azure Azure SQL](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).

## <a name="see-also"></a>Zie ook
| Onderwerp | Beschrijving |
| :---- | :---- |
| [Data Transformation activiteiten](data-factory-data-transformation-activities.md) | Dit artikel bevat een lijst met data transformation activiteiten (zoals component HDInsight-transformatie die u in deze zelfstudie wordt gebruikt) door Azure Data Factory ondersteund. | 
| [Planning en uitvoering](data-factory-scheduling-and-execution.md) | In dit artikel wordt uitgelegd dat de planning en uitvoering van aspecten van Azure Data Factory toepassingsmodel. |
| [Pijpleidingen](data-factory-create-pipelines.md) | Dit artikel helpt u bij het begrijpen van pijpleidingen en activiteiten in Azure Data Factory en het gebruik maken van end-to-end gegevensgestuurde werkstromen voor uw scenario of bedrijf. |
| [DataSets](data-factory-create-datasets.md) | Dit artikel helpt u bij het begrijpen van datasets in Azure Data Factory.
| [Bewaken en beheren van Monitoring App via pijpleidingen](data-factory-monitor-manage-app.md) | In dit artikel wordt beschreven hoe bewaken, beheren en foutopsporing met de Monitoring en beheer App pijpleidingen. 

  

