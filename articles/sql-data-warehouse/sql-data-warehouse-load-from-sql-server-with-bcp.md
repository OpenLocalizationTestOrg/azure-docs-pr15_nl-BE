<properties
   pageTitle="Laden van gegevens van SQL Server in Azure SQL Data Warehouse (bcp) | Microsoft Azure"
   description="Voor een kleine gegevensgrootte wordt bcp gegevens uit SQL Server om bestanden te exporteren en de gegevens rechtstreeks importeren in Azure SQL Data Warehouse."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="06/30/2016"
   ms.author="lodipalm;barbkess;sonyama"/>


# <a name="load-data-from-sql-server-into-azure-sql-data-warehouse-flat-files"></a>Laden van gegevens van SQL Server in Azure SQL Data Warehouse (bestanden)

> [AZURE.SELECTOR]
- [SSI 'S](sql-data-warehouse-load-from-sql-server-with-integration-services.md)
- [PolyBase](sql-data-warehouse-load-from-sql-server-with-polybase.md)
- [BCP](sql-data-warehouse-load-from-sql-server-with-bcp.md)

Voor kleine gegevensverzamelingen, kunt u het opdrachtregelprogramma bcp gegevens exporteren van SQL Server en laadt u deze vervolgens rechtstreeks naar Azure SQL Data Warehouse.

In deze zelfstudie wordt u bcp te gebruiken:

- Een tabel uit exporteren van SQL Server met behulp van de bcp-opdracht (of maakt u een eenvoudig voorbeeld van een bestand)
- De tabel importeren vanaf een plat bestand naar SQL Data Warehouse.
- Statistieken van de geladen gegevens maken.

>[AZURE.VIDEO loading-data-into-azure-sql-data-warehouse-with-bcp]

## <a name="before-you-begin"></a>Voordat u begint

### <a name="prerequisites"></a>Vereisten

Stap in deze zelfstudie, wilt u het volgende nodig:

- Een SQL Data Warehouse-database
- Het bcp-opdrachtregelprogramma geïnstalleerd
- Het opdrachtregelprogramma van sqlcmd geïnstalleerd

De hulpprogramma's bcp en sqlcmd kunt u downloaden vanaf het [Microsoft Download Center][].

### <a name="data-in-ascii-or-utf-16-format"></a>Gegevens in ASCII- of UTF-16 indeling

Als u deze zelfstudie met uw eigen gegevens probeert, moeten uw gegevens de ASCII- of UTF-16-codering gebruiken aangezien bcp biedt geen ondersteuning voor UTF-8. 

PolyBase biedt ondersteuning voor UTF-8, maar nog geen ondersteuning voor UTF-16. Houd er rekening mee dat als u wilt combineren bcp met PolyBase u transformeren naar UTF-8 moet na het exporteren van SQL Server. 


## <a name="1-create-a-destination-table"></a>1. Maak een doeltabel

Definieert een tabel in SQL Data Warehouse die de doeltabel voor de belasting. De kolommen in de tabel moeten overeenkomen met de gegevens in elke rij van het gegevensbestand.

Een tabel maken, open een opdrachtprompt en sqlcmd.exe gebruiken de volgende opdracht uit te voeren:


```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "
    CREATE TABLE DimDate2
    (
        DateId INT NOT NULL,
        CalendarQuarter TINYINT NOT NULL,
        FiscalQuarter TINYINT NOT NULL
    )
    WITH
    (
        CLUSTERED COLUMNSTORE INDEX,
        DISTRIBUTION = ROUND_ROBIN
    );
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

## <a name="4-create-statistics"></a>4. statistieken maken

SQL Data Warehouse biedt nog geen ondersteuning voor automatisch maken of statistieken automatisch bijwerken. Als u de beste prestaties van query's, is het belangrijk om statistieken te maken op alle kolommen van alle tabellen na de eerste keer worden geladen of nadat u aanzienlijke wijzigingen optreden in de gegevens. Zie voor een gedetailleerde uitleg van de statistieken, [Statistieken][]. 

Voer de volgende opdracht om statistieken te maken op de tabel pas wordt geladen.

```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "
    create statistics [DateId] on [DimDate2] ([DateId]);
    create statistics [CalendarQuarter] on [DimDate2] ([CalendarQuarter]);
    create statistics [FiscalQuarter] on [DimDate2] ([FiscalQuarter]);
"
```

## <a name="5-export-data-from-sql-data-warehouse"></a>5. gegevens exporteren vanuit SQL Data Warehouse
U kunt de gegevens die u zojuist hebt geladen buiten het datawarehouse SQL exporteren voor de lol.  De opdracht exporteren is precies hetzelfde als het exporteren van SQL Server.

Er is echter een verschil in de resultaten. Omdat de gegevens worden opgeslagen in de gedistribueerde locaties in SQL Data Warehouse, wanneer u gegevens exporteert schrijft elk computerknooppunt u deze gegevens naar het uitvoerbestand. De volgorde van de gegevens in het uitvoerbestand is waarschijnlijk anders dan de volgorde van de gegevens in het invoerbestand.

### <a name="export-a-table-and-compare-exported-results"></a>Een tabel exporteren en het geëxporteerde resultaten vergelijken

De geëxporteerde gegevens bekijken, opent u een opdrachtprompt en voert deze opdracht met uw eigen parameters. Servernaam is de naam van uw Azure logische SQL Server.

```sql
bcp DimDate2 out C:\Temp\DimDate2_export.txt -S <Server Name> -d <Database Name> -U <Username> -P <password> -q -c -t ','
```
U kunt controleren of dat de gegevens correct zijn geëxporteerd door het nieuwe bestand te openen. De gegevens in het bestand moet overeenkomen met de onderstaande tekst, maar waarschijnlijk in een andere volgorde worden gesorteerd:

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

### <a name="export-the-results-of-a-query"></a>De resultaten van een query exporteren

De functie **queryout** van bcp kunt u de resultaten van een query in plaats van de hele tabel exporteren exporteren. 

## <a name="next-steps"></a>Volgende stappen
Zie voor een overzicht van de geladen [gegevens laden in SQL Data Warehouse][].
Zie [SQL Data Warehouse ontwikkelen-overzicht][]voor meer tips voor ontwikkeling.
Zie [Overzicht van de tabel][] of de [syntaxis CREATE TABLE][] voor meer informatie over het maken van een tabel in SQL Data Warehouse.

<!--Image references-->

<!--Article references-->

[Laden van gegevens in SQL Data Warehouse]: ./sql-data-warehouse-overview-load.md
[SQL Data Warehouse ontwikkelen-overzicht]: ./sql-data-warehouse-overview-develop.md
[Tabel-overzicht]: ./sql-data-warehouse-tables-overview.md
[Statistieken]: ./sql-data-warehouse-tables-statistics.md

<!--MSDN references-->
[bcp]: https://msdn.microsoft.com/library/ms162802.aspx
[De syntaxis CREATE TABLE]: https://msdn.microsoft.com/library/mt203953.aspx

<!--Other Web references-->
[Microsoft Downloadcentrum]: https://www.microsoft.com/download/details.aspx?id=36433
