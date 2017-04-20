<properties
   pageTitle="Power BI met SQL datawarehouse gebruiken | Microsoft Azure"
   description="Tips voor het gebruik van Power BI met Azure SQL Data Warehouse voor het ontwikkelen van oplossingen."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="05/31/2016"
   ms.author="lodipalm;barbkess;sonyama"/>

# <a name="use-power-bi-with-sql-data-warehouse"></a>Power BI met SQL datawarehouse gebruiken
Als met Azure SQL-Database kan SQL gegevens magazijn Direct Connect gebruiker gebruikmaken van krachtige logische naar beneden duwen naast de analysefuncties van Power BI.  Query's met directe verbinding terug naar uw magazijn Azure SQL gegevens in realtime verzonden als u de gegevens verkennen.  Dit, gecombineerd met de schaal van SQL Data Warehouse, kunnen gebruikers dynamische rapporten maken in minuten tegen terabytes aan gegevens.  Bovendien kan de invoering van de openen in Power BI knop gebruikers rechtstreeks verbinding Power BI hun SQL Data Warehouse zonder gegevens te verzamelen uit andere delen van Azure.

Bij gebruik van Direct Connect Neem notitie:

+ De volledig gekwalificeerde naam opgeven wanneer u verbinding maakt (Zie hieronder voor meer details)
+ Controleer firewall-regels voor de database zijn geconfigureerd op 'Toegang toestaan tot Azure services'.
+ Elke actie, zoals een kolom te selecteren of het toevoegen van een filter wordt het datawarehouse rechtstreeks opvragen
+ Tegels worden vernieuwd ongeveer elke 15 minuten (vernieuwen niet hoeft te worden gepland)
+ Q & A is niet beschikbaar voor datasets Direct Connect
+ Schemawijzigingen zijn niet opgenomen automatisch
+ Alle Direct Connect query's na 2 minuten

Deze beperkingen en notities kunnen veranderen wanneer we doorgaan met het verbeteren van de ervaring. De stappen uit om verbinding te maken, worden hieronder beschreven.  

## <a name="using-the-open-in-power-bi-button"></a>Met behulp van de knop 'Openen in Power BI'
De eenvoudigste manier om te verplaatsen tussen uw SQL Data Warehouse en Power BI is met de openen in Power BI-knop. Met deze knop kunt u naadloos met nieuwe dashboards maken in Power BI.  

1.  Aan de slag Ga naar het exemplaar van SQL Data Warehouse in de klassieke Azure-Portal.
2.  Klik op de knop 'Openen in Power BI'.
3.  Als we niet kunt u rechtstreeks inloggen, of als u niet een Power BI-account hebt, u moet inloggen.  
4.  U wordt doorgestuurd naar de pagina SQL Data Warehouse-verbinding met de gegevens uit de SQL Data Warehouse vooraf ingevuld.
5.  Nadat u uw referenties hebt u volledig verbonden aan uw SQL Data Warehouse.

## <a name="connecting-through-the-power-bi-portal"></a>Verbinding maken via de Power BI-portal
Gebruikers kunnen ook de openen in Power BI-knop gebruiken, verbinding met hun SQL Data Warehouse via de Power BI-Portal.

1.  Klik op gegevens ophalen onderaan in het navigatiedeelvenster.
2.  Selecteer 'Databases'.
3.  Eenmaal op de pagina Databases selecteren Azure SQL datawarehouse en klik op 'Verbinden'.
4.  Voer de benodigde verbindingsinformatie.  De servernaam en database vindt u in de Portal Azure.
5.  U wordt doorgestuurd naar de hoofdpagina van Power BI en nadat de verbinding is gemaakt van een nieuwe vermelding onder 'Datasets' wordt weergegeven met de naam van de instantie.  
6.   U kunt klikken op de nieuwe gegevensset te onderzoeken alle tabellen en weergaven in de database. Als u een kolom selecteert, stuurt een query terug naar de bron, dynamisch maken uw visual. Deze beelden kunnen worden opgeslagen in een nieuw rapport en terug op het dashboard is vastgemaakt.

<!--Image references-->

<!--Article references-->
[SQL Data Warehouse development overview]:  ./sql-data-warehouse-overview-develop/
[SQL Data Warehouse integration overview]:  ./sql-data-warehouse-overview-integration/

<!--MSDN references-->

<!--Other Web references-->
