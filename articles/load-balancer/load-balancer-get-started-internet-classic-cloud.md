<properties
   pageTitle="Maken van een internetverbinding taakverdeling in het klassieke deployment model gebruiken voor cloud services aan de slag | Microsoft Azure"
   description="Informatie over het maken van een internetverbinding taakverdeling in het klassieke implementatiemodel voor cloud services"
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
   ms.date="03/17/2016"
   ms.author="sewhee" />

# <a name="get-started-creating-an-internet-facing-load-balancer-for-cloud-services"></a>Taakverdeling voor cloud services van een internetverbinding maken

[AZURE.INCLUDE [load-balancer-get-started-internet-classic-selectors-include.md](../../includes/load-balancer-get-started-internet-classic-selectors-include.md)]

[AZURE.INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Dit artikel heeft betrekking op de klassieke implementatiemodel. U kunt ook [informatie over het maken van een internetverbinding met behulp van bronbeheer Azure taakverdeling](load-balancer-get-started-internet-arm-cli.md).

Cloud-services worden automatisch geconfigureerd met een load balancer en kunnen worden aangepast via de service-model.

## <a name="create-a-load-balancer-using-the-service-definition-file"></a>Maak een taakverdeling met behulp van het bestand van de definitie

U kunt gebruikmaken van de Azure SDK voor .NET 2.5 voor het bijwerken van uw cloud-service. Eindpunt instellingen voor cloud services worden aangebracht in de [definitie van](https://msdn.microsoft.com/library/azure/gg557553.aspx).csdef-bestand.

In het volgende voorbeeld ziet u hoe een bestand servicedefinition.csdef voor de implementatie van een wolk is geconfigureerd:

Controle van de snipet voor het .csdef-bestand gegenereerd door de implementatie van een wolk, ziet u het externe eindpunt dat is geconfigureerd voor het gebruik van HTTP-poorten op de poort 10000 en 10001 10002.


    <ServiceDefinition name=“Tenant“>
    <WorkerRole name=“FERole” vmsize=“Small“>
    <Endpoints>
        <InputEndpoint name=“FE_External_Http” protocol=“http” port=“10000“ />
        <InputEndpoint name=“FE_External_Tcp“  protocol=“tcp“  port=“10001“ />
        <InputEndpoint name=“FE_External_Udp“  protocol=“udp“  port=“10002“ />

        <InputEndpointname=“HTTP_Probe” protocol=“http” port=“80” loadBalancerProbe=“MyProbe“ />

        <InstanceInputEndpoint name=“InstanceEP” protocol=“tcp” localPort=“80“>
           <AllocatePublicPortFrom>
              <FixedPortRange min=“10110” max=“10120“  />
           </AllocatePublicPortFrom>
        </InstanceInputEndpoint>
        <InternalEndpoint name=“FE_InternalEP_Tcp” protocol=“tcp“ />
    </Endpoints>
    </WorkerRole>
    </ServiceDefinition>




## <a name="check-load-balancer-health-status-for-cloud-services"></a>Load balancer gezondheidsstatus voor cloud-services controleren


Hier volgt een voorbeeld van een sonde gezondheid:

        <LoadBalancerProbes>
        <LoadBalancerProbe name=“MyProbe” protocol=“http” path=“Probe.aspx” intervalInSeconds=“5” timeoutInSeconds=“100“ />
        </LoadBalancerProbes>

De taakverdeling combineert de informatie van het eindpunt en de informatie van de sonde voor het maken van een URL in de vorm van http://{DIP van VM}:80/Probe.aspx die kunnen worden gebruikt voor het opvragen van de status van de service.

De service wordt gedetecteerd periodieke sondes van hetzelfde IP-adres. Dit is de gezondheid sonde aanvraag die afkomstig zijn van de host van het knooppunt waarop de virtuele machine wordt uitgevoerd.
De service heeft om te reageren met een 200 HTTP-statuscode voor de taakverdeling te veronderstellen dat de service in orde is. Eventuele andere HTTP-statuscode (bijvoorbeeld 503) direct wordt de virtuele machine van rotatie.

De definitie van de sonde bepaalt ook de frequentie van de sonde. In ons geval is de taakverdeling scannen het eindpunt elke 5 seconden. Als er geen positief antwoord wordt ontvangen voor 10 sec (twee sonde intervallen), wordt uitgegaan van de sonde omlaag en rotatie, de virtuele machine wordt uitgeschakeld. Op dezelfde manier als de service niet gedraaid is en een positief antwoord is ontvangen, is de service terug naar plaatsen direct draaien. Als de service is schommelde tussen gezonde en beschadigde, kan de taakverdeling vertraging voor het weer binnenbrengen van de dienst terug te draaien totdat het is goed voor een aantal sondes besluiten.

Controleer of het schema van de definitie van service voor de [gezondheid sonde](https://msdn.microsoft.com/library/azure/jj151530.aspx) voor meer informatie.

## <a name="next-steps"></a>Volgende stappen

[Een interne taakverdeling configureren aan de slag](load-balancer-get-started-ilb-arm-ps.md)

[Een load balancer distributie-modus configureren](load-balancer-distribution-mode.md)

[Niet-actieve TCP-time-outinstellingen voor de taakverdeling configureren](load-balancer-tcp-idle-timeout.md)

