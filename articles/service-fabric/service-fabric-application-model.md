<properties
   pageTitle="Service Fabric toepassingsmodel | Microsoft Azure"
   description="Het model en de toepassingen en services in Fabric Service beschrijven."
   services="service-fabric"
   documentationCenter=".net"
   authors="rwike77"
   manager="timlt"
   editor="mani-ramaswamy"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/10/2016"   
   ms.author="seanmck"/>

# <a name="model-an-application-in-service-fabric"></a>Model voor een toepassing in Service Fabric

Dit artikel bevat een overzicht van de toepassing Azure Service Fabric model. Ook wordt beschreven hoe een toepassing en via de manifest-bestanden definiëren en de toepassing verpakt en gereed is voor implementatie.

## <a name="understand-the-application-model"></a>Het application-objectmodel begrijpen

Een toepassing is een collectie van de diensten die een bepaalde functie of functies uitvoeren. Een service wordt uitgevoerd een volledige en zelfstandige functie (kunt starten en onafhankelijk van andere diensten) en bestaat uit code, configuratie en gegevens. Voor elke service, code bestaat uit de binaire uitvoerbare bestanden van configuratie bestaat uit de service-instellingen die kunnen worden geladen tijdens het uitvoeren en gegevens bestaat uit willekeurige statische gegevens kunnen worden gebruikt door de service. Elk onderdeel in dit toepassingsmodel hiërarchische kan worden versioned en bijgewerkte onafhankelijk van elkaar.

![Het toepassingsmodel Fabric-Service][appmodel-diagram]


Een toepassingstype is een categorisatie van een toepassing en bestaat uit een bundel van servicetypen. Een servicetype is een categorisatie van een service. De classificatie kan verschillende instellingen en configuraties, maar de belangrijkste functionaliteit blijft hetzelfde. De exemplaren van een service worden de verschillende configuratie variaties van hetzelfde servicetype.  

Klassen (of "types") van toepassingen en services worden beschreven via XML-bestanden (Toepassingsmanifesten en manifesten service) die zijn de sjablonen die toepassingen uit het archief van de installatiekopie van het cluster kunnen worden gemaakt. De schemadefinitie voor de ServiceManifest.xml en ApplicationManifest.xml-bestand wordt geïnstalleerd met de Service Fabric SDK en de hulpprogramma's naar *C:\Program Files\Microsoft SDKs\Service Fabric\schemas\ServiceFabricServiceModel.xsd*.

De code voor de instanties van de andere toepassing wordt als afzonderlijke processen, zelfs wanneer deze wordt gehost door hetzelfde knooppunt Fabric-Service uitgevoerd. Voorts kan de levenscyclus van elke toepassingsinstantie (d.w.z. worden beheerd upgrade) onafhankelijk van elkaar. In het volgende diagram ziet u hoe de toepassingstypen servicetype, op hun beurt bestaan uit een code, configuratie en pakketten zijn samengesteld. Ter vereenvoudiging van het diagram, alleen de code/config/gegevens-pakketten voor `ServiceType4` worden weergegeven, hoewel elk servicetype opnemen wilt of al die typen inpakken.

![Service Fabric toepassingstypen en servicetypen][cluster-imagestore-apptypes]

Twee verschillende manifest-bestanden worden gebruikt om toepassingen en services te beschrijven: het servicemanifest en het manifest van de toepassing. Deze vallen in de daaropvolgende secties gedetailleerd.

Er kan een of meer exemplaren van een type-service actief zijn in het cluster. Bijvoorbeeld bereiken stateful-exemplaren of replica's hoge betrouwbaarheid staat tussen replica's die op de andere knooppunten in het cluster worden gerepliceerd. Deze replicatie wordt in feite biedt redundantie voor de service, zelfs als een van de knooppunten in een cluster niet beschikbaar zijn. Een [gepartitioneerde service](service-fabric-concepts-partitioning.md) verder verdeelt de staat (en patronen voor toegang tot die staat) over de knooppunten in het cluster.

In het volgende diagram wordt de relatie tussen toepassingen en service-exemplaren, partities en replica's.

![Partities en replica's binnen een service][cluster-application-instances]


>[AZURE.TIP] U kunt de lay-out van toepassingen weergeven in een cluster met behulp van het hulpprogramma Service Fabric Explorer op http://&lt;yourclusteraddress&gt;: 19080/Explorer. Voor meer informatie Zie [cluster Service Fabric Explorer visualiseren](service-fabric-visualizing-your-cluster.md).

## <a name="describe-a-service"></a>Beschrijving van een service

Het servicemanifest definieert declaratief het servicetype en de versie. Deze geeft metadata service zoals servicetype eigenschappen gezondheid, load balancing-statistieken, de binaire bestanden en configuratiebestanden.  Anders gezegd, beschrijving deze van de code-, configuratie- en pakketten waaruit een servicepakket ter ondersteuning van een of meer servicetypen. Hier volgt een eenvoudig voorbeeld servicemanifest:

~~~
<?xml version="1.0" encoding="utf-8" ?>
<ServiceManifest Name="MyServiceManifest" Version="SvcManifestVersion1" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <Description>An example service manifest</Description>
  <ServiceTypes>
    <StatelessServiceType ServiceTypeName="MyServiceType" />
  </ServiceTypes>
  <CodePackage Name="MyCode" Version="CodeVersion1">
    <SetupEntryPoint>
      <ExeHost>
        <Program>MySetup.bat</Program>
      </ExeHost>
    </SetupEntryPoint>
    <EntryPoint>
      <ExeHost>
        <Program>MyServiceHost.exe</Program>
      </ExeHost>
    </EntryPoint>
  </CodePackage>
  <ConfigPackage Name="MyConfig" Version="ConfigVersion1" />
  <DataPackage Name="MyData" Version="DataVersion1" />
</ServiceManifest>
~~~

**Versie** kenmerken zijn ongestructureerde tekenreeksen en niet door het systeem worden geparseerd. Deze worden gebruikt om elk onderdeel naar versie voor upgrades.

**ServiceTypes** verklaart welke servicetypen worden ondersteund door **CodePackages** in dit manifest. Wanneer een service tegen een van deze service wordt gestart, worden alle code pakketten in dit manifest wordt aangegeven door het uitvoeren van hun ingangspunten geactiveerd. De resulterende processen worden voor het registreren van de ondersteunde servicetypen tijdens runtime verwacht. Houd er rekening mee dat servicetypen worden gedefinieerd op het manifest niveau niveau niet de code-pakket. Dus als er meerdere code-pakketten, worden alle geactiveerd wanneer het systeem wordt gezocht naar een van de aangegeven servicetypen.

**SetupEntryPoint** is een beschermde post die wordt uitgevoerd met dezelfde referenties als Service Fabric (doorgaans de account *LocalSystem* ) voordat een ingangspunt. Het uitvoerbare bestand opgegeven met **EntryPoint** is meestal de langdurige ServiceHost. De aanwezigheid van een afzonderlijke installatie van binnenkomst wordt vermeden dat de ServiceHost uitvoeren met hoge bevoegdheden voor langere tijd. Het uitvoerbare bestand opgegeven met **EntryPoint** wordt uitgevoerd nadat de **SetupEntryPoint** met succes wordt afgesloten. Het resulterende proces wordt gecontroleerd en opnieuw gestart (begin opnieuw met **SetupEntryPoint**) als het ooit wordt beëindigd of vastloopt.

**DataPackage** declareert een map met de naam van het kenmerk **Name** met willekeurige statische gegevens tijdens runtime worden verbruikt door het proces.

**ConfigPackage** declareert een map met de naam van het kenmerk **Name** met een *Settings.xml* bestand. Dit bestand bevat gedeelten van de instellingen door de gebruiker gedefinieerde, sleutel / waarde-paar dat het proces tijdens de uitvoering kan lezen. Tijdens een upgrade, als alleen de **ConfigPackage** **versie** is gewijzigd, is vervolgens het actieve proces niet gestart. In plaats daarvan wordt een retouraanroep tijdens het configuratie-instellingen hebt gewijzigd, zodat ze opnieuw kunnen worden geladen dynamisch. Hier volgt een voorbeeld van *Settings.xml* bestand:

~~~
<Settings xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">
  <Section Name="MyConfigurationSecion">
    <Parameter Name="MySettingA" Value="Example1" />
    <Parameter Name="MySettingB" Value="Example2" />
  </Section>
</Settings>
~~~

> [AZURE.NOTE] Een servicemanifest kan bevatten meerdere code-, configuratie- en gegevenspakketten. Elk van deze kan in versies bestaan onafhankelijk van elkaar.

<!--
For more information about other features supported by service manifests, refer to the following articles:

*TODO: LoadMetrics, PlacementConstraints, ServicePlacementPolicies
*TODO: Resources
*TODO: Health properties
*TODO: Trace filters
*TODO: Configuration overrides
-->

## <a name="describe-an-application"></a>Een toepassing beschrijven


Manifest van de toepassing beschrijving declaratief van de toepassing en de versie. Deze geeft service samenstelling metagegevens zoals stabiel namen, schema, exemplaar count/replicatie factor beveiliging/isolatie beleid, plaatsing, beperkingen, configuratie overschrijvingen en samenstellende servicetypen partitionering. De taakverdeling domeinen waarin de toepassing wordt geplaatst, worden ook beschreven.

Manifest van de toepassing worden op het niveau van de elementen beschreven en dus verwijst naar een of meer service manifesten voor het opstellen van een toepassingstype. Hier volgt een eenvoudig voorbeeld toepassingsmanifest:

~~~
<?xml version="1.0" encoding="utf-8" ?>
<ApplicationManifest
      ApplicationTypeName="MyApplicationType"
      ApplicationTypeVersion="AppManifestVersion1"
      xmlns="http://schemas.microsoft.com/2011/01/fabric"
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <Description>An example application manifest</Description>
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="MyServiceManifest" ServiceManifestVersion="SvcManifestVersion1"/>
  </ServiceManifestImport>
  <DefaultServices>
     <Service Name="MyService">
         <StatelessService ServiceTypeName="MyServiceType" InstanceCount="1">
             <SingletonPartition/>
         </StatelessService>
     </Service>
  </DefaultServices>
</ApplicationManifest>
~~~

Als service manifesten, **versie** kenmerken zijn ongestructureerde tekenreeksen en niet door het systeem worden geparseerd. Deze worden ook gebruikt om elk onderdeel naar versie voor upgrades.

**ServiceManifestImport** bevat verwijzingen naar service manifesten waaruit dit toepassingstype. Geïmporteerde service manifesten bepalen welke servicetypen geldig zijn binnen dit toepassingstype.

**DefaultServices** verklaart exemplaren van de service die automatisch worden gemaakt wanneer een toepassing wordt gestart tegen dit toepassingstype. Services die standaard zijn slechts een gemak en zich gedragen als normale services in alle opzichten nadat ze zijn gemaakt. Zij samen met andere diensten in het exemplaar van de toepassing worden bijgewerkt en kunnen ook worden verwijderd.

> [AZURE.NOTE] Manifest van de toepassing kan meerdere service manifest invoer en standaardservices bevatten. Elk manifest import service kan in versies bestaan onafhankelijk van elkaar.

Zie informatie over het onderhouden van de andere toepassing en serviceparameters voor afzonderlijke omgevingen, [toepassingsparameters beheren voor meerdere omgevingen](service-fabric-manage-multiple-environment-app-configuration.md).

<!--
For more information about other features supported by application manifests, refer to the following articles:

*TODO: Application parameters
*TODO: Security, Principals, RunAs, SecurityAccessPolicy
*TODO: Service Templates
-->

## <a name="package-an-application"></a>Een toepassing Inpakken

### <a name="package-layout"></a>Lay-out

Manifest van de toepassing, service, manifest(s) en andere benodigde bestanden moeten worden ingedeeld in een specifieke indeling voor het implementeren in een cluster Service Fabric. De manifesten voorbeeld in dit artikel moet worden ingedeeld in de volgende mappenstructuur:

~~~
PS D:\temp> tree /f .\MyApplicationType

D:\TEMP\MYAPPLICATIONTYPE
│   ApplicationManifest.xml
│
└───MyServiceManifest
    │   ServiceManifest.xml
    │
    ├───MyCode
    │       MyServiceHost.exe
    │
    ├───MyConfig
    │       Settings.xml
    │
    └───MyData
            init.dat
~~~

De mappen zijn naam overeenkomt met de **naam** kenmerken van elk corresponderend element. Als de servicemanifest twee code pakketten met de namen **MyCodeA** en **MyCodeB bevatte**, zou vervolgens twee mappen met dezelfde namen de benodigde binaire bestanden voor elk pakket code bevatten.

### <a name="use-setupentrypoint"></a>SetupEntryPoint gebruiken

Typische scenario's voor het gebruik van **SetupEntryPoint** zijn wanneer u moet een uitvoerbaar bestand uitvoeren voordat de service wordt gestart of moet u een bewerking met verhoogde bevoegdheden. Bijvoorbeeld:

- Instellen en het initialiseren van omgevingsvariabelen die het uitvoerbare bestand van de service nodig heeft. Dit is niet beperkt tot uitvoerbare bestanden via de Service Fabric-programmeermodellen worden geschreven. Npm.exe moet bijvoorbeeld sommige omgevingsvariabelen die is geconfigureerd voor het implementeren van een toepassing node.js.

- Toegangsbeheer instellen door het installeren van beveiligingscertificaten.

### <a name="build-a-package-by-using-visual-studio"></a>Een pakket samenstellen met behulp van Visual Studio

Als u Visual Studio 2015 gebruikt om uw toepassing te maken, kunt u de opdracht Inpakken automatisch maken van een pakket dat overeenkomt met de indeling hierboven is beschreven.

Een OLE-pakket maken, de application-project in de Solution Explorer met de rechtermuisknop en kies de opdracht pakket zoals hieronder wordt weergegeven:

![Verpakking van een toepassing met Visual Studio][vs-package-command]

Wanneer de verpakking is voltooid, zult u de locatie van het pakket in het venster **uitvoer** . Houd er rekening mee dat de verpakking stap vindt automatisch plaats als u implementeren of fouten opsporen in uw toepassing in Visual Studio.

### <a name="test-the-package"></a>Het pakket testen

De pakketstructuur lokaal via PowerShell kunt u controleren met de opdracht **Test-ServiceFabricApplicationPackage** . Met deze opdracht wordt controleren op problemen bij het parseren van manifest en controleer of alle verwijzingen. Houd er rekening mee dat deze opdracht alleen gecontroleerd of de structurele juistheid van de mappen en bestanden in het pakket. Het wordt niet gecontroleerd of een van de inhoud van een pakket code of gegevens na controle dat alle benodigde bestanden aanwezig zijn.

~~~
PS D:\temp> Test-ServiceFabricApplicationPackage .\MyApplicationType
False
Test-ServiceFabricApplicationPackage : The EntryPoint MySetup.bat is not found.
FileName: C:\Users\servicefabric\AppData\Local\Temp\TestApplicationPackage_7195781181\nrri205a.e2h\MyApplicationType\MyServiceManifest\ServiceManifest.xml
~~~

Deze fout geeft aan dat het *MySetup.bat* -bestand waarnaar wordt verwezen in het servicemanifest **SetupEntryPoint** uit het pakket code ontbreekt. Nadat het ontbrekende bestand wordt toegevoegd, wordt de toepassing controle doorgegeven:

~~~
PS D:\temp> tree /f .\MyApplicationType

D:\TEMP\MYAPPLICATIONTYPE
│   ApplicationManifest.xml
│
└───MyServiceManifest
    │   ServiceManifest.xml
    │
    ├───MyCode
    │       MyServiceHost.exe
    │       MySetup.bat
    │
    ├───MyConfig
    │       Settings.xml
    │
    └───MyData
            init.dat

PS D:\temp> Test-ServiceFabricApplicationPackage .\MyApplicationType
True
PS D:\temp>
~~~

Zodra de toepassing goed verpakt wordt en controle wordt doorgegeven, is het gereed is voor implementatie.

## <a name="next-steps"></a>Volgende stappen

[Installeren en verwijderen van toepassingen][10]

[Toepassingsparameters voor omgevingen met meerdere beheren][11]

[RunAs: Een Service Fabric-toepassing uitvoert met verschillende machtigingen][12]

<!--Image references-->
[appmodel-diagram]: ./media/service-fabric-application-model/application-model.png
[cluster-imagestore-apptypes]: ./media/service-fabric-application-model/cluster-imagestore-apptypes.png
[cluster-application-instances]: media/service-fabric-application-model/cluster-application-instances.png
[vs-package-command]: ./media/service-fabric-application-model/vs-package-command.png

<!--Link references--In actual articles, you only need a single period before the slash-->
[10]: service-fabric-deploy-remove-applications.md
[11]: service-fabric-manage-multiple-environment-app-configuration.md
[12]: service-fabric-application-runas-security.md
