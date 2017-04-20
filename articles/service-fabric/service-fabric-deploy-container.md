<properties
   pageTitle="Stof en distributie Containers | Microsoft Azure"
   description="Fabric-service en het gebruik van containers voor de implementatie van toepassingen voor microservice. Dit artikel beschrijft de mogelijkheden die Fabric-Service voor containers biedt en het implementeren van een Windows-installatiekopie container in een cluster"
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
   ms.date="10/24/2016"
   ms.author="msfussell"/>

# <a name="preview-deploy-a-windows-container-to-service-fabric"></a>Voorbeeld: Een container voor Windows aan de Fabric-Service implementeren

> [AZURE.SELECTOR]
- [Implementatie van Windows-Container](service-fabric-deploy-container.md)
- [Docker Container implementeren](service-fabric-deploy-container-linux.md)

Dit artikel helpt u bij het bouwen van beperkte services in de Windows-containers. 

>[AZURE.NOTE] Deze functie is in de voorvertoning voor Linux en is momenteel niet beschikbaar voor gebruik met Windows Server 2016. Dit is beschikbaar in de preview voor Windows Server 2016 in de volgende release van Service-structuur en wordt ondersteund in de volgende release daarna.

Fabric-service heeft diverse mogelijkheden voor container die u helpen bij het bouwen van toepassingen die zijn samengesteld van microservices die beperkte zijn. Deze worden beperkte services genoemd. 

De mogelijkheden zijn onder andere;

- Container image te implementeren en activeren
- Resource governance
- Verificatie van de opslagplaats
- Container-poorttoewijzing host-poort
- Container-container-detectie en communicatie
- Mogelijkheid tot configureren en omgevingsvariabelen instellen

U kunt zoeken op elk van de mogelijkheden op hun beurt wanneer de verpakking een beperkte service in uw toepassing moet worden opgenomen.

## <a name="packaging-a-windows-container"></a>Verpakking van een container voor Windows

Wanneer de verpakking een container, kunt u ofwel een Visual Studio-project-sjabloon of [het toepassingspakket handmatig maken](#manually). Met behulp van Visual Studio, worden de toepassing pakketstructuur en de manifest-bestanden gemaakt door de wizard Nieuw project voor u (dit binnenkort in de volgende release).

## <a name="using-visual-studio-to-package-an-existing-container-image"></a>Met behulp van Visual Studio om een bestaande afbeelding van de container

>[AZURE.NOTE] In een toekomstige versie van het Visual Studio SDK gereedschap kun je naar een container op een soortgelijke manier als u uitvoerbare Gast vandaag kunt toevoegen aan een toepassing toevoegt. Zie [Deploy Gast aan Service Fabric uitvoerbare](service-fabric-deploy-existing-app.md) onderwerp. U hebt momenteel doen handmatige verpakking zoals hieronder beschreven.

<a id="manually"></a>
## <a name="manually-packaging-and-deploying-a-container"></a>Handmatig verpakken en distribueren van een container
Het proces van het handmatig verpakking een beperkte service is gebaseerd op de volgende stappen uit:

1. De containers naar de opslagplaats worden gepubliceerd.
2. Maak de mapstructuur van het pakket.
3. Het manifestbestand van service bewerken.
4. Het manifestbestand toepassing bewerken.

## <a name="container-image-deployment-and-activation"></a>Container image te implementeren en activeren.
In de Service Fabric [toepassingsmodel](service-fabric-application-model.md)vertegenwoordigt een container een toepassinghost meerdere service replica's worden geplaatst. Om te implementeren en activeren van een container, plaatst u de naam van de afbeelding van de container in een `ContainerHost` -element in het servicemanifest.

In het servicemanifest toevoegen een `ContainerHost` voor de post aan te wijzen en stel de `ImageName` moet de naam van de opslagplaats van de container en de afbeelding. Het volgende manifest gedeeltelijke ziet u een voorbeeld van de implementatie van de zogenaamd *myimage:v1* uit een bibliotheek genoemd *myrepo*

    <CodePackage Name="Code" Version="1.0">
        <EntryPoint>
          <ContainerHost>
            <ImageName>myrepo/myimagename:v1</ImageName>
            <Commands></Commands>
          </ContainerHost>
        </EntryPoint>
    </CodePackage>

Kunt u uw opdrachten invoer naar de container afbeelding opgeven van de optionele `Commands` -element met een door komma's gescheiden reeks opdrachten uit te voeren binnen de container. 

## <a name="resource-governance"></a>Resource governance
Resource governance is een mogelijkheid van de container en zorgt ervoor dat de middelen die de container op de host gebruiken kunt. De `ResourceGovernancePolicy`, opgegeven in het manifest van de toepassing, kunt u de resource limieten voor een pakket service code declareren. Resource kunt limieten voor;

- Geheugen
- MemorySwap
- CpuShares (relatieve gewicht CPU)
- MemoryReservationInMB  
- BlkioWeight (BlockIO relatieve gewicht). 

>[AZURE.NOTE] In een toekomstige versie ondersteuning voor een specifiek blok i/o-limieten zoals IOP's geven, lezen/schrijven BPS en anderen kunnen worden.


    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
        <Policies>
            <ResourceGovernancePolicy CodePackageRef="FrontendService.Code" CpuShares="500" 
            MemoryInMB="1024" MemorySwapInMB="4084" MemoryReservationInMB="1024" />
        </Policies>
    </ServiceManifestImport>


## <a name="repository-authentication"></a>Verificatie van de opslagplaats
Voor het downloaden van een container die u mogelijk moet u aanmeldingsreferenties voor de opslagplaats van de container. De inloggegevens opgegeven in het manifest van de *toepassing* worden gebruikt om de aanmeldingsgegevens of SSH-sleutel voor het downloaden van de afbeelding van de container uit de opslagplaats van de afbeelding opgeven.  In het volgende voorbeeld ziet u een account *TestUser* en het bijbehorende wachtwoord in leesbare tekst genoemd. Dit wordt **niet** aanbevolen.


    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
        <Policies>
            <ContainerHostPolicies CodePackageRef="FrontendService.Code">
                <RepositoryCredentials AccountName="TestUser" Password="12345" PasswordEncrypted="false"/>
            </ContainerHostPolicies>
        </Policies>
    </ServiceManifestImport>

Het wachtwoord kan en moet worden gecodeerd met een certificaat dat is geïmplementeerd op de computer.

In het volgende voorbeeld ziet u een account *TestUser* aangeroepen met het wachtwoord gecodeerd met een certificaat *MyCert*genoemd. U kunt de `Invoke-ServiceFabricEncryptText` Powershell-opdracht voor het maken van de geheime gecodeerde tekst voor het wachtwoord. Raadpleeg dit artikel [geheimen in Service Fabric-toepassingen beheren](service-fabric-application-secret-management.md) voor meer informatie over. De persoonlijke sleutel van het certificaat voor het decoderen van het wachtwoord moet worden geïmplementeerd op de lokale computer in een out-of-band-methode (in Azure dit via de ARM is). Vervolgens, wanneer Fabric-Service het servicepakket naar de computer implementeert, is het geheim te decoderen en verifiëren met de container-bibliotheek met behulp van deze referenties samen met de rekeningnaam.


    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
        <Policies>
            <ContainerHostPolicies CodePackageRef="FrontendService.Code">
                <RepositoryCredentials AccountName="TestUser" Password="[Put encrypted password here using MyCert certificate ]" PasswordEncrypted="true"/>
            </ContainerHostPolicies>
        </Policies>
    </ServiceManifestImport>

## <a name="container-port-to-host-port-mapping"></a>Container-poorttoewijzing host-poort
Een host-poort die wordt gebruikt om te communiceren met de container door op te geven kunt u een `PortBinding` in het manifest van de toepassing. De binding met poort wijst de poort waarop de service op binnen de container op een poort op de host luistert.


    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
        <Policies>
            <ContainerHostPolicies CodePackageRef="FrontendService.Code">
                <PortBinding ContainerPort="8905"/>
            </ContainerHostPolicies>
        </Policies>
    </ServiceManifestImport>


## <a name="container-to-container-discovery-and-communication"></a>Container-container-detectie en communicatie
Met behulp van de `PortBinding` beleid kunt u een container-poort toewijzen een `Endpoint` in het servicemanifest zoals in het volgende voorbeeld wordt getoond. Het eindpunt `Endpoint1` kan een vaste poort opgeven, bijvoorbeeld poort 80 of geef geen poort, in welk geval een willekeurige poort van de clusters toepassing poortbereik voor u wordt gekozen.

Gast-containers, geven een `Endpoint` zoals in het servicemanifest is hierdoor Fabric-Service automatisch dit eindpunt naar de Naming service publiceren zodat andere services die in het cluster kunnen detecteren deze container met de REST oplossen-naamservicequery's. 

    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
        <Policies>
            <ContainerHostPolicies CodePackageRef="FrontendService.Code">
                <PortBinding ContainerPort="8905" EndpointRef="Endpoint1"/>
            </ContainerHostPolicies>
        </Policies>
    </ServiceManifestImport>

Met de Naming service registreert, kunt u gemakkelijk container met container communicatie in de code in de container met behulp van de [reverse proxy](service-fabric-reverseproxy.md)doen. U hoeft te doen is reverse proxy HTTP-poort luisteren en de naam van de services die u communiceren wilt met de door deze als omgevingsvariabelen. Zie de volgende sectie over hoe u dit moet doen.  

## <a name="configure-and-set-environment-variables"></a>Configureren en omgevingsvariabelen instellen
Omgevingsvariabelen zijn opgegeven tegenstander manifest voor beide services geïmplementeerd in containers of als processen/Gast uitvoerbare bestanden van elk pakket code in de service. Deze waarden kunnen worden overschreven in het bijzonder in het manifest van de toepassing of tijdens de implementatie van parameters voor de toepassing opgegeven.

Het volgende servicemanifest-XML-fragment toont een voorbeeld van het instellen van omgevingsvariabelen voor een pakket code. 

    <ServiceManifest Name="FrontendServicePackage" Version="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <Description>a guest executable service in a container</Description>
        <ServiceTypes>
            <StatelessServiceType ServiceTypeName="StatelessFrontendService"  UseImplicitHost="true"/>
        </ServiceTypes>
        <CodePackage Name="FrontendService.Code" Version="1.0">
            <EntryPoint>
            <ContainerHost>
                <ImageName>myrepo/myimage:v1</ImageName>
                <Commands></Commands>
            </ContainerHost>
            </EntryPoint>
            <EnvironmentVariables>
                <EnvironmentVariable Name="HttpGatewayPort" Value=""/>
                <EnvironmentVariable Name="BackendServiceName" Value=""/>
            </EnvironmentVariables>
        </CodePackage>
    </ServiceManifest>

Deze omgevingsvariabelen kunnen worden overschreven op het manifest niveau:

    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
        <EnvironmentOverrides CodePackageRef="FrontendService.Code">
            <EnvironmentVariable Name="BackendServiceName" Value="[BackendSvc]"/>
            <EnvironmentVariable Name="HttpGatewayPort" Value="19080"/>
        </EnvironmentOverrides>
    </ServiceManifestImport>

In het bovenstaande voorbeeld, we hebben een expliciete waarde opgegeven voor de `HttpGateway` omgevingsvariabele (19000) terwijl de waarde voor `BackendServiceName` parameter is ingesteld de `[BackendSvc]` toepassing-parameter. Hiermee kunt u de waarde voor `BackendServiceName`waarde op het moment van de implementatie van de toepassing en niet over een vaste waarde in het manifest. 

## <a name="complete-examples-for-application-and-service-manifest"></a>Voorbeelden van de toepassing en servicemanifest voltooien
Hier volgt een voorbeeld toepassingsmanifest waarin de functies van de container.


    <ApplicationManifest ApplicationTypeName="SimpleContainerApp" ApplicationTypeVersion="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <Description>A simple service container application</Description>
        <Parameters>
            <Parameter Name="ServiceInstanceCount" DefaultValue="3"></Parameter>
            <Parameter Name="BackEndSvcName" DefaultValue="bkend"></Parameter>
        </Parameters>
        <ServiceManifestImport>
            <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
            <EnvironmentOverrides CodePackageRef="FrontendService.Code">
                <EnvironmentVariable Name="BackendServiceName" Value="[BackendSvcName]"/>
                <EnvironmentVariable Name="HttpGatewayPort" Value="19080"/>
            </EnvironmentOverrides>
            <Policies>
                <ResourceGovernancePolicy CodePackageRef="Code" CpuShares="500" MemoryInMB="1024" MemorySwapInMB="4084" MemoryReservationInMB="1024" />
                <ContainerHostPolicies CodePackageRef="FrontendService.Code">
                    <RepositoryCredentials AccountName="username" Password="****" PasswordEncrypted="true"/>
                    <PortBinding ContainerPort="8905" EndpointRef="Endpoint1"/>
                </ContainerHostPolicies>
            </Policies>
        </ServiceManifestImport>
    </ApplicationManifest>


Het volgende is een voorbeeld van de servicemanifest (opgegeven in de voorgaande toepassingsmanifest) waarin de functies van de container

    <ServiceManifest Name="FrontendServicePackage" Version="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <Description> A service that implements a stateless frontend in a container</Description>
        <ServiceTypes>
            <StatelessServiceType ServiceTypeName="StatelessFrontendService"  UseImplicitHost="true"/>
        </ServiceTypes>
        <CodePackage Name="FrontendService.Code" Version="1.0">
            <EntryPoint>
            <ContainerHost>
                <ImageName>myrepo/myimage:v1</ImageName>
                <Commands></Commands>
            </ContainerHost>
            </EntryPoint>
            <EnvironmentVariables>
                <EnvironmentVariable Name="HttpGatewayPort" Value=""/>
                <EnvironmentVariable Name="BackendServiceName" Value=""/>
            </EnvironmentVariables>
        </CodePackage>
        <ConfigPackage Name="FrontendService.Config" Version="1.0" />
        <DataPackage Name="FrontendService.Data" Version="1.0" />
        <Resources>
            <Eendpoints>
                <Endpoint Name="Endpoint1" Port="80"  UriScheme="http" />
            </Eendpoints>
        </Resources>
    </ServiceManifest>
