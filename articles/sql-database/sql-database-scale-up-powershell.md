<properties 
    pageTitle="Het serviceniveau voor laag en de prestaties van een Azure SQL-database met PowerShell wijzigen | Microsoft Azure" 
    description="Wijzig de servicelaag en prestatieniveau van Azure SQL-database ziet u hoe u de SQL-database geschaald omhoog of omlaag met PowerShell. Het wijzigen van de prijzen laag van Azure SQL-database met PowerShell." 
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="10/12/2016"
    ms.author="sstein"
    ms.workload="data-management"
    ms.topic="article"
    ms.tgt_pltfrm="NA"/>


# <a name="change-the-service-tier-and-performance-level-pricing-tier-of-a-sql-database-with-powershell"></a>De laag en prestaties serviceniveau (prijzen laag) van een SQL-database met PowerShell wijzigen


> [AZURE.SELECTOR]
- [Azure portal](sql-database-scale-up.md)
- [**PowerShell**](sql-database-scale-up-powershell.md)


Service niveaus en prestatieniveaus beschrijven de functies en bronnen die beschikbaar zijn voor uw SQL-database en kunnen worden bijgewerkt als de behoeften van uw toepassing wijzigen. Zie voor meer informatie, [Service Tiers](sql-database-service-tiers.md).

Veranderen van het niveau van de service en/of prestatieniveau van een database een replica van de oorspronkelijke database maakt op het nieuwe prestatieniveau van en vervolgens overgeschakeld verbindingen in de replica. Er zijn geen gegevens verloren is gegaan tijdens dit proces, maar tijdens het korte moment wanneer we naar de replica overschakelen, verbindingen met de database zijn uitgeschakeld, zodat sommige transacties tijdens de vlucht kunnen worden teruggedraaid. Dit venster kan variëren, maar is gemiddeld binnen 4 seconden en is in meer dan 99% van de gevallen minder dan 30 seconden. Heel af en toe vooral als er grote aantallen transacties tijdens de vlucht op het moment dat verbindingen zijn uitgeschakeld, dit venster kan niet langer.  

De duur van het hele proces van schaal-up is afhankelijk van de grootte en service niveaus van de database vóór en na de wijziging. Bijvoorbeeld, voltooid een 250 GB-database wordt gewijzigd naar, vanuit of binnen een laag standaard service binnen 6 uur. Voor een database van dezelfde grootte prestatieniveaus binnen de tier Premium service wordt gewijzigd, moet er binnen drie uur uitgevoerd.


- Als u wilt wijzigen in een database, moet de database kleiner zijn dan de maximale toegestane grootte van de doel-tier-service. 
- Bij het upgraden van een database met [Geo-replicatie](sql-database-geo-replication-portal.md) is ingeschakeld, moet u eerst de secundaire databases upgraden naar de gewenste prestaties laag voordat u een upgrade van de primaire database.
- Bij het downgraden van een laag Premium service, moet u eerst alle relaties van Geo-replicatie beëindigen. U kunt de stappen beschreven in het onderwerp [herstellen na een storing](sql-database-disaster-recovery.md) niet meer het replicatieproces tussen de primaire en de secundaire actieve databases.
- Het serviceaanbod terugzetten zijn verschillend voor de verschillende niveaus van de service. Als u bent u downgraden kunnen verliezen de mogelijkheid om te zetten naar een punt in de tijd, of hebben een lagere bewaartermijn voor back-up. Zie [Azure SQL Database back-up en terugzetten](sql-database-business-continuity.md)voor meer informatie.
- De nieuwe eigenschappen van de database worden pas toegepast als de wijzigingen voltooid zijn.



**Voor het voltooien van dit artikel hebt u het volgende nodig:**

- Een abonnement op Azure. Als u een abonnement op Azure gewoon **Gratis ACCOUNT** boven aan deze pagina op en teruggaan naar het voltooien van dit artikel.
- Azure SQL-database. Als u niet een SQL-database, maken een volgens de stappen in dit artikel: [de eerste Azure SQL-Database maken](sql-database-get-started.md).
- Azure PowerShell.


[AZURE.INCLUDE [Start your PowerShell session](../../includes/sql-database-powershell.md)]



## <a name="change-the-service-tier-and-performance-level-of-your-sql-database"></a>Het serviceniveau voor laag en de prestaties van de SQL-database wijzigen

De [Set AzureRmSqlDatabase] worden uitgevoerd (https://msdn.microsoft.com/library/azure/mt619433(v=azure.300\).aspx)-cmdlet en stel de **-RequestedServiceObjectiveName** aan de prestaties van de gewenste prijzen laag; hoog bijvoorbeeld *S0*, *S1*, *S2*, *S3*, *P1*, *P2*,...

```
$ResourceGroupName = "resourceGroupName"
    
$ServerName = "serverName"
$DatabaseName = "databaseName"

$NewEdition = "Standard"
$NewPricingTier = "S2"

Set-AzureRmSqlDatabase -DatabaseName $DatabaseName -ServerName $ServerName -ResourceGroupName $ResourceGroupName -Edition $NewEdition -RequestedServiceObjectiveName $NewPricingTier
```

  

   


## <a name="sample-powershell-script-to-change-the-service-tier-and-performance-level-of-your-sql-database"></a>Voorbeeld van PowerShell script het serviceniveau voor laag en de prestaties van de SQL-database wijzigen

Vervangen ```{variables}``` met de waarden (niet de accolades bevatten).

```
$SubscriptionId = "{4cac86b0-1e56-bbbb-aaaa-000000000000}"
    
$ResourceGroupName = "{resourceGroup}"
$Location = "{AzureRegion}"
    
$ServerName = "{server}"
$DatabaseName = "{database}"
    
$NewEdition = "{Standard}"
$NewPricingTier = "{S2}"
    
Add-AzureRmAccount
Set-AzureRmContext -SubscriptionId $SubscriptionId
    
Set-AzureRmSqlDatabase -DatabaseName $DatabaseName -ServerName $ServerName -ResourceGroupName $ResourceGroupName -Edition $NewEdition -RequestedServiceObjectiveName $NewPricingTier
```
        


## <a name="next-steps"></a>Volgende stappen

- [Uit- en schalen](sql-database-elastic-scale-get-started.md)
- [Verbinding maken en een SQL-database met SSMS](sql-database-connect-query-ssms.md)
- [Azure SQL-database exporteren](sql-database-export-powershell.md)

## <a name="additional-resources"></a>Aanvullende bronnen

- [Business Continuity-overzicht](sql-database-business-continuity.md)
- [Documentatie voor SQL-Database](http://azure.microsoft.com/documentation/services/sql-database/)
- [Cmdlets azure SQL Database] (http://msdn.microsoft.com/library/azure/mt574084https :/ / msdn.microsoft.com/bibliotheek/azure/mt619433(v=azure.300\).aspx.aspx)