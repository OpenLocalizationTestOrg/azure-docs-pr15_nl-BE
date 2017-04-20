
<properties
   pageTitle="Upgrade van de toepassing: upgrade parameters | Microsoft Azure"
   description="Beschrijving van parameters die betrekking hebben op het upgraden van een toepassing Service weefsel, met inbegrip van controles uit te voeren en het beleid om de upgrade automatisch ongedaan te maken."
   services="service-fabric"
   documentationCenter=".net"
   authors="mani-ramaswamy"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/14/2016"
   ms.author="subramar"/>



# <a name="application-upgrade-parameters"></a>Parameters voor de toepassing bijwerken

Dit artikel beschrijft de verschillende parameters die van toepassing zijn tijdens het upgraden van een toepassing Azure Service Fabric. De parameters omvatten de naam en het versienummer van de toepassing. Knoppen waarmee de time-outs en de controles die worden toegepast tijdens de upgrade zijn en zij geven het beleid dat moeten worden toegepast wanneer u een upgrade mislukt.


<br>

| Parameter | Beschrijving |
| --- | --- |
| ApplicationName | De naam van de toepassing die wordt bijgewerkt. Voorbeelden: fabric: / VisualObjects, stof: / ClusterMonitor  |
| TargetApplicationTypeVersion | Typt u de versie van de toepassing die de doelen van de upgrade. |
| FailureAction | De actie op die door de Service Fabric wanneer de upgrade is mislukt. De toepassing kan worden teruggezet naar de versie vóór update (rollback) of de upgrade kan worden gestopt in het huidige domein upgraden. In het laatste geval wordt de upgrade ook gewijzigd in handmatig. Toegestane waarden zijn terugdraaien en handleiding. |
| HealthCheckWaitDurationSec | De tijd om te wachten (in seconden) nadat de upgrade is voltooid op het domein upgraden voordat Service Fabric evalueert de gezondheid van de toepassing. Deze duur kan ook worden beschouwd als de tijd dat een toepassing moet worden uitgevoerd voordat het kan worden beschouwd als goed. Als u de statuscontrole geeft, verloopt het upgradeproces in het volgende domein upgraden.  Als de statuscontrole mislukt, wacht Fabric-Service op een interval (UpgradeHealthCheckInterval) voordat opnieuw de statuscontrole opnieuw proberen totdat de HealthCheckRetryTimeout wordt bereikt. De standaard en de aanbevolen waarde is 0 seconden. |
| HealthCheckRetryTimeoutSec | De duur (in seconden) verder die Fabric-Service uit te voeren evaluatie gezondheid voordat de upgrade declareren als mislukt. De standaardwaarde is 600 seconden. Deze duur wordt gestart nadat HealthCheckWaitDuration is bereikt. In deze HealthCheckRetryTimeout voeren Fabric-Service mogelijk meerdere controles van de gezondheid van de toepassing. De standaardwaarde is 10 minuten en op de juiste wijze moet worden aangepast voor uw toepassing. |
| HealthCheckStableDurationSec | De duur (in seconden) om te controleren of de toepassing stabiel is voordat met de volgende upgrade domein verplaatsen of voltooiing van de upgrade. Deze duur wachten wordt niet gevonden om wijzigingen te voorkomen van volksgezondheid rechts na de statuscontrole gebruikt. De standaardwaarde is 120 seconden, en moet op passende wijze worden aangepast voor uw toepassing. |
| UpgradeDomainTimeoutSec | De maximumtijd (in seconden) voor het upgraden van een enkel domein upgraden. Als de time-outwaarde is bereikt, wordt de upgrade reageert en verloopt op basis van de instelling voor de UpgradeFailureAction. De standaardwaarde is nooit (onbeperkte) en op de juiste wijze moeten worden aangepast voor uw toepassing. |
| UpgradeTimeout | Een time-outwaarde (in seconden) die van toepassing voor de gehele upgrade is. Als de time-outwaarde is bereikt, wordt de upgrade reageert en de UpgradeFailureAction geactiveerd. De standaardwaarde is nooit (onbeperkte) en op de juiste wijze moeten worden aangepast voor uw toepassing. |
| UpgradeHealthCheckInterval | De frequentie dat de status wordt gecontroleerd. Deze parameter is opgegeven in de sectie ClusterManager van het _cluster_ van de _bestandenlijst_en is niet opgegeven als onderdeel van de upgrade cmdlet. De standaardwaarde is 60 seconden.  |






<br>
## <a name="service-health-evaluation-during-application-upgrade"></a>Evaluatie van gezondheid service tijdens de upgrade van de toepassing

<br>
Evaluatiecriteria voor de gezondheid zijn optioneel. Als de evaluatiecriteria voor de gezondheid niet zijn opgegeven bij het starten van een upgrade, Fabric-Service maakt gebruik van het statusbeleid voor toepassing opgegeven in de ApplicationManifest.xml van het toepassingsexemplaar.


<br>

| Parameter | Beschrijving |
| --- | --- |
| ConsiderWarningAsError | Standaardwaarde is False. De waarschuwingen voor de gezondheid van de toepassing als fouten behandelen bij de beoordeling van de gezondheid van de toepassing tijdens de upgrade. Fabric-Service wordt standaard waarschuwing gezondheid gebeurtenissen worden fouten (fouten), zodat u de upgrade kunt doorgaan, zelfs als er waarschuwingen niet geëvalueerd.   |
| MaxPercentUnhealthyDeployedApplications | Standaard en de aanbevolen waarde is 0. Geef het maximum aantal gedistribueerde toepassingen (Zie de [sectie status](service-fabric-health-introduction.md)) beschadigd zijn kan voordat de toepassing wordt beschouwd wat de netwerkverbindingsmogelijkheden ervan en de upgrade mislukt. Deze parameter definieert de gezondheid van toepassing op het knooppunt en helpt problemen detecteren tijdens de upgrade. De replica's van de toepassing krijgen meestal verdeeld naar het andere knooppunt waarmee de toepassing wordt weergegeven in orde, waardoor de upgrade om verder te gaan. Door op te geven van een strikte MaxPercentUnhealthyDeployedApplications gezondheid, Fabric Service snel opsporen van een probleem met het toepassingspakket en helpen bij het maken van een snelle upgrade mislukt. |
| MaxPercentUnhealthyServices | Standaard en de aanbevolen waarde is 0. Geef het maximum aantal services in het toepassingsexemplaar dat wat de netwerkverbindingsmogelijkheden ervan worden kan voordat de toepassing wordt beschouwd wat de netwerkverbindingsmogelijkheden ervan en de upgrade mislukt. |
| MaxPercentUnhealthyPartitionsPerService | Standaard en de aanbevolen waarde is 0. Geef het maximum aantal partities in een service die beschadigd worden kan voordat de service wordt gezien als beschadigd. |
| MaxPercentUnhealthyReplicasPerPartition | Standaard en de aanbevolen waarde is 0. Geef het maximum aantal replica's in de partitie die beschadigd worden kan voordat de partitie is beschadigd. |
| UpgradeReplicaSetCheckTimeout | **Stateless service**--binnen één upgrade domein Service Fabric probeert om ervoor te zorgen dat er extra exemplaren van de service beschikbaar zijn. Als het doel exemplaar count meer dan één is, wacht Service stof meer dan één exemplaar beschikbaar, tot een maximum time-outwaarde. De time-outwaarde is opgegeven met de eigenschap UpgradeReplicaSetCheckTimeout. Als de time-out is verstreken, gaat Fabric-Service door met de upgrade, ongeacht het aantal exemplaren van de service. Als het aantal doel exemplaar een Fabric-Service niet wacht en direct verder met de upgrade. **Stateful service**--binnen één upgrade domein Service Fabric probeert om ervoor te zorgen dat de replicaset een quorum heeft. Fabric-service wacht op een quorum, tot een maximale time-out-waarde (zoals opgegeven door de eigenschap UpgradeReplicaSetCheckTimeout) beschikbaar. Als de time-out is verstreken, gaat Fabric-Service door met de upgrade, ongeacht het quorum. Deze instelling is ingesteld als nooit (oneindig) wanneer naar voren en 900 seconden wanneer wordt teruggedraaid. |
| ForceRestart | Als u een configuratie of een gegevenspakket bijwerken zonder de code bij te werken, de service opnieuw gestart als de eigenschap ForceRestart is ingesteld op true. Wanneer de update voltooid is, meldt Fabric-Service de service een nieuwe configuratie of gegevenspakket beschikbaar is. De service is verantwoordelijk voor de toepassing van de wijzigingen. Indien nodig, kan de service zelf starten. |



<br>
<br>
De criteria van MaxPercentUnhealthyServices, MaxPercentUnhealthyPartitionsPerService en MaxPercentUnhealthyReplicasPerPartition kunnen per servicetype voor een toepassingsexemplaar worden opgegeven. Deze parameters per service kan om een toepassing te typen met verschillende evaluatie beleid verschillende services bevatten. Een type stateless gateway service kan bijvoorbeeld een MaxPercentUnhealthyPartitionsPerService die afwijkt van een stateful engine service-type voor een specifieke toepassingsinstantie.

## <a name="next-steps"></a>Volgende stappen

[Upgraden van uw toepassing met behulp van Visual Studio](service-fabric-application-upgrade-tutorial.md) doorloopt u de upgrade van een toepassing met behulp van Visual Studio.

[Upgraden van uw toepassing met behulp van Powershell](service-fabric-application-upgrade-tutorial-powershell.md) doorloopt u de upgrade van een toepassing met PowerShell.

De toepassingsupgrades van uw compatibel maken door te leren hoe u [Gegevens serialisatie](service-fabric-application-upgrade-data-serialization.md).

Informatie over het gebruik van geavanceerde functionaliteit tijdens het bijwerken van uw toepassing door te verwijzen naar [Geavanceerde onderwerpen](service-fabric-application-upgrade-advanced.md).

Veelvoorkomende problemen oplossen in de toepassingsupgrades door te verwijzen naar de stappen in [Upgrades voor het oplossen van toepassing](service-fabric-application-upgrade-troubleshooting.md).
 
