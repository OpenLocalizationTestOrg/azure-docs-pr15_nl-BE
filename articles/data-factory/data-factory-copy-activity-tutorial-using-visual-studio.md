<properties 
    pageTitle="Zelfstudie: Maak een pijpleiding met kopie activiteit met behulp van Visual Studio | Microsoft Azure" 
    description="In deze zelfstudie maakt u een pijpleiding Azure Data Factory met een activiteit kopiëren met behulp van Visual Studio." 
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
    ms.topic="get-started-article" 
    ms.date="10/17/2016" 
    ms.author="spelluru"/>

# <a name="tutorial-create-a-pipeline-with-copy-activity-using-visual-studio"></a>Zelfstudie: Een pijpleiding aan met behulp van Visual Studio kopiëren-activiteit maken
> [AZURE.SELECTOR]
- [Overzicht en vereisten](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
- [Wizard kopiëren](data-factory-copy-data-wizard-tutorial.md)
- [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md)
- [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)
- [PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)
- [Sjabloon voor Azure Resource Manager](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
- [REST-API](data-factory-copy-activity-tutorial-using-rest-api.md)
- [.NET API](data-factory-copy-activity-tutorial-using-dotnet-api.md)


In deze zelfstudie wordt beschreven hoe u maken en controleren van de fabriek van een Azure gegevens met behulp van Visual Studio. Een activiteit kopiëren de pijpleiding die in de fabriek gegevens gebruikt om gegevens te kopiëren van Azure Blob-opslag met Azure SQL-Database.

Hier zijn de stappen die u als onderdeel van deze zelfstudie uitvoeren:

1. Maak twee gekoppelde services: **AzureStorageLinkedService1** en **AzureSqlinkedService1**. 

    De AzureStorageLinkedService1 koppelt een Azure opslag en AzureSqlLinkedService1 koppelingen Azure SQL-database naar de fabriek gegevens: **ADFTutorialDataFactoryVS**. De ingevoerde gegevens voor de pijplijn bevindt zich in een blob-container in de Azure blobopslag en uitvoergegevens worden opgeslagen in een tabel in de database SQL Azure. Daarom toevoegen u deze twee winkels als gekoppelde services gegevens fabriek.
2. Twee gegevenssets maken: **InputDataset** en **OutputDataset**, die de i/o-gegevens die zijn opgeslagen in opgeslagen gegevens. 

    Voor de InputDataset geeft u de container blob met een blob met de brongegevens. Voor de OutputDataset geeft u de SQL-tabel waarin de uitvoergegevens. U ook opgeven andere eigenschappen zoals structuur, beschikbaarheid en beleid.
3. Maak een pijpleiding met de naam **ADFTutorialPipeline** in de ADFTutorialDataFactoryVS. 

    De pijplijn is een **Kopie activiteit** input gegevens vanaf de Azure BLOB aan de uitvoer Azure SQL-tabel. De activiteit kopie uitvoert verplaatsing van gegevens in Azure Data Factory. De activiteit wordt aangedreven door een wereldwijd beschikbare service waarmee gegevens tussen verschillende opgeslagen gegevens op een veilige, schaalbare en betrouwbare manier kunt kopiëren. Zie [Gegevensverplaatsing activiteiten](data-factory-data-movement-activities.md) -artikel voor meer informatie over de activiteit van de kopie. 
4. Maak een data factory met de naam **VSTutorialFactory**. De fabriek van gegevens en alle Data Factory entiteiten (gekoppelde services, tabellen en de pijpleiding) implementeren.    

## <a name="prerequisites"></a>Vereisten

1. Lees artikel [Overzicht zelfstudie](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) en de **vereiste** stappen. 
2. U moet een **beheerder van het abonnement Azure** kunnen Data Factory entiteiten publiceren naar Azure Data Factory.  
3. Hiervoor hebt u het volgende op uw computer is geïnstalleerd: 
    - Visual Studio 2013 of Visual Studio 2015
    - Azure SDK voor Visual Studio 2013 of Visual Studio 2015 downloaden. Navigeer naar de [Pagina voor het downloaden van Azure](https://azure.microsoft.com/downloads/) en klik **2013 VS** **VS 2015** in het gedeelte van **.NET** .
    - De meest recente Azure Data Factory-invoegtoepassing voor Visual Studio downloaden: [2013 VS](https://visualstudiogallery.msdn.microsoft.com/754d998c-8f92-4aa7-835b-e89c8c954aa5) of [VS 2015](https://visualstudiogallery.msdn.microsoft.com/371a4cf9-0093-40fa-b7dd-be3c74f49005). U kunt ook de invoegtoepassing bijwerken door de volgende stappen uit te voeren: klik op **Extra**in het menu -> **uitbreidingen en Updates** -> **Online** -> **Visual Studio-galerie** -> **Microsoft Azure Data Factory-hulpprogramma's voor Visual Studio** -> **bijwerken**.

## <a name="create-visual-studio-project"></a>Visual Studio-project maken 
1. Start **Visual Studio 2013**. Klik op **bestand**, wijs **Nieuw**aan en klik op **Project**. Hier ziet u het dialoogvenster **Nieuw Project** .  
2. In het dialoogvenster **Nieuw Project** de **DataFactory** -sjabloon selecteren en klik op **Lege Data Factory-Project**. Als u de sjabloon DataFactory niet ziet, sluit Visual Studio, Azure SDK installeren voor Visual Studio 2013, en open Visual Studio.  

    ![Het dialoogvenster Nieuw project](./media/data-factory-copy-activity-tutorial-using-visual-studio/new-project-dialog.png)

3. Voer een **naam** voor het project, **locatie**en een naam voor de **oplossing**en klik op **OK**.

    ![Solution Explorer](./media/data-factory-copy-activity-tutorial-using-visual-studio/solution-explorer.png) 

## <a name="create-linked-services"></a>Gekoppelde services maken
Gekoppelde services opgeslagen gegevens koppelen of diensten aan een fabriek Azure gegevens berekenen. Zie [ondersteunde gegevens worden opgeslagen](data-factory-data-movement-activities.md##supported-data-stores-and-formats) voor de bronnen en putten die worden ondersteund door de activiteit van de kopie. Zie voor de lijst met compute-services worden ondersteund door Data Factory [berekenen gekoppelde services](data-factory-compute-linked-services.md) . In deze zelfstudie, u niet elke compute-service gebruikt. 

In deze stap maakt u twee gekoppelde services: **AzureStorageLinkedService1** en **AzureSqlLinkedService1**. AzureStorageLinkedService1 gekoppeld koppelingen service een Account Azure opslag en AzureSqlLinkedService Azure SQL-database koppelingen naar de fabriek gegevens: **ADFTutorialDataFactory**. 

### <a name="create-the-azure-storage-linked-service"></a>De gekoppelde Azure-Storage-service maken

4. **Gekoppelde Services** in de solution explorer met de rechtermuisknop, wijs **toevoegen**en klik op **Nieuw Item**.      
5. Selecteer **Gekoppelde Azure Storage-Service** in de lijst in het dialoogvenster **Add New Item** en klikt u op **toevoegen**. 

    ![Nieuwe gekoppelde Service](./media/data-factory-copy-activity-tutorial-using-visual-studio/new-linked-service-dialog.png)
 
3. Vervangen `<accountname>` en `<accountkey>`* met de naam van uw account Azure opslag en de sleutel. 

    ![Azure Storage Service gekoppeld](./media/data-factory-copy-activity-tutorial-using-visual-studio/azure-storage-linked-service.png)

4. Sla het bestand **AzureStorageLinkedService1.json** .

> Zie [gegevens van/naar Azure Blob verplaatsen](data-factory-azure-blob-connector.md#azure-storage-linked-service) voor meer informatie over eigenschappen van JSON.

### <a name="create-the-azure-sql-linked-service"></a>De gekoppelde Azure SQL service maken

5. Klik met de rechtermuisknop op het knooppunt in de **Solution Explorer** met **Gekoppelde Services** opnieuw en klik op **Nieuw Item** **toevoegen**. 
6. Deze keer **Azure SQL gekoppelde Service**selecteren en klik op **toevoegen**. 
7. Vervang in het **bestand AzureSqlLinkedService1.json**, `<servername>`, `<databasename>`, `<username@servername>`, en `<password>` met de namen van uw Azure SQL server, database-gebruikersaccount en wachtwoord.    
8.  Sla het bestand **AzureSqlLinkedService1.json** . 

> [AZURE.NOTE]
> Zie [gegevens van/naar Azure SQL-Database verplaatsen](data-factory-azure-sql-connector.md#azure-sql-linked-service-properties) voor meer informatie over eigenschappen van JSON.

## <a name="create-datasets"></a>Gegevenssets maken
In de vorige stap u gekoppelde services, **AzureStorageLinkedService1** en **AzureSqlLinkedService1** een Azure opslag account en Azure SQL-database koppelen aan de fabriek van de gegevens gemaakt: **ADFTutorialDataFactory**. In deze stap definieert u twee datasets-- **InputDataset** en **OutputDataset** --die de i/o-gegevens die zijn opgeslagen in respectievelijk AzureStorageLinkedService1 en AzureSqlLinkedService1 genoemde opgeslagen gegevens vertegenwoordigen. Voor InputDataset geeft u de container blob met een blob met de brongegevens. Voor OutputDataset geeft u de SQL-tabel waarin de uitvoergegevens.

### <a name="create-input-dataset"></a>Invoer gegevensset maken
In deze stap maakt u een dataset met de naam **InputDataset** die naar een blob-container in de Azure opslag die wordt vertegenwoordigd door de service **AzureStorageLinkedService1** gekoppeld wijst. Een tabel is een rechthoekig dataset en het enige type dataset nu ondersteund. 

9. **Tabellen** in de **Solution Explorer**met de rechtermuisknop, wijs **toevoegen**en klik op **Nieuw Item**.
10. **Azure Blob**selecteren in het dialoogvenster **Add New Item** en klik op **toevoegen**.   
10. De JSON-tekst vervangen door de volgende tekst en sla het bestand **AzureBlobLocation1.json** . 

        {
          "name": "InputDataset",
          "properties": {
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
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService1",
            "typeProperties": {
              "folderPath": "adftutorial/",
              "format": {
                "type": "TextFormat",
                "columnDelimiter": ","
              }
            },
            "external": true,
            "availability": {
              "frequency": "Hour",
              "interval": 1
            }
          }
        }

     Houd rekening met de volgende punten: 
    
    - DataSet- **type** is ingesteld op **AzureBlob**.
    - **linkedServiceName** is ingesteld op **AzureStorageLinkedService**. U kunt deze service gekoppelde gemaakt in stap 2.
    - **mappad** is ingesteld op de container **adftutorial** . Ook kunt u de naam van een blob in de map met behulp van de eigenschap **fileName** . Nadat u de naam van de blob niet opgeeft, worden gegevens van alle BLOB's in de container wordt beschouwd als een ingevoerde gegevens.  
    - notatie **type** is ingesteld op **tekstopmaak**
    - Er zijn twee velden in het tekstbestand: **Voornaam** en **Achternaam** , gescheiden door een komma-teken (**columnDelimiter**) 
    - De **beschikbaarheid** is ingesteld op **elk uur** (**frequentie** is ingesteld op **uren** en **interval** is ingesteld op **1**). Daarom Data Factory gezocht voor invoergegevens elk uur in de hoofdmap van de blob container (**adftutorial**) dat u hebt opgegeven. 
    
    Als u een **bestandsnaam** voor een dataset **invoer** niet opgeeft, worden alle bestanden/BLOB's uit de map met invoer (**mappad**) beschouwd als invoer. Als u een bestandsnaam in de JSON opgeeft, alleen het opgegeven bestand/blob asn invoer beschouwd.
 
    Als u een **bestandsnaam** voor een **uitvoertabel**niet opgeeft, wordt de gegenereerde bestanden in het **mappad** heten in de volgende indeling: gegevens. &lt;Guid\&gt;. txt (voorbeeld: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt.).

    **Mappad** en de **bestandsnaam** dynamisch op basis van de **SliceStart** -tijd instellen, gebruikt u de eigenschap **partitionedBy** . In het volgende voorbeeld folderPath maakt gebruik van jaar, maand en dag van de SliceStart (begintijd van het segment wordt verwerkt) en bestandsnaam gebruikt uur vanaf de SliceStart. Bijvoorbeeld, als een segment wordt geproduceerd voor 2016-09-20T08:00:00, de mapnaam wordt ingesteld op wikidatagateway/wikisampledataout/2016/09/20 en de bestandsnaam ingesteld op 08.csv. 

            "folderPath": "wikidatagateway/wikisampledataout/{Year}/{Month}/{Day}",
            "fileName": "{Hour}.csv",
            "partitionedBy": 
            [
                { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
                { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } }, 
                { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } }, 
                { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "hh" } } 

> [AZURE.NOTE]
> Zie [gegevens van/naar Azure Blob verplaatsen](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties) voor meer informatie over eigenschappen van JSON.

### <a name="create-output-dataset"></a>Uitvoer-gegevensset maken
In deze stap maakt u een dataset uitvoer is met de naam **OutputDataset**. Deze dataset verwijst naar een SQL-tabel in de Azure SQL-database die wordt vertegenwoordigd door **AzureSqlLinkedService1**. 

11. Opnieuw met de rechtermuisknop op de **tabellen** in de **Solution Explorer** en klik op **Nieuw Item** **toevoegen**.
12. **Azure SQL**selecteert in het dialoogvenster **Add New Item** en klik op **toevoegen**. 
13. De JSON-tekst vervangen door de volgende JSON en sla het bestand **AzureSqlTableLocation1.json** .

        {
          "name": "OutputDataset",
          "properties": {
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
            "type": "AzureSqlTable",
            "linkedServiceName": "AzureSqlLinkedService1",
            "typeProperties": {
              "tableName": "emp"
            },
            "availability": {
              "frequency": "Hour",
              "interval": 1
            }
          }
        }

     Houd rekening met de volgende punten: 
    
    - DataSet- **type** is ingesteld op **AzureSQLTable**.
    - **linkedServiceName** is ingesteld op **AzureSqlLinkedService** (deze service gekoppelde in stap 2 gemaakte).
    - **emp**is **TableName** ingesteld.
    - Er zijn drie kolommen: **ID**, **Voornaam**en **Achternaam** – in de emp-tabel in de database. -ID is een id-kolom, zodat u moet hier alleen de **Voornaam** en **Achternaam** opgeven.
    - De **beschikbaarheid** is ingesteld op **elk uur** (**frequentie** ingesteld op **uur** en **interval** is ingesteld op **1**).  De service Data Factory genereert een segment output elk uur in de tabel **emp** in de Azure SQL-database.

> [AZURE.NOTE]
> Zie [gegevens van/naar Azure SQL-Database verplaatsen](data-factory-azure-sql-connector.md#azure-sql-linked-service-properties) voor meer informatie over eigenschappen van JSON.

## <a name="create-pipeline"></a>Pipeline maken 
U hebt gekoppelde services invoer/uitvoer en tabellen tot nu toe gemaakt. Nu maakt u een pijpleiding een **Activiteit kopie** kopiëren van gegevens uit de Azure blob met Azure SQL-database. 


1. **Pijpleidingen** in de **Solution Explorer**met de rechtermuisknop, wijs **toevoegen**en klik op **Nieuw Item**.  
15. **Copy Data pijpleiding** selecteert in het dialoogvenster **Add New Item** en klikt u op **toevoegen**. 
16. De JSON vervangen door de volgende JSON en sla het bestand **CopyActivity1.json** .
            
        {
          "name": "ADFTutorialPipeline",
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
                  "style": "StartOfInterval",
                  "retry": 0,
                  "timeout": "01:00:00"
                }
              }
            ],
            "start": "2015-07-12T00:00:00Z",
            "end": "2015-07-13T00:00:00Z",
            "isPaused": false
          }
        }

    Houd rekening met de volgende punten:

    - In de sectie activiteiten is er slechts één activiteit waarvan het **type** is ingesteld op **kopiëren**.
    - Input voor de activiteit is ingesteld op **InputDataset** en uitvoer voor de activiteit is ingesteld op **OutputDataset**.
    - **BlobSource** is opgegeven als het bronsoort en **SqlSink** als het type sink is opgegeven in de sectie **typeProperties** .

    Vervang de waarde van de eigenschap **start** met de huidige waarde voor de **dag- en** met de volgende dag. U kunt alleen het datumgedeelte opgeven en het deel van de tijd van de tijd datum overslaan. Bijvoorbeeld: '2016-02-03', die gelijk is aan ' 2016-02-03T00:00:00Z "
    
    Beide start en eind datum/tijd in de [ISO-notatie](http://en.wikipedia.org/wiki/ISO_8601)moet zijn. Bijvoorbeeld: 2016-10-14T16:32:41Z. **De eindtijd** is optioneel, maar we gebruiken in deze zelfstudie. 
    
    Als u geen waarde voor de eigenschap **end** opgeeft, wordt deze berekend als '**start + 48 uur**'. Geef de pijplijn voor onbepaalde tijd uitvoeren, **9999-09-09** als de waarde voor de eigenschap **end** .
    
    In het vorige voorbeeld zijn er 24 gegevens segmenten elk segment per uur wordt geproduceerd.

## <a name="publishdeploy-data-factory-entities"></a>Data Factory entiteiten publiceren/implementeren
In deze stap maakt u Data Factory entiteiten (gekoppelde services, datasets en pijpleiding) publiceert u eerder hebt gemaakt. U wordt ook de naam van de nieuwe fabriek van gegevens moet worden gemaakt voor het opslaan van deze entiteiten.  

18. Project in de Solution Explorer met de rechtermuisknop en klik op **publiceren**. 
19. Als u het dialoogvenster **aanmelden bij uw Microsoft-account** , Geef uw referenties voor de account met Azure abonnement en klik op **aanmelden**.
20. U ziet het volgende dialoogvenster:

    ![Publiceren, dialoogvenster](./media/data-factory-copy-activity-tutorial-using-visual-studio/publish.png)
21. Voer de volgende stappen uit in de Configure data factory-pagina: 
    1. Selecteer de optie **Nieuwe Data Factory maken** .
    2. **VSTutorialFactory** voor een **naam**invoeren.  
    
        > [AZURE.IMPORTANT]  
        > De naam van de fabriek Azure gegevens moet uniek zijn. Als er een over de naam van de fabriek van gegevens foutbericht wanneer u publiceert, wijzigt u de naam van de fabriek van gegevens (bijvoorbeeld yournameVSTutorialFactory) en probeer het opnieuw publiceren. Zie [Data Factory - regels voor naamgeving van](data-factory-naming-rules.md) onderwerp voor naamgevingsregels voor Data Factory artefacten.     
    3. Selecteer uw Azure abonnement voor het veld **abonnement** .
     
        > [AZURE.IMPORTANT]Als u een abonnement niet ziet, ervoor te zorgen dat u aangemeld met een account die door een beheerder of een co-beheerder van het abonnement.  
    4. Selecteer de **resourcegroep** voor de fabriek van de gegevens moet worden gemaakt. 5. Selecteer de **regio** voor de fabriek van gegevens. Regio's ondersteund door de service Data Factory worden weergegeven in de vervolgkeuzelijst.
6. Klik op **volgende** om te schakelen naar de pagina **Items publiceren** .
    
        ![Data factory-pagina configureren](media/data-factory-copy-activity-tutorial-using-visual-studio/configure-data-factory-page.png)   
23. Op de pagina **Items publiceren** zorgen ervoor dat alle gegevens-fabrieken entiteiten zijn geselecteerd en klik op **volgende** om te schakelen naar de pagina **Overzicht** .
    
    ![Pagina-items publiceren](media/data-factory-copy-activity-tutorial-using-visual-studio/publish-items-page.png)     
24. Bekijk het overzicht en klik op **volgende** om het implementatieproces te starten en de **Implementatiestatus**weergeven.

    ![Overzicht pagina publiceren](media/data-factory-copy-activity-tutorial-using-visual-studio/publish-summary-page.png)
25. **Implementatiestatus** op de pagina ziet u de status van het implementatieproces. Klik op Voltooien nadat de installatie is voltooid. 
    ![De pagina status implementatie](media/data-factory-copy-activity-tutorial-using-visual-studio/deployment-status.png) Let op de volgende punten: 

- Als het foutbericht: '**dit abonnement is niet geregistreerd voor het gebruik van de naamruimte Microsoft.DataFactory**', een van de volgende handelingen uit en probeer opnieuw te publiceren: 

    - In Azure PowerShell de volgende opdracht voor het registreren van de provider Data Factory. 
        
            Register-AzureRmResourceProvider -ProviderNamespace Microsoft.DataFactory
    
        De Data Factory provider is geregistreerd, kunt u de volgende opdracht om te controleren of uitvoeren. 
    
            Get-AzureRmResourceProvider
    - Aanmelden met de Azure abonnement in de [portal Azure](https://portal.azure.com) en navigeer naar een Data Factory blade (of) een fabriek data in Azure portal maken. Met deze actie de provider voor u automatisch wordt geregistreerd.
-   De naam van de fabriek gegevens kan worden geregistreerd als een DNS-naam in de toekomst en dus zichtbaar onlinebronnen.

> [AZURE.IMPORTANT] Data Factory om instanties te maken, moet u een beheerder/co-beheerder van het abonnement Azure

## <a name="summary"></a>Samenvatting
In deze zelfstudie, die u hebt gemaakt een fabriek Azure gegevens kopiëren naar een database SQL Azure blob-gegevens uit een Azure. U gebruikt Visual Studio voor het maken van de data factory, gekoppelde services datasets en een pijpleiding. Hier worden de hoofdstappen beschreven die u in deze zelfstudie hebt uitgevoerd:  

1.  Een Azure **data factory**gemaakt.
2.  **Gekoppelde services**gemaakt:
    1. Een **Opslag Azure** gekoppeld service uw Azure opslag account waarin de ingevoerde gegevens te koppelen.    
    2. Een gekoppelde **SQL Azure** service voor het koppelen van uw Azure SQL-database waarin de gegevens over de output. 
3.  **Datasets**, wordt gemaakt waarin invoergegevens en uitvoergegevens van pijpleidingen.
4.  Een **pijpleiding** met een **Kopie activiteit** met **BlobSource** als de bron- en **SqlSink** als sink gemaakt. 


## <a name="use-server-explorer-to-view-data-factories"></a>Server Explorer gebruiken om weer te geven gegevens fabrieken

1. In **Visual Studio**, klikt u op **weergave** in het menu en klik op **Server Explorer**.
2. In het venster Server Explorer **Azure** vouwen en vouw **Data Factory**. Als u **zich aanmelden bij Visual Studio**, voer is de **account** die is gekoppeld aan uw abonnement Azure en klik op **Doorgaan**. Voer **wachtwoord in**en klik op **aanmelden**. Visual Studio zoekt naar informatie over alle gegevens Azure fabrieken in uw abonnement. U ziet dat de status van deze bewerking in het venster **Taakoverzicht van Data Factory** .
    ![Server Explorer](./media/data-factory-copy-activity-tutorial-using-visual-studio/server-explorer.png)
3. U kunt met de rechtermuisknop op een fabriek gegevens en Data Factory exporteren naar een nieuw Project maken op basis van een bestaande data factory een Visual Studio-project selecteren.
    ![Fabriek gegevens exporteren naar een project van de VS](./media/data-factory-copy-activity-tutorial-using-visual-studio/export-data-factory-menu.png)  

## <a name="update-data-factory-tools-for-visual-studio"></a>Extra Data Factory-update voor Visual Studio
Als u wilt bijwerken Azure Data Factory tools voor Visual Studio, kunt u de volgende stappen uitvoeren:

1. Klik op **Extra** in het menu en selecteer **uitbreidingen en Updates**. 
2. Selecteer de **Updates die** in het linkerdeelvenster en selecteer vervolgens de **Visual Studio-galerie**.
4. Selecteer **Extra Azure Data Factory voor Visual Studio** en klik op **bijwerken**. Als u deze vermelding niet ziet, hebt u al de meest recente versie van de hulpprogramma's. 

Zie [Monitor datasets en pipeline](data-factory-copy-activity-tutorial-using-azure-portal.md#monitor-pipeline) voor instructies over hoe de Azure portal gebruiken om te controleren van de pijpleiding en datasets die u hebt gemaakt in deze zelfstudie.

## <a name="see-also"></a>Zie ook
| Onderwerp | Beschrijving |
| :---- | :---- |
| [Data verkeer activiteiten](data-factory-data-movement-activities.md) | Dit artikel bevat gedetailleerde informatie over de kopie activiteit die u hebt gebruikt in de zelfstudie. |
| [Planning en uitvoering](data-factory-scheduling-and-execution.md) | In dit artikel wordt uitgelegd dat de planning en uitvoering van aspecten van Azure Data Factory toepassingsmodel. |
| [Pijpleidingen](data-factory-create-pipelines.md) | Dit artikel helpt u bij het begrijpen van pijpleidingen en activiteiten in Azure Data Factory |
| [DataSets](data-factory-create-datasets.md) | Dit artikel helpt u bij het begrijpen van datasets in Azure Data Factory.
| [Bewaken en beheren van Monitoring App via pijpleidingen](data-factory-monitor-manage-app.md) | In dit artikel wordt beschreven hoe bewaken, beheren en foutopsporing met de Monitoring en beheer App pijpleidingen. 
