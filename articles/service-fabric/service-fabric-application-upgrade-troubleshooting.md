<properties
   pageTitle="Het oplossen van toepassingsupgrades | Microsoft Azure"
   description="In dit artikel komen een aantal problemen rond een upgrade uitvoert van een toepassing Fabric-Service en hoe deze op te lossen."
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

# <a name="troubleshoot-application-upgrades"></a>Toepassingsupgrades oplossen

In dit artikel komen een aantal van de problemen rond het upgraden van een toepassing Azure Service weefsel en hoe deze op te lossen.

## <a name="troubleshoot-a-failed-application-upgrade"></a>Problemen oplossen met een mislukte toepassing upgrade

Wanneer u een upgrade mislukt, bevat de uitvoer van de opdracht **Get-ServiceFabricApplicationUpgrade** extra informatie voor foutopsporing van de storing.  De volgende lijst wordt aangegeven hoe de extra informatie kan worden gebruikt:

1. Identificatie van het fouttype.
2. Identificeer de oorzaak van fout.
3. Isoleren van een of meer beschadigde onderdelen voor verder onderzoek.

Deze informatie is beschikbaar als Fabric-Service de storing detecteert ongeacht of de **FailureAction** terugzetten of onderbreking van de upgrade.

### <a name="identify-the-failure-type"></a>Identificatie van het fouttype

In de uitvoer van de **Get-ServiceFabricApplicationUpgrade**geeft **FailureTimestampUtc** de tijdstempel (in UTC) waarop een upgrade fout is gedetecteerd door Fabric Service en **FailureAction** is geactiveerd. **FailureReason** geeft een van de drie algemene oorzaken van de fout:

1. UpgradeDomainTimeout - geeft aan dat een bepaald domein voor de upgrade duurde te lang om te voltooien en **UpgradeDomainTimeout** verlopen.
2. OverallUpgradeTimeout - geeft aan dat de totale upgrade duurde te lang om te voltooien en **UpgradeTimeout** verlopen.
3. Health Check - geeft aan dat na het upgraden van een domein bijwerken, de toepassing beschadigd volgens de opgegeven statusbeleid bleef en **HealthCheckRetryTimeout** verlopen.

Deze posten alleen weergegeven in de uitvoer wanneer u de upgrade mislukt en wordt teruggedraaid. Verdere informatie is afhankelijk van het type van de fout weergegeven.

### <a name="investigate-upgrade-timeouts"></a>Upgrade time-outs onderzoeken

Timeout-fouten meestal door problemen met de beschikbaarheid service veroorzaakt worden bijwerken. De uitvoer hieronder is typisch voor upgrades waarbij service replica's of exemplaren niet worden gestart in de nieuwe versie van de code. Het veld **UpgradeDomainProgressAtFailure** wordt een momentopname van de upgrade werk op het tijdstip van de fout die in behandeling zijn vastgelegd.

~~~
PS D:\temp> Get-ServiceFabricApplicationUpgrade fabric:/DemoApp

ApplicationName                : fabric:/DemoApp
ApplicationTypeName            : DemoAppType
TargetApplicationTypeVersion   : v2
ApplicationParameters          : {}
StartTimestampUtc              : 4/14/2015 9:26:38 PM
FailureTimestampUtc            : 4/14/2015 9:27:05 PM
FailureReason                  : UpgradeDomainTimeout
UpgradeDomainProgressAtFailure : MYUD1

                                 NodeName            : Node4
                                 UpgradePhase        : PostUpgradeSafetyCheck
                                 PendingSafetyChecks :
                                     WaitForPrimaryPlacement - PartitionId: 744c8d9f-1d26-417e-a60e-cd48f5c098f0

                                 NodeName            : Node1
                                 UpgradePhase        : PostUpgradeSafetyCheck
                                 PendingSafetyChecks :
                                     WaitForPrimaryPlacement - PartitionId: 4b43f4d8-b26b-424e-9307-7a7a62e79750
UpgradeState                   : RollingBackCompleted
UpgradeDuration                : 00:00:46
CurrentUpgradeDomainDuration   : 00:00:00
NextUpgradeDomain              :
UpgradeDomainsStatus           : { "MYUD1" = "Completed";
                                 "MYUD2" = "Completed";
                                 "MYUD3" = "Completed" }
UpgradeKind                    : Rolling
RollingUpgradeMode             : UnmonitoredAuto
ForceRestart                   : False
UpgradeReplicaSetCheckTimeout  : 00:00:00
~~~

In dit voorbeeld wordt de upgrade mislukt upgrade domein *MYUD1* en twee partities (*744c8d9f-1d26-417e-a60e-cd48f5c098f0* en *4b43f4d8-b26b-424e-9307-7a7a62e79750*) zijn vastgelopen. De partities zijn vastgelopen omdat de runtime is geen op doelknooppunten *Node1* en *Knooppunt4*om primaire replica's (*WaitForPrimaryPlacement*).

De opdracht **Get-ServiceFabricNode** kan worden gebruikt om te controleren of deze twee knooppunten in upgrade domein *MYUD1*. De *UpgradePhase* zegt *PostUpgradeSafetyCheck*, wat betekent dat deze controles veiligheid optreden nadat alle knooppunten in de upgrade-domein bijgewerkt hebt. Al deze informatie wijst op een mogelijk probleem met de nieuwe versie van het Wetboek van toepassing. De meest voorkomende problemen zijn fouten in de open of promoveren tot primaire codepaden service.

Een *UpgradePhase* van *PreUpgradeSafetyCheck* betekent dat er zijn problemen met het domein van de upgrade wordt voorbereid voordat deze is uitgevoerd. De meest voorkomende problemen zijn in dit geval service fouten in de degradatie van primaire codepaden of sluiten.

De huidige **UpgradeState** is *RollingBackCompleted*, zodat de oorspronkelijke upgrade moet worden uitgevoerd met een rollback **FailureAction**, die automatisch teruggedraaid de upgrade mislukt. Als de oorspronkelijke upgrade is uitgevoerd met een handmatige **FailureAction**, vervolgens de upgrade zou in plaats daarvan worden geschorst om live voor foutopsporing van de toepassing.

### <a name="investigate-health-check-failures"></a>Gezondheid selectievakje storingen onderzoeken

Gezondheid selectievakje storingen kunnen worden veroorzaakt door verschillende problemen die gebeuren kunnen nadat alle knooppunten in een upgrade-domein upgraden en het doorgeven van alle controles van de veiligheid. De uitvoer hieronder is typisch voor een mislukte upgrade als gevolg van mislukte controles. Het veld **UnhealthyEvaluations** wordt een momentopname van de controles die niet op het moment van de upgrade op basis van het opgegeven [beleid](service-fabric-health-introduction.md)vastgelegd.

~~~
PS D:\temp> Get-ServiceFabricApplicationUpgrade fabric:/DemoApp

ApplicationName                         : fabric:/DemoApp
ApplicationTypeName                     : DemoAppType
TargetApplicationTypeVersion            : v4
ApplicationParameters                   : {}
StartTimestampUtc                       : 4/24/2015 2:42:31 AM
UpgradeState                            : RollingForwardPending
UpgradeDuration                         : 00:00:27
CurrentUpgradeDomainDuration            : 00:00:27
NextUpgradeDomain                       : MYUD2
UpgradeDomainsStatus                    : { "MYUD1" = "Completed";
                                          "MYUD2" = "Pending";
                                          "MYUD3" = "Pending" }
UnhealthyEvaluations                    :
                                          Unhealthy services: 50% (2/4), ServiceType='PersistedServiceType', MaxPercentUnhealthyServices=0%.

                                          Unhealthy service: ServiceName='fabric:/DemoApp/Svc3', AggregatedHealthState='Error'.

                                              Unhealthy partitions: 100% (1/1), MaxPercentUnhealthyPartitionsPerService=0%.

                                              Unhealthy partition: PartitionId='3a9911f6-a2e5-452d-89a8-09271e7e49a8', AggregatedHealthState='Error'.

                                                  Error event: SourceId='Replica', Property='InjectedFault'.

                                          Unhealthy service: ServiceName='fabric:/DemoApp/Svc2', AggregatedHealthState='Error'.

                                              Unhealthy partitions: 100% (1/1), MaxPercentUnhealthyPartitionsPerService=0%.

                                              Unhealthy partition: PartitionId='744c8d9f-1d26-417e-a60e-cd48f5c098f0', AggregatedHealthState='Error'.

                                                  Error event: SourceId='Replica', Property='InjectedFault'.

UpgradeKind                             : Rolling
RollingUpgradeMode                      : Monitored
FailureAction                           : Manual
ForceRestart                            : False
UpgradeReplicaSetCheckTimeout           : 49710.06:28:15
HealthCheckWaitDuration                 : 00:00:00
HealthCheckStableDuration               : 00:00:10
HealthCheckRetryTimeout                 : 00:00:10
UpgradeDomainTimeout                    : 10675199.02:48:05.4775807
UpgradeTimeout                          : 10675199.02:48:05.4775807
ConsiderWarningAsError                  :
MaxPercentUnhealthyPartitionsPerService :
MaxPercentUnhealthyReplicasPerPartition :
MaxPercentUnhealthyServices             :
MaxPercentUnhealthyDeployedApplications :
ServiceTypeHealthPolicyMap              :
~~~

Gezondheid selectievakje fouten onderzoekt, moet u eerst een goed begrip van het statusmodel Fabric-Service. Maar zelfs zonder een diepgaand inzicht, kunnen we zien dat twee services beschadigd zijn: *fabric: / DemoApp/Svc3* en *fabric: / DemoApp/Svc2*, samen met de foutenrapporten gezondheid ('InjectedFault' in dit geval). In dit voorbeeld, twee van de vier services zijn beschadigd, die lager is dan het doel van de standaard van 0% beschadigd (*MaxPercentUnhealthyServices*).

De upgrade werd geschorst bij storing door te geven van een **FailureAction** van de handleiding bij het starten van de upgrade. In deze modus kunt ons de live systeem in de status mislukt onderzoeken alvorens verdere actie te ondernemen.

### <a name="recover-from-a-suspended-upgrade"></a>Herstellen na een upgrade onderbroken

Met een **FailureAction**ongedaan maken is niet nodig aangezien de upgrade automatisch teruggedraaid in bij storing herstel. Met een handmatige **FailureAction**zijn er verschillende opties voor Systeemherstel:

1. Handmatig een rollback tot gevolg
2. Volg de stappen in de rest van de upgrade handmatig
3. De gecontroleerde upgrade hervatten

De opdracht **Start-ServiceFabricApplicationRollback** kan worden gebruikt op elk gewenst moment starten van de toepassing worden teruggedraaid. Nadat u de opdracht met succes wordt geretourneerd, wordt het verzoek terugdraaien is geregistreerd in het systeem en kort nadien begint.

De opdracht **Resume ServiceFabricApplicationUpgrade** kan worden gebruikt om door te gaan met de rest van de upgrade handmatig upgraden domeinen tegelijk. In deze modus worden alleen veiligheid controles uitgevoerd door het systeem. Niet meer worden uitgevoerd. Deze opdracht kan alleen worden gebruikt wanneer de *UpgradeState* ziet u *RollingForwardPending*, wat betekent dat het huidige domein upgraden upgrade is voltooid, maar er een niet gestart is (in behandeling).

De opdracht **Update ServiceFabricApplicationUpgrade** kan worden gebruikt voor het hervatten van de gecontroleerde upgrade met zowel de veiligheid en gezondheid gecontroleerd wordt uitgevoerd.

~~~
PS D:\temp> Update-ServiceFabricApplicationUpgrade fabric:/DemoApp -UpgradeMode Monitored

UpgradeMode                             : Monitored
ForceRestart                            :
UpgradeReplicaSetCheckTimeout           :
FailureAction                           :
HealthCheckWaitDuration                 :
HealthCheckStableDuration               :
HealthCheckRetryTimeout                 :
UpgradeTimeout                          :
UpgradeDomainTimeout                    :
ConsiderWarningAsError                  :
MaxPercentUnhealthyPartitionsPerService :
MaxPercentUnhealthyReplicasPerPartition :
MaxPercentUnhealthyServices             :
MaxPercentUnhealthyDeployedApplications :
ServiceTypeHealthPolicyMap              :

PS D:\temp>
~~~

De upgrade blijft van de upgrade-domein waar de onderbroken is en gebruik dezelfde parameters en het statusbeleid gehandhaafd als vóór upgrade. Indien nodig, kan een van de parameters voor het bijwerken en het statusbeleid die in de voorgaande uitvoer worden gewijzigd in dezelfde opdracht als de upgrade wordt gehaald. In dit voorbeeld wordt de upgrade in de modus met de parameters en het statusbeleid ongewijzigd gecontroleerde hervat.

## <a name="further-troubleshooting"></a>Problemen op te lossen

### <a name="service-fabric-is-not-following-the-specified-health-policies"></a>Service Fabric volgt niet de opgegeven statusbeleid

Mogelijke oorzaak 1

Fabric-service interpreteert alle percentages in het werkelijke aantal entiteiten (bijvoorbeeld replica's, partities en services) voor de evaluatie van de gezondheid en altijd omhoog wordt afgerond op hele entiteiten. Als de maximale _MaxPercentUnhealthyReplicasPerPartition_ 21 bedraagt % en er vijf replica's zijn, vervolgens Fabric-Service kunnen bijvoorbeeld twee beschadigde replica's (die is,'Math.Ceiling (5\*0.21)). Statusbeleid moeten dus dienovereenkomstig worden ingesteld.

Mogelijke oorzaak 2

Statusbeleid worden opgegeven in percentages van de totale services en geen specifieke service-exemplaren. Bijvoorbeeld voor een upgrade, als een toepassing vier heeft exemplaren A, B, C en D, service waar D-service beschadigd is, maar met weinig invloed op de toepassing. Willen we de bekende beschadigd service D negeren tijdens de upgrade ingesteld met de parameter *MaxPercentUnhealthyServices* 25%, ervan uitgaande dat alleen A, B en C moeten zijn in orde.

Echter tijdens de upgrade komen D gezonde terwijl C beschadigd raakt. De upgrade zou nog steeds mislukt, omdat slechts 25% van de diensten beschadigd zijn. Echter, kan dit resulteren in onverwachte fouten als gevolg van C wordt onverwacht beschadigd in plaats van D. In dit geval D gemodelleerd als een andere service-type van A, B en C. Aangezien het statusbeleid worden gespecificeerd per servicetype, kunnen andere beschadigde percentage drempels worden toegepast op verschillende services. 

### <a name="i-did-not-specify-a-health-policy-for-application-upgrade-but-the-upgrade-still-fails-for-some-time-outs-that-i-never-specified"></a>Een statusbeleid voor een upgrade van de toepassing niet is opgegeven, maar de upgrade nog steeds niet voor sommige time-outs die ik nooit opgegeven

Als het statusbeleid worden niet doorgegeven aan het verzoek van de upgrade, worden ze overgenomen van de *ApplicationManifest.xml* van de huidige versie van de toepassing. Als u een X toepassing van versie 1.0 naar versie 2.0, gezondheid toepassingenbeleid opgegeven upgrade uitvoert voor in versie 1.0 worden bijvoorbeeld gebruikt. Als u een ander beleid voor de upgrade, moet het beleid worden opgegeven als onderdeel van de upgrade API-aanroep van toepassing. De opgegeven als onderdeel van de API-aanroep beleid alleen van toepassing tijdens de upgradeprocedure. Zodra de upgrade voltooid is, wordt het beleid dat in de *ApplicationManifest.xml* worden gebruikt.

### <a name="incorrect-time-outs-are-specified"></a>Onjuiste time-outs zijn opgegeven.

U mag hebben zich afgevraagd wat er gebeurt wanneer de time-outs inconsistent zijn ingesteld. Bijvoorbeeld, wellicht een *UpgradeTimeout* die lager is dan de *UpgradeDomainTimeout*. Het antwoord is dat er een fout geretourneerd. Fouten worden geretourneerd als de *UpgradeDomainTimeout* kleiner is dan de som van *HealthCheckWaitDuration* en *HealthCheckRetryTimeout*, of als de *UpgradeDomainTimeout* kleiner is dan de som van *HealthCheckWaitDuration* en *HealthCheckStableDuration*.

### <a name="my-upgrades-are-taking-too-long"></a>Mijn upgrades duurt te lang

De tijd voor een upgrade wilt uitvoeren, is afhankelijk van health checks en time-outs die zijn opgegeven. Health checks en time-outs afhankelijk van hoe lang het duurt om te kopiëren, distribueren en regulering van de toepassing. Te agressief met time-outs wordt kan het zijn dat meer mislukte upgrades, zodat u het beste eerst voorzichtig met langere time-outs.

Hier is een korte opfriscursus op de wisselwerking tussen de time-outs en de upgrade tijden:

Upgrades voor een domein upgraden niet, sneller dan de *HealthCheckWaitDuration voltooien* + *HealthCheckStableDuration*.

Mislukte upgrade kan niet plaatsvinden, sneller dan *HealthCheckWaitDuration* + *HealthCheckRetryTimeout*.

De upgrade voor een domein upgraden tijd wordt beperkt door *UpgradeDomainTimeout*.  Als *HealthCheckRetryTimeout* en *HealthCheckStableDuration* beide niet nul zijn en de gezondheid van de toepassing heen en weer schakelen blijven en vervolgens de upgrade uiteindelijk een optreedt op *UpgradeDomainTimeout time-out*. *UpgradeDomainTimeout* begint te tellen ingedrukt zodra de upgrade voor het huidige domein upgraden begint.

## <a name="next-steps"></a>Volgende stappen

[Upgraden van uw toepassing met behulp van Visual Studio](service-fabric-application-upgrade-tutorial.md) doorloopt u de upgrade van een toepassing met behulp van Visual Studio.

[Upgraden van uw toepassing met behulp van Powershell](service-fabric-application-upgrade-tutorial-powershell.md) doorloopt u de upgrade van een toepassing met PowerShell.

Bepalen hoe uw toepassing met behulp van [Parameters Upgrade](service-fabric-application-upgrade-parameters.md)upgrades.

De toepassingsupgrades van uw compatibel maken door te leren hoe u [Gegevens serialisatie](service-fabric-application-upgrade-data-serialization.md).

Informatie over het gebruik van geavanceerde functionaliteit tijdens het bijwerken van uw toepassing door te verwijzen naar [Geavanceerde onderwerpen](service-fabric-application-upgrade-advanced.md).

Veelvoorkomende problemen oplossen in de toepassingsupgrades door te verwijzen naar de stappen in [Upgrades voor het oplossen van toepassing](service-fabric-application-upgrade-troubleshooting.md).
 
