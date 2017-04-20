<properties 
    pageTitle="Gegevens verplaatsen naar SQL Server op een Azure virtuele machine | Azure" 
    description="Gegevens verplaatsen van bestanden of van een SQL-Server op locatie met SQL Server op Azure VM." 
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

# <a name="move-data-to-sql-server-on-an-azure-virtual-machine"></a>Gegevens verplaatsen naar SQL Server op een Azure virtuele machine

In dit onderwerp worden de opties voor het verplaatsen van gegevens van bestanden (CSV- of TSV-indeling) of van een lokale SQL Server met SQL Server op een Azure virtuele machine. Deze taken voor het verplaatsen van gegevens naar de cloud maken deel uit van het Team gegevens wetenschap proces.

Zie voor een onderwerp waarin de opties voor het verplaatsen van gegevens naar een SQL-Database Azure voor Machine Learning, [verplaatsen naar een Azure SQL Database voor Azure Machine Learning](machine-learning-data-science-move-sql-azure.md).

Het **menu** onder koppelingen naar onderwerpen waarin wordt beschreven hoe u de gegevens in andere omgevingen doel waar de gegevens kunnen worden opgeslagen en verwerkt tijdens het Team gegevens wetenschap proces (TDSP) te nemen.

[AZURE.INCLUDE [cap-ingest-data-selector](../../includes/cap-ingest-data-selector.md)]


De volgende tabel bevat een overzicht van de opties voor het verplaatsen van gegevens naar SQL Server op een Azure virtuele machine.

<b>BRON</b> |<b>BESTEMMING: SQL Server op Azure VM</b> |
------------------ |-------------------- |
<b>Plat bestand</b> |1. <a href="#insert-tables-bcp">vanaf de opdrachtregel bulk copy hulpprogramma (BCP)</a><br> 2. <a href="#insert-tables-bulkquery">in bulk Insert SQL-Query</a><br> 3. <a href="#sql-builtin-utilities">grafische ingebouwde hulpprogramma's in SQL Server</a>
<b>On-Premises SQL Server</b> | 1. <a href="#deploy-a-sql-server-database-to-a-microsoft-azure-vm-wizard">implementeren van een SQL Server-Database naar een Microsoft Azure VM-wizard</a><br> 2. <a href="#export-flat-file">naar een plat bestand exporteren</a><br> 3. <a href="#sql-migration">Wizard SQL-Database migreren</a> <br> 4. <a href="#sql-backup">database back-en terugzetten</a><br>

De notitie die in dit document wordt ervan uitgegaan dat SQL-opdrachten worden uitgevoerd in SQL Server Management Studio of Visual Studio Database Explorer.

> [AZURE.TIP] Als alternatief kunt u maken en plannen van een pijpleiding die gegevens naar een SQL Server-VM in Azure verplaatsen [Azure Data Factory](https://azure.microsoft.com/services/data-factory/) . Voor meer informatie Zie [kopiëren van gegevens met Azure Data Factory (kopie activiteit)](../data-factory/data-factory-data-movement-activities.md).


## <a name="prereqs"></a>Vereisten
In deze zelfstudie wordt ervan uitgegaan dat u beschikt over:

* Een **abonnement op Azure**. Als u niet een abonnement hebt, kunt u zich aanmelden voor een [gratis proefperiode](https://azure.microsoft.com/pricing/free-trial/).
* Een **account Azure opslag**. U kunt een account Azure opslag wordt gebruikt voor het opslaan van de gegevens in deze zelfstudie. Als u geen account Azure opslag, Zie het artikel [een opslag-account maken](../storage/storage-create-storage-account.md#create-a-storage-account) . Nadat u de opslag-account hebt gemaakt, moet u de sleutel van de account gebruikt voor toegang tot de opslag te verkrijgen. Zie [weergave, kopieer en toegangstoetsen opslag opnieuw genereren](../storage/storage-create-storage-account.md#view-copy-and-regenerate-storage-access-keys).
* **SQL Server op een Azure VM**ingericht. Zie [een Azure SQL Server virtuele machine als een server IPython-laptops voor geavanceerde analytics instellen](machine-learning-data-science-setup-sql-server-virtual-machine.md)voor instructies.
* Geïnstalleerde en geconfigureerde **Azure PowerShell** lokaal. Zie voor instructies [hoe u kunt installeren en configureren van Azure PowerShell](../powershell-install-configure.md).


## <a name="filesource_to_sqlonazurevm"></a>Verplaatsen van gegevens vanuit een plat bestandsbron met SQL Server op een Azure VM

Als de gegevens in een plat bestand (gerangschikt in een rij/kolom), kan worden verplaatst naar SQL Server VM in Azure via de volgende methoden:

1. [Hulpprogramma voor opdrachtregels bulk kopiëren (BCP)](#insert-tables-bcp) 
2. [Bulk Insert SQL-Query](#insert-tables-bulkquery)
3. [Grafische ingebouwde hulpprogramma's in SQL Server (Import/Export, SSIS)](#sql-builtin-utilities)


### <a name="insert-tables-bcp"></a>Hulpprogramma voor opdrachtregels bulk kopiëren (BCP)

BCP is een opdrachtregelprogramma dat wordt geïnstalleerd met SQL Server en is een van de snelste manieren om gegevens te verplaatsen. Het werkt in alle drie SQL Server-varianten (On-premises SQL Server, SQL Azure en SQL Server VM in Azure). 

> [AZURE.NOTE]**Waar de gegevens moeten voor BCP?**  
> Het is niet vereist, dat bestanden met brongegevens die zich op dezelfde computer als de doel-SQL server transfers mogelijk maakt sneller (netwerk snelheid versus lokale schijf i/o-snelheid). U kunt de bestanden met de gegevens op de computer verplaatsen waarop SQL Server is geïnstalleerd met behulp van verschillende bestand kopiëren van hulpprogramma's zoals [AZCopy](../storage/storage-use-azcopy.md), [Azure Opslagverkenner](http://storageexplorer.com/) of windows kopiëren en plakken via Remote Desktop Protocol (RDP).

1. Zorg ervoor dat de database en de tabellen op de doel-SQL Server-database worden gemaakt. Hier volgt een voorbeeld van hoe u deze met behulp van de `Create Database` en `Create Table` opdrachten:

        CREATE DATABASE <database_name>
    
        CREATE TABLE <tablename>
        (
            <columnname1> <datatype> <constraint>,
            <columnname2> <datatype> <constraint>,
            <columnname3> <datatype> <constraint>
        ) 
    
2. De indelingsbestand genereren dat het schema voor de tabel beschrijft door de volgende opdracht uit vanaf de opdrachtregel van de machine waarop de bcp is geïnstalleerd.

    `bcp dbname..tablename format nul -c -x -f exportformatfilename.xml -S servername\sqlinstance -T -t \t -r \n` 

3. Plaats de gegevens in de database met de bcp-opdracht als volgt. Ervan uitgaande dat de SQL-Server is geïnstalleerd op dezelfde computer moet dit werkt vanaf de opdrachtregel:

    `bcp dbname..tablename in datafilename.tsv -f exportformatfilename.xml -S servername\sqlinstancename -U username -P password -b block_size_to_move_in_single_attemp -t \t -r \n`

> **Hiermee voegt u het optimaliseren van de BCP** Raadpleeg het volgende artikel in de ['Richtlijnen voor het optimaliseren van bulkimport'](https://technet.microsoft.com/library/ms177445%28v=sql.105%29.aspx) optimaliseren van dergelijke toevoegingen.


### <a name="insert-tables-bulkquery-parallel"></a>Parallelizing voegt voor snellere verplaatsing van gegevens

Als de gegevens die u wilt verplaatsen te groot is, kunt u versnellen dingen door meerdere BCP-opdrachten gelijktijdig parallel in een PowerShell Script uitvoeren.

> [AZURE.NOTE]**Big data opname** Partitioneren om te optimaliseren voor grote en zeer grote gegevenssets laden van gegevens, de logische en fysieke databasetabellen meerdere bestandsgroepen en partitie tabellen gebruiken. Zie voor meer informatie over het maken en laden van gegevens naar partitietabellen [Parallelle laden SQL-partitietabellen](machine-learning-data-science-parallel-load-sql-partitioned-tables.md).


Het onderstaande voorbeeld PowerShell script parallelle invoegen met behulp van bcp aantonen:
    
    $NO_OF_PARALLEL_JOBS=2

     Set-ExecutionPolicy RemoteSigned #set execution policy for the script to execute
     # Define what each job does
       $ScriptBlock = {
           param($partitionnumber)

           #Explictly using SQL username password
           bcp database..tablename in datafile_path.csv -F 2 -f format_file_path.xml -U username@servername -S tcp:servername -P password -b block_size_to_move_in_single_attempt -t "," -r \n -o path_to_outputfile.$partitionnumber.txt

            #Trusted connection w.o username password (if you are using windows auth and are signed in with that credentials)
            #bcp database..tablename in datafile_path.csv -o path_to_outputfile.$partitionnumber.txt -h "TABLOCK" -F 2 -f format_file_path.xml  -T -b block_size_to_move_in_single_attempt -t "," -r \n 
      }
    

    # Background processing of all partitions
    for ($i=1; $i -le $NO_OF_PARALLEL_JOBS; $i++)
    {
      Write-Debug "Submit loading partition # $i"
      Start-Job $ScriptBlock -Arg $i      
    }
    

    # Wait for it all to complete
    While (Get-Job -State "Running")
    {
      Start-Sleep 10
      Get-Job
    }
    
    # Getting the information back from the jobs
    Get-Job | Receive-Job
    Set-ExecutionPolicy Restricted #reset the execution policy


### <a name="insert-tables-bulkquery"></a>Bulk Insert SQL-Query

[Bulksgewijs invoegen SQL-Query](https://msdn.microsoft.com/library/ms188365) kan worden gebruikt om gegevens te importeren in de database van de rij/kolom op basis van bestanden (ondersteunde typen in de[Gegevens voorbereiden voor Bulk Export of Import (SQL Server)](https://msdn.microsoft.com/library/ms188609)worden behandeld) onderwerp. 

Hier volgen enkele Voorbeeldopdrachten voor Bulk Insert zijn zoals hieronder:  

1. Analyseren van uw gegevens en eventuele aangepaste opties instellen voordat u importeert om ervoor te zorgen dat de SQL Server-database wordt ervan uitgegaan dezelfde indeling voor de speciale velden zoals datums dat. Hier volgt een voorbeeld van het instellen van de notatie jaar-maand-dag (als uw gegevens de datum in de notatie jaar-maand-dag):

        SET DATEFORMAT ymd; 
    
2. Gegevens importeren met instructies voor bulk import:

        BULK INSERT <tablename>
        FROM    
        '<datafilename>'
        WITH 
        (
        FirstRow=2,
        FIELDTERMINATOR =',', --this should be column separator in your data
        ROWTERMINATOR ='\n'   --this should be the row separator in your data
        )
      

### <a name="sql-builtin-utilities"></a>Ingebouwde hulpprogramma's in SQL Server

SQL Server integraties Services (SSIS) kunt u gegevens importeren in SQL Server VM in Azure vanaf een plat bestand. SSI's is beschikbaar in twee studio-omgevingen. Zie voor details de [Integration Services (SSIS) en Studio-omgevingen](https://technet.microsoft.com/library/ms140028.aspx):

- Zie voor meer informatie over hulpmiddelen voor gegevens van SQL Server, [Microsoft SQL Server Data Tools](https://msdn.microsoft.com/data/tools.aspx)  
- Zie voor meer informatie over de Wizard importeren/exporteren, [Wizard SQL Server importeren en exporteren](https://msdn.microsoft.com/library/ms141209.aspx)


## <a name="sqlonprem_to_sqlonazurevm"></a>Gegevens verplaatsen van on-premises SQL Server met SQL Server op een Azure VM

U kunt ook de volgende strategieën:

1. [Implementeren van een SQL Server-Database op een wizard Microsoft Azure VM](#deploy-a-sql-server-database-to-a-microsoft-azure-vm-wizard)
2. [Exporteren naar een plat bestand](#export-flat-file) 
3. [Wizard voor SQL-Database migreren](#sql-migration)
4. [Database back-en terugzetten](#sql-backup)

We beschrijven elk van deze hieronder:

### <a name="deploy-a-sql-server-database-to-a-microsoft-azure-vm-wizard"></a>Implementeren van een SQL Server-Database op een wizard Microsoft Azure VM

Het **implementeren van een SQL Server-Database naar een Microsoft Azure VM-wizard** is een eenvoudig en aanbevolen manier om gegevens te verplaatsen vanuit een SQL Server-exemplaar op ruimten met SQL Server op een Azure VM. Zie voor gedetailleerde stappen, alsmede een beschrijving van de alternatieven, [een database aan SQL Server op een Azure VM migreren](../virtual-machines/virtual-machines-windows-migrate-sql.md).

### <a name="export-flat-file"></a>Exporteren naar een plat bestand

Verschillende methoden kunnen gegevens exporteren uit een SQL-Server On-Premises massaal zoals beschreven in het onderwerp [Bulk importeren en exporteren van gegevens (SQL Server)](https://msdn.microsoft.com/library/ms175937.aspx) worden gebruikt. Dit document wordt uitgelegd hoe de Bulk Copy Program (BCP) als voorbeeld. Zodra de gegevens worden geëxporteerd naar een plat bestand, kan het worden geïmporteerd naar een andere SQL server met behulp van bulkimport. 

1. De gegevens uit SQL-Server voor bedrijfsruimten exporteren naar een bestand met het hulpprogramma bcp als volgt

    `bcp dbname..tablename out datafile.tsv -S  servername\sqlinstancename -T -t \t -t \n -c`

2. De database en de tabel maken in SQL Server VM Azure gebruiken de `create database` en `create table` voor het tabelschema geëxporteerd in stap 1.

3. Maak een bestandsindeling voor het beschrijven van het tabelschema van de gegevens die worden verzonden/binnengebracht. Details van het bestand worden beschreven in de [Format-bestand (SQL Server) maken](https://msdn.microsoft.com/library/ms191516.aspx).

    Indeling genereren BCP bij het uitvoeren van de SQL Server-computer 

        bcp dbname..tablename format nul -c -x -f exportformatfilename.xml -S servername\sqlinstance -T -t \t -r \n 

    Indeling genereren bij het uitvoeren van BCP op afstand ten opzichte van een SQL-Server 

        bcp dbname..tablename format nul -c -x -f  exportformatfilename.xml  -U username@servername.database.windows.net -S tcp:servername -P password  --t \t -r \n
        
    
4. Gebruik een van de methoden die worden beschreven in de sectie [Verplaatsen van gegevens uit de bron van](#filesource_to_sqlonazurevm) gegevens in bestanden te verplaatsen naar een SQL Server.

### <a name="sql-migration"></a>Wizard voor SQL-Database migreren

[SQL Server Database Migration Wizard](http://sqlazuremw.codeplex.com/) biedt een gebruikersvriendelijke manier om gegevens te verplaatsen tussen twee exemplaren van SQL server. Dit kan de gebruiker naar het schema van de tussen- en doeltabellen bronnen toewijzen, kiest u kolomtypen en diverse andere functies. Massaal kopiëren (BCP) onder het achterblad wordt gebruikt. Hieronder vindt u een screenshot van het welkomstscherm van de wizard SQL-Database migreren.  

![SQL Server migratiewizard][2]

### <a name="sql-backup"></a>Database back-en terugzetten

SQL Server ondersteunt: 

1. [Database back-en herstel functionaliteit](https://msdn.microsoft.com/library/ms187048.aspx) (op een lokaal bestand of Bacpac-exporteren naar blob) en [Toepassingen met lagen](https://msdn.microsoft.com/library/ee210546.aspx) (met bacpac). 
2. Mogelijkheid om SQL Server VMs op Azure rechtstreeks te maken met een gekopieerde database of naar een bestaande SQL Azure database kopiëren. Zie [werken met de Wizard Copy Database](https://msdn.microsoft.com/library/ms188664.aspx)voor meer informatie. 

Een schermafbeelding van de Database back-up/terugzetten opties van SQL Server Management Studio wordt hieronder weergegeven.

![Hulpprogramma voor SQL Server importeren][1]

## <a name="resources"></a>Bronnen

[Een Database migreren naar SQL Server op een Azure VM](../virtual-machines/virtual-machines-windows-migrate-sql.md)

[SQL Server op de virtuele Machines van Azure-overzicht](../virtual-machines/virtual-machines-windows-sql-server-iaas-overview.md)

[1]: ./media/machine-learning-data-science-move-sql-server-virtual-machine/sqlserver_builtin_utilities.png
[2]: ./media/machine-learning-data-science-move-sql-server-virtual-machine/database_migration_wizard.png
