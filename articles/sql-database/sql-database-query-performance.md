<properties 
   pageTitle="Azure SQL Database Query prestaties inzicht" 
   description="Prestaties controleren in query geeft u de meeste CPU-gebruik van query's voor Azure SQL-Database." 
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
   ms.date="08/09/2016"
   ms.author="sstein"/>

# <a name="azure-sql-database-query-performance-insight"></a>Azure SQL Database Query prestaties inzicht


Beheren en optimaliseren van relationele databases is een uitdaging waarvoor aanzienlijke expertise en investering in tijd. Query prestaties inzicht kunt u minder tijd problemen met prestaties van de database biedt de volgende nodig:

- Meer inzicht in uw verbruik van databases (DTU). 
- De top-query's per aantal CPU/duur/uitvoeren, die mogelijk voor betere prestaties kunnen worden afgestemd.
- De mogelijkheid om een drilldownbewerking uitvoeren op de details van een query, de tekst en de geschiedenis van het gebruik van bronnen weergeven 
- Prestaties afstemmen aantekeningen die acties uitgevoerd door [SQL Azure Database Advisor](sql-database-advisor.md) weergeven  



## <a name="prerequisites"></a>Vereisten

- Query prestaties inzicht is alleen beschikbaar met Azure SQL Database V12.
- Query prestaties inzicht vereist dat [Winkel Query](https://msdn.microsoft.com/library/dn817826.aspx) actief op de database is. Als het archief van de Query niet wordt uitgevoerd, de portal wordt u gevraagd te schakelen.

 
## <a name="permissions"></a>Machtigingen

Gebruik Query prestaties inzicht zijn de volgende machtigingen voor [toegangsbeheer op basis van de rol](../active-directory/role-based-access-control-configure.md) vereist: 

- **Eigenaar**, **lezer**, **Inzender**, **SQL DB Inzender**of **Inzender voor SQL Server** -machtigingen zijn vereist voor het beslag resource top-query's en grafieken weergeven. 
- **Eigenaar**, **Inzender**, **SQL DB medewerker**of **Medewerker van SQL Server** -machtigingen zijn vereist om tekst weer te geven.



## <a name="using-query-performance-insight"></a>Met behulp van Query prestaties inzicht

Query prestaties inzicht is eenvoudig te gebruiken:

- [Azure portal](https://portal.azure.com/) openen en zoeken naar de database die u wilt onderzoeken. 
  - Selecteer "Query prestaties inzicht" uit het menu links onder ondersteuning en het oplossen van problemen.
- Bekijk de lijst met top-bron die query's op het eerste tabblad.
- Selecteer een afzonderlijke query om de details te bekijken.
- Open [SQL Azure Database Advisor](sql-database-advisor.md) en controleren of eventuele aanbevelingen beschikbaar zijn.
- Gebruik de schuifregelaars of uitzoomen of pictogrammen waargenomen interval wijzigen.

    ![Performance dashboard](./media/sql-database-query-performance/performance.png)

> [AZURE.NOTE] Een paar uur van gegevens moet worden vastgelegd door Query winkel voor SQL-Database, zodat query prestaties inzichten. Als de database geen activiteit heeft of Query opslaan in een bepaalde periode niet actief was, is de grafieken leeg wanneer deze periode weergegeven. U kan Query winkel op elk gewenst moment inschakelen als deze niet wordt uitgevoerd.   



## <a name="review-top-cpu-consuming-queries"></a>Bovenste CPU verbruikt query's bekijken

In de [portal](http://portal.azure.com) doen het volgende:

1. Ga naar een SQL-database en **alle instellingen**op > **ondersteuning + probleemoplossing** > **Query prestaties inzicht**. 

    ![Query prestaties inzicht][1]

    De weergave top-query's wordt geopend en de bovenste CPU in beslag nemen query's worden weergegeven.

1. Klik op rond de grafiek voor meer informatie.<br>De bovenste regel ziet u algemene DTU % voor de database, terwijl de balken CPU % tijdens het geselecteerde interval worden gebruikt door de geselecteerde query's weergeven (bijvoorbeeld als de **afgelopen week** is geselecteerd elke staaf vertegenwoordigt één dag).

    ![Top-query 's][2]

    Het raster onder vertegenwoordigt geaggregeerde gegevens voor de zichtbare query's.

  - Query-ID: de unieke id van een query in een database.
  - CPU per query tijdens waarneembare interval (aggregatiefunctie afhankelijk).
  - Duur per query (aggregatiefunctie afhankelijk).
  - Totaal aantal uitvoeringen voor een bepaalde query.

    Schakel afzonderlijke query's wilt opnemen of uitsluiten ze uit de grafiek met behulp van de selectievakjes in of uit.

1. Als uw gegevens verlopen is, klikt u op de knop **vernieuwen** .
1. Kunt u de schuifregelaars gebruiken en zoomknoppen waarneming interval wijzigen en onderzoeken pieken:  ![instellingen](./media/sql-database-query-performance/zoom.png)
1. Optioneel, als u een andere weergave wilt, kunt u selecteren tabblad **aangepast** en instellen:
  
  - Metric (CPU, duur, aantal keer uitgevoerd)
  - Tijdsinterval (vorige week, vorige maand afgelopen 24 uur). 
  - Aantal query's.
  - Aggregatiefunctie.

    ![Instellingen](./media/sql-database-query-performance/custom-tab.png)

## <a name="viewing-individual-query-details"></a>Details van de afzonderlijke query's weergeven

Om Querydetails te bekijken:

1. Klik op de query in de lijst met top-query's.

    ![Details](./media/sql-database-query-performance/details.png)

1. De detailweergave wordt geopend en het aantal query's CPU-verbruik/duur/uitvoeren na verloop van tijd wordt opgesplitst.
1. Klik op rond de grafiek voor meer informatie.
  - Bovenste grafiek bevat een regel met een algemene database DTU % en de balken zijn CPU-percentage gebruikt door de geselecteerde query.
  - Tweede grafiek geeft de totale duur van de geselecteerde query.
  - Onder grafiek geeft de totale aantal uitvoeringen van de geselecteerde query.
    
    ![Querydetails][3]

1. Eventueel schuifregelaars gebruiken, knoppen Inzoomen en uitzoomen of klik op **Instellingen** om te bepalen hoe querygegevens wordt weergegeven of een andere periode kiezen.

## <a name="review-top-queries-per-duration"></a>Bekijk de top-query's per duur

In de recente update van Query prestaties inzicht we twee nieuwe parameters waarmee u potentiële knelpunten geïntroduceerd: aantal duur en uitvoering.<br>

Langdurige query's hebben het grootste potentieel voor meer bronnen vergrendelen, andere gebruikers blokkeren en schaalbaarheid te beperken. Ze zijn ook de beste kandidaten voor optimalisatie.<br>

Langdurige query's identificeren:

1. **Aangepast** tabblad in Query prestaties inzicht voor de geselecteerde database openen
1. Wijzigen van parameters te **duur** zijn
1. Selecteer het aantal query's en -interval waarneming
1. Selecteer aggregatiefunctie.
  - **Som** is de som van alle tijd van de query kan worden uitgevoerd tijdens de hele waarneming interval.
  - **Max** zoekt query's welke tijd kan worden uitgevoerd met volledige waarneming interval maximum is.
  - **Avg** gemiddelde uitvoeringstijd van alle uitvoeringen van de query worden gevonden en u de bovenkant van deze gemiddelden. 

    ![duur van de query][4]

## <a name="review-top-queries-per-execution-count"></a>Bekijk de top-query's per aantal keer uitgevoerd

Groot aantal uitvoeringen mogelijk zonder dat de database zelf en verbruik van resources kan laag, maar algemene toepassing krijgen langzaam.

In sommige gevallen tot het uitvoeren van zeer hoge aantal kan leiden tot het verhogen van retouren-netwerk. Retouren aanzienlijk trager. Ze zijn hiervoor netwerklatentie en op de downstream-server latentie. 

Bijvoorbeeld veel gegevensgestuurde websites sterk toegang krijgen tot de database voor elke aanvraag van de gebruiker. Terwijl connection pooling helpt, de toegenomen netwerkverkeer en de verwerking van de belasting van de databaseserver de prestaties nadelig beïnvloeden kan.  Algemeen advies is vaak tot een absoluut minimum te houden.

Om aan te duiden vaak uitgevoerd ("chatty") query's query:

1. **Aangepast** tabblad in Query prestaties inzicht voor de geselecteerde database openen
1. Maatstelsel van **Totaal uitvoering** zijn wijzigen
1. Selecteer het aantal query's en -interval waarneming

    ![uitvoering van het aantal query 's][5]

## <a name="understanding-performance-tuning-annotations"></a>Informatie over performance tuning aantekeningen 

Tijdens het verkennen van uw werkbelasting in Query prestaties inzicht, zult u merken pictogrammen met verticale lijn op de grafiek.<br>

Deze pictogrammen zijn aantekeningen; zij vertegenwoordigen de prestaties op het gebied van acties die worden uitgevoerd door [SQL Azure Database Advisor](sql-database-advisor.md). Door zwevende aantekening krijgt u basisinformatie over de actie:

![aantekening van de query][6]

Als u wilt meer weten of aanbeveling advisor toepassen, klikt u op het pictogram. Details van de actie wordt geopend. Als het een actieve aanbeveling dat kunt u deze meteen met de opdracht vereffenen.

![query annotation details][7]

### <a name="multiple-annotations"></a>Meerdere aantekeningen. ###

Het is mogelijk dat vanwege het zoomniveau, aantekeningen die dicht bij elkaar krijgen in één samengevouwen zullen. Dit wordt aangegeven door een speciaal pictogram, erop te klikken zal openen nieuwe blade waar de lijst met aantekeningen gegroepeerd worden weergegeven.
Correleren van query's en acties afstemmen van prestaties kunt u de werkbelasting beter te begrijpen. 


##  <a name="optimizing-the-query-store-configuration-for-query-performance-insight"></a>De Query opslaan configuratie voor Query prestaties inzicht optimaliseren

Tijdens het gebruik van Query prestaties inzicht kunnen er de volgende Query opslaan weergegeven:

- 'Query winkel is niet goed geconfigureerd op deze database. Klik hier voor meer informatie."
- 'Query winkel is niet goed geconfigureerd op deze database. Klik hier om instellingen te wijzigen." 

Deze berichten worden meestal weergegeven wanneer Query winkel, kan geen nieuwe gegevens te verzamelen. 

Eerste geval gebeurt wanneer Query Store staat alleen-lezen en parameters optimaal zijn ingesteld. U kunt dit oplossen door het vergroten van het archief van de Query of Query opslaan uit te schakelen.

![knop qds][8]

Tweede geval gebeurt wanneer Query opslaan uitgeschakeld is of parameters worden niet optimaal ingesteld. <br>U kunt het beleid voor behoud en opname wijzigen en Query opslaan door de onderstaande opdrachten uitvoeren of rechtstreeks vanaf de portal in te schakelen:

![knop qds][9]

### <a name="recommended-retention-and-capture-policy"></a>Aanbevolen beleid voor behoud en vastleggen

Er zijn twee soorten bewaarbeleid:

- Grootte op basis – als ingesteld op AUTO het wordt schoon gegevens automatisch wanneer u in de buurt van maximale grootte wordt bereikt.
- Gebaseerd op tijd - standaard zullen we deze ingesteld op 30 dagen, dat wil zeggen, als Query winkel vol loopt, querygegevens die ouder zijn dan 30 dagen wordt verwijderd

Vastleggen van beleid kan worden ingesteld op:

- **Alle** – worden vastgelegd dat alle query's.
- **Auto** – incidentele's en query's met onbeduidende compileren en uitvoeren van duur worden genegeerd. Drempelwaarden voor de uitvoering van het aantal, de compilatie en uitvoering duur worden intern bepaald. Dit is de standaardoptie.
- **Geen** – Query winkel stopt het opnemen nieuwe query's, maar de runtime-statistieken voor al de vastgelegde query's worden nog steeds verzameld.
    
Wij raden u aan alle beleid instelt op automatisch en schone beleid tot 30 dagen:

    ALTER DATABASE [YourDB] 
    SET QUERY_STORE (SIZE_BASED_CLEANUP_MODE = AUTO);
        
    ALTER DATABASE [YourDB] 
    SET QUERY_STORE (CLEANUP_POLICY = (STALE_QUERY_THRESHOLD_DAYS = 30));
    
    ALTER DATABASE [YourDB] 
    SET QUERY_STORE (QUERY_CAPTURE_MODE = AUTO);

Vergroten van de Query opslaan. Dit kan worden uitgevoerd door een verbinding met een database en de afgifte van volgende query:

    ALTER DATABASE [YourDB]
    SET QUERY_STORE (MAX_STORAGE_SIZE_MB = 1024);

Toepassen van deze instellingen maakt uiteindelijk het verzamelen van nieuwe query's, Query-winkel maar als u niet wilt wachten voordat u Query winkel kunt wissen. 
> [AZURE.NOTE] Uitvoeren van de volgende query worden alle huidige gegevens in de Query-archief verwijderd. 


    ALTER DATABASE [YourDB] SET QUERY_STORE CLEAR;


## <a name="summary"></a>Samenvatting

Query prestaties inzicht verschaft inzicht in het effect van uw query werkbelasting en hoe deze zich verhoudt tot verbruik van database. Met deze functie zal u meer informatie over query's die boven en gemakkelijk herkennen die op te lossen voordat ze een probleem worden.




## <a name="next-steps"></a>Volgende stappen

Klik op [aanbevelingen](sql-database-advisor.md) op het blad **Query prestaties inzicht** voor extra aanbevelingen over het verbeteren van de prestaties van de SQL-database.

![Performance Advisor](./media/sql-database-query-performance/ia.png)


<!--Image references-->
[1]: ./media/sql-database-query-performance/tile.png
[2]: ./media/sql-database-query-performance/top-queries.png
[3]: ./media/sql-database-query-performance/query-details.png
[4]: ./media/sql-database-query-performance/top-duration.png
[5]: ./media/sql-database-query-performance/top-execution.png
[6]: ./media/sql-database-query-performance/annotation.png
[7]: ./media/sql-database-query-performance/annotation-details.png
[8]: ./media/sql-database-query-performance/qds-off.png
[9]: ./media/sql-database-query-performance/qds-button.png

