<properties
   pageTitle="Overzicht van de configuratie van Azure Service Fabric betrouwbare actoren ReliableDictionaryActorStateProvider | Microsoft Azure"
   description="Informatie over het configureren van Azure Service Fabric stateful actoren van het type ReliableDictionaryActorStateProvider."
   services="Service-Fabric"
   documentationCenter=".net"
   authors="sumukhs"
   manager="timlt"
   editor=""/>

<tags
   ms.service="Service-Fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="07/18/2016"
   ms.author="sumukhs"/>

# <a name="configuring-reliable-actors--reliabledictionaryactorstateprovider"></a>Betrouwbare actoren--ReliableDictionaryActorStateProvider configureren
U kunt de standaardconfiguratie van de ReliableDictionaryActorStateProvider wijzigen door het settings.xml bestand in de hoofdmap van de Visual Studio-pakket in de map Config gegenereerd voor de opgegeven actor.

De Azure Service Fabric-runtime gezocht naar sectienamen van vooraf gedefinieerde in het bestand settings.xml en de configuratiewaarden verbruikt tijdens het maken van de onderliggende runtimeonderdelen.

>[AZURE.NOTE] Kan **niet** verwijderen of wijzigen van de sectienamen van de volgende configuraties in het settings.xml bestand dat wordt gegenereerd in de Visual Studio-oplossing.

Er zijn algemene instellingen voor de configuratie van ReliableDictionaryActorStateProvider.

## <a name="global-configuration"></a>Globale configuratie

De globale configuratie is opgegeven in het manifest van het cluster voor het cluster in de sectie KtlLogger. Kan de configuratie van de gedeelde locatie en grootte plus de globale geheugenlimieten gebruikt door het logboek. Houd er rekening mee dat wijzigingen in het manifest van het cluster zijn van invloed op alle services die gebruikmaken van ReliableDictionaryActorStateProvider en betrouwbare stateful services.

De cluster-manifestbestand is een XML-bestand met instellingen en configuraties die van toepassing zijn op alle knooppunten en services in het cluster. Het bestand wordt meestal ClusterManifest.xml genoemd. U kunt zien het cluster voor het cluster met de opdracht Get-ServiceFabricClusterManifest powershell manifest.

### <a name="configuration-names"></a>Van configuratienamen

|Naam|Eenheid|Standaardwaarde|Opmerkingen|
|----|----|-------------|-------|
|WriteBufferMemoryPoolMinimumInKB|KB|8388608|Minimum aantal KB in kernel-modus voor het logboek schrijven buffergroep geheugen toewijzen. Dit geheugen wordt gebruikt voor het in cache opslaan van informatie over de status voordat het naar schijf schrijven.|
|WriteBufferMemoryPoolMaximumInKB|KB|Geen limiet|Maximale grootte waaraan het logboek schrijven buffer geheugen-pool kan groeien.|
|SharedLogId|GUID|""|Hiermee geeft u een unieke GUID waarmee de gedeelde standaardlogboek gebruikt door alle betrouwbare services op alle knooppunten in het cluster dat de SharedLogId niet in de configuratie van de specifieke service opgeeft. Als SharedLogId is opgegeven, worden moet SharedLogPath ook opgegeven.|
|SharedLogPath|Volledige padnaam|""|Hiermee geeft u het volledige pad waar het gedeelde bestand wordt gebruikt door alle betrouwbare services op alle knooppunten in het cluster dat de SharedLogPath niet in de configuratie van de specifieke service opgeeft. Echter, als SharedLogPath is opgegeven, moet SharedLogId ook worden opgegeven.|
|SharedLogSizeInMB|Megabytes|8192|Geeft het aantal MB schijfruimte statisch toewijzen voor het gedeelde logboek. De waarde moet 2048 of groter.|

### <a name="sample-cluster-manifest-section"></a>Voorbeeld-manifest sectie cluster
```xml
   <Section Name="KtlLogger">
     <Parameter Name="WriteBufferMemoryPoolMinimumInKB" Value="8192" />
     <Parameter Name="WriteBufferMemoryPoolMaximumInKB" Value="8192" />
     <Parameter Name="SharedLogId" Value="{7668BB54-FE9C-48ed-81AC-FF89E60ED2EF}"/>
     <Parameter Name="SharedLogPath" Value="f:\SharedLog.Log"/>
     <Parameter Name="SharedLogSizeInMB" Value="16383"/>
   </Section>
```

### <a name="remarks"></a>Opmerkingen
Het logboek is een globale groep geheugen niet-wisselbaar kernelgeheugen dat beschikbaar is voor alle betrouwbare services op een knooppunt voor het cachen van gegevens voordat de betrouwbare service replica wordt geschreven naar het logboek speciale gekoppeld is toegewezen. De grootte van de groep van toepassingen wordt bepaald door de instellingen voor WriteBufferMemoryPoolMinimumInKB en WriteBufferMemoryPoolMaximumInKB. WriteBufferMemoryPoolMinimumInKB geeft de begingrootte van dit geheugen-pool en de laagste krimpt waaraan de geheugen-pool kan. WriteBufferMemoryPoolMaximumInKB is de hoogste grootte waarmee de geheugen-pool kan groeien. Elke replica betrouwbare service die is geopend, kan de grootte van de geheugen-pool met een bepaald systeem bedrag tot WriteBufferMemoryPoolMaximumInKB verhogen. Als er meer vraag naar het geheugen van de geheugen-pool dan beschikbaar is, worden aanvragen voor geheugen uitgesteld totdat het geheugen beschikbaar is. Dus als de geheugen-pool van schrijven buffer te klein voor een bepaalde configuratie vervolgens prestaties nadelig beïnvloeden.

De instellingen voor SharedLogId en SharedLogPath worden altijd samen gebruikt voor het definiëren van de GUID en de locatie voor het gedeelde standaardlogboek voor alle knooppunten in het cluster. De standaard gedeelde logboek wordt gebruikt voor alle betrouwbare diensten die niet de instellingen in de settings.xml voor de specifieke service opgeeft. Voor de beste prestaties moeten de gedeelde bestanden op schijven die worden gebruikt voor het gedeelde bestand verkleinen bronconflicten worden geplaatst.

SharedLogSizeInMB geeft de hoeveelheid schijfruimte toe te wijzen voor het standaard gedeelde logboek op alle knooppunten.  SharedLogId en SharedLogPath hoeft niet te worden opgegeven voor SharedLogSizeInMB worden opgegeven.

## <a name="replicator-security-configuration"></a>Replicator Beveiligingsconfiguratie
Replicator beveiligingsconfiguraties worden gebruikt voor het beveiligen van de communicatiekanaal dat wordt gebruikt tijdens de replicatie. Dit betekent dat services elkaars replicatieverkeer, ervoor te zorgen dat de gegevens die in hoge mate beschikbaar wordt gesteld, is ook veiliger kunnen niet zien.
Standaard wordt een leeg beveiligingsconfiguratiesectie voorkomen dat replicatie.

### <a name="section-name"></a>De naam van sectie
&lt;ActorName&gt;ServiceReplicatorSecurityConfig

## <a name="replicator-configuration"></a>Replicatie configureren
Replicator configuraties worden gebruikt voor het configureren van replicatie die verantwoordelijk is voor de acteur staat Provider staat zeer betrouwbaar door te repliceren en de staat lokaal persistent maken.
De standaardconfiguratie wordt gegenereerd door Visual Studio-sjabloon en doorgaans voldoende. In deze sectie gesprekken voert aanvullende configuraties die beschikbaar zijn voor replicatie afstemmen.

### <a name="section-name"></a>De naam van sectie
&lt;ActorName&gt;ServiceReplicatorConfig

### <a name="configuration-names"></a>Van configuratienamen

|Naam|Eenheid|Standaardwaarde|Opmerkingen|
|----|----|-------------|-------|
|BatchAcknowledgementInterval|Seconden|0.015|De periode waarvoor de replicator op de secundaire Wacht na ontvangst van een bewerking voor het verzenden van een bevestiging terug naar de primaire. Eventuele andere bevestigingen voor activiteiten die binnen dit interval verwerkt worden verzonden als een antwoord verzonden.||
|ReplicatorEndpoint|N.V.T.|Geen standaard--vereiste parameter|IP-adres en poort die door de primaire en secundaire replicator gebruikt om te communiceren met andere distributeurs in de replica ingesteld. Dit moet verwijzen naar een bron TCP-eindpunt in het servicemanifest. Verwijzen naar [Service manifest bronnen](service-fabric-service-manifest-resources.md) voor meer informatie over het definiëren van resources eindpunt in het servicemanifest. |
|MaxReplicationMessageSize|Bytes|50 MB|Maximale grootte van de van replicatiegegevens dat in één bericht kan worden verzonden.|
|MaxPrimaryReplicationQueueSize|Aantal bewerkingen|8192|Maximum aantal bewerkingen in de primaire wachtrij. Een bewerking wordt vrijgemaakt nadat de primaire replicator een bevestiging van de secundaire replicators ontvangt. Deze waarde moet groter zijn dan 64 en een macht van 2.|
|MaxSecondaryReplicationQueueSize|Aantal bewerkingen|16384|Maximum aantal bewerkingen in de wachtrij voor secundaire. Een bewerking is na het aanbrengen van de staat maximaal beschikbaar is via de persistentie vrijgemaakt. Deze waarde moet groter zijn dan 64 en een macht van 2.|
|CheckpointThresholdInMB|MB|200|De hoeveelheid ruimte in logboekbestand waarna de status gecontroleerd wordt.|
|MaxRecordSizeInKB|KB|1024|Record maximumgrootte de replicator in het logboek schrijven kan. Deze waarde moet een veelvoud van 4 en groter dan 16.|
|OptimizeLogForLowerDiskUsage|Boole-waarde|True|Waar is, is het logboek zo geconfigureerd dat speciale logboekbestand van de replica wordt gemaakt met een verspreid bestand NTFS. Dit verlaagt het werkelijke gebruik van schijfruimte voor het bestand. Als de waarde false is, wordt het bestand gemaakt met vaste toewijzingen, dat de beste schrijfprestaties biedt.|
|SharedLogId|GUID|""|Hiermee geeft u een unieke guid waarmee het gedeelde bestand gebruikt met deze replica. Normaal gesproken moeten services niet gebruiken voor deze instelling. Echter, als SharedLogId is opgegeven, moet SharedLogPath ook worden opgegeven.|
|SharedLogPath|Volledige padnaam|""|Hiermee geeft u het volledige pad waar het gedeelde bestand voor deze replica wordt gemaakt. Normaal gesproken moeten services niet gebruiken voor deze instelling. Echter, als SharedLogPath is opgegeven, moet SharedLogId ook worden opgegeven.|


## <a name="sample-configuration-file"></a>Een voorbeeld configuratiebestand

```xml
<?xml version="1.0" encoding="utf-8"?>
<Settings xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <Section Name="MyActorServiceReplicatorConfig">
      <Parameter Name="ReplicatorEndpoint" Value="MyActorServiceReplicatorEndpoint" />
      <Parameter Name="BatchAcknowledgementInterval" Value="0.05"/>
      <Parameter Name="CheckpointThresholdInMB" Value="180" />
   </Section>
   <Section Name="MyActorServiceReplicatorSecurityConfig">
      <Parameter Name="CredentialType" Value="X509" />
      <Parameter Name="FindType" Value="FindByThumbprint" />
      <Parameter Name="FindValue" Value="9d c9 06 b1 69 dc 4f af fd 16 97 ac 78 1e 80 67 90 74 9d 2f" />
      <Parameter Name="StoreLocation" Value="LocalMachine" />
      <Parameter Name="StoreName" Value="My" />
      <Parameter Name="ProtectionLevel" Value="EncryptAndSign" />
      <Parameter Name="AllowedCommonNames" Value="My-Test-SAN1-Alice,My-Test-SAN1-Bob" />
   </Section>
</Settings>
```

## <a name="remarks"></a>Opmerkingen
De parameter BatchAcknowledgementInterval bepaalt de replicatielatentie. De waarde '0' resulteert in de laagst mogelijke latentie ten koste van de doorvoer (zoals meer acknowledgement-berichten moeten worden verzonden en verwerkt, elk met minder bevestigingen).
Hoe groter de waarde voor BatchAcknowledgementInterval, hoe hoger de totale replicatie doorvoersnelheid, ten koste van hogere wachttijd voor bewerking. Dit wordt direct omgezet in de latentie van transactiebevestigingen.

De parameter CheckpointThresholdInMB bepaalt de hoeveelheid schijfruimte die de replicator gebruiken kunt voor het opslaan van informatie over de status in speciale logboek van de replica. Dit op een hogere waarde dan de standaardwaarde verhogen kan leiden tot snellere herconfiguratie keer wanneer een nieuwe replica wordt toegevoegd aan de set. Dit komt door de gedeeltelijke staat overdracht die uitgevoerd als gevolg van de beschikbaarheid van meer geschiedenis van bewerkingen in het logboek wordt. Hierdoor kan de hersteltijd van een replica mogelijk na een crash toenemen.

Als u OptimizeForLowerDiskUsage op true instelt, ruimte in logboekbestand niet te veel ingerichte zodat actieve replica's kunnen meer informatie over de status in de logboekbestanden opslaan terwijl het inactief replica's minder schijfruimte wordt gebruikt. Hierdoor kunnen als host voor meer replica's op een knooppunt. Als u OptimizeForLowerDiskUsage op false instelt, wordt de statusinformatie sneller naar logboekbestanden geschreven.

De instelling van de MaxRecordSizeInKB bepaalt de maximale grootte van een record die door de distributeur in het logboekbestand kan worden geschreven. In de meeste gevallen is de standaard 1024 KB-record optimaal. Echter, als de service wordt veroorzaakt door grotere gegevensitems moet deel uitmaken van de informatie staat, deze waarde moet mogelijk worden verhoogd. Er is weinig voordeel bij het maken van MaxRecordSizeInKB kleiner zijn dan 1024, terwijl kleinere records met alleen de benodigde ruimte voor de record kleiner. We verwachten dat deze waarde zou moeten alleen in uitzonderlijke gevallen worden gewijzigd.

De instellingen voor SharedLogId en SharedLogPath worden altijd samen gebruikt om een service een afzonderlijke gedeelde logboek van het gedeelde standaardlogboek gebruikt voor het knooppunt. Zo veel services mogelijk moeten hetzelfde gedeelde logboek opgeven voor een optimale efficiency. Gedeelde bestanden moeten worden geplaatst op de schijven die worden gebruikt voor het gedeelde bestand bronconflicten bewegingen te verminderen. We verwachten dat deze waarden moet alleen in uitzonderlijke gevallen worden gewijzigd.
