<properties
   pageTitle="SQL Server-database migreren naar Microsoft Azure Database Wizard Database implementeren met SQL-Database | Microsoft Azure"
   description="Microsoft Azure SQL-Database, databasemigratie, Microsoft Azure Wizard Database"
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

# <a name="migrate-sql-server-database-to-sql-database-using-deploy-database-to-microsoft-azure-database-wizard"></a>SQL Server-database naar Microsoft Azure Database Wizard Database implementeren met SQL-Database migreren


> [AZURE.SELECTOR]
- [SSMS migratiewizard](sql-database-cloud-migrate-compatible-using-ssms-migration-wizard.md)
- [Exporteren naar bestand Bacpac-](sql-database-cloud-migrate-compatible-export-bacpac-ssms.md)
- [Importeren uit bestand Bacpac-](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md)
- [Transactionele replicatie](sql-database-cloud-migrate-compatible-using-transactional-replication.md)

Een [compatibele SQL Server-database](sql-database-cloud-migrate.md) Hiermee de Database implementeren om de wizard Microsoft Azure Database in SQL Server Management Studio rechtstreeks in uw Azure SQL-databaseserver.

## <a name="use-the-deploy-database-to-microsoft-azure-database-wizard"></a>De Database distribueren naar Microsoft Azure databasewizard gebruiken

> [AZURE.NOTE] De volgende stappen wordt ervan uitgegaan dat er een [Database van SQL server ingericht](https://azure.microsoft.com/documentation/learning-paths/sql-database-training-learn-sql-database/).

1. Controleer of u de nieuwste versie van SQL Server Management Studio. Nieuwe versies van Management Studio worden maandelijks geactualiseerd op met updates voor de Azure portal gesynchroniseerd blijven.

    > [AZURE.IMPORTANT] Het wordt aanbevolen dat u altijd de meest recente versie van het Management Studio gebruiken om met updates voor Microsoft Azure en SQL-Database gesynchroniseerd blijven. [Bijwerken van SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).

2. Management Studio te openen en verbinding met uw SQL Server-database te migreren in Object Explorer.
3. Klik met de rechtermuisknop op de database in het Explorer-Object, wijst u **taken**en klikt u op **Database implementeren met Microsoft Azure SQL-Database...**

    ![Vanuit het menu taken op Azure implementeren](./media/sql-database-cloud-migrate/MigrateUsingDeploymentWizard01.png)

4.  Klik op **volgende**in de wizard uitvoeren en klik vervolgens op **verbinden** om de verbinding met de SQL-Database-server configureren.

    ![Vanuit het menu taken op Azure implementeren](./media/sql-database-cloud-migrate/MigrateUsingDeploymentWizard002.png)

5. In de verbinding met de Server van het dialoogvenster, voer de verbindingsinformatie om verbinding met de Database van SQL server.

    ![Vanuit het menu taken op Azure implementeren](./media/sql-database-cloud-migrate/MigrateUsingDeploymentWizard00.png)

5.  De volgende onderwerpen voor het [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) -bestand dat u met deze wizard wordt gemaakt tijdens het migratieproces bieden:

 - De **nieuwe naam** 
 - De **Editie van Microsoft Azure SQL-Database** ([servicelaag](sql-database-service-tiers.md))
 - De **maximale grootte van de database**
 - De **Doelstelling van de Service** (performance niveau).
 - De **tijdelijke bestandsnaam**  

    ![Instellingen exporteren](./media/sql-database-cloud-migrate/MigrateUsingDeploymentWizard02.png)

6.  Voltooi de wizard. Afhankelijk van de grootte en complexiteit van de database nemen implementatie van een paar minuten vele uren. Deze wizard detecteert compatibiliteitsproblemen, fouten worden weergegeven op het scherm als de migratie niet doorgaat. Voor instructies over het oplossen van problemen met toepassingscompatibiliteit, gaat u naar [problemen met toepassingscompatibiliteit te verhelpen](sql-database-cloud-migrate-fix-compatibility-issues.md).

7.  Object Explorer gebruiken, verbinding maken met uw database gemigreerd in de Azure SQL-databaseserver.
8.  Met behulp van de portal Azure bekijken uw database en de bijbehorende eigenschappen.

## <a name="next-steps"></a>Volgende stappen

- [Nieuwste versie van SSDT](https://msdn.microsoft.com/library/mt204009.aspx)
- [Nieuwste versie van SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx)

## <a name="additional-resources"></a>Aanvullende bronnen

- [SQL Database V12](sql-database-v12-whats-new.md)
- [Transact-SQL gedeeltelijk of niet-ondersteunde functies](sql-database-transact-sql-information.md)
- [Migreren van SQL Server databases SQL Server migratie-assistent](http://blogs.msdn.com/b/ssma/)
