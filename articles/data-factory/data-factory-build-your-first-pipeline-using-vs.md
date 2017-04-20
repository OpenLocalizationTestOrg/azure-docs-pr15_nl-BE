<properties
    pageTitle="Bouwen van de eerste gegevens fabriek (Visual Studio) | Microsoft Azure"
    description="In deze zelfstudie maakt u een monster Azure Data Factory pijpleiding met behulp van Visual Studio."
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
    ms.date="10/17/2016"
    ms.author="spelluru"/>

# <a name="tutorial-build-your-azure-first-data-factory-using-microsoft-visual-studio"></a>Zelfstudie: Uw eerste Azure Build data factory met behulp van Microsoft Visual Studio
> [AZURE.SELECTOR]
- [Overzicht en vereisten](data-factory-build-your-first-pipeline.md)
- [Azure portal](data-factory-build-your-first-pipeline-using-editor.md)
- [Visual Studio](data-factory-build-your-first-pipeline-using-vs.md)
- [PowerShell](data-factory-build-your-first-pipeline-using-powershell.md)
- [Sjabloon voor bronbeheer](data-factory-build-your-first-pipeline-using-arm.md)
- [REST-API](data-factory-build-your-first-pipeline-using-rest-api.md)

In dit artikel wordt u de Microsoft Visual Studio maakt uw eerste Azure data factory genoemd.

## <a name="prerequisites"></a>Vereisten
1. Lees artikel [Overzicht zelfstudie](data-factory-build-your-first-pipeline.md) en de **vereiste** stappen.
2. U moet een **beheerder van het abonnement Azure** kunnen publiceren Data Factory entiteiten vanuit Visual Studio Azure Data Factory.
3. Hiervoor hebt u het volgende op uw computer is geïnstalleerd: 
    - Visual Studio 2013 of Visual Studio 2015
    - Azure SDK voor Visual Studio 2013 of Visual Studio 2015 downloaden. Navigeer naar de [Pagina voor het downloaden van Azure](https://azure.microsoft.com/downloads/) en klik **2013 VS** **VS 2015** in het gedeelte van **.NET** .
    - De meest recente Azure Data Factory-invoegtoepassing voor Visual Studio downloaden: [2013 VS](https://visualstudiogallery.msdn.microsoft.com/754d998c-8f92-4aa7-835b-e89c8c954aa5) of [VS 2015](https://visualstudiogallery.msdn.microsoft.com/371a4cf9-0093-40fa-b7dd-be3c74f49005). U kunt ook de invoegtoepassing bijwerken door het volgende te doen: klik op **Extra**in het menu -> **uitbreidingen en Updates** -> **Online** -> **Visual Studio-galerie** -> **Microsoft Azure Data Factory-hulpprogramma's voor Visual Studio** -> **bijwerken**. 
 
Nu we Visual Studio gebruiken voor het maken van een Azure data factory genoemd. 


## <a name="create-visual-studio-project"></a>Visual Studio-project maken 
1. Start **Visual Studio 2013** of **Visual Studio 2015**. Klik op **bestand**, wijs **Nieuw**aan en klik op **Project**. Hier ziet u het dialoogvenster **Nieuw Project** .  
2. In het dialoogvenster **Nieuw Project** de **DataFactory** -sjabloon selecteren en klik op **Lege Data Factory-Project**.   

    ![Het dialoogvenster Nieuw project](./media/data-factory-build-your-first-pipeline-using-vs/new-project-dialog.png)

3. Voer een **naam** voor het project, **locatie**en een naam voor de **oplossing**en klik op **OK**.

    ![Solution Explorer](./media/data-factory-build-your-first-pipeline-using-vs/solution-explorer.png)

## <a name="create-linked-services"></a>Gekoppelde services maken
Een data factory genoemd kan een of meer pijpleidingen hebben. Een pijpleiding kan één of meer activiteiten erin hebben. Bijvoorbeeld, een activiteit kopiëren gegevens kopiëren van een bron naar een gegevensarchief voor de bestemming en de activiteit van een component HDInsight script transformeren invoergegevens component uit te voeren. Zie [ondersteunde gegevens worden opgeslagen](data-factory-data-movement-activities.md##supported-data-stores-and-formats) voor de bronnen en putten die worden ondersteund door de activiteit van de kopie. Zie voor de lijst met compute-services worden ondersteund door Data Factory [berekenen gekoppelde services](data-factory-compute-linked-services.md) . 

In deze stap koppelt u uw account Azure opslag en een cluster op verzoek Azure HDInsight aan de fabriek van uw gegevens. De opslag van Azure-account bevat de gegevens invoer en uitvoer voor de pijplijn in dit voorbeeld. De service HDInsight gekoppeld wordt gebruikt in de activiteit van de pijpleiding die in dit voorbeeld opgegeven component-script uit te voeren. Bepalen welke gegevens winkel/compute services in uw scenario worden gebruikt en die diensten koppelen aan de fabriek van gegevens door het maken van gekoppelde services.  

U opgeven de naam en de instellingen voor de fabriek gegevens later wanneer u uw Data Factory-oplossing publiceert.

#### <a name="create-azure-storage-linked-service"></a>Azure opslag gekoppeld service maken
In deze stap kunt u uw Azure opslag account koppelen aan uw data factory genoemd. Voor deze zelfstudie kunt u de Azure opslag rekening gebruiken voor het opslaan van gegevens voor invoer/uitvoer en het scriptbestand HQL. 

4. **Gekoppelde Services** in de solution explorer met de rechtermuisknop, wijs **toevoegen**en klik op **Nieuw Item**.      
5. Selecteer **Gekoppelde Azure Storage-Service** in de lijst in het dialoogvenster **Add New Item** en klikt u op **toevoegen**. 
3. **Accountnaam** en **accountkey** vervangen door de naam van uw account Azure opslag en de sleutel. Zie informatie over het ophalen van de sleutel van de toegang tot opslag, [weergeven, kopiëren en opnieuw genereren opslag toegangstoetsen](../storage/storage-create-storage-account.md#view-copy-and-regenerate-storage-access-keys)

    ![Azure Storage Service gekoppeld](./media/data-factory-build-your-first-pipeline-using-vs/azure-storage-linked-service.png)

4. Sla het bestand **AzureStorageLinkedService1.json** .

#### <a name="create-azure-hdinsight-linked-service"></a>Azure HDInsight gekoppeld service maken
In deze stap koppelt u een cluster op vraag HDInsight aan uw data factory genoemd. Het cluster HDInsight wordt automatisch gemaakt tijdens runtime en is verwijderd nadat het is verwerkt en niet-actief voor de opgegeven tijdsduur. In plaats van een cluster op verzoek HDInsight kunt u uw eigen cluster HDInsight. Zie [Gekoppelde Services berekenen](data-factory-compute-linked-services.md) voor meer informatie. 

1. In de **Solution Explorer**met de rechtermuisknop op de **Gekoppelde Services**, wijs **toevoegen**en klik op **Nieuw Item**.
2. **HDInsight op verzoek gekoppelde Service**te selecteren en klik op **toevoegen**. 
3. De **JSON** vervangen door het volgende:

        {
          "name": "HDInsightOnDemandLinkedService",
          "properties": {
            "type": "HDInsightOnDemand",
            "typeProperties": {
              "version": "3.2",
              "clusterSize": 1,
              "timeToLive": "00:30:00",
              "linkedServiceName": "AzureStorageLinkedService1"
            }
          }
        }
    
    In de volgende tabel vindt u beschrijvingen van de JSON-eigenschappen in het fragment gebruikt:
    
    Eigenschap | Beschrijving
    -------- | -----------
    Versie | Hiermee geeft u aan dat de versie van de HDInsight 3.2 worden gemaakt. 
    ClusterSize | Hiermee geeft u de grootte van de cluster HDInsight. 
    TimeToLive | Geeft aan dat de niet-actieve tijd voor het cluster HDInsight voordat deze wordt verwijderd.
    linkedServiceName | Hiermee geeft u de account van de opslag die wordt gebruikt voor het opslaan van de logboekbestanden die worden gegenereerd door HDInsight

    Let op het volgende: 
    
    - De Data Factory gemaakt een HDInsight **Windows-gebaseerde** clusters met de voorgaande JSON. U kunt ook het maken van een HDInsight **op basis van Linux** cluster hebben. Zie [Gekoppelde HDInsight-Service op verzoek](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) voor meer informatie. 
    - In plaats van een cluster op verzoek HDInsight kunt u **uw eigen cluster HDInsight** . Zie [Gekoppelde HDInsight-Service](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) voor meer informatie.
    - Het cluster HDInsight maakt een **standaardcontainer** in de blobopslag die u hebt opgegeven in de JSON (**linkedServiceName**). HDInsight, verwijdert deze container wanneer het cluster wordt verwijderd. Dit gedrag is inherent aan het ontwerp. Op verzoek HDInsight gekoppeld service, een HDInsight cluster gemaakt telkens wanneer een segment wordt verwerkt, tenzij er een live bestaand cluster (**timeToLive**). Het cluster wordt automatisch verwijderd wanneer de verwerking is voltooid.
    
        Als u meer segmenten worden verwerkt, ziet u veel containers in Azure blobopslag. Als u niet hen hoeft voor het oplossen van de taken, kunt u deze om de kosten voor opslag te verwijderen. De namen van deze containers een patroon volgen: "adf**yourdatafactoryname**-**linkedservicename**- datetimestamp". Met hulpmiddelen zoals [Microsoft Opslagverkenner](http://storageexplorer.com/) containers in Azure blobopslag verwijderen.

    Zie [Gekoppelde HDInsight-Service op verzoek](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) voor meer informatie. 
4. Sla het bestand **HDInsightOnDemandLinkedService1.json** .

## <a name="create-datasets"></a>Gegevenssets maken
In deze stap maakt u gegevenssets te vertegenwoordigen van de invoer en uitvoer van gegevens voor verwerking van de component. Deze datasets verwijzen naar de **AzureStorageLinkedService1** die u eerder in deze handleiding hebt gemaakt. De gekoppelde service verwijst naar een Azure opslag- en datasets container, map, bestandsnaam opgeven in de opslag die in het bezit van de invoer en uitvoer van gegevens.   

#### <a name="create-input-dataset"></a>Invoer gegevensset maken

1. In de **Solution Explorer**met de rechtermuisknop op **tabellen**, wijs **toevoegen**en klik op **Nieuw Item**. 
2. **Azure Blob** selecteert in de lijst wijzigen in de naam van het bestand **InputDataSet.json**en klik op **toevoegen**.
3. De **JSON** in de editor vervangen door het volgende: 

    In het fragment JSON maakt u een dataset **AzureBlobInput** met gegevens die zijn ingevoerd voor een activiteit in de pijplijn genoemd. Bovendien geeft u dat de ingevoerde gegevens bevindt zich in de blob zogenaamd **adfgetstarted** en de map met de naam **inputdata**
        
        {
            "name": "AzureBlobInput",
            "properties": {
                "type": "AzureBlob",
                "linkedServiceName": "AzureStorageLinkedService1",
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
  	| linkedServiceName | verwijst naar de AzureStorageLinkedService1 die u eerder hebt gemaakt. |
  	| Bestandsnaam | Deze eigenschap is optioneel. Als u deze eigenschap niet opgeeft, worden alle bestanden uit de folderPath opgenomen. In dit geval wordt alleen de input.log verwerkt. |
  	| type | De logboekbestanden worden in tekstindeling, zodat we de tekstopmaak gebruiken. | 
  	| columnDelimiter | kolommen in de logboekbestanden worden gescheiden door de tekens door komma's () |
  	| frequentie-interval | frequentie ingesteld op maand en -interval is 1, wat betekent dat de invoer segmenten maandelijks beschikbaar zijn. | 
  	| externe | Deze eigenschap is ingesteld op true als de ingevoerde gegevens niet door de service Data Factory wordt gegenereerd. | 
      
    
3. Sla het bestand **InputDataset.json** . 

 
#### <a name="create-output-dataset"></a>Uitvoer-gegevensset maken
Maak nu de dataset uitvoer voor de opgeslagen uitvoergegevens in de Azure Blob-opslag. 

1. In de **Solution Explorer**met de rechtermuisknop op **tabellen**, wijs **toevoegen**en klik op **Nieuw Item**. 
2. **Azure Blob** selecteert in de lijst wijzigen in de naam van het bestand **OutputDataset.json**en klik op **toevoegen**. 
3. De **JSON** in de editor vervangen door het volgende: 

    In het fragment JSON maakt u een dataset genaamd **AzureBlobOutput**en de structuur van de gegevens die wordt geproduceerd door het onderdeel script op te geven. Bovendien is het u dat de resultaten worden opgeslagen in de blob-container **adfgetstarted** genoemd en de map met de naam **partitioneddata**. De sectie **beschikbaarheid** geeft aan dat de uitvoer dataset is geproduceerd op een maandelijkse basis.
    
        {
          "name": "AzureBlobOutput",
          "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService1",
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

4. Sla het bestand **OutputDataset.json** .


### <a name="create-pipeline"></a>Pipeline maken
In deze stap maakt u uw eerste pijpleiding met een **HDInsightHive** activiteit. De invoer slice maandelijks beschikbaar is (frequentie: maand, interval: 1), output segment maandelijks wordt geproduceerd en de eigenschap planner voor de activiteit wordt ook ingesteld op maandelijks. De instellingen voor de gegevensset uitvoer en de scheduler activiteit moeten overeenkomen. Uitvoer dataset is momenteel wat de planning, stations, dus u een output-gegevensset maken moet zelfs als de activiteit niet er geen uitvoer geproduceerd. Als de activiteit niet elke input, kunt u het maken van de dataset invoer overslaan. De eigenschappen die worden gebruikt in de volgende JSON worden aan het einde van deze sectie beschreven.

1. In de **Solution Explorer**met de rechtermuisknop op de **pijpleidingen**, wijs **toevoegen**en klik op **Nieuw Item.** 
2. **Transformatie pijplijn component** selecteert in de lijst en klik op **toevoegen**. 
3. De **JSON** vervangen door het volgende fragment.

    > [AZURE.IMPORTANT] **storageaccountname** vervangen door de naam van de account van uw opslag.

        {
            "name": "MyFirstPipeline",
            "properties": {
                "description": "My first Azure Data Factory pipeline",
                "activities": [
                    {
                        "type": "HDInsightHive",
                        "typeProperties": {
                            "scriptPath": "adfgetstarted/script/partitionweblogs.hql",
                            "scriptLinkedService": "AzureStorageLinkedService1",
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
    
    In het fragment JSON maakt u een pijpleiding die bestaat uit één activiteit die gebruikmaakt van component te verwerken gegevens op een cluster van HDInsight.
    
    Het scriptbestand component, **partitionweblogs.hql**, wordt opgeslagen in de Azure opslag account (zoals opgegeven door de scriptLinkedService, **AzureStorageLinkedService1**genoemd) en in de map **scripts** in de container **adfgetstarted**.

    De sectie **definieert** wordt gebruikt om de runtime instellingen die worden doorgegeven aan het script component als component configuratiewaarden (bijvoorbeeld ${hiveconf: inputtable}, {hiveconf:partitionedtable} $).

    De eigenschappen van het **begin** en **einde** van de pijpleiding geeft de actieve periode van de pijpleiding.

    In de activiteit JSON, kunt u opgeven dat het onderdeel script wordt uitgevoerd op de compute opgegeven door de **linkedServiceName** - **HDInsightOnDemandLinkedService**.

    > [AZURE.NOTE] Zie [de anatomie van een pijpleiding](data-factory-create-pipelines.md#anatomy-of-a-pipeline) voor informatie over de JSON-eigenschappen die in het voorbeeld worden gebruikt. 
3. Sla het bestand **HiveActivity1.json** .

### <a name="add-partitionweblogshql-and-inputlog-as-a-dependency"></a>Partitionweblogs.hql en input.log als een afhankelijkheid toevoegen 

1. **Afhankelijkheden** in het venster **Solution Explorer** met de rechtermuisknop, wijs **toevoegen**en op **Bestaande Item**.  
2. Ga naar de **C:\ADFGettingStarted** en **partitionweblogs.hql**, **input.log** -bestanden selecteren en klikt u op **toevoegen**. U hebt deze twee bestanden als onderdeel van de voorwaarden uit het [Overzicht zelfstudie](data-factory-build-your-first-pipeline.md).

Wanneer u de oplossing in de volgende stap, wordt het bestand **partitionweblogs.hql** geüpload naar de scriptmap in de container **adfgetstarted** blob.   

### <a name="publishdeploy-data-factory-entities"></a>Data Factory entiteiten publiceren/implementeren

18. Project in de Solution Explorer met de rechtermuisknop en klik op **publiceren**. 
19. Als u het dialoogvenster **aanmelden bij uw Microsoft-account** , Geef uw referenties voor de account met Azure abonnement en klik op **aanmelden**.
20. U ziet het volgende dialoogvenster:

    ![Publiceren, dialoogvenster](./media/data-factory-build-your-first-pipeline-using-vs/publish.png)

21. Op de Configure data factory-pagina het volgende doen: 
    1. Selecteer de optie **Nieuwe Data Factory maken** .
    2. Voer een unieke **naam** voor de fabriek van gegevens. Bijvoorbeeld: **FirstDataFactoryUsingVS09152016**. De naam moet uniek zijn.  
    
    
        > [AZURE.IMPORTANT] Als het foutbericht **Data factory naam 'FirstDataFactoryUsingVS' is niet beschikbaar** wanneer u publiceert, wijzigt u de naam (bijvoorbeeld yournameFirstDataFactoryUsingVS). Zie [Data Factory - regels voor naamgeving van](data-factory-naming-rules.md) onderwerp voor naamgevingsregels voor Data Factory artefacten.
3. Selecteer het juiste abonnement voor het veld **abonnement** .
     
     
        > [AZURE.IMPORTANT] Als u een abonnement niet ziet, ervoor te zorgen dat u aangemeld met een account die door een beheerder of een co-beheerder van het abonnement.  
        
    4. Selecteer de **resourcegroep** voor de fabriek van de gegevens moet worden gemaakt. 
    5. Selecteer de **regio** voor de fabriek van gegevens. 
    6. Klik op **volgende** om te schakelen naar de pagina **Items publiceren** . (Druk op **TAB** om uit het veld naam als de knop **volgende** uitgeschakeld.) 
23. Op de pagina **Items publiceren** zorgen ervoor dat alle gegevens-fabrieken entiteiten zijn geselecteerd en klik op **volgende** om te schakelen naar de pagina **Overzicht** .     
24. Bekijk het overzicht en klik op **volgende** om het implementatieproces te starten en de **Implementatiestatus**weergeven.
25. **Implementatiestatus** op de pagina ziet u de status van het implementatieproces. Klik op Voltooien nadat de installatie is voltooid. 

 
Belangrijke punten om te onthouden: 

- Als het foutbericht: '**dit abonnement is niet geregistreerd voor het gebruik van de naamruimte Microsoft.DataFactory**', een van de volgende handelingen uit en probeer opnieuw te publiceren: 

    - In Azure PowerShell de volgende opdracht voor het registreren van de provider Data Factory. 
        
            Register-AzureRmResourceProvider -ProviderNamespace Microsoft.DataFactory
    
        De Data Factory provider is geregistreerd, kunt u de volgende opdracht om te controleren of uitvoeren. 
    
            Get-AzureRmResourceProvider
    - Aanmelden met de Azure abonnement in de [portal Azure](https://portal.azure.com) en navigeer naar een Data Factory blade (of) een fabriek data in Azure portal maken. Met deze actie de provider voor u automatisch wordt geregistreerd.
-   De naam van de fabriek gegevens kan worden geregistreerd als een DNS-naam in de toekomst en dus zichtbaar onlinebronnen.
-   Data Factory om instanties te maken, moet u een beheerder of een co-beheerder van het abonnement Azure

 
## <a name="monitor-pipeline"></a>Monitor-pipeline

### <a name="monitor-pipeline-using-diagram-view"></a>Monitor pijpleiding met behulp van de weergave Netwerkdiagram
6. Log in op de [Azure portal](https://portal.azure.com/), doet u het volgende:
    1. **Meer services** en klik op **gegevens fabrieken**.
        ![Fabrieken gegevens bladeren](./media/data-factory-build-your-first-pipeline-using-vs/browse-datafactories.png) 
    2. Selecteer de naam van de fabriek van gegevens (bijvoorbeeld: **FirstDataFactoryUsingVS09152016**) uit de lijst met gegevens fabrieken. 
        ![Selecteer uw data factory](./media/data-factory-build-your-first-pipeline-using-vs/select-first-data-factory.png)
7. Op de introductiepagina voor de fabriek van gegevens, klikt u op het **Diagram**.
  
    ![Diagram naast elkaar](./media/data-factory-build-your-first-pipeline-using-vs/diagram-tile.png)
7. In de weergave Netwerkdiagram ziet u een overzicht van de pijpleidingen en datasets die in deze zelfstudie wordt gebruikt.
    
    ![Diagram weergeven](./media/data-factory-build-your-first-pipeline-using-vs/diagram-view-2.png) 
8. Alle activiteiten in de pijplijn, met de rechtermuisknop op de pijpleiding die in het diagram en klik op Open pijpleiding. 

    ![Menu openen pijpleiding](./media/data-factory-build-your-first-pipeline-using-vs/open-pipeline-menu.png)
9. Bevestig dat u ziet dat de HDInsightHive activiteit in de pijplijn. 
  
    ![Open pijpleiding weergeven](./media/data-factory-build-your-first-pipeline-using-vs/open-pipeline-view.png)

    Als u wilt navigeren naar de vorige weergave, klikt u op **Data factory genoemd** in de ' breadcrumb ' op het menu boven. 
10. Dubbelklik op de dataset **AzureBlobInput**in de **Weergave Netwerkdiagram**. Controleer of het segment in de status **Gereed** is. Het duurt enkele minuten voor het segment in de status gereed weergegeven. Als deze niet voordoet nadat u ergens wacht, zien als u het invoerbestand (input.log) geplaatst in de juiste container (adfgetstarted) en de map (inputdata).

    ![Invoer segment in de status gereed](./media/data-factory-build-your-first-pipeline-using-vs/input-slice-ready.png)
11. Klik op **X** om te sluiten **AzureBlobInput** blade. 
12. Dubbelklik op de dataset **AzureBlobOutput**in de **Weergave Netwerkdiagram**. U ziet dat het segment dat momenteel wordt verwerkt.

    ![DataSet](./media/data-factory-build-your-first-pipeline-using-vs/dataset-blade.png)
9. Wanneer de verwerking is voltooid, ziet u het segment in de status **Gereed** .

    > [AZURE.IMPORTANT] Maken van een cluster op verzoek HDInsight duurt meestal ergens (ongeveer 20 minuten). Daarom verwachten de pijpleiding om **ongeveer 30 minuten** voor het verwerken van het segment.  

    ![DataSet](./media/data-factory-build-your-first-pipeline-using-vs/dataset-slice-ready.png) 
    
10. Als het segment **gereed is** , controleert u de map **partitioneddata** in de container **adfgetstarted** in de blob-opslag voor de uitvoergegevens.  
 
    ![uitvoergegevens](./media/data-factory-build-your-first-pipeline-using-vs/three-ouptut-files.png)
11. Klik op het segment details over het in een **segment** blade.

    ![Gegevens segment details](./media/data-factory-build-your-first-pipeline-using-vs/data-slice-details.png)  
12. Klik op een activiteit uitvoeren in de **lijst met activiteiten uitgevoerd** details over een activiteit (Component activiteit in ons scenario) uitgevoerd in een venster **uitgevoerd details activiteit** .   
    ![Activiteit uitvoeren details](./media/data-factory-build-your-first-pipeline-using-vs/activity-window-blade.png)  
    
    Uit de logboekbestanden kunt u de component query die is uitgevoerd en de statusinformatie bekijken. Deze logboeken zijn handig voor het oplossen van problemen.  
 

Zie [Monitor datasets en pipeline](data-factory-monitor-manage-pipelines.md) voor instructies over hoe de Azure portal gebruiken om te controleren van de pijpleiding en datasets die u hebt gemaakt in deze zelfstudie.

### <a name="monitor-pipeline-using-monitor--manage-app"></a>Monitor Monitor & App beheren via pijpleiding
Beeldscherm & toepassing voor het controleren van de pijpleidingen te beheren. Zie voor gedetailleerde informatie over het gebruik van deze toepassing [controleren en beheren van Azure Data Factory pijpleidingen met Monitoring en beheer App](data-factory-monitor-manage-app.md).

1. Klik op Monitor & tegel beheren.

    ![Bewaken en beheren van tegel](./media/data-factory-build-your-first-pipeline-using-vs/monitor-and-manage-tile.png) 
2. U moet controleren Zie & toepassing te beheren. Wijzig de **Begintijd** en **eindtijd** aan start (01-04-2016 12:00 uur)- en eindtijden (04-02-2016 12:00 uur) van de pijpleiding, en klik op **toepassen**.

    ![Bewaken en beheren van App](./media/data-factory-build-your-first-pipeline-using-vs/monitor-and-manage-app.png) 
3. Een venster van de activiteit in de lijst met activiteiten Windows voor informatie over het selecteren. 
    ![Venster details van activiteit](./media/data-factory-build-your-first-pipeline-using-vs/activity-window-details.png)


> [AZURE.IMPORTANT] Het invoerbestand wordt verwijderd wanneer het segment wordt verwerkt. Dus als u wilt dat het segment opnieuw uitvoeren of de zelfstudie opnieuw doen, upload het invoerbestand (input.log) naar de map inputdata van de container adfgetstarted.
 

## <a name="use-server-explorer-to-view-data-factories"></a>Server Explorer gebruiken om weer te geven gegevens fabrieken

1. In **Visual Studio**, klikt u op **weergave** in het menu en klik op **Server Explorer**.
2. In het venster Server Explorer **Azure** vouwen en vouw **Data Factory**. Als u **zich aanmelden bij Visual Studio**, voer is de **account** die is gekoppeld aan uw abonnement Azure en klik op **Doorgaan**. Voer **wachtwoord in**en klik op **aanmelden**. Visual Studio zoekt naar informatie over alle gegevens Azure fabrieken in uw abonnement. U ziet dat de status van deze bewerking in het venster **Taakoverzicht van Data Factory** .

    ![Server Explorer](./media/data-factory-build-your-first-pipeline-using-vs/server-explorer.png)
3. U kunt met de rechtermuisknop op een fabriek gegevens en **Data Factory exporteren naar een nieuw Project** maken op basis van een bestaande data factory een Visual Studio-project selecteren.

    ![Export data factory](./media/data-factory-build-your-first-pipeline-using-vs/export-data-factory-menu.png)

## <a name="update-data-factory-tools-for-visual-studio"></a>Extra Data Factory-update voor Visual Studio

Als u wilt bijwerken Azure Data Factory tools voor Visual Studio, het volgende doen:

1. Klik op **Extra** in het menu en selecteer **uitbreidingen en Updates**.
2. Selecteer de **Updates die** in het linkerdeelvenster en selecteer vervolgens de **Visual Studio-galerie**.
3. Selecteer **Extra Azure Data Factory voor Visual Studio** en klik op **bijwerken**. Als u deze vermelding niet ziet, hebt u al de meest recente versie van de hulpprogramma's. 

## <a name="use-configuration-files"></a>Configuratiebestanden gebruiken
Eigenschappen van gekoppelde services/tabellen/pijpleidingen voor elke omgeving anders configureren kunt u de configuratiebestanden in Visual Studio. 

U kunt de volgende JSON-definitie van een service gekoppeld Azure-opslag. **ConnectionString** opgeven met verschillende waarden voor accountnaam en accountkey op basis van de omgeving (Test-Dev/productie) waarop u Data Factory entiteiten implementeert. Dit probleem kunt u bereiken met behulp van afzonderlijke configuratiebestand voor elke omgeving. 

    {
        "name": "StorageLinkedService",
        "properties": {
            "type": "AzureStorage",
            "description": "",
            "typeProperties": {
                "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
            }
        }
    } 

### <a name="add-a-configuration-file"></a>Een configuratiebestand toevoegen
Een configuratiebestand voor elke omgeving toevoegen door de volgende stappen uit te voeren:   

1. Klik met de rechtermuisknop op de Data Factory-project in de Visual Studio-oplossing **toevoegen**en klik op **Nieuw item**.
2. **Config** selecteren uit de lijst met geïnstalleerde sjablonen aan de linkerkant, selecteer **Configuratiebestand**en voer een **naam** voor het configuratiebestand klikt u op **toevoegen**.

    ![Configuratiebestand toevoegen](./media/data-factory-build-your-first-pipeline-using-vs/add-config-file.png)
3. Parameters voor configuratie en de bijbehorende waarden in de volgende indeling toevoegen.

        {
            "$schema": "http://datafactories.schema.management.azure.com/vsschemas/V1/Microsoft.DataFactory.Config.json",
            "AzureStorageLinkedService1": [
                {
                    "name": "$.properties.typeProperties.connectionString",
                    "value": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
                }
            ],
            "AzureSqlLinkedService1": [
                {
                    "name": "$.properties.typeProperties.connectionString",
                    "value":  "Server=tcp:spsqlserver.database.windows.net,1433;Database=spsqldb;User ID=spelluru;Password=Sowmya123;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
                }
            ]
        }

    In dit voorbeeld wordt de connectionString-eigenschap van een service gekoppeld Azure-opslag en een gekoppelde Azure SQL service geconfigureerd. U ziet dat de syntaxis voor het opgeven van de naam [JsonPath](http://goessner.net/articles/JsonPath/).   

    Als JSON een eigenschap heeft heeft die een matrix met waarden zoals in de volgende code:  

        "structure": [
            {
                "name": "FirstName",
                "type": "String"
            },
            {
                "name": "LastName",
                "type": "String"
            }
        ],
    
    Configureer de eigenschappen zoals in het volgende configuratiebestand (gebruik op nul gebaseerde indexering): 
        
        {
            "name": "$.properties.structure[0].name",
            "value": "FirstName"
        }
        {
            "name": "$.properties.structure[0].type",
            "value": "String"
        }
        {
            "name": "$.properties.structure[1].name",
            "value": "LastName"
        }
        {
            "name": "$.properties.structure[1].type",
            "value": "String"
        }

### <a name="property-names-with-spaces"></a>Namen met spaties
Als een naam spaties bevat, gebruikt u vierkante haken zoals in het volgende voorbeeld (Database servernaam): 

     {
         "name": "$.properties.activities[1].typeProperties.webServiceParameters.['Database server name']",
         "value": "MyAsqlServer.database.windows.net"
     }


### <a name="deploy-solution-using-a-configuration"></a>Een configuratie met oplossing implementeren
Wanneer u Azure Data Factory-entiteiten in de VS publiceert, kunt u de configuratie die u wilt gebruiken voor de bewerking publiceren. 

Entiteiten in een configuratiebestand Azure Data Factory-project publiceren:   

1. Data Factory-project met de rechtermuisknop en klik op **publiceren** om het dialoogvenster **Items publiceren** . 
2. Selecteer een bestaande data factory of geef waarden op voor het maken van een fabriek voor de gegevens op de pagina **data factory genoemd configureren** en op **volgende**.   
3. Op de pagina **Items publiceren** : ziet u een vervolgkeuzelijst met beschikbare configuraties voor het veld **Selecteren implementatie Config** .

    ![Config-bestand selecteren](./media/data-factory-build-your-first-pipeline-using-vs/select-config-file.png)

4. Selecteer het **bestand** dat u wilt gebruiken en klik op **volgende**. 
5. Bevestig dat u de naam van de JSON-bestand op de pagina **Samenvatting ziet** en klik op **volgende**. 
6. Klik op **Voltooien** nadat de bewerking voor de implementatie is voltooid. 

Wanneer u implementeert, worden de waarden uit het configuratiebestand gebruikt voor het instellen van waarden voor eigenschappen in JSON bestanden voor Data Factory entiteiten voordat de entiteiten worden geïmplementeerd op Azure Data Factory service.   

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
