<properties
    pageTitle="SQL-Database elastische groep prijs- prestatieverhouding"
    description="Prijsinformatie voor elastische database van toepassingen."
    services="sql-database"
    documentationCenter=""
    authors="srinia"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="05/27/2016"
    ms.author="srinia"
    ms.workload="data-management"
    ms.topic="article"
    ms.tgt_pltfrm="NA"/>


# <a name="elastic-database-pool-billing-and-pricing-information"></a>Elastische database toepassingen facturerings- en prijsinformatie

Elastische database toepassingen worden gefactureerd per de volgende kenmerken:

- Een elastische toepassingen wordt gefactureerd bij het maken, zelfs als er geen databases in de groep zijn.
- Een elastische toepassingen wordt per uur gefactureerd. Dit is dezelfde meting frequentie als voor de prestaties van afzonderlijke databases.
- Als u het formaat van een elastische toepassingen naar een nieuwe hoeveelheid eDTUs vervolgens de toepassingen niet gefactureerd aan de nieuwe hoeveelheid eDTUS totdat de formaat-bewerking is voltooid. Dit komt overeen met hetzelfde patroon als het prestatieniveau van zelfstandige databases wijzigen.


- De prijs van een elastische toepassingen is gebaseerd op het aantal eDTUs van de groep. De prijs van een elastische toepassingen is afhankelijk van het aantal en het gebruik van de elastische databases in het.
- Prijs wordt berekend door (het aantal eDTUs van toepassingen) x (eenheidsprijs per eDTU).

Eenheidsprijs voor een groep met elastische eDTU is hoger dan de prijs per eenheid DTU voor een afzonderlijke database in de servicelaag met dezelfde. Zie voor details de [prijzen van de SQL-Database](https://azure.microsoft.com/pricing/details/sql-database/). 


Zie [Opties voor SQL-Database en prestaties](sql-database-service-tiers.md)voor de eDTUs en service niveaus.

## <a name="next-steps"></a>Volgende stappen

- [Maak een elastische database toepassingen](sql-database-elastic-pool-create-portal.md)
- [Bewaken, beheren en het formaat van een elastische database toepassingen](sql-database-elastic-pool-manage-portal.md)
- [SQL-Database-opties en prestaties: begrijpen wat beschikbaar is in elke servicelaag](sql-database-service-tiers.md)
- [PowerShell script voor het identificeren van databases die geschikt zijn voor een elastische database toepassingen](sql-database-elastic-pool-database-assessment-powershell.md)
