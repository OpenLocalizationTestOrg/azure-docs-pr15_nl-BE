<properties
   pageTitle="Een SQL Server-database exporteren naar een Bacpac-bestand met behulp van SqlPackage | Microsoft Azure"
   description="Microsoft Azure SQL-Database, databasemigratie, database exporteren, sqlpackage-Bacpac-bestand exporteren"
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

# <a name="export-a-sql-server-database-to-a-bacpac-file-using-sqlpackage"></a>Een SQL Server-database exporteren naar een Bacpac-bestand met behulp van SqlPackage

> [AZURE.SELECTOR]
- [SSMS](sql-database-cloud-migrate-compatible-export-bacpac-ssms.md)
- [SqlPackage](sql-database-cloud-migrate-compatible-export-bacpac-sqlpackage.md)

In dit artikel ziet u hoe u een SQL Server-database exporteren naar een [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) -bestand met het opdrachtregelprogramma [SqlPackage](https://msdn.microsoft.com/library/hh550080.aspx) . Dit hulpprogramma wordt geleverd met de nieuwste versies van [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) en [SQL Server Data Tools voor Visual Studio](https://msdn.microsoft.com/library/mt204009.aspx), of u de nieuwste versie van [SqlPackage](https://www.microsoft.com/en-us/download/details.aspx?id=53876) rechtstreeks vanaf het Microsoft Downloadcentrum kunt downloaden.

1. Een opdrachtpromptvenster openen en wijzigen van een map met het opdrachtregelprogramma sqlpackage.exe - Dit hulpprogramma wordt geleverd met zowel Visual Studio en SQL Server. Zoeken op uw computer gebruiken om het pad te vinden in uw omgeving.
2. De volgende sqlpackage.exe-opdracht met de volgende argumenten voor uw omgeving worden uitgevoerd:

    ' sqlpackage.exe /Action:Export /ssn: /sdn < servernaam >: /tf < databasenaam >: < target_file >

  	| Argument  | Beschrijving  |
  	|---|---|
  	| < servernaam >  | naam van de gegevensbron  |
  	| < databasenaam >  | naam van gegevensbron database  |
  	| < target_file >  | de naam en locatie voor Bacpac-bestand  |

    ![Een gegevens-tier-toepassing exporteren in het menu taken](./media/sql-database-cloud-migrate/TestForCompatibilityUsingSQLPackage01b.png)

## <a name="next-steps"></a>Volgende stappen

- [Nieuwste versie van SSDT](https://msdn.microsoft.com/library/mt204009.aspx)
- [Nieuwste versie van SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx)
- [Een BACPAC Azure SSMS met SQL-database importeren](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md)
- [Importeren van een BACPAC in Azure SQL-Database-SqlPackage](sql-database-cloud-migrate-compatible-import-bacpac-sqlpackage.md)
- [Importeren van een BACPAC Azure SQL Database Azure Portal](sql-database-import.md)
- [Importeren van een BACPAC in Azure SQL Database PowerShell](sql-database-import-powershell.md)

## <a name="additional-resources"></a>Aanvullende bronnen

- [SQL Database V12](sql-database-v12-whats-new.md)
- [Transact-SQL gedeeltelijk of niet-ondersteunde functies](sql-database-transact-sql-information.md)
- [Migreren van SQL Server databases SQL Server migratie-assistent](http://blogs.msdn.com/b/ssma/)
