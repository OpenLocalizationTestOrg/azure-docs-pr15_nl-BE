<properties
    pageTitle="Toepassingsbeheer Service Fabric automatiseren met behulp van PowerShell | Microsoft Azure"
    description="Implementeren, upgrade, testen en Service Fabric-toepassingen verwijderen met PowerShell."
    services="service-fabric"
    documentationCenter=".net"
    authors="rwike77"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-fabric"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="08/25/2016"
    ms.author="ryanwi"/>

# <a name="automate-the-application-lifecycle-using-powershell"></a>De levenscyclus van de toepassing met PowerShell te automatiseren

Veel aspecten van het [weefsel van de Service application lifecycle](service-fabric-application-lifecycle.md) kunnen worden geautomatiseerd.  In dit artikel ziet u hoe PowerShell gebruiken voor het automatiseren van algemene taken voor het implementeren, bijwerken, verwijderen en testen van toepassingen Azure Service Fabric.  Beheerd en HTTP-APIs voor het beheer app zijn ook beschikbaar. Zie [app lifecycle](service-fabric-application-lifecycle.md) voor meer informatie.  

## <a name="prerequisites"></a>Vereisten
Voordat u op de taken in het artikel verplaatst, moet u:

+ Vertrouwd raken met de Service Fabric-concepten zoals beschreven in [technisch overzicht van Service-Fabric](service-fabric-technical-overview.md).
+ [De runtime, SDK, en hulpprogramma's te installeren](service-fabric-get-started.md), die ook de module **ServiceFabric** PowerShell geïnstalleerd.
+ [Inschakelen van PowerShell script worden uitgevoerd](service-fabric-get-started.md#enable-powershell-script-execution).
+ Een lokale cluster start.  Een nieuw venster PowerShell als beheerder te starten en vervolgens het cluster setup-script uitvoeren vanuit de map SDK:`& "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\ClusterSetup\DevClusterSetup.ps1"`
+ Voordat u een PowerShell-opdrachten in dit artikel uitvoert, eerst verbinding maken met de lokale Service Fabric '-cluster met behulp van [**Verbinding maken met ServiceFabricCluster**](https://msdn.microsoft.com/library/azure/mt125938.aspx):`Connect-ServiceFabricCluster localhost:19000`
+ De volgende taken vereisen een pakket v1 toepassing te implementeren en een v2-toepassing voor een upgrade. De [voorbeeldtoepassing **WordCount** ](http://aka.ms/servicefabricsamples) (te vinden in de metingen aan de slag) downloaden. Samenstellen en verpakken van de toepassing in Visual Studio (Klik met de rechtermuisknop op **WordCount** in de Solution Explorer en selecteer **pakket**). Kopieer het pakket v1 in `C:\ServiceFabricSamples\Services\WordCount\WordCount\pkg\Debug` naar `C:\Temp\WordCount`. Copy `C:\Temp\WordCount` te `C:\Temp\WordCountV2`, maken van het pakket v2-toepassing voor een upgrade. Open `C:\Temp\WordCountV2\ApplicationManifest.xml` in een teksteditor. Wijzig het kenmerk **ApplicationTypeVersion** van "1.0.0" naar "2.0.0" is het versienummer app bijwerken in het element **ApplicationManifest** . Sla het gewijzigde bestand van ApplicationManifest.xml.

## <a name="task-deploy-a-service-fabric-application"></a>Taak: Service Fabric-toepassingen distribueren

Nadat u hebt gebouwd en verpakt de toepassing (of gedownload van het toepassingspakket), kunt u de toepassing naar een lokale Service Fabric '-cluster implementeren. Implementatie omvat het uploaden van het toepassingspakket, registratie van het toepassingstype en maken van het toepassingsexemplaar. Volg de instructies in deze sectie voor de implementatie van een nieuwe toepassing aan een cluster.

### <a name="step-1-upload-the-application-package"></a>Stap 1: Het toepassingspakket uploaden
Het toepassingspakket uploaden naar de afbeelding winkel zet deze op een locatie toegankelijk voor interne Fabric Service-onderdelen.  Het programmapakket bevat de benodigde manifest van de toepassing, service manifesten en code, configuratie en pakket(ten) gegevens voor het maken van de toepassing en de exemplaren van de service.  De opdracht [**Copy ServiceFabricApplicationPackage**](https://msdn.microsoft.com/library/azure/mt125905.aspx) uploadt u het pakket. Bijvoorbeeld:

```powershell
Copy-ServiceFabricApplicationPackage C:\Temp\WordCount\ -ImageStoreConnectionString file:C:\SfDevCluster\Data\ImageStoreShare -ApplicationPackagePathInImageStore WordCount
```

### <a name="step-2-register-the-application-type"></a>Stap 2: Het toepassingstype registreren
Registreren van het toepassingspakket wordt de toepassing en de versie die zijn gedefinieerd in het manifest van de toepassing beschikbaar voor gebruik. Het systeem leest het pakket geüpload in de stap 1, Controleer of het pakket (equivalent aan het lokaal [**Test-ServiceFabricApplicationPackage**](https://msdn.microsoft.com/library/azure/mt125950.aspx) uitgevoerd) en verwerken van de inhoud van het pakket het verwerkte pakket kopiëren naar de locatie van een systeem van interne.  Voer de cmdlet [**Register ServiceFabricApplicationType**](https://msdn.microsoft.com/library/azure/mt125958.aspx) :

```powershell
Register-ServiceFabricApplicationType WordCount
```
Overzicht van alle toepassingstypen die zijn geregistreerd in het cluster, voert de cmdlet [Get-ServiceFabricApplicationType](https://msdn.microsoft.com/library/azure/mt125871.aspx) :

```powershell
Get-ServiceFabricApplicationType
```

### <a name="step-3-create-the-application-instance"></a>Stap 3: Maak een exemplaar van de toepassing
Een toepassing kan worden gemaakt met behulp van een application-versie die is geregistreerd met de opdracht [**Nieuw ServiceFabricApplication**](https://msdn.microsoft.com/library/azure/mt125913.aspx) . De naam van elke toepassing wordt gedeclareerd op tijd implementeren en moet beginnen met de **fabric:** scheme en uniek zijn voor elk toepassingsexemplaar. De naam van toepassing en het type versie van de toepassing zijn gedefinieerd in het bestand **ApplicationManifest.xml** van het toepassingspakket. Als u alle services die standaard zijn gedefinieerd in het manifest van de toepassing van het type doeltoepassing, worden op dit moment die gemaakt.

```powershell
New-ServiceFabricApplication fabric:/WordCount WordCount 1.0.0
```

De opdracht [**Get-ServiceFabricApplication**](https://msdn.microsoft.com/library/azure/mt163515.aspx) worden alle toepassingsexemplaren die zijn gemaakt, samen met de algehele status. De opdracht [**Get-ServiceFabricService**](https://msdn.microsoft.com/library/azure/mt125889.aspx) worden alle van de service-exemplaren die zijn gemaakt binnen een bepaalde toepassingsexemplaar. Standaardservices (indien aanwezig) worden weergegeven.

```powershell
Get-ServiceFabricApplication

Get-ServiceFabricApplication | Get-ServiceFabricService
```

## <a name="task-upgrade-a-service-fabric-application"></a>Taak: Een toepassing Fabric-Service upgraden
U kunt een eerder gebruikte Service Fabric-toepassing met een bijgewerkte toepassing voor bijwerken. Deze taak voert een upgrade WordCount die is geïmplementeerd in ' taak: Service Fabric-toepassingen distribueren. " Lees [Service Fabric toepassing upgraden](service-fabric-application-upgrade.md) voor meer informatie.

Alleen het versienummer van toepassing is om te houden dingen eenvoudig voor dit voorbeeld, bijgewerkt in het pakket WordCountV2 toepassing gemaakt in de vereisten. Een meer realistische scenario zou betrekking hebben op uw service code of configuratie gegevens bestanden bijwerken en opnieuw te bouwen en verpakking van de toepassing met een bijgewerkte versienummers.  

### <a name="step-1-upload-the-updated-application-package"></a>Stap 1: Het pakket met bijgewerkte toepassing uploaden
De toepassing WordCount v1 is klaar om te worden bijgewerkt. Als u een venster PowerShell als beheerder en type [**Get-ServiceFabricApplication**](https://msdn.microsoft.com/library/azure/mt163515.aspx)opent, ziet u dat die versie 1.0.0 van het type WordCount toepassing wordt geïmplementeerd.  

Nu het pakket met bijgewerkte toepassing kopiëren naar het archief van de Service Fabric-installatiekopie (waar de toepassingspakketten zijn opgeslagen door Service Fabric). De parameter **ApplicationPackagePathInImageStore** informeert Service Fabric waar het toepassingspakket met de kan vinden. De volgende opdracht wordt het toepassingspakket op **WordCountV2** in het archief van de afbeelding:  

```powershell
Copy-ServiceFabricApplicationPackage C:\Temp\WordCountV2\ -ImageStoreConnectionString file:C:\SfDevCluster\Data\ImageStoreShare -ApplicationPackagePathInImageStore WordCountV2

```
### <a name="step-2-register-the-updated-application-type"></a>Stap 2: Het type bijgewerkte toepassing registreren
De volgende stap is de nieuwe versie van de toepassing registreren bij Fabric-Service, die kan worden uitgevoerd met behulp van de [**Register-ServiceFabricApplicationType**](https://msdn.microsoft.com/library/azure/mt125958.aspx) -cmdlet:

```powershell
Register-ServiceFabricApplicationType WordCountV2
```

### <a name="step-3-start-the-upgrade"></a>Stap 3: Start de upgrade
Verschillende parameters voor het bijwerken, time-outs en gezondheidscriteria kunnen worden toegepast op toepassingsupgrades. Lees de [toepassing bijwerken parameters](service-fabric-application-upgrade-parameters.md) en het [upgradeproces](service-fabric-application-upgrade.md) documenten voor meer informatie. Alle diensten en instanties moeten _in orde_ na de upgrade.  De **HealthCheckStableDuration** tot 60 seconden (zo instellen dat de services gezonde gedurende ten minste 20 seconden zijn voordat u de upgrade gaat door naar de volgende upgrade-domein).  Ook de **UpgradeDomainTimeout** op 1200 seconden en de **UpgradeTimeout** tot 3000 seconden ingesteld. Ten slotte stelt u de **UpgradeFailureAction** op **terugdraaien**, die vraagt dat Service Fabric teruggedraaid de toepassing naar de vorige versie als er fouten zijn opgetreden tijdens de upgrade.

U kunt de upgrade van de toepassing nu starten met behulp van de cmdlet [**Start ServiceFabricApplicationUpgrade**](https://msdn.microsoft.com/library/azure/mt125975.aspx) :

```powershell
Start-ServiceFabricApplicationUpgrade -ApplicationName fabric:/WordCount -ApplicationTypeVersion 2.0.0 -HealthCheckStableDurationSec 60 -UpgradeDomainTimeoutSec 1200 -UpgradeTimeout 3000  -FailureAction Rollback -Monitored
```

Houd er rekening mee dat de naam van de toepassing hetzelfde als de naam van de eerder gebruikte v1.0.0 is (fabric: / WordCount). Deze naam wordt gebruikt om te bepalen welke toepassing het ophalen van upgraden van Fabric-service. Als u de time-outs te kort zijn ingesteld, kunt u een time-foutbericht dat het probleem kunt tegenkomen. Verwijzen naar [upgrades van problemen met toepassingen](service-fabric-application-upgrade-troubleshooting.md)of verhoog de time-outs.

### <a name="step-4-check-upgrade-progress"></a>Stap 4: Selectievakje upgrade voortgang
Voortgang van toepassing upgrade kunt u controleren met behulp van [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md)of via de cmdlet [**Get-ServiceFabricApplicationUpgrade**](https://msdn.microsoft.com/library/azure/mt125988.aspx) :

```powershell
Get-ServiceFabricApplicationUpgrade fabric:/WordCount
```

In een paar minuten de cmdlet [Get-ServiceFabricApplicationUpgrade](https://msdn.microsoft.com/library/azure/mt125988.aspx) ziet u alle domeinen voor upgrade upgrade is uitgevoerd (voltooid).

## <a name="task-test-a-service-fabric-application"></a>Taak: Een toepassing Fabric-Service testen

Ontwikkelaars moeten schrijven diensten van hoge kwaliteit, kunnen fouten onbetrouwbaar infrastructuur voor het testen van de stabiliteit van hun diensten veroorzaken. Fabric-service biedt ontwikkelaars de mogelijkheid een fout met betrekking tot acties en diensten bij storingen testen met behulp van de chaos en failover-test-scenario's.  Lees de [testbaarheid-overzicht](service-fabric-testability-overview.md) voor meer informatie.

### <a name="step-1-run-the-chaos-test-scenario"></a>Stap 1: Het scenario chaos test uitvoeren
De chaos Testscenario genereert fouten over de gehele Service Fabric '-cluster. Het scenario comprimeert fouten in het algemeen gezien gedurende maanden of jaren in een paar uur. De combinatie van afwisselende fouten met een hoge fouttolerantie tarief wordt gezocht naar hoek gevallen die anders zouden worden gemist. In het volgende voorbeeld wordt het scenario chaos test uitgevoerd gedurende 60 minuten.

```powershell
$timeToRun = 60
$maxStabilizationTimeSecs = 180
$concurrentFaults = 3
$waitTimeBetweenIterationsSec = 60

Invoke-ServiceFabricChaosTestScenario -TimeToRunMinute $timeToRun -MaxClusterStabilizationTimeoutSec $maxStabilizationTimeSecs -MaxConcurrentFaults $concurrentFaults -EnableMoveReplicaFaults -WaitTimeBetweenIterationsSec $waitTimeBetweenIterationsSec
```

### <a name="step-2-run-the-failover-test-scenario"></a>Stap 2: Uitvoeren van de failover-test-scenario
Failover testdoelen scenario een specifieke partitie in plaats van het hele cluster en het verlaten van andere diensten niet beïnvloed. Het scenario doorloopt een reeks van gesimuleerde fouten in de validatie service terwijl uw bedrijfslogica wordt uitgevoerd. Een storing in de validatie service geeft aan een probleem dat verder onderzoek nodig. De failover-test induceert slechts één fout tegelijk in plaats van de chaos Testscenario kan meerdere fouten veroorzaken. In het volgende voorbeeld wordt de failover-test voor 60 minuten uitgevoerd tegen het weefsel: WordCount/WordCountService service.

```powershell
$timeToRun = 60
$maxStabilizationTimeSecs = 180
$waitTimeBetweenFaultsSec = 10
$serviceName = "fabric:/WordCount/WordCountService"

Invoke-ServiceFabricFailoverTestScenario -TimeToRunMinute $timeToRun -MaxServiceStabilizationTimeoutSec $maxStabilizationTimeSecs -WaitTimeBetweenFaultsSec $waitTimeBetweenFaultsSec -ServiceName $serviceName -PartitionKindUniformInt64 -PartitionKey 1
```

## <a name="task-remove-a-service-fabric-application"></a>Taak: Een toepassing Fabric-Service verwijderen
U kunt een exemplaar van een gedistribueerde toepassing verwijderen, het type ingerichte toepassing uit het cluster verwijderen en het toepassingspakket van de ImageStore verwijderen.

### <a name="step-1-remove-an-application-instance"></a>Stap 1: Een toepassingsexemplaar verwijderen
Instantie van een toepassing niet meer nodig is, kan permanent door de cmdlet [**Verwijderen ServiceFabricApplication**](https://msdn.microsoft.com/library/azure/mt125914.aspx) worden verwijderd. Hiermee verwijdert u automatisch ook alle services die deel uitmaken van de toepassing definitief verwijderen van de status van alle service. Deze bewerking kan niet ongedaan worden gemaakt en de toepassingsstatus kan niet worden hersteld.

```powershell
Remove-ServiceFabricApplication fabric:/WordCount
```

### <a name="step-2-unregister-the-application-type"></a>Stap 2: De registratie van het toepassingstype
Wanneer u een bepaalde versie van een toepassingstype niet meer nodig, de registratie van het met behulp van de [**Registratie-ServiceFabricApplicationType**](https://msdn.microsoft.com/library/azure/mt125885.aspx) -cmdlet. Afmelden van niet-gebruikte typen releases opslagruimte die wordt gebruikt door het toepassingspakket in het archief van de installatiekopie. Een toepassingstype kan worden gemaakt als er zijn geen toepassingen gemaakt tegen of in afwachting van de toepassingsupgrades ernaar wordt verwezen.

```powershell
Unregister-ServiceFabricApplicationType WordCount 1.0.0
```

### <a name="step-3-remove-the-application-package"></a>Stap 3: Het toepassingspakket verwijderen
Nadat u het toepassingstype is niet geregistreerd, kan het toepassingspakket worden verwijderd uit het archief van de afbeelding met de cmdlet [**Verwijderen ServiceFabricApplicationPackage**](https://msdn.microsoft.com/library/azure/mt163532.aspx) .

```powershell
Remove-ServiceFabricApplicationPackage -ImageStoreConnectionString file:C:\SfDevCluster\Data\ImageStoreShare -ApplicationPackagePathInImageStore WordCount
```

## <a name="next-steps"></a>Volgende stappen
[Fabric service application lifecycle](service-fabric-application-lifecycle.md)

[Een toepassing](service-fabric-deploy-remove-applications.md)

[Upgrade van de toepassing](service-fabric-application-upgrade.md)

[Azure Service Fabric-cmdlets](https://msdn.microsoft.com/library/azure/mt125965.aspx)

[Azure Service Fabric testbaarheid cmdlets](https://msdn.microsoft.com/library/azure/mt125844.aspx)
