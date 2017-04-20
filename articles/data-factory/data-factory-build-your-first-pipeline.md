<properties
    pageTitle="Data Factory zelfstudie: eerste data pijpleiding | Microsoft Azure"
    description="Deze zelfstudie Azure Data Factory wordt beschreven hoe u maken en plannen van een fabriek voor gegevens die met behulp van script component op een cluster Hadoop gegevens verwerkt."
    services="data-factory"
    keywords="Azure data factory zelfstudie, hadoop-cluster hadoop component"
    documentationCenter=""
    authors="spelluru"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="data-factory"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article" 
    ms.date="09/26/2016"
    ms.author="spelluru"/>

# <a name="tutorial-build-your-first-pipeline-to-process-data-using-hadoop-cluster"></a>Zelfstudie: Uw eerste pijpleiding om gegevens te verwerken met behulp van Hadoop-cluster maken 
> [AZURE.SELECTOR]
- [Overzicht en vereisten](data-factory-build-your-first-pipeline.md)
- [Azure portal](data-factory-build-your-first-pipeline-using-editor.md)
- [Visual Studio](data-factory-build-your-first-pipeline-using-vs.md)
- [PowerShell](data-factory-build-your-first-pipeline-using-powershell.md)
- [Sjabloon voor bronbeheer](data-factory-build-your-first-pipeline-using-arm.md)
- [REST-API](data-factory-build-your-first-pipeline-using-rest-api.md)

In deze zelfstudie bouwt u uw eerste fabriek Azure gegevens met een pijpleiding gegevens waarmee gegevens worden verwerkt door de component script uitvoeren op een cluster Azure HDInsight (Hadoop). 

> [AZURE.NOTE] Dit artikel biedt een overzicht van Azure Data Factory geen. Zie [Inleiding tot Azure Data Factory](data-factory-introduction.md)voor een overzicht van de service. Zie [Data Factory leren pad](https://azure.microsoft.com/documentation/learning-paths/data-factory/) voor een aanbevolen manier om te navigeren door de inhoud voor meer informatie over Data Factory.

## <a name="whats-covered-in-this-tutorial"></a>Wat vindt u in deze zelfstudie? 
**Azure Data Factory** kunt u voor het opstellen van de **verplaatsing** van gegevens en gegevens **verwerken van** taken als gegevensgestuurde werkstromen (ook wel gegevens pijpleidingen). Leert u hoe u uw eerste data pijpleiding met gegevensverwerking (of gegevenstransformatie) activiteit. Deze activiteit gebruikt een cluster HDInsight Hadoop te transformeren en te analyseren monster weblogs.  

In deze zelfstudie kunt u de volgende stappen uitvoeren:

1.  Maak een **data factory genoemd**. Een fabriek gegevens kan bevatten een of meer gegevens pijpleidingen die gegevens verplaatsen en te verwerken. 
2.  **Gekoppelde services**maken. Maakt u een gekoppelde service voor het koppelen van een gegevensarchief of een compute-service de data factory genoemd. Een gegevensarchief zoals Azure opslag bevat de gegevens van de invoer/uitvoer van activiteiten in de pijplijn. Een compute-service zoals HDInsight Hadoop cluster processen/transformaties gegevens.    
3.  Maken van input en output **datasets**. Een dataset invoer vertegenwoordigt de invoer voor een activiteit in de pijplijn en een dataset output de output voor de activiteit.
3.  Maak de **pijpleiding**. Een pijpleiding kan één of meer activiteiten hebben (voorbeelden: kopie activiteit, activiteit van HDInsight component). In dit voorbeeld gebruikt de component HDInsight activiteit die een component-script wordt uitgevoerd op een cluster HDInsight Hadoop. Het script maakt eerst een tabel die verwijst naar de web onbewerkte logboekgegevens opgeslagen in Azure blob-opslag en vervolgens de onbewerkte gegevens per jaar en maand-partities.

    Er zijn twee soorten activiteiten die worden ondersteund door Azure Data Factory. Ze zijn: [data verkeer activiteiten](data-factory-data-movement-activities.md) en [data transformation activiteiten](data-factory-data-transformation-activities.md). Er is slechts één beweging activiteit die de activiteit van de kopie is. In deze zelfstudie gebruikt u niet de kopie activiteit. Zie voor een zelfstudie over het gebruik van de activiteit kopiëren [Zelfstudie: blob-gegevens kopiëren van een Azure Azure SQL](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md). HDInsight component-activiteit die u in deze zelfstudie is een van de data transformation activiteiten worden ondersteund door Data Factory.  
 
Hier is de **weergave Netwerkdiagram** de sample data fabriek die u in deze zelfstudie bouwen. 

![De weergave Netwerkdiagram in Data Factory-zelfstudie](./media/data-factory-build-your-first-pipeline/data-factory-tutorial-diagram-view.png)

In deze zelfstudie bevat de map **inputdata** van de container **adfgetstarted** Azure blob één bestand met de naam input.log. Dit logboekbestand is posten van drie maanden: januari, februari en maart van 2016. Hier vindt u de voorbeeld-rijen voor elke maand in het invoerbestand. 

    2016-01-01,02:01:09,SAMPLEWEBSITE,GET,/blogposts/mvc4/step2.png,X-ARR-LOG-ID=2ec4b8ad-3cf0-4442-93ab-837317ece6a1,80,-,1.54.23.196,Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36,-,http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx,\N,200,0,0,53175,871 
    2016-02-01,02:01:10,SAMPLEWEBSITE,GET,/blogposts/mvc4/step7.png,X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c,80,-,1.54.23.196,Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36,-,http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx,\N,200,0,0,30184,871
    2016-03-01,02:01:10,SAMPLEWEBSITE,GET,/blogposts/mvc4/step7.png,X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c,80,-,1.54.23.196,Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36,-,http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx,\N,200,0,0,30184,871

Wanneer het bestand wordt verwerkt door de pijplijn met HDInsight-activiteit component, de activiteit component wordt een script uitgevoerd op het cluster HDInsight dat partities invoer per jaar en maand. Het script maakt drie uitvoermappen met een bestand met de gegevens van elke maand.  

    adfgetstarted/partitioneddata/year=2016/month=1/000000_0
    adfgetstarted/partitioneddata/year=2016/month=2/000000_0
    adfgetstarted/partitioneddata/year=2016/month=3/000000_0

Vanuit de voorbeelden hierboven, de eerste knop (met de 2016 01-01-) wordt geschreven naar het bestand 000000_0 in de maand = 1 map. Ook in het tweede voorbeeld wordt weggeschreven naar het bestand in de maand = 2 map en de derde een wordt weggeschreven naar het bestand in de maand = 3 map.  


## <a name="pre-requisites"></a>Minimumvereisten
Voordat u deze zelfstudie hebt u de volgende vereisten:

1.  **Azure abonnement** - als u niet een Azure-abonnement hebt, kunt u een gratis proefperiode account in een paar minuten. Zie de [Gratis evaluatieversie](https://azure.microsoft.com/pricing/free-trial/) artikel over hoe kunt u een gratis proefperiode account.

2.  **Azure opslag** – u een Azure opslag gebruikt voor het opslaan van de gegevens in deze zelfstudie. Als u geen account Azure opslag, Zie het artikel [een opslag-account maken](../storage/storage-create-storage-account.md#create-a-storage-account) . Nadat u de opslag-account hebt gemaakt, ziet u de **accountnaam** en de **toegangstoets**. Zie [weergave, kopieer en toegangstoetsen opslag opnieuw genereren](../storage/storage-create-storage-account.md#view-and-copy-storage-access-keys). 

### <a name="upload-files-to-azure-storage-for-the-tutorial"></a>Bestanden uploaden naar Azure opslag voor de zelfstudie
Voordat u de zelfstudie start, moet u uw account Azure opslag met voorbeeldbestanden voorbereiden voor de zelfstudie.

1. Query componentbestand (HQL) uploaden naar **de map van de container **adfgetstarted** blob** .
2. Input bestand uploaden naar de map **inputdata** van de container **adfgetstarted** blob. 

#### <a name="create-hql-script-file"></a>HQL-script maken 

1. Start **Kladblok** en plak het volgende script voor HQL. Dit script component maakt twee tabellen: **WebLogsRaw** en **WebLogsPartitioned**. Klik op **bestand** in het menu en selecteer **Opslaan**. Ga naar de map **C:\adfgetstarted** op de vaste schijf. Selecteer * *alle bestanden (*.*) **voor de** Opslaan als typt** veld. Voer **partitionweblogs.hql** voor de **naam**. Bevestigen dat de **** onder aan het dialoogvenster is ingesteld op **ANSI**-codering. Zo niet, stel deze in op **ANSI **.  

        set hive.exec.dynamic.partition.mode=nonstrict;
        
        DROP TABLE IF EXISTS WebLogsRaw; 
        CREATE TABLE WebLogsRaw (
          date  date,
          time  string,
          ssitename string,
          csmethod  string,
          csuristem  string,
          csuriquery string,
          sport int,
          susername string,
          cipcsUserAgent string,
          csCookie string,
          csReferer string,
          cshost  string,
          scstatus  int,
          scsubstatus  int,
          scwin32status  int,
          scbytes int,
          csbytes int,
          timetaken int
        )
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
        LINES TERMINATED BY '\n' 
        tblproperties ("skip.header.line.count"="2");
        
        LOAD DATA INPATH '${hiveconf:inputtable}' OVERWRITE INTO TABLE WebLogsRaw;
        
        DROP TABLE IF EXISTS WebLogsPartitioned ; 
        create external table WebLogsPartitioned (  
          date  date,
          time  string,
          ssitename string,
          csmethod  string,
          csuristem  string,
          csuriquery string,
          sport int,
          susername string,
          cipcsUserAgent string,
          csCookie string,
          csReferer string,
          cshost  string,
          scstatus  int,
          scsubstatus  int,
          scwin32status  int,
          scbytes int,
          csbytes int,
          timetaken int
        )
        partitioned by ( year int, month int)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' 
        STORED AS TEXTFILE 
        LOCATION '${hiveconf:partitionedtable}';
        
        INSERT INTO TABLE WebLogsPartitioned  PARTITION( year , month) 
        SELECT
          date,
          time,
          ssitename,
          csmethod,
          csuristem,
          csuriquery,
          sport,
          susername,
          cipcsUserAgent,
          csCookie,
          csReferer,
          cshost,
          scstatus,
          scsubstatus,
          scwin32status,
          scbytes,
          csbytes,
          timetaken,
          year(date),
          month(date)
        FROM WebLogsRaw

Tijdens runtime, wordt de component activiteit in de pijplijn Data Factory waarden voor de **inputtable** en **partitionedtable** parameters zoals in het volgende fragment:  

        "inputtable": "wasb://adfgetstarted@<storageaccountname>.blob.core.windows.net/inputdata",
        "partitionedtable": "wasb://adfgetstarted@<storageaccountname>.blob.core.windows.net/partitioneddata"

De **storageaccountname** is de naam van uw account Azure opslag.
 
#### <a name="create-a-sample-input-file"></a>Voorbeeld van een input bestand maken
Maak in Kladblok een bestand met de naam **input.log** in de **c:\adfgetstarted** met de volgende inhoud: 

    #Software: Microsoft Internet Information Services 8.0
    #Fields: date time s-sitename cs-method cs-uri-stem cs-uri-query s-port cs-username c-ip cs(User-Agent) cs(Cookie) cs(Referer) cs-host sc-status sc-substatus sc-win32-status sc-bytes cs-bytes time-taken
    2016-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step2.png X-ARR-LOG-ID=2ec4b8ad-3cf0-4442-93ab-837317ece6a1 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 53175 871 46
    2016-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step3.png X-ARR-LOG-ID=9eace870-2f49-4efd-b204-0d170da46b4a 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 51237 871 32
    2016-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step4.png X-ARR-LOG-ID=4bea5b3d-8ac9-46c9-9b8c-ec3e9500cbea 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 72177 871 47
    2016-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step5.png X-ARR-LOG-ID=9b0c14b1-434d-495a-9b0d-46775194257b 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 37931 871 32
    2016-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step6.png X-ARR-LOG-ID=f99cff81-ec38-4a13-b2fe-21b10adca996 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 27146 871 47
    2016-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step1.png X-ARR-LOG-ID=af94d3b4-8e05-4871-82c4-638f866d4e83 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 66259 871 140
    2016-02-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2016-02-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2016-02-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2016-02-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2016-02-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2016-02-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2016-02-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2016-02-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2016-03-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2016-03-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2016-03-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2016-03-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2016-03-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47

#### <a name="upload-input-file-and-hql-file-to-your-azure-blob-storage"></a>Invoer en HQL-bestand uploaden naar uw Azure Blob-opslag

Deze sectie bevat instructies voor het hulpprogramma voor **AzCopy** input.log en partitionweblogs.hql-bestanden kopiëren naar Azure Blob-opslag. U kunt een hulpprogramma van uw keuze (bijvoorbeeld: [Microsoft Azure Opslagverkenner](http://storageexplorer.com/), [CloudXPlorer door ClumsyLeaf Software](http://clumsyleaf.com/products/cloudxplorer) voor deze taak.   
     
1. Download de [nieuwste versie van **AzCopy**](http://aka.ms/downloadazcopy)of de [meest recente voorbeeldversie](http://aka.ms/downloadazcopypr). Zie [het gebruik van AzCopy](../storage/storage-use-azcopy.md) artikel voor instructies over het gebruik van het hulpprogramma.
2. Ga naar de map c:\adfgetstarted en voer de volgende opdracht uit: 

        "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\AzCopy" /Source:. /Dest:https://<storageaccountname>.blob.core.windows.net/adfgetstarted/inputdata /DestKey:<storageaccesskey>  /Pattern:input.log

    Met deze opdracht uploadt u het bestand **input.log** naar de rekening van de opslag (**adfgetstarted** container en de map **inputdata** ). **Storageaccountname** vervangen door de naam van uw account voor opslag en **storageaccesskey** met de toegangstoets opslag.

    > [AZURE.NOTE] Deze opdracht maakt een container met de naam **adfgetstarted** in Azure Blob-opslag en het bestand **input.log** van de lokale schijf gekopieerd naar de map **inputdata** in de container. 
    
3. Nadat het bestand is geüpload, ziet u de uitvoer die lijkt op de volgende van AzCopy.
    
        Finished 1 of total 1 file(s).
        [2015/12/16 23:07:33] Transfer summary:
        -----------------
        Total files transferred: 1
        Transfer successfully:   1
        Transfer skipped:        0
        Transfer failed:         0
        Elapsed time:            00.00:00:01
5. Voer de volgende opdracht om het uploaden van het bestand **partitionweblogs.hql** naar de map **scripts** van de container **adfgetstarted** . Dit is de opdracht: 
    
        AzCopy /Source:. /Dest:https://<storageaccountname>.blob.core.windows.net/adfgetstarted/script /DestKey:<storageaccesskey>  /Pattern:partitionweblogs.hql

U kunt de vereisten hebt voltooid. U kunt een data factory met behulp van een van de volgende manieren maken. Klik op een van de tabs aan de bovenkant of de volgende koppelingen voor het uitvoeren van de zelfstudie. 

- [Azure portal](data-factory-build-your-first-pipeline-using-editor.md)
- [Visual Studio](data-factory-build-your-first-pipeline-using-vs.md)
- [PowerShell](data-factory-build-your-first-pipeline-using-powershell.md)
- [Sjabloon voor bronbeheer](data-factory-build-your-first-pipeline-using-arm.md)
- [REST-API](data-factory-build-your-first-pipeline-using-rest-api.md)