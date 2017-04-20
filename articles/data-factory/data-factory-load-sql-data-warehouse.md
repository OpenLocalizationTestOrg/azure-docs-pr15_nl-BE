<properties 
    pageTitle="Terabytes aan gegevens in SQL Data Warehouse laden | Microsoft Azure" 
    description="Laat zien hoe 1 TB aan gegevens kunnen worden geladen in Azure SQL Data Warehouse in 15 minuten met Azure Data Factory" 
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
    ms.date="10/28/2016" 
    ms.author="jingwang"/>

# <a name="load-1-tb-into-azure-sql-data-warehouse-under-15-minutes-with-azure-data-factory-copy-wizard"></a>1 TB in Azure SQL Data Warehouse met Azure Data Factory [Wizard kopiëren] onder 15 minuten laden
[Azure SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md) is een cloud-gebaseerde, schalen database kunnen grote hoeveelheden relationele en niet-relationele gegevens verwerken.  Gebaseerd op de architectuur massively parallel processing (MPP) en is SQL Data Warehouse geoptimaliseerd voor enterprise data magazijn werkbelasting.  Wolk elasticiteit biedt de flexibiliteit om te schalen, opslag en onafhankelijk van elkaar te berekenen.

Aan de slag met Azure SQL Data Warehouse is nu eenvoudiger dan ooit met **Azure Data Factory**.  Azure Data Factory is een volledig beheerde cloud-gebaseerde integratie-gegevensservice kan worden gebruikt voor het vullen van een SQL-datawarehouse met de gegevens van uw bestaande systeem en zodat u kostbare tijd tijdens het evalueren van SQL Data Warehouse en uw analytics oplossingen die erop.  Hier volgen de belangrijkste voordelen van het laden van gegevens in Azure SQL Data Warehouse met Azure Data Factory:

- **Gemakkelijk te installeren**: intuïtieve wizard 5 stap met geen scripts nodig zijn.
- **Ondersteuning voor rijke gegevensarchief**: ingebouwde ondersteuning voor een groot aantal lokale en cloud-gebaseerde gegevensopslag.
- **Veilige en compatibele**: gegevens worden overgebracht via HTTPS- of ExpressRoute en wereldwijde service aanwezigheid garandeert uw gegevens nooit verlaat de geografische begrenzing
- **Ongeëvenaarde prestaties door middel van PolyBase** – met behulp van Polybase is de meest efficiënte manier om gegevens te verplaatsen naar Azure SQL Data Warehouse. De tijdelijke blob-functie kunt u hoge belasting snelheden van alle soorten gegevensopslaglocaties naast Azure Blob-opslag die de Polybase standaard ondersteunt bereiken.

In dit artikel wordt beschreven hoe u Data Factory-Wizard kopiëren met 1 TB aan gegevens laden van Azure Blob-opslag in Azure SQL Data Warehouse onder 15 minuten op de doorvoer van meer dan 1,2 GBps.

Dit artikel bevat stapsgewijze instructies voor het verplaatsen van gegevens met behulp van de Wizard kopiëren in Azure SQL Data Warehouse. 

> [AZURE.NOTE] Zie artikel [verplaatsen van gegevens van en naar Azure SQL Data Warehouse Azure Data Factory gebruiken](data-factory-azure-sql-data-warehouse-connector.md) voor algemene informatie over de mogelijkheden van Data Factory bij het verplaatsen van gegevens naar/van Azure SQL Data Warehouse. 
> 
> U kunt ook samenstellen met Azure portal, Visual Studio, PowerShell, pijpleidingen enz. Zie [Zelfstudie: gegevens kopiëren vanuit Azure Blob met Azure SQL-Database](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) voor een snelle scenario met stapsgewijze instructies voor het gebruik van de activiteit van de kopie in Azure Data Factory.  

## <a name="prerequisites"></a>Vereisten
- Azure Blob-opslag: dit experiment gebruikt voor het opslaan van de TPC-H testen dataset Azure Blob-opslag (GRS).  Als u niet een account Azure opslag, informatie over [het maken van een account voor opslag](../storage/storage-create-storage-account.md#create-a-storage-account).
- Gegevens van de [TPC-H](http://www.tpc.org/tpch/) : we gaan TPC-H als de dataset testen gebruiken.  Hiervoor moet u `dbgen` van de TPC-H-toolkit die u helpt bij het genereren van de dataset.  Kunt u de broncode voor downloaden `dbgen` [TPC's](http://www.tpc.org/tpc_documents_current_versions/current_specifications.asp) van uzelf compileren en het gecompileerde binaire bestand downloaden van [GitHub](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/TPCHTools).  Dbgen.exe worden uitgevoerd met de volgende opdrachten voor het genereren van 1 TB plat bestand voor `lineitem` tabel spread over 10 bestanden:
    - `Dbgen -s 1000 -S **1** -C 10 -T L -v`
    - `Dbgen -s 1000 -S **2** -C 10 -T L -v`
    - …
    - `Dbgen -s 1000 -S **10** -C 10 -T L -v` 

    Nu de gegenereerde bestanden kopiëren voor Azure Blob.  Verwijzen naar [gegevens verplaatsen naar en van een bestandssysteem in ruimten met behulp van Azure Data Factory](data-factory-onprem-file-system-connector.md) voor die kopie van de automatische Documentinvoer.   
- Azure SQL datawarehouse: dit experiment laadt gegevens in Azure SQL Data Warehouse gemaakt met 6.000 DWUs

    Verwijzen naar [een datawarehouse Azure SQL maken](../sql-data-warehouse/sql-data-warehouse-get-started-provision/) voor gedetailleerde instructies voor het maken van een SQL Data Warehouse-database.  Als u de best mogelijke belasting prestaties in SQL Data Warehouse Polybase gebruiken, kiest u maximum aantal magazijn eenheden (DWUs) in de prestatie-instelling 6.000 DWUs is toegestaan.

    > [AZURE.NOTE] 
    > Bij het laden van Azure Blob, is het laden van de prestaties van gegevens rechtevenredig met het aantal DWUs dat u op het datawarehouse SQL:
    > 
    > 1 TB in 1000 laden duurt DWU SQL Data Warehouse 87min (~ 200MBps doorvoer) 1 TB in 2.000 laden duurt DWU SQL Data Warehouse 46min (~ 380MBps doorvoer) 1 TB in 6.000 laden duurt DWU SQL Data Warehouse 14min (~1.2GBps doorvoer) 

    Een datawarehouse SQL met 6.000 DWUs verplaatst de schuifregelaar prestaties naar rechts:

    ![Schuifregelaar voor prestaties](media/data-factory-load-sql-data-warehouse/performance-slider.png)

    Voor een bestaande database die niet is geconfigureerd met 6.000 DWUs, kunt u deze schalen met behulp van Azure portal.  Ga naar de database in Azure portal en bevindt zich een knop **schaal** in het deelvenster **Overzicht** is weergegeven in de volgende afbeelding:

    ![Knop schaal](media/data-factory-load-sql-data-warehouse/scale-button.png)    

    Sleep de schuifregelaar om de maximale waarde klikt u op de **schaal** om het volgende deelvenster te openen en klik op de knop **Opslaan** .

    ![Dialoogvenster schaal](media/data-factory-load-sql-data-warehouse/scale-dialog.png)
    
    Dit experiment gegevens laadt in Azure SQL Data Warehouse gebruiken `xlargerc` resource class.

    Voor de best mogelijke doorvoer kopie moet worden uitgevoerd met behulp van een SQL Data Warehouse-gebruiker die deel uitmaken van `xlargerc` resource class.  Informatie over hoe u dat kunt doen door het volgende [voorbeeld van een gebruiker resources klasse wijzigen](../sql-data-warehouse/sql-data-warehouse-develop-concurrency.md#change-a-user-resource-class-example).  

- Tabelschema bestemming in Azure SQL Data Warehouse-database maken met de volgende DDL-instructie:

        CREATE TABLE [dbo].[lineitem]
        (
            [L_ORDERKEY] [bigint] NOT NULL,
            [L_PARTKEY] [bigint] NOT NULL,
            [L_SUPPKEY] [bigint] NOT NULL,
            [L_LINENUMBER] [int] NOT NULL,
            [L_QUANTITY] [decimal](15, 2) NULL,
            [L_EXTENDEDPRICE] [decimal](15, 2) NULL,
            [L_DISCOUNT] [decimal](15, 2) NULL,
            [L_TAX] [decimal](15, 2) NULL,
            [L_RETURNFLAG] [char](1) NULL,
            [L_LINESTATUS] [char](1) NULL,
            [L_SHIPDATE] [date] NULL,
            [L_COMMITDATE] [date] NULL,
            [L_RECEIPTDATE] [date] NULL,
            [L_SHIPINSTRUCT] [char](25) NULL,
            [L_SHIPMODE] [char](10) NULL,
            [L_COMMENT] [varchar](44) NULL
        )
        WITH
        (
            DISTRIBUTION = ROUND_ROBIN,
            CLUSTERED COLUMNSTORE INDEX
        )

Met de vereiste stappen voltooid, zijn we nu klaar voor het configureren van de kopie activiteit met behulp van de Wizard kopiëren.

## <a name="launch-copy-wizard"></a>Wizard kopiëren starten

1.  Log in op de [Azure portal](https://portal.azure.com).
2.  Klik op **+ Nieuw** vanaf de linkerbovenhoek **Intelligence + analytics**, en op **Data Factory**. 
6. In de **nieuwe data factory** blade:
    1. **LoadIntoSQLDWDataFactory** voor de **naam**invoeren.
        De naam van de fabriek Azure gegevens moet uniek zijn. Als het foutbericht: **Data factory naam 'LoadIntoSQLDWDataFactory' is niet beschikbaar**, wijzig de naam van de fabriek van gegevens (bijvoorbeeld yournameLoadIntoSQLDWDataFactory) en probeer het opnieuw maken. Zie [Data Factory - regels voor naamgeving van](data-factory-naming-rules.md) onderwerp voor naamgevingsregels voor Data Factory artefacten.  
     
    2. Selecteer uw Azure **abonnement**.
    3. Voor de resourcegroep, voer een van de volgende stappen uit: 
        1. Selecteer **bestaande gebruiken** om een bestaande resourcegroep selecteren.
        2. Selecteer **Nieuw** om een naam invoeren voor een resourcegroep.
    3. Selecteer een **locatie** voor de fabriek van gegevens.
    4. Schakel het selectievakje **pincode aan dashboard** onderaan het blad.  
    5. Klik op **maken**.
10. Nadat het is gemaakt, ziet u de **Data Factory** blade zoals in de volgende afbeelding wordt getoond:

    ![Data factory-startpagina](media/data-factory-load-sql-data-warehouse/data-factory-home-page-copy-data.png)
11. Klik op de tegel **gegevens kopiëren** als u wilt **Kopiëren, Wizard**starten op de introductiepagina Data Factory. 

    > [AZURE.NOTE] Als de webbrowser vastzit op 'Machtigen...' wordt weergegeven, **cookies van derden blokkeren en sitegegevens** instelling uitschakelen/uitschakelen (of) blijft ingeschakeld en een uitzondering maken voor **login.microsoftonline.com** en probeer het opnieuw starten van de wizard.


## <a name="step-1-configure-data-loading-schedule"></a>Stap 1: Gegevens laden planning configureren
De eerste stap is het configureren van het schema te laden van gegevens.  

In de pagina **Eigenschappen** :
1. **CopyFromBlobToAzureSqlDataWarehouse** voor de **naam van de taak** invoeren
2. De optie **nu eenmaal uitvoeren** .   
3. Klik op **volgende**.  

![Wizard - pagina Eigenschappen kopiëren](media/data-factory-load-sql-data-warehouse/copy-wizard-properties-page.png)

## <a name="step-2-configure-source"></a>Stap 2: Gegevensbron configureren
In deze sectie worden de stappen voor het configureren van de bron: Azure Blob met de 1 TB TPC-H regel item bestanden.

Selecteer de **Azure Blob-opslag** zoals de gegevens worden opgeslagen en klik op **volgende**.

![Wizard - pagina Selecteer bron kopiëren](media/data-factory-load-sql-data-warehouse/select-source-connection.png)

Vul de verbindingsgegevens voor de account Azure Blob-opslag en klik op **volgende**.

![Wizard - informatie van de gegevensbronverbinding te kopiëren](media/data-factory-load-sql-data-warehouse/source-connection-info.png)

Kies de **map** met de bestanden TPC-H artikel op de regel en klik op **volgende**.

![De Wizard kopiëren - invoer map selecteren](media/data-factory-load-sql-data-warehouse/select-input-folder.png)

Nadat u op **volgende**klikt, worden de opmaakinstellingen bestand automatisch gedetecteerd.  Controleer of dit scheidingsteken kolom ' | 'in plaats van de komma standaard','.  Klik op **volgende** nadat u de gegevens hebt bekeken.

![Wizard - opmaakinstellingen kopiëren](media/data-factory-load-sql-data-warehouse/file-format-settings.png)

## <a name="step-3-configure-destination"></a>Stap 3: Bestemming configureren
In deze sectie ziet u hoe u de bestemming: `lineitem` tabel in de database Azure SQL Data Warehouse.

**Azure SQL Data Warehouse** als de bestemming winkel en klik op **volgende**.

![Wizard voor kopiëren - gegevensarchief bestemming selecteren](media/data-factory-load-sql-data-warehouse/select-destination-data-store.png)

Vul de verbindingsgegevens voor Azure SQL Data Warehouse.  Controleer of u de gebruiker die lid is van de rol van `xlargerc` (Zie de sectie **vereisten** voor gedetailleerde instructies), en klik op **volgende**. 

![Wizard - verbindingsgegevens bestemming voor kopiëren](media/data-factory-load-sql-data-warehouse/destination-connection-info.png)

Kies de doeltabel en klik op **volgende**.

![Wizard - tabel toewijzingspagina kopiëren](media/data-factory-load-sql-data-warehouse/table-mapping-page.png)

Accepteer de standaardinstellingen voor de kolomtoewijzing en klik op **volgende**.

![Wizard - schema pagina kopiëren](media/data-factory-load-sql-data-warehouse/schema-mapping.png)


## <a name="step-4-performance-settings"></a>Stap 4: Prestatie-instellingen

**Polybase toestaan** is standaard ingeschakeld.  Klik op **volgende**.

![Wizard - schema pagina kopiëren](media/data-factory-load-sql-data-warehouse/performance-settings-page.png)

## <a name="step-5-deploy-and-monitor-load-results"></a>Stap 5: Implementatie en resultaten laden controleren
Klik op de knop **Voltooien** om te implementeren. 

![Wizard - pagina kopiëren](media/data-factory-load-sql-data-warehouse/summary-page.png)

Nadat de installatie voltooid is, klikt u op `Click here to monitor copy pipeline` voor het controleren van de voortgang uitvoeren kopiëren.

Selecteer de kopie pijpleiding die u hebt gemaakt in de lijst **Windows activiteit** .

![Wizard - pagina kopiëren](media/data-factory-load-sql-data-warehouse/select-pipeline-monitor-manage-app.png)

U kunt de kopie uitvoeren van gegevens in het **Venster Explorer van activiteit** in het rechterdeelvenster, met inbegrip van het gegevensvolume lezen van bron en geschreven naar de bestemming, de duur en de gemiddelde doorvoer voor de verwerking bekijken.

Zoals u in de volgende schermafdruk zien kunt, duurde kopiëren van 1 TB van Azure Blob-opslag in SQL Data Warehouse effectief bereiken van 1,22 GBps doorvoer 14 minuten!

![Wizard - geslaagd dialoogvenster kopiëren](media/data-factory-load-sql-data-warehouse/succeeded-info.png)

## <a name="best-practices"></a>Aanbevolen procedures
Hier volgen een paar tips om uw Azure SQL Data Warehouse-database uit te voeren:

- Een grotere bronklasse gebruiken bij het laden van een COLUMNSTORE GECLUSTERDE INDEX.
- Voor een efficiëntere joins, kunt u overwegen hash-distributie door een kolom selecteren in plaats van standaard round robin distributie.
- Voor sneller laden snelheid, kunt u overwegen heap voor tijdelijke gegevens.
- Statistieken maken als u klaar bent met het laden van Azure SQL Data Warehouse.

Zie [Aanbevolen procedures voor Azure SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-best-practices.md) voor meer informatie. 

## <a name="next-steps"></a>Volgende stappen
- [Wizard kopiëren van data Factory](data-factory-copy-wizard.md) - dit artikel bevat informatie over de Wizard kopiëren. 
- [Kopie activiteit prestaties en tuning guide](data-factory-copy-activity-performance.md) - dit artikel bevat de referentiemetingen voor prestaties en tuning guide.

