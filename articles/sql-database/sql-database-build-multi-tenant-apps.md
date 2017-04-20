<properties
   pageTitle="Azure SQL-Database wordt gemaakt met meerdere huurder Apps met isolatie en efficiëntie"
   description="Meer informatie over hoe SQL-Database wordt gemaakt met meerdere huurder apps"
   keywords=""
   services="sql-database"
   documentationCenter=""
   authors="CarlRabeler"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-management"
   ms.date="10/13/2016"
   ms.author="carlrab"/>

# <a name="builds-multi-tenant-apps-with-azure-sql-database-with-isolation-and-efficiency"></a>Maakt de huurder met meerdere Apps met Azure SQL-Database met isolatie en efficiëntie

## <a name="leverage-elastic-pools-and-build-more-efficient-multi-tenant-apps"></a>Elastische toepassingen gebruiken en efficiënter met meerdere huurder apps bouwen

Als u een ontwikkelaar SaaS een huurder met meerdere app schrijven en veel klanten verwerken, u vaak gebruik van systeembronnen in klant prestaties, beheer en beveiliging. Met Azure SQL Database elastische Database toepassingen hoeft u niet langer die inbreuk maken. Deze toepassingen te beheren en huurder met meerdere toepassingen en voordelen van de isolatie van een klant per database. Zie [ontwerppatronen voor meerdere huurder SaaS-toepassingen met Azure SQL-Database](sql-database-design-patterns-multi-tenancy-saas-applications.md).

![Build multi-huurder apps](./media/sql-database-build-multi-tenant-apps/sql-database-build-multi-tenant-apps.png)

## <a name="auto-scaling-you-control"></a>Automatisch schalen beheren

Toepassingen schaal automatisch prestaties en opslagcapaciteit voor elastische databases onderweg. U kunt bepalen de prestaties die is toegewezen aan een groep toevoegen of elastische databases op verzoek verwijderen en prestaties van elastische databases zonder dat de totale kosten van de groep te definiëren. Dit betekent dat u geen zorgen te maken over het gebruik van afzonderlijke databases beheren.

[Raadpleeg de documentatie](sql-database-elastic-pool.md)

## <a name="intelligent-management-of-your-environment"></a>Intelligent beheer van uw omgeving

Ingebouwde sizing aanbevelingen bepalen proactief databases die kunnen van de toepassingen profiteren zouden. Deze aanbevelingen kunnen 'wat als'-analyse voor een snelle geoptimaliseerd om te voldoen aan de prestatiedoelstellingen. Rich-prestaties controleren en problemen met dashboards kunt u historische pool gebruik visualiseren.

[Raadpleeg de documentatie](sql-database-elastic-pool-guidance.md)

## <a name="performance-and-price-to-meet-your-needs"></a>Prestaties en prijs op uw behoeften

Basic, Standard en Premium-toepassingen bieden u een breed spectrum van de prestaties, de opslag en de prijzen. Groepen kunnen maximaal 400 elastische databases bevatten. Elastische databases kunnen auto-schaal tot 1000 elastische database transactie-eenheden (eDTU).

[Raadpleeg de documentatie](https://azure.microsoft.com/pricing/details/sql-database/?b=16.50)

## <a name="elastic-tools"></a>Extra elastisch

Naast de elastische pools zijn er functies om u te helpen bij het beheer van operationele activiteiten over meerdere databases SQL-Database:

**Cross database query's en rapporten uitvoeren.**  
[Elastische databasequery](sql-database-elastic-query-overview.md) kunt u query's en rapporten in databases in de groep met elastische uitvoeren en toegang krijgen tot externe gegevens in veel databases van uw groep in één keer.

**Cross databasetransacties worden uitgevoerd.**  
[Elastische databasetransacties](sql-database-elastic-transactions-overview.md) kunt u transacties die een reeks verschillende databases in SQL-Databases en bewerkingen worden uitgevoerd (dat wil zeggen bij de verwerking van financiële transacties tussen databases, of bij het bijwerken van de voorraad in een database en orders) wordt uitgevoerd.

**Uitvoeren van bewerkingen in meerdere databases.**  
[Elastische database taken](sql-database-elastic-jobs-overview.md) uitvoeren administratieve bewerkingen, zoals het opnieuw opbouwen van indexen of schema's voor elke database in uw groep elastische bijwerken.

Ga naar de homepage te zien wat andere SQL-Database te bieden heeft.
[Het uitchecken](https://azure.microsoft.com/services/sql-database/) 

## <a name="next-steps"></a>Volgende stappen

Krijg een [gratis abonnement van Azure](https://azure.microsoft.com/get-started/) en [uw eerste Azure SQL-Database maken](sql-database-get-started.md).

## <a name="additional-resources"></a>Aanvullende bronnen

Ontdek alle [mogelijkheden van SQL-Database](https://azure.microsoft.com/services/sql-database/).
 
Bekijk het [technisch overzicht van de SQL-Database](sql-database-technical-overview.md).  
