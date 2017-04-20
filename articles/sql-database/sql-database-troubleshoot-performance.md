<properties
    pageTitle="SQL-Database performance tuning tips | Microsoft Azure"
    description="Tips voor het afstemmen in Azure SQL-Database via de evaluatie en verbetering van prestaties."
    services="sql-database"
    documentationCenter=""
    authors="v-shysun"
    manager="felixwu"
    editor=""
    keywords="SQL afstelling van prestaties, databaseprestaties afstemmen, tuning tips, sql-prestaties optimaliseren voor sql-database"/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/13/2016"
    ms.author="v-shysun"/>

# <a name="sql-database-performance-tuning-tips"></a>SQL-Database performance tuning tips
Kunt u de [service tier](sql-database-service-tiers.md) van één database wijzigen of verhogen van de eDTUs van een elastische database toepassingen te allen tijde de prestaties te verbeteren, maar kunt u de mogelijkheden te verbeteren en optimaliseren van prestaties van query's eerst in kaart brengen. Ontbrekende indexen en slecht geoptimaliseerde query's worden veelvoorkomende oorzaken van slechte databaseprestaties. Dit artikel bevat richtlijnen voor prestaties afstemmen in SQL-Database.

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="steps-to-evaluate-and-tune-database-performance"></a>Stappen om te evalueren en afstemmen van de prestaties van databases
1.  Klik op **SQL-databases**in [Azure Portal](https://portal.azure.com), selecteert u de database en vervolgens de grafiek controle gebruiken om te zoeken naar resources hun maximum nadert. DTU-verbruik wordt standaard weergegeven. Klik op **bewerken** om het tijdsbereik en waarden te wijzigen.
2.  Met [Query prestaties inzicht](sql-database-query-performance.md) kunt u de query's met behulp van DTUs en [SQL Database Advisor](sql-database-advisor.md) gebruik om aanbevelingen voor het maken en indexen worden weggehaald, parameters voorzien van query's en het oplossen van problemen met schema te bekijken.
3.  Kunt u beheer van dynamische weergaven (DMVs), Extended-gebeurtenissen (Xevents) en de Query opslaan in SSMS prestatieparameters in real-time ophalen. Zie het [onderwerp van prestaties richtlijnen](sql-database-performance-guidance.md) voor gedetailleerde controleren en afstemmen van tips.


    > [AZURE.IMPORTANT] Het wordt aanbevolen dat u altijd de meest recente versie van het Management Studio gebruiken om met updates voor Microsoft Azure en SQL-Database gesynchroniseerd blijven. [Bijwerken van SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).


## <a name="steps-to-improve-database-performance-with-more-resources"></a>Stappen voor het verbeteren van de prestaties van de database met meer bronnen
1.  Voor enkele databases kunt u de [service niveaus wijzigen](sql-database-scale-up.md) op verzoek voor het verbeteren van de prestaties van de database.
2.  Kunt u resources automatisch schalen met [elastische database toepassingen](sql-database-elastic-pool-guidance.md) voor meerdere databases.
