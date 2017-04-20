<properties
   pageTitle="Gegevens uit een CSV-bestand laden in Azure SQL Databaase (bcp) | Microsoft Azure"
   description="Gebruikt voor een kleine gegevensgrootte bcp om gegevens te importeren in Azure SQL-Database."
   services="sql-database"
   documentationCenter="NA"
   authors="CarlRabeler"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/13/2016"
   ms.author="carlrab"/>


# <a name="load-data-from-csv-into-azure-sql-data-warehouse-flat-files"></a>Gegevens laden vanuit CSV in Azure SQL Data Warehouse (bestanden)

U kunt het opdrachtregelprogramma bcp gegevens uit een CSV-bestand importeren in Azure SQL-Database.

## <a name="before-you-begin"></a>Voordat u begint

### <a name="prerequisites"></a>Vereisten

Stap in deze zelfstudie, wilt u het volgende nodig:

- Een logische server Azure SQL-Database en de database
- Het bcp-opdrachtregelprogramma geïnstalleerd
- Het opdrachtregelprogramma van sqlcmd geïnstalleerd

De hulpprogramma's bcp en sqlcmd kunt u downloaden vanaf het [Microsoft Download Center][].

### <a name="data-in-ascii-or-utf-16-format"></a>Gegevens in ASCII- of UTF-16 indeling

Als u deze zelfstudie met uw eigen gegevens probeert, moeten uw gegevens de ASCII- of UTF-16-codering gebruiken aangezien bcp biedt geen ondersteuning voor UTF-8. 

## <a name="1-create-a-destination-table"></a>1. Maak een doeltabel

Een tabel in SQL-Database definiëren als de doeltabel. De kolommen in de tabel moeten overeenkomen met de gegevens in elke rij van het gegevensbestand.

Een tabel maken, open een opdrachtprompt en sqlcmd.exe gebruiken de volgende opdracht uit te voeren:


```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "
    CREATE TABLE DimDate2
    (
        DateId INT NOT NULL,
        CalendarQuarter TINYINT NOT NULL,
        FiscalQuarter TINYINT NOT NULL
    )
    ;
"
```


## <a name="2-create-a-source-data-file"></a>2. een brongegevensbestand maken

Open Kladblok en kopieer de volgende regels met gegevens in een nieuw tekstbestand en sla dit bestand op de lokale tijdelijke map C:\Temp\DimDate2.txt. Deze gegevens zijn in ASCII-indeling.

```
20150301,1,3
20150501,2,4
20151001,4,2
20150201,1,3
20151201,4,2
20150801,3,1
20150601,2,4
20151101,4,2
20150401,2,4
20150701,3,1
20150901,3,1
20150101,1,3
```

(Optioneel) Om uw eigen gegevens exporteren uit een SQL Server-database, opent u een opdrachtprompt en voer de volgende opdracht uit. TableName, servernaam DatabaseName, gebruikersnaam en wachtwoord vervangen door uw eigen informatie.

```sql
bcp <TableName> out C:\Temp\DimDate2_export.txt -S <ServerName> -d <DatabaseName> -U <Username> -P <Password> -q -c -t ','
```

## <a name="3-load-the-data"></a>3. de gegevens worden geladen
Open een opdrachtprompt en voert u de volgende opdracht de waarden voor de naam van de Server, Database naam, gebruikersnaam en wachtwoord te vervangen door uw eigen informatie voor het laden van de gegevens.

```sql
bcp DimDate2 in C:\Temp\DimDate2.txt -S <ServerName> -d <DatabaseName> -U <Username> -P <password> -q -c -t  ','
```

Met deze opdracht kunt u controleren of dat de gegevens juist is geladen

```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "SELECT * FROM DimDate2 ORDER BY 1;"
```

Het resultaat ziet er zo uit:

DateId |CalendarQuarter |FiscalQuarter
----------- |--------------- |-------------
20150101 |1 |3
20150201 |1 |3
20150301 |1 |3
20150401 |2 |4
20150501 |2 |4
20150601 |2 |4
20150701 |3 |1
20150801 |3 |1
20150801 |3 |1
20151001 |4 |2
20151101 |4 |2
20151201 |4 |2


## <a name="next-steps"></a>Volgende stappen

Zie [SQL Server-database migreren](sql-database-cloud-migrate.md)om te migreren van SQL Server-database.

<!--MSDN references-->
[bcp]: https://msdn.microsoft.com/library/ms162802.aspx
[CREATE TABLE syntax]: https://msdn.microsoft.com/library/mt203953.aspx

<!--Other Web references-->
[Microsoft Downloadcentrum]: https://www.microsoft.com/download/details.aspx?id=36433
