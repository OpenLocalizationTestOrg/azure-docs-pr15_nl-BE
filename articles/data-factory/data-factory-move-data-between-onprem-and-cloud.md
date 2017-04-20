<properties 
    pageTitle="Gegevens - Data Management Gateway | Microsoft Azure"
    description="Een gegevensgateway instellen om gegevens te verplaatsen tussen lokale en de cloud. Data Management Gateway in Azure Data Factory gebruiken om uw gegevens te verplaatsen." 
    keywords="gegevensgateway, integratie van gegevens, gegevens, gatewayreferenties verplaatsen"
    services="data-factory" 
    documentationCenter="" 
    authors="linda33wj" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/18/2016" 
    ms.author="jingwang"/>

# <a name="move-data-between-on-premises-sources-and-the-cloud-with-data-management-gateway"></a>Gegevens verplaatsen tussen bronnen op gebouwen en de cloud met Data Management Gateway
Dit artikel biedt een overzicht van gegevensintegratie tussen gegevensopslagplaatsen op gebouwen en cloud gegevensarchieven met Data Factory. Het is gebaseerd op het artikel [Gegevensverplaatsing activiteiten](data-factory-data-movement-activities.md) en andere data factory core concepten: [datasets](data-factory-create-datasets.md) en [pijpleidingen](data-factory-create-pipelines.md). 

## <a name="data-management-gateway"></a>Data Management Gateway
Data Management Gateway moet u installeren op uw computer op gebouwen verplaatsen van gegevens uit een gegevensarchief op gebouwen inschakelen. De gateway kan worden geïnstalleerd op dezelfde computer als de data store of op een andere computer als de gateway verbinding met het gegevensarchief maken kan. 

> [AZURE.IMPORTANT] Zie [Data Management Gateway](data-factory-data-management-gateway.md) -artikel voor meer informatie over Data Management Gateway.   

Het volgende scenario wordt beschreven hoe u een gegevens-fabriek met een pijpleiding die gegevens uit een **SQL Server** -database op ruimten naar een Azure blobopslag verplaatst maken. Als onderdeel van de procedure, installeren en configureren van de Data Management Gateway op de computer. 

## <a name="walkthrough-copy-on-premises-data-to-cloud"></a>Procedure: op ruimten gegevens kopiëren naar cloud
  
## <a name="create-data-factory"></a>Data factory maken
In deze stap gebruikt u de Azure portal een Azure Data Factory-instantie met de naam **ADFTutorialOnPremDF**te maken. 

1.  Log in op de [Azure portal](https://portal.azure.com). 
2.  Klik op **+ Nieuw** **Intelligence + analytics**, en op **Data Factory**.

    ![Nieuw -> DataFactory](./media/data-factory-move-data-between-onprem-and-cloud/NewDataFactoryMenu.png)  
2. Voer **ADFTutorialOnPremDF** voor de naam in de **nieuwe data factory** -blade.

    ![Toevoegen aan Startboard](./media/data-factory-move-data-between-onprem-and-cloud/OnPremNewDataFactoryAddToStartboard.png)

    > [AZURE.IMPORTANT] 
    > De naam van de fabriek Azure gegevens moet uniek zijn. Als het foutbericht: **Data factory naam 'ADFTutorialOnPremDF' is niet beschikbaar**, wijzig de naam van de fabriek van gegevens (bijvoorbeeld yournameADFTutorialOnPremDF) en probeer het opnieuw maken. Deze naam in plaats van ADFTutorialOnPremDF tijdens het uitvoeren van de overige stappen in deze zelfstudie gebruiken.
    > 
    > De naam van de fabriek gegevens kan worden geregistreerd als een **DNS-** naam in de toekomst en dus zichtbaar onlinebronnen.
3. Selecteer het **abonnement Azure** waar u de gegevens fabriek worden gemaakt. 
4.  Selecteer bestaande **bronnengroep** of een resourcegroep te maken. Maak een groep met de naam voor de zelfstudie: **ADFTutorialResourceGroup**. 
5.  Klik op **maken** op de **nieuwe data factory** -blade.

    > [AZURE.IMPORTANT] Data Factory om instanties te maken, moet u lid zijn van de rol van [Inzender fabriek](../active-directory/role-based-access-built-in-roles.md/#data-factory-contributor) op groepsniveau abonnement of een resource. 
11. Nadat het is gemaakt, ziet u de **Data Factory** blade zoals in de volgende afbeelding wordt getoond:

    ![Data Factory-startpagina](./media/data-factory-move-data-between-onprem-and-cloud/OnPremDataFactoryHomePage.png)

## <a name="create-gateway"></a>Gateway maken
5. Klik op in het blad **Data Factory** **auteur en implementeren van** naast elkaar voor het starten van de **Editor** voor de fabriek van gegevens.

    ![Ontwerpen en implementeren van tegel](./media/data-factory-move-data-between-onprem-and-cloud/author-deploy-tile.png) 
6.  In de Data Factory-Editor, klikt u op de **... Meer** op de werkbalk en klik vervolgens op **nieuwe gegevensgateway**. U kunt ook **Gegevensgateways** in de boomstructuurweergave met de rechtermuisknop en klik op **nieuwe gegevensgateway**. 

    ![Nieuwe gegevensgateway op werkbalk](./media/data-factory-move-data-between-onprem-and-cloud/NewDataGateway.png)
2. In het blad **maken** **adftutorialgateway** voor de **naam**, en klik op **OK**.    

    ![Blade Gateway maken](./media/data-factory-move-data-between-onprem-and-cloud/OnPremCreateGatewayBlade.png)
3. Klik op **installeren op deze computer rechtstreeks**in het blad **configureren** . Deze actie downloadt het installatiepakket voor de gateway, installeert, configureert en registreert de gateway op de computer.  

    > [AZURE.NOTE] 
    > Gebruik Internet Explorer of een compatibel ClickOnce Microsoft-webbrowser.
    > 
    > Als u chroom gebruikt, gaat u aan de [Chrome web store](https://chrome.google.com/webstore/)zoeken met het trefwoord 'ClickOnce', kies een van de extensies ClickOnce en installeren. 
    >  
    > Doe hetzelfde voor de Firefox (installatie-invoegtoepassing). Klik op de knop **Menu openen** op de werkbalk (**drie horizontale lijnen** in de rechterbovenhoek), klik op **invoegtoepassingen**met 'ClickOnce' trefwoord zoeken, kiest u een van de extensies ClickOnce en installeren.    

    ![Gateway - blade configureren](./media/data-factory-move-data-between-onprem-and-cloud/OnPremGatewayConfigureBlade.png)

    Op deze manier is de gemakkelijkste manier (met één muisklik) te downloaden, installeren, configureren en registreren van de gateway in één enkele stap. U kunt zien dat de toepassing **Microsoft Data Management Gateway Configuration Manager** op uw computer is geïnstalleerd. U kunt het uitvoerbare bestand **ConfigManager.exe** ook vinden in de map: **C:\Program Files\Microsoft Data Management Gateway\2.0\Shared**.

    U kunt ook downloaden en handmatig gateway installeren met behulp van de koppelingen in deze blade en registreren met behulp van de toets weergegeven in het tekstvak van de **Nieuwe sleutel** .
    
    Zie [Data Management Gateway](data-factory-data-management-gateway.md) artikel voor de details over de gateway.

    >[AZURE.NOTE] U moet een beheerder op de lokale computer installeren en configureren van de Data Management Gateway is. U kunt extra gebruikers toevoegen aan de groep lokale **Gebruikers gegevens Gateway** Windows. De leden van deze groep kunt het Data Management Gateway Configuration Manager-hulpprogramma voor het configureren van de gateway. 

5. Enkele minuten wachten of wachten totdat de volgende melding:

    ![Gateway installatie geslaagd](./media/data-factory-move-data-between-onprem-and-cloud/gateway-install-success.png) 
6. **Data Management Gateway Configuration Manager** -programma op uw computer starten. Typ in **het zoekvenster** **Data Management Gateway** voor toegang tot dit hulpprogramma. U kunt het uitvoerbare bestand **ConfigManager.exe** ook vinden in de map: **C:\Program Files\Microsoft Data Management Gateway\2.0\Shared** 

    ![Gateway Configuration Manager](./media/data-factory-move-data-between-onprem-and-cloud/OnPremDMGConfigurationManager.png)
6. Bevestig dat u ziet `adftutorialgateway is connected to the cloud service` bericht. De statusbalk onderaan wordt **verbonden met de cloud-service** samen met een **groen vinkje**weergegeven.

    Op het tabblad **Start** , kunt u de volgende verrichtingen doen: 
    - **Registreer** een gateway met een sleutel uit de Azure portal met behulp van de knop kassa. 
    - **Stop** de Service Data Management Gateway-Host op uw gateway-computer. 
    - **Schema-updates** worden geïnstalleerd op een bepaald tijdstip van de dag. 
    - Weergeven wanneer de gateway **voor het laatst bijgewerkt is**.
    - Geef de tijd waarop een update van de gateway kan worden geïnstalleerd. 

8. Ga naar het tabblad **Instellingen** . Het certificaat dat is opgegeven in de sectie van het **certificaat** wordt gebruikt om de referenties voor de gegevensopslag op de locatie die u op de portal opgeeft voor coderen/decoderen. (optioneel) Klik op **wijzigen** om uw eigen certificaat gebruiken. De gateway standaard in het certificaat dat automatisch wordt gegenereerd door de service Data Factory.

    ![Configuratie van gateway certificaten](./media/data-factory-move-data-between-onprem-and-cloud/gateway-certificate.png)

    U kunt ook de volgende acties op het tabblad **Instellingen** doen: 
    - Weergeven of exporteren van het certificaat dat door de gateway wordt gebruikt.
    - Wijzig het HTTPS-eindpunt die door de gateway wordt gebruikt.    
    - Een HTTP-proxy moet worden gebruikt door de gateway instellen.   
9. (optioneel) Ga naar het tabblad **Diagnostische gegevens** , schakel het selectievakje **Logboekregistratie inschakelen** als u uitgebreide logboekregistratie kunt u eventuele problemen oplossen met de gateway inschakelen. Gegevens in het logboek vindt u in **Logboeken** in **Logboeken toepassingen en Services** -> **Data Management Gateway** knooppunt. 

    ![Tabblad Diagnostische gegevens](./media/data-factory-move-data-between-onprem-and-cloud/diagnostics-tab.png)

    U kunt ook de volgende acties uitvoeren op het tabblad **Diagnostische gegevens** : 
    
    - **Verbinding testen** sectie gebruiken om een gegevensbron op ruimten is via de gateway.
    - Klik op **Logboeken weergeven** als u wilt zien van het logboek gegevens Management Gateway in een venster Logboeken. 
    - Klik op **Verzenden logboeken** met Logboeken van de afgelopen zeven dagen in een zip-bestand te uploaden naar Microsoft te vergemakkelijken met het oplossen van problemen met uw. 
10. Op het tabblad **Diagnostische gegevens** in de sectie **Verbinding testen** **SQL Server** voor het type van de data store selecteert, voert u de naam van de databaseserver, de naam van de database verificatietype opgeven, geef gebruikersnaam en wachtwoord en klik op **testen** om te controleren of de gateway verbinding met de database maken kan. 
11. Ga naar de webbrowser, en in de **portal Azure**, klikt u op **OK** op het blad **configureren** en klik vervolgens op de **nieuwe gegevensgateway** blade.
6. Er **adftutorialgateway** onder **Gegevensgateways** in de boomstructuur aan de linkerkant.  Als u erop klikt, ziet u de bijbehorende JSON. 
    

## <a name="create-linked-services"></a>Gekoppelde services maken 
In deze stap maakt u twee gekoppelde services: **AzureStorageLinkedService** en **SqlServerLinkedService**. De **SqlServerLinkedService** koppelt een SQL Server-database op de gebouwen en de **AzureStorageLinkedService** gekoppelde service koppelingen een Azure blob-opslag gegevens fabriek. U kunt een pijpleiding maken verderop in dit overzicht die gegevens opgehaald uit de SQL Server-database op de gebouwen aan de winkel Azure blob. 

#### <a name="add-a-linked-service-to-an-on-premises-sql-server-database"></a>Een gekoppelde service toevoegen aan een SQL Server-database op de gebouwen
1.  In de **Data Factory-Editor**, klikt u op **nieuwe gegevens opslaan** op de werkbalk en selecteer **SQL-Server**. 

    ![Nieuwe gekoppelde SQL-Server-service](./media/data-factory-move-data-between-onprem-and-cloud/NewSQLServer.png) 
3.  Voer de volgende stappen uit in de **JSON-editor** aan de rechterkant: 
    1. Geef de **adftutorialgateway**voor de **gatewayName**. 
    2. Voer de volgende stappen uit in de **connectionString**: 
        1. Voer voor de **servernaam**, de naam van de server waarop de SQL Server-database.
        2. Voer voor de **databasenaam**, de naam van de database.
        3. Klik op de knop **coderen** op de werkbalk. Deze downloads en start de toepassing van de Manager van referenties.
        
            ![Referenties Manager application](./media/data-factory-move-data-between-onprem-and-cloud/credentials-manager-application.png)
        5. Verificatietype, gebruikersnaam en wachtwoord opgeven in het dialoogvenster **Referenties instellen** en klik op **OK**. Als de verbinding voltooid is, worden de gecodeerde referenties zijn opgeslagen in de JSON en het dialoogvenster wordt gesloten. 
        6. Sluit de browser leeg tabblad dat in het dialoogvenster wordt geopend als niet automatisch gesloten en terug te gaan naar het tabblad met de Azure portal. 
  
            Op de gateway-computer deze referenties zijn **gecodeerd** met een certificaat waarin de Data Factory-service. Als u wilt dat het certificaat dat is gekoppeld aan de Data Management Gateway in plaats daarvan gebruiken, Zie [referenties veilig te stellen](#set-credentials-and-security).    
    1.  Klik op **Deploy** op de werkbalk voor de implementatie van de service SQL Server is gekoppeld. Hier ziet u de gekoppelde service in de structuurweergave. 
        
        ![De service SQL Server gekoppeld in de overzichtsweergave](./media/data-factory-move-data-between-onprem-and-cloud/sql-linked-service-in-tree-view.png)  

#### <a name="add-a-linked-service-for-an-azure-storage-account"></a>Een gekoppelde service voor een account Azure opslag toevoegen
 
1. In de **Editor voor Data Factory**, **nieuwe gegevens worden opgeslagen** op de opdrachtbalk en klik op **Azure opslag**.
2. De naam van uw account Azure opslag voor de **accountnaam**invoeren.
3. Typ de code voor uw account Azure opslag voor de **sleutel van de Account**.
4. Klik op **Deploy** om de **AzureStorageLinkedService**te implementeren.
   
 
## <a name="create-datasets"></a>Gegevenssets maken
In deze stap maakt input en output datasets die invoer en uitvoer van gegevens voor de kopieerbewerking vertegenwoordigen (On-premises SQL Server-database = > Azure blob-opslag). Gegevenssets maken, voordat u de volgende handelingen (gedetailleerde stappen volgt de lijst):

- Maak een tabel met de naam **emp** in de SQL Server-Database die u als een gekoppelde service toegevoegd aan de fabriek van gegevens en een aantal voorbeelden van vermeldingen in de tabel invoegen.
- Een blob-container met de naam **adftutorial** in de Azure blob opslag-account die u als een gekoppelde service toegevoegd aan de fabriek van de gegevens maken.

### <a name="prepare-on-premises-sql-server-for-the-tutorial"></a>On-premises SQL Server voorbereiden voor de zelfstudie

1. In de database die u hebt opgegeven voor de SQL-Server op locatie gekoppelde service (**SqlServerLinkedService**) de volgende SQL-script gebruiken voor het maken van de **emp** -tabel in de database.

        CREATE TABLE dbo.emp
        (
            ID int IDENTITY(1,1) NOT NULL, 
            FirstName varchar(50),
            LastName varchar(50),
            CONSTRAINT PK_emp PRIMARY KEY (ID)
        )
        GO 
2. Sommige voorbeelden in de tabel invoegen: 

        INSERT INTO emp VALUES ('John', 'Doe')
        INSERT INTO emp VALUES ('Jane', 'Doe')

### <a name="create-input-dataset"></a>Invoer gegevensset maken

1. Klik in de **Editor voor Data Factory**, **... Meer**, klikt u op **nieuwe dataset** op de opdrachtbalk en klik op **SQL Server-tabel**. 
2.  De JSON in het rechterdeelvenster met de volgende tekst vervangen:
        
            {       
                "name": "EmpOnPremSQLTable",
                "properties": {
                    "type": "SqlServerTable",
                    "linkedServiceName": "SqlServerLinkedService",
                    "typeProperties": {
                        "tableName": "emp"
                    },
                    "external": true,
                    "availability": {
                        "frequency": "Hour",
                        "interval": 1
                    },
                    "policy": {
                        "externalData": {
                            "retryInterval": "00:01:00",
                            "retryTimeout": "00:10:00",
                            "maximumRetry": 3
                        }
                    }
                }
            }    

    Houd rekening met de volgende punten: 

    - **type** is ingesteld op **SqlServerTable**.
    - **emp**is **tableName** ingesteld.
    - **linkedServiceName** is ingesteld op **SqlServerLinkedService** (u kunt deze service gekoppelde had gemaakt eerder in deze procedure.).
    - Voor een input-dataset die is gegenereerd door een andere pijpleiding in Azure Data Factory, moet u **extern** op **true**ingesteld. Het betekent dat de ingevoerde gegevens buiten de service Azure Data Factory wordt geproduceerd. Desgewenst kunt u externe gegevens met behulp van het element **externalData** in de sectie **beleid** beleid.    

    Zie [gegevens van SQL Server](data-factory-sqlserver-connector.md) voor meer informatie over eigenschappen van JSON.
2. Klik op **Deploy** op de werkbalk voor de implementatie van de dataset.  


### <a name="create-output-dataset"></a>Uitvoer-gegevensset maken

1.  In de **Data Factory-Editor**, klikt u op **nieuwe dataset** op de opdrachtbalk en op **Azure Blob-opslag**.
2.  De JSON in het rechterdeelvenster met de volgende tekst vervangen: 

            {
                "name": "OutputBlobTable",
                "properties": {
                    "type": "AzureBlob",
                    "linkedServiceName": "AzureStorageLinkedService",
                    "typeProperties": {
                        "folderPath": "adftutorial/outfromonpremdf",
                        "format": {
                            "type": "TextFormat",
                            "columnDelimiter": ","
                        }
                    },
                    "availability": {
                        "frequency": "Hour",
                        "interval": 1
                    }
                }
            }
  
    Houd rekening met de volgende punten: 
    
    - **type** is ingesteld op **AzureBlob**.
    - **linkedServiceName** is ingesteld op **AzureStorageLinkedService** (u kunt deze service gekoppelde had gemaakt in stap 2).
    - **mappad** is ingesteld op **adftutorial/outfromonpremdf** , waarbij outfromonpremdf staat voor de map in de container adftutorial. De container **adftutorial** maken als deze nog niet bestaat. 
    - De **beschikbaarheid** is ingesteld op **elk uur** (**frequentie** ingesteld op **uur** en **interval** is ingesteld op **1**).  De service Data Factory genereert een segment output elk uur in de tabel **emp** in de Azure SQL-Database. 

    Als u een **bestandsnaam** voor een **uitvoertabel**niet opgeeft, wordt de gegenereerde bestanden in het **mappad** heten in de volgende indeling: gegevens. <Guid>.txt (bijvoorbeeld:: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt.).

    **Mappad** en de **bestandsnaam** dynamisch op basis van de **SliceStart** -tijd instellen, gebruikt u de eigenschap partitionedBy. In het volgende voorbeeld folderPath maakt gebruik van jaar, maand en dag van de SliceStart (begintijd van het segment wordt verwerkt) en bestandsnaam gebruikt uur vanaf de SliceStart. Bijvoorbeeld, als een segment wordt geproduceerd voor 2014-10-20T08:00:00, de mapnaam wordt ingesteld op wikidatagateway/wikisampledataout/2014/10/20 en de bestandsnaam ingesteld op 08.csv. 

        "folderPath": "wikidatagateway/wikisampledataout/{Year}/{Month}/{Day}",
        "fileName": "{Hour}.csv",
        "partitionedBy": 
        [
            { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
            { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } }, 
            { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } }, 
            { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "hh" } } 
        ],

 
    Zie [gegevens van Azure Blob-opslag verplaatsen](data-factory-azure-blob-connector.md) voor meer informatie over eigenschappen van JSON.
2.  Klik op **Deploy** op de werkbalk voor de implementatie van de dataset. Bevestig dat u ziet dat zowel de gegevenssets in de structuurweergave.  

## <a name="create-pipeline"></a>Pipeline maken
In deze stap maakt u een **pijpleiding** met één **Kopie activiteit** die wordt **EmpOnPremSQLTable** als invoer gebruikt en **OutputBlobTable** als uitvoer.

1.  In Data Factory-Editor, klikt u op de **... Meer**, en klik op **nieuwe pijpleiding**. 
2.  De JSON in het rechterdeelvenster met de volgende tekst vervangen: 
    
            {
                "name": "ADFTutorialPipelineOnPrem",
                "properties": {
                "description": "This pipeline has one Copy activity that copies data from an on-prem SQL to Azure blob",
                "activities": [
                {
                    "name": "CopyFromSQLtoBlob",
                    "description": "Copy data from on-prem SQL server to blob",
                    "type": "Copy",
                    "inputs": [
                    {
                        "name": "EmpOnPremSQLTable"
                    }
                    ],
                    "outputs": [
                    {
                        "name": "OutputBlobTable"
                      }
                    ],
                    "typeProperties": {
                      "source": {
                        "type": "SqlSource",
                        "sqlReaderQuery": "select * from emp"
                      },
                      "sink": {
                        "type": "BlobSink"
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
                "start": "2016-07-05T00:00:00Z",
                "end": "2016-07-06T00:00:00Z",
                "isPaused": false
              }
            }

    > [AZURE.IMPORTANT]
    > Vervang de waarde van de eigenschap **start** met de huidige waarde voor de **dag- en** met de volgende dag.

    Houd rekening met de volgende punten:
 
    - In de sectie activiteiten is er enige activiteit waarvan het **type** is ingesteld op **kopiëren**.
    - **Input** voor de activiteit is ingesteld op **EmpOnPremSQLTable** en **uitvoer** voor de activiteit is ingesteld op **OutputBlobTable**.
    - **SqlSource** als het **type gegevensbron** is opgegeven in de sectie **typeProperties** en **BlobSink **is opgegeven als het **type wastafel**.
    - SQL-query `select * from emp` is opgegeven voor de eigenschap **sqlReaderQuery** van **SqlSource**.

     Beide start en eind datum/tijd in de [ISO-notatie](http://en.wikipedia.org/wiki/ISO_8601)moet zijn. Bijvoorbeeld: 2014-10-14T16:32:41Z. **De eindtijd** is optioneel, maar we gebruiken in deze zelfstudie. 
    
    Als u geen waarde voor de eigenschap **end** opgeeft, wordt deze berekend als '**start + 48 uur**'. Om de pijplijn voor onbepaalde tijd worden uitgevoerd, geeft u **9/9/9999** als de waarde voor de eigenschap **end** . 
    
    U definieert de tijdsduur waarin de segmenten van de gegevens worden verwerkt op basis van de **beschikbaarheid van** eigenschappen die zijn gedefinieerd voor elke gegevensset Azure Data Factory.
    
    In het voorbeeld zijn er 24 gegevens segmenten elk segment per uur wordt geproduceerd.     
2. Klik op **Deploy** op de opdrachtbalk dataset implementeren (tabel is een rechthoekig dataset). Bevestig dat de pijpleiding wordt weergegeven in de boomstructuur onder het knooppunt van **pijpleidingen** .  
5. Klik nu op **X** tweemaal om te sluiten van de bladen die u terug naar de blade **Data Factory** voor de **ADFTutorialOnPremDF**.

**Gefeliciteerd!** Je hebt een Azure data factory, gekoppelde services datasets en een pijpleiding gemaakt en gepland van de pijpleiding.

#### <a name="view-the-data-factory-in-a-diagram-view"></a>De fabriek van de gegevens weergeven in een Diagram weergeven 
1. Klik op **Diagram** naast elkaar op de introductiepagina voor de fabriek van **ADFTutorialOnPremDF** data in **Azure portal**. :

    ![Diagram koppeling](./media/data-factory-move-data-between-onprem-and-cloud/OnPremDiagramLink.png)

2. Het diagram lijkt op de volgende afbeelding wordt weergegeven:

    ![Diagram weergeven](./media/data-factory-move-data-between-onprem-and-cloud/OnPremDiagramView.png)

    U kunt inzoomen, uitzoomen, in-en uitzoomen op 100%, in-/ uitzoomen past automatisch pijpleidingen en datasets en informatie lineage (upstream en downstream artikelen gemarkeerd van geselecteerde items).  U kunt een object (invoer/uitvoer-dataset of pijpleiding) voor een overzicht van eigenschappen voor het dubbelklikken. 

## <a name="monitor-pipeline"></a>Monitor-pipeline
In deze stap kunt u de Azure portal gebruiken om te controleren wat er gebeurt in een fabriek Azure gegevens. U kunt ook PowerShell-cmdlets gebruiken om gegevenssets en pijpleidingen te bewaken. Zie voor meer informatie over het controleren, [controleren en beheren van pijpleidingen](data-factory-monitor-manage-pipelines.md).

5. Dubbelklik op **EmpOnPremSQLTable**in het diagram.  

    ![EmpOnPremSQLTable-segmenten](./media/data-factory-move-data-between-onprem-and-cloud/OnPremSQLTableSlicesBlade.png)

6. U ziet dat alle gegevens van segmenten in **Gereed** staat omdat de duur van de pijpleiding (Begintijd Eindtijd) in het verleden. Het is ook omdat u de gegevens hebt ingevoerd in de SQL Server-database en er altijd is. Zorg ervoor dat geen segmenten worden weergegeven in de sectie **probleem segmenten** aan de onderkant. Alle segmenten, klikt u op **Meer** onder aan de lijst van segmenten. 
7. Klik nu In het blad **Datasets** op **OutputBlobTable**.

    ![OputputBlobTable-segmenten](./media/data-factory-move-data-between-onprem-and-cloud/OutputBlobTableSlicesBlade.png)
9. Klik op een segment uit de lijst en ziet u het **Segment** blade. U ziet de activiteit wordt uitgevoerd voor het segment. Er is slechts één activiteit gewoonlijk uitgevoerd.  

    ![Gegevens segment Blade](./media/data-factory-move-data-between-onprem-and-cloud/DataSlice.png)

    Als het segment niet in de toestand **Gereed** is, ziet u de upstream segmenten die niet klaar zijn en wordt het huidige segment wordt uitgevoerd in de lijst **Upstream segmenten die niet gereed zijn** geblokkeerd.

10. Klik op de **activiteit uitvoeren** in de lijst onderaan Zie **details activiteit worden uitgevoerd**.

    ![Activiteit uitvoeren Details blade](./media/data-factory-move-data-between-onprem-and-cloud/ActivityRunDetailsBlade.png)

    Hier ziet u informatie zoals doorvoer, de duur en de gateway die wordt gebruikt voor de gegevensoverdracht. 
11. Klik op **X** om te sluiten van alle bladen totdat u 
12. terug te gaan naar het huis blad voor de **ADFTutorialOnPremDF**.
14. (optioneel) Klik op **pijpleidingen**en detailanalyse invoertabellen (**verbruikt**) of uitvoer datasets (**geproduceerde**) op **ADFTutorialOnPremDF**.
15. Hulpprogramma's zoals gebruik hulpprogramma's zoals [Microsoft Opslagverkenner](http://storageexplorer.com/) gebruiken om te controleren of een blob of het bestand is gemaakt voor elk uur.

    ![Azure Opslagverkenner](./media/data-factory-move-data-between-onprem-and-cloud/OnPremAzureStorageExplorer.png)

## <a name="next-steps"></a>Volgende stappen

- Zie [Data Management Gateway](data-factory-data-management-gateway.md) artikel voor de details over de Data Management Gateway.
- [Gegevens kopiëren van Azure Blob Azure SQL](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) voor meer informatie over de activiteit van de kopie gebruiken om gegevens te verplaatsen vanuit een bron-gegevensarchief naar een gegevensarchief sink zien. 
