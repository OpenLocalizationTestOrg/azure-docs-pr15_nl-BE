<properties
   pageTitle="Uw oplossing migreren naar SQL Data Warehouse | Microsoft Azure"
   description="Instructies voor migratie van uw oplossing te brengen Azure SQL Data Warehouse-platform."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="barbkess"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/30/2016"
   ms.author="barbkess;jrj;sonyama"/>

# <a name="migrate-your-solution-to-sql-data-warehouse"></a>Uw oplossing migreren naar SQL Data Warehouse

SQL Data Warehouse is een gedistribueerd databasesysteem die elastically wordt aangepast aan uw behoeften. Zodat zowel prestaties als de schaal zijn niet alle functies binnen SQL Server geïmplementeerd binnen SQL Data Warehouse. De volgende onderwerpen voor migratie raakt op sommige van de belangrijkste factoren bij de migratie van uw oplossing naar SQL Data Warehouse. Ontwerpen van datawarehouses voor schaal introduceert ander ontwerp patronen en dus de traditionele methoden zijn niet altijd de beste. Daarom kan het gebeuren dat aanpassing van de bestaande oplossing waarborgt dat u volledig profiteren van de gedistribueerde platform geleverd door SQL Data Warehouse.

Het is ook belangrijk te weten dat SQL Data Warehouse een platform op basis van Microsoft Azure is. Onderdeel van de migratie bevatten daarom ook uw gegevens worden overgebracht naar de cloud. Overdracht van gegevens is een onderwerp op zichzelf en moet zorgvuldig worden overwogen; vooral als de toename van volumes. Overdracht van gegevens en gegevens laden zijn afzonderlijke onderwerpen.

## <a name="migration-guidance"></a>Richtlijnen voor migratie

Zorg ervoor dat u via deze artikelen, zodat u bepaalde product verschillen en fundamentele begrippen begrijpen voordat u de migratie hebt gelezen.

- [Uw schema migreren][]
- [Uw gegevens migreren][]
- [Uw code migreren][]

## <a name="next-steps"></a>Volgende stappen

De kat (Customer Advisory Team) heeft ook enkele grote SQL Data Warehouse richtsnoeren die ze via blogs publiceren.  Bekijk de [gegevens migreren naar het datawarehouse van Azure SQL in de praktijk][] voor extra ondersteuning voor migratie-artikel.

<!--Image references-->

<!--Article references-->
[Uw schema migreren]: sql-data-warehouse-migrate-schema.md
[Uw gegevens migreren]: sql-data-warehouse-migrate-data.md
[Uw code migreren]: sql-data-warehouse-migrate-code.md


<!--MSDN references-->


<!--Other Web references-->
[Gegevens migreren naar Azure SQL Data Warehouse in de praktijk]: https://blogs.msdn.microsoft.com/sqlcat/2016/08/18/migrating-data-to-azure-sql-data-warehouse-in-practice/
