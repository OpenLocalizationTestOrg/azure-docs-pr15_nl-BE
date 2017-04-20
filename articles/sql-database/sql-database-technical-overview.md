<properties
    pageTitle="Wat is een SQL-Database? Inleiding tot SQL-Database | Microsoft Azure"
    description="Kennismaking met de SQL-Database: technische details en mogelijkheden van Microsoft relationele database managementsysteem (RDBMS) in de cloud."
    keywords="Inleiding tot sql-Inleiding tot sql, wat is sql-database"
    services="sql-database"
    documentationCenter=""
    authors="shontnew"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management"
   ms.date="08/16/2016"
   ms.author="shkurhek"/>

# <a name="what-is-sql-database-introduction-to-sql-database"></a>Wat is een SQL-Database? Inleiding tot SQL-Database

SQL-Database is een relationele databaseservice in de cloud op basis van de toonaangevende Microsoft SQL Server-engine, met essentiële mogelijkheden. SQL-Database biedt voorspelbare prestaties, schaalbaarheid zonder uitvaltijd, bedrijfscontinuïteit en gegevensbescherming — allemaal met beheer in de buurt van nul. U kunt zich concentreren op de ontwikkeling van snelle Apps en uw tijd op de markt te versnellen in plaats van beheer van virtuele machines en infrastructuur. Omdat het gebaseerd op de [SQL Server](https://msdn.microsoft.com/library/bb545450.aspx) -engine, ondersteunt SQL-Database bestaande SQL Server-hulpprogramma's, bibliotheken en API's, waardoor het makkelijker om te verplaatsen en uitbreiden naar de cloud.

Dit artikel bevat een inleiding tot SQL-Database core concepten en functies met betrekking tot prestaties, schaalbaarheid en beheerbaarheid, met koppelingen naar de details te bekijken. Bent u klaar om te springen, kunt u [uw eerste SQL-database maken](sql-database-get-started.md) of [een elastische database-toepassingen maken](sql-database-elastic-pool-create-portal.md) in minuten. Als u een diepere Kennismaking wilt, bekijk deze video van 30 minuten.

> [AZURE.VIDEO azurecon-2015-get-started-with-azure-sql-database]

## <a name="adjust-performance-and-scale-without-downtime"></a>Prestaties en de schaal zonder uitvaltijd aanpassen

SQL-databases is beschikbaar in de Basic, Standard en Premium *service tiers*. Elke servicelaag biedt [verschillende niveaus van prestaties en mogelijkheden](sql-database-service-tiers.md) ter ondersteuning van lightweight naar heavyweight database werkbelasting. U kunt bouwen uw eerste app op een kleine database voor een paar tegoed per maand, vervolgens [de servicelaag wijzigen](sql-database-scale-up.md) handmatig of via programmering op elk gewenst moment als uw app virale wereldwijd, zonder uitvaltijd naar uw app of voor uw klanten gaat.

Voor veel bedrijven en apps is te kunnen maken van databases en prestaties van de database voor eenmalige omhoog of omlaag bellen op verzoek voldoende, vooral als gebruikspatronen relatief voorspelbaar zijn. Maar als er onvoorspelbaar gebruikspatronen, deze kunt u vaste kosten en het bedrijfsmodel van uw te beheren.

Dit probleem wordt opgelost door [elastische van toepassingen](sql-database-elastic-pool.md) in de SQL-Database. Het concept is eenvoudig. U prestaties aan een groep toewijzen en betalen voor de collectieve prestaties van de groep in plaats van één databaseprestaties. U hoeft niet te kiezen databaseprestaties omhoog of omlaag. De databases in de groep, de zogenaamde *elastische databases*, schaal automatisch omhoog en naar ontmoeten vraag. Elastische databases verbruiken, maar niet groter zijn dan de grenzen van de groep van toepassingen, zodat de kosten voorspelbare blijft, zelfs als geen gebruik van de database. Sterker nog, kunt u [toevoegen en verwijderen van databases naar de groep](sql-database-elastic-pool-manage-portal.md), schalen van uw app uit een handvol databases naar duizenden alle binnen een budget waarmee u. Zie voor meer informatie over ontwerppatronen voor SaaS-toepassingen met elastische van toepassingen, [Patronen ontwerpen voor SaaS-toepassingen met meerdere huurder met Azure SQL-Database](sql-database-design-patterns-multi-tenancy-saas-applications.md).

In beide gevallen u gaat — single- of elastische, u niet op de plek. U kunt enkele databases met elastische database toepassingen laten overvloeien en de Servicelagen enkele databases en innovatieve ontwerpen maken van toepassingen wijzigen. Bovendien kunt u met de kracht en het bereik van Azure mix-and-match Azure services met SQL-Database aan uw unieke moderne app ontwerp behoeften, kosten- en efficiëntie station en nieuwe zakelijke kansen te ontgrendelen.

Maar hoe kunt u de relatieve prestaties van databases en toepassingen van database vergelijken? Hoe weet u met de rechtermuisknop op stoppen wanneer u omhoog en omlaag kunt u bellen? Het antwoord is de Database transactie-eenheid (DTU) voor enkele databases en de elastische DTU (eDTU) voor elastische databases en toepassingen van de database. Zie [SQL-Database-opties en prestaties: begrijpen wat beschikbaar is in elke servicelaag](sql-database-service-tiers.md) voor meer informatie.

## <a name="keep-your-app-and-business-running"></a>Uw app en bedrijf draaiende houden

Azure van industrie toonaangevende 99,99% beschikbaarheid service level agreement [(SLA)](http://azure.microsoft.com/support/legal/sla/), aangedreven door een wereldwijd netwerk van Microsoft beheerde datacenters helpt uw app met 24 x 7. Met elke SQL-database, is u gebruikmaken van de ingebouwde gegevensbescherming, fouttolerantie en bescherming van de gegevens die u anders moet ontwerpen, kopen, bouwen en beheren. Afhankelijk van de behoeften van uw bedrijf kan u toch extra lagen van bescherming om dat uw app en uw bedrijf snel ongedaan kunnen in geval van een ramp, een fout of een andere vraag. Elke servicelaag biedt een ander menu functies die kunt u aan de slag en uitgevoerd en verblijf op die manier met de SQL-Database. Point-in-time terugzetten kunt u een database terug te keren naar een eerdere toestand, tot 35 dagen. Bovendien, als er een storing in het datacenter hosting van uw databases, kunt u replica's database in een andere regio. Of u replica's kunt gebruiken voor upgrades of verplaatsing van de verschillende regio's.

![SQL Database Geo-replicatie](./media/sql-database-technical-overview/azure_sqldb_map.png)


Zie [Bedrijfscontinuïteit](sql-database-business-continuity.md) voor meer informatie over de verschillende business continuity functies beschikbaar voor verschillende niveaus.

## <a name="secure-your-data"></a>Beveilig uw gegevens
SQL Server heeft een traditie van solide beveiliging dat SQL-Database met voorzieningen die de toegang beperken beschermt, gegevens beschermen en controleren van activiteit. Zie [de SQL-database beveiligen](sql-database-security.md) voor een snel overzicht van beveiligingsopties zijn er in de SQL-Database. Zie voor een meer uitgebreid overzicht van de beveiligingsfuncties van [Beveiligingscentrum voor SQL Server-Database-Engine en de SQL-Database](https://msdn.microsoft.com/library/bb510589) . En Ga naar het [Vertrouwenscentrum Azure](https://azure.microsoft.com/support/trust-center/security/) voor informatie over de beveiliging van de Azure platform.

## <a name="next-steps"></a>Volgende stappen
Hebt u een inleiding tot SQL-Database wordt gelezen en beantwoord de vraag "Wat is de SQL-Database?", bent u klaar om:

- Zie de [prijzen pagina](https://azure.microsoft.com/pricing/details/sql-database/) voor één database en elastische database vergelijking en rekenmachines.
- Meer informatie over [groepen van elastisch](sql-database-elastic-pool.md).
- Aan de slag met [uw eerste database maakt](sql-database-get-started.md).
- [Verbinding maken en een query met SSMS](sql-database-connect-query-ssms.md)
- Uw eerste app in C#, Java, Node.js, PHP, Python en Ruby bouwen: [bibliotheken verbinding voor SQL-Database en SQL Server](sql-database-libraries.md)
- Zie een index van de titels en beschrijvingen van [alle onderwerpen voor service Azure sql-database](sql-database-index-all-articles.md).
