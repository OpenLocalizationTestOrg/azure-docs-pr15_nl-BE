<properties
   pageTitle="Magazijn van SQL-gegevens terugzetten | Microsoft Azure"
   description="Overzicht van de opties voor database terugzetten voor het terugzetten van een database in Azure SQL Data Warehouse."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="Lakshmi1812"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/29/2016"
   ms.author="lakshmir;barbkess;sonyama"/>


# <a name="sql-data-warehouse-restore"></a>Magazijn van SQL-gegevens terugzetten

> [AZURE.SELECTOR]
- [Overzicht][]
- [Portal][]
- [PowerShell][]
- [REST][]

SQL Data Warehouse biedt herstelt de lokale en geografische als onderdeel van de gegevens magazijn disaster recovery mogelijkheden. Magazijn back-ups van gegevens gebruiken voor het herstellen van uw gegevensmagazijn naar een herstelpunt in de regio voor primaire of geo-redundante back-ups gebruiken om een andere geografische regio te herstellen. In dit artikel wordt uitgelegd dat de specifieke kenmerken van het terugzetten van een datawarehouse.

## <a name="what-is-a-data-warehouse-restore"></a>Wat is een magazijn gegevens terugzetten?

Een magazijn gegevens terugzetten is een nieuw magazijn die is gemaakt op basis van een back-up van een bestaand of verwijderde gegevensmagazijn. Het magazijn van de teruggezette gegevens wordt opnieuw gemaakt het magazijn back-ups van gegevens op een bepaald tijdstip. Aangezien SQL Data Warehouse een gedistribueerd systeem is, wordt een magazijn gegevens terugzetten vanuit veel back-bestanden die zijn opgeslagen in Azure BLOB's gemaakt. 

Database terugzetten is een essentieel onderdeel van de continuïteit en noodherstel herstel bedrijfsstrategie omdat uw gegevens opnieuw worden gemaakt na een ongeluk beschadigd of verwijderd.

Zie voor meer informatie:

-  [Back-ups van SQL Data Warehouse](sql-data-warehouse-backups.md)
-  [Business continuity-overzicht](../sql-database/sql-database-business-continuity.md)

## <a name="data-warehouse-restore-points"></a>Gegevens magazijn herstelpunten

Een voordeel van het gebruik van Azure Premium opslag gebruikt SQL Data Warehouse Azure opslag Blob momentopnamen back-up van het primaire datawarehouse. Een momentopname is een herstelpunt dat is de tijd dat de momentopname wordt gestart. Als u met het herstellen van een datawarehouse, kies een herstelpunt en een restore-opdracht.  

SQL Data Warehouse herstelt altijd u de back-up naar een gegevensmagazijn met nieuwe. U kunt houden van het magazijn van de teruggezette gegevens en de huidige of een van deze te verwijderen. Als u wilt dat de huidige datawarehouse vervangen door het magazijn van de teruggezette gegevens, kunt u de naam wijzigen.

Als u nodig hebt om te herstellen van een datawarehouse verwijderd of is onderbroken, kunt u [een support ticket maken](sql-data-warehouse-get-started-create-support-ticket.md). 

<!-- 
### Can I restore a deleted data warehouse?

Yes, you can restore the last available restore point.

Yes, for the next seven calendar days. When you delete a data warehouse, SQL Data Warehouse actually keeps the data warehouse and its snapshots for seven days just in case you need the data. After seven days, you won't be able to restore to any of the restore points. -->

## <a name="geo-redundant-restore"></a>Geo-redundante terugzetten

Als u de geo-redundante opslag gebruikt, kunt u het datawarehouse terugzetten naar uw [Datacenter paarsgewijs](../best-practices-availability-paired-regions.md) in een andere geografische regio. Het datawarehouse wordt van de laatste dagelijkse back-up teruggezet. 

## <a name="restore-timeline"></a>Tijdlijn herstellen

U kunt een database terugzetten naar elk gewenst herstelpunt beschikbaar gedurende de afgelopen zeven dagen. Momentopnamen start elke vier tot acht uur en zijn beschikbaar voor zeven dagen. Wanneer u een momentopname is ouder dan zeven dagen, deze verloopt en het herstelpunt is niet langer beschikbaar.

## <a name="restore-costs"></a>Kosten herstellen

De kosten van opslag voor het magazijn van de teruggezette gegevens wordt gefactureerd tegen een tarief Azure Premium opslag. 

Als u een teruggezette gegevensmagazijn onderbreekt, worden in rekening gebracht voor opslag Azure Premium opslag snelheid. Het voordeel van onderbreken is dat u niet voor de computerbronnen voor DWU worden aangerekend.

Voor meer informatie over SQL Data Warehouse prijzen Zie [Prijzen van SQL gegevens magazijn](https://azure.microsoft.com/pricing/details/sql-data-warehouse/).

## <a name="uses-for-restore"></a>Wordt gebruikt voor het terugzetten van

Het primaire gebruik voor het terugzetten van gegevens magazijn is om gegevens te herstellen na per ongeluk gegevens verloren gaan of beschadigd raken.

Ook kunt u gegevens magazijn, terugzetten naar een back-up voor meer dan zeven dagen bewaren. Wanneer de back-up is teruggezet, kunt u het datawarehouse on line zijn en kunt onderbreken voor onbepaalde tijd berekenen om kosten te besparen. De onderbroken database systeem toeslagen snelheid Azure Premium opslag opslag. 

## <a name="related-topics"></a>Verwante onderwerpen

### <a name="scenarios"></a>Scenario 's

- Zie voor een overzicht van business continuity, [Business continuity-overzicht](../sql-database/sql-database-business-continuity.md)


<!-- ### Tasks -->

Voor het uitvoeren van een magazijn gegevens terugzetten herstellen met behulp van:

- Azure portal, Zie [herstellen van een datawarehouse met de Azure portal](sql-data-warehouse-restore-database-portal.md)
- PowerShell-cmdlets Zie [herstellen van een datawarehouse met PowerShell-cmdlets](sql-data-warehouse-restore-database-powershell.md)
- API's PLAATST, Zie [herstellen van een datawarehouse met REST API 's](sql-data-warehouse-restore-database-rest-api.md)

<!-- ### Tutorials -->

<!--Image references-->

<!--Article references-->
[Azure SQL Database business continuity overview]: ../sql-database/sql-database-business-continuity.md
[Overzicht]: ./sql-data-warehouse-restore-database-overview.md
[Portal]: ./sql-data-warehouse-restore-database-portal.md
[PowerShell]: ./sql-data-warehouse-restore-database-powershell.md
[REST]: ./sql-data-warehouse-restore-database-rest-api.md

<!--MSDN references-->


<!--Other Web references-->
