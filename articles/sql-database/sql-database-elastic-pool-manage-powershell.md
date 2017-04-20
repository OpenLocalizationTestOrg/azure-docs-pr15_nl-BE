<properties 
    pageTitle="Beheren van een elastische database-toepassingen (PowerShell) | Microsoft Azure" 
    description="Leer een elastische database toepassingen beheren met PowerShell."  
    services="sql-database" 
    documentationCenter="" 
    authors="srinia" 
    manager="jhubbard" 
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="powershell"
    ms.workload="data-management" 
    ms.date="06/22/2016"
    ms.author="srinia"/>

# <a name="monitor-and-manage-an-elastic-database-pool-with-powershell"></a>Bewaken en beheren van een elastische database toepassingen met PowerShell 

> [AZURE.SELECTOR]
- [Azure portal](sql-database-elastic-pool-manage-portal.md)
- [PowerShell](sql-database-elastic-pool-manage-powershell.md)
- [C#](sql-database-elastic-pool-manage-csharp.md)
- [T-SQL](sql-database-elastic-pool-manage-tsql.md)

Een [groep met elastische database](sql-database-elastic-pool.md) met PowerShell-cmdlets beheren. 

Zie voor algemene foutcodes, [foutcodes van de SQL voor SQL-Database-clienttoepassingen: verbindingsfout en andere problemen](sql-database-develop-error-messages.md).

Waarden voor groepen vindt u in de [opslag-en eDTU](sql-database-elastic-pool.md#eDTU-and-storage-limits-for-elastic-pools-and-elastic-databases). 

## <a name="prerequisites"></a>Vereisten

* Azure PowerShell 1.0 of hoger. Zie voor gedetailleerde informatie [het installeren en configureren van Azure PowerShell](../powershell-install-configure.md).
* Elastische database toepassingen zijn alleen beschikbaar in de SQL-Database V12 servers. Als u een V11: Database van SQL server, [gebruikt u PowerShell te upgraden naar V12 en een groep maken](sql-database-upgrade-server-portal.md) in één stap.


## <a name="move-a-database-into-an-elastic-pool"></a>Een database verplaatsen naar een elastische toepassingen

U kunt een database verplaatsen naar of uit een groep met de [Set AzureRmSqlDatabase](https://msdn.microsoft.com/library/azure/mt619433.aspx). 

    Set-AzureRmSqlDatabase -ResourceGroupName "resourcegroup1" -ServerName "server1" -DatabaseName "database1" -ElasticPoolName "elasticpool1"

## <a name="change-performance-settings-of-a-pool"></a>Prestatie-instellingen van een groep wijzigen

Als prestaties suffers, kunt u de instellingen van de groep ten behoeve van groei. Gebruik de cmdlet [Set-AzureRmSqlElasticPool](https://msdn.microsoft.com/library/azure/mt603511.aspx) . Stel de parameter - Dtu op de eDTUs per groep van toepassingen. Zie [opslag-en eDTU](sql-database-elastic-pool.md#eDTU-and-storage-limits-for-elastic-pools-and-elastic-databases) voor mogelijke waarden.  

    Set-AzureRmSqlElasticPool –ResourceGroupName “resourcegroup1” –ServerName “server1” –ElasticPoolName “elasticpool1” –Dtu 1200 –DatabaseDtuMax 100 –DatabaseDtuMin 50 


## <a name="get-the-status-of-pool-operations"></a>De status van de groep van toepassingen

Een groep maken kan enige tijd duren. Gebruik de cmdlet [Get-AzureRmSqlElasticPoolActivity](https://msdn.microsoft.com/library/azure/mt603812.aspx) voor het bijhouden van de status van een groep bewerkingen zoals het maken en updates.

    Get-AzureRmSqlElasticPoolActivity –ResourceGroupName “resourcegroup1” –ServerName “server1” –ElasticPoolName “elasticpool1” 


## <a name="get-the-status-of-moving-an-elastic-database-into-and-out-of-a-pool"></a>De status van een elastische database verplaatsen in en uit een groep opvragen

Een database verplaatsen kan enige tijd duren. De status van een verplaatsing met de cmdlet [Get-AzureRmSqlDatabaseActivity](https://msdn.microsoft.com/library/azure/mt603687.aspx) bijhouden.

    Get-AzureRmSqlDatabaseActivity -ResourceGroupName "resourcegroup1" -ServerName "server1" -DatabaseName "database1" -ElasticPoolName "elasticpool1"

## <a name="get-resource-usage-data-for-a-pool"></a>Gegevens over brongebruik ophalen voor een groep

De parameters die kunnen worden opgehaald als een percentage van de limiet van de bron:   


| Metrische naam | Beschrijving |
| :-- | :-- |
| CPU\_procent | Gemiddelde berekenen voor gebruik in een percentage van de limiet van de groep. |
| fysieke\_data\_lezen\_procent | Gemiddeld gebruik van i/o-percentage is gebaseerd op de grens van de pool. |
| logboek\_schrijven\_procent | Gemiddelde schrijven gebruik van bronnen in een percentage van de limiet van de groep. | 
| DTU\_verbruik\_procent | Gebruik eDTU gemiddelde percentage van de limiet voor de groep eDTU | 
| opslag\_procent | Gemiddelde gebruik van de opslagruimte in een percentage van de opslaglimiet van de pool. |  
| werknemers\_procent | Maximale gelijktijdige werknemers (aanvragen) in een percentage is gebaseerd op de grens van de pool. |  
| sessies\_procent | Maximale aantal gelijktijdige sessies in een percentage is gebaseerd op de grens van de pool. | 
| eDTU_limit | Huidige maximale elastische pool DTU instelling voor deze groep elastische tijdens dit interval. |
| opslag\_beperken | Huidige maximale elastische pool opslaglimiet voor deze elastische toepassingen in megabytes tijdens dit interval instellen. |
| eDTU\_gebruikt | Gemiddelde eDTUs gebruikt door de toepassingen in dit interval. |
| opslag\_gebruikt | Gemiddelde opslag gebruikt door de toepassingen in dit interval in bytes |

**Metrics granulatie/aanhoudperiode:**

* Op 5 minuten granulatie worden gegevens geretourneerd.  
* Bewaren van gegevens is 35 dagen.  

Deze cmdlet en API beperkt het aantal rijen dat in één aanroep van 1000 rijen (ongeveer 3 dagen bij 5 minuut granulatie) kunnen worden opgehaald. Maar met deze opdracht wordt meerdere keren met verschillende begin/einde tijdsintervallen meer gegevens op te halen 

De parameters ophalen:

    $metrics = (Get-AzureRmMetric -ResourceId /subscriptions/<subscriptionId>/resourceGroups/FabrikamData01/providers/Microsoft.Sql/servers/fabrikamsqldb02/elasticPools/franchisepool -TimeGrain ([TimeSpan]::FromMinutes(5)) -StartTime "4/18/2015" -EndTime "4/21/2015")  


## <a name="get-resource-usage-data-for-an-elastic-database"></a>Gegevens over brongebruik voor een elastische database ophalen

Deze API's zijn hetzelfde als de huidige (V12)-API's gebruikt voor het toezicht op het gebruik van bronnen van een zelfstandige database, met uitzondering van de volgende semantische verschil. 

Voor deze API, metrics opgehaald worden uitgedrukt als een percentage van de per max eDTUs (of gelijkwaardige cap voor de onderliggende waarde zoals CPU, IO enz) ingesteld voor die groep. Bijvoorbeeld 50% gebruik van een van deze gegevens geeft aan dat het verbruik van bepaalde middelen aan 50% van de per database cap limiet voor de resource in de bovenliggende groep. 

De parameters ophalen:

    $metrics = (Get-AzureRmMetric -ResourceId /subscriptions/<subscriptionId>/resourceGroups/FabrikamData01/providers/Microsoft.Sql/servers/fabrikamsqldb02/databases/myDB -TimeGrain ([TimeSpan]::FromMinutes(5)) -StartTime "4/18/2015" -EndTime "4/21/2015") 

## <a name="add-an-alert-to-a-pool-resource"></a>Een waarschuwing toevoegen aan een resourcegroep resource

U kunt regels toevoegt aan resources e-mailberichten of tekenreeksen waarschuwingen verzenden naar [URL eindpunten](https://msdn.microsoft.com/library/mt718036.aspx) wanneer de bron raakt een drempel die u hebt ingesteld voor gebruik. Gebruik de cmdlet Add-AzureRmMetricAlertRule. 

> [AZURE.IMPORTANT]Brongebruik controleren voor elastische toepassingen heeft een vertraging van ten minste 20 minuten. Instellen van waarschuwingen van minder dan 30 minuten voor elastische toepassingen wordt momenteel niet ondersteund. Waarschuwingen instellen voor elastisch van toepassingen met een punt (parameter met de naam "-venstergrootte" in PowerShell API) van minder dan 30 minuten wordt niet geactiveerd. Zorg ervoor dat de waarschuwingen die u voor groepen van elastische definieert een periode (WindowSize) van 30 minuten of langer.

In dit voorbeeld wordt een waarschuwing voor het ophalen van een melding wanneer een groep van toepassingen eDTU verbruik boven een bepaalde drempel gaat.

    # Set up your resource ID configurations
    $subscriptionId = '<Azure subscription id>'      # Azure subscription ID
    $location =  '<location'                         # Azure region
    $resourceGroupName = '<resource group name>'     # Resource Group
    $serverName = '<server name>'                    # server name
    $poolName = '<elastic pool name>'                # pool name 

    #$Target Resource ID
    $ResourceID = '/subscriptions/' + $subscriptionId + '/resourceGroups/' +$resourceGroupName + '/providers/Microsoft.Sql/servers/' + $serverName + '/elasticpools/' + $poolName

    # Create an email action
    $actionEmail = New-AzureRmAlertRuleEmail -SendToServiceOwners -CustomEmail JohnDoe@contoso.com

    # create a unique rule name
    $alertName = $poolName + "- DTU consumption rule"

    # Create an alert rule for DTU_consumption_percent
    Add-AzureRMMetricAlertRule -Name $alertName -Location $location -ResourceGroup $resourceGroupName -TargetResourceId $ResourceID -MetricName "DTU_consumption_percent"  -Operator GreaterThan -Threshold 80 -TimeAggregationOperator Average -WindowSize 00:60:00 -Actions $actionEmail 

## <a name="add-alerts-to-all-databases-in-a-pool"></a>Waarschuwingen voor alle databases in een groep toevoegen

U kunt regels toevoegen aan alle databaseobjecten in een elastische toepassingen voor het verzenden van e-mailberichten of waarschuwing tekenreeksen [eindpunten URL](https://msdn.microsoft.com/library/mt718036.aspx) wanneer een resource een drempel voor gebruik door de waarschuwing ingesteld raakt. 

> [AZURE.IMPORTANT] Brongebruik controleren voor elastische toepassingen heeft een vertraging van ten minste 20 minuten. Instellen van waarschuwingen van minder dan 30 minuten voor elastische toepassingen wordt momenteel niet ondersteund. Waarschuwingen instellen voor elastisch van toepassingen met een punt (parameter met de naam "-venstergrootte" in PowerShell API) van minder dan 30 minuten wordt niet geactiveerd. Zorg ervoor dat de waarschuwingen die u voor groepen van elastische definieert een periode (WindowSize) van 30 minuten of langer.

In dit voorbeeld wordt een waarschuwing toegevoegd aan elk van de databases in een groep van toepassingen voor het opvragen van een melding wanneer DTU-verbruik van de database boven een bepaalde drempel gaat.

    # Set up your resource ID configurations
    $subscriptionId = '<Azure subscription id>'      # Azure subscription ID
    $location = '<location'                          # Azure region
    $resourceGroupName = '<resource group name>'     # Resource Group
    $serverName = '<server name>'                    # server name
    $poolName = '<elastic pool name>'                # pool name 

    # Get the list of databases in this pool.
    $dbList = Get-AzureRmSqlElasticPoolDatabase -ResourceGroupName $resourceGroupName -ServerName $serverName -ElasticPoolName $poolName

    # Create an email action
    $actionEmail = New-AzureRmAlertRuleEmail -SendToServiceOwners -CustomEmail JohnDoe@contoso.com

    # Get resource usage metrics for a database in an elastic database for the specified time interval.
    foreach ($db in $dbList)
    {
    $dbResourceId = '/subscriptions/' + $subscriptionId + '/resourceGroups/' + $resourceGroupName + '/providers/Microsoft.Sql/servers/' + $serverName + '/databases/' + $db.DatabaseName

    # create a unique rule name
    $alertName = $db.DatabaseName + "- DTU consumption rule"

    # Create an alert rule for DTU_consumption_percent
    Add-AzureRMMetricAlertRule -Name $alertName  -Location $location -ResourceGroup $resourceGroupName -TargetResourceId $dbResourceId -MetricName "dtu_consumption_percent"  -Operator GreaterThan -Threshold 80 -TimeAggregationOperator Average -WindowSize 00:60:00 -Actions $actionEmail

    # drop the alert rule
    #Remove-AzureRmAlertRule -ResourceGroup $resourceGroupName -Name $alertName
    } 



## <a name="collect-and-monitor-resource-usage-data-across-multiple-pools-in-a-subscription"></a>Verzamelen en bijhouden van gegevens over brongebruik over meerdere toepassingen in een abonnement

Wanneer u een groot aantal databases in een abonnement hebt, is het lastig om elke elastische toepassingen afzonderlijk controleren. In plaats daarvan kunnen SQL database PowerShell-cmdlets en T-SQL-query's worden gecombineerd voor het verzamelen van gegevens over brongebruik van meerdere groepen en hun databases voor toezicht op en analyse van de weergave Resourcegebruik. Een [Voorbeeldimplementatie](https://github.com/Microsoft/sql-server-samples/tree/master/samples/manage/azure-sql-db-elastic-pools) van deze set van powershell scripts vindt u in de opslagplaats van de monsters GitHub SQL Server samen met wat het doet en hoe u deze kunt gebruiken.

Gebruik deze voorbeeldimplementatie volgt hieronder.


1. De [scripts en documentatie](https://github.com/Microsoft/sql-server-samples/tree/master/samples/manage/azure-sql-db-elastic-pools)downloaden:
2. De scripts voor uw omgeving wijzigen. Geef een of meer servers waarop elastische toepassingen worden gehost.
3. Geef een telemetrie database waar de verzamelde gegevens zullen worden opgeslagen. 
4. Pas het script voor het opgeven van de duur van het uitvoeren van scripts'.

Op een hoog niveau, de scripts als volgt:

*   Inventariseert alle servers in een bepaald abonnement Azure (of een opgegeven lijst van servers).
*   Een achtergrondtaak voor elke server wordt uitgevoerd. De taak wordt uitgevoerd in een lus met regelmatige tussenpozen en telemetriegegevens voor de toepassingen op de server worden verzameld. Vervolgens worden de verzamelde gegevens in de database opgegeven telemetrie geladen.
*   Een lijst met databases in elke groep van toepassingen voor het verzamelen van de gegevens over brongebruik database opgesomd. Vervolgens worden de verzamelde gegevens in de database telemetrie geladen.

De verzamelde statistieken in de telemetrie-database kunnen worden geanalyseerd om te controleren van de gezondheid van de elastische pools en de databases in het. Het script installeert ook een vooraf gedefinieerde waarde functie (TVF) in de telemetrie database waarmee u samengestelde metrics voor een venster. Resultaten van de TVF kunnen bijvoorbeeld worden gebruikt weergeven "top N elastische pools met het gebruik van de maximale eDTU in een bepaalde tijdvenster." Gebruik eventueel de analytische hulpmiddelen zoals Excel of Power BI opvragen en analyseren van de verzamelde gegevens.

## <a name="example-retrieve-resource-consumption-metrics-for-a-pool-and-its-databases"></a>Voorbeeld: resource verbruik metrics voor een groep en de databases ophalen

In dit voorbeeld haalt de verbruik cijfers voor een bepaalde groep met elastische en alle bijbehorende databases. Verzamelde gegevens wordt opgemaakt en naar een geformatteerde CSV-bestand geschreven. Het bestand kan worden bekeken met Excel.

    $subscriptionId = '<Azure subscription id>'       # Azure subscription ID
    $resourceGroupName = '<resource group name>'             # Resource Group
    $serverName = <server name>                              # server name
    $poolName = <elastic pool name>                          # pool name
        
    # Login to Azure account and select the subscription.
    Login-AzureRmAccount
    Set-AzureRmContext -SubscriptionId $subscriptionId
    
    # Get resource usage metrics for an elastic pool for the specified time interval.
    $startTime = '4/27/2016 00:00:00'  # start time in UTC
    $endTime = '4/27/2016 01:00:00'    # end time in UTC
    
    # Construct the pool resource ID and retrive pool metrics at 5 minute granularity.
    $poolResourceId = '/subscriptions/' + $subscriptionId + '/resourceGroups/' + $resourceGroupName + '/providers/Microsoft.Sql/servers/' + $serverName + '/elasticPools/' + $poolName
    $poolMetrics = (Get-AzureRmMetric -ResourceId $poolResourceId -TimeGrain ([TimeSpan]::FromMinutes(5)) -StartTime $startTime -EndTime $endTime) 
    
    # Get the list of databases in this pool.
    $dbList = Get-AzureRmSqlElasticPoolDatabase -ResourceGroupName $resourceGroupName -ServerName $serverName -ElasticPoolName $poolName
    
    # Get resource usage metrics for a database in an elastic database for the specified time interval.
    $dbMetrics = @()
    foreach ($db in $dbList)
    {
        $dbResourceId = '/subscriptions/' + $subscriptionId + '/resourceGroups/' + $resourceGroupName + '/providers/Microsoft.Sql/servers/' + $serverName + '/databases/' + $db.DatabaseName
        $dbMetrics = $dbMetrics + (Get-AzureRmMetric -ResourceId $dbResourceId -TimeGrain ([TimeSpan]::FromMinutes(5)) -StartTime $startTime -EndTime $endTime)
    }
    
    #Optionally you can format the metrics and output as .csv file using the following script block.
    $command = {
    param($metricList, $outputFile)

    # Format metrics into a table.
    $table = @()
    foreach($metric in $metricList) { 
      foreach($metricValue in $metric.MetricValues) {
        $sx = New-Object PSObject -Property @{
            Timestamp = $metricValue.Timestamp.ToString()
            MetricName = $metric.Name; 
            Average = $metricValue.Average;
            ResourceID = $metric.ResourceId 
          }
          $table = $table += $sx
      }
    }
    
    # Output the metrics into a .csv file.
    write-output $table | Export-csv -Path $outputFile -Append -NoTypeInformation
    }
    
    # Format and output pool metrics
    Invoke-Command -ScriptBlock $command -ArgumentList $poolMetrics,c:\temp\poolmetrics.csv
    
    # Format and output database metrics
    Invoke-Command -ScriptBlock $command -ArgumentList $dbMetrics,c:\temp\dbmetrics.csv



## <a name="latency-of-elastic-pool-operations"></a>Latentie van de elastische pool

- Meestal de eDTUs min per database of het maximum aantal eDTUs per database wijzigen is voltooid in 5 minuten of minder.
- De eDTUs per groep wijzigen, is afhankelijk van de totale hoeveelheid ruimte die wordt gebruikt door alle databases in de groep. Wijzigingen gemiddeld 90 minuten of minder per 100 GB. Bijvoorbeeld, als de totale ruimte gebruikt door alle databases in de groep van toepassingen is 200 GB en vervolgens de verwachte vertraging voor het wijzigen van de eDTU van de groep per groep van toepassingen is 3 uur of minder.

## <a name="migrate-from-v11-to-v12-servers"></a>V11: migreren naar V12 servers

PowerShell-cmdlets zijn beschikbaar voor het starten, stoppen of een upgrade naar Azure SQL Database V12 uit V11: of een andere versie van de pre-V12 controleren.

- [Upgrade naar SQL Database V12 met PowerShell](sql-database-upgrade-server-powershell.md)

Voor documentatie over deze PowerShell-cmdlets, Zie:


- [Get-AzureRMSqlServerUpgrade](https://msdn.microsoft.com/library/azure/mt603582.aspx)
- [Start AzureRMSqlServerUpgrade](https://msdn.microsoft.com/library/azure/mt619403.aspx)
- [Stop AzureRMSqlServerUpgrade](https://msdn.microsoft.com/library/azure/mt603589.aspx)


De Stop-cmdlet betekent annuleren, niet onderbreken. Er is geen manier om een upgrade, dan start het programma opnieuw vanaf het begin te hervatten. De Stop-cmdlet schoont en versies van alle passende middelen.

## <a name="next-steps"></a>Volgende stappen

- [Elastische taken maken](sql-database-elastic-jobs-overview.md) Elastische taken kunnen u T-SQL-scripts uitvoeren op een onbeperkt aantal databases in de groep.
- [Scaling out met Azure SQL-Database](sql-database-elastic-scale-introduction.md)Zie: elastische Databasehulpprogramma's gebruiken om te schalen, verplaatsen van gegevens, te zoeken of te maken.
