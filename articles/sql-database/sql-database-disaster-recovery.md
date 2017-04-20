<properties
   pageTitle="Noodherstel van SQL-Database | Microsoft Azure"
   description="Informatie over het herstellen van een database van een storing in regionale datacenter of storing Geo-herstel-mogelijkheden en de Azure SQL-Database actief Geo-replicatie."
   services="sql-database"
   documentationCenter=""
   authors="CarlRabeler"
   manager="jhubbard"
   editor="monicar"/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/13/2016"
   ms.author="carlrab"/>

# <a name="restore-an-azure-sql-database-or-failover-to-a-secondary"></a>Een Azure SQL-Database of een failover herstellen naar een secundair

Azure SQL-Database biedt de volgende mogelijkheden voor het herstellen van een storing:

- [Actieve Geo-replicatie](sql-database-geo-replication-overview.md)
- [Geo-terugzetten](sql-database-recovery-using-backups.md#point-in-time-restore)

Zie meer informatie over business continuity scenario's en de functies ondersteunende deze scenario's, [bedrijfscontinuïteit](sql-database-business-continuity.md).

### <a name="prepare-for-the-event-of-an-outage"></a>Voorbereiden voor de gebeurtenis van een stroomstoring

Succes met herstel naar een andere data regio met actieve Geo-replicatie of geo-redundante back-ups, moet u voor het voorbereiden van een server in een ander datacenter uitvalt, worden de nieuwe primaire server moet de noodzaak ontstaan, alsmede ook stappen beschreven en getest op een vlotte herstel hebt gedefinieerd. Deze voorbereidende stappen omvatten:

- De logische server in een ander gebied te worden van de nieuwe primaire server herkennen. Met actieve Geo-replicatie, is dit ten minste één en misschien wel een van de secundaire servers. Voor Geo-Restore is dit in het algemeen een server in het [gekoppelde gebied](../best-practices-availability-paired-regions.md) voor de regio waarin de database zich bevindt.
- Identificeren en eventueel, het niveau van de server firewallregels definiëren op voor gebruikers die toegang tot de nieuwe primaire database nodig.
- Bepaal hoe u gaat om gebruikers te leiden naar de nieuwe primaire server, bijvoorbeeld door tekenreeksen of door het wijzigen van de DNS-vermeldingen.
- Identificeren en eventueel maken van de aanmeldingen die moeten aanwezig zijn in de hoofddatabase op de nieuwe primaire server, en er zeker van deze aanmeldingen juiste machtigingen in de master-database, indien aanwezig. Zie [SQL-Database beveiliging na noodherstel](sql-database-geo-replication-security-config.md) voor meer informatie.
- Waarschuwingsregels die moeten worden bijgewerkt als u wilt toewijzen aan de nieuwe primaire database identificeren.
- Documenteer de configuratie controleren op de huidige primaire database
- Een [Noodherstel analyse](sql-database-disaster-recovery-drills.md)worden uitgevoerd. U kunt u een stroomstoring simuleren voor Geo terugzetten, verwijderen of wijzig de naam van de brondatabase als u de connectiviteit toepassingsfout. Als u wilt simuleren van een storing voor actieve Geo-replicatie, kunt u de web-toepassing of virtuele machine is verbonden met de database of de database failover zodat toepassingsfouten connectity uitschakelen.

## <a name="when-to-initiate-recovery"></a>Wanneer Systeemherstel starten

De toepassing heeft gevolgen voor de herstelbewerking. Het wijzigen van de SQL-verbindingsreeks of omleiding via DNS vereist en kan leiden tot permanent verlies van gegevens. Dus moet het gebeuren als de onderbreking is waarschijnlijk langer dan van uw toepassing herstel tijd doelstelling. Wanneer de toepassing wordt gedistribueerd naar productie moet u regelmatige controle van de gezondheid van de toepassing en gebruik de volgende gegevenspunten als het bevestigt dat het herstel is gegrond:

1.  Permanente verbinding fout van de application-laag aan de database.
2.  De Azure portal bevat een waarschuwing over een incident in het gebied met een grote impact.
3.  De server Azure SQL-Database is gemarkeerd als verslechterd.

Afhankelijk van uw toepassing tolerantie uitvaltijd en mogelijke aansprakelijkheid van business kunt u rekening houden met de volgende opties voor Systeemherstel.

[Herstelbare Database ophalen](https://msdn.microsoft.com/library/dn800985.aspx) (*LastAvailableBackupDate*) gebruiken om het meest recente herstelpunt voor Geo gerepliceerd.

## <a name="wait-for-service-recovery"></a>Wachten op recovery service

Het werk Azure teams naar eer en geweten te herstellen van de beschikbaarheid van de service als snel mogelijk, maar afhankelijk van de hoofdmap, wordt deze duurt uren of dagen.  Als uw toepassing mogelijk zonder aanzienlijke uitvaltijd die u gewoon tot het herstellen wachten kunt te voltooien. In dat geval is geen actie van uw kant vereist. De huidige servicestatus kunt u bekijken op onze [Azure Service Health Dashboard](https://azure.microsoft.com/status/). Na het herstel van de regio worden, de beschikbaarheid van uw toepassing teruggezet.

## <a name="failover-to-geo-replicated-secondary-database"></a>Failover naar secundaire database gerepliceerd geo

Als de uitvaltijd van uw toepassing leiden aansprakelijkheid van business tot kan moet u geo-gerepliceerde database (s) in uw toepassing. Het kan de toepassing de beschikbaarheid in een andere regio in het geval van een storing snel herstellen. Meer informatie over het [configureren van Geo-replicatie](sql-database-geo-replication-portal.md).

Beschikbaarheid van de database (s) die u nodig hebt om de failover naar de secundaire geo gerepliceerd met behulp van een van de ondersteunde methoden herstellen.

Gebruik een van de volgende gidsen voor failover naar een secundaire geo gerepliceerd-database:

- [Failover naar een secundaire geo gerepliceerd met Azure Portal](sql-database-geo-replication-portal.md)
- [Failover naar een secundaire geo gerepliceerd met PowerShell](sql-database-geo-replication-powershell.md)
- [Failover naar een secundaire geo gerepliceerd met T-SQL](sql-database-geo-replication-transact-sql.md)

## <a name="recover-using-geo-restore"></a>Met Geo terugzetten herstellen

Als uitvaltijd van uw toepassing niet leidt aansprakelijkheid business tot kunt u Geo terugzetten als een methode om te herstellen van uw toepassing database (s). Een kopie van de database wordt gemaakt van de meest recente geo-redundante back-up.

Gebruik een van de volgende handleidingen geo terugzetten een database in een nieuw gebied:

- [Geo-terugzetten van een database naar een nieuw gebied met Azure Portal](sql-database-geo-restore-portal.md)
- [Geo-terugzetten van een database naar een nieuw gebied met PowerShell](sql-database-geo-restore-powershell.md)

## <a name="configure-your-database-after-recovery"></a>De database configureren na herstel

Als u gebruikmaakt van geo-replicatie-failover of geo terugzetten herstellen na een stroomstoring, moet u ervoor zorgen dat de verbinding met de nieuwe databases juist is geconfigureerd, zodat de functie van de normale toepassing kan worden hervat. Dit is een controle van de taken voor de herstelde database productie gereed.

### <a name="update-connection-strings"></a>Verbindingsreeksen bijwerken

Omdat de herstelde database bevinden zich in een andere server, moet u de verbindingsreeks van de toepassing om te verwijzen naar die server bijwerken.

Zie voor meer informatie over het wijzigen van tekenreeksen voor de juiste taal voor uw [Verbindingsbibliotheek](sql-database-libraries.md).

### <a name="configure-firewall-rules"></a>Firewallregels configureren

U moet ervoor zorgen dat de firewall-regels geconfigureerd op de server en de database overeenkomen met de waarden die zijn geconfigureerd op de primaire server en de primaire database. Zie voor meer informatie [hoe: Firewall-instellingen configureren (Azure SQL Database)](sql-database-configure-firewall-settings.md).


### <a name="configure-logins-and-database-users"></a>Configureren, aanmeldingen en Database-gebruikers

U moet ervoor zorgen dat alle aanmeldingen door de toepassing gebruikt op de server die als de herstelde database host fungeert bestaat. Zie [Beveiligingsconfiguratie voor Geo-replicatie](sql-database-geo-replication-security-config.md)voor meer informatie.

>[AZURE.NOTE] U moet configureren en testen van uw server firewall-regels en -aanmeldingen (en hun machtigingen) tijdens een detailanalyse disaster recovery. Deze objecten van het niveau van de server en de bijbehorende configuratie mogelijk niet beschikbaar tijdens de onderbreking.

### <a name="setup-telemetry-alerts"></a>Telemetrie waarschuwingen instellen

U moet ervoor zorgen dat de bestaande instellingen voor waarschuwingsregel worden toegewezen aan de herstelde database en de andere server worden bijgewerkt.

Zie voor meer informatie over database waarschuwingsregels [Waarschuwing meldingen ontvangt](../monitoring-and-diagnostics/insights-receive-alert-notifications.md) en de [Gezondheid van spoor-Service](../monitoring-and-diagnostics/insights-service-health.md).

### <a name="enable-auditing"></a>Controle inschakelen

Als controle is vereist voor toegang tot uw database, moet u de controle inschakelen na het herstellen van databases. Een goede indicator dat controle vereist is, is dat clienttoepassingen in een patroon van tekenreeksen voor beveiligde verbinding gebruiken *. database.secure.windows.net. Zie [aan de slag met SQL-database controleren](sql-database-auditing-get-started.md)voor meer informatie.


## <a name="next-steps"></a>Volgende stappen

- Zie informatie over Azure SQL-Database automatisch back-ups, [geautomatiseerde back-ups van SQL-Database](sql-database-automated-backups.md)
- Zie voor meer informatie over business continuity ontwerp en herstellen scenario's [continuïteit scenario 's](sql-database-business-continuity.md)
- Zie informatie over automatische back-ups gebruikt voor herstel, [een database van de service gestart back-ups terugzetten](sql-database-recovery-using-backups.md)
