<properties
   pageTitle="Bepalen van de SQL-Database compatibiliteit met SqlPackage.exe | Microsoft Azure"
   description="Microsoft Azure SQL-Database, databasemigratie, compatibiliteit met SQL-Database SqlPackage"
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

# <a name="determine-sql-database-compatibility-using-sqlpackageexe"></a>SQL-Database compatibiliteit met SqlPackage.exe bepalen

> [AZURE.SELECTOR]
- [SSDT](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md)
- [SqlPackage](sql-database-cloud-migrate-determine-compatibility-sqlpackage.md)
- [SSMS](sql-database-cloud-migrate-determine-compatibility-ssms.md)
- [Upgrade Advisor](http://www.microsoft.com/download/details.aspx?id=48119)
- [SAMW](sql-database-cloud-migrate-fix-compatibility-issues.md)

In dit artikel leert u om te bepalen of een SQL Server-database te migreren naar een SQL-Database met behulp van het opdrachtregelprogramma [SqlPackage](https://msdn.microsoft.com/library/hh550080.aspx) compatibel is.

## <a name="using-sqlpackageexe"></a>Met behulp van SqlPackage.exe

1. Een opdrachtpromptvenster openen en wijzigen van een map met de nieuwste versie van sqlpackage.exe. Dit hulpprogramma wordt geleverd met de nieuwste versies van [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) en [SQL Server Data Tools voor Visual Studio](https://msdn.microsoft.com/library/mt204009.aspx), of u de nieuwste versie van [SqlPackage](https://www.microsoft.com/en-us/download/details.aspx?id=53876) rechtstreeks vanaf het Microsoft Downloadcentrum kunt downloaden.
2. De volgende SqlPackage-opdracht met de volgende argumenten voor uw omgeving worden uitgevoerd:

    ' sqlpackage.exe /Action:Export /ssn: /sdn < servernaam >: /tf < databasenaam >: < target_file > /p:TableData < schema_name.table_name >>< = uitvoerbestand > 2 > & 1

  	| Argument  | Beschrijving  |
  	|---|---|
  	| < servernaam >  | naam van de gegevensbron  |
  	| < databasenaam >  | naam van gegevensbron database  |
  	| < target_file >  | de naam en locatie voor Bacpac-bestand  |
  	| < schema_name.table_name >  | de tabellen waarvoor de gegevens uitvoer naar het doelbestand zijn.  |
  	| < uitvoerbestand >  | de bestandsnaam en locatie voor het uitvoerbestand met fouten, indien van toepassing  |

    De reden voor het argument /p:TableName is dat we alleen wilt testen op databasecompatibiliteit voor export naar Azure SQL DB V12 in plaats van de gegevens uit alle tabellen exporteren. Het argument van de export van sqlpackage.exe ondersteunt helaas geen nul tabellen uitpakken. U moet ten minste één tabel, zoals een enkele kleine tabel opgeven. De < uitvoerbestand > bevat de lijst van eventuele fouten. De "> 2 > & 1" string buizen zowel de standaarduitvoer en standaardfouten die voortvloeien uit de uitvoering van de opdracht aan het opgegeven uitvoerbestand.

    ![Een gegevens-tier-toepassing exporteren in het menu taken](./media/sql-database-cloud-migrate/TestForCompatibilityUsingSQLPackage01.png)

3. Het uitvoerbestand te openen en bekijk de compatibiliteitsproblemen, indien van toepassing. 

    ![Een gegevens-tier-toepassing exporteren in het menu taken](./media/sql-database-cloud-migrate/TestForCompatibilityUsingSQLPackage02.png)

## <a name="next-steps"></a>Volgende stappen

- [Nieuwste versie van SSDT](https://msdn.microsoft.com/library/mt204009.aspx)
[nieuwste versie van SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx)
- [De databasemigratie compatibiliteitsproblemen oplossen](sql-database-cloud-migrate.md#fix-database-migration-compatibility-issues)
- [SQL Server-databases compatibel met SQL-Database migreren](sql-database-cloud-migrate.md#migrate-a-compatible-sql-server-database-to-sql-database)

## <a name="additional-resources"></a>Aanvullende bronnen

- [SQL Database V12](sql-database-v12-whats-new.md)
- [Transact-SQL gedeeltelijk of niet-ondersteunde functies](sql-database-transact-sql-information.md)
- [Migreren van SQL Server databases SQL Server migratie-assistent](http://blogs.msdn.com/b/ssma/)
