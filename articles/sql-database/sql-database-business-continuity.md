<properties
   pageTitle="Bedrijfscontinuïteit cloud - herstel - Database SQL databases | Microsoft Azure"
   description="Ontdek hoe Azure SQL-Database ondersteunt wolk bedrijfscontinuïteit en herstellen van databases en kunt u belangrijke cloud-toepassingen die worden uitgevoerd."
   keywords="bedrijfscontinuïteit, cloud bedrijfscontinuïteit, noodherstel database, database herstellen"
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
   ms.workload="NA"
   ms.date="10/13/2016"
   ms.author="carlrab"/>

# <a name="overview-of-business-continuity-with-azure-sql-database"></a>Overzicht van de bedrijfscontinuïteit met Azure SQL-Database

Dit overzicht bevat een beschrijving van de mogelijkheden die Azure SQL-Database voor bedrijfscontinuïteit en noodherstel. Het biedt opties, aanbevelingen en zelfstudies voor het herstellen van ontwrichtende gebeurtenissen die kunnen leiden tot gegevensverlies of tot gevolg hebben dat uw database en de toepassing niet meer beschikbaar. Komen wat te doen wanneer een gebruiker de volgende fout is van invloed op de integriteit van gegevens, een Azure regio heeft een storing, of uw toepassing vereist onderhoud. 

## <a name="sql-database-features-that-you-can-use-to-provide-business-continuity"></a>SQL-Database-functies die u gebruiken kunt voor bedrijfscontinuïteit

SQL-Database biedt verschillende business continuïteit, met inbegrip van geautomatiseerde back-ups en optionele databasereplicatie. Elk heeft andere kenmerken voor de geschatte hersteltijd (invoegen) en verlies van gegevens voor de recente transacties. Als u deze opties eenmaal begrijpt, kunt u kiezen tussen deze - en, in de meeste gevallen ze samen gebruiken voor verschillende scenario's. Als uw plan voor bedrijfscontinuïteit te ontwikkelen, moet u weten de maximale toegestane tijd voordat de toepassing volledig hersteld na de gebeurtenis storend - dit uw herstel tijd doelstelling (RTO is). Ook moet u weten hoeveel recente Gegevensupdates (tijdsinterval) de toepassing kan tolereren dat verloren gaat bij het herstellen na de ingrijpende gebeurtenis - de doelstelling van herstel punt (vrijgegeven Productieorder). 

De volgende tabel ziet het invoegen en de vrijgegeven Productieorder voor de drie meest voorkomende scenario's.

| Mogelijkheden |  Basis laag | Standaard laag  | Premium-laag |
|---|---|---|---|
| Punt in tijd terugzetten uit back-up | Een herstelpunt binnen 7 dagen   | Een herstelpunt in 35 dagen  | Een herstelpunt in 35 dagen |
Geo-terugzetten uit back-ups geo gerepliceerd | < 12h, vrijgegeven Productieorder < 1h invoegen   | < 12h, vrijgegeven Productieorder < 1h invoegen   | < 12h, vrijgegeven Productieorder < 1h invoegen |
|Actieve Geo-replicatie | < 30s, vrijgegeven Productieorder < 5s invoegen   | < 30s, vrijgegeven Productieorder < 5s invoegen | < 30s, vrijgegeven Productieorder < 5s invoegen |


### <a name="use-database-backups-to-recover-a-database"></a>Back-ups gebruiken om te herstellen van een database

SQL-Database voert automatisch een combinatie van volledige databaseback-ups wekelijks differentiële database elk uur-ups en transactielogboekback-ups om de vijf minuten uw bedrijf beschermen tegen verlies van gegevens. Deze back-ups worden opgeslagen in een lokaal redundante opslag voor 35 dagen voor databases in de Standard en Premium service niveaus en zeven dagen voor databases in de laag Basic service - Zie [Servicelagen](sql-database-service-tiers.md) voor meer informatie over service tiers. Als de bewaartermijn voor de servicelaag van de niet aan uw zakelijke vereisten, kunt u de bewaarperiode verhogen door [het niveau van de service](sql-database-scale-up.md). De volledige en differentiële database back-ups ook worden gerepliceerd naar een [gepaarde Datacenter](../best-practices-availability-paired-regions.md) ter bescherming tegen een stroomstoring van data center - Zie [Automatische back-ups](sql-database-automated-backups.md) voor meer informatie.

Deze automatische back-ups kunt u een database terugzetten vanuit verschillende ontwrichtende gebeurtenissen, zowel binnen uw datacenter en naar een ander datacenter. Met behulp van automatische back-ups, de geschatte tijd van het herstel is afhankelijk van verschillende factoren zoals het totale aantal databases herstellen in hetzelfde gebied, op hetzelfde moment, de grootte van de database de transactielogboekgrootte en bandbreedte van het netwerk. In de meeste gevallen is de hersteltijd minder dan 12 uur. Bij het herstellen van een andere regio van gegevens, is verlies van gegevens beperkt tot 1 uur door de geo-redundante opslag van per uur differentiële back-ups. 

> [AZURE.IMPORTANT] Als u wilt herstellen met behulp van automatische back-ups, moet u lid zijn van de rol van inzender voor SQL Server of de eigenaar van de abonnement - Zie [RBAC: ingebouwde functies](../active-directory/role-based-access-built-in-roles.md). U kunt herstellen met behulp van de portal Azure, PowerShell of de REST API. U kunt geen Transact-SQL.

Automatische back-ups gebruiken als uw business continuity en herstel mechanisme als uw toepassing:

- Geen missie als kritiek beschouwd.
- Heeft een binding SLA daarom de uitvaltijd van 24 uur of langer niet leiden financiële aansprakelijkheid tot zal.
- Heeft een lage snelheid van de gegevens wijzigen (lage transacties per uur) en verlies van maximaal een uur van de wijziging is een aanvaardbare gegevens verloren gaan. 
- Kosten is gevoelig. 

Als u sneller herstel, gebruikt u [Actieve Geo-replicatie](sql-database-geo-replication-overview.md) (volgende besproken). Als u nodig hebt om te kunnen herstellen van gegevens uit een periode die ouder zijn dan 35 dagen, kunt u de database regelmatig om een Bacpac-bestand archiveren opgeslagen (een gecomprimeerd bestand met het databaseschema en de bijbehorende gegevens) in Azure blob-opslag of op een andere locatie van uw keuze. Zie voor meer informatie over het maken van een transactioneel consistente database archief [een databasekopie maken](sql-database-copy.md) en [de databasekopie te exporteren](sql-database-export.md). 

### <a name="use-active-geo-replication-to-reduce-recovery-time-and-limit-data-loss-associated-with-a-recovery"></a>Actieve Geo-replicatie gebruiken om de hersteltijd reduceren en verlies van gegevens die zijn gekoppeld aan een herstel te beperken

Back-ups voor databaseherstel gebruiken in geval van een onderbreking van het bedrijf, kunt u [Actieve Geo-replicatie](sql-database-geo-replication-overview.md) configureren van een database voor maximaal vier leesbaar secundaire databases hebben in de regio van uw keuze. Deze secundaire databases worden gesynchroniseerd met de primaire database met behulp van een replicatiemechanisme voor de asynchrone. Deze functie wordt gebruikt om te beschermen tegen verstoring van de zakelijke in geval van een stroomstoring van data center of tijdens een upgrade van de toepassing. Actieve Geo-replicatie kan ook worden gebruikt voor betere prestaties van query's voor query's alleen-lezen voor geografisch verspreide gebruikers.

Als de primaire database onverwacht off line raakt, of moet u het off line nemen voor onderhoudsactiviteiten, kunt u snel een secundair worden de primaire (ook wel een failover) en het configureren van toepassingen die verbinding maken met de primaire onlangs bevorderd promoveren. Met een geplande failover is er geen gegevens verloren gaan. Met een niet-geplande failover kunnen er enkele kleine hoeveelheid gegevensverlies voor zeer recente transacties door de aard van de asynchrone replicatie. Nadat failover is uitgevoerd kunt u later failback - volgens een schema of wanneer het datacenter weer on line komt. In alle gevallen gebruikers ondervinden een kleine hoeveelheid uitvaltijd en moeten opnieuw verbinding te maken. 

> [AZURE.IMPORTANT] Actieve Geo-replicatie gebruikt, moet u de eigenaar van het abonnement of beheerdersmachtigingen hebben in SQL Server. U kunt configureren en failover via de portal Azure, PowerShell of de REST API met behulp van machtigingen op het abonnement of met behulp van Transact-SQL met behulp van machtigingen in SQL Server.

Actieve Geo-replicatie wordt gebruikt als de toepassing van deze criteria voldoet:

- Missie is van cruciaal belang.
- Heeft een service level agreement (SLA) die geen 24 uur of langer van downtime.
- Financiële aansprakelijkheid zal leiden tot uitvaltijd.
- Een hoge frequentie van de gegevens wijzigen hoog en een uur gegevens verliezen niet acceptabel is.
- De extra kosten van actieve geo-replicatie is lager dan de potentiële financiële aansprakelijkheid en bijbehorende verlies van zakelijke.

## <a name="recover-a-database-after-a-user-or-application-error"></a>Een database herstellen na een fout van de gebruiker of toepassing

* Niemand is perfect! Een gebruiker kan per ongeluk verwijderen van bepaalde gegevens, per ongeluk een belangrijk tabel verwijderen of zelfs een hele database weghalen. Of een toepassing mogelijk goede gegevens per ongeluk overschrijven met ongeldige gegevens als gevolg van een gebrek van toepassing. 

In dit scenario zijn de opties voor Systeemherstel.

### <a name="perform-a-point-in-time-restore"></a>Het terugzetten van een point-in-time

Die tijd binnen de bewaartermijn voor de database is, kunt u de automatische back-ups herstellen van een kopie van de database naar een bekende, goed punt in de tijd. Nadat de database is teruggezet, kunt u de oorspronkelijke database vervangen door de herstelde database of de benodigde gegevens van de teruggezette gegevens te kopiëren in de oorspronkelijke database. Als de database actief Geo-replicatie gebruikt, wordt aangeraden de benodigde gegevens van de teruggezette kopie in de oorspronkelijke database kopiëren. Als u de oorspronkelijke database door de herstelde database vervangen, moet u configureren en synchronisatie Active Geo-replicatie (dit kan lang duren voor een grote database). 

Voor meer informatie Zie en voor gedetailleerde stappen voor het terugzetten van een database naar een punt in de tijd met behulp van de portal Azure of met PowerShell, [herstellen punt in tijd](sql-database-recovery-using-backups.md#point-in-time-restore). Kunt u niet herstellen met behulp van Transact-SQL.

### <a name="restore-a-deleted-database"></a>Een verwijderde database terugzetten

Als de database wordt verwijderd, maar niet de logische server is verwijderd, kunt u de verwijderde database terugzetten naar het punt waar het is verwijderd. Een back-up van de database wordt teruggezet naar dezelfde logische SQL server waaruit het is verwijderd. U kunt herstellen met behulp van de oorspronkelijke naam of geef een nieuwe naam of de teruggezette database.

Voor meer informatie Zie en voor gedetailleerde stappen voor een verwijderde database terugzetten met behulp van de portal Azure of met PowerShell, [een verwijderde database terugzetten](sql-database-recovery-using-backups.md#deleted-database-restore). U kunt niet herstellen met behulp van Transact-SQL.

> [AZURE.IMPORTANT] Als de logische server is verwijderd, kunt u een verwijderde database niet herstellen. 

### <a name="import-from-a-database-archive"></a>Importeren uit een database archief

Als het verlies van gegevens buiten de huidige bewaarperiode voor geautomatiseerde back-ups opgetreden en u de database is gearchiveerd, kunt u [een gearchiveerd Bacpac-bestand importeren](sql-database-import.md) naar een nieuwe database. Op dit moment kunt u de oorspronkelijke database vervangen door de geïmporteerde database of de benodigde gegevens uit de geïmporteerde gegevens in de oorspronkelijke database kopiëren. 

## <a name="recover-a-database-to-another-region-from-an-azure-regional-data-center-outage"></a>Een database naar een andere regio herstellen na een storing Azure regionale data center

<!-- Explain this scenario -->

Hoewel zeldzaam zijn, kan een storing hebben die een Azure Datacenter. Wanneer er een storing optreedt, wordt dit een verstoring van de zakelijke die mogelijk alleen laatst voor een paar minuten of uren kan duren. 

- Eén optie is om te wachten tot de database weer on line komen na de onderbreking data center. Dit geldt voor toepassingen die de database off line zich kunnen veroorloven. Bijvoorbeeld een ontwikkelingsproject of een gratis proefversie u hoeft niet voortdurend aan werken. Wanneer een datacenter een storing heeft, weet u niet hoe lang duurt de stroomstoring, zodat deze optie alleen werkt als u niet de database nodig voor een tijdje hebt.
- Een andere mogelijkheid is een failover naar een ander gegevensgebied als u actieve Geo-replicatie of het systeem herstellen met behulp van geo-redundante back-ups (Geo herstellen). Failover duurt maar een paar seconden, terwijl het herstellen van back-ups van een uur duurt.

Wanneer u actie moet ondernemen, hoe lang duurt het om te herstellen en hoeveel gegevens verloren gaan die u in geval van een stroomstoring van data center oplopen is afhankelijk van hoe u de business continuity functies hierboven besproken in uw toepassing bepalen. Inderdaad, u kunt een combinatie van back-ups en actieve Geo-replicatie, afhankelijk van de toepassingsvereisten van uw gebruiken. Zie voor een bespreking van de ontwerpaspecten van toepassing voor zelfstandige databases en elastische van toepassingen met behulp van deze functies van business continuity [ontwerp van een toepassing voor noodherstel wolk](sql-database-designing-cloud-solutions-for-disaster-recovery.md) en [elastische Pool disaster recovery strategieën](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md).

In de volgende secties bieden een overzicht van de stappen om te herstellen met behulp van back-ups of actieve Geo-replicatie. Zie voor gedetailleerde stappen, met inbegrip van de vereisten, post stappen en informatie over hoe u een stroomstoring simuleren voor het uitvoeren van een detailanalyse disaster recovery planning [van een storing in een SQL-Database herstellen](sql-database-disaster-recovery.md).

### <a name="prepare-for-an-outage"></a>Voorbereiden op een storing

Ongeacht de business continuity functie waarmee u, moet u:

- Identificeren en de doelserver, waaronder serverniveau firewallregels, aanmeldingen en machtigingen op gebruikersniveau hoofddatabase voorbereiden.
- Het omleiden van clients en toepassingen naar de nieuwe server bepalen
- Document andere afhankelijkheden, zoals de controle-instellingen en waarschuwingen 
 
Als u niet van plan bent en goed om uw toepassingen online voorbereiden na een failover of een herstel extra tijd kost en waarschijnlijk ook problemen moet oplossen op een moment van stress - een onjuiste combinatie.

### <a name="failover-to-a-geo-replicated-secondary-database"></a>Failover naar een secundaire geo gerepliceerd-database 

Als u actieve Geo-replicatie als de herstelfunctie [een failover-bewerking voor een secundaire geo gerepliceerd](sql-database-disaster-recovery.md#failover-to-geo-replicated-secondary-database). Binnen enkele seconden, de secundaire wordt gepromoveerd tot de nieuwe primaire en gereed is voor nieuwe transacties vastleggen en reageren op query's - met maar een paar seconden van het verlies van gegevens voor de gegevens die waren nog niet gerepliceerd. Zie voor meer informatie over het automatiseren van de failover-procedure [ontwerp van een toepassing voor noodherstel wolk](sql-database-designing-cloud-solutions-for-disaster-recovery.md).

> [AZURE.NOTE] Wanneer het datacenter weer on line komt, kunt u failback naar de oorspronkelijke primaire (of niet).

### <a name="perform-a-geo-restore"></a>Geo-herstelbewerking uit te voeren 

Als u automatische back-ups met geo-redundante opslag replicatie als de herstelfunctie [initiëren met Geo-Restore database herstellen](sql-database-disaster-recovery.md#recover-using-geo-restore). Herstel meestal plaatsvindt binnen 12 uur - met verlies van gegevens van maximaal één uur, afhankelijk van wanneer de laatste uur differentiële back-up met genomen en gerepliceerd. Totdat het herstel is voltooid, kan de database niet opnemen van transacties of het reageren op query's. 

> [AZURE.NOTE] Als het datacenter afkomstig weer on line zijn voordat u de toepassing via de herstelde database activeert, kunt u het herstel te annuleren.  

### <a name="perform-post-failover--recovery-tasks"></a>Boeken failover uitvoeren / recovery-taken 

Na het herstellen van een herstelfunctie, moet u de volgende aanvullende taken uitvoeren voordat u uw gebruikers en toepassingen weer actief zijn:

- Redirect clients en toepassingen die met de nieuwe server en de teruggezette database
- Passende niveau van de server firewall-regels zorgen voor gebruikers kunnen verbinding maken met (of [database-niveau firewalls](sql-database-firewall-configure.md#creating-database-level-firewall-rules)gebruiken)
- Geschikte aanmeldingen en machtigingen op gebruikersniveau hoofddatabase zorgen (of gebruik [die gebruikers](https://msdn.microsoft.com/library/ff929188.aspx))
- Configureren, controle, naargelang
- Configureer waarschuwingen,

## <a name="upgrade-an-application-with-minimal-downtime"></a>Upgrades uitvoeren voor toepassingen met een minimale uitval

Soms moet een toepassing vanwege gepland onderhoud, bijvoorbeeld een upgrade van de toepassing off line worden genomen. [Beheren toepassingsupgrades](sql-database-manage-application-rolling-upgrade.md) wordt beschreven hoe u Active Geo-replicatie gebruiken om te schakelen van rolling upgrades van de wolk toepassing uitvaltijd te minimaliseren tijdens upgrades en geef het pad herstellen in het geval er iets mis gaat. Dit artikel wordt gekeken naar twee verschillende methoden van het upgradeproces regie en de voordelen en de-en nadelen van elke optie.

## <a name="next-steps"></a>Volgende stappen

Zie voor een bespreking van toepassing overwegingen bij het ontwerpen voor zelfstandige databases en toepassingen elastisch [ontwerp van een toepassing voor noodherstel wolk](sql-database-designing-cloud-solutions-for-disaster-recovery.md) en [elastische Pool disaster recovery strategieën](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md).






