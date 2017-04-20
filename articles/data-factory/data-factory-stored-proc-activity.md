<properties 
    pageTitle="SQL Server opgeslagen Procedure activiteit" 
    description="Informatie over hoe u kunt de activiteit SQL Server opgeslagen Procedure aanroepen van een opgeslagen procedure in een Azure SQL-Database of een SQL-datawarehouse Azure van een pijpleiding Data Factory." 
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
    ms.date="09/30/2016" 
    ms.author="spelluru"/>

# <a name="sql-server-stored-procedure-activity"></a>SQL Server opgeslagen Procedure activiteit
> [AZURE.SELECTOR]
[Component](data-factory-hive-activity.md)  
[Varken](data-factory-pig-activity.md)  
[MapReduce](data-factory-map-reduce.md)  
[Streaming Hadoop](data-factory-hadoop-streaming-activity.md)
[Machine Learning](data-factory-azure-ml-batch-execution-activity.md) 
[Opgeslagen Procedure](data-factory-stored-proc-activity.md)
[Gegevens Lake Analytics U SQL](data-factory-usql-activity.md)
[.NET aangepaste](data-factory-use-custom-activities.md)

Aanroepen van een opgeslagen procedure in een van de volgende gegevens winkels kunt u de opgeslagen Procedure van SQL Server-activiteit in een Data Factory [pijpleiding](data-factory-create-pipelines.md) : 


- Azure SQL-Database 
- Azure SQL datawarehouse  
- SQL Server-Database in uw onderneming of een Azure VM. U moet gegevens Management Gateway installeren op dezelfde computer waarop de database of op een afzonderlijke computer om te voorkomen dat meedingt naar resources met de database. Data Management Gateway is een software die wordt aangesloten op-premises gegevensbronnen/gegevensbronnen in Azure VMs hosed cloud services zodanig beveiligd en beheerd. Zie artikel [verplaatst gegevens tussen op gebouwen en cloud](data-factory-move-data-between-onprem-and-cloud.md) voor meer informatie over Data Management Gateway. 

Dit artikel is gebaseerd op het artikel [data transformation activiteiten](data-factory-data-transformation-activities.md) , dat een algemeen overzicht van de gegevenstransformatie van en de transformatie van ondersteunde activiteiten geeft.

## <a name="walkthrough"></a>Stapsgewijze instructies

### <a name="sample-table-and-stored-procedure"></a>Voorbeeldtabel en opgeslagen procedure
1. De volgende **tabel** in uw SQL Server Management Studio of een ander hulpprogramma dat u vertrouwd met bent Azure SQL-Database maken. De kolom datetimestamp is de datum en tijd wanneer de bijbehorende ID wordt gegenereerd. 

        CREATE TABLE dbo.sampletable
        (
            Id uniqueidentifier,
            datetimestamp nvarchar(127)
        )
        GO

        CREATE CLUSTERED INDEX ClusteredID ON dbo.sampletable(Id);
        GO

    Id uniek geïdentificeerd is en de kolom datetimestamp is de datum en tijd wanneer de bijbehorende ID wordt gegenereerd.
    ![Voorbeeldgegevens](./media/data-factory-stored-proc-activity/sample-data.png)

    > [AZURE.NOTE] In dit voorbeeld Azure SQL-Database gebruikt, maar werkt op dezelfde manier voor Azure SQL Data Warehouse en SQL Server-Database. 
2. Maak de volgende **opgeslagen procedure** waarmee gegevens in de **sampletable**wordt ingevoegd.

        CREATE PROCEDURE sp_sample @DateTime nvarchar(127)
        AS
        
        BEGIN
            INSERT INTO [sampletable]
            VALUES (newid(), @DateTime)
        END

    > [AZURE.IMPORTANT] **De naam** en de **behuizing** van de parameter (DateTime in dit voorbeeld) moeten overeenkomen met die van de parameter die is opgegeven in de pijplijn/activiteit JSON. Ervoor zorgen dat in de definitie van de opgeslagen procedure **@** wordt gebruikt als voorvoegsel voor de parameter.
    
### <a name="create-a-data-factory"></a>Maak een data factory  
4. Log in op [Azure portal](https://portal.azure.com/). 
5. Klik op **Nieuw** in het linkermenu **Intelligence + Analytics**, en op **Data Factory**.
    
    ![Nieuwe data factory](media/data-factory-stored-proc-activity/new-data-factory.png)   
4.  Voer **SProcDF** voor de naam in de **nieuwe data factory** -blade. Azure Data Factory namen zijn **uniek**. Moet u als voorvoegsel voor de naam van de fabriek van gegevens met de naam voor het succesvol maken van de fabriek.

    ![Nieuwe data factory](media/data-factory-stored-proc-activity/new-data-factory-blade.png)      
3.  Selecteer uw **abonnement Azure**. 
4.  Voor de **Resourcegroep**, voer een van de volgende stappen uit: 
    1.  Klik op **Nieuw** en typ een naam voor de resourcegroep.
    2.  Klik op **bestaande gebruiken** en selecteer een bestaande brongroep.  
5.  Selecteer de **locatie** voor de fabriek van gegevens.
6.  Selecteer **aan het dashboard vastmaken** zodat u kunt zien de fabriek van de gegevens op het dashboard zodra die u zich aanmeldt. 
6.  Klik op **maken** op de **nieuwe data factory** -blade.
6.  Ziet u de data factory wordt gemaakt in het **dashboard** van de portal voor Azure. Als de data factory is gemaakt, ziet u de data factory-pagina waarin u de inhoud van de data factory genoemd.
    ![Data Factory-startpagina](media/data-factory-stored-proc-activity/data-factory-home-page.png)

### <a name="create-an-azure-sql-linked-service"></a>Maak een gekoppelde Azure SQL service  
Na het maken van de fabriek van gegevens, kunt u een gekoppelde Azure SQL service die uw Azure SQL-Database is gekoppeld aan de fabriek van de gegevens maken. Deze database bevat de sampletable tabel en sp_sample opgeslagen procedure.

7.  Klik op **auteur en implementeren van** op de **Data Factory** -blade voor **SProcDF** aan de Data Factory-Editor te starten.
2.  Klik op **nieuwe gegevens opslaan** op de werkbalk en kies **Azure SQL-Database**. U ziet de JSON-script voor het maken van een gekoppelde Azure SQL service in de editor. 

    ![Nieuw gegevensarchief](media/data-factory-stored-proc-activity/new-data-store.png)
4. In de JSON-script, moet u de volgende wijzigingen aanbrengen: 
    1. Vervangen ** &lt;servernaam&gt; ** met de naam van uw server Azure SQL-Database.
    2. Vervangen ** &lt;NaamDatabase&gt; ** met de database waarin u de tabel en de opgeslagen procedure gemaakt.
    3. Vervangen ** &lt; username@servername ** met de gebruikersaccount die toegang tot de database heeft.
    4. Vervangen ** &lt;wachtwoord&gt; ** met het wachtwoord voor de gebruikersaccount. 

    ![Nieuw gegevensarchief](media/data-factory-stored-proc-activity/azure-sql-linked-service.png)
5. Klik op **Deploy** op de opdrachtbalk aan de gekoppelde service implementeren. Bevestig dat u de AzureSqlLinkedService in de boomstructuur aan de linkerkant ziet. 

    ![boomstructuur met gekoppelde service](media/data-factory-stored-proc-activity/tree-view.png)

### <a name="create-an-output-dataset"></a>Een output-gegevensset maken
6. Klik op **... Meer** op de werkbalk, klikt u op **nieuwe dataset**en klikt u op **SQL Azure**. **Nieuwe dataset** op de werkbalk en selecteer **SQL Azure**.

    ![boomstructuur met gekoppelde service](media/data-factory-stored-proc-activity/new-dataset.png)
7. Het volgende script van JSON in JSON-editor kopiëren en plakken.

        {               
            "name": "sprocsampleout",
            "properties": {
                "type": "AzureSqlTable",
                "linkedServiceName": "AzureSqlLinkedService",
                "typeProperties": {
                    "tableName": "sampletable"
                },
                "availability": {
                    "frequency": "Hour",
                    "interval": 1
                }
            }
        }
7. Klik op **Deploy** op de werkbalk voor de implementatie van de dataset. Bevestig dat u ziet dat de dataset in de structuurweergave. 

    ![boomstructuur met gekoppelde services](media/data-factory-stored-proc-activity/tree-view-2.png)

### <a name="create-a-pipeline-with-sqlserverstoredprocedure-activity"></a>Een pijpleiding met SqlServerStoredProcedure activiteit maken
Nu maken we een pijpleiding met een SqlServerStoredProcedure activiteit.
 
9. Klik op **... Meer** op de opdracht balk en klikt u op **nieuwe pijpleiding**. 
9. In het volgende fragment van JSON kopiëren en plakken. De **storedProcedureName** is ingesteld op **sp_sample**. De naam en de behuizing van de **DateTime** -parameter moeten overeenkomen met de naam en de behuizing van de parameter in de definitie van de opgeslagen procedure.  

        {
            "name": "SprocActivitySamplePipeline",
            "properties": {
                "activities": [
                    {
                        "type": "SqlServerStoredProcedure",
                        "typeProperties": {
                            "storedProcedureName": "sp_sample",
                            "storedProcedureParameters": {
                                "DateTime": "$$Text.Format('{0:yyyy-MM-dd HH:mm:ss}', SliceStart)"
                            }
                        },
                        "outputs": [
                            {
                                "name": "sprocsampleout"
                            }
                        ],
                        "scheduler": {
                            "frequency": "Hour",
                            "interval": 1
                        },
                        "name": "SprocActivitySample"
                    }
                ],
                "start": "2016-08-02T00:00:00Z",
                "end": "2016-08-02T05:00:00Z",
                "isPaused": false
            }
        }

    Als u nodig hebt voor een parameter null doorgeeft, gebruikt u de syntaxis: "param1": null (allemaal kleine letters). 
9. Klik op **Deploy** op de werkbalk voor de implementatie van de pijpleiding.  

### <a name="monitor-the-pipeline"></a>Monitor de pijplijn

6. **X** -blades Data Factory-Editor te sluiten en terug te keren naar het blade Data Factory, en klik op **Diagram**.

    ![diagram naast elkaar](media/data-factory-stored-proc-activity/data-factory-diagram-tile.png)
7. In de **Weergave Netwerkdiagram**ziet u een overzicht van de pijpleidingen en datasets die in deze zelfstudie wordt gebruikt. 

    ![diagram naast elkaar](media/data-factory-stored-proc-activity/data-factory-diagram-view.png)
8. Dubbelklik in de weergave Netwerkdiagram op de dataset **sprocsampleout**. U ziet de segmenten in de status gereed. Moet er vijf segmenten omdat een segment wordt geproduceerd voor elk uur tussen de begintijd en eindtijd van de JSON.

    ![diagram naast elkaar](media/data-factory-stored-proc-activity/data-factory-slices.png) 
10. Als een segment **Gereed** is, een * *Schakel* in sampletable*uitgevoerd * query op de Azure SQL-database om te controleren dat de gegevens ingevoegd de tabel door de opgeslagen procedure.

    ![Uitvoergegevens](./media/data-factory-stored-proc-activity/output.png)

    Zie [Monitor de pijplijn](data-factory-monitor-manage-pipelines.md) voor gedetailleerde informatie over het controleren van Azure Data Factory pijpleidingen.  

> [AZURE.NOTE] In dit voorbeeld wordt heeft de SprocActivitySample geen invoer. Als u een keten van deze activiteit aan een activiteit stroom (dat wil zeggen voorafgaand processing) wilt maken, de uitgangen van de upstream-activiteiten kunnen worden gebruikt als invoer in deze activiteit. In dat geval uitvoeren deze activiteit niet totdat de upstream activiteit is voltooid en de resultaten van de upstream-activiteiten beschikbaar (in de status gereed zijn). De invoer kunnen niet worden gebruikt als een parameter voor de opgeslagen procedure activiteit rechtstreeks

## <a name="json-format"></a>JSON-indeling
    {
        "name": "SQLSPROCActivity",
        "description": "description", 
        "type": "SqlServerStoredProcedure",
        "inputs":  [ { "name": "inputtable"  } ],
        "outputs":  [ { "name": "outputtable" } ],
        "typeProperties":
        {
            "storedProcedureName": "<name of the stored procedure>",
            "storedProcedureParameters":  
            {
                "param1": "param1Value"
                …
            }
        }
    }

## <a name="json-properties"></a>JSON-eigenschappen

Eigenschap | Beschrijving | Vereist
-------- | ----------- | --------
naam | Naam van de activiteit | Ja
Beschrijving | Tekst met een beschrijving van wat de activiteit wordt gebruikt | Nee
type | SqlServerStoredProcedure | Ja
ingangen | Dit is optioneel. Als u een dataset invoer opgeeft, moet deze zijn beschikbaar (in de status "Gereed") voor de activiteit van de opgeslagen procedure uit te voeren. De invoer dataset kan niet worden verbruikt in de opgeslagen procedure als een parameter. Het wordt alleen gebruikt om de afhankelijkheid te controleren voordat u begint met de activiteit van de opgeslagen procedure. | Nee
uitgangen | Moet u een dataset uitvoer voor de activiteit van een opgeslagen procedure. Dataset uitvoer bevat het **schema** voor de opgeslagen procedure activiteit (per uur, wekelijks, maandelijks, enz.). <br/><br/>De dataset uitvoer moet een **service gekoppeld** die verwijst naar een Azure SQL-Database of een datawarehouse Azure SQL of een SQL Server-Database waarin u de opgeslagen procedure uit te voeren wilt gebruiken. <br/><br/>De dataset uitvoer kan dienen als een manier om aan te geven van het resultaat van de opgeslagen procedure voor latere verwerking door een andere activiteit ([activiteiten koppelen](data-factory-scheduling-and-execution.md#chaining-activities)) in de pijplijn. Wel schrijft Data Factory automatisch niet de uitvoer van een opgeslagen procedure met deze DataSet. Het is de opgeslagen procedure die u naar een SQL-tabel die de dataset uitvoer schrijft naar verwijst. <br/><br/>In sommige gevallen is de dataset uitvoer een **dataset dummy**, die alleen om een schema voor het uitvoeren van de activiteit van de opgeslagen procedure wordt gebruikt. | Ja
storedProcedureName | Geef de naam van de opgeslagen procedure in de Azure SQL-database of Azure SQL Data Warehouse wordt vertegenwoordigd door de gekoppelde service die gebruikmaakt van de uitvoertabel. | Ja
storedProcedureParameters | Geef waarden op voor de opgeslagen procedureparameters. Als u null voor een parameter doorgeven, gebruikt u de syntaxis: "param1": null (alle kleine letters). Zie de volgende voorbeelden voor meer informatie over het gebruik van deze eigenschap.| Nee

## <a name="passing-a-static-value"></a>Een statische waarde doorgeven 
Nu laten we eens een andere kolom met de naam 'Scenario' toe te voegen in de tabel een statische waarde met de naam 'Document voorbeeld'.

![Voorbeeldgegevens 2](./media/data-factory-stored-proc-activity/sample-data-2.png)

    CREATE PROCEDURE sp_sample @DateTime nvarchar(127) , @Scenario nvarchar(127)
    
    AS
    
    BEGIN
        INSERT INTO [sampletable]
        VALUES (newid(), @DateTime, @Scenario)
    END

Nu het Scenario-parameter en de waarde van de activiteit van de opgeslagen procedure doorgeven. De sectie typeProperties in het vorige voorbeeld ziet er in het volgende fragment:

    "typeProperties":
    {
        "storedProcedureName": "sp_sample",
        "storedProcedureParameters": 
        {
            "DateTime": "$$Text.Format('{0:yyyy-MM-dd HH:mm:ss}', SliceStart)",
            "Scenario": "Document sample"
        }
    }

