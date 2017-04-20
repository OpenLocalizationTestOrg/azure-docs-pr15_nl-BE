<properties
   pageTitle="Implementeren van een bestaande uitvoerbaar bestand naar Azure Service Fabric | Microsoft Azure"
   description="Stapsgewijze instructies voor het inpakken van een bestaande toepassing als gast uitvoerbaar, zodat deze kan worden toegepast op een cluster Service Fabric"
   services="service-fabric"
   documentationCenter=".net"
   authors="msfussell"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="na"
   ms.date="10/22/2016"
   ms.author="msfussell;mikhegn"/>

# <a name="deploy-a-guest-executable-to-service-fabric"></a>Uitvoerbare Gast aan Fabric Service implementeren

U kunt elk type toepassing zoals node.js, Java of eigen toepassingen uitvoeren in Azure Service Fabric. Service Fabric verwijst naar dit soort toepassingen als gast uitvoerbare bestanden.
Gast uitvoerbare bestanden worden behandeld door Fabric Service zoals stateless services. Hierdoor worden deze geplaatst op de knooppunten in een cluster, op basis van beschikbaarheid en andere criteria. In dit artikel wordt beschreven hoe verpakken en distribueren van uitvoerbare Gast aan een cluster Service Fabric, met behulp van Visual Studio of een hulpprogramma voor de opdrachtregel.

In dit artikel dekken we de stappen voor het pakket uitvoerbare Gast en deze implementeren aan de Fabric-Service.  

## <a name="benefits-of-running-a-guest-executable-in-service-fabric"></a>Voordelen van de Gast uitvoerbaar in Service Fabric

Er zijn verschillende voordelen die deel uitmaken van de Gast in een cluster Service Fabric uitvoerbaar wordt uitgevoerd:

- Hoge beschikbaarheid. Toepassingen die worden uitgevoerd in Service Fabric uiterst beschikbaar gesteld. Fabric-service zorgt ervoor dat exemplaren van een toepassing worden uitgevoerd.
- Statuscontrole. Statuscontrole Fabric-service wordt gedetecteerd als een toepassing wordt uitgevoerd en diagnostische informatie biedt als er een storing.   
- Levenscyclusbeheer voor uw toepassing. Fabric-Service biedt naast bieden geen downtime upgrades, automatische terugdraaien naar de vorige versie als er een slechte gezondheid gebeurtenis gemeld tijdens een upgrade.    
- Dichtheid. U kunt meerdere toepassingen uitvoeren in een cluster hebben voor elke toepassing uit te voeren op zijn eigen hardware.


## <a name="overview-of-application-and-service-manifest-files"></a>Overzicht van de toepassing en service manifest-bestanden

Als onderdeel van de implementatie van een gast uitvoerbaar, is het nuttig om te begrijpen van de verpakking Fabric-Service en implementatiemodel als [toepassingsmodel](service-fabric-application-model.md)beschreven. Het model van de verpakking Fabric-Service is afhankelijk van twee XML-bestanden: de manifesten van toepassing en service. De schemadefinitie voor de bestanden ApplicationManifest.xml en ServiceManifest.xml wordt geïnstalleerd met de SDK Service Fabric in *C:\Program Files\Microsoft SDKs\Service Fabric\schemas\ServiceFabricServiceModel.xsd*.

* **Manifest van de toepassing** Manifest van de toepassing wordt gebruikt voor het beschrijven van de toepassing. Hiermee geeft u de services die deze en andere parameters die worden gebruikt om te definiëren hoe de een of meer services moeten worden geïmplementeerd, zoals het aantal exemplaren opstellen.

  In het weefsel van de Service is een toepassing een eenheid van implementaties en upgrades. Een toepassing kan worden bijgewerkt als een eenheid, waarbij mogelijke fouten en mogelijke terugdraaiversies worden beheerd. Fabric-service zorgt ervoor dat het proces is een succes, of als de upgrade mislukt, verlaat niet de toepassing onbekend/instabiel.

* **Manifest van de service** Het servicemanifest beschrijving van de onderdelen van een service. Gegevens, zoals de naam en het type service en de code, configuratie en gegevens bevat. Het servicemanifest bevat tevens enkele aanvullende parameters die kunnen worden gebruikt voor het configureren van de service nadat deze is geïmplementeerd.


## <a name="application-package-file-structure"></a>Toepassing pakket bestandsstructuur
De toepassing moet als volgt een vooraf gedefinieerde mapstructuur voor de implementatie van een toepassing aan de Fabric-Service. In het volgende voorbeeld van die structuur.

```
|-- ApplicationPackageRoot
  	|-- GuestService1Pkg
        |-- Code
            |-- existingapp.exe
        |-- Config
            |-- Settings.xml
        |-- Data
        |-- ServiceManifest.xml
  	|-- ApplicationManifest.xml
```

De ApplicationPackageRoot bevat het bestand ApplicationManifest.xml dat de toepassing definieert. Een submap voor elke service die is opgenomen in de toepassing wordt gebruikt om alle de artefacten die de service nodig heeft, de ServiceManifest.xml en normaal gesproken de volgende drie mappen bevatten:

- *Code*. Deze map bevat de servicecode.
- *Config*. Deze map bevat een bestand Settings.xml (en andere bestanden indien nodig) de service toegankelijk tijdens het uitvoeren voor het ophalen van specifieke configuratie-instellingen.
- *Gegevens*. Dit is een extra map voor het opslaan van extra lokale gegevens die de service mogelijk nodig. Opmerking: Gegevens moet worden gebruikt voor alleen tijdelijke gegevens opgeslagen. Fabric-service wordt niet kopiëren/repliceren wijzigingen in de map data als de service worden verplaatst moet, bijvoorbeeld tijdens een overname.

Opmerking: U hoeft niet voor het maken van de `config` en `data` mappen als u deze niet nodig hebt.

## <a name="packaging-an-existing-executable"></a>Verpakking van een bestaande uitvoerbaar bestand

Wanneer de verpakking een gast uitvoerbaar bestand, kunt u ofwel een Visual Studio-project-sjabloon of [het toepassingspakket handmatig maken](#manually). Met behulp van Visual Studio, worden de toepassing pakketstructuur en de manifest-bestanden gemaakt door de wizard Nieuw project voor u.

>[AZURE.NOTE] De eenvoudigste manier om een bestaande Windows uitvoerbaar in een service pakket is het gebruik van Visual Studio.

## <a name="using-visual-studio-to-package-an-existing-executable"></a>Een bestaande uitvoerbaar bestand inpakken met behulp van Visual Studio

Visual Studio biedt een Service Fabric service-sjabloon waarmee u uitvoerbare Gast implementeren op een cluster Service Fabric.

Doorloop de volgende stappen voor het voltooien van de publicatie:

1. Kies Bestand -> Nieuw Project en een Service Fabric-toepassing maken.
2. Gast uitvoerbaar bestand kiezen als servicesjabloon voor de.
3. Klik op Bladeren en selecteer de map met het uitvoerbare bestand en vul de rest van de parameters om de service te maken.
    - *Code pakketgedrag* kan worden ingesteld op de inhoud van de map kopiëren naar de Visual Studio-Project, hetgeen handig is als het uitvoerbare bestand wordt niet gewijzigd. Als u verwacht het uitvoerbare bestand dat en de mogelijkheid om op te pikken nieuwe builds dynamisch wilt wijzigen, kunt u in plaats daarvan een koppeling naar de map. Houd er rekening mee dat u gekoppelde mappen gebruiken kunt bij het maken van de application-project in Visual Studio. Deze koppelingen naar de bronlocatie van binnen het project, waardoor u de Gast uitvoerbaar in de bestemming bron bijwerken met de updates die deel uitmaken van het toepassingspakket op build.
    - *Programma* - Kies het uitvoerbare bestand dat moet worden uitgevoerd om de service te starten.
    - *Argumenten* - waarden opgeven die moeten worden doorgegeven aan het uitvoerbare bestand. Een lijst met parameters met argumenten kan zijn.
    - *WorkingFolder* - Hiermee geeft u de werkmap voor het proces dat u wilt starten. U kunt drie waarden opgeven:
        - `CodeBase`Hiermee geeft u aan dat de werkmap zal worden ingesteld op de codemap in het programmapakket (`Code` map in de structuur van het voorgaande weergegeven).
        - `CodePackage`Hiermee geeft u aan dat de werkmap is opgegeven om te worden naar de hoofdmap van het toepassingspakket (`GuestService1Pkg` in de structuur van het voorgaande weergegeven).
        - `Work`Hiermee geeft u aan dat de bestanden worden opgeslagen in de submap werk
4. De service een naam en klik op OK.
5. Als uw service een eindpunt voor communicatie moet, kunt u nu het Protocol, de poort en het Type toevoegen aan het bestand ServiceManifest.xml. e.g. `<Endpoint Name="NodeAppTypeEndpoint" Protocol="http" Port="3000" UriScheme="http" PathSuffix="myapp/" Type="Input" />`.
6. U kunt nu het pakket en actie tegen het lokale cluster publiceren door het opsporen van fouten in de oplossing in Visual Studio. Als u klaar bent u de toepassing uitgeeft aan een cluster extern of selectievakje in de oplossing voor het besturingselement.
7. Ga naar het einde van dit artikel om te zien hoe u Gast uitvoerbare service die wordt uitgevoerd in Service Fabric Explorer bekijken.

<a id="manually"></a>
## <a name="manually-packaging-and-deploying-an-existing-executable"></a>Handmatig verpakken en implementeren van een bestaande uitvoerbaar bestand
Het proces van het handmatig verpakking een gast uitvoerbaar bestand is gebaseerd op de volgende stappen uit:

1. Maak de mapstructuur van het pakket.
2. De bestanden en de configuratie van de toepassing toevoegen.
3. Het manifestbestand van service bewerken.
4. Het manifestbestand toepassing bewerken.

<!--
>[AZURE.NOTE] We do provide a packaging tool that allows you to create the ApplicationPackage automatically. The tool is currently in preview. You can download it from [here](http://aka.ms/servicefabricpacktool).
-->

### <a name="create-the-package-directory-structure"></a>Maak de mapstructuur van het pakket
Zoals eerder is beschreven kunt u starten door de mapstructuur maken.

### <a name="add-the-applications-code-and-configuration-files"></a>Van de toepassing en de configuratie-bestanden toevoegen
Nadat u de directory-structuur hebt gemaakt, kunt u de code van de toepassing en configuratiebestanden onder de code en config-mappen toevoegen. U kunt ook aanvullende mappen of submappen onder de code of config mappen maken.

Fabric-service heeft een xcopy van de inhoud van de hoofdmap van de toepassing, dus er geen andere is dan de twee bovenste mappen, code en instellingen maken gebruik van vooraf gedefinieerde structuur. (Kunt u verschillende namen als u wilt. Meer details zijn in het volgende gedeelte.)

>[AZURE.NOTE] Zorg ervoor dat u alle bestanden/afhankelijkheden die voor de toepassing nodig. Service Fabric kopieert de inhoud van het toepassingspakket op alle knooppunten in het cluster waar services van de toepassing zal worden geïmplementeerd. Het pakket moet bevatten de code die de toepassing moet worden uitgevoerd. Wij raden niet aangenomen dat de afhankelijkheden zijn al geïnstalleerd.

### <a name="edit-the-service-manifest-file"></a>Het manifestbestand van service bewerken
De volgende stap is voor het bewerken van het manifestbestand van de service met de volgende informatie:

- De naam van het servicetype. Dit is een ID die Service-structuur wordt gebruikt om een service te identificeren.
- De opdracht die moet worden gebruikt voor het starten van de toepassing (ExeHost).
- Een script dat moet worden uitgevoerd om in te stellen omhoog/configureren van de toepassing (SetupEntrypoint).

Het volgende is een voorbeeld van een `ServiceManifest.xml` bestand:

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Name="NodeApp" Version="1.0.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <ServiceTypes>
      <StatelessServiceType ServiceTypeName="NodeApp" UseImplicitHost="true"/>
   </ServiceTypes>
   <CodePackage Name="code" Version="1.0.0.0">
      <SetupEntryPoint>
         <ExeHost>
             <Program>scripts\launchConfig.cmd</Program>
         </ExeHost>
      </SetupEntryPoint>
      <EntryPoint>
         <ExeHost>
            <Program>node.exe</Program>
            <Arguments>bin/www</Arguments>
            <WorkingFolder>CodePackage</WorkingFolder>
         </ExeHost>
      </EntryPoint>
   </CodePackage>
   <Resources>
      <Endpoints>
         <Endpoint Name="NodeAppTypeEndpoint" Protocol="http" Port="3000" Type="Input" />
      </Endpoints>
   </Resources>
</ServiceManifest>
```

We gaan over de verschillende onderdelen van het bestand dat u wilt bijwerken:

#### <a name="updating-the-servicetypes"></a>De ServiceTypes bijwerken

```xml
<ServiceTypes>
  <StatelessServiceType ServiceTypeName="NodeApp" UseImplicitHost="true" />
</ServiceTypes>
```

- U kunt kiezen de naam die u wilt gebruiken voor `ServiceTypeName`. De waarde die wordt gebruikt in de `ApplicationManifest.xml` bestand om de service te identificeren.
- U moet opgeven `UseImplicitHost="true"`. Dit kenmerk instrueert Service weefsel dat de service is gebaseerd op een zelfstandige toepassing, zodat alle Service weefsel moet gebeuren als een proces te starten en controleren van de gezondheid.

#### <a name="updating-the-codepackage"></a>De CodePackage bijwerken
Het element CodePackage geeft de locatie (en versie) van de code van de service.

```xml
<CodePackage Name="Code" Version="1.0.0.0">
```

De `Name` element geeft u de naam van de map in toepassingspakket voor de de code van de service bevat wordt gebruikt. `CodePackage`ook heeft de `version` kenmerk. Dit kan worden gebruikt om op te geven van de versie van de code-- en mogelijk ook van de service om code te upgraden met behulp van Service-Fabric application lifecycle management infrastructuur kan worden gebruikt.
#### <a name="optional-updating-the-setupentrypoint"></a>Optioneel: De SetupEntrypoint bijwerken

```xml
<SetupEntryPoint>
   <ExeHost>
       <Program>scripts\launchConfig.cmd</Program>
   </ExeHost>
</SetupEntryPoint>
```
Het element SetupEntrypoint wordt gebruikt voor alle uitvoerbare bestanden of batchbestanden bestand opgeven dat moet worden uitgevoerd voordat de code van de service wordt gestart. Dit is een optionele stap, dus u hoeft niet te worden opgenomen als er geen initialisatie/setup vereist. De SetupEntryPoint wordt uitgevoerd elke keer dat de service opnieuw wordt gestart.

Er is slechts één SetupEntrypoint, zodat setup/config scripts worden gegroepeerd in een enkele batch-bestand moeten als de toepassing setup/config meerdere scripts nodig. De SetupEntrypoint kan elk gewenst type bestand--uitvoerbare bestanden, batchbestanden en PowerShell-cmdlets kunt uitvoeren. Zie [SetupEntryPoint configureren](service-fabric-application-runas-security.md)voor meer informatie.

In het voorgaande voorbeeld de SetupEntrypoint wordt uitgevoerd een batchbestand met de naam `LaunchConfig.cmd` die zich de `scripts` submap van de codemap (ervan uitgaande dat het element WorkingFolder op CodeBase is ingesteld).

#### <a name="updating-the-entrypoint"></a>Het toegangspunt activeerde bijwerken

```xml
<EntryPoint>
  <ExeHost>
    <Program>node.exe</Program>
    <Arguments>bin/www</Arguments>
    <WorkingFolder>CodeBase</WorkingFolder>
  </ExeHost>
</EntryPoint>
```

De `Entrypoint` -element in het manifestbestand van de service wordt gebruikt om aan te geven hoe u de service te starten. De `ExeHost` element geeft het uitvoerbare bestand (en argumenten) die moet worden gebruikt voor het starten van de service.

- `Program`Hiermee geeft u de naam van het uitvoerbare bestand dat moet worden uitgevoerd om de service te starten.
- `Arguments`Hiermee geeft u de argumenten die moeten worden doorgegeven aan het uitvoerbare bestand. Een lijst met parameters met argumenten kan zijn.
- `WorkingFolder`Hiermee geeft u de werkmap voor het proces dat u wilt starten. U kunt drie waarden opgeven:
    - `CodeBase`Hiermee geeft u aan dat de werkmap zal worden ingesteld op de codemap in het programmapakket (`Code` map in de structuur van het voorgaande).
    - `CodePackage`Hiermee geeft u aan dat de werkmap is opgegeven om te worden naar de hoofdmap van het toepassingspakket (`GuestService1Pkg` in de structuur van het voorgaande).
  - `Work`Hiermee geeft u aan dat de bestanden worden opgeslagen in de submap werk

De WorkingFolder is handig voor het instellen van de juiste werkmap zodat relatieve paden door de toepassing of de initialisatie scripts kunnen worden gebruikt.

#### <a name="updating-the-endpoints-and-registering-with-naming-service-for-communication"></a>De eindpunten bijwerken en registreren bij Naming Service voor communicatie

```xml
<Endpoints>
   <Endpoint Name="NodeAppTypeEndpoint" Protocol="http" Port="3000" Type="Input" />
</Endpoints>

```
In het voorgaande voorbeeld de `Endpoint` element geeft de eindpunten die de toepassing kunt luisteren. In dit voorbeeld wordt de toepassing Node.js luistert naar http op poort 3000.

Bovendien vraagt u Fabric-Service dit eindpunt naar de Naming Service publiceren zodat andere diensten adres van het eindpunt voor deze service kunnen ontdekken. Hiermee kunt u communiceren tussen services die gast uitvoerbare bestanden.
Het adres van de gepubliceerde eindpunt is van het formulier `UriScheme://IPAddressOrFQDN:Port/PathSuffix`. `UriScheme`en `PathSuffix` zijn optionele kenmerken. `IPAddressOrFQDN`is het IP-adres of volledig gekwalificeerde domeinnaam van het knooppunt dit uitvoerbare bestand op wordt geplaatst en wordt voor u berekend.

In het volgende voorbeeld zodra de service wordt geïmplementeerd, in de Service Fabric Explorer ziet u een eindpunt vergelijkbaar met `http://10.1.4.92:3000/myapp/` gepubliceerd voor de service-exemplaar of een lokale computer, u ziet als het `http://localhost:3000/myapp/`. 

```xml
<Endpoints>
   <Endpoint Name="NodeAppTypeEndpoint" Protocol="http" Port="3000"  UriScheme="http" PathSuffix="myapp/" Type="Input" />
</Endpoints>
```
Deze adressen kunt u bij de [reverse-proxy](service-fabric-reverseproxy.md) voor de communicatie tussen services.

### <a name="edit-the-application-manifest-file"></a>Het manifestbestand voor een toepassing bewerken

Nadat u hebt geconfigureerd de `Servicemanifest.xml` -bestand, moet u enkele wijzigingen aanbrengen in de `ApplicationManifest.xml` bestand om ervoor te zorgen dat de juiste service en de naam worden gebruikt.

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="NodeAppType" ApplicationTypeVersion="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="NodeApp" ServiceManifestVersion="1.0.0.0" />
   </ServiceManifestImport>
</ApplicationManifest>
```

#### <a name="servicemanifestimport"></a>ServiceManifestImport

In de `ServiceManifestImport` -element, kunt u een of meer services die u wilt opnemen in de app. Services wordt verwezen met `ServiceManifestName`, die de naam aangeeft van de map waarin de `ServiceManifest.xml` -bestand zich bevindt.

```xml
<ServiceManifestImport>
  <ServiceManifestRef ServiceManifestName="NodeApp" ServiceManifestVersion="1.0.0.0" />
</ServiceManifestImport>
```

## <a name="set-up-logging"></a>Logboekregistratie instellen
Gast uitvoerbare bestanden is het handig om te kunnen zien van de console Logboeken om na te gaan als de scripts toepassings- en eventuele fouten weergegeven.
Console-omleiding kan worden geconfigureerd de `ServiceManifest.xml` het bestand met de `ConsoleRedirection` element.

```xml
<EntryPoint>
  <ExeHost>
    <Program>node.exe</Program>
    <Arguments>bin/www</Arguments>
    <WorkingFolder>CodeBase</WorkingFolder>
    <ConsoleRedirection FileRetentionCount="5" FileMaxSizeInKb="2048"/>
  </ExeHost>
</EntryPoint>
```

* `ConsoleRedirection`kan worden gebruikt om de console-uitvoer (stdout en stderr) omgeleid naar een werkmap zodat deze kunnen worden gebruikt om te controleren of er geen fouten tijdens de installatie of uitvoering van de toepassing in het cluster Service Fabric.

    * `FileRetentionCount`Hiermee bepaalt u hoeveel bestanden worden opgeslagen in de werkmap. Een waarde van 5, betekent bijvoorbeeld dat de logboekbestanden voor de vorige vijf uitvoeringen worden opgeslagen in de werkmap.
    * `FileMaxSizeInKb`Hiermee geeft u de maximumgrootte van de logboekbestanden.

Logboekbestanden worden opgeslagen in een van de mappen van de dienst werken. Om te bepalen waar de bestanden zich bevinden, moet u de Service Fabric Explorer gebruiken om te bepalen welk knooppunt dat op de service wordt uitgevoerd en welke werkmap wordt gebruikt. Dit proces wordt verderop in dit artikel vallen.

## <a name="deployment"></a>Implementatie
De laatste stap wordt de toepassing wordt geïmplementeerd. De volgende PowerShell script laat zien hoe uw toepassing aan de plaatselijke ontwikkeling cluster implementeren en een nieuwe Service Fabric-service starten.

```PowerShell

Connect-ServiceFabricCluster localhost:19000

Write-Host 'Copying application package...'
Copy-ServiceFabricApplicationPackage -ApplicationPackagePath 'C:\Dev\MultipleApplications' -ImageStoreConnectionString 'file:C:\SfDevCluster\Data\ImageStoreShare' -ApplicationPackagePathInImageStore 'nodeapp'

Write-Host 'Registering application type...'
Register-ServiceFabricApplicationType -ApplicationPathInImageStore 'nodeapp'

New-ServiceFabricApplication -ApplicationName 'fabric:/nodeapp' -ApplicationTypeName 'NodeAppType' -ApplicationTypeVersion 1.0

New-ServiceFabricService -ApplicationName 'fabric:/nodeapp' -ServiceName 'fabric:/nodeapp/nodeappservice' -ServiceTypeName 'NodeApp' -Stateless -PartitionSchemeSingleton -InstanceCount 1

```
Een weefsel van Service-service kan worden geïmplementeerd in verschillende "configuraties." Bijvoorbeeld, als één of meerdere keren kan worden geïnstalleerd of op zodanige wijze dat er een exemplaar van de service op elk knooppunt van de cluster-Service Fabric wordt kan worden geïnstalleerd.

De `InstanceCount` -parameter van de `New-ServiceFabricService` cmdlet wordt gebruikt om aan te geven hoeveel exemplaren van de service wordt gestart in de cluster-Service Fabric. U kunt instellen de `InstanceCount` -waarde, afhankelijk van het type toepassing die u wilt implementeren. De twee meest voorkomende scenario's zijn:

* `InstanceCount = "1"`. In dit geval wordt slechts één exemplaar van de service geïmplementeerd in het cluster. De Taakplanner-service-structuur bepaalt welk knooppunt de service zal worden ingezet op.

* `InstanceCount ="-1"`. In dit geval is één exemplaar van de service op elk knooppunt in het cluster Service Fabric geïmplementeerd. Het resultaat is één (en slechts één) exemplaar van de service voor elk knooppunt in het cluster hebben.

Dit is een nuttige configuratie voor front-end toepassingen (bijvoorbeeld een eindpunt REST) omdat u clienttoepassingen verbinding moeten maken "" met een van de knooppunten in het cluster te gebruiken van het eindpunt. Deze configuratie kan ook worden gebruikt wanneer u bijvoorbeeld alle knooppunten van de cluster-Service Fabric zijn verbonden met een load balancer verkeer kan worden verdeeld over de service die wordt uitgevoerd op alle knooppunten in het cluster.

## <a name="check-your-running-application"></a>Controleer de actieve toepassing.

Identificeren in Service Fabric Explorer het knooppunt waarop de service wordt uitgevoerd. In dit voorbeeld wordt uitgevoerd op knooppunt1:

![Knooppunt waarop de service wordt uitgevoerd](./media/service-fabric-deploy-existing-app/nodeappinsfx.png)

Als u navigeert u naar het knooppunt en blader naar de toepassing, ziet u het knooppunt essentiële informatie, met inbegrip van de locatie op schijf.

![Locatie op schijf](./media/service-fabric-deploy-existing-app/locationondisk2.png)

Als u naar de map bladert met behulp van Server Explorer, vindt u de werkmap en de map voor het logboek van de service zoals in de volgende afbeelding wordt weergegeven.

![Locatie van logboekbestand](./media/service-fabric-deploy-existing-app/loglocation.png)


## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u geleerd hoe een gast uitvoerbaar bestand en deze aan de Fabric-Service installeren. Een volgende stap kunt u de extra inhoud voor dit onderwerp uitchecken.

- [Voorbeeld voor het verpakken en distribueren van Gast uitvoerbaar op GitHub](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/tree/master/GuestExe/SimpleApplication), inclusief een koppeling naar de voorlopige versie van het hulpprogramma voor verpakking
- [Implementatie van meerdere Gast uitvoerbare bestanden](service-fabric-deploy-multiple-apps.md)
- [De eerste Service Fabric toepassing maken met Visual Studio](service-fabric-create-your-first-application-in-visual-studio.md)
