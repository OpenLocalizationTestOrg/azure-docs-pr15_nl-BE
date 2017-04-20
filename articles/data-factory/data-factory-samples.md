<properties     
    pageTitle="Azure Data Factory - voorbeelden" 
    description="Bevat details over de monsters die worden geleverd bij de service Azure Data Factory." 
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
    ms.date="10/18/2016" 
    ms.author="shlo"/>

# <a name="azure-data-factory---samples"></a>Azure Data Factory - voorbeelden

## <a name="samples-on-github"></a>Monsters op GitHub
De [opslagplaats van GitHub Azure-DataFactory](https://github.com/azure/azure-datafactory) bevat enkele voorbeelden die u helpen snel helling omhoog met Azure Data Factory service (of) de scripts wijzigen en gebruiken in eigen toepassing. De map Samples\JSON bevat fragmenten JSON voor veelvoorkomende scenario's.

| Monster | Beschrijving |
| :----- | :---------- | 
| [Scenario met automatische Documentinvoer](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/ADFWalkthrough) | In dit voorbeeld biedt een end-to-end scenario voor de verwerking van logboekbestanden Azure Data Factory met gegevens uit logboekbestanden in omzetten naar inzichten. <br/><br/>In dit scenario maakt de pijpleiding Data Factory verzamelt monster zich aanmeldt, processen verrijkt de gegevens uit de logboeken met referentiegegevens en transformeert de gegevens voor het evalueren van de effectiviteit van een campagne die onlangs is gestart. |
| [JSON-voorbeelden](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/JSON) | In dit voorbeeld ziet u voorbeelden van JSON voor veelvoorkomende scenario's. | 
| [Voorbeeld van HTTP-gegevens Downloader](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/HttpDataDownloaderSample) | In dit voorbeeld showcases downloaden van gegevens van een HTTP-eindpunt naar Azure Blob-opslag met behulp van aangepaste .NET activiteit. |
| [Cross-AppDomain Dot Net activiteit monster](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/CrossAppDomainDotNetActivitySample) | In dit voorbeeld kunt u om een aangepaste .NET activiteit die niet is beperkt tot de assembly-versies die worden gebruikt door de automatische Documentinvoer starten (bijvoorbeeld, WindowsAzure.Storage v4.3.0, Newtonsoft.Json, v6.0.x, enz.). |
| [R-script uitvoeren](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample) |  In dit voorbeeld bevat de aangepaste Data Factory-activiteit die kan worden gebruikt voor het aanroepen van RScript.exe. Dit voorbeeld werkt alleen met uw eigen HDInsight cluster (niet op aanvraag) R geïnstalleerd op al. |
| [Taken op HDInsight Hadoop cluster Spark aanroepen](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/Spark) | In dit voorbeeld ziet u hoe een vonk programma aanroepen met MapReduce activiteit. De spark NET gekopieerd gegevens uit één Azure Blob container naar de andere. |
| [Twitter-analyse met behulp van Azure Machine Learning Batch scoren-activiteit](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/TwitterAnalysisSample-AzureMLBatchScoringActivity) | In dit voorbeeld ziet u hoe u AzureMLBatchScoringActivity gebruiken om op te roepen een Azure Machine Learning model dat twitter sentiment analyse voert, scoren, voorspelling, enz. |
| [Twitter analyse met behulp van aangepaste activiteit](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/TwitterAnalysisSample-CustomC%23Activity) |  In dit voorbeeld ziet u hoe u een aangepaste activiteit met .NET gebruiken om op te roepen een Azure Machine Learning model dat twitter sentiment analyse voert, scoren, voorspelling, enz. |
| [Geparametriseerde pijpleidingen voor Azure Machine Learning](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/ParameterizedPipelinesForAzureML/) | Het monster biedt een end-to-end C#-code voor de implementatie van pijpleidingen voor scoren en bijscholing elk met een andere regio parameter waar de lijst met regio's afkomstig zijn uit een parameters.txt-bestand dat opgenomen in dit voorbeeld is N. | 
| [Verwijzing naar gegevens vernieuwen voor Azure Stream Analytics taken](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/ReferenceDataRefreshForASAJobs) |  In dit voorbeeld ziet u hoe Azure Data Factory en Azure Stream Analytics samen gebruiken om de query's uitvoeren met referentiegegevens en het vernieuwen voor een verwijzing naar gegevens op een schema instellen. |
| [Hybride pijpleiding met On-premises Hortonworks Hadoop](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/HybridPipelineWithOnPremisesHortonworksHadoop) | Dit voorbeeld worden een cluster op gebouwen Hadoop als doel berekenen voor het uitvoeren van taken in de fabriek gegevens net zoals u andere compute doelen toevoegt zou als een HDInsight op basis van Hadoop cluster in de cloud. |
| [JSON-conversieprogramma](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/JSONConversionTool) | Met dit hulpprogramma kunt u JSONs van versie vóór 2015-07-01-voorbeeld om de meest recente of 2015-07-01-voorbeeld (standaard). |  
| [Voorbeeldbestand voor U SQL-invoer](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/U-SQL%20Sample%20Input%20File) |  Dit bestand is een voorbeeld van een bestand door een activiteit U SQL gebruikt. | 

## <a name="azure-resource-manager-templates"></a>Azure Resource Manager-sjablonen
U vindt de volgende Azure Resource Manager-sjablonen voor Data Factory op Github. 

| Sjabloon | Beschrijving |
| -------- | ----------- | 
| [Kopiëren van Azure Blob-opslag met Azure SQL-Database](https://github.com/Azure/azure-quickstart-templates/tree/master/101-data-factory-blob-to-sql-copy) | Implementatie van deze sjabloon maakt u een fabriek Azure gegevens met een pijpleiding die gegevens opgehaald uit de opgegeven Azure blobopslag met de Azure SQL-database |    
| [Kopiëren van televergaderingen naar Azure Blob-opslag](https://github.com/Azure/azure-quickstart-templates/tree/master/101-data-factory-salesforce-to-blob-copy) | Implementatie van deze sjabloon maakt u een fabriek Azure gegevens met een pijpleiding die gegevens opgehaald uit de opgegeven account van de televergaderingen naar de Azure blobopslag. |    
| [Transformatie van gegevens met een component-script op een cluster Azure HDInsight](https://github.com/Azure/azure-quickstart-templates/tree/master/101-data-factory-hive-transformation) | Implementatie van deze sjabloon maakt u een fabriek Azure gegevens met een pijpleiding die gegevens worden omgezet door het voorbeeldscript component uit te voeren op een cluster Azure HDInsight Hadoop. | 


## <a name="samples-in-azure-portal"></a>Monsters in Azure portal
U kunt de **pijpleidingen monster** -tegel op de introductiepagina van uw data factory genoemd monster pijpleidingen en hun bijbehorende entiteiten (datasets en gekoppelde services) in op de fabriek gegevens implementeren. 

1. Een fabriek gegevens maken of open een bestaande data factory genoemd. Zie [aan de slag met Azure Data Factory](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md#CreateDataFactory) voor stapsgewijze instructies voor het maken van een data factory genoemd.
2. In het blad **DATA FACTORY** voor de fabriek van gegevens, klikt u op de tegel **monster pijpleidingen** .

    ![Monster pijpleidingen tegel](./media/data-factory-samples/SamplePipelinesTile.png)

2. Klik op het **voorbeeld** dat u wilt implementeren in de blade **monster pijpleidingen** . 
    
    ![Voorbeeld pijpleidingen blade](./media/data-factory-samples/SampleTile.png)

3. Geef de configuratie-instellingen voor het monster. Bijvoorbeeld uw Azure opslag account naam en account-sleutel, Azure SQL-servernaam, database, gebruikersnaam en wachtwoord, enz. 

    ![Monster blade](./media/data-factory-samples/SampleBlade.png)

4. Nadat u klaar bent met de configuratie-instellingen op te geven, klikt u op **maken** als u wilt maken/distribueren het monster pijpleidingen en gekoppelde services/tabellen die worden gebruikt door de pijpleidingen.
5. U ziet de status van de implementatie op de monster tegel die u eerder hebt geklikt op de bladeserver **monster pijpleidingen** .

    ![Implementatiestatus](./media/data-factory-samples/DeploymentStatus.png)

6. Wanneer u het bericht **installatie voltooid** op de tegel voor het monster ziet, sluit u het **monster pijpleidingen** blade.  
5. In **DATA FACTORY** blade ziet u dat de gekoppelde services, gegevenssets en pijpleidingen worden toegevoegd aan uw data factory genoemd.  

    ![Data Factory-blade](./media/data-factory-samples/DataFactoryBladeAfter.png)
   
## <a name="samples-in-visual-studio"></a>Voorbeelden in Visual Studio

### <a name="prerequisites"></a>Vereisten

Hiervoor hebt u het volgende op uw computer is geïnstalleerd: 

- Visual Studio 2013 of Visual Studio 2015
- Azure SDK voor Visual Studio 2013 of Visual Studio 2015 downloaden. Navigeer naar de [Pagina voor het downloaden van Azure](https://azure.microsoft.com/downloads/) en klik **2013 VS** **VS 2015** in het gedeelte van **.NET** .
- De meest recente Azure Data Factory-invoegtoepassing voor Visual Studio downloaden: [2013 VS](https://visualstudiogallery.msdn.microsoft.com/754d998c-8f92-4aa7-835b-e89c8c954aa5) of [VS 2015](https://visualstudiogallery.msdn.microsoft.com/371a4cf9-0093-40fa-b7dd-be3c74f49005). Als u Visual Studio 2013, kunt u de invoegtoepassing ook bijwerken door de volgende stappen uit te voeren: klik op **Extra**in het menu -> **uitbreidingen en Updates** -> **Online** -> **Visual Studio-galerie** -> **Microsoft Azure Data Factory-hulpprogramma's voor Visual Studio** -> **bijwerken**.

### <a name="use-data-factory-templates"></a>Data Factory-sjablonen gebruiken

1. Klik op **bestand** in het menu, wijs **Nieuw**aan en klik op **Project**. 
2. Voer in het dialoogvenster **Nieuw Project** de volgende stappen uit: 
    1. **DataFactory** onder **sjablonen**selecteren. 
    2. **Gegevenssjablonen fabriek** in het rechterdeelvenster selecteren. 
    3. Voer een **naam** voor het project. 
    4. Selecteer een **locatie** voor het project. 
    5. Klik op **OK**. 

    ![Het dialoogvenster Nieuw project](./media/data-factory-samples/vs-new-project-adf-templates.png)
6. Selecteer de sjabloon voorbeeldgegevens uit het gedeelte van de **Use-Case sjablonen** in het dialoogvenster **Gegevenssjablonen fabriek** en op **volgende**. De volgende stappen doorlopen die u met behulp van de sjabloon voor het **Profileren van de klant** . Stappen zijn identiek voor de overige monsters. 

    ![Het dialoogvenster sjablonen Factory](./media/data-factory-samples/vs-data-factory-templates-dialog.png) 
7. Klik op **volgende** op de pagina **Data Factory grondbeginselen** in het dialoogvenster **Configuratie van Data Factory** .
8. Voer de volgende stappen uit op de pagina **data factory genoemd configureren** : 
    1. Selecteer **nieuwe Data Factory**. Ook kunt u **het bestaande data factory gebruiken**.
    2. Voer een **naam** voor de fabriek van gegevens.
    3. Selecteer het **abonnement Azure** die u wilt de gegevens fabriek worden gemaakt. 
    4. Selecteer de **resourcegroep** voor de fabriek van gegevens.
    5. De **Amerikaanse West**, **Oost-VS**of **Noord-Europa** voor de **regio**selecteren.
    6. Klik op **volgende**. 
9. In de pagina **configureren-gegevens worden opgeslagen** , geeft u een bestaande **Azure SQL-database** en **Azure opslag account** (of) maken van database-opslag en op volgende. 
10. Selecteer standaardinstellingen op de pagina **berekenen configureren** en op **volgende**. 
11. Bekijk alle instellingen op de pagina **Samenvatting** en klikt u op **volgende**. 
12. Wacht totdat de installatie is voltooid op de pagina **Status van de implementatie** en klik op **Voltooien**.
13. Project in de Solution Explorer met de rechtermuisknop en klik op **publiceren**. 
19. Als u het dialoogvenster **aanmelden bij uw Microsoft-account** , Geef uw referenties voor de account met Azure abonnement en klik op **aanmelden**.
20. U ziet het volgende dialoogvenster:

    ![Publiceren, dialoogvenster](./media/data-factory-build-your-first-pipeline-using-vs/publish.png)

21. **Data factory genoemd configureren** op de pagina kunt u de volgende stappen uitvoeren: 
    1. Die optie **Gebruik bestaande data factory** bevestigen.
    2. Selecteer de **data factory** was selecteren wanneer u met behulp van de sjabloon. 
    6. Klik op **volgende** om te schakelen naar de pagina **Items publiceren** . (Druk op **TAB** om uit het veld naam als de knop **volgende** uitgeschakeld.) 
23. Op de pagina **Items publiceren** zorgen ervoor dat alle gegevens-fabrieken entiteiten zijn geselecteerd en klik op **volgende** om te schakelen naar de pagina **Overzicht** .     
24. Bekijk het overzicht en klik op **volgende** om het implementatieproces te starten en de **Implementatiestatus**weergeven.
25. **Implementatiestatus** op de pagina ziet u de status van het implementatieproces. Klik op Voltooien nadat de installatie is voltooid. 

Zie [de eerste gegevens fabriek (Visual Studio) maken](data-factory-build-your-first-pipeline-using-vs.md) voor meer informatie over het gebruik van Visual Studio om Data Factory entiteiten en publiceren naar Azure.          