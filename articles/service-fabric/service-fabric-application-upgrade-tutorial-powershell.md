<properties
   pageTitle="Service Fabric App upgrade met PowerShell | Microsoft Azure"
   description="Dit artikel helpt bij de ervaring van een Service Fabric-toepassing implementeren en een upgrade met PowerShell rolt de code te wijzigen."
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


# <a name="service-fabric-application-upgrade-using-powershell"></a>Service Fabric toepassing upgrade met PowerShell

> [AZURE.SELECTOR]
- [PowerShell](service-fabric-application-upgrade-tutorial-powershell.md)
- [Visual Studio](service-fabric-application-upgrade-tutorial.md)

<br/>

De meestgebruikte en aanbevolen upgrade benadering is de gecontroleerde rolling upgrade.  Azure Service Fabric wordt de status van de toepassing wordt bijgewerkt op basis van een set van het volksgezondheidsbeleid. Nadat de upgrade van een domein bijwerken (UD) Fabric-Service de toepassingsstatus wordt geëvalueerd en gaat door naar de volgende update domein of mislukt de upgrade is afhankelijk van het statusbeleid.

Een gecontroleerde toepassingen-upgrade kan worden uitgevoerd met behulp van de beheerde systeemeigen API's, PowerShell of REST. Zie voor instructies voor het uitvoeren van een upgrade met behulp van Visual Studio, [upgraden van uw toepassing met behulp van Visual Studio](service-fabric-application-upgrade-tutorial.md).

De beheerder van de toepassing kunt rolling upgrades Service Fabric gecontroleerd, de evaluatie van statusbeleid Fabric-Service gebruikt om te bepalen of de toepassing goed configureren. Bovendien kan de beheerder configureren dat de actie die moet worden genomen bij de beoordeling van de gezondheid is mislukt (bijvoorbeeld een automatisch herstel doen.) Deze sectie helpt bij een gecontroleerde upgrade voor één van de SDK-voorbeelden met PowerShell.

## <a name="step-1-build-and-deploy-the-visual-objects-sample"></a>Stap 1: Bouwen en implementeren van het monster visuele objecten


Maak en publiceer de toepassing met de rechtermuisknop op het toepassingsproject, **VisualObjectsApplication,** en als u de opdracht **publiceren** selecteert.  Zie [zelfstudie upgrade Service Fabric-toepassing](service-fabric-application-upgrade-tutorial.md)voor meer informatie.  Ook kunt u PowerShell implementatie van uw toepassing.

> [AZURE.NOTE] Voordat een van de opdrachten Fabric-Service kan worden gebruikt in PowerShell, moet u eerst verbinding maken met het cluster met behulp van de `Connect-ServiceFabricCluster` cmdlet. Ook wordt ervan uitgegaan dat het Cluster is al ingesteld op uw lokale computer. Zie het artikel over het [instellen van uw Service Fabric-ontwikkelomgeving](service-fabric-get-started.md).

Na het maken van het project in Visual Studio, kunt u de PowerShell-opdracht **Copy ServiceFabricApplicationPackage** het toepassingspakket kopiëren naar de ImageStore. De volgende stap is voor het registreren van de toepassing op de Service Fabric runtime met behulp van de **Register-ServiceFabricApplicationPackage** -cmdlet. De laatste stap is een exemplaar van de toepassing starten met de cmdlet **New-ServiceFabricApplication** .  Deze drie stappen zijn vergelijkbaar met het gebruik van het menu-item **implementeren** in Visual Studio.

Nu kunt u [Service Fabric Explorer bekijken en de toepassing van het cluster](service-fabric-visualizing-your-cluster.md). De toepassing heeft een webservice waarmee u kunt navigeren op in Internet Explorer door [http://localhost: 8081/visualobjects](http://localhost:8081/visualobjects) in de adresbalk te typen.  Sommige zwevende visuele objecten verplaatsen in het scherm te zien.  Bovendien kunt u **Get-ServiceFabricApplication** om de toepassingsstatus.

## <a name="step-2-update-the-visual-objects-sample"></a>Stap 2: Het monster visuele objecten bijwerken

U zult merken dat door de versie die u in stap 1 is geïmplementeerd, visuele objecten niet draaien. We werken deze toepassing op een plaats waar de visuele objecten die ook draaien.

Selecteer het project VisualObjects.ActorService van de VisualObjects oplossing en open het bestand StatefulVisualObjectActor.cs. In dat bestand, gaat u naar de methode `MoveObject`, een reactie van `this.State.Move()`, en verwijder de opmerkingen `this.State.Move(true)`. Deze wijziging worden de objecten nadat de service is bijgewerkt.

Ook moeten we het bestand *ServiceManifest.xml* (in PackageRoot) van het project **VisualObjects.ActorService**bijwerken. De *CodePackage* en de versie 2.0 en de overeenkomende regels in het bestand *ServiceManifest.xml* bijwerken.
U kunt de optie Visual Studio *Manifest-bestanden bewerken* nadat u met de rechtermuisknop op de oplossing van het manifestbestand wijzigingen aanbrengen.


Nadat de wijzigingen zijn aangebracht, het manifest eruit moet de volgende (wijzigingen in de gemarkeerde gedeelten weergeven):

```xml
<ServiceManifestName="VisualObjects.ActorService" Version="2.0" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">

<CodePackageName="Code" Version="2.0">
```

Het *ApplicationManifest.xml* -bestand (te vinden onder het **VisualObjects** -project onder de **VisualObjects** -oplossing) wordt nu bijgewerkt naar versie 2.0 van het project **VisualObjects.ActorService** . Bovendien is versie van de toepassing bijgewerkt naar 2.0.0.0 van 1.0.0.0. De *ApplicationManifest.xml* ziet er bijvoorbeeld het volgende fragment:

```xml
<ApplicationManifestxmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="VisualObjects" ApplicationTypeVersion="2.0.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">

 <ServiceManifestRefServiceManifestName="VisualObjects.ActorService" ServiceManifestVersion="2.0" />
```


Nu, het project maken door alleen het **ActorService** -project, en met de rechtermuisknop op en selecteert de optie **maken** in Visual Studio. Als u **opnieuw alle**selecteert, moet u de versies voor alle projecten bijwerken, omdat de code zou hebben gewijzigd. Vervolgens gaan we de bijgewerkte toepassing inpakken door met de rechtermuisknop op ***VisualObjectsApplication***, selecteert het Menu Service Fabric en **pakket**te kiezen. Deze actie maakt een toepassingspakket die kan worden geïmplementeerd.  De bijgewerkte toepassing is klaar om te worden geïmplementeerd.


## <a name="step-3--decide-on-health-policies-and-upgrade-parameters"></a>Stap 3: Bepaal op statusbeleid en parameters bijwerken

Lees de [toepassing parameters bijwerken](service-fabric-application-upgrade-parameters.md) en de [upgrade gaat uitvoeren](service-fabric-application-upgrade.md) om een goed inzicht in de verschillende parameters voor het bijwerken, time-outs en gezondheid criterium toegepast. Voor dit scenario de evaluatiecriterium service gezondheid is ingesteld op de standaardwaarde (en aanbevolen) waarden, wat betekent dat alle diensten en instanties moeten _in orde_ na de upgrade.  

Echter we verhogen de *HealthCheckStableDuration* tot 60 seconden (zodat de services gezonde gedurende ten minste 20 seconden zijn voordat u de upgrade gaat door naar de volgende update-domein).  Stel de *UpgradeDomainTimeout* 1200 seconden zijn en de *UpgradeTimeout* tot 3000 seconden worden ook de.

Ten slotte ook Stel de *UpgradeFailureAction* terug te draaien. Deze optie moet de Service Fabric terug te draaien de toepassing naar de vorige versie als u eventuele problemen worden aangetroffen tijdens de upgradeprocedure. Dus wanneer u een upgrade (in stap 4), worden de volgende parameters opgegeven:

FailureAction = ongedaan maken

HealthCheckStableDurationSec = 60

UpgradeDomainTimeoutSec = 1200

UpgradeTimeout = 3000


## <a name="step-4-prepare-application-for-upgrade"></a>Stap 4: Voorbereiden voor upgrade

De toepassing is nu ingebouwd en klaar om te worden bijgewerkt. Als u een venster PowerShell als beheerder en type **Get-ServiceFabricApplication openen**, laat het u weten dat het toepassingstype 1.0.0.0 van **VisualObjects** wordt geïmplementeerd.  

Het toepassingspakket is opgeslagen onder het volgende relatieve pad waar u de SDK van Service Fabric - *Samples\Services\Stateful\VisualObjects\VisualObjects\obj\x64\Debug*gedecomprimeerd. U moet een map 'Pakket' in die map, waar het toepassingspakket is opgeslagen. Controleer de tijdstempels om ervoor te zorgen dat het is de laatste build (wellicht moet u de paden op de juiste wijze ook wijzigen).

Nu gaan we het pakket met bijgewerkte toepassing kopiëren naar de Service Fabric ImageStore (waar de toepassingspakketten zijn opgeslagen door Service Fabric). De parameter *ApplicationPackagePathInImageStore* informeert Service Fabric waar het toepassingspakket met de kan vinden. We hebben de bijgewerkte toepassing plaatsen ' VisualObjects\_V2 "met de volgende opdracht (mogelijk moet u paden opnieuw op de juiste wijze te wijzigen).

```powershell
Copy-ServiceFabricApplicationPackage  -ApplicationPackagePath .\Samples\Services\Stateful\VisualObjects\VisualObjects\obj\x64\Debug\Package
-ImageStoreConnectionString fabric:ImageStore   -ApplicationPackagePathInImageStore "VisualObjects\_V2"
```

De volgende stap is deze toepassing registreren bij Fabric-Service, die kan worden uitgevoerd met de volgende opdracht:

```powershell
Register-ServiceFabricApplicationType -ApplicationPathInImageStore "VisualObjects\_V2"
```

Als deze opdracht niet slaagt, is het waarschijnlijk dat u alle services opgebouwd moet. Zoals vermeld in stap 2, moet u wellicht uw WebService-versie bijwerken.

## <a name="step-5-start-the-application-upgrade"></a>Stap 5: Start de upgrade van de toepassing

Nu we bent nu klaar om de upgrade van de toepassing start met de volgende opdracht:

```powershell
Start-ServiceFabricApplicationUpgrade -ApplicationName fabric:/VisualObjects -ApplicationTypeVersion 2.0.0.0 -HealthCheckStableDurationSec 60 -UpgradeDomainTimeoutSec 1200 -UpgradeTimeout 3000   -FailureAction Rollback -Monitored
```


De naam van de toepassing is hetzelfde als in het bestand *ApplicationManifest.xml beschreven* . Deze naam wordt gebruikt om te bepalen welke toepassing het ophalen van upgraden van Fabric-service. Als u de time-outs te kort zijn ingesteld, kunt u een foutbericht dat het probleem tegenkomen. Raadpleeg de sectie over probleemoplossing of de time-outs te verhogen.

Nu, als de toepassing upgrade opbrengst, kunt u controleren met behulp van Service Fabric Explorer, of met behulp van de volgende PowerShell command: **fabric Get-ServiceFabricApplicationUpgrade: / VisualObjects**.

In een paar minuten dat u hebt gekregen met de voorgaande PowerShell-opdracht zou de status dat alle update-domeinen zijn bijgewerkt (voltooid). En zult u de visuele objecten die in uw browservenster hebt gestart draaien!

U kunt upgraden van versie 2 naar versie 3 of van versie 2 naar versie 1 als een oefening. Verplaatsen van versie 2 naar versie 1 wordt ook beschouwd als een upgrade. Spelen met time-outs en statusbeleid voor uzelf bekend mee te maken. Wanneer u aan een Azure cluster implementeert, moeten de parameters op de juiste wijze zijn ingesteld. Is het raadzaam om voorzichtig de time-outs instellen.


## <a name="next-steps"></a>Volgende stappen

[Upgraden van uw toepassing met behulp van Visual Studio](service-fabric-application-upgrade-tutorial.md) doorloopt u de upgrade van een toepassing met behulp van Visual Studio.

Bepalen hoe uw toepassing met behulp van [parameters upgrade](service-fabric-application-upgrade-parameters.md)upgrades.

De toepassingsupgrades van uw compatibel maken door te leren hoe u [gegevens serialisatie](service-fabric-application-upgrade-data-serialization.md).

Informatie over het gebruik van geavanceerde functionaliteit tijdens het bijwerken van uw toepassing door te verwijzen naar [Geavanceerde onderwerpen](service-fabric-application-upgrade-advanced.md).

Veelvoorkomende problemen oplossen in de toepassingsupgrades door te verwijzen naar de stappen in de [Probleemoplossing toepassingsupgrades](service-fabric-application-upgrade-troubleshooting.md).
