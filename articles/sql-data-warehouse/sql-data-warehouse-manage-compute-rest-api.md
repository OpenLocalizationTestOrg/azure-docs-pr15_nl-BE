<properties
   pageTitle="Beheren van rekenkracht in Azure SQL gegevens magazijn (REST) | Microsoft Azure"
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
   ms.date="08/08/2016"
   ms.author="barbkess;sonyama"/>

# <a name="manage-compute-power-in-azure-sql-data-warehouse-rest"></a>Beheren van rekenkracht in Azure SQL gegevens magazijn (REST)

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

<a name="scale-performance-bk"></a>
<a name="scale-compute-bk"></a>

## <a name="scale-compute-power"></a>Schaal rekenkracht

[AZURE.INCLUDE [SQL Data Warehouse scale DWUs description](../../includes/sql-data-warehouse-scale-dwus-description.md)]

Gebruik de API van de REST [maken of de Database bijwerken][] als u wilt wijzigen in de DWUs. In het volgende voorbeeld wordt de service level doelstelling ingesteld op DW1000 voor de database MySQLDW dat wordt gehost op een server MijnServer. De server is in een groep Azure resource met de naam ResourceGroup1.

```
PUT https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/ResourceGroup1/providers/Microsoft.Sql/servers/MyServer/databases/MySQLDW?api-version=2014-04-01-preview HTTP/1.1
Content-Type: application/json; charset=UTF-8

{
    "properties": {
        "requestedServiceObjectiveName": DW1000
    }
}
```

<a name="pause-compute-bk"></a>

## <a name="pause-compute"></a>Compute onderbreken

[AZURE.INCLUDE [SQL Data Warehouse pause description](../../includes/sql-data-warehouse-pause-description.md)]

Onderbreken van een database, moet u de [Database onderbreken][] REST-API gebruiken. In het volgende voorbeeld wordt een database met de naam Database02 die worden gehost op een server met de naam Server01 onderbroken. De server is in een groep Azure resource met de naam ResourceGroup1.

```
POST https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/ResourceGroup1/providers/Microsoft.Sql/servers/Server01/databases/Database02/pause?api-version=2014-04-01-preview HTTP/1.1
```

<a name="resume-compute-bk"></a>

## <a name="resume-compute"></a>Cv berekenen

[AZURE.INCLUDE [SQL Data Warehouse resume description](../../includes/sql-data-warehouse-resume-description.md)]

Om te starten in een database, de REST [Cv Database][] API te gebruiken. In het volgende voorbeeld wordt een database met de naam Database02 die worden gehost op een server die de naam Server01 heeft gestart. De server is in een groep Azure resource met de naam ResourceGroup1. 

```
POST https://management.azure.com/subscriptions{subscription-id}/resourceGroups/ResourceGroup1/providers/Microsoft.Sql/servers/Server01/databases/Database02/resume?api-version=2014-04-01-preview HTTP/1.1
```

<a name="next-steps-bk"></a>

## <a name="next-steps"></a>Volgende stappen

Zie [beheer-overzicht][]voor andere beheertaken uitvoeren.

<!--Image references-->

<!--Article references-->
[Beheer-overzicht]: ./sql-data-warehouse-overview-manage.md
[Overzicht compute beheren]: ./sql-data-warehouse-manage-compute-overview.md

<!--MSDN references-->
[Pause-Database]: https://msdn.microsoft.com/library/azure/mt718817.aspx
[Cv-Database]: https://msdn.microsoft.com/library/azure/mt718820.aspx
[Maken of bijwerken van de Database]: https://msdn.microsoft.com/library/azure/mt163685.aspx

<!--Other Web references-->

[Azure portal]: http://portal.azure.com/
