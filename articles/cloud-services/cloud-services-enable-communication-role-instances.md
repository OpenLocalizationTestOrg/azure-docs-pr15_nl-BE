<properties 
pageTitle="Communicatie met rollen in Cloud Services | Microsoft Azure" 
description="Exemplaren van de rol in de Cloud-Services hebben eindpunten (http, https, tcp, udp) gedefinieerd die met de buitenwereld of tussen andere exemplaren van de rol communiceren." 
services="cloud-services" 
documentationCenter="" 
authors="Thraka" 
manager="timlt" 
editor=""/>
<tags 
ms.service="cloud-services" 
ms.workload="tbd" 
ms.tgt_pltfrm="na" 
ms.devlang="na" 
ms.topic="article" 
ms.date="09/06/2016" 
ms.author="adegeo"/>

# <a name="enable-communication-for-role-instances-in-azure"></a>Communicatie inschakelen voor rol exemplaren in azure

Cloud service rollen communiceren door middel van interne en externe verbindingen. Externe verbindingen zijn **invoer eindpunten** genoemd, terwijl de interne verbindingen **interne eindpunten**worden genoemd. In dit onderwerp wordt beschreven hoe de [definitie](cloud-services-model-and-package.md#csdef) om eindpunten wijzigen.


## <a name="input-endpoint"></a>Invoer eindpunt
Het eindpunt van de invoer wordt gebruikt wanneer u een poort aan de buitenkant. Geef het protocoltype en de poort van het eindpunt dat is van toepassing voor de interne en externe poorten voor het eindpunt. Als u wilt, kunt u een andere interne poort voor het eindpunt met het kenmerk [LokalePoort](https://msdn.microsoft.com/library/azure/gg557552.aspx#InputEndpoint) .

Het eindpunt voor invoer kunt gebruiken de volgende protocollen: **http, https, tcp, udp**.

Toevoegen wilt maken op een eindpunt van de invoer, het onderliggende element van **InputEndpoint** aan het element van de **eindpunten** van een web- of werknemer.

```xml
<Endpoints>
  <InputEndpoint name="StandardWeb" protocol="http" port="80" localPort="80" />
</Endpoints> 
```

## <a name="instance-input-endpoint"></a>Invoer eindpunt exemplaar
Exemplaar invoer eindpunten zijn vergelijkbaar met de invoer van eindpunten, maar kunt u specifieke openbare poorten voor elk exemplaar van de afzonderlijke rol toewijzen met port forwarding op de verdeling van de belasting. U kunt één openbare poort, of een bereik van poorten opgeven.

Het exemplaar invoer eindpunt kan alleen **tcp** of **UDP-** protocol gebruiken.

Een invoer eindpunt exemplaar maken, toevoegen het onderliggende element van **InstanceInputEndpoint** aan het element van de **eindpunten** van een web- of werknemer.

```xml
<Endpoints>
  <InstanceInputEndpoint name="Endpoint2" protocol="tcp" localPort="10100">
    <AllocatePublicPortFrom>
      <FixedPortRange max="10109" min="10105" />
    </AllocatePublicPortFrom>
  </InstanceInputEndpoint>
</Endpoints>
```

## <a name="internal-endpoint"></a>Interne eindpunt
Interne eindpunten zijn beschikbaar voor de communicatie van exemplaar tot exemplaar. De poort is optioneel en als dit argument wordt weggelaten, een dynamische poort is toegewezen aan het eindpunt. Een bereik van poorten kan worden gebruikt. Er is een limiet van vijf interne eindpunten per rol.

De interne eindpunt kan de volgende protocollen gebruiken: **http, tcp, udp, een**.

Toevoegen om een interne input eindpunt maken, het onderliggende element van **InternalEndpoint** aan het element van de **eindpunten** van een web- of werknemer.

```xml
<Endpoints>
  <InternalEndpoint name="Endpoint3" protocol="any" port="8999" />
</Endpoints> 
```

U kunt ook een bereik van poorten.

```xml
<Endpoints>
  <InternalEndpoint name="Endpoint3" protocol="any">
    <FixedPortRange max="8995" min="8999" />
  </InternalEndpoint>
</Endpoints>
```


## <a name="worker-roles-vs-web-roles"></a>Werknemer rollen vs. Web-rollen

Er is een klein verschil met eindpunten als u werkt met een web-rollen en werknemer. De Webrol moet ten minste een enkele invoer eindpunt met behulp van het **http-** protocol.


```xml
<Endpoints>
  <InputEndpoint name="StandardWeb" protocol="http" port="80" localPort="80" />
  <!-- more endpoints may be declared after the first InputEndPoint -->
</Endpoints>
```

## <a name="using-the-net-sdk-to-access-an-endpoint"></a>De SDK voor .NET gebruiken voor toegang tot een eindpunt
De bibliotheek Azure beheerd biedt methoden voor rol exemplaren te communiceren tijdens runtime. Vanuit de programmacode uitvoeren binnen een exemplaar van de rol, kunt u gegevens over het bestaan van andere exemplaren van de rol en de eindpunten, en informatie over de huidige rol exemplaar ophalen.

> [AZURE.NOTE] U kunt alleen informatie ophalen over rol exemplaren die in de cloud-service worden uitgevoerd en die minstens één interne eindpunt definiëren. Gegevens over de rol van exemplaren die worden uitgevoerd in een andere service, kan niet worden verkregen.

U kunt de eigenschap [exemplaren](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.role.instances.aspx) exemplaren van een rol halen. Gebruik eerst de [CurrentRoleInstance](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.currentroleinstance.aspx) als resultaat een verwijzing naar het huidige exemplaar van de rol en gebruik vervolgens de eigenschap [Role](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleinstance.role.aspx) als resultaat een verwijzing naar de rol zelf.

Wanneer u verbinding met een exemplaar van de rol via programmacode door de .NET SDK maakt, is het betrekkelijk eenvoudig toegang tot de informatie van het eindpunt. Nadat u al met een specifieke rol-omgeving verbonden bent, kunt u bijvoorbeeld de haven van een bepaald eindpunt met deze code opvragen:

```csharp
int port = RoleEnvironment.CurrentRoleInstance.InstanceEndpoints["StandardWeb"].IPEndpoint.Port;
```

De **instanties** -eigenschap retourneert een collectie van objecten **RoleInstance** . Deze collectie bevat altijd de huidige sessie. Als de rol van een interne eindpunt definieert, wordt de collectie bevat de huidige sessie, maar geen andere exemplaren. Het aantal exemplaren in de collectie rol is 1 in het geval waarin er geen interne eindpunt is gedefinieerd voor de rol. Als een interne eindpunt de rol zijn gedefinieerd, de exemplaren kunnen worden gevonden tijdens runtime en het aantal exemplaren in de collectie komt overeen met het aantal keren dat is opgegeven voor de functie in het configuratiebestand van de service.

> [AZURE.NOTE] De bibliotheek Azure beheerd voorziet niet in een methode voor het bepalen van de gezondheid van de andere exemplaren van de rol, maar u kunt implementeren deze beoordeling van de gezondheid van uzelf als uw service deze functionaliteit nodig heeft. [Azure diagnostische gegevens](cloud-services-dotnet-diagnostics.md) kunt u informatie opvragen over een actieve rol exemplaren.

Om te bepalen het poortnummer voor een interne eindpunt op een exemplaar van de rol, kunt u de eigenschap [InstanceEndpoints](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleinstance.instanceendpoints.aspx) retourneert een Dictionary-object met het eindpunt namen en de bijbehorende IP-adressen en poorten. De eigenschap [IPEndpoint](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleinstanceendpoint.ipendpoint.aspx) retourneert het IP-adres en de poort voor een opgegeven eindpunt. De eigenschap **PublicIPEndpoint** retourneert de poort voor het eindpunt van een taakverdeling. Het IP-adres deel van de eigenschap **PublicIPEndpoint** wordt niet gebruikt.

Hier is een voorbeeld van exemplaren van de rol van een iteratie uitgevoerd.

```csharp
foreach (RoleInstance roleInst in RoleEnvironment.CurrentRoleInstance.Role.Instances)
{
    Trace.WriteLine("Instance ID: " + roleInst.Id);
    foreach (RoleInstanceEndpoint roleInstEndpoint in roleInst.InstanceEndpoints.Values)
    {
        Trace.WriteLine("Instance endpoint IP address and port: " + roleInstEndpoint.IPEndpoint);
    }
}
```

Hier volgt een voorbeeld van de rol van een werknemer die het eindpunt blootgesteld wordt via de servicedefinitie van de en start met luisteren naar verbindingen.

> [AZURE.WARNING] Deze code werkt alleen voor een gedistribueerde service. Wanneer wordt uitgevoerd in de Emulator Azure berekenen, worden service-configuratie-elementen die direct poort eindpunten (**InstanceInputEndpoint** elementen) genegeerd.

```csharp
using System;
using System.Diagnostics;
using System.Linq;
using System.Net;
using System.Net.Sockets;
using System.Threading;
using Microsoft.WindowsAzure;
using Microsoft.WindowsAzure.Diagnostics;
using Microsoft.WindowsAzure.ServiceRuntime;
using Microsoft.WindowsAzure.StorageClient;

namespace WorkerRole1
{
  public class WorkerRole : RoleEntryPoint
  {
    public override void Run()
    {
      try
      {
        // Initialize method-wide variables
        var epName = "Endpoint1";
        var roleInstance = RoleEnvironment.CurrentRoleInstance;
        
        // Identify direct communication port
        var myPublicEp = roleInstance.InstanceEndpoints[epName].PublicIPEndpoint;
        Trace.TraceInformation("IP:{0}, Port:{1}", myPublicEp.Address, myPublicEp.Port);

        // Identify public endpoint
        var myInternalEp = roleInstance.InstanceEndpoints[epName].IPEndpoint;
                
        // Create socket listener
        var listener = new Socket(
          myInternalEp.AddressFamily, SocketType.Stream, ProtocolType.Tcp);
                
        // Bind socket listener to internal endpoint and listen
        listener.Bind(myInternalEp);
        listener.Listen(10);
        Trace.TraceInformation("Listening on IP:{0},Port: {1}",
          myInternalEp.Address, myInternalEp.Port);

        while (true)
        {
          // Block the thread and wait for a client request
          Socket handler = listener.Accept();
          Trace.TraceInformation("Client request received.");

          // Define body of socket handler
          var handlerThread = new Thread(
            new ParameterizedThreadStart(h =>
            {
              var socket = h as Socket;
              Trace.TraceInformation("Local:{0} Remote{1}",
                socket.LocalEndPoint, socket.RemoteEndPoint);

              // Shut down and close socket
              socket.Shutdown(SocketShutdown.Both);
              socket.Close();
            }
          ));

          // Start socket handler on new thread
          handlerThread.Start(handler);
        }
      }
      catch (Exception e)
      {
        Trace.TraceError("Caught exception in run. Details: {0}", e);
      }
    }

    public override bool OnStart()
    {
      // Set the maximum number of concurrent connections 
      ServicePointManager.DefaultConnectionLimit = 12;

      // For information on handling configuration changes
      // see the MSDN topic at http://go.microsoft.com/fwlink/?LinkId=166357.
      return base.OnStart();
    }
  }
}
```

## <a name="network-traffic-rules-to-control-role-communication"></a>Netwerk netwerkverkeer voorschriften voor de bestrijding van de rol van communicatie
Nadat u een interne endpoints definieert, kunt u netwerkverkeer regels (gebaseerd op de eindpunten die u hebt gemaakt) toevoegen om te bepalen hoe rol exemplaren met elkaar kunnen communiceren. In het volgende diagram ziet u enkele algemene scenario's voor het beheren van de rol van communicatie:

![Netwerkverkeer regels voor scenario 's] (./media/cloud-services-enable-communication-role-instances/scenarios.png "Netwerkverkeer regels voor scenario 's")

In het volgende voorbeeld ziet u roldefinities voor de rollen die in het vorige diagram weergegeven. De roldefinitie van elke omvat ten minste één interne eindpunt gedefinieerd:

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
  <WebRole name="WebRole1" vmsize="Medium">
    <Sites>
      <Site name="Web">
        <Bindings>
          <Binding name="HttpIn" endpointName="HttpIn" />
        </Bindings>
      </Site>
    </Sites>
    <Endpoints>
      <InputEndpoint name="HttpIn" protocol="http" port="80" />
      <InternalEndpoint name="InternalTCP1" protocol="tcp" />
    </Endpoints>
  </WebRole>
  <WorkerRole name="WorkerRole1">
    <Endpoints>
      <InternalEndpoint name="InternalTCP2" protocol="tcp" />
    </Endpoints>
  </WorkerRole>
  <WorkerRole name="WorkerRole2">
    <Endpoints>
      <InternalEndpoint name="InternalTCP3" protocol="tcp" />
      <InternalEndpoint name="InternalTCP4" protocol="tcp" />
    </Endpoints>
  </WorkerRole>
</ServiceDefinition>
```

> [AZURE.NOTE] Beperking van de communicatie tussen rollen kan met interne eindpunten van zowel vaste als automatisch toegewezen poorten optreden.

Standaard nadat een interne eindpunt is gedefinieerd, kan communicatie doorstromen van een rol tot het interne eindpunt van een rol zonder beperkingen. Om communicatie beperken, moet u een **NetworkTrafficRules** -element toevoegen aan het **ServiceDefinition** -element in het definitiebestand.

### <a name="scenario-1"></a>Scenario 1
Alleen netwerkverkeer toestaan van **WebRole1** naar **WorkerRole1**.

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo>
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP2" roleName="WorkerRole1"/>
      </Destinations>
      <AllowAllTraffic/>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WebRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
</ServiceDefinition>
```

### <a name="scenario-2"></a>Scenario 2
Staat alleen netwerkverkeer van **WebRole1** **WorkerRole1** en **WorkerRole2**.

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo>
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP2" roleName="WorkerRole1"/>
        <RoleEndpoint endpointName="InternalTCP3" roleName="WorkerRole2"/>
      </Destinations>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WebRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
</ServiceDefinition>
```

### <a name="scenario-3"></a>Scenario 3
Staat alleen netwerkverkeer van **WebRole1** **WorkerRole1**en **WorkerRole1** naar **WorkerRole2**.

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo>
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP2" roleName="WorkerRole1"/>
      </Destinations>
      <AllowAllTraffic/>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WebRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo>
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP3" roleName="WorkerRole2"/>
      </Destinations>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WorkerRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
</ServiceDefinition>
```

### <a name="scenario-4"></a>Scenario 4
Staat alleen netwerkverkeer van **WebRole1** **WorkerRole1**, **WebRole1** tot **WorkerRole2**en **WorkerRole1** naar **WorkerRole2**.

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo>
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP2" roleName="WorkerRole1"/>
      </Destinations>
      <AllowAllTraffic/>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WebRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo >
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP3" roleName="WorkerRole2"/>
      </Destinations>
      <AllowAllTraffic/>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WorkerRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo >
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP4" roleName="WorkerRole2"/>
      </Destinations>
      <AllowAllTraffic/>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WebRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
</ServiceDefinition>
```

Een XML-schema: referentie voor de elementen boven vindt u [hier](https://msdn.microsoft.com/library/azure/gg557551.aspx).

## <a name="next-steps"></a>Volgende stappen
Lees meer over de Cloud-Service- [model](cloud-services-model-and-package.md).