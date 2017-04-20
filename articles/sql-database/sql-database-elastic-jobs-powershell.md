<properties 
    pageTitle="Maken en beheren met PowerShell elastische Database-taken" 
    description="PowerShell is gebruikt voor het beheren van toepassingen Azure SQL-Database" 
    services="sql-database" documentationCenter=""  
    manager="jhubbard" 
    authors="ddove"/>

<tags 
    ms.service="sql-database" 
    ms.workload="sql-database" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="05/27/2016" 
    ms.author="ddove" />

# <a name="create-and-manage-a-sql-database-elastic-database-jobs-using-powershell-preview"></a>Maken en beheren van een SQL-Database database elastische taken met PowerShell (voorbeeld)

> [AZURE.SELECTOR]
- [Azure portal](sql-database-elastic-jobs-create-and-manage.md)
- [PowerShell](sql-database-elastic-jobs-powershell.md)



De PowerShell-APIs voor **elastische Database taken** (in het voorbeeld), kunt u definiëren een groep van databases die scripts worden uitgevoerd. Dit artikel ziet hoe u kunt maken en beheren van **taken elastische Database** met PowerShell-cmdlets. Zie [overzicht van elastisch](sql-database-elastic-jobs-overview.md). 

## <a name="prerequisites"></a>Vereisten
* Een abonnement op Azure. Zie voor een gratis evaluatieversie, [gratis proefperiode van een maand](https://azure.microsoft.com/pricing/free-trial/).
* Een set van databases die zijn gemaakt met de elastische Database tools. Zie [aan de slag met elastische Databasehulpmiddelen](sql-database-elastic-scale-get-started.md).
* Azure PowerShell. Zie voor gedetailleerde informatie [het installeren en configureren van Azure PowerShell](../powershell-install-configure.md).
* **Elastische Database taken** Pakket PowerShell: overzicht [projecten elastische Database installeren](sql-database-elastic-jobs-service-installation.md)

### <a name="select-your-azure-subscription"></a>Selecteer uw abonnement Azure

Selecteer het abonnement moet u uw abonnements-Id (**-SubscriptionId**) of een abonnement een naam (**-SubscriptionName**). Als u meerdere abonnementen kunt u de cmdlet **Get-AzureRmSubscription** uitvoeren en kopieer de abonnementsgegevens van het gewenste van de resultaatset. Zodra u uw abonnement op informatie hebt, de volgende commandlet als u dit abonnement standaard, namelijk het doel voor het maken en beheren van taken worden uitgevoerd:

    Select-AzureRmSubscription -SubscriptionId {SubscriptionID}

De [PowerShell ISE](https://technet.microsoft.com/library/dd315244.aspx) wordt aanbevolen voor gebruik ontwikkelen en uitvoeren van PowerShell scripts ten opzichte van de taken van de elastische Database.

## <a name="elastic-database-jobs-objects"></a>Elastische databaseobjecten taken

De volgende tabel geeft een overzicht van alle objecttypen **taken elastische Database** samen met de beschrijving en de relevante PowerShell APIs.

<table style="width:100%">
  <tr>
    <th>Objecttype</th>
    <th>Beschrijving</th>
    <th>Verwante PowerShell API 's</th>
  </tr>
  <tr>
    <td>Referentie</td>
    <td>Gebruikersnaam en wachtwoord te gebruiken bij het maken van verbinding met databases voor de uitvoering van scripts of de toepassing van DACPACs. <p>Het wachtwoord wordt gecodeerd alvorens te verzenden en op te slaan in de database elastische Database taken.  Het wachtwoord wordt gedecodeerd door de service elastische Database taken via de referentie gemaakt en geüpload door het installatiescript.</td>
    <td><p>Get-AzureSqlJobCredential</p>
    <p>Nieuwe AzureSqlJobCredential</p><p>Set AzureSqlJobCredential</p></td></td>
  </tr>

  <tr>
    <td>Script</td>
    <td>Transact-SQL-script moet worden gebruikt voor de uitvoering van verschillende databases.  Het script moet worden geschreven als idempotency is ingeschakeld omdat de service wordt opnieuw geprobeerd de uitvoering van het script op fouten.
    </td>
    <td>
    <p>Get-AzureSqlJobContent</p>
    <p>Get-AzureSqlJobContentDefinition</p>
    <p>Nieuwe AzureSqlJobContent</p>
    <p>Set AzureSqlJobContentDefinition</p>
    </td>
  </tr>

  <tr>
    <td>DACPAC</td>
    <td><a href="https://msdn.microsoft.com/library/ee210546.aspx">Gegevens-tier-toepassing </a> pakket moet worden toegepast op databases.

    </td>
    <td>
    <p>Get-AzureSqlJobContent</p>
    <p>Nieuwe AzureSqlJobContent</p>
    <p>Set AzureSqlJobContentDefinition</p>
    </td>
  </tr>
  <tr>
    <td>Doel van de database</td>
    <td>Database en server naam verwijst naar een Azure SQL-Database.

    </td>
    <td>
    <p>Get-AzureSqlJobTarget</p>
    <p>Nieuwe AzureSqlJobTarget</p>
    </td>
  </tr>
  <tr>
    <td>Shard kaart doel</td>
    <td>De combinatie van een doel van de database en een referentie die worden gebruikt om te bepalen van informatie die is opgeslagen in een Database met elastische shard hyperlinks.
    </td>
    <td>
    <p>Get-AzureSqlJobTarget</p>
    <p>Nieuwe AzureSqlJobTarget</p>
    <p>Set AzureSqlJobTarget</p>
    </td>
  </tr>
<tr>
    <td>Aangepaste collectie doel</td>
    <td>Gedefinieerde groep databases wordt gebruikt voor de uitvoering.</td>
    <td>
    <p>Get-AzureSqlJobTarget</p>
    <p>Nieuwe AzureSqlJobTarget</p>
    </td>
  </tr>
<tr>
    <td>Aangepaste collectie onderliggende doel</td>
    <td>Doel database waarnaar wordt verwezen vanuit een aangepaste verzameling.</td>
    <td>
    <p>Toevoegen-AzureSqlJobChildTarget</p>
    <p>Verwijderen AzureSqlJobChildTarget</p>
    </td>
  </tr>

<tr>
    <td>Taak</td>
    <td>
    <p>Definitie van parameters voor een taak die aan de trigger worden uitgevoerd of om te voldoen aan een schema kan worden gebruikt.</p>
    </td>
    <td>
    <p>Get-AzureSqlJob</p>
    <p>Nieuwe AzureSqlJob</p>
    <p>Set AzureSqlJob</p>
    </td>
  </tr>

<tr>
    <td>Taakuitvoering</td>
    <td>
    <p>Container van de taken die nodig zijn om te voldoen aan het uitvoeren van een script of toepassing van een DACPAC naar een doel met behulp van referenties voor databaseverbindingen met fouten verwerkt overeenkomstig een beleid kan worden uitgevoerd.</p>
    </td>
    <td>
    <p>Get-AzureSqlJobExecution</p>
    <p>Start AzureSqlJobExecution</p>
    <p>Stop AzureSqlJobExecution</p>
    <p>Wachten op AzureSqlJobExecution</p>
  </tr>

<tr>
    <td>Uitvoering van de taken van taak</td>
    <td>
    <p>De eenheid van het werk om te voldoen aan een taak.</p>
    <p>Als een taak kan niet met succes worden uitgevoerd, de resulterende uitzonderingsbericht wordt vastgelegd en een nieuwe projecttaak overeenkomende worden gemaakt en uitgevoerd in overeenstemming met hun orderuitvoeringsbeleid opgegeven.</p></p>
    </td>
    <td>
    <p>Get-AzureSqlJobExecution</p>
    <p>Start AzureSqlJobExecution</p>
    <p>Stop AzureSqlJobExecution</p>
    <p>Wachten op AzureSqlJobExecution</p>
  </tr>

<tr>
    <td>Taak uitvoeren van beleid</td>
    <td>
    <p>Besturingselementen taak uitvoering time-outs, limiet voor opnieuw proberen en intervallen tussen nieuwe pogingen.</p>
    <p>Elastische Database taken bevat de taak worden uitgevoerd voor het standaardbeleid die in feite oneindig pogingen van taak taak storingen veroorzaken met exponentiële backoff van intervallen tussen nieuwe pogingen.</p>
    </td>
    <td>
    <p>Get-AzureSqlJobExecutionPolicy</p>
    <p>Nieuwe AzureSqlJobExecutionPolicy</p>
    <p>Set AzureSqlJobExecutionPolicy</p>
    </td>
  </tr>

<tr>
    <td>Planning</td>
    <td>
    <p>Tijd op basis van specificatie voor het plaatsvinden van een terugkerende interval of één keer.</p>
    </td>
    <td>
    <p>Get-AzureSqlJobSchedule</p>
    <p>Nieuwe AzureSqlJobSchedule</p>
    <p>Set AzureSqlJobSchedule</p>
    </td>
  </tr>

<tr>
    <td>Triggers van de taak</td>
    <td>
    <p>Een toewijzing tussen een taak en een planning tot de uitvoering van de trigger taak volgens de planning.</p>
    </td>
    <td>
    <p>Nieuwe AzureSqlJobTrigger</p>
    <p>Verwijderen AzureSqlJobTrigger</p>
    </td>
  </tr>
</table>

## <a name="supported-elastic-database-jobs-group-types"></a>Ondersteunde elastische Database taken groeperen typen
De taak wordt uitgevoerd van scripts van Transact-SQL (T-SQL) of de toepassing van de DACPACs over een groep van databases. Wanneer een taak moet worden uitgevoerd binnen een groep van databases is ingediend, de taak "uitbreiding" het in onderliggende taken waarbij elk de gevraagde uitvoering tegen één database in de groep uitvoert. 
 
Er zijn twee typen groepen die u kunt maken: 

* Groep [Shard kaart](sql-database-elastic-scale-shard-map-management.md) : wanneer een taak wordt verzonden om een kaart shard, de taak de shard-kaart om te bepalen van de huidige set shards query's en maakt u onderliggende taken voor elke shard in de map shard.
* Aangepaste collectie groep: een aangepaste set van databases. Wanneer een taak is bedoeld voor een aangepaste verzameling, wordt onderliggende taken voor elke database die momenteel in de collectie aangepaste.

## <a name="to-set-the-elastic-database-jobs-connection"></a>Taken in te stellen van de elastische Database verbinding

Een verbinding moet worden ingesteld op de taken *control-database* voordat u met de API's van taken. Deze cmdlet uitvoert activeert een venster referenties voor aanvragen van de gebruikersnaam en het wachtwoord die worden gemaakt bij het installeren van elastische Database taken worden weergegeven. Alle voorbeelden die in dit onderwerp wordt ervan uitgegaan dat deze stap al is uitgevoerd.

Een verbinding met de taken van de elastische Database openen:

    Use-AzureSqlJobConnection -CurrentAzureSubscription 

## <a name="encrypted-credentials-within-the-elastic-database-jobs"></a>Gecodeerde referenties in de Database elastische taken

Databasereferenties kunnen in de projecten *database besturingselement* worden ingevoegd met het gecodeerde wachtwoord. Het is nodig voor het opslaan van referenties als u taken op een later tijdstip wordt uitgevoerd (met taakplanningen).
 
Codering werkt door middel van een certificaat dat is gemaakt als onderdeel van het installatiescript. De installatiescript maakt en uploadt u het certificaat in de Azure Cloud Service voor decodering van gecodeerde wachtwoorden opgeslagen. De Azure Cloud-Service slaat later de openbare sleutel binnen de taken *besturingselement database* waarmee de interface PowerShell API of Azure klassieke Portal voor het coderen van een opgegeven wachtwoord zonder het certificaat moet lokaal zijn geïnstalleerd.
 
De referentie-wachtwoorden worden gecodeerd en veilig van gebruikers met alleen-lezen toegang aan taken van elastische databaseobjecten. Maar het is mogelijk dat een kwaadwillende gebruiker met alleen-lezen toegang tot taken voor elastische Database-objecten ophalen van een wachtwoord. Referenties zijn ontworpen voor de hele uitvoering worden gebruikt. Referenties worden doorgegeven aan doeldatabases bij het maken van verbindingen. Er zijn momenteel geen beperkingen voor de doeldatabases die worden gebruikt voor elke referentie, kwaadwillende gebruiker een doel van de database voor een database onder controle van de kwaadwillende gebruiker kan toevoegen. De gebruiker kan vervolgens een taak voor deze database om te krijgen van de referentie-wachtwoord starten.

Aanbevolen procedures voor beveiliging voor elastische Database taken omvatten:

* Beperk het gebruik van de API's aan betrouwbare personen.
* Referenties moeten de minimaal benodigde bevoegdheden voor het uitvoeren van de taak nodig hebt.  Meer informatie kunt bekijken binnen deze [machtigingen en machtigingen voor](https://msdn.microsoft.com/library/bb669084.aspx) SQL Server MSDN-artikel.

### <a name="to-create-an-encrypted-credential-for-job-execution-across-databases"></a>Een gecodeerde referentie voor de taakuitvoering maken voor databases

U maakt een nieuwe gecodeerde referentie, de [**cmdlet Get-referenties**](https://technet.microsoft.com/library/hh849815.aspx) gevraagd een gebruikersnaam en wachtwoord die kan worden doorgegeven aan de [**cmdlet New-AzureSqlJobCredential**](https://msdn.microsoft.com/library/mt346063.aspx).

    $credentialName = "{Credential Name}"
    $databaseCredential = Get-Credential
    $credential = New-AzureSqlJobCredential -Credential $databaseCredential -CredentialName $credentialName
    Write-Output $credential

### <a name="to-update-credentials"></a>Voor het bijwerken van referenties

Wanneer u wachtwoorden wijzigt, gebruikt u de [**cmdlet Set-AzureSqlJobCredential**](https://msdn.microsoft.com/library/mt346062.aspx) en stelt u de parameter **CredentialName** .

    $credentialName = "{Credential Name}"
    Set-AzureSqlJobCredential -CredentialName $credentialName -Credential $credential 

## <a name="to-define-an-elastic-database-shard-map-target"></a>Voor het definiëren van een doel elastische Database shard kaart

Gebruik voor het uitvoeren van een taak ten opzichte van alle databases in een shard (gemaakt met [elastische Database client library](sql-database-elastic-database-client-library.md)), een shard kaart als het doel van de database. Dit voorbeeld vereist een een laptopgeheugen gemaakt met behulp van de elastische Database client library-toepassing. Zie [aan de slag met elastische Database extra monster](sql-database-elastic-scale-get-started.md).

De database manager shard moet worden ingesteld als het doel van een database en vervolgens de specifieke shard-kaart moet worden opgegeven als doel.

    $shardMapCredentialName = "{Credential Name}"
    $shardMapDatabaseName = "{ShardMapDatabaseName}" #example: ElasticScaleStarterKit_ShardMapManagerDb
    $shardMapDatabaseServerName = "{ShardMapServerName}"
    $shardMapName = "{MyShardMap}" #example: CustomerIDShardMap
    $shardMapDatabaseTarget = New-AzureSqlJobTarget -DatabaseName $shardMapDatabaseName -ServerName $shardMapDatabaseServerName
    $shardMapTarget = New-AzureSqlJobTarget -ShardMapManagerCredentialName $shardMapCredentialName -ShardMapManagerDatabaseName $shardMapDatabaseName -ShardMapManagerServerName $shardMapDatabaseServerName -ShardMapName $shardMapName
    Write-Output $shardMapTarget

## <a name="create-a-t-sql-script-for-execution-across-databases"></a>Een T-SQL-Script voor het uitvoeren van verschillende databases maken

Bij het maken van T-SQL-scripts wordt uitgevoerd, is het ten zeerste aanbevolen om samen te stellen dat deze als [idempotency is ingeschakeld](https://en.wikipedia.org/wiki/Idempotence) en veerkrachtig tegen storingen. Elastische Database taken zal uitvoeren van het script worden herhaald als uitvoering van een storing, ongeacht de indeling van de fout wordt aangetroffen.

Gebruik de [**cmdlet New-AzureSqlJobContent**](https://msdn.microsoft.com/library/mt346085.aspx) maken en opslaan van een script voor de uitvoering van en de **ContentName -** en **- CommandText** parameters instellen.

    $scriptName = "Create a TestTable"

    $scriptCommandText = "
    IF NOT EXISTS (SELECT name FROM sys.tables WHERE name = 'TestTable')
    BEGIN
        CREATE TABLE TestTable(
            TestTableId INT PRIMARY KEY IDENTITY,
            InsertionTime DATETIME2
        );
    END
    GO
    INSERT INTO TestTable(InsertionTime) VALUES (sysutcdatetime());
    GO"

    $script = New-AzureSqlJobContent -ContentName $scriptName -CommandText $scriptCommandText
    Write-Output $script

### <a name="create-a-new-script-from-a-file"></a>Een nieuw script maken vanuit een bestand
Als de T-SQL-script in een bestand is gedefinieerd, gebruikt u dit voor het importeren van het script:

    $scriptName = "My Script Imported from a File"
    $scriptPath = "{Path to SQL File}"
    $scriptCommandText = Get-Content -Path $scriptPath
    $script = New-AzureSqlJobContent -ContentName $scriptName -CommandText $scriptCommandText
    Write-Output $script

### <a name="to-update-a-t-sql-script-for-execution-across-databases"></a>Een T-SQL-script voor het uitvoeren van verschillende databases bijwerken  

Deze PowerShell script werkt de T-SQL-opdrachttekst voor een bestaand script.

Stel de volgende variabelen zijn aangepast aan de definitie van het gewenste script wilt instellen:

    $scriptName = "Create a TestTable"
    $scriptUpdateComment = "Adding AdditionalInformation column to TestTable"
    $scriptCommandText = "
    IF NOT EXISTS (SELECT name FROM sys.tables WHERE name = 'TestTable')
    BEGIN
    CREATE TABLE TestTable(
        TestTableId INT PRIMARY KEY IDENTITY,
        InsertionTime DATETIME2
    );
    END
    GO

    IF NOT EXISTS (SELECT columns.name FROM sys.columns INNER JOIN sys.tables on columns.object_id = tables.object_id WHERE tables.name = 'TestTable' AND columns.name = 'AdditionalInformation')
    BEGIN
    ALTER TABLE TestTable
    ADD AdditionalInformation NVARCHAR(400);
    END
    GO

    INSERT INTO TestTable(InsertionTime, AdditionalInformation) VALUES (sysutcdatetime(), 'test');
    GO"

### <a name="to-update-the-definition-to-an-existing-script"></a>De definitie voor een bestaand script bijwerken

    Set-AzureSqlJobContentDefinition -ContentName $scriptName -CommandText $scriptCommandText -Comment $scriptUpdateComment 

## <a name="to-create-a-job-to-execute-a-script-across-a-shard-map"></a>Voor het maken van een taak voor het uitvoeren van een script in een map shard

Deze PowerShell-script start een taak voor het uitvoeren van een script via elke shard in een schaal met elastische shard hyperlinks.

Stel de volgende variabelen om het gewenste script en het doel weer te geven:

    $jobName = "{Job Name}"
    $scriptName = "{Script Name}"
    $shardMapServerName = "{Shard Map Server Name}"
    $shardMapDatabaseName = "{Shard Map Database Name}"
    $shardMapName = "{Shard Map Name}"
    $credentialName = "{Credential Name}"
    $shardMapTarget = Get-AzureSqlJobTarget -ShardMapManagerDatabaseName $shardMapDatabaseName -ShardMapManagerServerName $shardMapServerName -ShardMapName $shardMapName 
    $job = New-AzureSqlJob -ContentName $scriptName -CredentialName $credentialName -JobName $jobName -TargetId $shardMapTarget.TargetId
    Write-Output $job

## <a name="to-execute-a-job"></a>Een taak uitvoeren 

Deze PowerShell-script wordt uitgevoerd voor een bestaande taak:

Update van de volgende variabele, zodat de naam van de gewenste taak hebt uitgevoerd:

    $jobName = "{Job Name}"
    $jobExecution = Start-AzureSqlJobExecution -JobName $jobName 
    Write-Output $jobExecution
 
## <a name="to-retrieve-the-state-of-a-single-job-execution"></a>De status van de uitvoering van een enkele taak ophalen

Gebruik de [**cmdlet Get-AzureSqlJobExecution**](https://msdn.microsoft.com/library/mt346058.aspx) en stelt u de parameter **JobExecutionId** om de status van de taakuitvoering weer te geven.

    $jobExecutionId = "{Job Execution Id}"
    $jobExecution = Get-AzureSqlJobExecution -JobExecutionId $jobExecutionId
    Write-Output $jobExecution

De dezelfde **Get-AzureSqlJobExecution** -cmdlet gebruiken met de parameter **IncludeChildren** om de status van de onderliggende uitvoering, namelijk de specifieke status voor elke taak worden uitgevoerd voor elke database gericht door de taak weer te geven.

    $jobExecutionId = "{Job Execution Id}"
    $jobExecutions = Get-AzureSqlJobExecution -JobExecutionId $jobExecutionId -IncludeChildren
    Write-Output $jobExecutions 

## <a name="to-view-the-state-across-multiple-job-executions"></a>De status weergeven in meerdere uitvoeringen van taak

De [**cmdlet Get-AzureSqlJobExecution**](https://msdn.microsoft.com/library/mt346058.aspx) heeft meerdere optionele parameters die kunnen worden gebruikt om meerdere uitvoering, gefilterd door de opgegeven parameters weer te geven. Hieronder ziet u enkele van de mogelijke manieren om Get-AzureSqlJobExecution:

Alle actieve bovenste niveau uitvoering ophalen:

    Get-AzureSqlJobExecution

Alle top level uitvoering, met inbegrip van niet-actieve uitvoering ophalen:

    Get-AzureSqlJobExecution -IncludeInactive

Alle onderliggende taak uitvoeringen van een opgegeven taak kan worden uitgevoerd-ID, met inbegrip van niet-actieve uitvoering ophalen:

    $parentJobExecutionId = "{Job Execution Id}"
    Get-AzureSqlJobExecution -AzureSqlJobExecution -JobExecutionId $parentJobExecutionId –IncludeInactive -IncludeChildren

Ophalen van alle uitvoering gemaakt met behulp van een planning / job combinatie, met inbegrip van niet-actieve taken:

    $jobName = "{Job Name}"
    $scheduleName = "{Schedule Name}"
    Get-AzureSqlJobExecution -JobName $jobName -ScheduleName $scheduleName -IncludeInactive

Alle taken die zijn gericht op een opgegeven shard kaart, met inbegrip van niet-actieve taken ophalen:

    $shardMapServerName = "{Shard Map Server Name}"
    $shardMapDatabaseName = "{Shard Map Database Name}"
    $shardMapName = "{Shard Map Name}"
    $target = Get-AzureSqlJobTarget -ShardMapManagerDatabaseName $shardMapDatabaseName -ShardMapManagerServerName $shardMapServerName -ShardMapName $shardMapName
    Get-AzureSqlJobExecution -TargetId $target.TargetId –IncludeInactive

Alle taken die voor een opgegeven aangepaste-collectie, met inbegrip van niet-actieve taken ophalen:

    $customCollectionName = "{Custom Collection Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    Get-AzureSqlJobExecution -TargetId $target.TargetId –IncludeInactive
 
Ophalen van de lijst van de taak uitvoering binnen een bepaalde taak kan worden uitgevoerd:

    $jobExecutionId = "{Job Execution Id}"
    $jobTaskExecutions = Get-AzureSqlJobTaskExecution -JobExecutionId $jobExecutionId
    Write-Output $jobTaskExecutions 

Ophalen van details van taak worden uitgevoerd:

De volgende PowerShell script kan worden gebruikt om de details van een taak taak worden uitgevoerd, dat vooral handig is bij het opsporen van fouten bij de uitvoering fouten weer te geven.

    $jobTaskExecutionId = "{Job Task Execution Id}"
    $jobTaskExecution = Get-AzureSqlJobTaskExecution -JobTaskExecutionId $jobTaskExecutionId
    Write-Output $jobTaskExecution

## <a name="to-retrieve-failures-within-job-task-executions"></a>Storingen in uitvoering taak ophalen

Het **JobTaskExecution-object** bevat een eigenschap voor de levenscyclus van de taak in combinatie met de berichteigenschap van een. Een taak taak niet uitvoeren, de levenscyclus van de eigenschap wordt ingesteld op *mislukt* als de berichteigenschap ingesteld op het resulterende uitzonderingsbericht en de stack. Als een taak niet is gelukt, is het belangrijk dat u de details weergeven van taken die voor een bepaalde taak is mislukt.

    $jobExecutionId = "{Job Execution Id}"
    $jobTaskExecutions = Get-AzureSqlJobTaskExecution -JobExecutionId $jobExecutionId
    Foreach($jobTaskExecution in $jobTaskExecutions) 
        {
        if($jobTaskExecution.Lifecycle -ne 'Succeeded')
            {
            Write-Output $jobTaskExecution
            }
        }

## <a name="to-wait-for-a-job-execution-to-complete"></a>Wachten op de uitvoering van een taak te voltooien

De volgende PowerShell script kan worden gebruikt om te wachten op een taak te voltooien:

    $jobExecutionId = "{Job Execution Id}"
    Wait-AzureSqlJobExecution -JobExecutionId $jobExecutionId 

## <a name="create-a-custom-execution-policy"></a>Maak een aangepaste uitvoering van beleid

Elastische Database projecten ondersteunt het maken van aangepaste uitvoering van beleid dat kunnen worden toegepast bij het starten van taken.
  
Uitvoering van beleid dat momenteel voor het definiëren van:

* Naam: Id voor het uitvoeren van beleid.
* Time-out van taak: Totale tijd voordat een project door taken van elastische Database wordt geannuleerd.
* Interval voor eerste poging: Interval moet worden gewacht voordat de eerste poging.
* Maximale Interval voor opnieuw proberen: Dop intervallen voor nieuwe pogingen te gebruiken.
* Interval Backoff coëfficiënt proberen: Coëfficiënt die wordt gebruikt voor het berekenen van het volgende interval tussen nieuwe pogingen.  De volgende formule gebruikt: (eerste Interval voor opnieuw proberen) * Math.pow ((Interval Backoff coëfficiënt) (Number of Retries) - 2). 
* Maximum aantal pogingen: Het maximale aantal nieuwe pogingen probeert uit te voeren binnen een project.

Het standaardbeleid voor de uitvoering van de volgende waarden gebruikt:

* Naam: Standaard uitvoering van beleid
* Time-out van taak: 1 week
* Interval voor eerste poging: 100 milliseconden
* Maximale Interval voor opnieuw proberen: 30 minuten
* Coëfficiënt Interval voor opnieuw proberen: 2
* Maximum aantal pogingen: 2.147.483.647

Maak de gewenste uitvoering van beleid:

    $executionPolicyName = "{Execution Policy Name}"
    $initialRetryInterval = New-TimeSpan -Seconds 10
    $jobTimeout = New-TimeSpan -Minutes 30
    $maximumAttempts = 999999
    $maximumRetryInterval = New-TimeSpan -Minutes 1
    $retryIntervalBackoffCoefficient = 1.5
    $executionPolicy = New-AzureSqlJobExecutionPolicy -ExecutionPolicyName $executionPolicyName -InitialRetryInterval $initialRetryInterval -JobTimeout $jobTimeout -MaximumAttempts $maximumAttempts -MaximumRetryInterval $maximumRetryInterval 
    -RetryIntervalBackoffCoefficient $retryIntervalBackoffCoefficient
    Write-Output $executionPolicy

### <a name="update-a-custom-execution-policy"></a>Een aangepaste uitvoeringsbeleid bijwerken

Update voor het gewenste beleid om bij te werken:

    $executionPolicyName = "{Execution Policy Name}"
    $initialRetryInterval = New-TimeSpan -Seconds 15
    $jobTimeout = New-TimeSpan -Minutes 30
    $maximumAttempts = 999999
    $maximumRetryInterval = New-TimeSpan -Minutes 1
    $retryIntervalBackoffCoefficient = 1.5
    $updatedExecutionPolicy = Set-AzureSqlJobExecutionPolicy -ExecutionPolicyName $executionPolicyName -InitialRetryInterval $initialRetryInterval -JobTimeout $jobTimeout -MaximumAttempts $maximumAttempts -MaximumRetryInterval $maximumRetryInterval -RetryIntervalBackoffCoefficient $retryIntervalBackoffCoefficient
    Write-Output $updatedExecutionPolicy
 
## <a name="cancel-a-job"></a>Een taak annuleren

Elastische Database projecten ondersteunt annulering aanvragen van taken.  Als taken met elastische Database een annulering aanvragen voor een project dat momenteel wordt uitgevoerd detecteert, wordt geprobeerd de taak stoppen.

Er zijn twee verschillende manieren dat een annulering door elastische Database taken kunt uitvoeren:

1. Actieve taken annuleren: een annulering als een annulering wordt aangetroffen terwijl een taak wordt uitgevoerd, worden geprobeerd in de momenteel uitgevoerde aspect van de taak.  Bijvoorbeeld: als er een langdurige query momenteel wordt uitgevoerd wanneer een annulering wordt uitgevoerd, zal er een poging tot het annuleren van de query.
2. Pogingen van de taak annuleren: als een annulering wordt gedetecteerd door de thread besturingselement voordat een taak voor uitvoering wordt gestart, de thread van het besturingselement wordt voorkomen dat de taak wordt gestart en de aanvraag declareren als geannuleerd.

Als u een annulering voor een taak wordt aangevraagd voor een bovenliggende functie, het verzoek om annulering wordt van kracht voor de bovenliggende taak en alle bijbehorende onderliggende taken.
 
Een annuleringsverzoek verzenden, gebruikt u de [**cmdlet Stop AzureSqlJobExecution**](https://msdn.microsoft.com/library/mt346053.aspx) en stelt u de parameter **JobExecutionId** .

    $jobExecutionId = "{Job Execution Id}"
    Stop-AzureSqlJobExecution -JobExecutionId $jobExecutionId

## <a name="to-delete-a-job-and-job-history-asynchronously"></a>Een taak verwijderen en Taakgeschiedenis asynchroon

Elastische Database projecten ondersteunt asynchroon verwijderen van taken. Een taak kan worden gemarkeerd voor verwijdering en wordt verwijderd en het project de taakgeschiedenis nadat alle uitvoeringen van de taak voor de taak hebt voltooid. Het systeem wordt uitgevoerd van de actieve taak niet automatisch geannuleerd.  

[**Stop AzureSqlJobExecution**](https://msdn.microsoft.com/library/mt346053.aspx) uitvoeringen van de actieve taak annuleren als u wilt aanroepen.

Voor het verwijderen van taak starten, gebruikt u de [**cmdlet verwijderen AzureSqlJob**](https://msdn.microsoft.com/library/mt346083.aspx) en stelt u de parameter **Taaknaam** .

    $jobName = "{Job Name}"
    Remove-AzureSqlJob -JobName $jobName
 
## <a name="to-create-a-custom-database-target"></a>Het doel van een aangepaste database maken

U kunt aangepaste database doelen voor directe uitvoering of voor opname in een groep aangepaste database definiëren. Bijvoorbeeld omdat de **Database elastische toepassingen** worden nog niet rechtstreeks ondersteund met PowerShell APIs, kunt u een aangepaste database doel en het doel van een aangepaste database-collectie die alle databases in de groep omvat.

Stel de volgende variabelen om de gewenste database-informatie weer te geven:

    $databaseName = "{Database Name}"
    $databaseServerName = "{Server Name}"
    New-AzureSqlJobDatabaseTarget -DatabaseName $databaseName -ServerName $databaseServerName 

## <a name="to-create-a-custom-database-collection-target"></a>Een doel van de collectie aangepaste database maken

Gebruik de cmdlet [**New-AzureSqlJobTarget**](https://msdn.microsoft.com/library/mt346077.aspx) voor het definiëren van een doel van de collectie aangepaste database zodanig worden uitgevoerd over meerdere database gedefinieerde doelen. Na het maken van een database-groep, kunnen databases worden gekoppeld aan het doel van aangepaste verzamelingen.

De volgende variabelen zijn aangepast aan de gewenste aangepaste verzameling doelconfiguratie instellen:

    $customCollectionName = "{Custom Database Collection Name}"
    New-AzureSqlJobTarget -CustomCollectionName $customCollectionName 

### <a name="to-add-databases-to-a-custom-database-collection-target"></a>Databases toevoegen aan een aangepaste database collectie doel

Een database aan een specifieke aangepaste verzameling gebruiken om toe te voegen de cmdlet [**Add-AzureSqlJobChildTarget**](https://msdn.microsoft.comlibrary/mt346064.aspx) .

    $databaseServerName = "{Database Server Name}"
    $databaseName = "{Database Name}"
    $customCollectionName = "{Custom Database Collection Name}"
    Add-AzureSqlJobChildTarget -CustomCollectionName $customCollectionName -DatabaseName $databaseName -ServerName $databaseServerName 

#### <a name="review-the-databases-within-a-custom-database-collection-target"></a>De collectie aangepaste database doel-databases bekijken

Gebruik de cmdlet [**Get-AzureSqlJobTarget**](https://msdn.microsoft.com/library/mt346077.aspx) voor het ophalen van de onderliggende databases binnen een collectie aangepaste database doel. 
 
    $customCollectionName = "{Custom Database Collection Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    $childTargets = Get-AzureSqlJobTarget -ParentTargetId $target.TargetId
    Write-Output $childTargets

### <a name="create-a-job-to-execute-a-script-across-a-custom-database-collection-target"></a>Maak een taak voor het uitvoeren van een script in een collectie aangepaste database doel

Gebruik de cmdlet [**New-AzureSqlJob**](https://msdn.microsoft.com/library/mt346078.aspx) voor het maken van een taak ten opzichte van een groep van databases die zijn gedefinieerd door het doel van een aangepaste database-collectie. Elastische Database taken, wordt de taak uitbreiden naar meerdere onderliggende taken die elk overeenkomen met een database die is gekoppeld aan het doel van de collectie aangepaste database en ervoor te zorgen dat het script wordt uitgevoerd voor elke database. Ook is het belangrijk dat scripts idempotency is ingeschakeld zijn tegen pogingen worden.

    $jobName = "{Job Name}"
    $scriptName = "{Script Name}"
    $customCollectionName = "{Custom Collection Name}"
    $credentialName = "{Credential Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    $job = New-AzureSqlJob -JobName $jobName -CredentialName $credentialName -ContentName $scriptName -TargetId $target.TargetId
    Write-Output $job

## <a name="data-collection-across-databases"></a>Het verzamelen van gegevens over databases

Een taak kunt u een query uitvoeren op een groep van databases en de resultaten naar een specifieke tabel. De tabel kan worden doorzocht om te zien van de resultaten van de query uit de database. Dit biedt een asynchrone methode voor het uitvoeren van een query in veel databases. Mislukte pogingen worden automatisch verwerkt via pogingen.

De opgegeven doeltabel worden automatisch gemaakt als deze nog niet bestaat. De nieuwe tabel overeenkomt met het schema van de geretourneerde resultaten. Als een script meerdere resultaatsets retourneert, wordt elastische Database taken alleen de eerste verzonden naar de doeltabel.

De volgende PowerShell-script een script wordt uitgevoerd en verzamelt de resultaten in een opgegeven tabel. Dit script wordt ervan uitgegaan dat er een T-SQL-script is gemaakt die een enkele resultaatset oplevert en dat een doel van de collectie aangepaste database is gemaakt.

Dit script maakt gebruik van de cmdlet [**Get-AzureSqlJobTarget**](https://msdn.microsoft.com/library/mt346077.aspx) . Stel de parameters voor het script, referenties en uitvoering van doel:

    $jobName = "{Job Name}"
    $scriptName = "{Script Name}"
    $executionCredentialName = "{Execution Credential Name}"
    $customCollectionName = "{Custom Collection Name}"
    $destinationCredentialName = "{Destination Credential Name}"
    $destinationServerName = "{Destination Server Name}"
    $destinationDatabaseName = "{Destination Database Name}"
    $destinationSchemaName = "{Destination Schema Name}"
    $destinationTableName = "{Destination Table Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName

### <a name="to-create-and-start-a-job-for-data-collection-scenarios"></a>Maken en starten van een taak voor data collectie scenario 's

Dit script wordt de [**Start-AzureSqlJobExecution**](https://msdn.microsoft.com/library/mt346055.aspx) -cmdlet gebruikt.
 
    $job = New-AzureSqlJob -JobName $jobName 
    -CredentialName $executionCredentialName 
    -ContentName $scriptName 
    -ResultSetDestinationServerName $destinationServerName 
    -ResultSetDestinationDatabaseName $destinationDatabaseName 
    -ResultSetDestinationSchemaName $destinationSchemaName 
    -ResultSetDestinationTableName $destinationTableName 
    -ResultSetDestinationCredentialName $destinationCredentialName 
    -TargetId $target.TargetId
    Write-Output $job
    $jobExecution = Start-AzureSqlJobExecution -JobName $jobName
    Write-Output $jobExecution

## <a name="to-schedule-a-job-execution-trigger"></a>Voor het plannen van een taak kan worden uitgevoerd-trigger

De volgende PowerShell script kan worden gebruikt voor het maken van een terugkerend schema. Dit script wordt een minuut interval, maar ook ondersteuning voor [**Nieuw-AzureSqlJobSchedule**](https://msdn.microsoft.com/library/mt346068.aspx) - DayInterval, HourInterval-, - MonthInterval, en WeekInterval - parameters. Schema's die slechts eenmaal uitgevoerd kunnen worden gemaakt door doorgeven - eenmalige.

Maak een nieuwe planning:

    $scheduleName = "Every one minute"
    $minuteInterval = 1
    $startTime = (Get-Date).ToUniversalTime()
    $schedule = New-AzureSqlJobSchedule 
    -MinuteInterval $minuteInterval 
    -ScheduleName $scheduleName 
    -StartTime $startTime 
    Write-Output $schedule

### <a name="to-trigger-a-job-executed-on-a-time-schedule"></a>Voor het starten van een taak die wordt uitgevoerd op een tijdschema

De trigger voor een taak kan worden gedefinieerd als u wilt dat een taak die wordt uitgevoerd volgens een tijdschema. De volgende PowerShell script kan worden gebruikt voor het maken van een trigger voor de taak.

[Nieuw AzureSqlJobTrigger](https://msdn.microsoft.com/library/mt346069.aspx) en stel de volgende variabelen overeen te komen met de gewenste taak en planning:

    $jobName = "{Job Name}"
    $scheduleName = "{Schedule Name}"
    $jobTrigger = New-AzureSqlJobTrigger
    -ScheduleName $scheduleName
    –JobName $jobName
    Write-Output $jobTrigger

### <a name="to-remove-a-scheduled-association-to-stop-job-from-executing-on-schedule"></a>Een associatie geplande taak wordt uitgevoerd op de planning stopt verwijderen

Als u wilt stoppen met terugkerende taak kan worden uitgevoerd door een trigger voor de taak, kan de trigger voor de taak worden verwijderd. Verwijder een trigger van de taak om een taak uit te voeren op basis van een schema met de [**cmdlet verwijderen AzureSqlJobTrigger**](https://msdn.microsoft.com/library/mt346070.aspx)stoppen.

    $jobName = "{Job Name}"
    $scheduleName = "{Schedule Name}"
    Remove-AzureSqlJobTrigger 
    -ScheduleName $scheduleName 
    -JobName $jobName

### <a name="retrieve-job-triggers-bound-to-a-time-schedule"></a>Taak triggers die zijn gekoppeld aan een tijdschema ophalen

De volgende PowerShell script kunt ophalen en weergeven taak triggers geregistreerd voor een bepaald tijdschema worden gebruikt.

    $scheduleName = "{Schedule Name}"
    $jobTriggers = Get-AzureSqlJobTrigger -ScheduleName $scheduleName
    Write-Output $jobTriggers

### <a name="to-retrieve-job-triggers-bound-to-a-job"></a>U haalt de triggers van de taak afhankelijk is van een taak 

[Get AzureSqlJobTrigger](https://msdn.microsoft.com/library/mt346067.aspx) ophalen en weergeven met een geregistreerde taak schema's gebruiken.

    $jobName = "{Job Name}"
    $jobTriggers = Get-AzureSqlJobTrigger -JobName $jobName
    Write-Output $jobTriggers

## <a name="to-create-a-data-tier-application-dacpac-for-execution-across-databases"></a>Een gegevens-tier-toepassing (DACPAC) voor de uitvoering van verschillende databases maken

Zie [toepassingen gegevenslaag](https://msdn.microsoft.com/library/ee210546.aspx)maken van een DACPAC. Gebruik de [cmdlet New-AzureSqlJobContent](https://msdn.microsoft.com/library/mt346085.aspx)voor de implementatie van een DACPAC. De DACPAC moet toegankelijk zijn voor de service. Het wordt aanbevolen voor het uploaden van een DACPAC gemaakt naar Azure opslag en een [Gedeelde Access-handtekening](../storage/storage-dotnet-shared-access-signature-part-1.md) maken voor de DACPAC.

    $dacpacUri = "{Uri}"
    $dacpacName = "{Dacpac Name}"
    $dacpac = New-AzureSqlJobContent -DacpacUri $dacpacUri -ContentName $dacpacName 
    Write-Output $dacpac

### <a name="to-update-a-data-tier-application-dacpac-for-execution-across-databases"></a>Een gegevens-tier-toepassing (DACPAC) voor de uitvoering van verschillende databases bijwerken

Bestaande DACPACs geregistreerd binnen elastische Database taken kunnen worden bijgewerkt als u wilt verwijzen naar een nieuwe URI's. Gebruik de [**cmdlet Set-AzureSqlJobContentDefinition**](https://msdn.microsoft.com/library/mt346074.aspx) voor het bijwerken van de URI DACPAC op een bestaande geregistreerde DACPAC:

    $dacpacName = "{Dacpac Name}"
    $newDacpacUri = "{Uri}"
    $updatedDacpac = Set-AzureSqlJobDacpacDefinition -ContentName $dacpacName -DacpacUri $newDacpacUri
    Write-Output $updatedDacpac

## <a name="to-create-a-job-to-apply-a-data-tier-application-dacpac-across-databases"></a>Een taak om een gegevens-tier-toepassing (DACPAC) van toepassing op databases maken

Nadat een DACPAC binnen de elastische taken van de Database is gemaakt, kunt u een taak gemaakt om toe te passen van de DACPAC over een groep van databases. De volgende PowerShell script kan worden gebruikt voor het maken van een DACPAC project in een aangepaste verzameling databases:

    $jobName = "{Job Name}"
    $dacpacName = "{Dacpac Name}"
    $customCollectionName = "{Custom Collection Name}"
    $credentialName = "{Credential Name}"
    $target = Get-AzureSqlJobTarget 
    -CustomCollectionName $customCollectionName
    $job = New-AzureSqlJob 
    -JobName $jobName 
    -CredentialName $credentialName 
    -ContentName $dacpacName -TargetId $target.TargetId
    Write-Output $job 

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-jobs-powershell/cmd-prompt.png
[2]: ./media/sql-database-elastic-jobs-powershell/portal.png
<!--anchors-->
