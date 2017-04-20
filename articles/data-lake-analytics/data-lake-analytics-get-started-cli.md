<properties 
   pageTitle="Aan de slag met Azure gegevens Lake Analytics opdrachtregel Azure | Microsoft Azure" 
   description="Informatie over het gebruik van de opdrachtregelinterface van Azure een gegevensarchief Lake-account maken, geen gegevens meer Analytics taak gebruikt U SQL maken en indienen van de taak. " 
   services="data-lake-analytics" 
   documentationCenter="" 
   authors="edmacauley" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="05/16/2016"
   ms.author="edmaca"/>

# <a name="tutorial-get-started-with-azure-data-lake-analytics-using-azure-command-line-interface-cli"></a>Zelfstudie: aan de slag met Azure gegevens Lake Analytics met Azure opdrachtregelinterface (CLI)

[AZURE.INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]


Leren werken met Azure CLI Azure gegevens Lake Analytics accounts maken, gegevens Lake Analytics taken definiëren in [U SQL](data-lake-analytics-u-sql-get-started.md)en taken aan gegevens Lake Analytics-accounts. Zie voor meer informatie over gegevens Lake Analytics [Azure gegevens Lake Analytics overzicht](data-lake-analytics-overview.md).

In deze zelfstudie wordt u een taak die u leest een tab gescheiden waarden (TSV) en zet deze in een bestand met door komma's gescheiden waarden (CSV) ontwikkelen. Om na te gaan via dezelfde zelfstudie met behulp van andere ondersteunde hulpprogramma's, klikt u op de tabs boven aan deze sectie.

##<a name="prerequisites"></a>Vereisten

Voordat u deze zelfstudie hebt u het volgende:

- **Azure een abonnement**. Zie [Azure krijg gratis proefperiode](https://azure.microsoft.com/pricing/free-trial/).
- **Azure CLI**. Zie [installeren en configureren van Azure CLI](../xplat-cli-install.md).
    - Downloaden en installeren van **voorlopige** [Azure CLI hulpmiddelen](https://github.com/MicrosoftBigData/AzureDataLake/releases) om deze demo te voltooien.
- **Verificatie**, met de volgende opdracht:

        azure login
    Zie voor meer informatie over verificatie via een account voor werk of school, [verbinding maken met een Azure-abonnement van de CLI Azure](../xplat-cli-connect.md).
- **Overschakelen naar de modus Azure Resource Manager**met de volgende opdracht:

        azure config mode arm
        
## <a name="create-data-lake-analytics-account"></a>Gegevens Lake Analytics-account maken

Voordat u alle taken kunt uitvoeren, moet u een account gegevens Lake Analytics hebben. Om een gegevens Lake Analytics-account maken, moet u het volgende opgeven:

- **Resourcegroep Azure**: A Data Lake Analytics account moet worden gemaakt binnen een groep Azure. [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) kunt u werken met de resources in uw toepassing als een groep. U kunt implementeren, bijwerken of verwijderen alle bronnen voor uw toepassing in een enkele, gecoördineerde werking.  

    Inventariseren van de resourcegroepen in uw abonnement:
    
        azure group list 
    
    Een nieuwe resourcegroep maken:

        azure group create -n "<Resource Group Name>" -l "<Azure Location>"

- **Naam Analytics gegevens Lake**
- **Locatie**: een van de Azure datacenters die gegevens Lake Analytics ondersteunt.
- **Standaard gegevens Lake account**: elke gegevens Lake Analytics-account heeft een standaardaccount voor meer gegevens.

    Overzicht van de bestaande gegevens Lake account:
    
        azure datalake store account list

    Een nieuwe account te maken meer gegevens:

        azure datalake store account create "<Data Lake Store Account Name>" "<Azure Location>" "<Resource Group Name>"

    > [AZURE.NOTE] De naam van de gegevens meer mag alleen kleine letters en cijfers bevatten.



**Een Data Lake Analytics-account maken**

        azure datalake analytics account create "<Data Lake Analytics Account Name>" "<Azure Location>" "<Resource Group Name>" "<Default Data Lake Account Name>"

        azure datalake analytics account list
        azure datalake analytics account show "<Data Lake Analytics Account Name>"          

![Gegevens Lake Analytics weergeven account](./media/data-lake-analytics-get-started-cli/data-lake-analytics-show-account-cli.png)

> [AZURE.NOTE] De naam van de gegevens Lake Analytics mag alleen kleine letters en cijfers bevatten.


## <a name="upload-data-to-data-lake-store"></a>Gegevens uploaden naar Lake gegevensopslag

In deze zelfstudie verwerkt u sommige zoeken in Logboeken.  Het logboek zoeken kan worden opgeslagen in de winkel gegevens Lake of Azure Blob-opslag. 

De Portal Azure biedt een gebruikersinterface voor het aantal voorbeeldbestanden kopiëren naar de standaardaccount gegevens Lake, waaronder een logboekbestand zoeken. [Brongegevens voorbereiden](data-lake-analytics-get-started-portal.md#prepare-source-data) de gegevens uploaden naar de standaardaccount gegevensarchief Lake zien.

Om bestanden te uploaden met behulp van de cli, gebruikt u de volgende opdracht:

    azure datalake store filesystem import "<Data Lake Store Account Name>" "<Path>" "<Destination>"
    azure datalake store filesystem list "<Data Lake Store Account Name>" "<Path>"

Gegevens Lake Analytics ook toegang tot Azure Blob-opslag.  Zie voor het uploaden van gegevens naar Azure Blob-opslag, [met behulp van de CLI Azure met Azure opslag](../storage/storage-azure-cli.md).

## <a name="submit-data-lake-analytics-jobs"></a>Indienen van gegevens Lake Analytics taken

De gegevens Lake Analytics taken zijn geschreven in de taal die U SQL. Zie voor meer informatie over U SQL, [aan de slag met U SQL-taal](data-lake-analytics-u-sql-get-started.md) en [U SQL language reference](http://go.microsoft.com/fwlink/?LinkId=691348).

**Een taak gegevens Lake Analytics script maken**

- Maak een tekstbestand met de volgende U SQL-script en sla het tekstbestand op uw werkstation:

        @searchlog =
            EXTRACT UserId          int,
                    Start           DateTime,
                    Region          string,
                    Query           string,
                    Duration        int?,
                    Urls            string,
                    ClickedUrls     string
            FROM "/Samples/Data/SearchLog.tsv"
            USING Extractors.Tsv();
        
        OUTPUT @searchlog   
            TO "/Output/SearchLog-from-Data-Lake.csv"
        USING Outputters.Csv();

    Dit U SQL-script leest het brongegevensbestand met **Extractors.Tsv()**en maakt vervolgens een CSV-bestand met behulp van **Outputters.Csv()**. 
    
    De twee paden niet worden gewijzigd, tenzij u het bronbestand naar een andere locatie kopiëren.  Gegevens Lake Analytics maakt de uitvoermap als het niet bestaat.
    
    Is het eenvoudiger de relatieve paden voor de bestanden opgeslagen in standaard gegevens meer accounts gebruikt. Ook kunt u absolute paden.  Bijvoorbeeld 
    
        adl://<Data LakeStorageAccountName>.azuredatalakestore.net:443/Samples/Data/SearchLog.tsv
        
    Absolute paden hebt u toegang tot bestanden in de gekoppelde accounts voor opslag.  De syntaxis voor bestanden die zijn opgeslagen in de gekoppelde Azure opslag account is:
    
        wasb://<BlobContainerName>@<StorageAccountName>.blob.core.windows.net/Samples/Data/SearchLog.tsv

    >[AZURE.NOTE] Azure Blob container met openbare BLOB's of machtigingen voor openbare containers worden momenteel niet ondersteund.      

    
**De taak verzenden**


    azure datalake analytics job create  "<Data Lake Analytics Account Name>" "<Job Name>" "<Script>"
    
    
De volgende opdrachten kunnen worden gebruikt voor lijst taken, taak informatie en taken annuleren:

    azure datalake analytics job cancel "<Data Lake Analytics Account Name>" "<Job Id>"
    azure datalake analytics job list "<Data Lake Analytics Account Name>"
    azure datalake analytics job show "<Data Lake Analytics Account Name>" "<Job Id>"

Nadat de taak is voltooid, kunt u de volgende cmdlets gebruiken om het bestand en het bestand downloaden:
    
    azure datalake store filesystem list "<Data Lake Store Account Name>" "/Output"
    azure datalake store filesystem export "<Data Lake Store Account Name>" "/Output/SearchLog-from-Data-Lake.csv" "<Destination>"
    azure datalake store filesystem read "<Data Lake Store Account Name>" "/Output/SearchLog-from-Data-Lake.csv" <Length> <Offset>

## <a name="see-also"></a>Zie ook

- Klik op het tabblad kiezers boven aan de pagina overzicht dezelfde zelfstudie met behulp van andere hulpprogramma's.
- Een complexe query's, Zie [Logboeken van Website analyseren met behulp van Azure gegevens Lake Analytics](data-lake-analytics-analyze-weblogs.md).
- Zie [ontwikkelt U SQL - scripts met behulp van hulpmiddelen voor gegevens Lake voor Visual Studio](data-lake-analytics-data-lake-tools-get-started.md)om te beginnen U SQL-toepassingen ontwikkelen.
- Zie informatie over U SQL, [aan de slag met Azure gegevens Lake Analytics U SQL - taal](data-lake-analytics-u-sql-get-started.md).
- Zie voor beheertaken [Beheren Azure gegevens Lake Analytics met Azure Portal](data-lake-analytics-manage-use-portal.md).
- Zie voor een overzicht van gegevens Lake Analytics, [Azure gegevens Lake Analytics overzicht](data-lake-analytics-overview.md).

