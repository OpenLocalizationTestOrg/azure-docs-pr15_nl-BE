<properties
   pageTitle="Corrigeren van compatibiliteitsproblemen van SQL Server-database voor de migratie naar een SQL-Database | Microsoft Azure"
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

# <a name="use-sql-azure-migration-wizard-to-fix-sql-server-database-compatibility-issues-before-migration-to-azure-sql-database"></a>Gebruik SQL Azure Migration Wizard voor herstellen SQL Server-database compatibiliteitsproblemen voor de migratie naar Azure SQL-Database

> [AZURE.SELECTOR]
- [SQL Azure migratiewizard](sql-database-cloud-migrate-fix-compatibility-issues.md) gebruiken
- [SSDT](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md) gebruiken
- [SSMS](sql-database-cloud-migrate-fix-compatibility-issues-ssms.md) gebruiken

In dit artikel leert u opsporen en oplossen van compatibiliteitsproblemen voor SQL Server-database met behulp van de Wizard SQL Azure migreren voordat u de migratie naar Azure SQL-Database.

## <a name="using-sql-azure-migration-wizard"></a>Met behulp van de wizard SQL Azure migratie

Gebruik het hulpprogramma [SQL Azure Migration wizard](http://sqlazuremw.codeplex.com/) CodePlex een T-SQL-script genereren uit een niet-compatibele brondatabase. Dit script wordt vervolgens omgezet door de wizard, zodat deze compatibel is met de SQL-Database. Maak verbinding met Azure SQL-Database voor het uitvoeren van het script. Deze tool analyseert ook traceringsbestanden om compatibiliteitsproblemen vast te stellen. Het script met schema alleen kan worden gegenereerd of gegevens kunt opnemen in de BCP-indeling. Aanvullende documentatie, met inbegrip van stapsgewijze begeleiding is beschikbaar op CodePlex op [SQL Azure Migration wizard](http://sqlazuremw.codeplex.com/).  

 ![SAMW migratie diagram](./media/sql-database-cloud-migrate/02SAMWDiagram.png)

  > [AZURE.NOTE] Niet alle incompatibel schema dat door de wizard zijn gedetecteerd, kan worden opgelost met de ingebouwde transformaties. Incompatibele script dat niet kan worden opgelost worden gerapporteerd als fouten met opmerkingen in het script gegenereerde geïnjecteerd. Als veel fouten worden gedetecteerd, gebruikt u Visual Studio of SQL Server Management Studio te doorlopen en elke fout die kan niet worden hersteld met de Wizard migreren van SQL Server op te lossen.

## <a name="next-steps"></a>Volgende stappen

- [Nieuwste versie van SSDT](https://msdn.microsoft.com/library/mt204009.aspx)
- [Nieuwste versie van SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx)
- [SQL Server-databases compatibel met SQL-Database migreren](sql-database-cloud-migrate.md#migrate-a-compatible-sql-server-database-to-sql-database)

## <a name="additional-resources"></a>Aanvullende bronnen

- [SQL Database V12](sql-database-v12-whats-new.md)
- [Transact-SQL gedeeltelijk of niet-ondersteunde functies](sql-database-transact-sql-information.md)
- [Migreren van SQL Server databases SQL Server migratie-assistent](http://blogs.msdn.com/b/ssma/)
