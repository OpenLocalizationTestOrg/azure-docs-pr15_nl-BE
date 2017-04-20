<properties 
   pageTitle="SQL Database Disaster Recovery oefeningen | Microsoft Azure" 
   description="Informatie over richtlijnen en aanbevolen procedures voor het uitvoeren van disaster recovery oefeningen die met Azure SQL Database houden uw missie essentiële bedrijfstoepassingen veerkrachtig storingen en storingen." 
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
   ms.date="07/31/2016"
   ms.author="sstein; sashan"/>

#<a name="performing-disaster-recovery-drill"></a>Disaster Recovery analyse uitvoeren

Het wordt aanbevolen dat de validatie van de gereedheid voor herstel workflow toepassing regelmatig wordt uitgevoerd. Controleren van het gedrag van de toepassing en de gevolgen van het verlies van gegevens en/of de verstoring die failover betrekking heeft op een goede technische praktijkgewoonten wordt. Het is ook een eis door de meeste industriestandaarden als onderdeel van business continuity certificering.

Uitvoeren van een detailanalyse disaster recovery bestaat uit:

- Ter simulering data tier stroomstoring
- Herstellen 
- Post-toepassingsherstel integriteit te valideren

Afhankelijk van hoe u [uw toepassing voor bedrijfscontinuïteit ontworpen](sql-database-business-continuity.md), de workflow voor het uitvoeren van de analyse kan variëren. Hieronder beschrijven we de beste praktijken voor de uitvoering van een detailanalyse van disaster recovery in de context van Azure SQL-Database. 

##<a name="geo-restore"></a>Geo-terugzetten

Als u wilt voorkomen dat het verlies van gegevens bij het verrichten van een detailanalyse disaster recovery, wordt u aangeraden uitvoeren van de analyse met behulp van een testomgeving zijn gemaakt door een kopie van de productie-omgeving te maken en deze om te controleren of de toepassing failover-workflow.
 
####<a name="outage-simulation"></a>Stroomstoring simuleren

U kunt verwijderen of de naam van de brondatabase wijzigen om de stroomstoring simuleren. Hierdoor wordt de toepassing verbinding mislukt. 

####<a name="recovery"></a>Herstel

- De Geo-het herstellen van de database naar een andere server als beschreven [hier](sql-database-disaster-recovery.md)uitvoeren. 
- De configuratie van de toepassing om verbinding maken met de herstelde database (s) en de handleiding [een database na herstel configureren](sql-database-disaster-recovery.md) om het herstel te wijzigen.

####<a name="validation"></a>Validatie

- De analyse voltooien door te controleren of de integriteit van post toepassingsherstel (dat wil zeggen verbindingsreeksen, aanmeldingen, basisfunctionaliteit testen of andere onderdeel validaties van standaardtoepassing ondertekeningen procedures).

##<a name="geo-replication"></a>Geo-replicatie

Voor een database die is beveiligd met behulp van geografische replicatie omvatten de oefening drill geplande failover naar de secundaire database. De geplande overname zorgt ervoor dat de primaire en de secundaire databases gesynchroniseerd blijven wanneer de rollen worden getransporteerd. In tegenstelling tot de niet-geplande failover leidt deze bewerking niet tot verlies van gegevens, zodat de analyse kan worden uitgevoerd in de productieomgeving. 

####<a name="outage-simulation"></a>Stroomstoring simuleren

U kunt web-toepassing of virtuele machine is verbonden met de database uitschakelen om de stroomstoring simuleren. Hierdoor wordt de connectiviteit storingen in voor de Internet-clients.

####<a name="recovery"></a>Herstel

- Controleer of de configuratie van de toepassing in de DR regio verwijst naar de voormalige secundaire die volledig toegankelijk zijn voor nieuwe primaire. 
- Uitvoeren van [failover gepland](sql-database-geo-replication-powershell.md#initiate-a-planned-failover) voordat u de tweede database een nieuwe primaire
- Volg de gids [een database na herstel configureren](sql-database-disaster-recovery.md) om de herstelbewerking te voltooien.

####<a name="validation"></a>Validatie

- De analyse voltooien door te controleren of de integriteit van post toepassingsherstel (dat wil zeggen verbindingsreeksen, aanmeldingen, basisfunctionaliteit testen of andere onderdeel validaties van standaardtoepassing ondertekeningen procedures).


## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over business continuity scenario's [continuïteit scenario 's](sql-database-business-continuity.md)
- Zie informatie over Azure SQL-Database automatisch back-ups, [geautomatiseerde back-ups van SQL-Database](sql-database-automated-backups.md)
- Zie informatie over automatische back-ups gebruikt voor herstel, [een database van de service gestart back-ups terugzetten](sql-database-recovery-using-backups.md)
- Zie voor meer informatie over herstelopties voor sneller [Actief-Geo-replicatie](sql-database-geo-replication-overview.md)  
