<properties
   pageTitle="Service Fabric eindpunten opgeven | Microsoft Azure"
   description="Het beschrijven van resources in een servicemanifest, met inbegrip van HTTPS eindpunten instellen eindpunt"
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

# <a name="specify-resources-in-a-service-manifest"></a>Bronnen opgeven in een servicemanifest

## <a name="overview"></a>Overzicht

Het servicemanifest kunt resources die worden gebruikt door de service worden gedefinieerd of gewijzigd zonder de gecompileerde code. Azure Service Fabric ondersteunt configuratie van eindpunt resources voor de service. De toegang tot de bronnen die zijn opgegeven in het servicemanifest kan worden beheerd via de SecurityGroup in het manifest van de toepassing. De verklaring van resources kan worden gewijzigd bij de implementatie, wat betekent dat de service niet hoeft in te voeren, een nieuw mechanisme voor de configuratie van deze bronnen. De schemadefinitie voor het bestand ServiceManifest.xml wordt geïnstalleerd met de Service Fabric SDK en de hulpprogramma's naar *C:\Program Files\Microsoft SDKs\Service Fabric\schemas\ServiceFabricServiceModel.xsd*.

## <a name="endpoints"></a>Eindpunten

Wanneer een bron voor het eindpunt is gedefinieerd in het servicemanifest, wijst Service Fabric de poorten van het poortbereik gereserveerde toepassing als een poort expliciet is opgegeven. Bekijk bijvoorbeeld het eindpunt *ServiceEndpoint1* opgegeven in het manifest fragment die na deze alinea. Services kunnen daarnaast ook een specifieke poort in een bron aanvragen. Service replica's uitgevoerd op andere clusterknooppunten kunnen verschillende poortnummers toegewezen terwijl de replica's van een service die wordt uitgevoerd op hetzelfde knooppunt poort delen. De replica's van service kunnen deze poorten vervolgens naar wens gebruiken voor replicatie en luisteren naar clientaanvragen.

```xml
<Resources>
  <Endpoints>
    <Endpoint Name="ServiceEndpoint1" Protocol="http"/>
    <Endpoint Name="ServiceEndpoint2" Protocol="http" Port="80"/>
    <Endpoint Name="ServiceEndpoint3" Protocol="https"/>
  </Endpoints>
</Resources>
```

Zie [configureren stateful betrouwbare Services](service-fabric-reliable-services-configuration.md) om te lezen over de eindpunten van de instellingen van het config pakket verwijzen (settings.xml) bestand.

## <a name="example-specifying-an-http-endpoint-for-your-service"></a>Voorbeeld: een HTTP-eindpunt voor uw service opgeven

De volgende servicemanifest definieert een TCP-eindpunt resource en twee bronnen van HTTP-eindpunt in de &lt;Resources&gt; element.

HTTP-eindpunten worden automatisch dat ACL zou door Fabric-Service.

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceManifest Name="Stateful1Pkg"
                 Version="1.0.0"
                 xmlns="http://schemas.microsoft.com/2011/01/fabric"
                 xmlns:xsd="http://www.w3.org/2001/XMLSchema"
                 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <ServiceTypes>
    <!-- This is the name of your ServiceType.
         This name must match the string used in the RegisterServiceType call in Program.cs. -->
    <StatefulServiceType ServiceTypeName="Stateful1Type" HasPersistedState="true" />
  </ServiceTypes>

  <!-- Code package is your service executable. -->
  <CodePackage Name="Code" Version="1.0.0">
    <EntryPoint>
      <ExeHost>
        <Program>Stateful1.exe</Program>
      </ExeHost>
    </EntryPoint>
  </CodePackage>

  <!-- Config package is the contents of the Config directoy under PackageRoot that contains an
       independently updateable and versioned set of custom configuration settings for your service. -->
  <ConfigPackage Name="Config" Version="1.0.0" />

  <Resources>
    <Endpoints>
      <!-- This endpoint is used by the communication listener to obtain the port number on which to
           listen. Note that if your service is partitioned, this port is shared with
           replicas of different partitions that are placed in your code. -->
      <Endpoint Name="ServiceEndpoint1" Protocol="http"/>
      <Endpoint Name="ServiceEndpoint2" Protocol="http" Port="80"/>
      <Endpoint Name="ServiceEndpoint3" Protocol="https"/>

      <!-- This endpoint is used by the replicator for replicating the state of your service.
           This endpoint is configured through the ReplicatorSettings config section in the Settings.xml
           file under the ConfigPackage. -->
      <Endpoint Name="ReplicatorEndpoint" />
    </Endpoints>
  </Resources>
</ServiceManifest>
```

## <a name="example-specifying-an-https-endpoint-for-your-service"></a>Voorbeeld: een HTTPS-eindpunt voor uw service opgeven

Het HTTPS-protocol biedt serververificatie en wordt ook gebruikt voor het versleutelen van client / server-communicatie. HTTPS op uw Fabric-Service-service is ingeschakeld, geeft u het protocol in de *middelen -> eindpunten eindpunt ->* gedeelte van het servicemanifest, zoals u eerder voor het eindpunt *ServiceEndpoint3*.

>[AZURE.NOTE] Protocol van de service kan niet worden gewijzigd tijdens de upgrade van de toepassing, zonder dat die deel uitmaken van een recente wijzigen.


Hier volgt een voorbeeld van ApplicationManifest die u nodig hebt om in te stellen voor HTTPS. De vingerafdruk van uw certificaat moet worden verstrekt. De EndpointRef is een verwijzing naar de EndpointResource in ServiceManifest, waarvoor u het HTTPS-protocol hebt ingesteld. U kunt meer dan één EndpointCertificate toevoegen.  

```
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest ApplicationTypeName="Application1Type"
                     ApplicationTypeVersion="1.0.0"
                     xmlns="http://schemas.microsoft.com/2011/01/fabric"
                     xmlns:xsd="http://www.w3.org/2001/XMLSchema"
                     xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <Parameters>
    <Parameter Name="Stateful1_MinReplicaSetSize" DefaultValue="2" />
    <Parameter Name="Stateful1_PartitionCount" DefaultValue="1" />
    <Parameter Name="Stateful1_TargetReplicaSetSize" DefaultValue="3" />
  </Parameters>
  <!-- Import the ServiceManifest from the ServicePackage. The ServiceManifestName and ServiceManifestVersion
       should match the Name and Version attributes of the ServiceManifest element defined in the
       ServiceManifest.xml file. -->
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="Stateful1Pkg" ServiceManifestVersion="1.0.0" />
    <ConfigOverrides />
    <Policies>
      <EndpointBindingPolicy CertificateRef="TestCert1" EndpointRef="ServiceEndpoint3"/>
    </Policies>
  </ServiceManifestImport>
  <DefaultServices>
    <!-- The section below creates instances of service types when an instance of this
         application type is created. You can also create one or more instances of service type by using the
         Service Fabric PowerShell module.

         The attribute ServiceTypeName below must match the name defined in the imported ServiceManifest.xml file. -->
    <Service Name="Stateful1">
      <StatefulService ServiceTypeName="Stateful1Type" TargetReplicaSetSize="[Stateful1_TargetReplicaSetSize]" MinReplicaSetSize="[Stateful1_MinReplicaSetSize]">
        <UniformInt64Partition PartitionCount="[Stateful1_PartitionCount]" LowKey="-9223372036854775808" HighKey="9223372036854775807" />
      </StatefulService>
    </Service>
  </DefaultServices>
  <Certificates>
    <EndpointCertificate Name="TestCert1" X509FindValue="FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF F0" X509StoreName="MY" />  
  </Certificates>
</ApplicationManifest>
```
