<properties
   pageTitle="Back-ups van SQL Data Warehouse | Microsoft Azure"
   description="Meer informatie over SQL Data Warehouse ingebouwde back-ups voor een magazijn Azure SQL-gegevens terugzetten naar een herstelpunt of een andere geografische regio."
   services="sql-data-warehouse"
   documentationCenter=""
   authors="lakshmi1812"
   manager="barbkess"
   editor="monicar"/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/06/2016"
   ms.author="lakshmir;barbkess"/>

# <a name="sql-data-warehouse-backups"></a>Back-ups van SQL Data Warehouse

SQL Data Warehouse biedt lokale en geografische back-ups als onderdeel van de back-up gegevens magazijn-mogelijkheden. Deze omvatten Azure opslag Blob momentopnamen en geo-redundante opslag. Magazijn back-ups van gegevens gebruiken om uw gegevensmagazijn herstellen naar een herstelpunt in de regio voor primaire of terugzetten naar een andere geografische regio. In dit artikel wordt uitgelegd dat de specifieke kenmerken van reservekopieën in SQL Data Warehouse.

## <a name="what-is-a-data-warehouse-backup"></a>Wat is een back-up van gegevens magazijn?

Een back-up van gegevens magazijn zijn de gegevens die u gebruiken kunt om te zetten een datawarehouse op een bepaald tijdstip.  Aangezien SQL Data Warehouse een gedistribueerd systeem is, wordt een back-up van gegevens magazijn bestaat uit veel bestanden die zijn opgeslagen in Azure BLOB's. 

Back-ups zijn een essentieel onderdeel van de continuïteit en noodherstel herstel bedrijfsstrategie omdat ze voorkomen dat gegevens per ongeluk beschadigd of verwijderd. Zie [Business continuity-overzicht](../sql-database/sql-database-business-continuity.md)voor meer informatie.

## <a name="data-redundancy"></a>Gegevensredundantie

SQL Data Warehouse beschermt uw gegevens door het opslaan van gegevens in een lokaal redundante (LRS) Azure Premium opslag. Deze functie Azure opslag slaat synchrone kopieën van de gegevens in het midden van de lokale gegevens transparant gegevensbescherming garanderen als gelokaliseerde mislukken. Gegevensredundantie zorgt ervoor dat uw gegevens problemen zoals schijffouten infrastructuur kunnen overleven. Gegevensredundantie garandeert bedrijfscontinuïteit met een fouttolerant en infrastructuur voor hoge beschikbaarheid.

Voor meer informatie over:

- Azure Premium opslag, Zie [Introduction to Azure Premium opslag](../storage/storage-premium-storage.md).
- Lokaal redundante opslag, Zie [replicatie Azure opslag](../storage/storage-redundancy.md#locally-redundant-storage).


## <a name="azure-storage-blob-snapshots"></a>Azure Blob Storage momentopnamen

Een voordeel van het gebruik van Azure Premium opslag gebruikt SQL Data Warehouse Azure opslag Blob momentopnamen back-up van het datawarehouse lokaal. U kunt een datawarehouse herstellen naar een herstelpunt momentopname. Momentopnamen minimaal elke acht uur starten en zijn beschikbaar voor zeven dagen.  

Voor meer informatie over:

- Azure blob snapshots, Zie [maken een momentopname van een blob](../storage/storage-blob-snapshots.md).


## <a name="geo-redundant-backups"></a>Geo-redundante back-ups

Elke 24 uur SQL Data Warehouse opgeslagen in de standaard opslag volledige datawarehouse. Het volledige datawarehouse wordt aan het tijdstip van de laatste momentopname gemaakt. De standaard opslag hoort bij een geo-redundante opslag-account met de machtiging lezen (RA-GRS). De functie Azure opslag RA-GRS repliceert de back-upbestanden naar een [gepaarde Datacenter](../best-practices-availability-paired-regions.md). Deze geo-replicatie zorgt dat u datawarehouse kunt herstellen in het geval u geen toegang de momentopnamen in uw primaire regio tot. 

Deze functie is standaard ingeschakeld. Als u niet dat geo-redundante back-ups gebruikt wilt, kunt u opt-out. 

>[AZURE.NOTE] In Azure opslag, wordt de term *replicatie* verwijst naar het kopiëren van bestanden van de ene locatie naar de andere. De SQL- *databasereplicatie* verwijst naar tot meerdere secundaire databases worden gesynchroniseerd met een primaire database. 

Voor meer informatie over:
- Geo-redundante opslag, Zie [replicatie Azure opslag](../storage/storage-redundancy.md).
- RA GRS opslag, Zie [leestoegang geo-redundante opslag](../storage/storage-redundancy.md#read-access-geo-redundant-storage).

## <a name="data-warehouse-backup-schedule-and-retention-period"></a>Back-upschema en de bewaarperiode datawarehouse

SQL Data Warehouse momentopnamen maakt op uw online datawarehouses elke vier tot acht uur en elke momentopname blijft gedurende zeven dagen. In de afgelopen zeven dagen kunt u uw on line database terugzetten op een van de herstelpunten. 

Om te zien wanneer de laatste momentopname wordt gestart, moet u deze query uitvoeren op uw on line SQL Data Warehouse. 

```sql
select top 1 *
from sys.pdw_loader_backup_runs 
order by run_id desc;
```

Als u een momentopname voor meer dan zeven dagen behouden, kunt u een herstelpunt herstellen naar een gegevensmagazijn met nieuwe. Nadat het terugzetten is voltooid, start SQL Data Warehouse momentopnamen maken op het nieuwe gegevenswarehouse. Als u geen wijzigingen in het nieuwe gegevenswarehouse aanbrengen, blijft leeg, de momentopnamen en derhalve de kosten voor de momentopname is minimaal. U kan ook de database om te voorkomen dat SQL Data Warehouse maken van momentopnamen onderbreken.


### <a name="what-happens-to-my-backup-retention-while-my-data-warehouse-is-paused"></a>Wat gebeurt er met mijn back-up bewaren terwijl Mijn datawarehouse is onderbroken?

SQL Data Warehouse maakt geen momentopnamen en momentopnamen niet verloopt terwijl een datawarehouse is onderbroken. De momentopname leeftijd verandert niet terwijl het datawarehouse is onderbroken. Behoud van de momentopname is gebaseerd op het aantal dagen dat het datawarehouse on line en geen kalenderdagen is.

Als een momentopname vanaf 1 oktober 16: 00 en het datawarehouse om 4 uur op 3 oktober is onderbroken, wordt de momentopname van het twee dagen oud. Wanneer het datawarehouse weer on line komt is de momentopname twee dagen oud. Als het datawarehouse on line op 5 oktober om 16: 00 komt, de momentopname twee dagen oud en blijft gedurende vijf dagen.

Wanneer het datawarehouse weer on line komt, wordt SQL Data Warehouse nieuwe momentopnamen wordt gehaald en momentopnamen verloopt als ze meer dan zeven dagen van de gegevens hebben.

### <a name="how-long-is-the-retention-period-for-a-dropped-data-warehouse"></a>Hoe lang is de bewaartermijn voor een entrepot van verloren gegane gegevens?
Wanneer een datawarehouse wordt neergezet, zijn het datawarehouse en de momentopnamen opgeslagen gedurende zeven dagen en vervolgens verwijderd. Een van de opgeslagen herstelpunten kunt u het datawarehouse herstellen.

> [AZURE.IMPORTANT] Als u een logische SQL server-exemplaar verwijdert, worden alle databases die deel uitmaken van het exemplaar worden ook verwijderd en kunnen niet worden hersteld. U kunt een verwijderde server niet herstellen.

## <a name="data-warehouse-backup-costs"></a>Back-up gegevens magazijn-kosten

De totale kosten voor uw primaire datawarehouse en zeven dagen van Azure Blob momentopnamen wordt afgerond op de dichtstbijzijnde TB. Bijvoorbeeld, als uw datawarehouse 1,5 TB is en de momentopnamen 100 GB gebruiken, u zijn een rekening voor 2 TB aan gegevens op Azure Premium-opslagruimte rentetarieven. 

>[AZURE.NOTE] Een momentopname is aanvankelijk leeg en wordt groter als u wijzigingen in het magazijn van de primaire gegevens aanbrengt. Alle momentopnamen toeneemt in omvang als wijzigingen in het magazijn. De opslagkosten voor momentopnamen groeien dus de snelheid van de wijziging.

Als u geo-redundante opslag gebruikt, ontvangt u een aparte opslag gratis. De geo-redundante opslag wordt gefactureerd tegen het standaardtarief leestoegang tot geografisch redundante opslag (RA-GRS).

Voor meer informatie over SQL Data Warehouse prijzen Zie [Prijzen van SQL gegevens magazijn](https://azure.microsoft.com/pricing/details/sql-data-warehouse/).

## <a name="using-database-backups"></a>Met behulp van back-ups

Het datawarehouse terugzetten op een van de herstelpunten binnen de bewaartermijn wordt de primaire gebruikt voor back-ups van SQL gegevens magazijn.  

- Als u een SQL-datawarehouse herstellen, Zie [een magazijn SQL-gegevens terugzetten](sql-data-warehouse-restore-database-overview.md).


## <a name="related-topics"></a>Verwante onderwerpen

### <a name="scenarios"></a>Scenario 's

- Zie voor een overzicht van business continuity, [Business continuity-overzicht](../sql-database/sql-database-business-continuity.md)


<!-- ### Tasks -->

- Om te zetten in een datawarehouse, Zie [een magazijn SQL-gegevens terugzetten](sql-data-warehouse-restore-database-overview.md)

<!-- ### Tutorials -->

