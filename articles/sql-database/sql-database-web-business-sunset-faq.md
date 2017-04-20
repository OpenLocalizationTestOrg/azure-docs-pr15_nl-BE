<properties
   pageTitle="Azure SQL Database Web- en Business Edition zonsondergang Veelgestelde vragen | Microsoft Azure"
   description="Meer informatie over wanneer de Azure SQL Web- en databases wordt ingetrokken en meer informatie over de functies en functionaliteit van de nieuwe service niveaus."
   services="sql-database"
   documentationCenter="na"
   authors="stevestein"
   manager="jhubbard"
   editor="monicar" />
<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management"
   ms.date="08/08/2016"
   ms.author="sstein" />

# <a name="web-and-business-edition-sunset-faq"></a>Web- en Business Edition zonsondergang Veelgestelde vragen

Azure SQL Web en bedrijfsdatabases nu teruggetrokken. Voor de niveaus Basic, Standard, Premium en elastische vervangen door de retiring Web- en databases.

Om u te helpen met het upgraden van Web- en databases, adviseert de service SQL-Database een passende niveaus en prestaties serviceniveau (prijzen laag) voor elke database die is gebaseerd op de historische werklast.

**Met ophalen prijzen laag aanbevelingen:**

- [Upgrade naar SQL Database V12 met de Azure portal](sql-database-upgrade-server-portal.md)
- [Upgrade naar SQL Database V12 met PowerShell](sql-database-upgrade-server-powershell.md)
- [De prijzen laag van een Web- of database wijzigen](sql-database-service-tier-advisor.md)



## <a name="why-does-the-azure-portal-show-my-web-and-business-edition-databases-as-retired"></a>Waarom wordt mijn Web- en Business edition-databases als ingetrokken weergegeven in de Azure portal?

Omdat het Web en Business edition-database niet meer beschikbaar na September 2015, labels de portal Web- en -databases zijn ingetrokken. Het label teruggetrokken biedt ook een herinnering dat Web- en -databases moeten worden bijgewerkt naar Standard, Basic of Premium. Zie [een upgrade uitvoeren naar Azure SQL Database V12](sql-database-upgrade-server-portal.md)voor gedetailleerde informatie over het upgraden van bestaande Web- of databases voor de nieuwe service niveaus.

## <a name="which-new-service-tier-is-the-best-choice-to-upgrade-my-existing-web-or-business-database-to"></a>Welke nieuwe service-niveau is de beste keuze om te upgraden van mijn bestaande Web- of database?

Selecteren van een geschikte nieuwe laag en prestaties serviceniveau voor uw bestaande Web- of database is afhankelijk van de specifieke functie en prestaties eisen voor uw toepassing.

Met de prijzen laag aanbevelingen beschreven hierboven, of voor meer informatie kunt u helpen bij het selecteren van een geschikte nieuwe service tier, Zie [Upgrade naar Azure SQL Database V12](sql-database-upgrade-server-portal.md).

## <a name="why-is-microsoft-introducing-new-service-tiers"></a>Waarom introduceert Microsoft nieuwe service niveaus?

Op basis van feedback van klanten, introduceert Azure SQL Database nieuwe service tiers, zodat klanten eenvoudig ondersteuning voor relationele database werkbelasting. De nieuwe lagen zijn ontworpen om voorspelbare prestaties over een breed scala van zeven niveaus voor lichte tot zware gewicht transactionele toepassing eisen. Bovendien bieden de nieuwe lagen een aantal zakelijke continuïteit functies, een betere uptime SLA, grotere database voor minder geld en een betere ervaring voor facturering.

## <a name="where-can-i-learn-more-about-the-new-service-tiers"></a>Waar vind ik meer informatie over de nieuwe service niveaus?

Zie voor gedetailleerde informatie over de nieuwe service niveaus en het model van de prestaties, [Service tiers](sql-database-service-tiers.md). Voor gedetailleerde prijsinformatie voor de nieuwe service tiers, Zie [prijzen voor SQL-Database](https://azure.microsoft.com/pricing/details/sql-database/).

## <a name="what-features-or-functionality-will-not-be-available-in-basic-standard-and-premium"></a>Welke functies of -functionaliteit is alleen beschikbaar in de Basic, Standard en Premium?

De functie van de overkoepelende organisaties wordt van de Web- en edities ingetrokken. Klanten willen hun databases schalen wordt aangeraden in plaats daarvan gebruiken of [elastische](sql-database-elastic-scale-get-started.md) Databasehulpmiddelen migreren voor de [Azure SQL-Database](sql-database-elastic-scale-get-started.md)maken en beheren van een toepassing die gebruikmaakt van sharding vereenvoudigt. Een .NET client library kan toepassingen om te definiëren hoe de gegevens wordt toegewezen aan shards en routes OLTP-aanvragen naar de juiste databases. Ter ondersteuning van die configureren hoe gegevens worden verdeeld over shards, vindt een sjabloon Azure cloud service u kunt hosten van uw eigen abonnement op Azure. Microsoft blijft naast [elastische Databasehulpmiddelen](sql-database-elastic-scale-get-started.md), maken en publiceren van de [richtlijnen voor aangepaste sharding patronen en werkwijzen](https://msdn.microsoft.com/library/azure/dn764977.aspx) op basis van de geleerde lessen van klant diep zijn. Nieuwe klanten die geschaald uitbreiden-functionaliteit moeten uitchecken [elastische Databasehulpmiddelen](sql-database-elastic-scale-get-started.md) en/of neem contact op met Microsoft Support voor het evalueren van hun opties.

Microsoft is ook de ervaring van de database kopiëren met Premium databases wijzigen. Eerder premium database contingent is beperkt, DATABASE maken... Als een kopie van het in T-SQL geschorst Premium database gemaakt zonder het gereserveerde bronnen die is belast met dezelfde snelheid als een database. Als premium quotum nu meer vrij toegankelijk is is, is geschorst premie niet meer ondersteund. Kopieën van de database wordt gemaakt met dezelfde editie en prestatieniveau als bron en dienovereenkomstig worden gefactureerd. Klanten kunnen kiezen voor de downgrade gekopieerde databases op een andere laag of prestaties serviceniveau aan hun kosten verlagen indien gewenst. Bestaande Premium geschorst databases worden geconverteerd naar de Business edition als onderdeel van deze release. Verwacht wordt dat de invoering van een [Point-In-Time herstellen](sql-database-recovery-using-backups.md#point-in-time-restore) zorgt ervoor dat de noodzaak van back-ups van databases.

## <a name="how-does-basic-standard-and-premium-improve-my-billing-experience"></a>Hoe Basic, Standard en Premium verbeteren mijn facturering?

Basic, Standard en Premium Azure SQL-databases worden gefactureerd per uur en u hebt de mogelijkheid om de schaal van elke database omhoog of omlaag. U wordt gefactureerd tegen een vaste rente op basis van het hoogste niveau en prestaties serviceniveau u voor elk uur kiest. Bovendien prestatieniveaus (voorbeeld: Basic, S1 en P2) zijn opgedeeld in de stuklijst gemakkelijker te zien van het aantal database dagen/uren u in één maand voor elk prestatieniveau ontstaan. Web- en databases blijven worden gefactureerd op basis van de grootte van de database Database-eenheden. Ga naar de [SQL-Database prijzen pagina](https://azure.microsoft.com/pricing/details/sql-database/) voor meer informatie over de tarieven en de verschillen tussen de nieuwe service niveaus.


## <a name="see-also"></a>Zie ook

[Azure SQL-Database](https://azure.microsoft.com/documentation/services/sql-database/)

[Web- en Business-prijzen](https://azure.microsoft.com/pricing/details/sql-database/web-business/)

[Service-lagen](sql-database-service-tiers.md)

[Een upgrade uitvoeren naar Azure SQL Database V12](sql-database-upgrade-server-portal.md)
