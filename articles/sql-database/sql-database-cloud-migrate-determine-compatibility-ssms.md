<properties
   pageTitle="Gebruik SQL Server Management Studio compatibiliteit voor de migratie naar Azure SQL-Database SQL-Database bepalen | Microsoft Azure"
   description="Microsoft Azure SQL-Database, databasemigratie, compatibiliteit met SQL-Database gegevens Tier toepassing Wizard exporteren"
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
   ms.date="08/29/2016"
   ms.author="carlrab"/>

# <a name="use-sql-server-management-studio-to-determine-sql-database-compatibility-before-migration-to-azure-sql-database"></a>Gebruik SQL Server Management Studio compatibiliteit voor de migratie naar Azure SQL-Database SQL-Database bepalen

> [AZURE.SELECTOR]
- [SSDT](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md)
- [SqlPackage](sql-database-cloud-migrate-determine-compatibility-sqlpackage.md)
- [SSMS](sql-database-cloud-migrate-determine-compatibility-ssms.md)
- [Upgrade Advisor](http://www.microsoft.com/download/details.aspx?id=48119)
- [SAMW](sql-database-cloud-migrate-fix-compatibility-issues.md)
 
In dit artikel u leert om te bepalen of een SQL Server-database is compatibel om te migreren naar een SQL-Database met de Wizard exporteren Tier toepassing in SQL Server Management Studio.

## <a name="using-sql-server-management-studio"></a>Met behulp van SQL Server Management Studio

1. Controleer of u de nieuwste versie van SQL Server Management Studio. Nieuwe versies van Management Studio worden maandelijks geactualiseerd op met updates voor de Azure portal gesynchroniseerd blijven.

     > [AZURE.IMPORTANT] Het wordt aanbevolen dat u altijd de meest recente versie van het Management Studio gebruiken om met updates voor Microsoft Azure en SQL-Database gesynchroniseerd blijven. [Bijwerken van SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).

2. Management Studio te openen en verbinding maken met de brondatabase in Object Explorer.
3. De brondatabase in Object Explorer met de rechtermuisknop, wijs **taken**en klikt u op **Exporteren gegevens-Tier-toepassing...**

    ![Een gegevens-tier-toepassing exporteren in het menu taken](./media/sql-database-cloud-migrate/TestForCompatibilityUsingSSMS01.png)

4. Klik op **volgende**en vervolgens op het tabblad **Instellingen** configureren om het Bacpac-bestand op een lokale locatie of op een Azure blob exporteren in de wizard exporteren. Als er geen compatibiliteitsproblemen database een Bacpac-bestand opgeslagen. Als er compatibiliteitsproblemen zijn, worden ze op de console worden weergegeven.

    ![Instellingen exporteren](./media/sql-database-cloud-migrate/TestForCompatibilityUsingSSMS02.png)

5. Om over te slaan met het exporteren van gegevens, klikt u op het **tabblad Geavanceerd** en schakel het selectievakje **Alles selecteren** . Ons doel is op dit moment alleen te testen op compatibiliteit.

    ![Instellingen exporteren](./media/sql-database-cloud-migrate/TestForCompatibilityUsingSSMS03.png)

6. Klik op **volgende** en klik vervolgens op **Voltooien**. Compatibiliteitsproblemen database, indien aanwezig, worden weergegeven nadat u de wizard valideert het schema.

    ![Instellingen exporteren](./media/sql-database-cloud-migrate/TestForCompatibilityUsingSSMS04.png)

7. Als er geen fouten worden weergegeven, de database compatibel is en u bent klaar om te migreren. Als er fouten zijn, moet u deze corrigeren. Overzicht van de fouten, klikt u op de **fout** voor **Validating schema**. 
    ![Instellingen exporteren](./media/sql-database-cloud-migrate/TestForCompatibilityUsingSSMS05.png)

8.  Als de *. Bacpac-bestand wordt gegenereerd, en vervolgens de database is compatibel met SQL-Database en u bent klaar om te migreren.

## <a name="next-steps"></a>Volgende stappen

- [Nieuwste versie van SSDT](https://msdn.microsoft.com/library/mt204009.aspx)
- [Nieuwste versie van SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx)
- [De databasemigratie compatibiliteitsproblemen oplossen](sql-database-cloud-migrate.md#fix-database-migration-compatibility-issues)
- [SQL Server-databases compatibel met SQL-Database migreren](sql-database-cloud-migrate.md#migrate-a-compatible-sql-server-database-to-sql-database)

## <a name="additional-resources"></a>Aanvullende bronnen

- [SQL Database V12](sql-database-v12-whats-new.md)
- [Transact-SQL gedeeltelijk of niet-ondersteunde functies](sql-database-transact-sql-information.md)
- [Migreren van SQL Server databases SQL Server migratie-assistent](http://blogs.msdn.com/b/ssma/)
