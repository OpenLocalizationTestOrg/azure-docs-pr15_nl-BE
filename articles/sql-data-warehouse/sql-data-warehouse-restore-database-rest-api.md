<properties
   pageTitle="Herstellen van een datawarehouse Azure SQL (REST API) | Microsoft Azure"
   description="Taken in de REST API voor het terugzetten van een Azure SQL Data Warehouse."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="Lakshmi1812"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/21/2016"
   ms.author="lakshmir;barbkess;sonyama"/>

# <a name="restore-an-azure-sql-data-warehouse-rest-api"></a>Herstellen van een datawarehouse Azure SQL (REST API)

> [AZURE.SELECTOR]
- [Overzicht][]
- [Portal][]
- [PowerShell][]
- [REST][]

In dit artikel leert u het herstellen van een Azure SQL Data Warehouse met de REST API.

## <a name="before-you-begin"></a>Voordat u begint

**Controleer of de capaciteit van de DTU.** Elke SQL Data Warehouse wordt gehost door een SQL-server (bijv. myserver.database.windows.net) met een standaardquotum DTU.  Voordat u een SQL-datawarehouse terugzetten kunt, Controleer de SQL server heeft weinig DTU resterende voor de database wordt hersteld. Meer informatie over het berekenen van DTU die nodig zijn of dat er meer DTU, Zie [een quotum DTU wijziging aanvragen][].

## <a name="restore-an-active-or-paused-database"></a>Een actief of onderbroken database terugzetten

Een database terugzetten:

1. De lijst met herstelpunten database met behulp van de bewerking Get Database herstelpunten ophalen.
2. Het terugzetten met behulp van de [database maken verzoek][] terugzetbewerking begint.
3. De status van het terugzetten met behulp van de [status van de bewerking Database][] bijhouden.

>[AZURE.NOTE] Nadat het terugzetten is voltooid, kunt u de herstelde database met de volgende [configuratie van de database na herstel][].

## <a name="restore-a-deleted-database"></a>Een verwijderde database terugzetten

Een verwijderde database terugzetten:

1.  De lijst alle verwijderde databases terug te zetten met behulp van de [lijst te herstellen verloren gegane databases][] .
2.  De details voor de verwijderde database die u herstellen wilt met behulp van de bewerking [ophalen te herstellen verloren gegane database][] ophalen.
3.  Het terugzetten met behulp van de [database maken verzoek][] terugzetbewerking begint.
4.  De status van het terugzetten met behulp van de [status van de bewerking Database][] bijhouden.

>[AZURE.NOTE] Zie de database configureren nadat het terugzetten is voltooid, [de database na herstel configureren][]. 


## <a name="next-steps"></a>Volgende stappen
Lees meer informatie over de functies van de business continuity van edities Azure SQL-Database, de [business continuity overzicht van Azure SQL-Database][].

<!--Image references-->

<!--Article references-->
[Azure SQL-Database business continuity-overzicht]: ./sql-database-business-continuity.md
[Een wijziging van de quota DTU aanvragen]: ./sql-data-warehouse-get-started-create-support-ticket.md#request-quota-change
[De database configureren na herstel]: ./sql-database-disaster-recovery.md#configure-your-database-after-recovery
[How to install and configure Azure PowerShell]: ./powershell-install-configure.md
[Overzicht]: ./sql-data-warehouse-restore-database-overview.md
[Portal]: ./sql-data-warehouse-restore-database-portal.md
[PowerShell]: ./sql-data-warehouse-restore-database-powershell.md
[REST]: ./sql-data-warehouse-restore-database-rest-api.md

<!--MSDN references-->
[Database terugzetten aanvraag maken]: https://msdn.microsoft.com/library/azure/dn509571.aspx
[Status van de database-bewerking]: https://msdn.microsoft.com/library/azure/dn720371.aspx
[Te herstellen van verloren gegane database ophalen]: https://msdn.microsoft.com/library/azure/dn509574.aspx
[Lijst van te herstellen verloren databases]: https://msdn.microsoft.com/library/azure/dn509562.aspx
[Restore-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt693390.aspx

<!--Other Web references-->
[Azure Portal]: https://portal.azure.com/
[Microsoft Web Platform Installer]: https://aka.ms/webpi-azps
