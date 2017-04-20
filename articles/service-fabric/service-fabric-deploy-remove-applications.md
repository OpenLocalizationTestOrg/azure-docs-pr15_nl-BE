<properties
   pageTitle="Toepassingsimplementatie service Fabric | Microsoft Azure"
   description="Het implementeren en toepassingen in Fabric-Service verwijderen"
   services="service-fabric"
   documentationCenter=".net"
   authors="rwike77"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/25/2016"
   ms.author="ryanwi"/>

# <a name="deploy-and-remove-applications-using-powershell"></a>Implementeren en toepassingen met PowerShell verwijderen

> [AZURE.SELECTOR]
- [PowerShell](service-fabric-deploy-remove-applications.md)
- [Visual Studio](service-fabric-publish-app-remote-cluster.md)

<br/>

Eenmaal een die [zijn ingepakt toepassingstype][10], deze gereed is voor implementatie in een Azure Service Fabric '-cluster. Implementatie omvat de volgende drie stappen:

1. Het toepassingspakket uploaden
2. Het toepassingstype registreren
3. Exemplaar van de toepassing maken

>[AZURE.NOTE] Als u Visual Studio voor het implementeren en foutopsporing in toepassingen op uw cluster voor plaatselijke ontwikkeling, worden de volgende stappen automatisch afgehandeld via een PowerShell script gevonden in de map Scripts van de application-project. Dit artikel bevat achtergrondinformatie over wat deze scripts doen zodat u bewerkingen buiten Visual Studio kunt uitvoeren.

## <a name="upload-the-application-package"></a>Het toepassingspakket uploaden

Uploaden van het toepassingspakket geplaatst op een locatie die toegankelijk is voor interne Fabric Service-onderdelen. PowerShell kunt u het uploaden uitvoert. Voordat u een PowerShell-opdrachten in dit artikel uitvoert, altijd start met [Verbinden ServiceFabricCluster](https://msdn.microsoft.com/library/mt125938.aspx) verbinding maken met het cluster Service Fabric.

Stel dat u beschikt over een map met de naam *MyApplicationType* bevat die de nodige toepassingsmanifest, service manifesten en config-code/data pakketten. De opdracht [Copy ServiceFabricApplicationPackage](https://msdn.microsoft.com/library/mt125905.aspx) het pakket naar het cluster winkel afbeelding geüpload. De cmdlet **Get-ImageStoreConnectionStringFromClusterManifest** , dat deel van de module Service Fabric SDK PowerShell uitmaakt, wordt gebruikt om de verbindingsreeks van de afbeelding opslaan.  Voor het importeren van de module SDK worden uitgevoerd:

```
Import-Module "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\Tools\PSModule\ServiceFabricSDK\ServiceFabricSDK.psm1"
```

U kunt een toepassingspakket van *Studio C:\users\ryanwi\Documents\Visual 2015\Projects\MyApplication\myapplication\pkg\debug* kopiëren naar *c:\temp\MyApplicationType* (de "debug" map "MyApplicationType" rename). In het volgende voorbeeld worden het pakket geüpload:

~~~
PS C:\temp> dir

    Directory: c:\temp


Mode                LastWriteTime         Length Name                                                                                   
----                -------------         ------ ----                                                                                   
d-----        8/12/2016  10:23 AM                MyApplicationType                                                                          

PS C:\temp> tree /f .\Stateless1Pkg

C:\TEMP\MyApplicationType
│   ApplicationManifest.xml
|
└───Stateless1Pkg
  	|   ServiceManifest.xml
    │
    └───Code
    │   │  Microsoft.ServiceFabric.Data.dll
    │   │  Microsoft.ServiceFabric.Data.Interfaces.dll
    │   │  Microsoft.ServiceFabric.Internal.dll
    │   │  Microsoft.ServiceFabric.Internal.Strings.dll
    │   │  Microsoft.ServiceFabric.Services.dll
    │   │  ServiceFabricServiceModel.dll
    │   │  MyService.exe
    │   │  MyService.exe.config
    │   │  MyService.pdb
    │   │  System.Fabric.dll
    │   │  System.Fabric.Strings.dll
    │   │
    │   └───en-us
  	|         Microsoft.ServiceFabric.Internal.Strings.resources.dll
  	|         System.Fabric.Strings.resources.dll
  	|
    ├───Config
    │     Settings.xml
    │
    └───Data
          init.dat

PS C:\temp> Copy-ServiceFabricApplicationPackage -ApplicationPackagePath MyApplicationType -ImageStoreConnectionString (Get-ImageStoreConnectionStringFromClusterManifest(Get-ServiceFabricClusterManifest))
Copy application package succeeded

PS D:\temp>
~~~

## <a name="register-the-application-package"></a>Het toepassingspakket registreren

Registreren van het toepassingspakket wordt de toepassing en de versie die zijn gedefinieerd in het manifest van de toepassing beschikbaar voor gebruik. Het systeem leest het pakket geüpload in de vorige stap, controleren of het pakket (equivalent aan het lokaal [Test-ServiceFabricApplicationPackage](https://msdn.microsoft.com/library/mt125950.aspx) uitgevoerd), verwerkt de inhoud van het pakket en het pakket verwerkte kopiëren naar de locatie van een systeem van interne.

~~~
PS D:\temp> Register-ServiceFabricApplicationType MyApplicationType
Register application type succeeded

PS D:\temp> Get-ServiceFabricApplicationType

ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : AppManifestVersion1
DefaultParameters      : {}

PS D:\temp>
~~~

De opdracht [Register ServiceFabricApplicationType](https://msdn.microsoft.com/library/mt125958.aspx) retourneert nadat het systeem het programmapakket heeft gekopieerd. Hoe lang dit duurt, is afhankelijk van de inhoud van het toepassingspakket. Indien nodig kan de parameter **- TimeoutSec** worden gebruikt om een langere time-out. (De standaardtime-out is 60 seconden).

De opdracht [Get-ServiceFabricApplicationType](https://msdn.microsoft.com/library/mt125871.aspx) worden alle versies van de toepassing geregistreerd type.

## <a name="create-the-application"></a>De toepassing maken

U kunt een toepassing met behulp van een toepassing-versie die is geregistreerd via de opdracht [Nieuw-ServiceFabricApplication](https://msdn.microsoft.com/library/mt125913.aspx) instantiëren. De naam van elke toepassing moet beginnen met de *fabric:* scheme en uniek zijn voor elk toepassingsexemplaar. Alle services die standaard gedefinieerd in het manifest van de toepassing van het toepassingstype dat doel worden op dit moment gemaakt.

~~~
PS D:\temp> New-ServiceFabricApplication fabric:/MyApp MyApplicationType AppManifestVersion1

ApplicationName        : fabric:/MyApp
ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : AppManifestVersion1
ApplicationParameters  : {}

PS D:\temp> Get-ServiceFabricApplication  

ApplicationName        : fabric:/MyApp
ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : AppManifestVersion1
ApplicationStatus      : Ready
HealthState            : OK
ApplicationParameters  : {}

PS D:\temp> Get-ServiceFabricApplication | Get-ServiceFabricService

ServiceName            : fabric:/MyApp/MyService
ServiceKind            : Stateless
ServiceTypeName        : MyServiceType
IsServiceGroup         : False
ServiceManifestVersion : SvcManifestVersion1
ServiceStatus          : Active
HealthState            : Ok

PS D:\temp>
~~~

De opdracht [Get-ServiceFabricApplication](https://msdn.microsoft.com/library/mt163515.aspx) worden alle toepassingsexemplaren die zijn gemaakt, samen met de algehele status.

De opdracht [Get-ServiceFabricService](https://msdn.microsoft.com/library/mt125889.aspx) worden alle exemplaren van de service die zijn gemaakt binnen een bepaalde toepassingsexemplaar. Standaardservices (indien aanwezig) worden hier weergegeven.

Meerdere toepassingsexemplaren kunnen worden gemaakt voor een bepaalde versie van een type geregistreerde toepassing. Elk toepassingsexemplaar wordt uitgevoerd in een geïsoleerde omgeving met een eigen werk-directory en het proces.

## <a name="remove-an-application"></a>Een toepassing verwijderen

Instantie van een toepassing niet meer nodig is, kan permanent met de opdracht [Verwijderen ServiceFabricApplication](https://msdn.microsoft.com/library/mt125914.aspx) worden verwijderd. Met deze opdracht verwijdert automatisch alle services die deel uitmaken van de toepassing, ook permanent verwijderen van de status van alle service. Deze bewerking kan niet ongedaan worden gemaakt en toepassingsstatus niet kan worden hersteld.

~~~
PS D:\temp> Remove-ServiceFabricApplication fabric:/MyApp

Confirm
Continue with this operation?
[Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"):
Remove application instance succeeded

PS D:\temp> Get-ServiceFabricApplication
PS D:\temp>
~~~

Wanneer een bepaalde versie van een toepassingstype niet meer nodig is, moet u deze registratie ongedaan maken met de opdracht [Registratie ServiceFabricApplicationType](https://msdn.microsoft.com/library/mt125885.aspx) . Afmelden van niet-gebruikte typen releases opslagruimte die wordt gebruikt door de inhoud van het pakket met toepassing van dat type in het archief van de installatiekopie. Een toepassingstype kan worden gemaakt, zolang geen toepassingen tegen zijn gemaakt en niet in afwachting van toepassing upgrades hiernaar wordt verwezen.

~~~
PS D:\temp> Get-ServiceFabricApplicationType

ApplicationTypeName    : DemoAppType
ApplicationTypeVersion : v1
DefaultParameters      : {}

ApplicationTypeName    : DemoAppType
ApplicationTypeVersion : v2
DefaultParameters      : {}

ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : AppManifestVersion1
DefaultParameters      : {}

PS D:\temp> Unregister-ServiceFabricApplicationType MyApplicationType AppManifestVersion1
Unregister application type succeeded

PS D:\temp> Get-ServiceFabricApplicationType

ApplicationTypeName    : DemoAppType
ApplicationTypeVersion : v1
DefaultParameters      : {}

ApplicationTypeName    : DemoAppType
ApplicationTypeVersion : v2
DefaultParameters      : {}

PS D:\temp>
~~~

## <a name="troubleshooting"></a>Het oplossen van problemen

### <a name="copy-servicefabricapplicationpackage-asks-for-an-imagestoreconnectionstring"></a>Copy ServiceFabricApplicationPackage vraagt om een ImageStoreConnectionString

De Service Fabric SDK-omgeving moet al de juiste standaardwaarden instellen. Maar als dat nodig is, de ImageStoreConnectionString voor alle opdrachten moet overeenkomen met de waarde die wordt gebruikt door de cluster-Service Fabric. U kunt dit vinden in het cluster manifest opgehaald met de opdracht [Get-ServiceFabricClusterManifest](https://msdn.microsoft.com/library/mt126024.aspx) :

~~~
PS D:\temp> Copy-ServiceFabricApplicationPackage .\MyApplicationType

cmdlet Copy-ServiceFabricApplicationPackage at command pipeline position 1
Supply values for the following parameters:
ImageStoreConnectionString:

PS D:\temp> Get-ServiceFabricClusterManifest
<ClusterManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Name="Server-Default-SingleNode" Version="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">

    [...]

    <Section Name="Management">
      <Parameter Name="ImageStoreConnectionString" Value="file:D:\ServiceFabric\Data\ImageStore" />
    </Section>

    [...]

PS D:\temp> Copy-ServiceFabricApplicationPackage .\MyApplicationType -ImageStoreConnectionString file:D:\ServiceFabric\Data\ImageStore
Copy application package succeeded

PS D:\temp>
~~~

## <a name="next-steps"></a>Volgende stappen

[Service Fabric toepassing upgrade](service-fabric-application-upgrade.md)

[Service Fabric gezondheid-Inleiding](service-fabric-health-introduction.md)

[Opsporen en oplossen van problemen met een service Service Fabric](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[Model voor een toepassing in Service Fabric](service-fabric-application-model.md)

<!--Link references--In actual articles, you only need a single period before the slash-->
[10]: service-fabric-application-model.md
[11]: service-fabric-application-upgrade.md
