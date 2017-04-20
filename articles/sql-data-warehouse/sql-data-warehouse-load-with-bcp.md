<properties
   pageTitle="Bcp gebruiken om gegevens te laden in SQL Data Warehouse | Microsoft Azure"
   description="Informatie over welke bcp is en hoe u deze gebruikt voor scenario's voor gegevensopslag."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="twounder"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="10/10/2016"
   ms.author="mausher;barbkess;sonyama"/>


# <a name="load-data-with-bcp"></a>Laden van gegevens met bcp

> [AZURE.SELECTOR]
- [Redgate](sql-data-warehouse-load-with-redgate.md)  
- [Data Factory](sql-data-warehouse-get-started-load-with-azure-data-factory.md)  
- [PolyBase](sql-data-warehouse-get-started-load-with-polybase.md)  
- [BCP](sql-data-warehouse-load-with-bcp.md)


**[BCP][]** is een opdrachtregelprogramma bulk load-hulpprogramma waarmee u gegevens te kopiëren tussen SQL Server-gegevensbestanden en SQL Data Warehouse. Bcp gebruiken een groot aantal rijen in het datawarehouse SQL-tabellen importeren of gegevens uit een SQL Server-tabellen exporteren naar bestanden. Bcp vereist, behalve wanneer gebruikt in combinatie met de optie queryout, zonder enige kennis van Transact-SQL.

BCP is een snelle en eenvoudige manier om kleinere gegevenssets te verplaatsen naar en vanuit een SQL Data Warehouse-database. Is afhankelijk van de exacte hoeveelheid gegevens die wordt aanbevolen om te laden/extract via bcp op het netwerk van verbinding met het gegevenscentrum Azure.  In het algemeen dimensietabellen worden geladen en gemakkelijk kunnen worden geëxtraheerd met bcp, bcp wordt echter niet aanbevolen voor het laden of grote hoeveelheden gegevens te extraheren.  Polybase is het aanbevolen hulpmiddel voor het laden en het ophalen van grote hoeveelheden gegevens als een betere taak gebruik te maken van de architectuur massively parallelle verwerking van SQL Data Warehouse.

U kunt met bcp:

- Een eenvoudig hulpprogramma voor de opdrachtregel gebruiken om gegevens te laden in SQL Data Warehouse.
- Een eenvoudig hulpprogramma voor de opdrachtregel gebruiken om gegevens te extraheren uit SQL Data Warehouse.

In deze zelfstudie wordt uitgelegd hoe u naar:

- Gegevens importeren in een tabel met de bcp in opdracht
- Gegevens exporteren uit een tabel uisng de bcp-opdracht

>[AZURE.VIDEO loading-data-into-azure-sql-data-warehouse-with-bcp]

## <a name="prerequisites"></a>Vereisten

Stap in deze zelfstudie, wilt u het volgende nodig:

- Een SQL Data Warehouse-database
- Het bcp opdrachtregelprogramma geïnstalleerd
- Het hulpprogramma SQLCMD opdrachtregel geïnstalleerd

>[AZURE.NOTE] De hulpprogramma's bcp en sqlcmd kunt u downloaden vanaf het [Microsoft Download Center][].

## <a name="import-data-into-sql-data-warehouse"></a>Importeren van gegevens in SQL Data Warehouse

In deze zelfstudie wordt u een tabel maakt in Azure SQL Data Warehouse en importeren van gegevens in de tabel.

### <a name="step-1-create-a-table-in-azure-sql-data-warehouse"></a>Stap 1: Een tabel maken in Azure SQL Data Warehouse

Gebruik sqlcmd naar de volgende query om een tabel te maken op uw exemplaar vanaf een opdrachtprompt:

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

>[AZURE.NOTE] Zie [Overzicht van de tabel][] of de [syntaxis CREATE TABLE][] voor meer informatie over het maken van een tabel in SQL Data Warehouse en de opties die beschikbaar zijn in de WITH-component.

### <a name="step-2-create-a-source-data-file"></a>Stap 2: Maak een brongegevensbestand

Open Kladblok en kopieer de volgende regels met gegevens in een nieuw tekstbestand en sla dit bestand op de lokale tijdelijke map C:\Temp\DimDate2.txt.

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

> [AZURE.NOTE] Het is belangrijk te onthouden dat bcp.exe biedt geen ondersteuning voor de codering UTF-8 bestand. Gebruik ASCII- of UTF-16-gecodeerde bestanden bij het gebruik van bcp.exe.

### <a name="step-3-connect-and-import-the-data"></a>Stap 3: Verbinding maken en de gegevens importeren
Bcp wordt gebruikt, kunt u verbinding maken en de gegevens importeren met de volgende opdracht, vervangt de waarden:

```sql
bcp DimDate2 in C:\Temp\DimDate2.txt -S <Server Name> -d <Database Name> -U <Username> -P <password> -q -c -t  ','
```

U kunt controleren of dat de gegevens door het uitvoeren van de volgende query met behulp van sqlcmd is geladen:

```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "SELECT * FROM DimDate2 ORDER BY 1;"
```

Dit moet de volgende resultaten retourneren:

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

### <a name="step-4-create-statistics-on-your-newly-loaded-data"></a>Stap 4: Statistieken maken voor uw nieuwe geladen gegevens

Azure SQL Data Warehouse biedt nog geen ondersteuning voor automatische maken of automatische update statistieken. Om u de beste prestaties van uw query's, is het belangrijk dat statistieken op alle kolommen van alle tabellen worden gemaakt na de eerste keer worden geladen of belangrijke wijzigingen optreden in de gegevens. Zie het onderwerp van de [Statistieken][] in de groep onderwerpen opstellen voor een gedetailleerde uitleg van de statistieken. Hieronder is een snelle voorbeeld van het maken van statistieken over de tabellen bevatten geladen in dit voorbeeld

Voer de volgende instructies voor de statistieken maken vanaf een opdrachtprompt sqlcmd:

```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "
    create statistics [DateId] on [DimDate2] ([DateId]);
    create statistics [CalendarQuarter] on [DimDate2] ([CalendarQuarter]);
    create statistics [FiscalQuarter] on [DimDate2] ([FiscalQuarter]);
"
```

## <a name="export-data-from-sql-data-warehouse"></a>Exporteer gegevens uit SQL Data Warehouse
In deze zelfstudie maakt u een gegevensbestand uit een tabel in SQL Data Warehouse. Wij zullen de gegevens die we eerder hebben gemaakt exporteren naar een nieuw bestand genaamd DimDate2_export.txt.

### <a name="step-1-export-the-data"></a>Stap 1: De gegevens exporteren

Met het hulpprogramma bcp, kunt u verbinding maken en exporteren van gegevens met de volgende opdracht, vervangt de waarden:

```sql
bcp DimDate2 out C:\Temp\DimDate2_export.txt -S <Server Name> -d <Database Name> -U <Username> -P <password> -q -c -t ','
```
U kunt controleren of dat de gegevens correct zijn geëxporteerd door het nieuwe bestand te openen. De gegevens in het bestand moet overeenkomen met de onderstaande tekst:

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

>[AZURE.NOTE] Vanwege de aard van gedistribueerde systemen, de volgorde van de mogelijk niet hetzelfde in het datawarehouse SQL-databases. Een andere mogelijkheid is om de functie **queryout** van bcp aan het schrijven van een uittreksel van de query in plaats van de hele tabel exporteren.

## <a name="next-steps"></a>Volgende stappen
Zie voor een overzicht van de geladen [gegevens laden in SQL Data Warehouse][].
Zie [SQL Data Warehouse ontwikkelen-overzicht][]voor meer tips voor ontwikkeling.

<!--Image references-->

<!--Article references-->

[Laden van gegevens in SQL Data Warehouse]: ./sql-data-warehouse-overview-load.md
[SQL Data Warehouse ontwikkelen-overzicht]: ./sql-data-warehouse-overview-develop.md
[Tabel-overzicht]: ./sql-data-warehouse-tables-overview.md
[Statistieken]: ./sql-data-warehouse-tables-statistics.md

<!--MSDN references-->
[BCP]: https://msdn.microsoft.com/library/ms162802.aspx
[De syntaxis CREATE TABLE]: https://msdn.microsoft.com/library/mt203953.aspx

<!--Other Web references-->
[Microsoft Downloadcentrum]: https://www.microsoft.com/download/details.aspx?id=36433
