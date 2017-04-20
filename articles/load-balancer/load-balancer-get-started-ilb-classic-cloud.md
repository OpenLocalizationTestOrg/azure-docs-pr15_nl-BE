<properties
   pageTitle="Een interne taakverdeling voor cloud services maken in het klassieke implementatiemodel | Microsoft Azure"
   description="Informatie over het maken van een interne taakverdeling met PowerShell in het implementatiemodel klassiek"
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor=""
   tags="azure-service-management"
/>
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/09/2016"
   ms.author="sewhee" />

# <a name="get-started-creating-an-internal-load-balancer-classic-for-cloud-services"></a>Maken van een interne taakverdeling (klassiek) voor cloud services aan de slag

[AZURE.INCLUDE [load-balancer-get-started-ilb-classic-selectors-include.md](../../includes/load-balancer-get-started-ilb-classic-selectors-include.md)]

<BR>

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-classic-include.md)]Meer informatie over hoe u [deze stappen uitvoert met behulp van het model Resource Manager](load-balancer-get-started-ilb-arm-ps.md).


## <a name="configure-internal-load-balancer-for-cloud-services"></a>Interne taakverdeling voor cloud-services configureren

Interne taakverdeling wordt ondersteund voor zowel virtuele machines en cloud services. Een interne load balancer eindpunt in een cloud-service die buiten een regionale virtueel netwerk gemaakt zijn alleen binnen de service cloud toegankelijk.

De interne load balancer-configuratie moet worden ingesteld tijdens het maken van de eerste implementatie in de cloud-service, zoals in het onderstaande voorbeeld.

>[AZURE.IMPORTANT] Een vereiste voor het uitvoeren van de volgende stappen is om een virtueel netwerk al gemaakt voor de implementatie van de wolk. U moet de naam van de virtueel netwerk en subnet in interne taakverdeling maakt.

### <a name="step-1"></a>Stap 1

Het service-configuratiebestand (.cscfg) voor de implementatie van de wolk openen in Visual Studio en voeg de volgende sectie als u wilt maken met de interne lastenverdeling onder de laatste '`</Role>`'-item voor de configuratie van het netwerk.




    <NetworkConfiguration>
      <LoadBalancers>
        <LoadBalancer name="name of the load balancer">
          <FrontendIPConfiguration type="private" subnet="subnet-name" staticVirtualNetworkIPAddress="static-IP-address"/>
        </LoadBalancer>
      </LoadBalancers>
    </NetworkConfiguration>


We voegen de waarden voor het configuratiebestand netwerk weer te geven hoe deze eruit. In het voorbeeld wordt ervan uitgegaan dat u een subnet genaamd 'test_vnet' met een subnet 10.0.0.0/24 genaamd test_subnet en een statisch IP-adres 10.0.0.4 gemaakt. De taakverdeling krijgt de naam testLB.

    <NetworkConfiguration>
      <LoadBalancers>
        <LoadBalancer name="testLB">
          <FrontendIPConfiguration type="private" subnet="test_subnet" staticVirtualNetworkIPAddress="10.0.0.4"/>
        </LoadBalancer>
      </LoadBalancers>
    </NetworkConfiguration>

Zie voor meer informatie over het schema load balancer [de belasting voor documentconversies toevoegen](https://msdn.microsoft.com/library/azure/dn722411.aspx).

### <a name="step-2"></a>Stap 2


Wijzig het bestand met de formulierdefinitie (.csdef) service eindpunten toevoegen aan het interne taakverdeling. Het moment dat een exemplaar van de rol is gemaakt, wordt het definitiebestand de rol exemplaren toevoegen aan interne taakverdeling.


    <WorkerRole name="worker-role-name" vmsize="worker-role-size" enableNativeCodeExecution="[true|false]">
      <Endpoints>
        <InputEndpoint name="input-endpoint-name" protocol="[http|https|tcp|udp]" localPort="local-port-number" port="port-number" certificate="certificate-name" loadBalancerProbe="load-balancer-probe-name" loadBalancer="load-balancer-name" />
      </Endpoints>
    </WorkerRole>

Volgens dezelfde waarden uit het bovenstaande voorbeeld laten we optellen bij het definitiebestand.

    <WorkerRole name=WorkerRole1" vmsize="A7" enableNativeCodeExecution="[true|false]">
      <Endpoints>
        <InputEndpoint name="endpoint1" protocol="http" localPort="80" port="80" loadBalancer="testLB" />
      </Endpoints>
    </WorkerRole>

Het netwerkverkeer worden verdeeld met behulp van de testLB-taakverdeling met poort 80 voor binnenkomende aanvragen verzenden naar de werknemer rol exemplaren ook op poort 80.


## <a name="next-steps"></a>Volgende stappen

[Configureer een load balancer distributie modus bron-IP-affiniteit](load-balancer-distribution-mode.md)

[Niet-actieve TCP-time-outinstellingen voor de taakverdeling configureren](load-balancer-tcp-idle-timeout.md)