<properties
    pageTitle="XTP geheugenopslag controleren | Microsoft Azure"
    description="Raming en XTP geheugenopslag monitor gebruikt, capaciteit; capaciteit fout 41823 oplossen"
    services="sql-database"
    documentationCenter=""
    authors="jodebrui"
    manager="jhubbard"
    editor=""/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="jodebrui"/>


# <a name="monitor-in-memory-oltp-storage"></a>Monitor In Memory OLTP-opslag

Wanneer u [In Memory OLTP](sql-database-in-memory.md), staat gegevens in tabellen geoptimaliseerd voor geheugen en de tabelvariabelen in In Memory OLTP-opslag. Elke laag Premium service heeft een maximale In Memory OLTP opslaggrootte, die wordt beschreven in het [artikel SQL Database Service Tiers](sql-database-service-tiers.md#service-tiers-for-single-databases). Zodra deze limiet wordt overschreden, invoegen en bijwerken bewerkingen mislukken (met fout 41823) wordt gestart. Op dat punt wordt moet op een gegevens om geheugen vrij te verwijderen, of de laag van de prestaties van uw database bijwerken.

## <a name="determine-whether-data-will-fit-within-the-in-memory-storage-cap"></a>Bepalen of gegevens in het GLB opslag in het geheugen past

Bepalen van de lampvoet opslag: Raadpleeg het [artikel SQL Database Service Tiers](sql-database-service-tiers.md#service-tiers-for-single-databases) voor caps van de opslag van de verschillende niveaus van Premium service.

Schatten van de geheugenvereisten voor een geheugen geoptimaliseerd tabel werkt hetzelfde voor SQL Server als deze is in Azure SQL-Database. Neem even de tijd om te controleren dat onderwerp op [MSDN](https://msdn.microsoft.com/library/dn282389.aspx).

Houd er rekening mee dat de tabel en variabele tabelrijen, evenals indexen, ten opzichte van de grootte van de maximale gebruiker gegevens tellen. ALTER TABLE moet bovendien voldoende ruimte voor het maken van een nieuwe versie van de volledige tabel en de bijbehorende indexen.

## <a name="monitoring-and-alerting"></a>Controle en signalering

U kunt gebruiken als een percentage van de [opslag voor uw prestaties laag cap](sql-database-service-tiers.md#service-tiers-for-single-databases) in Azure [portal](https://portal.azure.com/)opslag in het geheugen controleren: 

- Ga naar het vak bron gebruik op het blad Database en klik op bewerken.
- Selecteer de metric `In-Memory OLTP Storage percentage`.
- Als u wilt een waarschuwing toevoegen, schakel het gebruik van bronnen in de metrische blade openen en klik vervolgens op op waarschuwing toevoegen.

Of gebruik de volgende query weer te geven van het gebruik van de opslagruimte in het geheugen:

    SELECT xtp_storage_percent FROM sys.dm_db_resource_stats


## <a name="correct-out-of-memory-situations---error-41823"></a>-Geheugen situaties - fout 41823 corrigeren

Resultaten-geheugen in de INSERT, UPDATE en maken bewerkingen mislukken met het foutbericht 41823 wordt uitgevoerd.

Foutbericht 41823 geeft aan de tabellen geoptimaliseerd voor geheugen en de tabelvariabelen de maximale grootte hebt overschreden.

Fout op te lossen deze, hetzij:


- Gegevens verwijderen uit de tabellen geheugen geoptimaliseerd offloading mogelijk de gegevens aan de tabellen voor traditionele, op basis van een schijf. of,
- De opgegeven service upgraden naar een met voldoende opslagruimte in het geheugen voor de gegevens die u wilt bewaren in tabellen geheugen wordt geoptimaliseerd.

## <a name="next-steps"></a>Volgende stappen
Aanvullende bronnen over over [Azure SQL-Database controleren met behulp van beheer van dynamische weergaven](sql-database-monitoring-with-dmvs.md)
