<properties
   pageTitle="Azure SQL Database oplossing snel aan de slag | Microsoft Azure"
   description="Meer informatie over Azure SQL Database-oplossingen"
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
   ms.workload="sqldb-quickstart"
   ms.date="09/06/2016"
   ms.author="carlrab"/>

# <a name="explore-azure-sql-database-solution-quick-starts"></a>Ontdek de Azure SQL Database oplossing snel aan de slag

Dit artikel bevat een overzicht van de Azure SQL Database oplossing snel begint. Deze snelle begint bevinden zich in de opslagplaats van GitHub SQL Server monsters en voorbeelden van het gebruik van SQL-Database in een complete oplossing op basis van realistische scenario's. Voor eenvoudige stapsgewijze zelfstudies die aantonen het gebruik van een bepaalde functie in de SQL-Database dat, Zie [zelfstudies Azure SQL-Database verkennen](sql-database-explore-tutorials.md).

## <a name="try-the-wingtiptickets-demo-and-hands-on-lab"></a>Probeer de demo van WingTipTickets en praktijkgerichte lessen

De demo [Azure SQL Database WingTipTickets](https://github.com/microsoft/wingtiptickets) en de praktijkcursus blijkt een Azure SQL-Database en een monster Azure op zoekopdrachten gebaseerde toepassing die wordt gebruikt om te verkopen van concertkaartjes.


## <a name="collect-and-monitor-resource-usage-data-across-multiple-pools"></a>Verzamelen en bijhouden van gegevens over brongebruik voor meerdere toepassingen

[Quick Start-oplossing: elastische Pool telemetrie met PowerShell](https://github.com/Microsoft/sql-server-samples/tree/master/samples/manage/azure-sql-db-elastic-pools) biedt een oplossing voor het verzamelen en brongebruik SQL-Database via meerdere toepassingen in een abonnement. Wanneer u een groot aantal databases in een abonnement hebt, is het lastig om elke elastische toepassingen afzonderlijk controleren.

U kunt dit probleem oplossen door SQL Database PowerShell-cmdlets en T-SQL-query's te verzamelen gegevens over brongebruik van meerdere groepen en hun databases combineren. Dit helpt u bij het controleren en het gebruik van systeembronnen efficiënter te analyseren.

Deze Quick Start biedt een set van PowerShell scripts en T-SQL-query's en documentatie over de oplossing de betekenis en de ter uitvoering van deze.

## <a name="get-started-with-elastic-database-in-an-saas-scenario"></a>Aan de slag met elastische Database in een scenario met SaaS

 [Quick Start-oplossing: aangepast dashboard voor SaaS toepassingen elastische](https://github.com/Microsoft/sql-server-samples/tree/master/samples/manage/azure-sql-db-elastic-pools-custom-dashboard) biedt een oplossing voor een Software-als-een-oplossing (SaaS)-scenario dat gebruikmaakt van de functie elastische Database van SQL-Database op een kosteneffectieve en schaalbare database backend bieden voor een SaaS-toepassing.

In deze oplossing helpt u bij de uitvoering van een web app. Dit web app kunt u visualiseren van de belasting die door de generator van een belasting die gebruikmaakt van een aangepast dashboard de Azure portal is een aanvulling op een elastische database wordt gemaakt.

Deze Quick Start biedt een generator laden en monitoring web app en de documentatie over de betekenis van de app en het gebruik ervan.

## <a name="create-an-azure-sql-database-by-using-code-first-development-and-the-entity-framework"></a>Azure SQL-database maken met behulp van Code eerste ontwikkeling en het kader van de entiteit

De video en het monster in de [Eerste Code naar een nieuwe Database](https://msdn.microsoft.com/data/jj193542.aspx) bevat een inleiding tot Code eerste ontwikkeling die gericht is op een nieuwe database. In dit scenario is bedoeld voor een database die niet bestaat, maar die wordt gemaakt door de eerste Code. U kunt ook maakt het scenario een lege database waarin Code eerste nieuwe tabellen voegt.

Code eerst kunt u uw model definiëren met behulp van de klassen C# of Visual Basic .NET. Met behulp van de kenmerken van de klassen en eigenschappen of met behulp van een API fluent kunt u optionele extra configuratiestappen uitvoeren.

## <a name="integrate-elastic-database-tools-into-an-entity-framework-application"></a>Elastische Databasehulpmiddelen integreren in een entiteit Framework-toepassing

De [elastische Database client-bibliotheek met entiteit Framework](sql-database-elastic-scale-use-entity-framework-applications-visual-studio.md) -voorbeeld ziet u de wijzigingen die u aanbrengen in een entiteit Framework-toepassing wilt te integreren met [Databasehulpmiddelen elastisch](sql-database-elastic-scale-get-started.md). De nadruk ligt op het opstellen van [shard beheer toewijzen](sql-database-elastic-scale-shard-map-management.md) en [gegevensafhankelijke routering](sql-database-elastic-scale-data-dependent-routing.md) met de eerste Code van entiteit Framework aanpak.

De [Eerste Code een nieuwe database steekproef voor EF](http://msdn.microsoft.com/data/jj193542.aspx) fungeert als ons voorbeeld uitgevoerd in dit voorbeeld. De voorbeeldcode die bij dit document maakt deel uit van de elastische Database extra set van de monsters in de voorbeeldcode van Visual Studio.

## <a name="integrate-elastic-database-tools-with-row-level-security"></a>Extra elastische Database met beveiliging op integreren

[Multitenant toepassingen met elastische Databasehulpmiddelen en beveiliging](sql-database-elastic-tools-multi-tenant-row-level-security.md) bevat de wijzigingen die u wilt aanbrengen in een entiteit Framework-toepassing [Extra elastische Database](sql-database-elastic-scale-get-started.md) met [beveiliging op](https://msdn.microsoft.com/library/dn765131)integreren. In dit voorbeeld ziet u hoe deze technologieën samen gebruiken om het maken van een toepassing met een zeer schaalbare gegevenslaag die multitenant shards ondersteunt.

U doen dit met behulp van ADO.NET SqlClient of entiteit Framework. In dit voorbeeld wordt de [elastische Database client-bibliotheek met entiteit Framework](sql-database-elastic-scale-use-entity-framework-applications-visual-studio.md) uitgebreid met ondersteuning voor databases multitenant shard.
Hiermee maakt u een eenvoudige consoletoepassing voor het maken van blogs en berichten, met vier huurders en twee multitenant shard databases.

## <a name="create-online-surveys-with-the-tailspin-surveys-application"></a>On line enquêtes maken met de toepassing van Tailspin enquêtes

Deze [voorbeeldtoepassing Tailspin enquêtes](https://github.com/Azure-Samples/guidance-identity-management-for-multitenant-apps/blob/master/docs/running-the-app.md) is een multitenant webtoepassing, enquêtes, de naam die gebruikers online enquêtes maken. Het monster heeft betrekking op enkele belangrijke bezorgdheid over het beheren van de identiteit van gebruikers in een multitenant toepassing, waaronder aanmelding, verificatie, machtiging en app rollen.

## <a name="learn-about-the-latest-security-features-of-sql-database-with-the-contoso-clinic-demo-application"></a>Meer informatie over de nieuwste beveiligingsvoorzieningen van SQL-Database met de toepassing van Contoso kliniek Demo

Deze [voorbeeldtoepassing Contoso kliniek](https://github.com/Microsoft/azure-sql-security-sample) toonbeeld geeft van de nieuwste beveiligingsvoorzieningen van SQL-Database.

## <a name="next-steps"></a>Volgende stappen

[Zelfstudies voor Azure SQL-Database verkennen](sql-database-explore-tutorials.md)
