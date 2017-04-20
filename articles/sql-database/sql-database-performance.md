<properties 
   pageTitle="Azure SQL Database prestaties inzicht | Microsoft Azure" 
   description="De Azure SQL-Database biedt hulpmiddelen waarmee u gebieden herkennen die in de huidige queryprestaties kunnen verbeteren." 
   services="sql-database" 
   documentationCenter="" 
   authors="stevestein" 
   manager="jhubbard" 
   editor="monicar"/>

<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management" 
   ms.date="07/19/2016"
   ms.author="sstein"/>

# <a name="sql-database-performance-insight"></a>SQL Database prestaties inzicht

Azure SQL-Database biedt prestaties van hulpmiddelen voor het opsporen en verbeteren de prestaties van uw databases door intelligente tuning acties en aanbevelingen. 

1. Ga naar de database in de [Azure Portal](http://portal.azure.com) en **alle instellingen**op > **prestaties **  >  **overzicht van** de **prestaties** pagina openen. 


2. Klik op **aanbevelingen** om de [Adviseur voor SQL-Database](#sql-database-advisor)openen en **query's** om te openen [Query prestaties inzicht](#query-performance-insight)op.

    ![Prestaties weergeven](./media/sql-database-performance/entries.png)



## <a name="performance-overview"></a>Prestaties-overzicht

Te klikken op het **Overzicht** of op de tegel **prestaties** , gaat u naar het Prestatiedashboard voor uw database. Deze weergave biedt een overzicht van de databaseprestaties van uw en kunt u prestaties afstemmen en het oplossen van problemen. 

![Prestaties](./media/sql-database-performance/performance.png)

- De tegel **aanbevelingen** geeft een opsplitsing van de aanbevelingen voor uw database tuning (top 3 aanbevelingen worden weergegeven als er nog meer). Deze tegel klikken keert u terug naar **SQL Database Advisor**. 
- De **activiteit afstemmen** tegel biedt een overzicht van lopende en voltooide acties voor uw database afstemmen, zodat u een snelle weergave in de geschiedenis van de activiteit afstemmen. Deze naast elkaar te klikken gaat u naar de volledige tuning Historie weergave voor uw database.
- De tegel **Auto-aanpassing** ziet u de auto-aanpassing voor de database (welke aangepaste acties zijn ingesteld automatisch worden toegepast op uw database). Op deze tegel, wordt het dialoogvenster automation geopend.
- De tegel **databasequery's** geeft een overzicht van de prestaties van query's voor uw database (DTU gebruik en boven resources die query's). Deze tegel klikken keert u terug naar **Query prestaties inzicht**.



## <a name="sql-database-advisor"></a>SQL-Database-adviseur


[SQL Database Advisor](sql-database-advisor.md) bevat intelligente tuning aanbevelingen die kunnen helpen bij het verbeteren van de prestaties van uw database. 

- Aanbevelingen op die indexen te maken of weg (en aanbevelingen van de index automatisch zonder tussenkomst van de gebruiker en automatisch teruggedraaid aanbevelingen die een negatieve invloed op prestaties hebben wordt toegepast).
- Aanbevelingen bij het schema problemen zijn gevonden in de database.
- Aanbevelingen bij query's van query's met parameters profiteren kunnen.




## <a name="query-performance-insight"></a>Query prestaties inzicht

[Query prestaties inzicht](sql-database-query-performance.md) kunt u minder problemen met prestaties van de database door middel van tijd besteden aan:

- Meer inzicht in uw verbruik van databases (DTU). 
- De bovenste CPU verbruikt query's mogelijk voor betere prestaties kunnen worden afgestemd. 
- De mogelijkheid om een drilldownbewerking uitvoeren op de details van een query. 


## <a name="additional-resources"></a>Aanvullende bronnen

- [Richtlijnen voor Azure SQL-Database prestaties voor enkele databases](sql-database-performance-guidance.md)
- [Wanneer moet een elastische database-toepassingen worden gebruikt?](sql-database-elastic-pool-guidance.md)