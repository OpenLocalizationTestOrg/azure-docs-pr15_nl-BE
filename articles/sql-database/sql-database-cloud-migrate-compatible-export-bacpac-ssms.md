
<properties
   pageTitle="Een SQL Server-database exporteren naar een Bacpac-bestand met behulp van SQL Server Management Studio | Microsoft Azure"
   description="Microsoft Azure SQL-Database, databasemigratie, database exporteren, wizard exporteren gegevens Tier toepassing Bacpac-bestand exporteren"
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
   ms.workload="data-management"
   ms.date="08/16/2016"
   ms.author="carlrab"/>

# <a name="export-a-sql-server-database-to-a-bacpac-file-using-sql-server-management-studio"></a>Een SQL Server-database exporteren naar een Bacpac-bestand met behulp van SQL Server Management Studio

> [AZURE.SELECTOR]
- [SSMS](sql-database-cloud-migrate-compatible-export-bacpac-ssms.md)
- [SqlPackage](sql-database-cloud-migrate-compatible-export-bacpac-sqlpackage.md)

 
In dit artikel ziet u hoe u een SQL Server-database exporteren naar een [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) -bestand met de Wizard exporteren Tier toepassing in SQL Server Management Studio. 

1. Controleer of u de nieuwste versie van SQL Server Management Studio. Nieuwe versies van Management Studio worden maandelijks geactualiseerd op met updates voor de Azure portal gesynchroniseerd blijven.

     > [AZURE.IMPORTANT] Het wordt aanbevolen dat u altijd de meest recente versie van het Management Studio gebruiken om met updates voor Microsoft Azure en SQL-Database gesynchroniseerd blijven. [Bijwerken van SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).

2. Management Studio te openen en verbinding maken met de brondatabase in Object Explorer.

    ![Een gegevens-tier-toepassing exporteren in het menu taken](./media/sql-database-cloud-migrate/MigrateUsingBACPAC01.png)

3. De brondatabase in Object Explorer met de rechtermuisknop, wijs **taken**en klikt u op **Exporteren gegevens-Tier-toepassing...**

    ![Een gegevens-tier-toepassing exporteren in het menu taken](./media/sql-database-cloud-migrate/TestForCompatibilityUsingSSMS01.png)

4. In de wizard Exporteren het Bacpac-bestand aan de locatie van een lokale schijf of naar een Azure blob-uitvoer te configureren. De geëxporteerde BACPAC bevat altijd de volledige databaseschema en gegevens uit alle tabellen standaard. Gebruik het tabblad Geavanceerd als u wilt uitsluiten van de gegevens van bepaalde of alle tabellen. U wilt mogelijk, bijvoorbeeld alleen de gegevens voor tabellen in plaats van alle tabellen exporteren.

***Belangrijk*** Bij het exporteren van een BACPAC naar Azure blobopslag, standaard opslag gebruiken. Een BACPAC importeren uit de opslag van de premie wordt niet ondersteund.

    ![Export settings](./media/sql-database-cloud-migrate/MigrateUsingBACPAC02.png)


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
