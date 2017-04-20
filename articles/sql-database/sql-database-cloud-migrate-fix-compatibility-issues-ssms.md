<properties
   pageTitle="Compatibiliteitsproblemen voor SQL Server-database met behulp van SQL Server Management Studio voor de migratie naar een SQL-Database herstellen | Microsoft Azure"
   description="Microsoft Azure SQL-Database, databasemigratie, compatibiliteit met SQL Azure Migration Wizard"
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
   ms.date="08/24/2016"
   ms.author="carlrab"/>

# <a name="fix-sql-server-database-compatibility-issues-using-sql-server-management-studio-before-migration-to-sql-database"></a>Compatibiliteitsproblemen voor SQL Server-database met behulp van SQL Server Management Studio voor de migratie naar een SQL-Database oplossen

> [AZURE.SELECTOR]
- [SQL Azure migratiewizard](sql-database-cloud-migrate-fix-compatibility-issues.md) gebruiken
- [SSDT](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md) gebruiken
- [SSMS](sql-database-cloud-migrate-fix-compatibility-issues-ssms.md) gebruiken

Ervaren gebruikers kunnen compatibiliteitsproblemen voor SQL Server-database met behulp van SQL Server Management Studio voor de migratie naar Azure SQL-Database herstellen.


> [AZURE.IMPORTANT] Het wordt aanbevolen dat u altijd de meest recente versie van het Management Studio gebruiken om met updates voor Microsoft Azure en SQL-Database gesynchroniseerd blijven. [Bijwerken van SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).


## <a name="using-sql-server-management-studio"></a>Met behulp van SQL Server Management Studio

SQL Server Management Studio gebruiken voor het oplossen van compatibiliteitsproblemen met behulp van diverse Transact-SQL-opdrachten, zoals de **DATABASE wijzigen**. Deze methode is voornamelijk bedoeld voor gevorderde gebruikers die vertrouwd zijn Transact-SQL werkt aan een actieve database. Anders is het aanbevolen SSDT te gebruiken. 



## <a name="next-steps"></a>Volgende stappen

- [Nieuwste versie van SSDT](https://msdn.microsoft.com/library/mt204009.aspx)
- [Nieuwste versie van SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx)
- [SQL Server-databases compatibel met SQL-Database migreren](sql-database-cloud-migrate.md#migrate-a-compatible-sql-server-database-to-sql-database)

## <a name="additional-resources"></a>Aanvullende bronnen

- [SQL Database V12](sql-database-v12-whats-new.md)
- [Transact-SQL gedeeltelijk of niet-ondersteunde functies](sql-database-transact-sql-information.md)
- [Migreren van SQL Server databases SQL Server migratie-assistent](http://blogs.msdn.com/b/ssma/)
