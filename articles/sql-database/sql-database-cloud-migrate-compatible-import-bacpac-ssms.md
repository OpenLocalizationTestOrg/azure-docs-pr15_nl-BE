<properties
   pageTitle="Een SQL Server-database migreren naar Azure SQL-Database | Microsoft Azure"
   description="Microsoft Azure SQL Database database implementeren, databasemigratie, database importeren, wizard migreren-database exporteren"
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

# <a name="import-from-bacpac-to-sql-database-using-ssms"></a>SQL-database met behulp van SSMS van BACPAC importeren

> [AZURE.SELECTOR]
- [SSMS](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md)
- [SqlPackage](sql-database-cloud-migrate-compatible-import-bacpac-sqlpackage.md)
- [Azure portal](sql-database-import.md)
- [PowerShell](sql-database-import-powershell.md)

In dit artikel ziet u hoe te importeren uit een bestand [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) SQL-database met de Wizard exporteren Tier toepassing in SQL Server Management Studio.

> [AZURE.NOTE] De volgende stappen wordt ervan uitgegaan dat u al uw logische Azure SQL-exemplaar hebt ingericht en de informatie bij de hand hebt.

1. Controleer of u de nieuwste versie van SQL Server Management Studio. Nieuwe versies van Management Studio worden maandelijks geactualiseerd op met updates voor de Azure portal gesynchroniseerd blijven.

     > [AZURE.IMPORTANT] Het wordt aanbevolen dat u altijd de meest recente versie van het Management Studio gebruiken om met updates voor Microsoft Azure en SQL-Database gesynchroniseerd blijven. [Bijwerken van SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).

2. Verbinding maken met uw server Azure SQL-Database, klik met de rechtermuisknop op de map **Databases** en klik op **importeren gegevens-tier-toepassing...**

    ![Gegevens-tier toepassing menu-item importeren](./media/sql-database-cloud-migrate/MigrateUsingBACPAC03.png)

3.  De database maken in Azure SQL-Database, Bacpac-bestanden importeren vanaf uw lokale schijf of de container waaraan u het Bacpac-bestand hebt geüpload en Azure opslag account selecteren.

    ![Instellingen importeren](./media/sql-database-cloud-migrate/MigrateUsingBACPAC04.png)

     > [AZURE.IMPORTANT] Bij het importeren van een BACPAC van Azure blob-opslag, standaard opslag gebruiken. Een BACPAC importeren uit de opslag van de premie wordt niet ondersteund.

4.  Geef de **nieuwe naam** voor de database op Azure SQL DB, stelt u de **Editie van Microsoft Azure SQL-Database** (servicelaag), **maximale grootte van de database**en **Service doelstelling** (performance niveau).

    ![Database-instellingen](./media/sql-database-cloud-migrate/MigrateUsingBACPAC05.png)

5.  Klik op **volgende** en klik vervolgens op **Voltooien** om het Bacpac-bestand importeren in een nieuwe database op de server Azure SQL-Database.

6. Object Explorer gebruiken, verbinding maken met uw database gemigreerd in de Azure SQL-databaseserver.

6.  Met behulp van de portal Azure bekijken uw database en de bijbehorende eigenschappen.

## <a name="next-steps"></a>Volgende stappen

- [Nieuwste versie van SSDT](https://msdn.microsoft.com/library/mt204009.aspx)
- [Nieuwste versie van SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx)

## <a name="additional-resources"></a>Aanvullende bronnen

- [SQL Database V12](sql-database-v12-whats-new.md)
- [Transact-SQL gedeeltelijk of niet-ondersteunde functies](sql-database-transact-sql-information.md)
- [Migreren van SQL Server databases SQL Server migratie-assistent](http://blogs.msdn.com/b/ssma/)
