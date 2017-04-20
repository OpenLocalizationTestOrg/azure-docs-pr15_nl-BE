<properties
   pageTitle="Bedrijfscontinuïteit cloud - een verwijderde database - SQL-Database herstellen | Microsoft Azure"
   description="Informatie over het terugzetten van Point-in-Time, waarmee u kunt terugdraaien Azure SQL-Database naar een vorig punt in tijd (maximaal 35 dagen)."
   services="sql-database"
   documentationCenter=""
   authors="stevestein"
   manager="jhubbard"
   editor="monicar"/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/01/2016"
   ms.author="sstein"/>

# <a name="recover-an-azure-sql-database-using-automated-database-backups"></a>Azure SQL-database met behulp van geautomatiseerde back-ups herstellen

SQL-Database bevat drie opties voor het herstellen van databases met behulp van [geautomatiseerde back-ups van SQL-Database](sql-database-automated-backups.md). In de [periode](sql-database-service-tiers.md) kunt u een database terugzetten vanaf back-ups-service is gestart:

- Een nieuwe database op dezelfde logische server herstellen naar een bepaald punt in de tijd binnen de bewaartermijn. 
- Een database op dezelfde logische server herstellen naar het tijdstip van verwijdering van een verwijderde database.
- Een nieuwe database op elke logische server in elke regio herstellen naar de meest recente dagelijkse back-ups in geo-gerepliceerde blob-opslag (RA-GRS).

U kunt ook [geautomatiseerde back-ups van SQL-Database](sql-database-automated-backups.md) gebruiken voor het maken van een [database kopiëren](sql-database-copy.md) op een logische servers in de regio die transactioneel in overeenstemming is met de huidige SQL-Database. U kunt database kopiëren en [exporteren naar een BACPAC](sql-database-export.md) om een transactioneel consistente kopie van een database voor de opslag voor langere termijn dan de bewaarperiode of een kopie van uw database overbrengen naar een locatie op of Azure VM exemplaar van SQL Server.

## <a name="recovery-time"></a>Hersteltijd

De hersteltijd is een database met behulp van geautomatiseerde back-ups terugzetten wordt beïnvloed door een aantal factoren: 
 - De grootte van de database
 - Het prestatieniveau van de database
 - Het aantal betrokken transactielogboeken
 - Het bedrag van de activiteit die u wilt herstellen naar het herstelpunt worden replay
 - De bandbreedte van het netwerk als het terugzetten naar een andere regio 
 - Het aantal gelijktijdige terugzetten aanvragen worden verwerkt in de doelregio. 
 
 De herstelbewerking kan enkele uren duren voor een zeer groot en/of de actieve database. Als er in de regio een langdurige stroomstoring, is het mogelijk dat er een groot aantal Geo-Restore-aanvragen worden verwerkt door andere regio's. Hierdoor kan de hersteltijd voor databases in die regio toenemen als er een groot aantal aanvragen. Het merendeel van de database terugzetten voltooid binnen 12 uur.

 Er is geen ingebouwde functionaliteit massaal terugzetten. De [Azure SQL-Database: volledig herstel van de Server](https://gallery.technet.microsoft.com/Azure-SQL-Database-Full-82941666) script is een voorbeeld van een manier van het uitvoeren van deze taak.

> [AZURE.IMPORTANT] Als u wilt herstellen met behulp van automatische back-ups, moet u lid zijn van de rol van inzender voor SQL Server in het abonnement of de eigenaar van het abonnement. U kunt herstellen met behulp van de portal voor Azure, PowerShell of de REST API. U kunt geen Transact-SQL. 

## <a name="point-in-time-restore"></a>Restore Point-In-Time

Point-In-Time herstellen kunt u een bestaande database terugzetten als een nieuwe database op een eerder tijdstip op dezelfde logische server met behulp van [geautomatiseerde back-ups van SQL-Database](sql-database-automated-backups.md). De bestaande database kan niet worden overschreven. U kunt op een eerder herstellen met behulp van de [portal Azure](sql-database-point-in-time-restore-portal.md), [PowerShell](sql-database-point-in-time-restore-powershell.md) of de [REST API](https://msdn.microsoft.com/library/azure/mt163685.aspx).

> [AZURE.SELECTOR]
- [Point-In-Time terugzetten: Azure portal](sql-database-point-in-time-restore-portal.md)
- [Restore Point-In-Time: PowerShell](sql-database-point-in-time-restore-powershell.md)

De database kan worden hersteld naar een prestatieniveau of elastische pool. U moet ervoor zorgen u beschikt over een voldoende DTU quota op de logische server of elastische pool. Houd er rekening mee dat het herstellen een nieuwe database gemaakt en dat het serviceniveau voor laag en de prestaties van de teruggezette database kan echter verschillen van de huidige status van de actieve database. Als het eenmaal is voltooid, is de teruggezette database een normale volledig toegankelijk online database tegen normale tarieven op basis van de dienstverlening laag en prestaties in rekening gebracht. U doet toeslagen niet oplopen tot het terugzetten van de database voltooid is.

In het algemeen dat u een database terugzet op een earler voor hersteldoeleinden. Wanneer u dit doet, kunt u de teruggezette database behandeld als een vervanging voor de oorspronkelijke database of gebruiken voor het ophalen van gegevens uit en werk vervolgens de oorspronkelijke database. 

- ***Nieuwe database:*** Als de teruggezette database is bedoeld als vervanging voor de oorspronkelijke database, moet u controleren of het prestatieniveau van de en/of service tier geschikt zijn en de database zo nodig geschaald. U kunt de naam van de oorspronkelijke database en vervolgens de oorspronkelijke naam met de opdracht ALTER DATABASE in T-SQL geven de teruggezette database. 
- ***Data recovery:*** Als u gegevens ophaalt uit de teruggezette database herstellen van de fout van een gebruiker of toepassing, moet u afzonderlijk schrijven en uitvoeren van welke gegevens herstel-scripts die u nodig hebt om gegevens te extraheren uit de herstelde database in de oorspronkelijke database. Hoewel de herstelbewerking lang duren kan om te voltooien, zal de database terugzetten zichtbaar zijn in de lijst in. Als u de database tijdens het herstellen verwijderen, dat de bewerking wordt geannuleerd en wordt niet aangerekend voor de database die de herstelbewerking is niet voltooid. 

Zie voor gedetailleerde informatie over het gebruik van Point-in-Time terugzetten herstellen van gebruikers- en fouten herstellen [Point-in-Time](sql-database-recovery-using-backups.md#point-in-time-restore)

## <a name="deleted-database-restore"></a>Verwijderde database terugzetten

Verwijderde database terugzetten kunt u een verwijderde database terugzetten tot het tijdstip van verwijdering van een verwijderde database op dezelfde logische server met behulp van [geautomatiseerde back-ups van SQL-Database](sql-database-automated-backups.md). 

> [AZURE.IMPORTANT] Als u een exemplaar van de server Azure SQL-Database verwijdert, worden alle van de databases worden ook verwijderd en kan niet worden hersteld. Er is geen ondersteuning voor het herstellen van een verwijderde server op dit moment.

U kunt dezelfde of een nieuwe naam voor de teruggezette database. U kunt de [portal Azure](sql-database-restore-deleted-database-portal.md) [PowerShell](sql-database-restore-deleted-database-powershell.md) of de [REST (createMode = terugzetten)](https://msdn.microsoft.com/library/azure/mt163685.aspx). 

> [AZURE.SELECTOR]
- [Verwijderde database terugzetten: Azure portal](sql-database-restore-deleted-database-portal.md)
- [Verwijderde database terugzetten: PowerShell](sql-database-restore-deleted-database-powershell.md)

## <a name="geo-restore"></a>Geo-terugzetten

Geo-terugzetten kunt u een SQL-database op elke server in elke regio Azure uit de meest recente gerepliceerd van geo- [Automatische dagelijkse back-up](sql-database-automated-backups.md)terugzetten. Geo-terugzetten een geo-redundante back-up als gegevensbron gebruikt en kan worden gebruikt om een database te herstellen, zelfs als de database of het datacenter niet toegankelijk wegens een storing is. U kunt de [portal Azure](sql-database-geo-restore-portal.md) [PowerShell](sql-database-geo-restore-powershell.md), of de [REST (createMode = herstel)](https://msdn.microsoft.com/library/azure/mt163685.aspx) 

> [AZURE.SELECTOR]
- [Geo-terugzetten: Azure portal](sql-database-geo-restore-portal.md)
- [Geo-terugzetten: PowerShell](sql-database-geo-restore-powershell.md)

Geo-terugzetten is de standaardhersteloptie wanneer de database niet beschikbaar is vanwege een incident in de regio waar de database wordt gehost. Als een grootschalig incident in een regio worden onbeschikbaarheid van de databasetoepassing, kunt u Geo terugzetten naar een database van de meest recente back-up terugzetten op een server in een andere regio. Alle back-ups zijn geo gerepliceerd en kan een vertraging tussen wanneer de back-up genomen en geo-gerepliceerd naar de Azure is blob in een andere regio. Deze vertraging zijn een uur lang een storing kunnen er van 1 uur gegevens verloren gaan, dat wil zeggen, een vrijgegeven Productieorder van maximaal 1 uur. Hieronder ziet u het terugzetten van de database van de laatste dagelijkse back-up.

![geo-terugzetten](./media/sql-database-geo-restore/geo-restore-2.png)

Zie voor gedetailleerde informatie over het gebruik van Geo-terugzetten herstellen na een storing [herstellen na een stroomstoring](sql-database-disaster-recovery.md)

> [AZURE.IMPORTANT] Geo terugzetten op alle Servicelagen beschikbaar is, is het meest elementaire van de disaster recovery oplossingen beschikbaar in de SQL-Database met de langste vrijgegeven Productieorder en raming herstel tijd (invoegen). Voor eenvoudige databases met een maximumgrootte van 2 GB Geo-Restore biedt een redelijke DR-oplossing met een invoegen van 12 uur. Voor grotere databases voor Standard of Premium aanzienlijk kortere tijden voor herstel gewenst zijn of u verkleint de kans op verlies van gegevens moet u rekening houden met behulp van actieve Geo-replicatie. Actieve Geo-replicatie biedt een veel lagere vrijgegeven Productieorder en invoegen als u alleen hoeft een failover-aan een voortdurend gerepliceerde secundair. Zie [Actieve Geo-replicatie](sql-database-geo-replication-overview.md)voor meer informatie.

## <a name="programmatically-performing-recovery-using-automated-backups"></a>Via een programma uitvoeren met behulp van automatische back-ups terugzetten

Zoals hierboven besproken in addiition de Azure Portal kan databaseherstel worden uitgevoerd wordt met Azure PowerShell en de REST-API. De onderstaande tabellen beschrijven de set opdrachten die beschikbaar zijn.

### <a name="powershell"></a>PowerShell

|Cmdlet|Beschrijving|
|------|-----------|
|[Get-AzureRmSqlDatabase](https://msdn.microsoft.com/en-us/library/azure/mt603648.aspx)|Met deze eigenschap haalt een of meer databases.|
|[Get-AzureRMSqlDeletedDatabaseBackup](https://msdn.microsoft.com/en-us/library/azure/mt693387.aspx)|Deze eigenschap haalt een verwijderde database die u kunt terugzetten.|
|[Get-AzureRmSqlDatabaseGeoBackup](https://msdn.microsoft.com/library/azure/mt693388.aspx)|Deze eigenschap haalt een geo-redundante back-up van een database.|
|[Herstel AzureRmSqlDatabase](https://msdn.microsoft.com/library/azure/mt693390.aspx)|Hiermee herstelt u een SQL-database.|
||||

### <a name="rest-api"></a>REST-API

|API|Beschrijving|
|---|-----------|
|[REST (createMode = herstel)](https://msdn.microsoft.com/library/azure/mt163685.aspx)|Hiermee herstelt u een database|
|[Get maken of bijwerken van de databasestatus](https://msdn.microsoft.com/library/azure/mt643934.aspx)|Deze eigenschap retourneert de status tijdens een bewerking voor terugzetten|
||||



## <a name="summary"></a>Samenvatting

Automatische back-ups beveiligen uw databases van de gebruiker en toepassingsfouten, per ongeluk database verwijderen en langdurige storingen. Deze oplossing met nul-kosten-nul-beheerder is beschikbaar voor alle SQL-databases. 

## <a name="next-steps"></a>Volgende stappen

- Zie voor een overzicht van business continuity en scenario's [Business continuity-overzicht](sql-database-business-continuity.md)
- Zie informatie over Azure SQL-Database automatisch back-ups, [geautomatiseerde back-ups van SQL-Database](sql-database-automated-backups.md)
- Zie voor meer informatie over herstelopties voor sneller [Actief-Geo-replicatie](sql-database-geo-replication-overview.md)  
- Zie voor meer informatie over het gebruik van geautomatiseerde back-ups voor het archiveren van [kopie](sql-database-copy.md)
