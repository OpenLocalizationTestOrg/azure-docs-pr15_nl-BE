<properties
    pageTitle="Upgrade naar Azure SQL Database V12 met PowerShell | Microsoft Azure"
    description="Wordt uitgelegd hoe u een upgrade uitvoeren naar Azure SQL Database V12 met inbegrip van het bijwerken van Web- en databases en het upgraden van een V11: server migreren van de databases rechtstreeks in een elastische database toepassingen met PowerShell."
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/19/2016"
    ms.author="sstein"/>

# <a name="upgrade-to-azure-sql-database-v12-using-powershell"></a>Upgrade naar Azure SQL Database V12 met PowerShell


> [AZURE.SELECTOR]
- [Azure portal](sql-database-upgrade-server-portal.md)
- [PowerShell](sql-database-upgrade-server-powershell.md)


SQL Database V12 is de meest recente versie, dus is het raadzaam een upgrade naar SQL Database V12.
SQL Database V12 heeft veel [voordelen ten opzichte van de vorige versie](sql-database-v12-whats-new.md) inclusief:

- Verbeterde compatibiliteit met SQL Server.
- Premie voor verbeterde prestaties en nieuwe prestatieniveaus.
- [Elastische database van toepassingen](sql-database-elastic-pool.md).

Dit artikel bevat instructies voor het upgraden van bestaande V11: SQL-Database-servers en databases voor SQL-Database V12.

Tijdens de upgrade naar V12 upgrade Web- en -databases naar een nieuw niveau van service zodat instructies voor het upgraden van Web- en databases opgenomen zijn.

Bovendien kan migreren naar een [groep met elastische database](sql-database-elastic-pool.md) voordeliger zijn dan een upgrade naar individuele prestaties (prijzen lagen) voor enkele databases. Database management vereenvoudigen toepassingen ook omdat u alleen nodig voor het beheer van de prestatie-instellingen voor de groep in plaats van de prestaties van afzonderlijke databases afzonderlijk beheren. Als u databases op verschillende servers, kunt u overwegen ze in dezelfde server en profiteert van ze in een groep te plaatsen.

U kunt de stappen in dit artikel eenvoudig databases migreren van V11: servers rechtstreeks in de database elastische pools.

Houd er rekening mee dat uw databases online blijft en blijven werken tijdens de upgradebewerking. Op het moment van de werkelijke overgang naar het nieuwe tijdelijke verbindingen met de database weghalen prestatieniveau kunnen voor een zeer kleine duur die meestal ongeveer 90 seconden kan gebeuren maar maar liefst 5 minuten. Als uw toepassing [tijdelijke fout afhandeling voor afsluitingen van de verbinding worden](sql-database-connectivity-issues.md) is het voldoende bescherming tegen verbroken verbindingen aan het einde van de upgrade.

Een upgrade naar SQL Database V12 kan niet ongedaan worden gemaakt. Na een upgrade van kan niet de server worden teruggedraaid door V11:.

Na de upgrade naar V12, [service tier aanbevelingen](sql-database-service-tier-advisor.md) en [aanbevelingen van de groep met elastische](sql-database-elastic-pool-create-portal.md) onmiddellijk worden niet beschikbaar totdat de service heeft tijd om te evalueren uw werkbelasting op de nieuwe server. V11: server aanbeveling geschiedenis niet van toepassing op V12 servers dus niet behouden blijft.  

## <a name="prepare-to-upgrade"></a>Bereid de upgrade

- **Alle Web- en databases bijwerken**: gebruik van de portal of [PowerShell aan databases en de server](sql-database-upgrade-server-powershell.md).
- **Controleren en schorsen Geo-replicatie**: als de Azure SQL-database is geconfigureerd voor Geo-replicatie moet u de huidige configuratie en [Geo-replicatie stoppen](sql-database-geo-replication-portal.md#remove-secondary-database)documenteren. Configuratie van de database voor Geo-replicatie nadat de upgrade is voltooid.
- **Openen van deze poorten als u clients op een Azure VM**: als het clientprogramma verbinding maakt met SQL Database V12 terwijl de client wordt uitgevoerd op een Azure VM (virtual machine), moet u poortbereiken 11000 11999 en 14000-14999 op de VM openen. Zie [poorten voor V12 voor SQL-Database](sql-database-develop-direct-route-ports-adonet-v12.md)voor meer informatie.


## <a name="prerequisites"></a>Vereisten

Upgradeserver naar V12 met PowerShell, moet u de meest recente Azure PowerShell geïnstalleerd en actief zijn. Zie voor gedetailleerde informatie [het installeren en configureren van Azure PowerShell](../powershell-install-configure.md).


## <a name="configure-your-credentials-and-select-your-subscription"></a>Uw referenties te configureren en selecteer uw abonnement

PowerShell cmdlets uitvoeren tegen uw abonnement Azure moet u eerst toegang tot uw account Azure instellen. De volgende en u krijgt een scherm-in uw referenties moet invoeren. Gebruik hetzelfde e-mailadres en wachtwoord die u gebruikt voor aanmelding bij de Azure portal.

    Add-AzureRmAccount

Na succesvol inloggen ziet u op het scherm met de Id die u aangemeld met en de Azure abonnementen u toegang tot hebt bepaalde informatie.

Selecteer het abonnement dat u wilt werken met u moet uw abonnements-Id (**-SubscriptionId**) of het abonnement een naam (**-SubscriptionName**). U kunt kopiëren uit de vorige stap of als er meerdere abonnementen kunt u uitvoeren met de cmdlet **Get-AzureRmSubscription** en kopieert u de gewenste abonnementsgegevens van de resultaatset.

Voer de volgende cmdlet met uw abonnementsgegevens voor het instellen van je huidige abonnement:

    Set-AzureRmContext -SubscriptionId 4cac86b0-1e56-bbbb-aaaa-000000000000

De volgende opdrachten wordt uitgevoerd voor het abonnement u zojuist hebt geselecteerd.

## <a name="get-recommendations"></a>Aanbevelingen ophalen

Upgrade uitvoeren als u de aanbeveling voor de server de volgende cmdlet:

    $hint = Get-AzureRmSqlServerUpgradeHint -ResourceGroupName “resourcegroup1” -ServerName “server1”

Zie voor meer informatie, [een elastische database-toepassingen maken](sql-database-elastic-pool-create-portal.md) en [Azure SQL Database prijzen laag aanbevelingen](sql-database-service-tier-advisor.md).



## <a name="start-the-upgrade"></a>De upgrade starten

Start de upgrade van de server de volgende cmdlet uitvoert:

    Start-AzureRmSqlServerUpgrade -ResourceGroupName “resourcegroup1” -ServerName “server1” -ServerVersion 12.0 -DatabaseCollection $hint.Databases -ElasticPoolCollection $hint.ElasticPools  


Tijdens het uitvoeren van begint deze opdracht-upgradeproces. U kunt de uitvoer van de aanbeveling en de bewerkte aanbeveling om deze cmdlet te bieden.


## <a name="upgrade-a-server"></a>Een server bijwerken


    # Adding the account
    #
    Add-AzureRmAccount

    # Setting the variables
    #
    $SubscriptionName = 'YOUR_SUBSCRIPTION'
    $ResourceGroupName = 'YOUR_RESOURCE_GROUP'
    $ServerName = 'YOUR_SERVER'

    # Selecting the right subscription
    #
    Set-AzureRmContext -SubscriptionName $SubscriptionName

    # Getting the upgrade recommendations
    #
    $hint = Get-AzureRmSqlServerUpgradeHint -ResourceGroupName $ResourceGroupName -ServerName $ServerName

    # Starting the upgrade process
    #
    Start-AzureRmSqlServerUpgrade -ResourceGroupName $ResourceGroupName -ServerName $ServerName -ServerVersion 12.0 -DatabaseCollection $hint.Databases -ElasticPoolCollection $hint.ElasticPools  


## <a name="custom-upgrade-mapping"></a>Aangepaste toewijzing bijwerken

Als de aanbevelingen niet geschikt voor uw server en de business case zijn, kunt vervolgens u kiezen hoe de databases worden bijgewerkt en kunnen toewijzen aan één of elastische databases.

ElasticPoolCollection en DatabaseCollection parameters zijn optioneel:

    # Creating elastic pool mapping
    #
    $elasticPool = New-Object -TypeName Microsoft.Azure.Management.Sql.Models.UpgradeRecommendedElasticPoolProperties
    $elasticPool.DatabaseDtuMax = 100
    $elasticPool.DatabaseDtuMin = 0
    $elasticPool.Dtu = 800
    $elasticPool.Edition = "Standard"
    $elasticPool.DatabaseCollection = ("DB1", “DB2”, “DB3”, “DB4”)
    $elasticPool.Name = "elasticpool_1"
    $elasticPool.StorageMb = 800

    # Creating single database mapping for 2 databases. DBMain1 mapped to S0 and DBMain2 mapped to S2
    #
    $databaseMap1 = New-Object -TypeName Microsoft.Azure.Management.Sql.Models.UpgradeDatabaseProperties
    $databaseMap1.Name = "DBMain1"
    $databaseMap1.TargetEdition = "Standard"
    $databaseMap1.TargetServiceLevelObjective = "S0"

    $databaseMap2 = New-Object -TypeName Microsoft.Azure.Management.Sql.Models.UpgradeDatabaseProperties
    $databaseMap2.Name = "DBMain2"
    $databaseMap2.TargetEdition = "Standard"
    $databaseMap2.TargetServiceLevelObjective = "S2"

    # Starting the upgrade
    #
    Start-AzureRmSqlServerUpgrade –ResourceGroupName resourcegroup1 –ServerName server1 -ServerVersion 12.0 -DatabaseCollection @($databaseMap1, $databaseMap2) -ElasticPoolCollection @($elasticPool)



## <a name="monitor-databases-after-upgrading-to-sql-database-v12"></a>Databases controleren na een upgrade naar SQL Database V12


Na de upgrade wordt het aanbevolen voor het controleren van de database actief zodat toepassingen worden uitgevoerd op de gewenste prestaties en gebruik optimaliseren indien nodig.

In aanvulling op het toezicht op afzonderlijke databases kunt u controleren elastische database van toepassingen [met behulp van de portal](sql-database-elastic-pool-manage-portal.md) of met [PowerShell](sql-database-elastic-pool-manage-powershell.md)


**Verbruiksgegevens bron:** Resourcegegevens verbruik is voor Basic-, Standard- en Premium-databases beschikbaar via de [sys.dm_ db_ resource_stats](http://msdn.microsoft.com/library/azure/dn800981.aspx) DMV in de database. Deze DMV vindt u in de buurt van real-time verbruik resourcegegevens op 15 tweede granulatie voor de vorige bewerking uur. De DTU percentage van het verbruik voor een interval wordt berekend als het verbruik van het maximumpercentage van de CPU, IO en logboek dimensies. Hier wordt een query voor het berekenen van het gemiddelde DTU percentage verbruik over het afgelopen uur:

    SELECT end_time
         , (SELECT Max(v)
             FROM (VALUES (avg_cpu_percent)
                         , (avg_data_io_percent)
                         , (avg_log_write_percent)
           ) AS value(v)) AS [avg_DTU_percent]
    FROM sys.dm_db_resource_stats
    ORDER BY end_time DESC;

Als u meer informatie over prestatiebewaking:

- [Azure SQL Database prestaties richtlijnen voor enkele databases](http://msdn.microsoft.com/library/azure/dn369873.aspx).
- [Overwegingen voor prijs- prestatieverhouding voor een elastische database-toepassingen](sql-database-elastic-pool-guidance.md).
- [Azure SQL-Database met behulp van beheer van dynamische weergaven controleren](sql-database-monitoring-with-dmvs.md)



**Waarschuwingen:** "Waarschuwingen instellen ' in de Azure portal om u te waarschuwen wanneer de DTU het verbruik voor een upgrade van een database bepaald hoog niveau nadert. Database waarschuwingen kunnen worden ingesteld in Azure portal voor verschillende prestatiegegevens zoals DTU, CPU, IO en logboek. Blader naar de database en selecteer **waarschuwingsregels** in het blad van de **Instellingen** .

Bijvoorbeeld: u kunt instellen een waarschuwingsbericht per e-mail op "DTU Percentage" Als de gemiddelde waarde van DTU percentage groter is dan 75% gedurende de laatste 5 minuten. Raadpleeg voor het [ontvangen van meldingen](../monitoring-and-diagnostics/insights-receive-alert-notifications.md) voor meer informatie over het configureren van meldingen.



## <a name="next-steps"></a>Volgende stappen

- [Een elastische database-toepassingen maken](sql-database-elastic-pool-create-portal.md) en sommige of alle van de databases in de groep toevoegen.
- [Het serviceniveau voor laag en de prestaties van uw database te wijzigen](sql-database-scale-up.md).



## <a name="related-information"></a>Verwante informatie

- [Get-AzureRmSqlServerUpgrade](https://msdn.microsoft.com/library/azure/mt603582.aspx)
- [Start AzureRmSqlServerUpgrade](https://msdn.microsoft.com/library/azure/mt619403.aspx)
- [Stop AzureRmSqlServerUpgrade](https://msdn.microsoft.com/library/azure/mt603589.aspx)
