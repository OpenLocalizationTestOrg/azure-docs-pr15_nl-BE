<properties
    pageTitle="SQL-Database: Wat is een DTU? | Microsoft Azure"
    description="Inzicht in wat een Azure SQL-Database is transactie-eenheid."
    keywords="databaseopties voor databaseprestaties"
    services="sql-database"
    documentationCenter=""
    authors="CarlRabeler"
    manager="jhubbard"
    editor="CarlRabeler"/>

<tags
    ms.service="sql-database"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="NA"
    ms.date="09/06/2016"
    ms.author="carlrab"/>

# <a name="explaining-database-transaction-units-dtus-and-elastic-database-transaction-units-edtus"></a>Uitleg over Database-transactie-eenheden (DTUs) en de elastische Database transactie-eenheden (eDTUs)

In dit artikel wordt uitgelegd Database transactie-eenheden (DTUs) en de elastische Database transactie-eenheden (eDTUs) en wat er gebeurt als u de maximale DTUs of eDTUs raakt.  

## <a name="what-are-database-transaction-units-dtus"></a>Wat zijn de Database-transactie-eenheden (DTUs)

Een DTU is een eenheid van de bronnen die beschikbaar zijn voor een zelfstandige Azure SQL-database op een specifieke prestatieniveau binnen een [zelfstandige service databaselaag](sql-database-service-tiers.md#standalone-database-service-tiers-and-performance-levels)zijn gegarandeerd. Een DTU is een gemengde eenheid van CPU, geheugen en I/O en transactie log i/o-bij een bepaald door een OLTP benchmark werkbelasting is ontworpen voor die kenmerkend zijn voor concrete OLTP workloads. De DTUs wordt verdubbeld door het verhogen van het prestatieniveau van een database is gelijk aan de set van de resource beschikbaar is voor die database wordt verdubbeld. Bijvoorbeeld, biedt een premie P11 database met 1750 DTUs 350 x DTU meer rekenkracht dan een eenvoudige database met 5 DTUs. De methodiek achter de OLTP-benchmark werkbelasting gebruikt om te bepalen van de overvloeiing DTU Zie [benchmark-overzicht SQL-Database](sql-database-benchmark-overview.md).

![Inleiding tot SQL-Database: één database DTUs door laag en niveau](./media/sql-database-what-is-a-dtu/single_db_dtus.png)

U kunt [wijzigen Servicelagen](sql-database-scale-up.md) op elk gewenst moment met een minimale uitval naar de toepassing (gemiddeld over het algemeen binnen vier seconden). Voor veel bedrijven en apps is te kunnen maken van databases en prestaties van de database voor eenmalige omhoog of omlaag bellen op verzoek voldoende, vooral als gebruikspatronen relatief voorspelbaar zijn. Maar als er onvoorspelbaar gebruikspatronen, deze kunt u vaste kosten en het bedrijfsmodel van uw te beheren. In dit scenario kunt u een elastische toepassingen gebruikt met een bepaald aantal eDTUs.

## <a name="what-are-elastic-database-transaction-units-edtus"></a>Wat zijn elastische Database transactie-eenheden (eDTUs)

Een eDTU is een eenheid van de reeks resources (DTUs) die kan worden gedeeld tussen de databases op een Azure SQL server - een [elastische groep](sql-database-elastic-pool.png)genoemd. Elastische toepassingen bieden een eenvoudige, rendabele oplossing voor het beheren van de prestatiedoelstellingen voor meerdere databases die sterk variëren en onvoorspelbare gebruikspatronen. Zie [elastische groepen en Servicelagen](sql-database-service-tiers.md#elastic-pool-service-tiers-and-performance-in-edtus) voor meer informatie.

![Inleiding tot SQL-Database: eDTUs door laag en niveau](./media/sql-database-what-is-a-dtu/sqldb_elastic_pools.png)

Een groep krijgt een bepaald aantal eDTUs voor een vaste prijs. Binnen de groep van toepassingen, afzonderlijke databases, krijgen de flexibiliteit auto-schaal binnen de ingestelde parameters. Bij zware belasting kan een database verbruiken meer eDTUs om te voldoen aan de vraag. Databases onder lichte lading minder verbruiken en databases geen belasting verbruiken geen eDTUs. Inrichten van resources voor de hele groep dan voor enkele databases vereenvoudigt uw taken. Bovendien hebt u een voorspelbaar budget voor de groep.

Extra eDTUs kunnen worden toegevoegd aan een bestaande toepassingen met geen downtime database of geen gevolgen voor de databases in de groep met elastische. Op dezelfde manier als extra eDTUs zijn niet langer nodig kunnen ze worden verwijderd uit een bestaande groep op elk punt in de tijd. U kunt optellen of aftrekken van databases naar de groep. Als een database is voorspelbaar onder-gebruik van bronnen, het verplaatsen van.

## <a name="how-can-i-determine-the-number-of-dtus-needed-by-my-workload"></a>Hoe kan ik vaststellen dat het aantal DTUs dat nodig is voor de werkbelasting?

Als u op zoek bent om te migreren van een bestaand op-lokalen of SQL Server virtuele machine werkdruk Azure SQL-Database, kunt u de [DTU Calculator](http://dtucalculator.azurewebsites.net/) bij benadering het aantal DTUs dat nodig is. Voor een bestaande werklast Azure SQL-Database, kunt u [SQL Database Query prestaties inzicht](sql-database-query-performance.md) inzicht in verbruik (DTUs) om meer inzicht in het optimaliseren van uw werkbelasting van uw database. U kunt ook [sys.dm_db_ resource_stats](https://msdn.microsoft.com/library/dn800981.aspx) DMV gebruiken om de gegevens voor het laatste één uur verbruik. Ook kan de catalogus weergeven [sys.resource_stats](http://msdn.microsoft.com/library/dn269979.aspx) ook worden doorzocht om dezelfde gegevens voor de laatste 14 dagen, maar bij een lagere kwaliteit van vijf-minuten-gemiddelden.

## <a name="how-do-i-know-if-i-could-benefit-from-an-elastic-pool-of-resources"></a>Hoe weet ik als ik van een elastische toepassingen van bronnen profiteren kan

Groepen zijn geschikt voor een groot aantal databases met gebruik van specifieke patronen. Dit patroon is voor een bepaalde database gekenmerkt door lage Gemiddeld gebruik met spikes relatief frequent gebruik. SQL-Database automatisch resulteert het gebruik van historische bronnen in een bestaande Database van SQL server-databases en raadt aan om de configuratie van de juiste groep in Azure portal. Zie voor meer informatie [Wanneer moet een elastische database-toepassingen worden gebruikt?](sql-database-elastic-pool-guidance.md)

## <a name="what-happens-when-i-hit-my-maximum-dtus"></a>Wat gebeurt er als ik mijn maximum DTUs bereikt

Prestaties zijn gekalibreerd en geregeld om de benodigde resources als u wilt de werkbelasting van de database de maximale toegestane toegestaan voor de geselecteerde service tier-prestatie-niveau uitvoeren. Als uw werkbelasting in één i/o/Log i/o CPU/gegevens van de limieten te raken is, blijft de bronnen aan het maximaal toegestane niveau, maar u ziet waarschijnlijk betere vertragingstijden voor uw vragen. Deze limieten resulteren niet in fouten, maar in plaats daarvan een vertraging van de werklast, tenzij de vertraging zo ernstig wordt dat query's timing beginnen. Als u zijn kunt u door de grenzen van de maximale toegestane, gelijktijdige gebruiker sessies aanvragen (worker-threads), ziet u expliciete fouten. Zie [resource limieten van Azure SQL-Database](sql-database-resource-limits.md) voor informatie over het beperken van bronnen buiten de CPU, geheugen, I/O gegevens en i/o-transactie-log.

## <a name="next-steps"></a>Volgende stappen

- Zie [Service tier](sql-database-service-tiers.md) voor meer informatie over de DTUs en eDTUs beschikbaar voor zelfstandige databases en voor groepen van elastisch.
- Zie [resource limieten van Azure SQL-Database](sql-database-resource-limits.md) voor informatie over het beperken van bronnen buiten de CPU, geheugen, I/O gegevens en i/o-transactie-log.
- Zie [SQL Database Query prestaties inzicht](sql-database-query-performance.md) te begrijpen uw verbruik (DTUs).
- Zie [overzicht van de SQL-Database-benchmark](sql-database-benchmark-overview.md) te begrijpen van de methodologie van het achter de OLTP-benchmark werkbelasting gebruikt om te bepalen van de overvloeiing DTU.