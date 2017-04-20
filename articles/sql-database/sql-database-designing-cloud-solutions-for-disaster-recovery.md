<properties
   pageTitle="Disaster recovery solutions - SQL Database actief Geo-replicatie van de wolk | Microsoft Azure"
   description="Informatie over het ontwerpen van de wolk disaster recovery oplossingen voor business continuity planning met behulp van geografische replicatie voor app gegevens back-up met Azure SQL-Database."
   keywords="noodherstel, disaster recovery oplossingen, app gegevens back-up, geo-replicatie, cloud business continuity planning"
   services="sql-database"
   documentationCenter=""
   authors="anosov1960"
   manager="jhubbard"
   editor="monicar"/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-management"
   ms.date="07/20/2016"
   ms.author="sashan"/>

# <a name="design-an-application-for-cloud-disaster-recovery-using-active-geo-replication-in-sql-database"></a>Een toepassing voor noodherstel wolk met actieve Geo-replicatie in SQL-Database


> [AZURE.NOTE] [Actieve Geo-replicatie](sql-database-geo-replication-overview.md) is nu beschikbaar voor alle databases in alle lagen.



Informatie over het [Actieve Geo-replicatie](sql-database-geo-replication-overview.md) gebruikt in SQL-Database database toepassingen robuuste regionale storingen en ernstige storingen. Voor business continuity planning, kunt u de topologie van de implementatie van toepassing, de serviceovereenkomst die u zijn gericht, kosten en vertraging voor verkeer. In dit artikel, we de algemene patronen van toepassing bekijken en bespreken de voordelen-en nadelen van elke optie. Zie voor informatie over actieve Geo-replicatie met elastische Pools [elastische Pool disaster recovery strategieën](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md).

## <a name="design-pattern-1-active-passive-deployment-for-cloud-disaster-recovery-with-a-co-located-database"></a>Ontwerppatroon 1: actieve-passieve distributie voor noodherstel wolk met een database reserveren

Deze optie is vooral geschikt voor toepassingen met de volgende kenmerken:

+ Actieve sessie in één regio Azure
+ Sterke afhankelijkheid van de toegang tot gegevens lezen en schrijven (RW)
+ Cross-regio connectiviteit tussen de toepassingslogica en de database is niet acceptabel vanwege de kosten van latentie en verkeer    

In dit geval is de topologie van de implementatie van toepassing geoptimaliseerd voor het verwerken van regionale rampen als alle componenten die erdoor worden beïnvloed en failover-als een eenheid nodig. De toepassingslogica en de database worden gerepliceerd naar een andere regio voor geografische redundantie, maar ze niet worden gebruikt voor de werkbelasting van toepassingen onder normale omstandigheden. De toepassing in de secundaire regio moet worden geconfigureerd voor gebruik van een SQL-verbindingsreeks met de secundaire database. Beheer van verkeer is te gebruiken [routeringsmethode failover](../traffic-manager/traffic-manager-configure-failover-routing-method.md)ingesteld.  

> [AZURE.NOTE] [Azure verkeer manager](../traffic-manager/traffic-manager-overview.md) wordt gebruikt in dit artikel alleen ter illustratie. U kunt een oplossing voor Netwerktaakverdeling die failover-methode ondersteunt.    

Naast de hoofdtoepassing exemplaren, kunt u overwegen een kleine [toepassing van werknemer rol](cloud-services-choose-me.md#tellmecs) die de primaire database gecontroleerd door periodieke T-SQL alleen-lezen (RO) opdrachten te implementeren. U kunt het automatisch failover geactiveerd, voor het genereren van een bericht op de beheerconsole van uw toepassing, of beide. Om ervoor te zorgen dat controle hebben geen voor hele regio storingen gevolgen, moet u controle instanties toepassing implementeren op elk gebied en elk exemplaar dat is verbonden met de primaire database in de regio van de primaire en de secundaire database in de regio. 

> [AZURE.NOTE] Beide toepassingen controleren moet actief zijn en zowel primaire als secundaire databases probe. Deze laatste kunnen worden gebruikt voor het detecteren van een storing in de secundaire regio en een waarschuwing als de toepassing niet is beveiligd.     

In het volgende diagram ziet u deze configuratie voordat een stroomstoring.

![SQL Database Geo-replicatieconfiguratie. Noodherstel van de wolk.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern1-1.png)

Na een storing in de primaire regio detecteert de toepassing controle dat de primaire database niet toegankelijk is en een waarschuwing geregistreerd. Afhankelijk van uw toepassing SLA, kunt u bepalen hoeveel opeenvolgende monitoring sondes zou moeten mislukken voordat er een stroomstoring database verklaart. Om te bereiken gecoördineerde failover van het eindpunt van de toepassing en de database, hebt u de controle-toepassing de volgende stappen uitvoeren:

1. [De status van het primaire eindpunt](https://msdn.microsoft.com/library/hh758250.aspx) om het eindpunt failover geactiveerd.
2. Bel de secundaire database te [initiëren van failover van de database](sql-database-geo-replication-portal.md).

Na een failover de toepassing zal verwerken de aanvragen van gebruikers in de secundaire regio, maar blijft met de database omdat de primaire database nu in het secundaire regio worden. In dit scenario wordt geïllustreerd door het volgende diagram. Ononderbroken lijnen geven de actieve verbindingen in alle diagrammen, stippellijnen onderbroken verbindingen en stop symptomen triggers actie geven.


![Geo-replicatie: Failover naar secundaire database. App gegevens back-up.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern1-2.png)

Als een storing in de secundaire regio gebeurt de replicatiekoppeling tussen de primaire en de secundaire database is geschorst en de controle toepassing registreert een waarschuwing dat de primaire database wordt blootgesteld. Heeft geen gevolgen voor de prestaties van de toepassing, maar de toepassing werkt blootgesteld en daarom hoger risico in geval beide regio's niet achter elkaar.

> [AZURE.NOTE] Alleen het beste implementatieconfiguraties met één DR regio. Dit komt omdat de meeste van de locaties van Azure twee regio's hebben. Deze configuraties biedt geen bescherming voor uw toepassing na een volledige uitval van beide regio's. In een onwaarschijnlijke geval van een dergelijke storing, kunt u uw databases in een derde regio met [geo terugzetten](sql-database-disaster-recovery.md#recovery-using-geo-restore)herstellen.

Zodra de stroomstoring is beperkt, wordt de secundaire database automatisch gesynchroniseerd met de primaire. Tijdens de synchronisatie prestaties van de primaire enigszins risico lopen afhankelijk van de hoeveelheid gegevens die moeten worden gesynchroniseerd. In het volgende diagram ziet u een storing in de secundaire regio.

![Secundaire database gesynchroniseerd met primaire. Noodherstel van de wolk.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern1-3.png)


De belangrijkste **voordelen** van dit ontwerppatroon zijn:

+ De SQL-verbindingsreeks is ingesteld tijdens de installatie van toepassingen in elke regio en na een failover niet wijzigen.
+ De prestaties van de toepassing hebben geen gevolgen voor failover als de toepassing en de database altijd reserveren.

De belangrijkste **verhouding** is een exemplaar van de redundante toepassing in de secundaire regio wordt alleen gebruikt voor noodherstel.

## <a name="design-pattern-2-active-active-deployment-for-application-load-balancing"></a>Ontwerppatroon 2: actieve implementatie voor taakverdeling van toepassing
Deze wolk disaster recovery optie is het meest geschikt voor toepassingen met de volgende kenmerken:

+ Hoge verhouding van de database wordt gelezen om te schrijven
+ Database schrijven latentie heeft geen gevolgen voor de eindgebruiker  
+ Met behulp van een andere verbindingsreeks worden logica alleen-lezen van lees-en schrijfrechten logica gescheiden
+ Alleen-lezen-logica is niet afhankelijk van gegevens volledig worden gesynchroniseerd met de meest recente updates  

Als uw toepassingen deze kenmerken hebben, kan de eindgebruiker verbindingen voor taakverdeling tussen meerdere instanties van de toepassing in verschillende regio's prestaties en de gebruikersbeleving verbeteren. Voor het implementeren van netwerktaakverdeling, moet elke regio een actief exemplaar van de toepassing met de logica voor lezen en schrijven (RW) verbonden met de primaire database in de primaire regio hebben. De logica voor alleen-lezen (RO) moet worden verbonden met een tweede database in dezelfde regio als het toepassingsexemplaar. Beheer van verkeer moet worden ingesteld kunt [round robin routering](../traffic-manager/traffic-manager-configure-round-robin-routing-method.md) of [prestaties routering](../traffic-manager/traffic-manager-configure-performance-routing-method.md) met [controle eindpunt](../traffic-manager/traffic-manager-monitoring.md) voor elk toepassingsexemplaar is ingeschakeld.

Als in patroon #1, kunt u overwegen een soortgelijke controle toepassing implementeren. Maar in tegenstelling tot patroon #1, de toepassing van de controle is niet verantwoordelijk voor het genereren van de overname van het eindpunt.

> [AZURE.NOTE] Dit patroon wordt meer dan één secundaire database gebruikt, zou slechts een van de secundaire servers worden gebruikt voor failover omwille van de eerder opgemerkt. Omdat dit patroon alleen-lezen toegang tot de secundaire, moet het actieve Geo-replicatie.

Beheer van verkeer moet worden geconfigureerd voor prestaties bewerkingsplan directe gebruikersverbindingen op een exemplaar van de toepassing die het dichtst bij de geografische locatie van de gebruiker. In het volgende diagram ziet u deze configuratie voordat een stroomstoring.
![Geen storing: prestaties van routering naar dichtstbijzijnde toepassing. Geo-replicatie.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern2-1.png)

Als een stroomstoring database wordt aangetroffen in de primaire regio, start u failover van de primaire database op een van de secundaire regio's, de locatie van de primaire database wijzigen. De manager van het verkeer automatisch worden uitgesloten van de off line eindpunt van de routeringstabel, maar blijft het verkeer eindgebruiker routering naar de resterende exemplaren online. Omdat de primaire database bevindt zich nu in een andere regio, moeten overal online hun lees-en schrijfrechten SQL-verbindingsreeks met de nieuwe primaire wijzigen. Het is belangrijk dat u deze wijziging voor het initiëren van failover database maken. De alleen-lezen verbindingsreeksen SQL te behouden als deze altijd naar de database in dezelfde regio verwijzen. De failover-stappen zijn:  

1. Wijzigen, lezen-schrijven SQL verbindingsreeksen om te verwijzen naar de nieuwe primaire.
2. De aangewezen secundaire database aanroepen om [failover-database starten](sql-database-geo-replication-portal.md).

In het volgende diagram ziet u de nieuwe configuratie na de failover.
![Na een failover-configuratie. Noodherstel van de wolk.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern2-2.png)

In het geval van een storing in een van de secundaire gebieden verwijderd de manager van het verkeer automatisch de off line eindpunt in die regio uit de routeringstabel. De replicatie-kanaal met de tweede database in die regio is geschorst. Omdat de resterende gebieden meer verkeer in dit scenario krijgen, is de prestaties van de toepassing hinder tijdens de onderbreking. Zodra de stroomstoring is beperkt, wordt de tweede database in het gebied risico onmiddellijk gesynchroniseerd met de primaire. Tijdens synchronisatie op de prestaties van de primaire enigszins risico lopen afhankelijk van de hoeveelheid gegevens die moeten worden gesynchroniseerd. In het volgende diagram ziet u een storing in een van de secundaire gebieden.

![Stroomstoring in secundaire regio. Noodherstel - Geo-replicatie van de wolk.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern2-3.png)

Het belangrijkste **voordeel** van dit ontwerppatroon is de werkbelasting van toepassingen over meerdere secundaire servers aan de eindgebruiker voor optimale prestaties kunt schalen. De **optimalisatie** van deze optie zijn:

+ Verbindingen tussen de instanties van de toepassing en de database alleen-lezen hebben verschillende latentie en kosten
+ Prestaties van toepassingen wordt gebracht tijdens de onderbreking
+ Instanties van toepassing zijn vereist om de SQL-verbindingsreeks dynamisch te wijzigen nadat failover database.  

> [AZURE.NOTE] Een soortgelijke benadering kan worden gebruikt voor offload van gespecialiseerde werkbelasting zoals taken, hulpprogramma's voor bedrijfsinformatie of back-ups te melden. Meestal deze werkbelasting database belangrijke bronnen verbruiken daarom wordt aanbevolen dat u een van de secundaire databases voor hen met het prestatieniveau van de zijn afgestemd op de verwachte werklast wijst.

## <a name="design-pattern-3-active-passive-deployment-for-data-preservation"></a>Ontwerppatroon 3: actieve-passieve distributie voor de bewaring van gegevens  
Deze optie is vooral geschikt voor toepassingen met de volgende kenmerken:

+ Elk verlies van gegevens is business hoog risico. De overname van de database kan alleen worden gebruikt als laatste redmiddel als de onderbreking definitief is.
+ De toepassing kan werken in de 'modus alleen-lezen' voor een bepaalde tijdsperiode.

In dit patroon wordt de toepassing op alleen-lezen-modus wanneer verbonden met de secundaire database. De toepassingslogica in de primaire regio bevindt zich samen met de primaire database en werkt in de modus lezen / schrijven (RW). De toepassingslogica in de secundaire regio bevindt zich samen met de secundaire database en is gereed voor gebruik in de modus alleen-lezen (RO).  Beheer van verkeer moet worden ingesteld voor [failover-routering](../traffic-manager/traffic-manager-configure-failover-routing-method.md) [controle eindpunt](../traffic-manager/traffic-manager-monitoring.md) voor beide toepassingsexemplaren is ingeschakeld.

In het volgende diagram ziet u deze configuratie voordat een stroomstoring.
![Actieve-passieve distributie voordat failover wordt uitgevoerd. Noodherstel van de wolk.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern3-1.png)

Als de manager verkeer verbindingen niet de primaire regio detecteert, dit systeem automatisch overschakelt gebruikersverkeer naar het toepassingsexemplaar in de secundaire regio. Met dit patroon is het belangrijk dat u **dit niet doet** failover database starten nadat de stroomstoring is gedetecteerd. De toepassing in de secundaire regio is geactiveerd en in de modus alleen-lezen met behulp van de secundaire database werkt. Dit wordt geïllustreerd door het volgende diagram.

![Stroomstoring: Toepassing in de modus alleen-lezen. Noodherstel van de wolk.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern3-2.png)

Zodra de storing in de primaire regio beperkt is, wordt verkeer manager detecteert het herstel van de verbinding in de primaire regio en gebruikersverkeer schakelt terug naar het exemplaar van de toepassing in de primaire regio. Dat toepassingsexemplaar wordt gehaald en in de modus lezen / schrijven met behulp van de primaire database werkt.

> [AZURE.NOTE] Omdat dit patroon voor alleen-lezen toegang tot de secundaire moet het actieve Geo-replicatie.

In het geval van een storing in de secundaire regio, de manager van het verkeer geeft aan het eindpunt van de toepassing in de primaire regio zoals verslechterd en het kanaal replicatie is geschorst. Deze storing heeft echter geen gevolgen voor de prestaties van de toepassing tijdens de onderbreking. Zodra de stroomstoring is beperkt, wordt de secundaire database direct gesynchroniseerd met de primaire. Tijdens synchronisatie op de prestaties van de primaire enigszins risico lopen afhankelijk van de hoeveelheid gegevens die moeten worden gesynchroniseerd.

![Stroomstoring: Secundaire database. Noodherstel van de wolk.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern3-3.png)

Dit ontwerppatroon heeft verschillende **voordelen**:

+ Wordt gegevensverlies voorkomen tijdens de tijdelijke storingen.
+ Dit hoeft u niet een monitoring-toepassing implementeren als het herstel wordt geactiveerd door het beheer van verkeer.
+ Uitvaltijd hangt alleen hoe snel verkeer manager detecteert de storing connectiviteit, die kan worden geconfigureerd.

Het **gebruik van systeembronnen** zijn:

+ Toepassing moet kunnen functioneren in de modus alleen-lezen.
+ Dit is vereist voor het dynamisch overschakelen wanneer deze is verbonden met een database alleen-lezen.
+ Hervatting van lezen / schrijven bewerkingen moet het herstel van de primaire regio, wat betekent dat toegang tot gegevens volledig worden uitgeschakeld voor uren of zelfs dagen.

> [AZURE.NOTE] In het geval van een stroomstoring permanente dienstverlening in de regio, moet u handmatig failover database activeren en het verlies van gegevens te accepteren. De toepassing zal functionele worden in het secundaire gebied met een alleen-lezen toegang tot de database.

## <a name="business-continuity-planning-choose-an-application-design-for-cloud-disaster-recovery"></a>Business continuity planning: Kies een toepassing ontwerpen voor noodherstel cloud

Uw specifieke cloud noodherstelplan kunt combineren of uitbreiden van deze ontwerppatronen om het beste aan de behoeften van uw toepassing.  Zoals eerder vermeld, wordt de strategie die u kiest is gebaseerd op de SLA wilt aanbieden aan uw klanten en de topologie van de implementatie van toepassing. Om u te helpen uw beslissing in de volgende tabel vergelijkt de keuzes op basis van het geschatte verlies of herstel doelstelling (vrijgegeven Productieorder) en geschatte hersteltijd (invoegen).

| Patroon | VRIJGEGEVEN PRODUCTIEORDER | INVOEGEN
| :--- |:--- | :---
| Actief-passief implementatie voor noodherstel met toegang tot de database reserveren | Alleen-lezen toegang < 5 sec | Tijd van detectie, failover-API aanroept + controle van toepassingen testen
| Actieve implementatie voor taakverdeling van toepassing | Alleen-lezen toegang < 5 sec | Tijd van detectie van failover-API-aanroep + SQL-verbindingsreeks wijzigen + controle van toepassingen testen
| Actief-passief-distributie voor de bewaring van gegevens | Alleen-lezen toegang < 5 sec lees-schrijftoegang = nul | Alleen-lezen toegang = verbinding mislukt detectie tijd + toepassing controle test <br>Lees-/ schrijftoegang = tijd beperken de stroomstoring

## <a name="next-steps"></a>Volgende stappen

- Zie informatie over Azure SQL-Database automatisch back-ups, [geautomatiseerde back-ups van SQL-Database](sql-database-automated-backups.md)
- Zie voor een overzicht van business continuity en scenario's [Business continuity-overzicht](sql-database-business-continuity.md)
- Zie informatie over automatische back-ups gebruikt voor herstel, [een database van de service gestart back-ups terugzetten](sql-database-recovery-using-backups.md)
- Zie voor meer informatie over herstelopties voor sneller [Actief-Geo-replicatie](sql-database-geo-replication-overview.md)  
- Zie voor meer informatie over het gebruik van geautomatiseerde back-ups voor het archiveren van [kopie](sql-database-copy.md)
- Zie voor informatie over actieve Geo-replicatie met elastische Pools [elastische Pool disaster recovery strategieën](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md).
