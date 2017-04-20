<properties
   pageTitle="Configureren van time-out voor inactiviteit van Load Balancer TCP | Microsoft Azure"
   description="Time-out voor inactiviteit van Load Balancer TCP configureren"
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor="" />
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/24/2016"
   ms.author="sewhee" />

# <a name="configure-tcp-idle-timeout-settings-for-azure-load-balancer"></a>TCP-time-out voor inactiviteit instellingen configureren voor taakverdeling Azure

In de standaardconfiguratie is Azure Load Balancer een instelling time-out voor inactiviteit van 4 minuten. Als een periode van inactiviteit langer dan de time-outwaarde is, is er geen garantie dat de TCP- of HTTP-sessie tussen de client en de cloud-service wordt bijgehouden.

Wanneer de verbinding is gesloten, de clienttoepassing wordt het volgende foutbericht weergegeven: ' de onderliggende verbinding is gesloten: een verbinding die werd verwacht leven wordt gehouden door de server is afgesloten. "

Een veelvoorkomende truc is om een permanente TCP te gebruiken. Hierdoor blijft de verbinding actief voor een langere periode. Zie voor meer informatie, [voorbeelden van .NET](https://msdn.microsoft.com/library/system.net.servicepoint.settcpkeepalive.aspx). Met keepalive ingeschakeld, pakketten tijdens periodes van inactiviteit op de verbinding worden verzonden. Deze keepalive-pakketten zorgen ervoor dat de waarde van time-out voor inactiviteit nooit bereikt is en dat de verbinding wordt gehandhaafd gedurende een lange periode.

Deze instelling werkt voor alleen binnenkomende verbindingen. Om te voorkomen dat de verbinding verloren gaat, moet u de TCP-keep-alive configureren met een tussenpoos van minder dan de instelling van de time-out voor inactiviteit of verhoog de waarde voor time-out voor inactiviteit. Ter ondersteuning van dergelijke scenario's, hebt ondersteuning voor een configureerbare inactiviteit toegevoegd. U kunt het nu voor een duur van 4 tot en met 30 minuten instellen.

TCP keepalive-geschikt voor scenario's waarbij levensduur van de batterij niet een beperking is. Het is niet aanbevolen voor mobiele toepassingen. Met een TCP-keep-alive in een mobiele toepassing kan de accu raakt door apparaat sneller.

![Time-out voor TCP](./media/load-balancer-tcp-idle-timeout/image1.png)

In de volgende secties wordt beschreven hoe u de instellingen voor time-out voor inactiviteit in virtuele machines en cloud services.

## <a name="configure-the-tcp-timeout-for-your-instance-level-public-ip-to-15-minutes"></a>De time-out voor TCP-configureren voor uw openbare IP instantieniveau tot 15 minuten

    Set-AzurePublicIP -PublicIPName webip -VM MyVM -IdleTimeoutInMinutes 15

`IdleTimeoutInMinutes`is optioneel. Als niet is ingesteld, is de standaardtime-out 4 minuten. Het bereik van acceptabele time-out is 4 tot 30 minuten.

## <a name="set-the-idle-timeout-when-creating-an-azure-endpoint-on-a-virtual-machine"></a>De time-out voor inactiviteit instellen bij het maken van een Azure eindpunt op een virtuele machine

U wijzigt de instelling time-out voor een eindpunt, gebruikt u de volgende:

    Get-AzureVM -ServiceName "mySvc" -Name "MyVM1" | Add-AzureEndpoint -Name "HttpIn" -Protocol "tcp" -PublicPort 80 -LocalPort 8080 -IdleTimeoutInMinutes 15| Update-AzureVM

Voor het ophalen van de configuratie van de time-out voor inactiviteit, gebruik de volgende opdracht:

    PS C:\> Get-AzureVM -ServiceName "MyService" -Name "MyVM" | Get-AzureEndpoint
    VERBOSE: 6:43:50 PM - Completed Operation: Get Deployment
    LBSetName : MyLoadBalancedSet
    LocalPort : 80
    Name : HTTP
    Port : 80
    Protocol : tcp
    Vip : 65.52.xxx.xxx
    ProbePath :
    ProbePort : 80
    ProbeProtocol : tcp
    ProbeIntervalInSeconds : 15
    ProbeTimeoutInSeconds : 31
    EnableDirectServerReturn : False
    Acl : {}
    InternalLoadBalancerName :
    IdleTimeoutInMinutes : 15

## <a name="set-the-tcp-timeout-on-a-load-balanced-endpoint-set"></a>De time-out voor TCP-instellen op een eindpunt verdeeld

Als onderdeel van een set taakverdeling eindpunt eindpunten, moet de time-out voor TCP-worden ingesteld op het eindpunt taakverdeling instellen. Bijvoorbeeld:

    Set-AzureLoadBalancedEndpoint -ServiceName "MyService" -LBSetName "LBSet1" -Protocol tcp -LocalPort 80 -ProbeProtocolTCP -ProbePort 8080 -IdleTimeoutInMinutes 15

## <a name="change-timeout-settings-for-cloud-services"></a>Time-outinstellingen voor cloud-services wijzigen

U kunt de SDK Azure bijwerken uw cloud-service. U eindpunt instellingen voor cloud services in het bestand .csdef. Bijwerken van de TCP-out voor de implementatie van een cloud-service is een upgrade implementatie vereist. Een uitzondering is als de time-out voor TCP-alleen voor een openbaar IP-adres is opgegeven. Openbare IP-instellingen worden in het bestand .cscfg en kunt u ze bijwerken via implementatie update en upgrade.

De wijzigingen van de .csdef voor eindpunt instellingen zijn:

    <WorkerRole name="worker-role-name" vmsize="worker-role-size" enableNativeCodeExecution="[true|false]">
      <Endpoints>
        <InputEndpoint name="input-endpoint-name" protocol="[http|https|tcp|udp]" localPort="local-port-number" port="port-number" certificate="certificate-name" loadBalancerProbe="load-balancer-probe-name" idleTimeoutInMinutes="tcp-timeout" />
      </Endpoints>
    </WorkerRole>

De wijzigingen van de .cscfg voor de time-outinstelling op openbare IP-adressen zijn:

    <NetworkConfiguration>
      <VirtualNetworkSite name="VNet"/>
      <AddressAssignments>
        <InstanceAddress roleName="VMRolePersisted">
        <PublicIPs>
          <PublicIP name="public-ip-name" idleTimeoutInMinutes="timeout-in-minutes"/>
        </PublicIPs>
        </InstanceAddress>
      </AddressAssignments>
    </NetworkConfiguration>

## <a name="rest-api-example"></a>Voorbeeld van de REST-API

Met behulp van de API voor het servicebeheer kunt u de time-out voor inactiviteit TCP. Zorg ervoor dat de `x-ms-version` header is ingesteld op versie `2014-06-01` of hoger. Bijwerken van de configuratie van de opgegeven taakverdeling invoer eindpunten op alle virtuele machines in een implementatie.

### <a name="request"></a>Aanvraag

    POST https://management.core.windows.net/<subscription-id>/services/hostedservices/<cloudservice-name>/deployments/<deployment-name>

### <a name="response"></a>Reactie

    <LoadBalancedEndpointList xmlns="http://schemas.microsoft.com/windowsazure" xmlns:i="http://www.w3.org/2001/XMLSchema-instance">
      <InputEndpoint>
        <LoadBalancedEndpointSetName>endpoint-set-name</LoadBalancedEndpointSetName>
        <LocalPort>local-port-number</LocalPort>
        <Port>external-port-number</Port>
        <LoadBalancerProbe>
          <Path>path-of-probe</Path>
          <Port>port-assigned-to-probe</Port>
          <Protocol>probe-protocol</Protocol>
          <IntervalInSeconds>interval-of-probe</IntervalInSeconds>
          <TimeoutInSeconds>timeout-for-probe</TimeoutInSeconds>
        </LoadBalancerProbe>
        <LoadBalancerName>name-of-internal-loadbalancer</LoadBalancerName>
        <Protocol>endpoint-protocol</Protocol>
        <IdleTimeoutInMinutes>15</IdleTimeoutInMinutes>
        <EnableDirectServerReturn>enable-direct-server-return</EnableDirectServerReturn>
        <EndpointACL>
          <Rules>
            <Rule>
              <Order>priority-of-the-rule</Order>
              <Action>permit-rule</Action>
              <RemoteSubnet>subnet-of-the-rule</RemoteSubnet>
              <Description>description-of-the-rule</Description>
            </Rule>
          </Rules>
        </EndpointACL>
      </InputEndpoint>
    </LoadBalancedEndpointList>

## <a name="next-steps"></a>Volgende stappen

[Interne load balancer-overzicht](load-balancer-internal-overview.md)

[Aan de slag een taakverdeling internetverbinding configureren](load-balancer-get-started-internet-arm-ps.md)

[Een load balancer distributie-modus configureren](load-balancer-distribution-mode.md)
