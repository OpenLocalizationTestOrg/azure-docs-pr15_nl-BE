<properties
   pageTitle="Beheren van rekenkracht in Azure SQL Data Warehouse (PowerShell) | Microsoft Azure"
   description="Taken voor het beheren van PowerShell rekenkracht. Schaal berekenen bronnen door DWUs aan te passen. Of onderbreken en hervatten middelen om op te slaan van de kosten berekenen."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="barbkess"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="06/13/2016"
   ms.author="barbkess;sonyama"/>

# <a name="manage-compute-power-in-azure-sql-data-warehouse-powershell"></a>Rekenkracht in Azure SQL Data Warehouse (PowerShell) beheren

> [AZURE.SELECTOR]
- [Overzicht](sql-data-warehouse-manage-compute-overview.md)
- [Portal](sql-data-warehouse-manage-compute-portal.md)
- [PowerShell](sql-data-warehouse-manage-compute-powershell.md)
- [REST](sql-data-warehouse-manage-compute-rest-api.md)
- [TSQL](sql-data-warehouse-manage-compute-tsql.md)


Prestaties door schalen schaal berekenen bronnen en het geheugen om te voldoen aan de veranderende behoeften van uw werkbelasting. Kosten besparen door de schaal terug resources tijdens niet-piekuren of onderbreken compute helemaal. 

Deze verzameling taken Azure portal gebruikt:

- Schaal berekenen
- Compute onderbreken
- Cv berekenen

Meer informatie hierover, Zie [beheren overzicht berekenen][].


## <a name="before-you-begin"></a>Voordat u begint

### <a name="install-the-latest-version-of-azure-powershell"></a>Installeer de nieuwste versie van Azure PowerShell

> [AZURE.NOTE]  Azure PowerShell met SQL Data Warehouse, hebt u nodig Azure PowerShell versie 1.0.3 of hoger.  Om te controleren of uw huidige versie, voert u de opdracht **Get Module - ListAvailable-naam Azure**. U kunt de meest recente versie van [Microsoft Web Platform Installer][]installeren.  Zie voor meer informatie [het installeren en configureren van Azure PowerShell][].

### <a name="get-started-with-azure-powershell-cmdlets"></a>Aan de slag met Azure PowerShell-cmdlets

Aan de slag:

1. Azure PowerShell openen. 
2. Bij de PowerShell-prompt uitvoeren van deze opdrachten te melden aan de bronnenbeheerder van Azure en selecteer uw abonnement.

    ```PowerShell
    Login-AzureRmAccount
    Get-AzureRmSubscription
    Select-AzureRmSubscription -SubscriptionName "MySubscription"
    ```

<a name="scale-performance-bk"></a>
<a name="scale-compute-bk"></a>

## <a name="scale-compute-power"></a>Schaal rekenkracht

[AZURE.INCLUDE [SQL Data Warehouse scale DWUs description](../../includes/sql-data-warehouse-scale-dwus-description.md)]

Met de PowerShell-cmdlet [Set-AzureRmSqlDatabase][] u de DWUs wijzigen. In het volgende voorbeeld wordt de service level doelstelling ingesteld op DW1000 voor de database MySQLDW dat wordt gehost op een server MijnServer. 

```Powershell
Set-AzureRmSqlDatabase -DatabaseName "MySQLDW" -ServerName "MyServer" -RequestedServiceObjectiveName "DW1000"
```

<a name="pause-compute-bk"></a>

## <a name="pause-compute"></a>Compute onderbreken

[AZURE.INCLUDE [SQL Data Warehouse pause description](../../includes/sql-data-warehouse-pause-description.md)]

Onderbreken van een database, gebruikt u de [Stand-by-AzureRmSqlDatabase][] -cmdlet. In het volgende voorbeeld wordt een database met de naam Database02 die worden gehost op een server met de naam Server01 onderbroken. De server is in een groep Azure resource met de naam ResourceGroup1. 

> [AZURE.NOTE] Overigens als uw server is foo.database.windows.net, "foo" als - servernaam in de PowerShell-cmdlets gebruiken.

```Powershell
Suspend-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" `
–ServerName "Server01" –DatabaseName "Database02"
```
Een variatie op, in dit volgende voorbeeld haalt de database naar het object $database. Deze pijpen [Stand-by-AzureRmSqlDatabase][]van het object vervolgens. De resultaten worden opgeslagen in het object resultDatabase. De laatste opdracht toont de resultaten.

```Powershell
$database = Get-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" `
–ServerName "Server01" –DatabaseName "Database02"
$resultDatabase = $database | Suspend-AzureRmSqlDatabase
$resultDatabase
```

<a name="resume-compute-bk"></a>

## <a name="resume-compute"></a>Cv berekenen

[AZURE.INCLUDE [SQL Data Warehouse resume description](../../includes/sql-data-warehouse-resume-description.md)]

Om te starten in een database, gebruik de cmdlet [Cv-AzureRmSqlDatabase][] . In het volgende voorbeeld wordt een database met de naam Database02 die worden gehost op een server die de naam Server01 heeft gestart. De server is in een groep Azure resource met de naam ResourceGroup1. 

```Powershell
Resume-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" `
–ServerName "Server01" -DatabaseName "Database02"
```

Een variatie op, in dit volgende voorbeeld haalt de database naar het object $database. Vervolgens pijpen van het [Cv-AzureRmSqlDatabase][] -object en worden de resultaten opgeslagen in $resultDatabase. De laatste opdracht toont de resultaten.

```Powershell
$database = Get-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" `
–ServerName "Server01" –DatabaseName "Database02"
$resultDatabase = $database | Resume-AzureRmSqlDatabase
$resultDatabase
```

<a name="next-steps-bk"></a>

## <a name="next-steps"></a>Volgende stappen

Zie [beheer-overzicht][]voor andere beheertaken uitvoeren.

<!--Image references-->

<!--Article references-->
[Service capacity limits]: ./sql-data-warehouse-service-capacity-limits.md
[Beheer-overzicht]: ./sql-data-warehouse-overview-manage.md
[Het installeren en configureren van Azure PowerShell]: ./powershell-install-configure.md
[Overzicht compute beheren]: ./sql-data-warehouse-manage-compute-overview.md

<!--MSDN references-->
[Cv-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619347.aspx
[Stand-by AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619337.aspx
[Set AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619433.aspx

<!--Other Web references-->
[Microsoft Web Platform Installer]: https://aka.ms/webpi-azps
[Azure portal]: http://portal.azure.com/
