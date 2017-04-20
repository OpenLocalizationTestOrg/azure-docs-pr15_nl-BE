<properties
   pageTitle="Een datawarehouse SQL maken in de portal Azure | Microsoft Azure"
   description="Informatie over het maken van een datawarehouse Azure SQL in Azure portal"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="barbkess"
   manager="jhubbard"
   editor=""
   tags="azure-sql-data-warehouse"/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="hero-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/25/2016"
   ms.author="barbkess;lodipalm;sonyama"/>

# <a name="create-an-azure-sql-data-warehouse"></a>Een datawarehouse Azure SQL maken

> [AZURE.SELECTOR]
- [Azure portal](sql-data-warehouse-get-started-provision.md)
- [TSQL](sql-data-warehouse-get-started-create-database-tsql.md)
- [PowerShell](sql-data-warehouse-get-started-provision-powershell.md)

In deze zelfstudie wordt de Azure portal voor het maken van een SQL-datawarehouse met AdventureWorksDW-voorbeelddatabase.


## <a name="prerequisites"></a>Vereisten

U moet om te beginnen:

- **Azure account**: Bezoek [Azure gratis proefperiode][] of [MSDN Azure verantwoording][] om een account te maken.
- **Azure SQL server**: Zie [maken van een logische server Azure SQL-Database met Azure portal][] voor meer informatie.

> [AZURE.NOTE] Maken van een SQL-datawarehouse kan leiden tot een nieuwe service te factureren.  Zie [SQL Data Warehouse prijzen][] voor meer informatie.

## <a name="create-a-sql-data-warehouse"></a>Maken van een SQL-datawarehouse

1. Log in om de [Azure portal](https://portal.azure.com).

2. Klik op **+ nieuwe** > **Data- +** > **SQL datawarehouse**.

    ![Maken](./media/sql-data-warehouse-get-started-provision/create-sample.gif)

3. In de blade **SQL Data Warehouse** Vul de informatie die nodig is, druk op 'Maken' te maken.

    ![Database maken](./media/sql-data-warehouse-get-started-provision/create-database.png)

    - **Server**: Wij adviseren u eerst uw server selecteren.  

    - **Naam**: de naam die wordt gebruikt om te verwijzen naar de SQL Data Warehouse.  Deze moet uniek zijn voor de server.
    
    - **Prestaties**: het beste eerst met 400 [DWUs][DWU]. U kunt de schuifregelaar naar links of naar rechts aanpassen de prestaties van het datawarehouse of schaal omhoog of omlaag na aanmaak.  Zie voor meer informatie over DWUs, onze documentatie op [schaal](./sql-data-warehouse-manage-compute-overview.md) of op onze [prijzen pagina][prijzen SQL Data Warehouse]. 

    - **Abonnement**: Selecteer het [abonnement] dat deze SQL Data Warehouse zal factureren aan.

    - **Resourcegroep**: [resourcegroepen] [ Resource group] zijn ontworpen om u te helpen bij het beheren van een verzameling bronnen Azure containers. Meer informatie over [resourcegroepen](../azure-resource-manager/resource-group-overview.md).

    - **Bron selecteren**: klik op **Selecteer bron** > **monster**. Azure vult automatisch de optie **selectie** met AdventureWorksDW.

> [AZURE.NOTE] De standaardsortering voor een datawarehouse SQL is SQL_Latin1_General_CP1_CI_AS. Als een andere sortering nodig is, kan [T-SQL][] de database maken met een andere sortering worden gebruikt.

4. Klik op **maken** om het maken van uw SQL-datawarehouse.

5. Wacht een paar minuten. Wanneer uw datawarehouse klaar is, moet u terug naar de [portal Azure](https://portal.azure.com). Uw SQL Data Warehouse kunt u vinden op uw dashboard, onder uw SQL-Databases, of in de resourcegroep die u hebt gebruikt om deze te maken. 

    ![Portal weergeven](./media/sql-data-warehouse-get-started-provision/database-portal-view.png)

[AZURE.INCLUDE [SQL Database create server](../../includes/sql-database-create-new-server-firewall-portal.md)] 

## <a name="next-steps"></a>Volgende stappen

U hebt een SQL-datawarehouse gemaakt, u bent klaar om te [verbinden](./sql-data-warehouse-connect-overview.md) en beginnen met het uitvoeren van query's.

Om gegevens te laden in SQL Data Warehouse, Zie het [Overzicht laden](./sql-data-warehouse-overview-load.md).

Als u probeert een bestaande database migreren naar SQL Data Warehouse, [migreren-overzicht](./sql-data-warehouse-overview-migrate.md) of het [Hulpprogramma voor migratie](./sql-data-warehouse-migrate-migration-utility.md)gebruiken.

Firewall-regels kunnen ook worden geconfigureerd met behulp van Transact-SQL. Voor meer informatie, Zie [sp_set_firewall_rule][] en [sp_set_database_firewall_rule][].

Het is ook een goed idee om de [beste praktijken][]te bekijken.

<!--Article references-->
[Een logische server Azure SQL-Database maken met de Azure portal]: ../sql-database/sql-database-get-started.md#create-an-azure-sql-database-logical-server
[Create an Azure SQL Database logical server with PowerShell]: ../sql-database/sql-database-get-started-powershell.md#database-setup-create-a-resource-group-server-and-firewall-rule
[resource groups]: ../resource-group-template-deploy-portal.md
[Aanbevolen procedures]: sql-data-warehouse-best-practices.md
[DWU]: sql-data-warehouse-overview-what-is.md#data-warehouse-units
[abonnement]: ../azure-glossary-cloud-terminology.md#subscription
[resource group]: ../azure-glossary-cloud-terminology.md#resource-group
[T-SQL]: ./sql-data-warehouse-get-started-create-database-tsql.md
 
<!--MSDN references-->
[sp_set_firewall_rule]: https://msdn.microsoft.com/library/dn270017.aspx
[sp_set_database_firewall_rule]: https://msdn.microsoft.com/library/dn270010.aspx

<!--Other Web references-->
[SQL Data Warehouse prijzen]: https://azure.microsoft.com/pricing/details/sql-data-warehouse/
[Azure gratis proefversie]: https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F
[MSDN Azure Credits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F

