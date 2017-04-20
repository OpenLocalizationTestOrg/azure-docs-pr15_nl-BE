<properties
   pageTitle="Load Balancer distributie-modus configureren | Microsoft Azure"
   description="Het configureren van Azure load balancer distributie modus ter ondersteuning van bron-IP-affiniteit"
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor="tysonn" />
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/24/2016"
   ms.author="sewhee" />


# <a name="configure-the-distribution-mode-for-load-balancer"></a>De distributie-modus voor taakverdeling configureren

## <a name="hash-based-distribution-mode"></a>Verdeling op basis van een hash-modus

De standaard distributie-algoritme is een 5-tupel (bron-IP, bronpoort, doel-, doelpoort, IP-protocoltype) hash verkeer toewijzen aan beschikbare servers. Kleverigheid biedt alleen een transportsessie. Pakketten in dezelfde sessie wordt doorgestuurd naar de zelfde instantie van datacenter IP (DIP) achter het eindpunt van de taakverdeling. De client start een nieuwe sessie van de dezelfde bron-IP, bronpoort verandert als zorgt ervoor dat het verkeer naar een eindpunt DIP.

![hash-op basis van taakverdeling](./media/load-balancer-distribution-mode/load-balancer-distribution.png)

Figuur 1-5-tupel distributie

## <a name="source-ip-affinity-mode"></a>Bron-IP-affiniteitsmodus

We hebben een andere distributie modus bron-IP-affiniteit (ook bekend als sessie affiniteit of IP-clientaffiniteit) genoemd. Azure taakverdeling kan worden geconfigureerd voor het gebruik van een 2-tupel (bron-IP, doel-IP-) of 3-tupel (bron-IP, doel-IP-Protocol) voor het toewijzen van verkeer aan de beschikbare servers. Met behulp van de affiniteit van bron-IP-verbindingen vanaf dezelfde clientcomputer gestart gaat naar hetzelfde eindpunt DIP.

In het volgende diagram ziet u de configuratie van een 2-tupel. U ziet hoe de 2-tupel wordt uitgevoerd via de taakverdeling met virtuele machine 1 (VM1) die vervolgens een reservekopie door VM2 en VM3.

![sessie-affiniteit](./media/load-balancer-distribution-mode/load-balancer-session-affinity.png)

Figuur 2-2-tupel distributie

Bron-IP-affiniteit is een incompatibiliteit tussen de Azure Load Balancer en de Gateway voor extern bureaublad (RD) opgelost. Nu kunt u een RD-gateway-bedrijf in een enkele cloud-service maken.

Een ander gebruik voorbeeldscenario is media uploaden wanneer het uploaden van gegevens vindt plaats via UDP, maar het besturingsvlak wordt bereikt via TCP:

- Een client een TCP-sessie aan het openbare adres taakverdeling Start eerst, wordt gericht aan een specifieke DIP dit kanaal blijft om de status van de verbinding te controleren
- Een nieuwe UDP-sessie vanaf dezelfde clientcomputer wordt gestart op hetzelfde openbare taakverdeling eindpunt, hier de verwachting is dat deze verbinding ook is gericht op hetzelfde eindpunt DIP als de vorige TCP-verbinding zodat media uploaden kan worden uitgevoerd op hoge doorvoer behoud van ook een besturingskanaal via TCP.

>[AZURE.NOTE] Wanneer u wijzigingen in een taakverdeling instellen (een virtuele machine toevoegen of verwijderen), de verdeling van aanvragen van clients opnieuw berekend. U kan niet afhankelijk zijn van nieuwe verbindingen van bestaande clients op dezelfde server loopt. Bovendien met bron-IP-affiniteit distributie modus mogelijk een ongelijke verdeling van het verkeer. Clients die worden uitgevoerd achter proxy's kunnen worden gezien als een unieke client-toepassing.

## <a name="configuring-source-ip-affinity-settings-for-load-balancer"></a>De belasting voor documentconversies configureren affiniteit van bron-IP-instellingen voor

Voor virtuele machines, kunt u PowerShell time-outinstellingen wijzigen:

Een eindpunt Azure toevoegen aan een virtuele Machine en load balancer distributie modus instellen

    Get-AzureVM -ServiceName mySvc -Name MyVM1 | Add-AzureEndpoint -Name HttpIn -Protocol TCP -PublicPort 80 -LocalPort 8080 –LoadBalancerDistribution sourceIP | Update-AzureVM

LoadBalancerDistribution kan worden ingesteld op sourceIP voor 2-tupel (bron-IP, doel-IP-) taakverdeling, sourceIPProtocol voor taakverdeling 3-tupel (bron-IP, doel-IP-protocol) of geen als u het standaardgedrag van 5-tupel taakverdeling.

Een eindpunt load balancer distributie modus configuratie ophalen, gebruikt u het volgende:

    PS C:\> Get-AzureVM –ServiceName MyService –Name MyVM | Get-AzureEndpoint

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
    LoadBalancerDistribution : sourceIP

Als het element LoadBalancerDistribution niet aanwezig is wordt de Azure taakverdeling de standaard 5-tuple-algoritme.

### <a name="set-the-distribution-mode-on-a-load-balanced-endpoint-set"></a>De distributie-modus instellen op een eindpunt taakverdeling

Als onderdeel van een set taakverdeling eindpunt eindpunten, kunnen de distributie-modus moet worden ingesteld op het eindpunt taakverdeling instellen:

    Set-AzureLoadBalancedEndpoint -ServiceName MyService -LBSetName LBSet1 -Protocol TCP -LocalPort 80 -ProbeProtocolTCP -ProbePort 8080 –LoadBalancerDistribution sourceIP

### <a name="cloud-service-configuration-to-change-distribution-mode"></a>Configuratie van de Service cloud distributie-modus wijzigen

U kunt gebruikmaken van de Azure SDK voor .NET 2.5 (om te worden uitgebracht in November) om het bijwerken van uw Cloud-Service. Eindpunt instellingen voor Cloud Services worden gemaakt in de .csdef. Om te werken de load balancer distributie modus voor de implementatie van een Cloud-Services, is een implementatie-upgrade vereist.
Hier volgt een voorbeeld van de wijzigingen in .csdef eindpunt instellingen:

    <WorkerRole name="worker-role-name" vmsize="worker-role-size" enableNativeCodeExecution="[true|false]">
      <Endpoints>
        <InputEndpoint name="input-endpoint-name" protocol="[http|https|tcp|udp]" localPort="local-port-number" port="port-number" certificate="certificate-name" loadBalancerProbe="load-balancer-probe-name" loadBalancerDistribution="sourceIP" />
      </Endpoints>
    </WorkerRole>
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

## <a name="api-example"></a>Voorbeeld van de API

U kunt de gelijkmatige verdeling van de belasting met behulp van de API voor het servicebeheer. Zorg ervoor dat u toevoegt de `x-ms-version` header is ingesteld op versie `2014-09-01` of hoger.

### <a name="update-the-configuration-of-the-specified-load-balanced-set-in-a-deployment"></a>De configuratie van de opgegeven reeks verdeeld in een implementatie bijwerken

#### <a name="request-example"></a>Voorbeeld van de aanvraag

    POST https://management.core.windows.net/<subscription-id>/services/hostedservices/<cloudservice-name>/deployments/<deployment-name>?comp=UpdateLbSet    x-ms-version: 2014-09-01
    Content-Type: application/xml

    <LoadBalancedEndpointList xmlns="http://schemas.microsoft.com/windowsazure" xmlns:i="http://www.w3.org/2001/XMLSchema-instance">
      <InputEndpoint>
        <LoadBalancedEndpointSetName> endpoint-set-name </LoadBalancedEndpointSetName>
        <LocalPort> local-port-number </LocalPort>
        <Port> external-port-number </Port>
        <LoadBalancerProbe>
          <Port> port-assigned-to-probe </Port>
          <Protocol> probe-protocol </Protocol>
          <IntervalInSeconds> interval-of-probe </IntervalInSeconds>
          <TimeoutInSeconds> timeout-for-probe </TimeoutInSeconds>
        </LoadBalancerProbe>
        <Protocol> endpoint-protocol </Protocol>
        <EnableDirectServerReturn> enable-direct-server-return </EnableDirectServerReturn>
        <IdleTimeoutInMinutes>idle-time-out</IdleTimeoutInMinutes>
        <LoadBalancerDistribution>sourceIP</LoadBalancerDistribution>
      </InputEndpoint>
    </LoadBalancedEndpointList>

De waarde van LoadBalancerDistribution is sourceIP voor 2-tupel affiniteit, sourceIPProtocol voor 3-tupel affiniteit of geen (geen affiniteit. dat wil zeggen 5-tupel)

#### <a name="response"></a>Reactie

    HTTP/1.1 202 Accepted
    Cache-Control: no-cache
    Content-Length: 0
    Server: 1.0.6198.146 (rd_rdfe_stable.141015-1306) Microsoft-HTTPAPI/2.0
    x-ms-servedbyregion: ussouth2
    x-ms-request-id: 9c7bda3e67c621a6b57096323069f7af
    Date: Thu, 16 Oct 2014 22:49:21 GMT

## <a name="next-steps"></a>Volgende stappen

[Interne load balancer-overzicht](load-balancer-internal-overview.md)

[Een internetverbinding taakverdeling configureren aan de slag](load-balancer-get-started-internet-arm-ps.md)

[Niet-actieve TCP-time-outinstellingen voor de taakverdeling configureren](load-balancer-tcp-idle-timeout.md)
