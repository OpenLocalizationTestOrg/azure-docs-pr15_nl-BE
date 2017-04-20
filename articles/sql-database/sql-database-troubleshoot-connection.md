<properties
    pageTitle="Database op de server is momenteel niet beschikbaar, verbinding maken met de SQL-Database | Microsoft Azure"
    description="Problemen oplossen met de database op server is niet beschikbaar fout wanneer een toepassing een verbinding met de SQL-Database."
    services="sql-database"
    documentationCenter=""
    authors="dalechen"
    manager="felixwu"
    editor=""
    keywords="database op de server is momenteel niet beschikbaar, verbinding maken met de sql-database"/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/21/2016"
    ms.author="daleche"/>

# <a name="error-database-on-server-is-not-currently-available-when-connecting-to-sql-database"></a>Fout 'De Database op de server is niet beschikbaar' wanneer u verbinding maakt met sql-database
[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

Wanneer een toepassing een verbinding met een Azure SQL-database, wordt het volgende foutbericht weergegeven:

```
Error code 40613: "Database <x> on server <y> is not currently available. Please retry the connection later. If the problem persists, contact customer support, and provide them the session tracing ID of <z>"
```

> [AZURE.NOTE] Dit foutbericht wordt meestal van voorbijgaande aard (tijdelijk).

Deze fout treedt op wanneer de Azure database wordt verplaatst (of opnieuw geconfigureerd) en uw toepassing de verbinding met de SQL-database wordt verbroken. SQL database herconfiguratie gebeurtenissen wordt veroorzaakt door een geplande gebeurtenis (bijvoorbeeld een software-upgrade) of een niet-geplande gebeurtenis (bijvoorbeeld een proces crash of netwerktaakverdeling). De meeste gebeurtenissen van de nieuwe configuratie meestal tijdelijk zijn en moeten worden uitgevoerd in minder dan 60 seconden maximaal. Deze gebeurtenissen kunnen echter soms duurt langer voltooien, bijvoorbeeld wanneer een grote transactie zorgt ervoor een langdurige herstel dat.

## <a name="steps-to-resolve-transient-connectivity-issues"></a>Stappen voor het oplossen van problemen met de tijdelijke
1.  Controleer of het [Dashboard van Microsoft Azure Service](https://azure.microsoft.com/status) op alle bekende storingen die zijn opgetreden tijdens de tijd gedurende welke de fouten zijn gemeld door de toepassing.
2. Toepassingen die verbinding met een cloud service maken zoals Azure SQL-Database moeten periodieke herconfiguratie gebeurtenissen verwachten en implementeren van proberen logica voor de afhandeling van deze fouten in plaats van deze als toepassingsfouten voor gebruikers die. Raadpleeg de sectie [tijdelijke fouten](sql-database-connectivity-issues.md) en de best practices en design richtlijnen op [SQL Database ontwikkelen-overzicht](sql-database-develop-overview.md) voor meer informatie en algemene strategieën opnieuw. Bekijk vervolgens de codevoorbeelden op [Bibliotheken voor SQL-Database en SQL Server Connection](sql-database-libraries.md) voor specifieke informatie.
3.  Als een database de grenzen van de bron nadert, kan het lijken te zijn van een tijdelijk verbindingsprobleem. Zie voor [het oplossen van prestatieproblemen](sql-database-troubleshoot-performance.md).
4.  Als problemen blijven, of als de duur waarvoor de toepassing de fout optreedt in meer dan 60 seconden of als u meerdere exemplaren van de fout wordt weergegeven in een bepaalde dag, het bestand een ondersteuningsverzoek Azure door **Ondersteuning krijgen** op de site [Ondersteuning van Azure](https://azure.microsoft.com/support/options) .

## <a name="next-steps"></a>Volgende stappen
- Als er een andere fout, beoordelen het [foutbericht](sql-database-develop-error-messages.md) voor aanwijzingen over de oorzaak.
- Als het probleem permanent is, gaat u naar de instructies in [veelvoorkomende verbindingsproblemen oplossen met Azure SQL-Database](sql-database-troubleshoot-common-connection-issues.md).
