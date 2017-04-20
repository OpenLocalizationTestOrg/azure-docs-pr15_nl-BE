<properties
    pageTitle="Aan de slag met elastische database taken"
    description="het gebruik van de elastische database taken"
    services="sql-database"
    documentationCenter=""  
    manager="jhubbard"
    authors="ddove"/>

<tags
    ms.service="sql-database"
    ms.workload="sql-database"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="ddove" />

# <a name="getting-started-with-elastic-database-jobs"></a>Aan de slag met elastische Database taken

Elastische Database taken (voorbeeld) voor Azure SQL-Database kunt u betrouwbaarheid bij het uitvoeren van scripts met T-SQL die zich uitstrekken over meerdere databases terwijl u automatisch opnieuw wordt geprobeerd het uiteindelijke voltooiing garanties bieden. Zie de [pagina met functies](sql-database-elastic-jobs-overview.md)voor meer informatie over de functie van de taak elastische Database.

In dit onderwerp wordt het monster gevonden in [aan de slag met elastische Databasehulpmiddelen](sql-database-elastic-scale-get-started.md)uitgebreid. Wanneer voltooid, zal u: informatie over het maken en beheren van taken die een groep van gerelateerde databases beheren. Het is niet vereist voor het gebruik van de extra elastische schaal als u wilt profiteren van de voordelen van de elastische taken.

## <a name="prerequisites"></a>Vereisten

Downloaden en uitvoeren van de [aan de slag met elastische Database extra monster](sql-database-elastic-scale-get-started.md).

## <a name="create-a-shard-map-manager-using-the-sample-app"></a>Maak een shard kaart manager met behulp van het monster app

Hier maakt u een kaart shard manager en verschillende shards, gevolgd door het invoegen van gegevens in de shards. Hebt u al shards bij een laptopgeheugen gegevens instellen, kunt u de volgende stappen overslaan en verplaatsen naar de volgende sectie.

1. Bouwen en uitvoeren van de voorbeeldtoepassing **aan de slag met elastische Databasehulpmiddelen** . Volg de stappen tot stap 7 in de sectie [downloaden en uitvoeren van het monster app](sql-database-elastic-scale-get-started.md#Getting-started-with-elastic-database-tools). Aan het einde van stap 7 ziet u de volgende opdrachtregel:

    ![MS-DOS-prompt][1]

2.  Typ "1" in het opdrachtvenster en druk op **Enter**. Dit maakt de shard kaart manager en twee shards toegevoegd aan de server. Vervolgens typt u "3" en druk op **Enter**. Herhaal deze actie vier keer. Dit invoegen monster gegevensrijen in uw shards.

3.  De [Azure Portal](https://portal.azure.com) moet drie nieuwe databases die worden weergegeven in de v12-server:

    ![Visual Studio-bevestiging][2]

    Op dit moment maken we een aangepaste database-collectie die alle databases in de overzichtsweergave shard weerspiegelt. Dit kunnen we maken en uitvoeren van een taak die een nieuwe tabel toevoegen via shards.

Hier zou meestal maken we een doel toewijzen voor shard met de cmdlet **New-AzureSqlJobTarget** . De database manager shard moet worden ingesteld als het doel van een database en vervolgens de specifieke shard-map als doel wordt opgegeven. We gaan in plaats daarvan voor het opsommen van alle databases op de server en de databases toevoegen aan de nieuwe aangepaste collectie, met uitzondering van de master-database.

##<a name="creates-a-custom-collection-and-adds-all-databases-in-the-server-to-the-custom-collection-target-with-the-exception-of-master"></a>Hiermee maakt u een aangepaste verzameling en alle databases op de server toegevoegd aan de collectie aangepaste doel, met uitzondering van de master.


    $customCollectionName = "dbs_in_server"
    New-AzureSqlJobTarget -CustomCollectionName $customCollectionName 
    $ResourceGroupName = "ddove_samples"
    $ServerName = "samples"
    $dbsinserver = Get-AzureRMSqlDatabase -ResourceGroupName $ResourceGroupName -ServerName $ServerName 
    $dbsinserver | %{
    $currentdb = $_.DatabaseName 
    $ErrorActionPreference = "Stop"
    Write-Output ""

    Try
    {
       New-AzureSqlJobTarget -ServerName $ServerName -DatabaseName $currentdb | Write-Output
    }
    Catch 
    {
        $ErrorMessage = $_.Exception.Message
        $ErrorCategory = $_.CategoryInfo.Reason
                
        if ($ErrorCategory -eq 'UniqueConstraintViolatedException')
        {
             Write-Host $currentdb "is already a database target." 
        }

        else
        {
            throw $_
        }
    
    }

    Try
    {
        if ($currentdb -eq "master")
        {
            Write-Host $currentdb "will not be added custom collection target" $CustomCollectionName "."
        }

        else
        {
            Add-AzureSqlJobChildTarget -CustomCollectionName $CustomCollectionName -ServerName $ServerName -DatabaseName $currentdb
            Write-Host $currentdb "was added to" $CustomCollectionName "."
        }

    }
    Catch 
    {
        $ErrorMessage = $_.Exception.Message
        $ErrorCategory = $_.CategoryInfo.Reason

        if ($ErrorCategory -eq 'UniqueConstraintViolatedException')
        {
             Write-Host $currentdb "is already in the custom collection target" $CustomCollectionName"."
        }

        else
        {
            throw $_
        }
    }
    $ErrorActionPreference = "Continue"
}
    
## <a name="create-a-t-sql-script-for-execution-across-databases"></a>Een T-SQL-Script voor het uitvoeren van verschillende databases maken

    $scriptName = "NewTable"
    $scriptCommandText = "
    IF NOT EXISTS (SELECT name FROM sys.tables WHERE name = 'Test')
    BEGIN
        CREATE TABLE Test(
            TestId INT PRIMARY KEY IDENTITY,
            InsertionTime DATETIME2
        );
    END
    GO
    INSERT INTO Test(InsertionTime) VALUES (sysutcdatetime());
    GO"

    $script = New-AzureSqlJobContent -ContentName $scriptName -CommandText $scriptCommandText
    Write-Output $script

##<a name="create-the-job-to-execute-a-script-across-the-custom-group-of-databases"></a>De taak voor het uitvoeren van een script in de aangepaste groep van databases maken

    $jobName = "create on server dbs"
    $scriptName = "NewTable"
    $customCollectionName = "dbs_in_server"
    $credentialName = "ddove66"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    $job = New-AzureSqlJob -JobName $jobName -CredentialName $credentialName -ContentName $scriptName -TargetId $target.TargetId
    Write-Output $job


##<a name="execute-the-job"></a>De taak uitvoeren 

De volgende PowerShell script kan worden gebruikt voor het uitvoeren van een bestaande taak:

Update van de volgende variabele, zodat de naam van de gewenste taak hebt uitgevoerd:

    $jobName = "create on server dbs"
    $jobExecution = Start-AzureSqlJobExecution -JobName $jobName 
    Write-Output $jobExecution
 
## <a name="retrieve-the-state-of-a-single-job-execution"></a>De status van de uitvoering van een enkele taak ophalen

De dezelfde **Get-AzureSqlJobExecution** -cmdlet gebruiken met de parameter **IncludeChildren** om de status van de onderliggende uitvoering, namelijk de specifieke status voor elke taak worden uitgevoerd voor elke database gericht door de taak weer te geven.

    $jobExecutionId = "{Job Execution Id}"
    $jobExecutions = Get-AzureSqlJobExecution -JobExecutionId $jobExecutionId -IncludeChildren
    Write-Output $jobExecutions 

## <a name="view-the-state-across-multiple-job-executions"></a>De status weergeven in meerdere uitvoeringen van taak

De cmdlet **Get-AzureSqlJobExecution** heeft meerdere optionele parameters die kunnen worden gebruikt om meerdere uitvoering, gefilterd door de opgegeven parameters weer te geven. Hieronder ziet u enkele van de mogelijke manieren om Get-AzureSqlJobExecution:

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

## <a name="retrieve-failures-within-job-task-executions"></a>Storingen in uitvoering taak ophalen

Het object JobTaskExecution bevat een eigenschap voor de levenscyclus van de taak in combinatie met de berichteigenschap van een. Een taak taak niet uitvoeren, de levenscyclus van de eigenschap wordt ingesteld op *mislukt* als de eigenschap van het bericht ingesteld op het resulterende uitzonderingsbericht en de stack. Als een taak niet is gelukt, is het belangrijk dat u de details weergeven van taken die voor een bepaalde taak is mislukt.

    $jobExecutionId = "{Job Execution Id}"
    $jobTaskExecutions = Get-AzureSqlJobTaskExecution -JobExecutionId $jobExecutionId
    Foreach($jobTaskExecution in $jobTaskExecutions) 
        {
        if($jobTaskExecution.Lifecycle -ne 'Succeeded')
            {
            Write-Output $jobTaskExecution
            }
        }

## <a name="waiting-for-a-job-execution-to-complete"></a>Wachten op de taakuitvoering van een te voltooien

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
    $executionPolicy = New-AzureSqlJobExecutionPolicy -ExecutionPolicyName $executionPolicyName -InitialRetryInterval $initialRetryInterval -JobTimeout $jobTimeout -MaximumAttempts $maximumAttempts -MaximumRetryInterval $maximumRetryInterval -RetryIntervalBackoffCoefficient $retryIntervalBackoffCoefficient
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

Elastische Database projecten ondersteunt projecten annulering aanvragen.  Als taken met elastische Database een annulering aanvragen voor een project dat momenteel wordt uitgevoerd detecteert, wordt geprobeerd de taak stoppen.

Er zijn twee verschillende manieren dat een annulering door elastische Database taken kunt uitvoeren:

1. Dat momenteel wordt uitgevoerd als u annuleert taken: een annulering als een annulering wordt aangetroffen terwijl een taak wordt uitgevoerd, worden geprobeerd in de momenteel uitgevoerde aspect van de taak.  Bijvoorbeeld: als er een langdurige query momenteel wordt uitgevoerd wanneer een annulering wordt uitgevoerd, zal er een poging tot het annuleren van de query.
2. Annuleren taak pogingen: Een annulering wordt gedetecteerd door de thread van het besturingselement, voordat een taak voor uitvoering wordt gestart, de thread van het besturingselement wordt te voorkomen dat de taak wordt gestart als de aanvraag declareren als geannuleerd.

Als u een annulering voor een taak wordt aangevraagd voor een bovenliggende functie, het verzoek om annulering wordt van kracht voor de bovenliggende taak en alle bijbehorende onderliggende taken.
 
Een annuleringsverzoek verzenden, gebruikt u de cmdlet **Stop AzureSqlJobExecution** en stelt u de parameter **JobExecutionId** .

    $jobExecutionId = "{Job Execution Id}"
    Stop-AzureSqlJobExecution -JobExecutionId $jobExecutionId

## <a name="delete-a-job-by-name-and-the-jobs-history"></a>Een taak met de naam en de geschiedenis van de taak verwijderen

Elastische Database projecten ondersteunt asynchroon verwijderen van taken. Een taak kan worden gemarkeerd voor verwijdering en wordt verwijderd en het project de taakgeschiedenis nadat alle uitvoeringen van de taak voor de taak hebt voltooid. Het systeem wordt uitgevoerd van de actieve taak niet automatisch geannuleerd.  

In plaats daarvan moet Stop AzureSqlJobExecution worden aangeroepen om de uitvoeringen van de actieve taak annuleren.

Voor het verwijderen van taak starten, gebruikt u de cmdlet **Verwijderen AzureSqlJob** en stelt u de parameter **Taaknaam** .

    $jobName = "{Job Name}"
    Remove-AzureSqlJob -JobName $jobName
 
## <a name="create-a-custom-database-target"></a>Het doel van een aangepaste database maken
Aangepaste database doelen kunnen worden gedefinieerd in elastische Database taken die kunnen worden gebruikt om rechtstreeks of opgenomen in een aangepaste database-groep. Aangezien **elastische Database toepassingen** worden nog niet rechtstreeks ondersteund via de APIs PowerShell, hoeft u een aangepaste database doel en het doel van een aangepaste database-collectie die alle databases in de groep omvat.

Stel de volgende variabelen om de gewenste database-informatie weer te geven:

    $databaseName = "{Database Name}"
    $databaseServerName = "{Server Name}"
    New-AzureSqlJobDatabaseTarget -DatabaseName $databaseName -ServerName $databaseServerName 

## <a name="create-a-custom-database-collection-target"></a>Een doel van de collectie aangepaste database maken
Een verzameling aangepaste database doel kan worden gedefinieerd zodat kan worden uitgevoerd op meerdere database gedefinieerde doelen. Nadat een groep van de database is gemaakt, kunnen databases worden gekoppeld aan de collectie aangepaste doel.

De volgende variabelen zijn aangepast aan de gewenste aangepaste verzameling doelconfiguratie instellen:

    $customCollectionName = "{Custom Database Collection Name}"
    New-AzureSqlJobTarget -CustomCollectionName $customCollectionName 

### <a name="add-databases-to-a-custom-database-collection-target"></a>Databases toevoegen aan een aangepaste database collectie doel

Doelen van de database kunnen worden gekoppeld aan de doelen van de aangepaste database-collectie om een groep van databases te maken. Wanneer een taak wordt gemaakt die gericht is op een doel van de collectie aangepaste database, zal het worden uitgebreid als doel de databases die zijn gekoppeld aan de groep op het moment van uitvoering.

De gewenste database toevoegen aan een specifieke aangepaste collectie:

    $serverName = "{Database Server Name}"
    $databaseName = "{Database Name}"
    $customCollectionName = "{Custom Database Collection Name}"
    Add-AzureSqlJobChildTarget -CustomCollectionName $customCollectionName -DatabaseName $databaseName -ServerName $databaseServerName 

#### <a name="review-the-databases-within-a-custom-database-collection-target"></a>De collectie aangepaste database doel-databases bekijken

Gebruik de cmdlet **Get-AzureSqlJobTarget** voor het ophalen van de onderliggende databases binnen een collectie aangepaste database doel. 
 
    $customCollectionName = "{Custom Database Collection Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    $childTargets = Get-AzureSqlJobTarget -ParentTargetId $target.TargetId
    Write-Output $childTargets

### <a name="create-a-job-to-execute-a-script-across-a-custom-database-collection-target"></a>Maak een taak voor het uitvoeren van een script in een collectie aangepaste database doel

Gebruik de cmdlet **New-AzureSqlJob** voor het maken van een taak ten opzichte van een groep van databases die zijn gedefinieerd door het doel van een aangepaste database-collectie. Elastische Database taken, wordt de taak uitbreiden naar meerdere onderliggende taken die elk overeenkomen met een database die is gekoppeld aan het doel van de collectie aangepaste database en ervoor te zorgen dat het script wordt uitgevoerd voor elke database. Ook is het belangrijk dat scripts idempotency is ingeschakeld zijn tegen pogingen worden.

    $jobName = "{Job Name}"
    $scriptName = "{Script Name}"
    $customCollectionName = "{Custom Collection Name}"
    $credentialName = "{Credential Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    $job = New-AzureSqlJob -JobName $jobName -CredentialName $credentialName -ContentName $scriptName -TargetId $target.TargetId
    Write-Output $job

## <a name="data-collection-across-databases"></a>Het verzamelen van gegevens over databases

**Taken van de elastische Database** ondersteunt het uitvoeren van een query over een groep van databases en stuurt de resultaten naar een opgegeven databasetabel. De tabel kan worden doorzocht om te zien van de resultaten van de query uit de database. Dit is een asynchrone mechanisme voor het uitvoeren van een query in veel databases. Gevallen mislukken als een van de databases wordt tijdelijk niet beschikbaar worden automatisch verwerkt via pogingen.

De opgegeven doeltabel worden automatisch gemaakt als deze nog niet bestaat, overeenkomen met het schema van de geretourneerde resultaten. Als de uitvoering van een script meerdere resultaatsets retourneert, verzendt elastische Database taken alleen de eerste die aan de opgegeven doeltabel.

De volgende PowerShell script kan worden gebruikt voor het uitvoeren van een script dat de resultaten verzamelen in een opgegeven tabel. Dit script wordt ervan uitgegaan dat een T-SQL-script is gemaakt die een enkele resultaatset oplevert en een collectie aangepaste database doel is gemaakt.

Stel het volgende om het gewenste script, referenties en uitvoering van doel weer te geven:

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

### <a name="create-and-start-a-job-for-data-collection-scenarios"></a>Maken en starten van een taak voor data collectie scenario 's
    $job = New-AzureSqlJob -JobName $jobName -CredentialName $executionCredentialName -ContentName $scriptName -ResultSetDestinationServerName $destinationServerName -ResultSetDestinationDatabaseName $destinationDatabaseName -ResultSetDestinationSchemaName $destinationSchemaName -ResultSetDestinationTableName $destinationTableName -ResultSetDestinationCredentialName $destinationCredentialName -TargetId $target.TargetId
    Write-Output $job
    $jobExecution = Start-AzureSqlJobExecution -JobName $jobName
    Write-Output $jobExecution

## <a name="create-a-schedule-for-job-execution-using-a-job-trigger"></a>Een schema voor de taak kan worden uitgevoerd met behulp van de trigger voor een taak maken

De volgende PowerShell script kan worden gebruikt om een terugkerende planning te maken. Dit script wordt een interval van één minuut, maar ook ondersteuning voor nieuw-AzureSqlJobSchedule - DayInterval, HourInterval-, - MonthInterval, en WeekInterval - parameters. Schema's die slechts eenmaal uitgevoerd kunnen worden gemaakt door doorgeven - eenmalige.

Maak een nieuwe planning:

    $scheduleName = "Every one minute"
    $minuteInterval = 1
    $startTime = (Get-Date).ToUniversalTime()
    $schedule = New-AzureSqlJobSchedule -MinuteInterval $minuteInterval -ScheduleName $scheduleName -StartTime $startTime 
    Write-Output $schedule

### <a name="create-a-job-trigger-to-have-a-job-executed-on-a-time-schedule"></a>Een trigger taak als u wilt dat een taak die wordt uitgevoerd op een tijdschema maken

De trigger voor een taak kan worden gedefinieerd als u wilt dat een taak die wordt uitgevoerd volgens een tijdschema. De volgende PowerShell script kan worden gebruikt voor het maken van een trigger voor de taak.

Stel de volgende variabelen overeen te komen met de gewenste taak en planning:

    $jobName = "{Job Name}"
    $scheduleName = "{Schedule Name}"
    $jobTrigger = New-AzureSqlJobTrigger -ScheduleName $scheduleName –JobName $jobName
    Write-Output $jobTrigger

### <a name="remove-a-scheduled-association-to-stop-job-from-executing-on-schedule"></a>Een koppeling geplande taak wordt uitgevoerd op de planning stopt verwijderen

Als u wilt stoppen met terugkerende taak kan worden uitgevoerd door een trigger voor de taak, kan de trigger voor de taak worden verwijderd. Verwijder een trigger van de taak om een taak uit te voeren op basis van een schema met de cmdlet **Verwijderen AzureSqlJobTrigger** stoppen.

    $jobName = "{Job Name}"
    $scheduleName = "{Schedule Name}"
    Remove-AzureSqlJobTrigger -ScheduleName $scheduleName -JobName $jobName





## <a name="import-elastic-database-query-results-to-excel"></a>Resultaten van een databasequery elastische importeren in Excel

 U kunt de resultaten van een query naar een Excel-bestand importeren.

1. Start Excel 2013.
2.  Ga naar het lint **gegevens** .
3.  **Van andere bronnen** en klik op **SQL Server uit**.

    ![Excel importeren uit andere bronnen][5]
4.  Typ de naam en de aanmelding referenties in de **Wizard Gegevensverbinding** . Klik vervolgens op **volgende**.
5.  In het dialoogvenster **Selecteer de database die de gegevens bevat**, selecteert u de database **ElasticDBQuery** .
6.  Selecteer de tabel **klanten** in de lijstweergave en klik op **volgende**. Klik vervolgens op **Voltooien**.
7.  In het formulier **Gegevens importeren** onder **Selecteer hoe u deze gegevens in de werkmap weergeven**, selecteert u **tabel** en klik op **OK**.

Alle rijen uit de tabel **klanten** , opgeslagen in verschillende shards vullen het Excel-werkblad.

## <a name="next-steps"></a>Volgende stappen
Nu kunt u functies van Excel gegevens. De verbindingsreeks gebruiken met uw naam, naam van de database en referenties verbinding maken met uw gegevens en BI-integratiefuncties de elastische querydatabase. Zorg ervoor dat de SQL Server als gegevensbron voor het hulpprogramma wordt ondersteund. Raadpleeg de querydatabase elastische en externe tabellen, net als elke andere SQL Server-database en SQL Server-tabellen waarmee u verbinding met het gereedschap maken wilt.

### <a name="cost"></a>Kosten
Er is geen extra kosten voor het gebruik van de elastische Database query functie. Hoewel deze functie is momenteel alleen beschikbaar in premium-databases als een eindpunt, de shards van een service kunnen worden.

Zie [SQL Database prijzen Details](https://azure.microsoft.com/pricing/details/sql-database/)voor prijsinformatie.


[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-query-getting-started/cmd-prompt.png
[2]: ./media/sql-database-elastic-query-getting-started/portal.png
[3]: ./media/sql-database-elastic-query-getting-started/tiers.png
[4]: ./media/sql-database-elastic-query-getting-started/details.png
[5]: ./media/sql-database-elastic-query-getting-started/exel-sources.png
<!--anchors-->
