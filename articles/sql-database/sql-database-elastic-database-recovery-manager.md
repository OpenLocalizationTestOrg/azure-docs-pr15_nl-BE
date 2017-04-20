<properties 
    pageTitle="Manager voor herstel op te lossen problemen met de netwerkplattegrond shard met | Microsoft Azure" 
    description="De klasse RecoveryManager gebruiken bij het oplossen van problemen met shard kaarten" 
    services="sql-database" 
    documentationCenter=""  
    manager="jhubbard"
    authors="ddove"/>

<tags 
    ms.service="sql-database" 
    ms.workload="sql-database" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="05/05/2016" 
    ms.author="ddove"/>

# <a name="using-the-recoverymanager-class-to-fix-shard-map-problems"></a>Met behulp van de klasse RecoveryManager shard kaart problemen op te lossen

De klasse [RecoveryManager](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.recovery.recoverymanager.aspx) kunt ADO.Net toepassingen gemakkelijk opsporen en corrigeren van inconsistenties tussen de globale shard kaart (GSM) en de lokale shard kaart (LSM) in een database een laptopgeheugen milieu. 

De GSM en LSM bijhouden van de toewijzing van elke database in een omgeving met een laptopgeheugen. In sommige gevallen wordt een einde ingevoegd tussen de GSM en de LSM. In dat geval gebruikt u de klasse RecoveryManager te detecteren en herstellen van de pauze.

De klasse RecoveryManager is onderdeel van de [elastische Database client library](sql-database-elastic-database-client-library.md). 


![Shard kaart][1]


Zie [elastische Database extra woordenlijst](sql-database-elastic-scale-glossary.md)voor definities van de term. Om te begrijpen hoe de **ShardMapManager** wordt gebruikt voor het beheren van gegevens in een oplossing voor een laptopgeheugen, Zie [Shard beheer toewijzen](sql-database-elastic-scale-shard-map-management.md).


## <a name="why-use-the-recovery-manager"></a>Waarom gebruikt u de herstel-manager?

In een databaseomgeving met een laptopgeheugen is een huurder per database en veel databases per server. Er kan ook worden veel servers in de omgeving. Elke database is toegewezen in de map shard zodat gesprekken kunnen worden doorgestuurd naar de juiste server en database. Databases worden bijgehouden volgens een **sharding sleutel**en elke shard een **reeks waarden**wordt toegewezen. Een sleutel sharding kan vertegenwoordigen bijvoorbeeld de namen van klanten van "D" voor "F." De toewijzing van alle shards (aka databases) en het bereiken van hun toewijzing zijn opgenomen in de **globale shard kaart (GSM)**. Elke database bevat ook een overzicht van de bereiken die zich op de shard, dit staat bekend als de **lokale shard toewijzen (LSM)**. Wanneer de toepassing een verbinding maakt met een shard, de toewijzing wordt in de cache opgeslagen met de app voor het snel ophalen. De LSM wordt gebruikt voor het valideren van gegevens in de cache. 

De GSM en LSM niet synchroon om de volgende redenen:

1. Het verwijderen van een shard waarvan bereik gaat ervan uit dat niet langer in gebruik of het hernoemen van een shard. Een shard resulteert in een **zwevende shard toewijzing**verwijderen. Similary, een gewijzigde database kan leiden tot een zwevende shard toewijzing. Afhankelijk van de bedoeling van de wijziging, de shard moet worden verwijderd of de locatie shard moet worden bijgewerkt. Als u wilt een verwijderde database terugzetten, Zie [herstellen van een database naar een vorig punt in de tijd, een verwijderde database terugzetten of herstellen na een stroomstoring gegevens-center](sql-database-troubleshoot-backup-and-restore.md).
2. Een gebeurtenis geo-failover plaatsvindt. Om door te gaan, moet een servernaam en een databasenaam van shard kaart manager in de toepassing bijwerken en werk vervolgens de details van de shard voor alle shards in een map shard. In het geval van een geo-failover moet dergelijke herstel logica binnen de failover-workflow worden geautomatiseerd. Herstelacties automatiseren kan een frictionless beheersbaarheid voor geo-geschikte databases en vermijdt u handmatige menselijk handelen.
3. Een shard of de ShardMapManager database is teruggezet naar een eerder punt in het verleden.

Voor meer informatie over Azure SQL Database elastische databasehulpmiddelen, Geo-replicatie en herstellen, raadpleegt u het volgende: 

* [Overzicht: Cloud business continuity en database noodherstel met SQL-Database](sql-database-business-continuity.md) 
* [Aan de slag met elastische Databasehulpmiddelen](sql-database-elastic-scale-get-started.md)  
* [ShardMap beheer](sql-database-elastic-scale-shard-map-management.md)

## <a name="retrieving-recoverymanager-from-a-shardmapmanager"></a>Ophalen van RecoveryManager van een ShardMapManager 

De eerste stap is een exemplaar van de RecoveryManager te maken. De [methode GetRecoveryManager](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.getrecoverymanager.aspx) retourneert de manager herstellen voor het huidige exemplaar van [ShardMapManager](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.aspx) . Om eventuele inconsistenties in de shard-kaart, moet u eerst de RecoveryManager voor de toewijzing bepaalde shard ophalen. 

    ShardMapManager smm = ShardMapManagerFactory.GetSqlShardMapManager(smmConnnectionString,  
             ShardMapManagerLoadPolicy.Lazy);
             RecoveryManager rm = smm.GetRecoveryManager(); 

In dit voorbeeld wordt de RecoveryManager van de ShardMapManager geïnitialiseerd. De ShardMapManager met een ShardMap is ook al geïnitialiseerd. 

Aangezien deze toepassingscode de shard kaart zelf manipuleert, moet de referenties die worden gebruikt in de fabrieksmethode (in het bovenstaande voorbeeld, smmConnectionString) zijn referenties met alleen-lezen machtigingen voor de GSM-database waarnaar wordt verwezen door de verbindingsreeks. Deze referenties zijn meestal verschillen van de referenties die worden gebruikt voor het openen van verbindingen voor het gegevensafhankelijke routering. Zie [werken met referenties in de elastische databaseclient](sql-database-elastic-scale-manage-credentials.md)voor meer informatie.

## <a name="removing-a-shard-from-the-shardmap-after-a-shard-is-deleted"></a>Een shard verwijderen uit de ShardMap nadat een shard is verwijderd.

De [methode DetachShard](https://msdn.microsoft.com/library/azure/dn842083.aspx) wordt losgekoppeld van de opgegeven shard uit de toewijzing shard en toewijzingen die zijn gekoppeld aan de shard verwijderd.  

* De locatie is de locatie shard, speciaal server en databasenaam, van de shard wordt losgekoppeld. 
* De parameter shardMapName is de naam van de map shard. Dit is alleen vereist wanneer meerdere shard toewijzingen worden beheerd door dezelfde shard kaart manager. Dit is optioneel. 

**Belangrijk**: Gebruik deze techniek alleen als u zeker weet dat het bereik voor de bijgewerkte toewijzing leeg is. De bovenstaande methoden controleren niet voor het bereik wordt verplaatst, is het verstandig om de controles in de code opnemen.

In het volgende voorbeeld wordt shards verwijderd uit de map shard. 

    rm.DetachShard(s.Location, customerMap); 

De toewijzing van de locatie shard in de GSM voor het verwijderen van de shard. Omdat de shard is verwijderd, wordt ervan uitgegaan dat opzettelijk was en het sharding sleutel bereik is niet langer in gebruik. Als dit niet het geval is, kunt u herstellen punt in tijd uitvoeren. met de shard herstellen vanaf een eerder punt in tijd. (In dat geval Lees de sectie hieronder voor het detecteren van inconsistenties shard.) Als u wilt herstellen, Zie [herstellen van een database naar een vorig punt in de tijd, een verwijderde database terugzetten of herstellen na een stroomstoring gegevens-center](sql-database-troubleshoot-backup-and-restore.md).

Aangezien ervan uitgegaan dat de database verwijdering geen vergissing was dat wordt, is de uiteindelijke administratieve opruimen actie verwijdert u de vermelding voor de shard in de map shard manager. Dit voorkomt dat de toepassing per ongeluk gegevens schrijven naar een bereik dat wordt niet verwacht.

## <a name="to-detect-mapping-differences"></a>Voor het detecteren van toewijzing verschillen 

De [methode DetectMappingDifferences](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.recovery.recoverymanager.detectmappingdifferences.aspx) wordt geselecteerd, een van de kaarten shard (lokaal of globaal) als resultaat gegeven als de bron van de waarheid en sluitend wordt gemaakt van de toewijzingen op beide kaarten shard (GSM en LSM).

    rm.DetectMappingDifferences(location, shardMapName);

* De *locatie* geeft de naam van de server en de databasenaam. 
* De parameter *shardMapName* is de naam van de map shard. Dit is alleen vereist als shard met meerdere hyperlinks worden beheerd door dezelfde shard kaart manager. Dit is optioneel. 

## <a name="to-resolve-mapping-differences"></a>Toewijzing verschillen oplossen

De [methode ResolveMappingDifferences](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.recovery.recoverymanager.resolvemappingdifferences.aspx) een van de kaarten shard (lokaal of globaal) selecteert als bron van waarheid en toewijzingen op beide kaarten shard (GSM en LSM) bijgewerkt.

    ResolveMappingDifferences (RecoveryToken, MappingDifferenceResolution.KeepShardMapping);
   
* De parameter *RecoveryToken* inventariseert de verschillen in de toewijzingen tussen de GSM en de LSM voor de specifieke shard. 

* De [opsomming van MappingDifferenceResolution](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.recovery.mappingdifferenceresolution.aspx) wordt gebruikt om aan te geven van de methode voor het oplossen van het verschil tussen de toewijzingen shard. 
* **MappingDifferenceResolution.KeepShardMapping** wordt aanbevolen in het geval dat de LSM de nauwkeurige toewijzing bevat en daarom de toewijzing in de shard moet worden gebruikt. Dit is meestal het geval bij een failover: de shard zich nu bevindt op een nieuwe server. Aangezien de shard moet eerst worden verwijderd uit de GSM (met de methode RecoveryManager.DetachShard), bestaat een toewijzing niet meer op de GSM. Daarom de de LSM moet worden gebruikt voor de toewijzing shard opnieuw in te stellen.

## <a name="attach-a-shard-to-the-shardmap-after-a-shard-is-restored"></a>Een shard koppelen aan de ShardMap nadat een shard is hersteld. 

De [methode AttachShard](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.recovery.recoverymanager.attachshard.aspx) koppelt de opgegeven shard aan de kaart shard. Eventuele inconsistenties shard kaart gedetecteerd en het overeenkomt met de shard op het moment van de herstelbewerking shard toewijzingen worden bijgewerkt. Er wordt vanuit gegaan dat de database is ook de naam gewijzigd zodat de naam van de oorspronkelijke database (voor wanneer de shard is teruggezet), sinds het punt in tijd standaardinstellingen terugzetten naar een nieuwe database met de tijdstempel toegevoegd. 

    rm.AttachShard(location, shardMapName) 

* De *locatie* is de naam van de server en database, van de shard wordt gekoppeld. 

* De parameter *shardMapName* is de naam van de map shard. Dit is alleen vereist wanneer meerdere shard toewijzingen worden beheerd door dezelfde shard kaart manager. Dit is optioneel. 

In dit voorbeeld wordt een shard toegevoegd aan de shard kaart die het laatst is teruggezet vanaf een eerder punt in tijd. Aangezien de shard (namelijk de toewijzing voor de shard in de LSM) is hersteld, is het mogelijk niet consistent met de vermelding shard in de GSM. Buiten deze voorbeeldcode werd de shard hersteld en gewijzigd in de oorspronkelijke naam van de database. Nadat dit is hersteld, wordt uitgegaan van dat de toewijzing in de LSM is de vertrouwde toewijzing. 

    rm.AttachShard(s.Location, customerMap); 
    var gs = rm.DetectMappingDifferences(s.Location); 
      foreach (RecoveryToken g in gs) 
       { 
       rm.ResolveMappingDifferences(g, MappingDifferenceResolution.KeepShardMapping); 
       } 

## <a name="updating-shard-locations-after-a-geo-failover-restore-of-the-shards"></a>Shard locaties worden bijgewerkt na een geo-failover (terugzetten) van de shards

Bij een geo-failover de secundaire database is schrijven toegankelijk gemaakt en wordt de nieuwe primaire database. De naam van de server, en eventueel de database (afhankelijk van uw configuratie), kan afwijken van de oorspronkelijke primaire. De toewijzingen voor de shard in de GSM en LSM moeten derhalve worden vastgesteld. Als de database is teruggezet naar een andere naam of locatie of op een eerder tijdstip, kan dit inconsistenties veroorzaken in de shard toewijzingen. De Map Shard Manager zorgt voor de verdeling van de open verbinding met de juiste database. Distributie is gebaseerd op de gegevens in de map shard en de waarde van de sleutel sharding dat het doel van de aanvraag van de toepassing. Na een failover geo, moet deze informatie worden bijgewerkt met de juiste servernaam, de naam en de toewijzing van de herstelde database shard. 

## <a name="best-practices"></a>Aanbevolen procedures

Geo-failover en herstel zijn bewerkingen die gewoonlijk beheerd door een beheerder van een wolk van de toepassing die opzettelijk gebruik van een van de functies voor zakelijke continuïteit Azure SQL-Databases. Business continuity planning vereist processen, procedures en maatregelen om ervoor te zorgen dat de bedrijfsactiviteiten zonder onderbreking kunnen blijven. De beschikbare methoden als onderdeel van de klasse RecoveryManager binnen deze werkstroom moet worden gebruikt om ervoor te zorgen dat de GSM en LSM worden bijgewerkt op basis van de herstelactie genomen. Er zijn 5 basisstappen voor het juist ervoor te zorgen dat de GSM en LSM weerspiegelen de accurate informatie na een failover-gebeurtenis. De toepassingscode voor het uitvoeren van deze stappen kan worden geïntegreerd in bestaande hulpprogramma's en workflow. 

1. De RecoveryManager van de ShardMapManager ophalen. 
2. De oude shard uit de toewijzing shard loskoppelen.
3. De nieuwe shard koppelen aan de kaart shard, met inbegrip van de nieuwe locatie van shard.
4. Inconsistenties in de toewijzing van de GSM LSM detecteren. 
5. Verschillen tussen de GSM en de LSM, vertrouwen de LSM oplossen. 

In dit voorbeeld voert de volgende stappen uit:
1. Shards verwijdert uit de Map Shard die aan shard locaties voor de failover-gebeurtenis.
2. Shards koppelt aan de Shard kaart als gevolg van de nieuwe shard locaties (de parameter 'Configuration.SecondaryServer' is de naam van de nieuwe server maar dezelfde naam van de database).
3. De tokens herstel doordat toewijzing verschillen tussen de GSM en de LSM voor elke shard opgehaald. 
4. De inconsistenties opgelost als u de toewijzing van de LSM van elke shard. 

    var shards = smm. GetShards();  foreach (shard s in shards) {als (s.Location.Server == Configuration.PrimaryServer) {ShardLocation slNew = nieuwe ShardLocation (Configuration.SecondaryServer, s.Location.Database); 
        
          rm.DetachShard(s.Location); 
        
          rm.AttachShard(slNew); 
        
          var gs = rm.DetectMappingDifferences(slNew); 
    
          foreach (RecoveryToken g in gs) 
            { 
               rm.ResolveMappingDifferences(g, MappingDifferenceResolution.KeepShardMapping); 
            } 
        } 
    } 



[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]


<!--Image references-->
[1]: ./media/sql-database-elastic-database-recovery-manager/recovery-manager.png
 
