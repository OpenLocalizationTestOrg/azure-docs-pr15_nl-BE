<properties
    pageTitle="Gegevens verplaatsen van een lokale SQL Server naar SQL Azure met Azure Data Factory | Azure"
    description="Een automatische Documentinvoer pijpleiding die twee data migration activiteiten samen verplaatsen van gegevens op een dagelijkse basis tussen databases Lokaal en in de cloud stelt het bericht instellen."
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun" />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/14/2016"
    ms.author="bradsev" />


# <a name="move-data-from-an-on-premise-sql-server-to-sql-azure-with-azure-data-factory"></a>Gegevens verplaatsen van een lokale SQL server naar SQL Azure met Azure Data Factory

In dit onderwerp wordt uitgelegd hoe gegevens van een lokale SQL Server-Database verplaatsen naar een SQL-Database Azure via Azure Blob-opslag met Azure Data Factory (ADF).

De volgende **menu** koppelingen naar onderwerpen waarin wordt beschreven hoe u de gegevens in de doel-omgevingen waar de gegevens kunnen worden opgeslagen en verwerkt tijdens de wetenschap Team gegevens nemen.

[AZURE.INCLUDE [cap-ingest-data-selector](../../includes/cap-ingest-data-selector.md)]


## <a name="intro"></a>Inleiding: Wat is automatische Documentinvoer en wanneer moet het worden gebruikt om gegevens te migreren?

Azure Data Factory is een volledig beheerde cloud-gebaseerde data integratieservice orchestrates en automatiseert het verkeer en de transformatie van gegevens. Het belangrijkste concept in de ADF-model is een pijpleiding. Een pijplijn is een logische groepering van activiteiten, die elk definieert de acties uit te voeren op de gegevens in de Datasets. Gekoppelde services worden gebruikt om de gegevens die nodig zijn voor Data Factory om verbinding met de gegevensbronnen te definiëren.

Met automatische Documentinvoer, kunnen bestaande gegevens verwerken services bestaan in pijpleidingen van gegevens die zeer stabiel en beheerd in de cloud zijn. Deze gegevens pijpleidingen voor consumptie, voorbereiden, transformeren, analyseren en publiceren van gegevens kunnen worden gepland en ADF beheert en de verwerking van afhankelijkheden en complexe gegevens orchestrates. Oplossingen kunnen snel worden gebouwd en geïmplementeerd in de cloud, verbinding maken met een toenemend aantal op gebouwen en gegevensbronnen van de wolk.

Overweeg het gebruik van automatische Documentinvoer:

- Wanneer gegevens moeten voortdurend worden gemigreerd in een hybride scenario dat toegang zoekt tot bronnen van zowel lokale als cloud 
- Als u de gegevens is verwerkt of moet worden gewijzigd of toegevoegd wanneer wordt gemigreerd bedrijfslogica. 

Automatische Documentinvoer kunt u de planning en de controle van de taken met behulp van eenvoudige JSON-scripts die het verkeer van gegevens op een periodieke basis beheren. ADF heeft ook andere functies zoals ondersteuning voor complexe bewerkingen. Zie de documentatie op [Azure Factory (ADF)](https://azure.microsoft.com/services/data-factory/)voor meer informatie over automatische Documentinvoer.


## <a name="scenario"></a>Het Scenario

Stelt een ADF pijpleiding die twee data migration activiteiten stelt het bericht. Samen gaan ze gegevens op een dagelijkse basis tussen een lokale SQL-database en een Azure SQL-Database in de cloud. Er zijn twee activiteiten:

* gegevens van een lokale SQL Server-database kopiëren naar een account Azure Blob-opslag
* gegevens kopiëren vanuit de Azure Blob-opslag met een Azure SQL-Database.

>[AZURE.NOTE] De stappen die hier zijn aangepast uit de uitgebreide handleiding geleverd door het team ADF weergegeven: [verplaatsen van gegevens tussen de bronnen op gebouwen en wolken met Data Management Gateway](../data-factory/data-factory-move-data-between-onprem-and-cloud.md) verwijzingen naar de relevante gedeelten van dat onderwerp beschikbaar zijn wanneer nodig.


## <a name="prereqs"></a>Vereisten
In deze zelfstudie wordt ervan uitgegaan dat u beschikt over:

* Een **abonnement op Azure**. Als u niet een abonnement hebt, kunt u zich aanmelden voor een [gratis proefperiode](https://azure.microsoft.com/pricing/free-trial/).
* Een **account Azure opslag**. U een Azure opslag gebruikt voor het opslaan van de gegevens in deze zelfstudie. Als u geen account Azure opslag, Zie het artikel [een opslag-account maken](storage-create-storage-account.md#create-a-storage-account) . Nadat u de opslag-account hebt gemaakt, moet u de sleutel van de account gebruikt voor toegang tot de opslag te verkrijgen. Zie [weergave, kopieer en toegangstoetsen opslag opnieuw genereren](storage-create-storage-account.md#view-copy-and-regenerate-storage-access-keys).
* Toegang tot een **Azure SQL-Database**. Als u een Azure SQL-Database, moet vindt de tpoic [Aan de slag met Azure SQL-Database van Microsoft](../sql-database/sql-database-get-started.md) u informatie over het inrichten van een nieuw exemplaar van een Azure SQL-Database.
* Geïnstalleerde en geconfigureerde **Azure PowerShell** lokaal. Zie voor instructies [hoe u kunt installeren en configureren van Azure PowerShell](../powershell-install-configure.md).

> [AZURE.NOTE] Deze procedure maakt gebruik van de [portal Azure](https://portal.azure.com/).


##<a name="upload-data"></a>De gegevens uploaden naar uw lokale SQL Server

We gebruiken de [dataset RDAM Taxi](http://chriswhong.com/open-data/foil_nyc_taxi/) om aan te tonen van het migratieproces. De dataset RDAM Taxi is beschikbaar, zoals vermeld in dat bericht op de Azure blob-opslag [RDAM Taxi gegevens](http://www.andresmh.com/nyctaxitrips/). De gegevens hebben twee bestanden, het bestand trip_data.csv bevat de details van de reis, en het bestand trip_far.csv, met details over de voor elke reis betaalde reissom. Een voorbeeld en een beschrijving van deze bestanden zijn beschikbaar in [RDAM Taxi Trips Dataset beschrijving](machine-learning-data-science-process-sql-walkthrough.md#dataset).


De procedure hier om uw eigen gegevens aan te passen of de stappen zoals beschreven met behulp van de dataset RDAM Taxi. De dataset RDAM Taxi uploaden naar uw lokale SQL Server-database, volgt u de procedure die wordt beschreven in [Bulk importeren van gegevens in SQL Server-Database](machine-learning-data-science-process-sql-walkthrough.md#dbload). Deze instructies zijn voor een SQL-Server op een Azure Virtual Machine, maar de procedure voor het uploaden naar de lokale SQL Server hetzelfde is.


##<a name="create-adf"></a>Maak een Azure Data Factory

De instructies voor het maken van een nieuwe Azure Data Factory en een resourcegroep in [Azure portal](https://portal.azure.com/) beschikbaar [maken een Azure Data Factory](../data-factory/data-factory-build-your-first-pipeline-using-editor.md#step-1-creating-the-data-factory). Naam van het nieuwe exemplaar ADF- *adfdsp* en de naam van de resource groep gemaakt *adfdsprg*.


## <a name="install-and-configure-up-the-data-management-gateway"></a>Installeren en configureren van de Data Management Gateway

Als u wilt dat uw pijpleidingen in een fabriek Azure gegevens om te werken met een lokale SQL Server, moet u toe te voegen als een Service gekoppeld aan de fabriek van de gegevens. Als u wilt een gekoppelde Service voor een lokale SQL Server maakt, moet u:

- Download en installeer Microsoft Data Management Gateway op de lokale computer. 
- Configureer de service gekoppelde gegevensbron gebruik van de gateway op gebouwen. 

De Data Management Gateway serialiseert en deserializes van de bron- en sink-gegevens op de computer waarbij deze wordt gehost.

Zie voor installatie-instructies en meer informatie over Data Management Gateway, [verplaatsen van gegevens tussen de bronnen op gebouwen en wolken met Data Management Gateway](../data-factory/data-factory-move-data-between-onprem-and-cloud.md)


## <a name="adflinkedservices"></a>Gekoppelde services verbinding maken met de gegevensbronnen maken

Een gekoppelde service definieert de gegevens die nodig zijn voor Data Factory Azure verbinding maken met een bron van gegevens. De stapsgewijze procedure voor het maken van gekoppelde services is beschikbaar in het [maken van gekoppelde services](../data-factory/data-factory-move-data-between-onprem-and-cloud.md#step-2-create-linked-services).

Wij hebben drie resources in dit scenario waarvoor de gekoppelde services vereist zijn.

1. [Gekoppelde service voor lokale SQL Server](#adf-linked-service-onprem-sql)
2. [Gekoppelde service voor Azure Blob-opslag](#adf-linked-service-blob-store)
3. [Gekoppelde service voor Azure SQL-database](#adf-linked-service-azure-sql)


###<a name="adf-linked-service-onprem-sql"></a>Gekoppelde service voor lokale SQL Server-database

De gekoppelde service voor de lokale SQL Server maken:

- Klik op het **Gegevensarchief** in de ADF landingspagina op Azure klassieke Portal 
- Selecteer **SQL** en voert u de *gebruikersnaam* en *wachtwoord* referenties voor de lokale SQL Server. U moet de servernaam invoeren als een **volledige servernaam backslash instantienaam (servernaam\exemplaarnaam)**. Naam van de gekoppelde service *adfonpremsql*.

###<a name="adf-linked-service-blob-store"></a>Gekoppelde service voor Blob

De gekoppelde service voor de account Azure Blob-opslag maken:

- Klik op het **Gegevensarchief** in de ADF landingspagina op Azure klassieke Portal
- Selecteer **Azure opslag Account** 
- Azure Blob-opslag container sleutel en de naam van de account invoeren. Naam van de gekoppelde Service *adfds*.

###<a name="adf-linked-service-azure-sql"></a>Gekoppelde service voor Azure SQL-database

De gekoppelde service voor de Azure SQL-Database maken:

- Klik op het **Gegevensarchief** in de ADF landingspagina op Azure klassieke Portal
- Selecteer **SQL Azure** en typ de *gebruikersnaam* en *wachtwoord* referenties voor de Azure SQL-Database. De *gebruikersnaam* moet worden opgegeven als *user@servername*.   


##<a name="adf-tables"></a>Definiëren en maken van tabellen om aan te geven hoe u de datasets

Tabellen die de structuur, de locatie en de beschikbaarheid van de datasets met de volgende procedures op basis van een script opgeeft te maken. JSON-bestanden worden gebruikt om de tabellen te definiëren. Zie voor meer informatie over de structuur van deze bestanden, [Datasets](../data-factory/data-factory-create-datasets.md).

> [AZURE.NOTE]  Moet worden uitgevoerd de `Add-AzureAccount` cmdlet voordat de cmdlet [New-AzureDataFactoryTable](https://msdn.microsoft.com/library/azure/dn835096.aspx) om te bevestigen dat het abonnement rechts Azure is geselecteerd voor de uitvoering van de opdracht wordt uitgevoerd. Zie voor de documentatie van deze cmdlet [Add-AzureAccount](https://msdn.microsoft.com/library/azure/dn790372.aspx).

De definities op basis van JSON in de tabellen gebruiken de volgende namen:

* de **naam van de tabel** in de lokale SQL server is *nyctaxi_data*
* de **naam van de container** in de account Azure Blob-opslag is *containername*  

Drie definities zijn nodig voor deze pijpleiding ADF:

1. [Lokale SQL-tabel](#adf-table-onprem-sql)
2. [BLOB-tabel](#adf-table-blob-store)
3. [SQL Azure tabel](#adf-table-azure-sql)

> [AZURE.NOTE]  Deze procedures met Azure PowerShell kunt definiëren en maken van de activiteiten van de ADF. Maar deze taken kunnen ook worden bereikt met behulp van de portal Azure. Zie voor details [maken van input en output datasets](../data-factory/data-factory-move-data-between-onprem-and-cloud.md#step-3-create-input-and-output-datasets).

###<a name="adf-table-onprem-sql"></a>Lokale SQL-tabel

De definitie van de tabel voor de lokale SQL Server is opgegeven in de volgende JSON-bestand:

        {
            "name": "OnPremSQLTable",
            "properties":
            {
                "location":
                {
                "type": "OnPremisesSqlServerTableLocation",
                "tableName": "nyctaxi_data",
                "linkedServiceName": "adfonpremsql"
                },
                "availability":
                {
                "frequency": "Day",
                "interval": 1,   
                "waitOnExternal":
                {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
                }

                }
            }
        }

De namen van kolommen zijn niet opgenomen in hier. U kunt sub selecteren op de namen van de door ze hier (details voor de controle van het onderwerp [ADF-documentatie](../data-factory/data-factory-data-movement-activities.md ) .

De JSON-definitie van de tabel kopiëren naar een bestand met de naam *onpremtabledef.json* bestand en sla het op een bekende locatie (hier uitgegaan van *C:\temp\onpremtabledef.json*). De tabel met de volgende Azure PowerShell-cmdlet van automatische Documentinvoer maken:

    New-AzureDataFactoryTable -ResourceGroupName ADFdsprg -DataFactoryName ADFdsp –File C:\temp\onpremtabledef.json


###<a name="adf-table-blob-store"></a>BLOB-tabel
De definitie voor de tabel voor de locatie van de blob uitvoer wordt in het volgende (wijst de geconsumeerde gegevens van lokale naar Azure blob):

        {
            "name": "OutputBlobTable",
            "properties":
            {
                "location":
                {
                "type": "AzureBlobLocation",
                "folderPath": "containername",
                "format":
                {
                "type": "TextFormat",
                "columnDelimiter": "\t"
                },
                "linkedServiceName": "adfds"
                },
                "availability":
                {
                "frequency": "Day",
                "interval": 1
                }
            }
        }

De JSON-definitie van de tabel kopiëren naar een bestand met de naam *bloboutputtabledef.json* bestand en sla het op een bekende locatie (hier uitgegaan van *C:\temp\bloboutputtabledef.json*). De tabel met de volgende Azure PowerShell-cmdlet van automatische Documentinvoer maken:

    New-AzureDataFactoryTable -ResourceGroupName adfdsprg -DataFactoryName adfdsp -File C:\temp\bloboutputtabledef.json  

###<a name="adf-table-azure-sq"></a>SQL Azure tabel
De definitie voor de tabel voor de Azure SQL output is in de volgende (dit schema toegewezen gegevens die afkomstig zijn uit de blob):

    {
        "name": "OutputSQLAzureTable",
        "properties":
        {
            "structure":
            [
                { "name": "column1", type": "String"},
                { "name": "column2", type": "String"}                
            ],
            "location":
            {
                "type": "AzureSqlTableLocation",
                "tableName": "your_db_name",
                "linkedServiceName": "adfdssqlazure_linked_servicename"
            },
            "availability":
            {
                "frequency": "Day",
                "interval": 1            
            }
        }
    }

De JSON-definitie van de tabel kopiëren naar een bestand met de naam *AzureSqlTable.json* bestand en sla het op een bekende locatie (hier uitgegaan van *C:\temp\AzureSqlTable.json*). De tabel met de volgende Azure PowerShell-cmdlet van automatische Documentinvoer maken:

    New-AzureDataFactoryTable -ResourceGroupName adfdsprg -DataFactoryName adfdsp -File C:\temp\AzureSqlTable.json  


##<a name="adf-pipeline"></a>Definiëren en maken van de pijplijn

De activiteiten die deel uitmaken van de pijpleiding en de pijpleiding maken met de volgende procedures op basis van een script opgeven. Een JSON-bestand wordt gebruikt voor het definiëren van de eigenschappen van de pijpleiding.

* Het script wordt ervan uitgegaan dat de **pipeline-naam** *AMLDSProcessPipeline*.
* Vergeet niet dat we de periodiciteit van de oorspronkelijke pijpleiding worden uitgevoerd op een dagelijkse basis en de uitvoeringstijd standaard gebruikt voor het project (12 uur UTC) ingesteld.

> [AZURE.NOTE]Azure PowerShell de volgende procedures gebruiken om te definiëren en maken van de automatische Documentinvoer pijpleiding. Maar deze taak kan ook worden bereikt met behulp van de portal Azure. Zie voor details, [maken en uitvoeren van een pijpleiding](../data-factory/data-factory-move-data-between-onprem-and-cloud.md#step-4-create-and-run-a-pipeline).

Met behulp van de eerder vastgestelde definities, wordt de definitie van de pijplijn voor de ADF als volgt opgegeven:

        {
            "name": "AMLDSProcessPipeline",
            "properties":
            {
                "description" : "This pipeline has one Copy activity that copies data from an on-premise SQL to Azure blob",
                 "activities":
                [
                    {
                        "name": "CopyFromSQLtoBlob",
                        "description": "Copy data from on-premise SQL server to blob",     
                        "type": "CopyActivity",
                        "inputs": [ {"name": "OnPremSQLTable"} ],
                        "outputs": [ {"name": "OutputBlobTable"} ],
                        "transformation":
                        {
                            "source":
                            {                               
                                "type": "SqlSource",
                                "sqlReaderQuery": "select * from nyctaxi_data"
                            },
                            "sink":
                            {
                                "type": "BlobSink"
                            }   
                        },
                        "Policy":
                        {
                            "concurrency": 3,
                            "executionPriorityOrder": "NewestFirst",
                            "style": "StartOfInterval",
                            "retry": 0,
                            "timeout": "01:00:00"
                        }       

                     },

                    {
                        "name": "CopyFromBlobtoSQLAzure",
                        "description": "Push data to Sql Azure",        
                        "type": "CopyActivity",
                        "inputs": [ {"name": "OutputBlobTable"} ],
                        "outputs": [ {"name": "OutputSQLAzureTable"} ],
                        "transformation":
                        {
                            "source":
                            {                               
                                "type": "BlobSource"
                            },
                            "sink":
                            {
                                "type": "SqlSink",
                                "WriteBatchTimeout": "00:5:00",             
                            }           
                        },
                        "Policy":
                        {
                            "concurrency": 3,
                            "executionPriorityOrder": "NewestFirst",
                            "style": "StartOfInterval",
                            "retry": 2,
                            "timeout": "02:00:00"
                        }
                     }
                ]
            }
        }

Deze JSON-definitie van de pijpleiding kopiëren naar een bestand met de naam *pipelinedef.json* bestand en sla het op een bekende locatie (hier uitgegaan van *C:\temp\pipelinedef.json*). De pijpleiding van automatische Documentinvoer maken met de volgende Azure PowerShell-cmdlet:

    New-AzureDataFactoryPipeline  -ResourceGroupName adfdsprg -DataFactoryName adfdsp -File C:\temp\pipelinedef.json

Bevestig dat u dat de pijpleiding in de ADF in de klassieke Portal Azure weergeven als volgt zien kunt (als u op het diagram)

![](media/machine-learning-data-science-move-sql-azure-adf/DJP1kji.png)


##<a name="adf-pipeline-start"></a>Start de pijplijn
De pijpleiding kan nu worden uitgevoerd met de volgende opdracht:

    Set-AzureDataFactoryPipelineActivePeriod -ResourceGroupName ADFdsprg -DataFactoryName ADFdsp -StartDateTime startdateZ –EndDateTime enddateZ –Name AMLDSProcessPipeline

De *begindatum* en *einddatum* parameterwaarden moeten worden vervangen door de werkelijke datums waartussen u wilt dat de pijpleiding worden uitgevoerd.

Zodra de pijpleiding wordt uitgevoerd, moet u de gegevens weergegeven in de container voor de blob, één bestand per dag geselecteerd.

Houd er rekening mee dat we niet de functionaliteit die stapsgewijs door ADF pipe gegevens hebben gebruikt. Zie de [documentatie van de automatische Documentinvoer](https://azure.microsoft.com/services/data-factory/)voor meer informatie over het uitvoeren van deze en andere mogelijkheden van de ADF.
