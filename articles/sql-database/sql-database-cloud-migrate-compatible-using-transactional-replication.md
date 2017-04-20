<properties
   pageTitle="Migreren naar een SQL-Database met behulp van transactionele replicatie | Microsoft Azure"
   description="Microsoft Azure SQL-Database, databasemigratie, database, importeert transactionele replicatie"
   services="sql-database"
   documentationCenter=""
   authors="CarlRabeler"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="sqldb-migrate"
   ms.date="08/23/2016"
   ms.author="carlrab"/>

# <a name="migrate-sql-server-database-to-azure-sql-database-using-transactional-replication"></a>SQL Server-database migreren naar Azure SQL-Database met behulp van transactionele replicatie

> [AZURE.SELECTOR]
- [SSMS migratiewizard](sql-database-cloud-migrate-compatible-using-ssms-migration-wizard.md)
- [Exporteren naar bestand Bacpac-](sql-database-cloud-migrate-compatible-export-bacpac-ssms.md)
- [Importeren uit bestand Bacpac-](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md)
- [Transactionele replicatie](sql-database-cloud-migrate-compatible-using-transactional-replication.md)

In dit artikel leert u compatibele SQL Server-database migreren naar Azure SQL-Database met een minimale uitval transactionele replicatie van SQL Server.

## <a name="understanding-the-transactional-replication-architecture"></a>Inzicht in de architectuur voor transactionele replicatie

Wanneer u zich niet veroorloven de SQL Server-database verwijderen uit productie tijdens de migratie, kunt u de transactionele replicatie SQL Server als migratieoplossing voor de. Voor het gebruik van deze oplossing, kunt u uw Azure SQL-Database configureren als abonnee van de SQL Server-exemplaar op de ruimten die u wilt migreren. De op-ruimten distributeur voor transactionele replicatie gegevens uit de database op de gebouwen (uitgever synchroniseert) worden gesynchroniseerd terwijl u nieuwe transacties blijven optreden. 

U kunt ook een transactionele replicatie gebruiken voor het migreren van een subset van uw database op gebouwen. De publicatie die u naar Azure SQL-Database repliceren kan worden beperkt tot een subset van de tabellen in de database wordt gerepliceerd. Voor elke tabel wordt gerepliceerd, kunt u de gegevens om een subset van de rijen en/of een subset van de kolommen te beperken.

Met transactionele replicatie weergegeven alle wijzigingen in de gegevens of het schema in uw Azure SQL-Database. Nadat de synchronisatie voltooid is en u bent klaar om te migreren, wijzigen in de verbindingsreeks van uw toepassingen, wijs de Azure SQL-Database. Wanneer transactionele replicatie links op uw database op ruimten en alle toepassingen DB Azure wijs wijzigingen wegloopt, kunt u de transactionele replicatie verwijderen. De Azure SQL-Database is nu uw productiesysteem.

 ![SeedCloudTR-diagram](./media/sql-database-cloud-migrate/SeedCloudTR.png)

## <a name="transactional-replication-requirements"></a>Vereisten voor transactionele replicatie

Transactionele replicatie is een ingebouwde en door de integratie met SQL Server-technologie sinds SQL Server 6.5. Het is een volwassen en beproefde technologie dat de meeste DBAs weet waarmee ze ervaring hebt. Met de [SQL Server-2016](https://www.microsoft.com/en-us/cloud-platform/sql-server)is nu mogelijk uw Azure SQL-Database configureren als een [abonnement voor transactionele replicatie](https://msdn.microsoft.com/library/mt589530.aspx) aan de publicatie op gebouwen. De ervaring die u krijgt het instellen van Management Studio is hetzelfde als wanneer u een abonnee voor transactionele replicatie op een server op locatie instellen. Ondersteuning voor dit scenario is wanneer de uitgever en de distributeur ten minste één van de volgende versies van SQL Server worden ondersteund:

 - SQL Server 2016 en hoger 
 - CU3 voor SQL Server 2014 SP1 en hoger
 - SQL Server 2014 RTM CU10 en hoger
 - CU8 van SQL Server 2012 SP2 en hoger
 - SQL Server 2012 SP3 en hoger


> [AZURE.IMPORTANT] Gebruik de nieuwste versie van SQL Server Management Studio te blijven gesynchroniseerd met updates voor Microsoft Azure en SQL-Database. Oudere versies van SQL Server Management Studio kunnen SQL-Database instellen als abonnee. [Bijwerken van SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).


## <a name="next-steps"></a>Volgende stappen

- [Nieuwste versie van SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx)
- [Nieuwste versie van SSDT](https://msdn.microsoft.com/library/mt204009.aspx)
- [SQL Server 2016](https://www.microsoft.com/en-us/cloud-platform/sql-server)

## <a name="additional-resources"></a>Aanvullende bronnen

- [Transactionele replicatie](https://msdn.microsoft.com/library/mt589530.aspx)
- [SQL Database V12](sql-database-v12-whats-new.md)
- [Transact-SQL gedeeltelijk of niet-ondersteunde functies](sql-database-transact-sql-information.md)
- [Migreren van SQL Server databases SQL Server migratie-assistent](http://blogs.msdn.com/b/ssma/)
