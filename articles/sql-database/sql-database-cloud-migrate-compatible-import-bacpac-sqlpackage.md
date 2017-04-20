<properties
   pageTitle="In de SQL-Database importeren vanuit een Bacpac-bestand met behulp van SqlPackage"
   description="Microsoft Azure SQL-Database, databasemigratie, database importeren, Bacpac-bestand importeert, sqlpackage"
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

# <a name="import-to-sql-database-from-a-bacpac-file-using-sqlpackage"></a>In de SQL-Database importeren vanuit een Bacpac-bestand met behulp van SqlPackage

> [AZURE.SELECTOR]
- [SSMS](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md)
- [SqlPackage](sql-database-cloud-migrate-compatible-import-bacpac-sqlpackage.md)
- [Azure portal](sql-database-import.md)
- [PowerShell](sql-database-import-powershell.md)

In dit artikel ziet u hoe u in de SQL-database importeren vanuit een [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) -bestand met het opdrachtregelprogramma [SqlPackage](https://msdn.microsoft.com/library/hh550080.aspx) . Dit hulpprogramma wordt geleverd met de nieuwste versies van [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) en [SQL Server Data Tools voor Visual Studio](https://msdn.microsoft.com/library/mt204009.aspx), of u de nieuwste versie van [SqlPackage](https://www.microsoft.com/en-us/download/details.aspx?id=53876) rechtstreeks vanaf het Microsoft Downloadcentrum kunt downloaden.


> [AZURE.NOTE] De volgende stappen wordt uitgegaan van een Database van SQL server al hebt ingericht, de informatie bij de hand hebt en hebt gecontroleerd of de brondatabase compatibel is.

## <a name="import-from-a-bacpac-file-into-azure-sql-database-using-sqlpackage"></a>Importeren uit een bestand Bacpac-Azure SQL-Database met behulp van SqlPackage

Ga als volgt uit een bestand Bacpac-compatibele SQL Server-database (of Azure SQL-database) importeren met het opdrachtregelprogramma [SqlPackage.exe](https://msdn.microsoft.com/library/hh550080.aspx) .

> [AZURE.NOTE] De volgende stappen wordt ervan uitgegaan dat u al een Azure SQL-databaseserver hebt ingericht en de informatie bij de hand hebt.

1. Een opdrachtpromptvenster openen en wijzigen van een map met het opdrachtregelprogramma sqlpackage.exe - Dit hulpprogramma wordt geleverd met zowel Visual Studio en SQL Server.
2. De volgende sqlpackage.exe-opdracht met de volgende argumenten voor uw omgeving worden uitgevoerd:

    `sqlpackage.exe /Action:Import /tsn:< server_name > /tdn:< database_name > /tu:< user_name > /tp:< password > /sf:< source_file >`

  	| Argument  | Beschrijving  |
  	|---|---|
  	| < servernaam >  | doel-servernaam  |
  	| < databasenaam >  | naam van doel-database  |
  	| < gebruikersnaam >  | de gebruikersnaam voor de doelserver |
  	| < wachtwoord >  | het wachtwoord van de gebruiker  |
  	| < source_file >  | de bestandsnaam en locatie voor het geïmporteerde Bacpac-bestand  |

    ![Een gegevens-tier-toepassing exporteren in het menu taken](./media/sql-database-cloud-migrate/TestForCompatibilityUsingSQLPackage01c.png)

## <a name="next-steps"></a>Volgende stappen

- [Nieuwste versie van SSDT](https://msdn.microsoft.com/library/mt204009.aspx)
- [Nieuwste versie van SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx)

## <a name="additional-resources"></a>Aanvullende bronnen

- [SQL Database V12](sql-database-v12-whats-new.md)
- [Transact-SQL gedeeltelijk of niet-ondersteunde functies](sql-database-transact-sql-information.md)
- [Migreren van SQL Server databases SQL Server migratie-assistent](http://blogs.msdn.com/b/ssma/)
