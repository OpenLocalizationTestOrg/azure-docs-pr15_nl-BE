<properties
   pageTitle="Voorbeeldgegevens in SQL Data Warehouse laden | Microsoft Azure"
   description="Voorbeeldgegevens in SQL Data Warehouse laden"
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
   ms.date="08/16/2016"
   ms.author="lodipalm;barbkess;sonyama"/>

#<a name="load-sample-data-into-sql-data-warehouse"></a>Voorbeeldgegevens in SQL Data Warehouse laden

Deze eenvoudige stappen om te laden en het opvragen van de voorbeelddatabase Adventure Works. Deze scripts eerst via sqlcmd SQL die tabellen weergaven en worden uitgevoerd. Zodra tabellen zijn gemaakt, worden de scripts bcp gebruiken om gegevens te laden.  Als u nog niet sqlcmd en bcp geïnstalleerd, klik op deze koppelingen [bcp][] te installeren en te [installeren van sqlcmd][].

##<a name="load-sample-data"></a>Voorbeeldgegevens worden geladen

1. Download het zip-bestand van [Adventure Works Sample Scripts for SQL Data Warehouse][] .

2. Pak de bestanden uit het gedownloade zip naar een map op uw lokale computer.

3. Bewerk het uitgepakte bestand aw_create.bat en stelt u de volgende variabelen aan het begin van het bestand is gevonden.  Zorg ervoor dat u laat geen spaties tussen de '=' en de parameter.  Hieronder vindt u voorbeelden van hoe uw bewerkingen uitzien.

    ```
    server=mylogicalserver.database.windows.net
    user=mydwuser
    password=Mydwpassw0rd
    database=mydwdatabase
    ```

4. Uitvoeren vanaf een Windows cmd-prompt de bewerkte aw_create.bat.  Zorg ervoor dat u in de map waar u de bewerkte versie van aw_create.bat opgeslagen.
Dit script wordt...
    * Adventure Works tabellen of weergaven die zich al in uw database weghalen
    * Adventure Works tabellen en weergaven maken
    * Elke Adventure Works tabel bcp met laden
    * Valideren van de aantallen rijen voor elke tabel Adventure Works
    * Verzamelen van statistische gegevens op voor elke kolom voor elke tabel Adventure Works


##<a name="query-sample-data"></a>Query-voorbeeldgegevens

Als u voorbeeldgegevens in uw magazijn SQL-gegevens hebt geladen, kunt u snel een paar query's uitvoeren.  Als u een query uitvoert, verbinding maken met de zojuist gemaakte database voor Adventure Works in Azure SQL DW met behulp van Visual Studio en SSDT, zoals beschreven in het document [opvragen met Visual Studio][] .

Voorbeeld van een eenvoudige select-instructie om alle informatie van de werknemers:

```sql
SELECT * FROM DimEmployee;
```

Voorbeeld van een complexere query met constructies zoals groep door te kijken naar het totale bedrag van alle verkopen op elke dag:

```sql
SELECT OrderDateKey, SUM(SalesAmount) AS TotalSales
FROM FactInternetSales
GROUP BY OrderDateKey
ORDER BY OrderDateKey;
```

Voorbeeld van een selectie met een WHERE-component om orders van een filter voor een bepaalde datum:

```
SELECT OrderDateKey, SUM(SalesAmount) AS TotalSales
FROM FactInternetSales
WHERE OrderDateKey > '20020801'
GROUP BY OrderDateKey
ORDER BY OrderDateKey;
```

SQL Data Warehouse ondersteunt bijna alle T-SQL-constructies die SQL Server ondersteunt.  Eventuele verschillen worden beschreven in onze documentatie [code migreren][] .

## <a name="next-steps"></a>Volgende stappen
Nu dat u enkele query's met voorbeeldgegevens uitproberen hebt, controleert u hoe te [ontwikkelen][], te [laden][]of te [migreren][] naar SQL Data Warehouse.

<!--Image references-->

<!--Article references-->
[migreren]: sql-data-warehouse-overview-migrate.md
[ontwikkelen]: sql-data-warehouse-overview-develop.md
[laden]: sql-data-warehouse-overview-load.md
[query met Visual Studio]: sql-data-warehouse-query-visual-studio.md
[code migreren]: sql-data-warehouse-migrate-code.md
[bcp installeren]: sql-data-warehouse-load-with-bcp.md
[sqlcmd installeren]: sql-data-warehouse-get-started-connect-sqlcmd.md

<!--Other Web references-->
[Adventure Works voorbeeldscripts voor SQL datawarehouse]: https://migrhoststorage.blob.core.windows.net/sqldwsample/AdventureWorksSQLDW2012.zip
