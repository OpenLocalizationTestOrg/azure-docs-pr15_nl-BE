<properties
   pageTitle="Implementeren van een Node.js-toepassing die gebruikmaakt van MongoDB | Microsoft Azure"
   description="Stapsgewijze instructies voor het verpakken van meerdere Gast uitvoerbare bestanden implementeren op een Azure Service Fabric '-cluster"
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
   ms.workload="NA"
   ms.date="10/22/2016"
   ms.author="msfussell;mikhegn"/>


# <a name="deploy-multiple-guest-executables"></a>Implementatie van meerdere Gast uitvoerbare bestanden

In dit artikel ziet u hoe verpakken en distribueren van meerdere Gast uitvoerbare bestanden naar Azure Service Fabric. Lees voor het maken en implementeren van één pakket Fabric-Service implementatie van [uitvoerbaar aan Service Fabric Gast](service-fabric-deploy-existing-app.md).

Tijdens deze procedure ziet u hoe een toepassing met Node.js front-endgedeelte MongoDB als het gegevensarchief wordt gebruikt, kunt u de stappen voor elke toepassing die afhankelijk van een andere toepassing is kunt toepassen.   

U kunt Visual Studio het programmapakket met meerdere Gast uitvoerbare bestanden produceren. Zie [Met behulp van Visual Studio een bestaande toepassing inpakken](service-fabric-deploy-existing-app.md#using-visual-studio-to-package-an-existing-executable). Nadat u de eerste Gast uitvoerbaar bestand hebt toegevoegd, klik met de rechtermuisknop op het toepassingsproject en selecteer de **Add -> nieuwe Service Fabric-service** de tweede Gast uitvoerbaar project toevoegen aan de oplossing. Opmerking: Als u kiest voor het koppelen van de bron in het project van Visual Studio, Visual Studio-oplossing bouwen zorgt ervoor dat het toepassingspakket bijgewerkt met de wijzigingen in het bronprogramma is. 

## <a name="manually-package-the-multiple-guest-executable-application"></a>Handmatig de meerdere Gast uitvoerbare toepassing Inpakken
Ook kunt u handmatig de Gast uitvoerbaar pakket. Voor de handmatige verpakking wordt dit artikel het hulpprogramma Service Fabric verpakking, dat beschikbaar op [http://aka.ms/servicefabricpacktool is](http://aka.ms/servicefabricpacktool).

### <a name="packaging-the-nodejs-application"></a>De toepassing Node.js verpakking
In dit artikel wordt ervan uitgegaan dat Node.js niet is geïnstalleerd op de knooppunten in de cluster-Service Fabric. Als gevolg hiervan moet u Node.exe toevoegen aan de hoofdmap van de toepassing van het knooppunt vóór verpakking. De mapstructuur van de toepassing van de Node.js (via Express web framework en de engine Jade sjabloon) moet uitzien hieronder:

```
|-- NodeApplication
  	|-- bin
        |-- www
  	|-- node_modules
        |-- .bin
        |-- express
        |-- jade
        |-- etc.
  	|-- public
        |-- images
        |-- etc.
  	|-- routes
        |-- index.js
        |-- users.js
  	|-- views
        |-- index.jade
        |-- etc.
  	|-- app.js
  	|-- package.json
  	|-- node.exe
```

Een volgende stap kunt u een pakket van toepassing voor de toepassing van Node.js maken. De volgende code maakt een toepassingspakket Fabric-Service met de toepassing Node.js.

```
.\ServiceFabricAppPackageUtil.exe /source:'[yourdirectory]\MyNodeApplication' /target:'[yourtargetdirectory] /appname:NodeService /exe:'node.exe' /ma:'bin/www' /AppType:NodeAppType
```

Hieronder volgt een beschrijving van de parameters die worden gebruikt:

- **/Source** verwijst naar de map van de toepassing die moet worden verpakt.
- **/ Target** wordt gedefinieerd voor de map waarin het pakket moet worden gemaakt. Deze map moet afwijken van de bronmap.
- **/ AppName** definieert de naam van de toepassing van de bestaande toepassing. Het is belangrijk te begrijpen dat dit komt neer op de servicenaam in het manifest en niet op de naam van de Service-Fabric.
- **exe** definieert het uitvoerbare bestand dat Fabric-Service moet worden gestart, in dit geval `node.exe`.
- **/ma** bepaalt het argument dat wordt gebruikt voor het starten van het uitvoerbare bestand. Als Node.js niet is geïnstalleerd, Fabric-Service moet de webserver Node.js starten door het uitvoeren van `node.exe bin/www`.  `/ma:'bin/www'`gebruik van het hulpprogramma verpakking vertelt `bin/ma` als het argument voor node.exe.
- **/AppType** definieert de naam van de Service Fabric type.

Als u naar de map die is opgegeven in de parameter/target bladert, kunt u zien dat het hulpprogramma een volledig functionerende Service Fabric-pakket heeft gemaakt, zoals hieronder wordt weergegeven:

```
|--[yourtargetdirectory]
  	|-- NodeApplication
        |-- C
              |-- bin
              |-- data
              |-- node_modules
              |-- public
              |-- routes
              |-- views
              |-- app.js
              |-- package.json
              |-- node.exe
        |-- config
              |--Settings.xml
        |-- ServiceManifest.xml
  	|-- ApplicationManifest.xml
```
De gegenereerde ServiceManifest.xml heeft nu een gedeelte waarin wordt beschreven hoe u de webserver Node.js wordt gestart, zoals in het onderstaande stukje code:

```xml
<CodePackage Name="C" Version="1.0">
    <EntryPoint>
        <ExeHost>
            <Program>node.exe</Program>
            <Arguments>'bin/www'</Arguments>
            <WorkingFolder>CodePackage</WorkingFolder>
        </ExeHost>
    </EntryPoint>
</CodePackage>
```
In dit voorbeeld wordt luistert de webserver Node.js op poort 3000, dus u de eindpunt-informatie in het bestand ServiceManifest.xml moet als volgt bijwerken.   

```xml
<Resources>
      <Endpoints>
        <Endpoint Name="NodeServiceEndpoint" Protocol="http" Port="3000" Type="Input" />
      </Endpoints>
</Resources>
```
### <a name="packaging-the-mongodb-application"></a>De toepassing MongoDB verpakking
Nu u de toepassing Node.js hebt ingepakt, kunt u doorgaan en MongoDB pakket. De stappen die u nu hebt doorlopen, zijn niet specifiek voor Node.js en MongoDB zoals eerder. In feite, zij gelden voor alle toepassingen die zijn bedoeld als een Service Fabric toepassing samen worden gebundeld.  

U wilt als u wilt inpakken MongoDB, om ervoor te zorgen dat u inpakt, Mongod.exe en Mongo.exe. Zowel binaire bestanden bevinden zich in de `bin` directory van de installatiemap van MongoDB. De directory-structuur lijkt op onderstaand.

```
|-- MongoDB
  	|-- bin
        |-- mongod.exe
        |-- mongo.exe
        |-- anybinary.exe
```
Fabric-service moet MongoDB beginnen met een vergelijkbaar met de opdracht hieronder, dus u hoeft te gebruiken de `/ma` parameter wanneer MongoDB verpakking.

```
mongod.exe --dbpath [path to data]
```
> [AZURE.NOTE] De gegevens is niet in het geval van een storing in het knooppunt wordt behouden als u de map MongoDB-gegevens op de lokale map van het knooppunt. U moet duurzame opslag gebruiken of implementeren van een replicaset MongoDB om gegevensverlies te voorkomen.  

We in PowerShell of de opdrachtshell, het hulpprogramma verpakking uitgevoerd met de volgende parameters:

```
.\ServiceFabricAppPackageUtil.exe /source: [yourdirectory]\MongoDB' /target:'[yourtargetdirectory]' /appname:MongoDB /exe:'bin\mongod.exe' /ma:'--dbpath [path to data]' /AppType:NodeAppType
```

U moet MongoDB toevoegen aan uw toepassingspakket Service Fabric, zorg ervoor dat de punten van de parameter/target op in dezelfde map als de toepassing al met het manifest samen met de toepassing Node.js. U moet ook om ervoor te zorgen dat u dezelfde naam ApplicationType gebruikt.

We Ga naar de map en bekijk wat het programma is gemaakt.

```
|--[yourtargetdirectory]
  	|-- MyNodeApplication
  	|-- MongoDB
        |-- C
            |--bin
                |-- mongod.exe
                |-- mongo.exe
                |-- etc.
        |-- config
            |--Settings.xml
        |-- ServiceManifest.xml
  	|-- ApplicationManifest.xml
```
Zoals u ziet, het hulpprogramma MongoDB, een nieuwe map toegevoegd aan de map waarin u de binaire bestanden van MongoDB. Als u opent de `ApplicationManifest.xml` -bestand, kunt u zien dat het pakket nu de Node.js toepassing en de MongoDB bevat. De volgende code bevat de inhoud van het manifest van de toepassing.

```xml
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="MyNodeApp" ApplicationTypeVersion="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="MongoDB" ServiceManifestVersion="1.0" />
   </ServiceManifestImport>
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="NodeService" ServiceManifestVersion="1.0" />
   </ServiceManifestImport>
   <DefaultServices>
      <Service Name="MongoDBService">
         <StatelessService ServiceTypeName="MongoDB">
            <SingletonPartition />
         </StatelessService>
      </Service>
      <Service Name="NodeServiceService">
         <StatelessService ServiceTypeName="NodeService">
            <SingletonPartition />
         </StatelessService>
      </Service>
   </DefaultServices>
</ApplicationManifest>  
```

### <a name="publishing-the-application"></a>Publiceren van de toepassing
De laatste stap wordt de toepassing uitgeeft aan de lokale Service Fabric '-cluster met behulp van de onderstaande PowerShell scripts:

```
Connect-ServiceFabricCluster localhost:19000

Write-Host 'Copying application package...'
Copy-ServiceFabricApplicationPackage -ApplicationPackagePath '[yourtargetdirectory]' -ImageStoreConnectionString 'file:C:\SfDevCluster\Data\ImageStoreShare' -ApplicationPackagePathInImageStore 'NodeAppType'

Write-Host 'Registering application type...'
Register-ServiceFabricApplicationType -ApplicationPathInImageStore 'NodeAppType'

New-ServiceFabricApplication -ApplicationName 'fabric:/NodeApp' -ApplicationTypeName 'NodeAppType' -ApplicationTypeVersion 1.0  
```

De toepassing naar het lokale cluster is gepubliceerd, kunt u de toepassing Node.js op de poort die we hebben ingevoerd in het servicemanifest van de Node.js-toepassing bijvoorbeeld http://localhost:3000 openen.

In deze zelfstudie hebt u twee bestaande toepassingen gemakkelijk Inpakken als één Service Fabric toepassing gezien. U hebt ook geleerd aan Fabric Service implementeren, zodat zij van bepaalde functies van het Service-structuur, zoals hoge beschikbaarheid en integratie van de gezondheid profiteren kan.

## <a name="next-steps"></a>Volgende stappen

- Klik hier voor informatie over het implementeren van recipiënten, [containers en Service](service-fabric-containers-overview.md) -overzicht
