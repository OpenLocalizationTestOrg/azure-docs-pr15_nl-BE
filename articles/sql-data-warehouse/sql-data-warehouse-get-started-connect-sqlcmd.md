<properties
   pageTitle="Query Azure SQL Data Warehouse (sqlcmd) | Microsoft Azure"
   description="Azure SQL Data Warehouse opvragen met de Command-line hulpprogramma sqlcmd."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="sonyam"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/06/2016"
   ms.author="barbkess;sonyama"/>

# <a name="query-azure-sql-data-warehouse-sqlcmd"></a>Query Azure SQL Data Warehouse (sqlcmd)

> [AZURE.SELECTOR]
- [Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md)
- [Azure Machine Learning](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md)
- [Visual Studio](sql-data-warehouse-query-visual-studio.md)
- [Sqlcmd](sql-data-warehouse-get-started-connect-sqlcmd.md) 

In dit scenario wordt het hulpprogramma [sqlcmd][] een datawarehouse Azure SQL query.  

## <a name="1-connect"></a>1. verbinding

Aan de slag met [sqlcmd][], opent u de opdrachtprompt en voer **sqlcmd** , gevolgd door de verbindingsreeks voor de SQL Data Warehouse-database. De verbindingsreeks vereist de volgende parameters:

+ **Server (-S):** Server in de vorm `<`servernaam`>`. database.windows.net
+ **Database (-d):** De naam van de database.
+ **Inschakelen id's tussen aanhalingstekens (-I):** Id's tussen aanhalingstekens moeten verbinding maken met een exemplaar van SQL Data Warehouse zijn ingeschakeld.

SQL Server-verificatie gebruikt, moet u de parameters van de gebruikersnaam en wachtwoord toevoegen:

+ **Gebruiker (-U):** Server-gebruiker in het formulier `<`gebruiker`>`
+ **Wachtwoord (-P):** Het wachtwoord dat is gekoppeld aan de gebruiker.

Bijvoorbeeld, uitzien de verbindingsreeks als volgt:

```sql
C:\>sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I
```

Azure Active Directory geïntegreerde verificatie gebruiken, moet u de parameters Azure Active Directory toevoegen:

+ **Azure Active Directory-verificatie (-G):** Azure Active Directory gebruiken voor verificatie

Bijvoorbeeld, uitzien de verbindingsreeks als volgt:

```sql
C:\>sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -G -I
```

> [AZURE.NOTE] U moet [Azure Active Directory-verificatie inschakelen](sql-data-warehouse-authentication.md) voor verificatie met Active Directory.

## <a name="2-query"></a>2. de query

Nadat de verbinding, kunt u alle ondersteunde Transact-SQL-instructies ten opzichte van het exemplaar uitgeven.  In dit voorbeeld worden query's verzonden in de interactieve modus.

```sql
C:\>sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I
1> SELECT name FROM sys.tables;
2> GO
3> QUIT
```

Deze volgende voorbeelden ziet u hoe u uw query's kunt uitvoeren in batchmodus met behulp van de optie -Q of de SQL voor sqlcmd sluizen.

```sql
sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I -Q "SELECT name FROM sys.tables;"
```

```sql
"SELECT name FROM sys.tables;" | sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I > .\tables.out
```

## <a name="next-steps"></a>Volgende stappen

Zie de [documentatie sqlcmd][sqlcmd] voor meer informatie over de details over de beschikbare opties in sqlcmd.

<!--Image references-->

<!--Article references-->

<!--MSDN references--> 
[Sqlcmd]: https://msdn.microsoft.com/library/ms162773.aspx
[Azure portal]: https://portal.azure.com

<!--Other Web references-->
