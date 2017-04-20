<properties
   pageTitle="Overzicht van de configuratie van Azure Service Fabric betrouwbare actoren KVSActorStateProvider | Microsoft Azure"
   description="Informatie over het configureren van Azure Service Fabric stateful actoren van het type KVSActorStateProvider."
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
   ms.date="09/20/2016"
   ms.author="sumukhs"/>

# <a name="configuring-reliable-actors--kvsactorstateprovider"></a>Betrouwbare actoren--KVSActorStateProvider configureren
U kunt de standaardconfiguratie van de KVSActorStateProvider wijzigen door het settings.xml bestand in de hoofdmap van de Microsoft Visual Studio-pakket in de map Config wordt gegenereerd voor de opgegeven actor.

De Azure Service Fabric-runtime gezocht naar sectienamen van vooraf gedefinieerde in het bestand settings.xml en de configuratiewaarden verbruikt tijdens het maken van de onderliggende runtimeonderdelen.

>[AZURE.NOTE] Kan **niet** verwijderen of wijzigen van de sectienamen van de volgende configuraties in het settings.xml bestand dat wordt gegenereerd in de Visual Studio-oplossing.

## <a name="replicator-security-configuration"></a>Replicator Beveiligingsconfiguratie
Replicator beveiligingsconfiguraties worden gebruikt voor het beveiligen van de communicatiekanaal dat wordt gebruikt tijdens de replicatie. Dit betekent dat services elkaars replicatieverkeer, ervoor te zorgen dat de gegevens die in hoge mate beschikbaar wordt gesteld is ook veiliger kunnen niet zien.
Standaard wordt een leeg beveiligingsconfiguratiesectie voorkomen dat replicatie.

### <a name="section-name"></a>De naam van sectie
&lt;ActorName&gt;ServiceReplicatorSecurityConfig

## <a name="replicator-configuration"></a>Replicatie configureren
Replicator configuraties configureren de replicator die verantwoordelijk is voor de acteur staat Provider staat zeer betrouwbaar.
De standaardconfiguratie wordt gegenereerd door Visual Studio-sjabloon en doorgaans voldoende. In deze sectie gesprekken voert aanvullende configuraties die beschikbaar zijn voor replicatie afstemmen.

### <a name="section-name"></a>De naam van sectie
&lt;ActorName&gt;ServiceReplicatorConfig

### <a name="configuration-names"></a>Van configuratienamen

|Naam|Eenheid|Standaardwaarde|Opmerkingen|
|----|----|-------------|-------|
|BatchAcknowledgementInterval|Seconden|0.015|De periode waarvoor de replicator op de secundaire Wacht na ontvangst van een bewerking voor het verzenden van een bevestiging terug naar de primaire. Eventuele andere bevestigingen voor activiteiten die binnen dit interval verwerkt worden verzonden als een antwoord verzonden.|
|ReplicatorEndpoint|N.V.T.|Geen standaard--vereiste parameter|IP-adres en poort die door de primaire en secundaire replicator gebruikt om te communiceren met andere distributeurs in de replica ingesteld. Dit moet verwijzen naar een bron TCP-eindpunt in het servicemanifest. Verwijzen naar [Service manifest bronnen](service-fabric-service-manifest-resources.md) voor meer informatie over het definiëren van resources eindpunt in het servicemanifest. |
|RetryInterval|Seconden|5|De periode waarna de replicator opnieuw een bericht verzendt als deze niet een bevestiging voor een bewerking ontvangt.|
|MaxReplicationMessageSize|Bytes|50 MB|Maximale grootte van de van replicatiegegevens dat in één bericht kan worden verzonden.|
|MaxPrimaryReplicationQueueSize|Aantal bewerkingen|1024|Maximum aantal bewerkingen in de primaire wachtrij. Een bewerking wordt vrijgemaakt nadat de primaire replicator een bevestiging van de secundaire replicators ontvangt. Deze waarde moet groter zijn dan 64 en een macht van 2.|
|MaxSecondaryReplicationQueueSize|Aantal bewerkingen|2048|Maximum aantal bewerkingen in de wachtrij voor secundaire. Een bewerking is na het aanbrengen van de staat maximaal beschikbaar is via de persistentie vrijgemaakt. Deze waarde moet groter zijn dan 64 en een macht van 2.|

## <a name="store-configuration"></a>Configuratie opslaan
Archief-configuraties worden gebruikt voor het configureren van het lokale archief dat wordt gebruikt om de status die wordt gerepliceerd.
De standaardconfiguratie wordt gegenereerd door Visual Studio-sjabloon en doorgaans voldoende. In deze sectie gesprekken voert aanvullende configuraties die beschikbaar zijn voor het afstemmen van het lokale archief.

### <a name="section-name"></a>De naam van sectie
&lt;ActorName&gt;ServiceLocalStoreConfig

### <a name="configuration-names"></a>Van configuratienamen

|Naam|Eenheid|Standaardwaarde|Opmerkingen|
|----|----|-------------|-------|
|MaxAsyncCommitDelayInMilliseconds|Milliseconden|200|Hiermee stelt u het maximale interval voor duurzame lokale archief doorvoeracties batchen.|
|MaxVerPages|Aantal pagina 's|16384|Het maximum aantal pagina's de versie in de lokale database opslaan. Bepaalt het maximum aantal uitstaande transacties.|

## <a name="sample-configuration-file"></a>Een voorbeeld configuratiebestand

```xml
<?xml version="1.0" encoding="utf-8"?>
<Settings xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <Section Name="MyActorServiceReplicatorConfig">
      <Parameter Name="ReplicatorEndpoint" Value="MyActorServiceReplicatorEndpoint" />
      <Parameter Name="BatchAcknowledgementInterval" Value="0.05"/>
   </Section>
   <Section Name="MyActorServiceLocalStoreConfig">
      <Parameter Name="MaxVerPages" Value="8192" />
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
