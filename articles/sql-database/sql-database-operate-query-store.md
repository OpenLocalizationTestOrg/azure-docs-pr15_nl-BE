<properties
   pageTitle="Query-winkel in Azure SQL-Database"
   description="Informatie over het werken in Azure SQL-Database het Query-archief"
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
   ms.tgt_pltfrm="sqldb-performance"
   ms.workload="data-management"
   ms.date="08/16/2016"
   ms.author="carlrab"/>

# <a name="operating-the-query-store-in-azure-sql-database"></a>De Query-winkel in Azure SQL-Database 

Query opslaan in Azure is een volledig beheerde database-functie die voortdurend worden verzameld en gedetailleerde historische gegevens over alle query's. U kunt zien over de Query opslaan als vergelijkbaar met een vliegtuig van flight data recorder die aanzienlijk vereenvoudigt de probleemoplossing voor cloud prestaties van query's en klanten bedrijfsruimten. In dit artikel wordt uitgelegd dat bepaalde aspecten van het Query-winkel in Azure. Met deze vooraf verzamelde querygegevens, kunt u snel te analyseren en oplossen van prestatieproblemen met en dus besteden meer tijd aan hun bedrijf. 

Query-winkel is sinds [Algemeen beschikbaar](https://azure.microsoft.com/updates/general-availability-azure-sql-database-query-store/) in Azure SQL Database November 2015. Query-winkel is de basis voor de prestatieanalyse en functies, zoals [SQL Database Advisor en Dashboard prestaties](https://azure.microsoft.com/updates/sqldatabaseadvisorga/)afstemmen. Op het moment van publicatie van dit artikel, wordt winkel Query uitgevoerd in de databases met meer dan 200.000 in Azure, verzamelen van query-informatie over de verschillende maanden, zonder onderbreking.

> [AZURE.IMPORTANT] Microsoft is aan het Query-winkel activeren voor alle Azure SQL-databases (oude en nieuwe). 

## <a name="optimal-query-store-configuration"></a>Optimale Query opslaan configuratie

Deze sectie beschrijft de optimale configuratie standaardwaarden die zijn ontworpen voor betrouwbare werking van de Query opslaan en de afhankelijke functies, zoals [SQL Database Advisor en Performance Dashboard](https://azure.microsoft.com/updates/sqldatabaseadvisorga/). Standaardconfiguratie is geoptimaliseerd voor continue gegevensverzameling die minimale tijd besteed aan de lidstaten uit/alleen-lezen is.

| Configuratie | Beschrijving | Standaard | Opmerking |
| ------------- | ----------- | ------- | ------- |
| MAX_STORAGE_SIZE_MB | Hiermee geeft u de limiet voor de ruimte die winkel Query in een klantendatabase z uitvoeren kunt | 100 | Afgedwongen voor nieuwe databases |
| INTERVAL_LENGTH_MINUTES | Bepaalt de grootte van de tijdsduur gedurende welke statistieken verzameld runtime voor query's worden samengevoegd en bewaard. Elke actieve query heeft hooguit één rij voor een periode die is gedefinieerd met behulp van deze configuratie | 60   | Afgedwongen voor nieuwe databases |
| STALE_QUERY_THRESHOLD_DAYS | Op basis van tijd opruimen beleid waarmee de bewaarperiode van permanente runtime statistieken en niet-actieve query 's | 30 | Afgedwongen voor nieuwe databases en databases met de vorige standaard (367) |
| SIZE_BASED_CLEANUP_MODE | Hiermee geeft u aan of automatisch opgeschoond plaats vindt wanneer de Query winkel gegevensgrootte nadert de limiet | Auto | Afgedwongen voor alle databases |
| QUERY_CAPTURE_MODE | Hiermee geeft u aan of alle query's of alleen een subset van de query's worden bijgehouden | Auto | Afgedwongen voor alle databases |
| FLUSH_INTERVAL_SECONDS | Hiermee geeft u de maximale periode gedurende welke statistieken worden bewaard in het geheugen naar schijf voordat runtime vastgelegd | 900 | Afgedwongen voor nieuwe databases |
||||||

> [AZURE.IMPORTANT] Deze standaardwaarden worden automatisch toegepast in de laatste fase van de activering van de Query opslaan in alle Azure SQL databases (Zie voorafgaand aan belangrijke opmerking). Na deze licht van won't Azure SQL-Database wijzigt configuratiewaarden zijn ingesteld door klanten, tenzij zij negatieve invloed op de primaire werklast of betrouwbare bewerkingen van het archief van de Query.

Als u blijven met uw aangepaste instellingen wilt, gebruikt u [ALTER DATABASE met opties voor Query](https://msdn.microsoft.com/library/bb522682.aspx) configuratie in de oorspronkelijke staat herstellen. [Aanbevolen procedures voor het archief van de Query](https://msdn.microsoft.com/library/mt604821.aspx) om te leren hoe boven de optimale configuratieparameters gekozen uitchecken.

## <a name="next-steps"></a>Volgende stappen

[SQL Database prestaties inzicht](sql-database-performance.md)

## <a name="additional-resources"></a>Aanvullende bronnen

Voor meer informatie uitchecken de volgende artikelen:

- [Een flight data recorder voor uw database](https://azure.microsoft.com/blog/query-store-a-flight-data-recorder-for-your-database) 

- [Prestaties controleren met behulp van de Query-winkel](https://msdn.microsoft.com/library/dn817826.aspx)

- [Scenario's voor het gebruik van query-winkel](https://msdn.microsoft.com/library/mt614796.aspx)

- [Prestaties controleren met behulp van de Query-winkel](https://msdn.microsoft.com/library/dn817826.aspx) 
