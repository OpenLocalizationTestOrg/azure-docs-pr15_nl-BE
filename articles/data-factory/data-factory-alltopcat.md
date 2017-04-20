<properties
    pageTitle="Alle onderwerpen voor Data Factory service | Microsoft Azure"
    description="Tabel van alle onderwerpen voor de Azure service met de naam Data Factory in http://azure.microsoft.com/documentation/articles/, titel en beschrijving."
    services="data-factory"
    documentationCenter=""
    authors="spelluru"
    manager="jhubbard"
    editor="MightyPen"/>

<tags
    ms.service="data-factory"
    ms.workload="data-factory"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/05/2016"
    ms.author="spelluru"/>


# <a name="all-topics-for-azure-data-factory-service"></a>Alle onderwerpen voor Azure Data Factory service

In dit onderwerp worden elk onderwerp dat van toepassing op de **Data Factory** service van Azure rechtstreeks is. U kunt deze webpagina voor trefwoorden zoeken met behulp van **Ctrl + F**, te vinden in de onderwerpen van de huidige rente.




## <a name="new"></a>Nieuw

| &nbsp; | Titel | Beschrijving |
| --: | :-- | :-- |
| 1 | [Gegevens van Amazon Redshift, met Azure Data Factory](data-factory-amazon-redshift-connector.md) | Informatie over het verplaatsen van gegevens van Amazon Redshift met Azure Data Factory. |
| 2 | [Gegevens van Amazon Simple Storage-Service via Azure Data Factory](data-factory-amazon-simple-storage-service-connector.md) | Meer informatie over het verplaatsen van gegevens van Amazon Simple Storage Service (S3 haalt) met behulp van Azure Data Factory. |
| 3 | [Azure Data Factory - Wizard kopiëren](data-factory-azure-copy-wizard.md) | Meer informatie over de Wizard Data Factory Azure kopiëren gebruiken om gegevens te kopiëren van de ondersteunde gegevensbronnen te putten. |
| 4 | [Zelfstudie: Uw eerste Azure data factory Data Factory REST API met bouwen](data-factory-build-your-first-pipeline-using-rest-api.md) | In deze zelfstudie maakt u een monster Azure Data Factory pijpleiding met Data Factory REST API. |
| 5 | [Zelfstudie: Een pijpleiding met kopie activiteit met .NET API maken](data-factory-copy-activity-tutorial-using-dotnet-api.md) | In deze zelfstudie maakt u een pijpleiding Azure Data Factory met een activiteit kopiëren met behulp van .NET API. |
| 6 | [Zelfstudie: Een pijpleiding met kopie activiteit met REST API maken](data-factory-copy-activity-tutorial-using-rest-api.md) | In deze zelfstudie maakt u een pijpleiding Azure Data Factory met een activiteit kopiëren met REST API. |
| 7 | [Wizard Data Factory-kopiëren](data-factory-copy-wizard.md) | Meer informatie over de Wizard fabriek kopie gebruiken om gegevens te kopiëren van de ondersteunde gegevensbronnen te putten. |
| 8 | [Data Management Gateway](data-factory-data-management-gateway.md) | Een gegevensgateway instellen om gegevens te verplaatsen tussen lokale en de cloud. Data Management Gateway in Azure Data Factory gebruiken om uw gegevens te verplaatsen. |
| 9 | [Gegevens verplaatsen uit een op gebouwen Cassandra database met Azure Data Factory](data-factory-onprem-cassandra-connector.md) | Informatie over het verplaatsen van gegevens uit een op gebouwen Cassandra database met Azure Data Factory. |
| 10 | [Gegevens van MongoDB met Azure Data Factory](data-factory-on-premises-mongodb-connector.md) | Informatie over het verplaatsen van gegevens uit MongoDB database met Azure Data Factory. |
| 11 | [Gegevens van de televergaderingen verplaatsen met behulp van Azure Data Factory](data-factory-salesforce-connector.md) | Informatie over het verplaatsen van gegevens uit de televergaderingen met behulp van Azure Data Factory. |


## <a name="updated-articles-data-factory"></a>Bijgewerkte artikelen, Data Factory

Deze sectie bevat een lijst van artikelen die onlangs zijn bijgewerkt waarin de update groot of significant is. Voor elk artikel bijgewerkt, wordt een ruwe fragment van de prijsverlaging toegevoegde tekst weergegeven. De artikelen zijn bijgewerkt binnen het datumbereik van **2016-08-22** tot en met **2016-10-05**.

| &nbsp; | Artikel | Bijgewerkte tekst, fragment | Bijgewerkt toen |
| --: | :-- | :-- | :-- |
| 12 | [Azure Data Factory - logbestand voor .NET API](data-factory-api-change-log.md) | Dit artikel bevat informatie over wijzigingen op Azure Data Factory SDK in een specifieke versie. Vindt u de meest recente NuGet package voor Azure Data Factory hier (https://www.nuget.org/packages/Microsoft.Azure.Management.DataFactories) **versie 4.11.0** functie toevoegingen: / de volgende typen gekoppelde services zijn toegevoegd: - OnPremisesMongoDbLinkedService (https://msdn.microsoft.com/library/mt765129.aspx) - AmazonRedshiftLinkedService (https://msdn.microsoft.com/library/mt765121.aspx) - AwsAccessKeyLinkedService (https://msdn.microsoft.com/library/mt765144.aspx) / zijn de volgende typen in de dataset toegevoegd: - MongoDbCollectionDataset (https://msdn.microsoft.com/library/mt765145.aspx) - AmazonS3Dataset (https://msdn.microsoft.com/library/mt765112.aspx) / het volgende exemplaar bronsoorten zijn toegevoegd. :-MongoDbSource (https://msdn.microsoft.com/en-US/library/mt765123.aspx) **versie 4.10.0** / de volgende optionele eigenschappen zijn toegevoegd aan de tekstopmaak:-Ski | 2016-09-22 |
| 13 | [Gegevens verplaatsen en naar Azure Blob met Azure Data Factory](data-factory-azure-blob-connector.md) |  / copyBehavior / bepaalt het gedrag kopiëren wanneer de bron BlobSource of bestandssysteem.  / **PreserveHierarchy:** de bestandshiërarchie in de doelmap blijven behouden. Het relatieve pad van het bronbestand naar bronmap is identiek aan het relatieve pad van het doelbestand naar doelmap... br /... br /. **FlattenHierarchy:** alle bestanden uit de bronmap zijn in het eerste niveau van de doelmap. De doelbestanden hebben de naam automatisch gegenereerd. .BR /... br /. **MergeFiles: (standaard)** alle bestanden uit de bronmap in één bestand worden samengevoegd. Als de naam van het bestand/Blob is opgegeven, is de naam van het samengevoegde de opgegeven naam; anders zou worden automatisch gegenereerde bestandsnaam.  / Nee / **BlobSource** biedt ook ondersteuning voor deze twee eigenschappen voor achterwaartse compatibiliteit. / **treatEmptyAsNull**: Hiermee bepaalt u of null of een lege tekenreeks als null-waarde te behandelen. / **skipHeaderLineCount** - Hiermee geeft u op hoeveel regels moeten worden overgeslagen. Het geldt alleen als invoer dataset is met behulp van tekstopmaak. **BlobSink** biedt ook ondersteuning voor th | 2016-09-28 |
| 14 | [Voorspellende pijpleidingen met Azure Machine Learning-activiteiten maken](data-factory-azure-ml-batch-execution-activity.md) | **Meerdere ingangen is vereist** Als de webservice meerdere ingangen heeft, gebruikt u de eigenschap **webServiceInputs** in plaats van **webServiceInput**. DataSets waarnaar wordt verwezen door de **webServiceInputs** moet ook worden opgenomen in de activiteit- **ingangen**. In het experiment Azure ML hebben web service input en output poorten en globale parameters standaardnamen ("input1", "input2") die u kunt aanpassen. De namen die u voor de webServiceInputs, webServiceOutputs en globalParameters-instellingen moeten exact overeenkomen met de namen in de experimenten. U kunt de nettolading monster aanvraag bekijken op de pagina Help voor Batch worden uitgevoerd voor uw Azure ML eindpunt om te controleren of de verwachte toewijzing.    {'naam': 'PredictivePipeline', 'Eigenschappen': {"description": "gebruiken AzureML model", "activiteiten": {"naam": "MLActivity", "type": "AzureMLBatchExecution", "omschrijving": 'voorspelling analyse op batch input', 'ingangen': {"naam": "inputDataset1"}, {"naam": "inputDatase | 13-09-2016 |
| 15 | [Prestaties van de activiteit en tuning guide kopiëren](data-factory-copy-activity-performance.md) | 1. **een basislijn maken**. Test uw pipeline via kopiëren activiteit ten opzichte van een representatief voorbeeld tijdens de ontwikkelingsfase. De Data Factory segmentering model (data-fabriek-planning-en-execution.md-time-series-datasets-and-data-slices) kunt u de hoeveelheid gegevens waarmee die u werkt te beperken.  Uitvoeringstijd en prestatiekenmerken verzamelen met behulp van de **controle en beheer App**. Kies **Monitor en beheren** op de startpagina van uw Data Factory. Kies de **dataset uitvoer**in de structuurweergave. Kies in de lijst **Windows activiteit** de kopie activiteit uitvoeren. **Activiteit Windows** geeft een overzicht van de duur van de activiteit van de kopie en de grootte van de gegevens die worden gekopieerd. De doorvoer wordt vermeld in het **Venster Explorer van activiteit**. Voor meer informatie over de app, Zie Monitor en Azure Data Factory pijpleidingen beheren met behulp van de controle en beheer App (data-fabriek-monitor-beheren-app.md).  ! Activiteit uitvoeren (./media/data-factory-copy-activity-performance/mmapp-activity-run-details.pn details | 2016-09-27 |
| 16 | [Zelfstudie: Een pijpleiding aan met behulp van Visual Studio kopiëren-activiteit maken](data-factory-copy-activity-tutorial-using-visual-studio.md) |   Let op de volgende punten:-dataset- **type** is ingesteld op **AzureBlob**.     - **linkedServiceName** is ingesteld op **AzureStorageLinkedService**. U kunt deze service gekoppelde gemaakt in stap 2.     - **folderPath** is ingesteld op de container **adftutorial** . Ook kunt u de naam van een blob in de map met behulp van de eigenschap **fileName** . Nadat u de naam van de blob niet opgeeft, worden gegevens van alle BLOB's in de container wordt beschouwd als een ingevoerde gegevens.    -indeling- **type** is ingesteld op **tekstopmaak** - er zijn twee velden in de tekst bestand ΓÇô **Voornaam** en **Achternaam** ΓÇô gescheiden door een komma-teken (**columnDelimiter**) - de **beschikbaarheid** is ingesteld op **elk uur** (**frequentie** is ingesteld op **uren** en **interval** is ingesteld op **1**). Daarom Data Factory gezocht voor invoergegevens elk uur in de hoofdmap van de blob container (**adftutorial**) dat u hebt opgegeven.  Als u een **bestandsnaam** voor een dataset **invoer** , zijn alle bestanden/BLOB's uit de map met invoer (**mappad**) Zorg | 2016-09-29 |
| 17 | [Maken, controleren en beheren van gegevens Azure fabrieken met Data Factory .NET SDK](data-factory-create-data-factories-programmatically.md) | Noteren de toepassings-ID en het wachtwoord (geheim van de client) en gebruikt in het scenario. **Azure ophalen abonnement en huurder id 's** Als er geen laatste versie van PowerShell op uw computer is geïnstalleerd, volgt u de instructies in het installeren en configureren van Azure PowerShell (.. / powershell-installatie-configure.md) artikel te installeren. 1. Azure PowerShell Start en voer de volgende opdracht 2. Voer de volgende opdracht uit en voert u de gebruikersnaam en het wachtwoord die u gebruikt voor aanmelding bij de Azure portal.         Login AzureRmAccount als die u slechts één Azure-abonnement is gekoppeld aan deze account hebt, hoeft u niet de volgende twee stappen uitvoeren. 3. Voer de volgende opdracht om de abonnementen voor deze account bekijken.       Get-AzureRmSubscription 4. Selecteer het abonnement dat u werken wilt met de volgende opdracht uitvoeren. **NameOfAzureSubscription** vervangen door de naam van uw abonnement op Azure.       Get-AzureRmSubscription - SubscriptionName-NameOfAzureSubscription / Set-AzureRmCo | 2016-09-14 |
| 18 | [Pijpleidingen en activiteiten in Azure Data Factory](data-factory-create-pipelines.md) |       'start': ' 2016-07-12T00:00:00Z ', 'end': ' 2016-07-13T00:00:00Z "}} Let op de volgende punten: In de sectie activiteiten, is slechts één activiteit waarvan het **type** is ingesteld op **kopiëren**. / Invoer voor de activiteit is ingesteld op **InputDataset** en uitvoer voor de activiteit is ingesteld op **OutputDataset**. / **BlobSource** is opgegeven als het brontype in de sectie **typeProperties** en **SqlSink** is opgegeven als het type sink. Zie voor een compleet overzicht van het maken van deze pijpleiding zelfstudie: gegevens van Blob-opslag kopiëren naar de SQL-Database (data-factory-copy-data-from-azure-blob-storage-to-sql-database.md). **Monster transformatie pijplijn** In het volgende voorbeeld pipeline is er een activiteit van het type **HDInsightHive** in de sectie **activiteiten** . In dit voorbeeld wordt de activiteit van de component HDInsight (data-fabriek-component-activity.md) gegevens uit een Azure Blob-opslag getransformeerd door het uitvoeren van een scriptbestand component op een cluster Azure HDInsight Hadoop.  {"name": "TransformPipeline", "p | 2016-09-27 |
| 19 | [Transformatie van gegevens in Azure Data Factory](data-factory-data-transformation-activities.md) | Data Factory ondersteunt de volgende data transformation activiteiten die kunnen worden toegevoegd aan pijpleidingen (data-fabriek-maken-pipelines.md) hetzij afzonderlijk of met een andere activiteit wordt geschakeld. .  AZURE. Zie Opmerking voor een scenario met stapsgewijze instructies maken een pijpleiding met artikel van component transformatie (data-factory-build-your-first-pipeline.md). **Component HDInsight activiteit** De activiteit van de HDInsight-component in een pijpleiding Data Factory voert component query's op uw eigen of bellen op Windows, Linux-gebaseerde HDInsight cluster. Zie Component activiteit (data-fabriek-component-activity.md)-artikel voor meer informatie over deze activiteit. **HDInsight varkens activiteit** De activiteit HDInsight varkens in een pijpleiding Data Factory voert varken query's op uw eigen of bellen op Windows, Linux-gebaseerde HDInsight cluster. Zie activiteit van varkens (data-fabriek-varken-activity.md)-artikel voor meer informatie over deze activiteit. **Activiteit HDInsight MapReduce** De activiteit HDInsight MapReduce in een pijpleiding Data Factory uitvoert MapReduce programma's op y | 2016-09-26 |
| 20 | [Data Factory-planning en uitvoering](data-factory-scheduling-and-execution.md) | CopyActivity2 zou worden alleen uitgevoerd als de CopyActivity1 met succes is uitgevoerd en Dataset2 beschikbaar is. Hier is de steekproef pijpleiding JSON: {'naam': 'ChainActivities', 'Eigenschappen': {"Beschrijving": "Activiteiten uitvoeren in de reeks", "activiteiten": {"type": 'Kopiëren', 'typeProperties': {"bron": {"type": "BlobSource"}, 'sink': {"type": "BlobSink", "copyBehavior": "PreserveHierarchy", "writeBatchSize": 0, "writeBatchTimeout": "00: 00:00"}}, "inputs": {"naam": "Dataset1"}, "uitvoer": {"naam": "Dataset2"}, 'beleid': {'timeout': ' 01: 00:00 "},"planner": {"frequentie": 'Uur', 'interval': 1},"name":"CopyFromBlob1ToBlob2","omschrijving": 'Gegevens uit een blob kopiëren naar een andere'}, {"type": 'Kopiëren', 'typeProperties': {"bron": {"type":"BlobSource"}, 'sink' : {"type": "BlobSink", "writeBatchSize": 0, "writeBatchTimeout": "00: 00:00"}}, "in | 2016-09-28 |





## <a name="tutorials"></a>Zelfstudies

| &nbsp; | Titel | Beschrijving |
| --: | :-- | :-- |
| 21 | [Zelfstudie: Uw eerste pijpleiding om gegevens te verwerken met behulp van Hadoop-cluster maken](data-factory-build-your-first-pipeline.md) | Deze zelfstudie Azure Data Factory wordt beschreven hoe u maken en plannen van een fabriek voor gegevens die met behulp van script component op een cluster Hadoop gegevens verwerkt. |
| 22 | [Zelfstudie: De eerste gegevens Azure fabriek met behulp van bronbeheer Azure-sjabloon maken](data-factory-build-your-first-pipeline-using-arm.md) | In deze zelfstudie maakt u een monster Azure Data Factory pijpleiding met behulp van een sjabloon Azure Resource Manager. |
| 23 | [Zelfstudie: Uw eerste Azure data factory met Azure portal maken](data-factory-build-your-first-pipeline-using-editor.md) | In deze zelfstudie maakt u een monster Azure Data Factory pijpleiding met Data Factory-Editor in de portal Azure. |
| 24 | [Zelfstudie: Uw eerste Azure data factory met Azure PowerShell bouwen](data-factory-build-your-first-pipeline-using-powershell.md) | In deze zelfstudie maakt u een monster Azure Data Factory pijpleiding met Azure PowerShell. |
| 25 | [Zelfstudie: Uw eerste Azure Build data factory met behulp van Microsoft Visual Studio](data-factory-build-your-first-pipeline-using-vs.md) | In deze zelfstudie maakt u een monster Azure Data Factory pijpleiding met behulp van Visual Studio. |
| 26 | [Zelfstudie: Een pijpleiding met kopie activiteit met Azure portal maken](data-factory-copy-activity-tutorial-using-azure-portal.md) | In deze zelfstudie maakt u een pijpleiding Azure Data Factory met een activiteit kopiëren met behulp van de Editor Data Factory in Azure portal. |
| 27 | [Zelfstudie: Een pijpleiding met kopie activiteit met Azure PowerShell maken](data-factory-copy-activity-tutorial-using-powershell.md) | In deze zelfstudie maakt u een pijpleiding Azure Data Factory met een activiteit kopiëren met Azure PowerShell. |
| 28 | [Zelfstudie: Een pijpleiding aan met behulp van Visual Studio kopiëren-activiteit maken](data-factory-copy-activity-tutorial-using-visual-studio.md) | In deze zelfstudie maakt u een pijpleiding Azure Data Factory met een activiteit kopiëren met behulp van Visual Studio. |
| 29 | [Gegevens kopiëren uit de Blob-opslag met SQL-Database met behulp van Data Factory](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) | In deze zelfstudie wordt beschreven hoe u kopie activiteit in een pijpleiding Azure Data Factory gebruiken om gegevens te kopiëren van de Blob-opslag met SQL-database. |
| 30 | [Zelfstudie: Een pijpleiding met kopie activiteit met Wizard fabriek kopie maken](data-factory-copy-data-wizard-tutorial.md) | In deze zelfstudie maakt u een pijpleiding Azure Data Factory met een activiteit kopiëren met behulp van de Wizard kopiëren, ondersteund door Data Factory |



## <a name="data-movement"></a>Verplaatsing van gegevens

| &nbsp; | Titel | Beschrijving |
| --: | :-- | :-- |
| 31 | [Gegevens verplaatsen en naar Azure Blob met Azure Data Factory](data-factory-azure-blob-connector.md) | Informatie over het kopiëren van blob-gegevens in Azure Data Factory. Gebruik onze voorbeeld: het kopiëren van gegevens naar en van Azure Blob-opslag en Azure SQL-Database. |
| 32 | [Gegevens verplaatsen en naar Azure Lake gegevensarchief maken met Azure Data Factory](data-factory-azure-datalake-connector.md) | Meer informatie over het verplaatsen van gegevens uit Azure Lake gegevensarchief maken met Azure Data Factory |
| 33 | [Gegevens verplaatsen naar en van de DocumentDB met Azure Data Factory](data-factory-azure-documentdb-connector.md) | Meer informatie over hoe gegevens verplaatsen/Azure DocumentDB collectie met Azure Data Factory |
| 34 | [Gegevens verplaatsen en naar Azure SQL-Database met behulp van Azure Data Factory](data-factory-azure-sql-connector.md) | Informatie over het verplaatsen van gegevens van Azure SQL-Database met behulp van Azure Data Factory. |
| 35 | [Gegevens verplaatsen en naar Azure SQL Data Warehouse met Azure Data Factory](data-factory-azure-sql-data-warehouse-connector.md) | Meer informatie over het verplaatsen van gegevens uit Azure SQL Data Warehouse met Azure Data Factory |
| 36 | [Gegevens verplaatsen en naar Azure tabel met Azure Data Factory](data-factory-azure-table-connector.md) | Informatie over het verplaatsen van gegevens van Azure tabelopslag met Azure Data Factory. |
| 37 | [Prestaties van de activiteit en tuning guide kopiëren](data-factory-copy-activity-performance.md) | Meer informatie over de belangrijkste factoren die invloed hebben op de prestaties van de verplaatsing van gegevens in Azure Data Factory wanneer u kopie activiteit. |
| 38 | [Gegevens verplaatsen met behulp van activiteit kopiëren](data-factory-data-movement-activities.md) | Meer informatie over het verplaatsen van gegevens Data Factory pijpleidingen: gegevensmigratie wolk winkels, en een winkel in ruimten tot een wolk winkel. Gebruik kopiëren activiteit. |
| 39 | [Release-opmerkingen voor Data Management Gateway](data-factory-gateway-release-notes.md) | Data Management Gateway taire release-opmerkingen |
| 40 | [Gegevens verplaatsen uit voor bedrijfsruimten HDFS met Azure Data Factory](data-factory-hdfs-connector.md) | Informatie over het verplaatsen van gegevens uit voor bedrijfsruimten HDFS met Azure Data Factory. |
| 41 | [Bewaken en beheren van Azure Data Factory pijpleidingen met nieuwe Monitoring en beheer App](data-factory-monitor-manage-app.md) | Informatie over Monitoring en beheer App gebruiken om te bewaken en beheren van gegevens Azure fabrieken en pijpleidingen. |
| 42 | [Gegevens verplaatsen tussen bronnen op gebouwen en de cloud met Data Management Gateway](data-factory-move-data-between-onprem-and-cloud.md) | Een gegevensgateway instellen om gegevens te verplaatsen tussen lokale en de cloud. Data Management Gateway in Azure Data Factory gebruiken om uw gegevens te verplaatsen. |
| 43 | [Gegevens uit een OData-bron met Azure Data Factory](data-factory-odata-connector.md) | Informatie over het verplaatsen van gegevens van de OData-bronnen met Azure Data Factory. |
| 44 | [Verplaats de opgeslagen gegevens van ODBC-gegevens met behulp van Azure Data Factory](data-factory-odbc-connector.md) | Informatie over het verplaatsen van gegevens uit ODBC-gegevensarchieven met Azure Data Factory. |
| 45 | [Verplaatsen van gegevens uit de DB2 met Azure Data Factory](data-factory-onprem-db2-connector.md) | Meer informatie over het verplaatsen van gegevens uit de DB2-Database met behulp van Azure Data Factory |
| 46 | [Gegevens verplaatsen en naar een bestandssysteem op ruimten met behulp van Azure Data Factory](data-factory-onprem-file-system-connector.md) | Informatie over het verplaatsen van gegevens naar en van een bestandssysteem in ruimten met behulp van Azure Data Factory. |
| 47 | [Verplaatsen van gegevens in MySQL met behulp van Azure Data Factory](data-factory-onprem-mysql-connector.md) | Informatie over het verplaatsen van gegevens van de MySQL-database met behulp van Azure Data Factory. |
| 48 | [Gegevens verplaatsen van Oracle van voor ruimten met Azure Data Factory](data-factory-onprem-oracle-connector.md) | Informatie over het verplaatsen van gegevens uit Oracle-database die op-ruimten met behulp van Azure Data Factory. |
| 49 | [Gegevens verplaatsen van PostgreSQL met Azure Data Factory](data-factory-onprem-postgresql-connector.md) | Informatie over het verplaatsen van gegevens uit de Database van PostgreSQL met Azure Data Factory. |
| 50 | [Gegevens verplaatsen van Sybase met Azure Data Factory](data-factory-onprem-sybase-connector.md) | Informatie over het verplaatsen van gegevens uit de Database van Sybase met Azure Data Factory. |
| 51 | [Gegevens verplaatsen van Teradata met Azure Data Factory](data-factory-onprem-teradata-connector.md) | Meer informatie over Teradata-Connector voor de Data Factory service waarmee u gegevens uit de Teradata-Database verplaatsen |
| 52 | [Verplaatsen van gegevens naar en van SQL Server op locatie of op IaaS (Azure VM) met Azure Data Factory](data-factory-sqlserver-connector.md) | Meer informatie over het verplaatsen van gegevens van SQL Server-database die op locatie of in een Azure VM met Azure Data Factory. |
| 53 | [Verplaatsen van gegevens uit een tabel Webbron met Azure Data Factory](data-factory-web-table-connector.md) | Informatie over het verplaatsen van gegevens op ruimten een tabel in een webpagina met Azure Data Factory. |



## <a name="data-transformation"></a>Data Transformation

| &nbsp; | Titel | Beschrijving |
| --: | :-- | :-- |
| 54 | [Voorspellende pijpleidingen met Azure Machine Learning-activiteiten maken](data-factory-azure-ml-batch-execution-activity.md) | Beschrijft hoe u voorspellende pijpleidingen met Azure Data Factory en Azure Machine Learning maken |
| 55 | [Gekoppelde Services berekenen](data-factory-compute-linked-services.md) | Meer informatie over enviornments berekenen dat u pijpleidingen Azure Data Factory gegevens transformeren kunt/proces. |
| 56 | [Verwerken grote datasets met Data Factory en Batch](data-factory-data-processing-using-batch.md) | Beschrijving van het verwerken van grote hoeveelheden gegevens in een pijpleiding Azure Data Factory met behulp van parallelle verwerking mogelijkheden van Azure Batch. |
| 57 | [Transformatie van gegevens in Azure Data Factory](data-factory-data-transformation-activities.md) | Informatie over het omzetten van gegevens of procesgegevens in Azure Data Factory met Hadoop of Machine Learning Azure gegevens Lake Analytics. |
| 58 | [Hadoop Streaming activiteit](data-factory-hadoop-streaming-activity.md) | Informatie over hoe u kunt de activiteit Hadoop Streaming in een fabriek Azure gegevens Hadoop Streaming programma's uitvoert op een op vraag/uw eigen HDInsight cluster. |
| 59 | [Activiteit van de component](data-factory-hive-activity.md) | Informatie over hoe u kunt de component activiteit in een fabriek Azure gegevens component query's uitvoeren op een op vraag/uw eigen HDInsight cluster. |
| 60 | [Data Factory MapReduce programma's aanroepen](data-factory-map-reduce.md) | Informatie over het verwerken van gegevens door MapReduce programma's uitvoeren op een cluster Azure HDInsight van een fabriek Azure gegevens. |
| 61 | [Varken activiteit](data-factory-pig-activity.md) | Informatie over hoe u kunt de activiteit van het varken in een fabriek Azure gegevens varken scripts uitvoeren op een op vraag/uw eigen HDInsight cluster. |
| 62 | [Starten programma's Spark van Data Factory](data-factory-spark.md) | Informatie over het Spark-programma's in de fabriek van een Azure gegevens met behulp van de activiteit MapReduce aanroepen. |
| 63 | [SQL Server opgeslagen Procedure activiteit](data-factory-stored-proc-activity.md) | Informatie over hoe u kunt de activiteit SQL Server opgeslagen Procedure aanroepen van een opgeslagen procedure in een Azure SQL-Database of een SQL-datawarehouse Azure van een pijpleiding Data Factory. |
| 64 | [Aangepaste activiteiten in een pijpleiding Azure Data Factory gebruiken](data-factory-use-custom-activities.md) | Informatie over het maken van aangepaste activiteiten en deze gebruiken in een pijpleiding Azure Data Factory. |
| 65 | [U SQL-script uitvoeren op Azure gegevens Lake Analytics van Azure Data Factory](data-factory-usql-activity.md) | Informatie over het verwerken van gegevens door U SQL-scripts op Azure gegevens Lake Analytics compute-service wordt uitgevoerd. |



## <a name="samples"></a>Voorbeelden

| &nbsp; | Titel | Beschrijving |
| --: | :-- | :-- |
| 66 | [Azure Data Factory - voorbeelden](data-factory-samples.md) | Bevat details over de monsters die worden geleverd bij de service Azure Data Factory. |



## <a name="use-cases"></a>Use-Cases

| &nbsp; | Titel | Beschrijving |
| --: | :-- | :-- |
| 67 | [Casestudy's bij klanten](data-factory-customer-case-studies.md) | Informatie over de manier waarop sommige van onze klanten hebben is Azure Data Factory. |
| 68 | [Use-Case - klant profilering](data-factory-customer-profiling-usecase.md) | Informatie over hoe Azure Data Factory wordt gebruikt voor het maken van gegevensgestuurde werkstromen (pijpleiding) profiel gaming-klanten. |
| 69 | [Use-Case - aanbevelingen](data-factory-product-reco-usecase.md) | Meer informatie over een use-case geïmplementeerd via Azure Data Factory samen met andere services. |



## <a name="monitor-and-manage"></a>Bewaken en beheren

| &nbsp; | Titel | Beschrijving |
| --: | :-- | :-- |
| 70 | [Bewaken en beheren van pijpleidingen Azure Data Factory](data-factory-monitor-manage-pipelines.md) | Informatie over het Azure Portal en Azure PowerShell gebruiken om te controleren en beheren van gegevens Azure fabrieken en pijpleidingen die u hebt gemaakt. |



## <a name="sdk"></a>SDK

| &nbsp; | Titel | Beschrijving |
| --: | :-- | :-- |
| 71 | [Azure Data Factory - logbestand voor .NET API](data-factory-api-change-log.md) | Beschrijving van recente wijzigingen, functie aanvullingen, correcties, enz... in een specifieke versie van .NET API voor de Azure Data Factory. |
| 72 | [Maken, controleren en beheren van gegevens Azure fabrieken met Data Factory .NET SDK](data-factory-create-data-factories-programmatically.md) | Informatie over het programmatisch maken, controleren en fabrieken Azure gegevens beheren met behulp van Data Factory SDK. |
| 73 | [Azure Data Factory: referentie voor ontwikkelaars](data-factory-sdks.md) | Informatie over verschillende manieren maken, controleren en fabrieken Azure gegevens beheren |



## <a name="miscellaneous"></a>Diverse

| &nbsp; | Titel | Beschrijving |
| --: | :-- | :-- |
| 74 | [Azure Data Factory - Veelgestelde vragen](data-factory-faq.md) | Veelgestelde vragen over Azure Data Factory. |
| 75 | [Azure Data Factory - functies en variabelen](data-factory-functions-variables.md) | Een lijst met Azure Data Factory-functies en variabelen |
| 76 | [Azure Data Factory - regels voor naamgeving](data-factory-naming-rules.md) | Beschrijving van de naamgevingsregels voor Data Factory entiteiten. |
| 77 | [Problemen met Data Factory](data-factory-troubleshoot.md) | Informatie over het oplossen van problemen met het gebruik van Azure Data Factory. |

