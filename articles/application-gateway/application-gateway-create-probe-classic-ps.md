<properties
   pageTitle="Een aangepaste sonde voor Application Gateway maken met PowerShell in het implementatiemodel klassiek | Microsoft Azure"
   description="Informatie over het maken van een aangepaste sonde voor Application Gateway met PowerShell in het implementatiemodel klassiek"
   services="application-gateway"
   documentationCenter="na"
   authors="georgewallace"
   manager="carmonm"
   editor=""
   tags="azure-service-management"
/>
<tags  
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/25/2016"
   ms.author="gwallace" />

# <a name="create-a-custom-probe-for-azure-application-gateway-classic-by-using-powershell"></a>Maak een aangepaste sonde voor Azure Application Gateway (klassiek) met PowerShell

> [AZURE.SELECTOR]
- [Azure portal](application-gateway-create-probe-portal.md)
- [Azure PowerShell voor bronbeheer](application-gateway-create-probe-ps.md)
- [Azure klassieke PowerShell](application-gateway-create-probe-classic-ps.md)

[AZURE.INCLUDE [azure-probe-intro-include](../../includes/application-gateway-create-probe-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-classic-include.md)]Meer informatie over hoe u [deze stappen uitvoert met behulp van het model Resource Manager](application-gateway-create-probe-ps.md).

[AZURE.INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="create-an-application-gateway"></a>Een toepassingsgateway maken

Voor het maken van een toepassingsgateway:

1. Maak een toepassingsbron gateway.
2. Maak een XML-bestand of een configuratieobject.
3. De configuratie van de nieuwe gateway toepassingsbron doorvoeren.

### <a name="create-an-application-gateway-resource"></a>Maken van een gateway-toepassingsbron

U kunt de gateway maken met de cmdlet **New-AzureApplicationGateway** , de waarden te vervangen door uw eigen. Facturering van de gateway op dit moment niet worden gestart. Facturering begint in een latere stap de gateway is gestart.

In het volgende voorbeeld wordt een toepassingsgateway met behulp van een virtueel netwerk "testvnet1" en een subnet genaamd "subnet 1" genoemd.

    New-AzureApplicationGateway -Name AppGwTest -VnetName testvnet1 -Subnets @("Subnet-1")

Om te valideren dat de gateway is gemaakt, kunt u de cmdlet **Get-AzureApplicationGateway** .

    Get-AzureApplicationGateway AppGwTest

>[AZURE.NOTE]  De standaardwaarde voor *InstanceCount* is 2, met een maximale waarde van 10. De standaardwaarde voor *GatewaySize* is normaal. U kunt kiezen tussen een klein, middelgroot en groot.

 *VirtualIPs* en de *DNS-naam* worden weergegeven als lege omdat de gateway is nog niet begonnen. Deze worden gemaakt zodra de gateway actief is.

## <a name="configure-an-application-gateway"></a>Een toepassingsgateway configureren

Met behulp van XML of een configuratieobject kunt u de toepassingsgateway configureren.

## <a name="configure-an-application-gateway-by-using-xml"></a>Een toepassingsgateway configureren met behulp van XML

In het volgende voorbeeld kunt u een XML-bestand alle application gateway-instellingen configureren en deze de gateway-toepassingsbron vastleggen.  

### <a name="step-1"></a>Stap 1

Kopieer de volgende tekst naar Kladblok.

    <ApplicationGatewayConfiguration xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/windowsazure">
    <FrontendIPConfigurations>
        <FrontendIPConfiguration>
            <Name>fip1</Name>
            <Type>Private</Type>
        </FrontendIPConfiguration>
    </FrontendIPConfigurations>    
    <FrontendPorts>
        <FrontendPort>
            <Name>port1</Name>
            <Port>80</Port>
        </FrontendPort>
    </FrontendPorts>
    <Probes>
        <Probe>
            <Name>Probe01</Name>
            <Protocol>Http</Protocol>
            <Host>contoso.com</Host>
            <Path>/path/custompath.htm</Path>
            <Interval>15</Interval>
            <Timeout>15</Timeout>
            <UnhealthyThreshold>5</UnhealthyThreshold>
        </Probe>
      </Probes>
     <BackendAddressPools>
        <BackendAddressPool>
            <Name>pool1</Name>
            <IPAddresses>
                <IPAddress>1.1.1.1</IPAddress>
                <IPAddress>2.2.2.2</IPAddress>
            </IPAddresses>
        </BackendAddressPool>
    </BackendAddressPools>
    <BackendHttpSettingsList>
        <BackendHttpSettings>
            <Name>setting1</Name>
            <Port>80</Port>
            <Protocol>Http</Protocol>
            <CookieBasedAffinity>Enabled</CookieBasedAffinity>
            <RequestTimeout>120</RequestTimeout>
            <Probe>Probe01</Probe>
        </BackendHttpSettings>
    </BackendHttpSettingsList>
    <HttpListeners>
        <HttpListener>
            <Name>listener1</Name>
            <FrontendIP>fip1</FrontendIP>
        <FrontendPort>port1</FrontendPort>
            <Protocol>Http</Protocol>
        </HttpListener>
    </HttpListeners>
    <HttpLoadBalancingRules>
        <HttpLoadBalancingRule>
            <Name>lbrule1</Name>
            <Type>basic</Type>
            <BackendHttpSettings>setting1</BackendHttpSettings>
            <Listener>listener1</Listener>
            <BackendAddressPool>pool1</BackendAddressPool>
        </HttpLoadBalancingRule>
    </HttpLoadBalancingRules>
    </ApplicationGatewayConfiguration>


De waarden tussen haakjes voor de configuratie-items bewerken. Sla het bestand met de extensie .xml.

In het volgende voorbeeld ziet u hoe u met een configuratiebestand voor het instellen van de toepassingsgateway verdelen van HTTP-verkeer op poort 80 van de openbare en het verzenden van netwerkverkeer naar back-end poort 80 tussen twee IP-adressen met behulp van een aangepaste sonde.

>[AZURE.IMPORTANT] Het artikel van het protocol Http of Https is hoofdlettergevoelig.

Een nieuwe configuratie-item \<Probe\> voor het configureren van aangepaste sondes wordt toegevoegd.

De configuratieparameters zijn:

- **Naam** - naam voor de aangepaste sonde.
- **Protocol** - Protocol dat wordt gebruikt (de mogelijke waarden zijn HTTP of HTTPS).
- **Host** en het **pad** - volledige URL-pad dat wordt aangeroepen door de toepassingsgateway om te bepalen van de status van het exemplaar. Bijvoorbeeld als er een http://contoso.com/ website, worden kan de aangepaste sonde geconfigureerd voor 'http://contoso.com/path/custompath.htm' voor de sonde controles hebben een geslaagde HTTP-antwoord.
- **Interval** - configureert de controles sonde interval in seconden.
- **Timeout** - definieert de sonde time-out voor een HTTP-antwoord uit.
- **UnhealthyThreshold** - het aantal mislukte HTTP-antwoorden die nodig zijn voor het markeren van de back-end-instantie als *beschadigd*.

De naam van de sonde wordt verwezen in de <BackendHttpSettings> configuratie toe te wijzen welke groep met back-end sonde aangepaste instellingen worden gebruikt.

## <a name="add-a-custom-probe-configuration-to-an-existing-application-gateway"></a>Een aangepaste sonde configuratie toevoegen aan een bestaande toepassingsgateway

Wijzigen van de huidige configuratie van een toepassingsgateway bestaat uit drie stappen: ophalen van de huidige XML-configuratiebestand en de toepassingsgateway configureren met de nieuwe XML-instellingen wijzigen als u een aangepaste sonde.

### <a name="step-1"></a>Stap 1

Het XML-bestand ophalen met behulp van get-AzureApplicationGatewayConfig. Hiermee exporteert de configuratie van XML worden gewijzigd zodat de sonde instelling toevoegen.

    Get-AzureApplicationGatewayConfig -Name "<application gateway name>" -Exporttofile "<path to file>"


### <a name="step-2"></a>Stap 2

Open het XML-bestand in een teksteditor. Voeg toe een `<probe>` sectie na `<frontendport>`.

    <Probes>
        <Probe>
            <Name>Probe01</Name>
            <Protocol>Http</Protocol>
            <Host>contoso.com</Host>
            <Path>/path/custompath.htm</Path>
            <Interval>15</Interval>
            <Timeout>15</Timeout>
            <UnhealthyThreshold>5</UnhealthyThreshold>
        </Probe>
    </Probes>

Toevoegen in de sectie backendHttpSettings van het XML-bestand, de naam van de sonde zoals in het volgende voorbeeld wordt getoond:

        <BackendHttpSettings>
            <Name>setting1</Name>
            <Port>80</Port>
            <Protocol>Http</Protocol>
            <CookieBasedAffinity>Enabled</CookieBasedAffinity>
            <RequestTimeout>120</RequestTimeout>
            <Probe>Probe01</Probe>
        </BackendHttpSettings>

Sla het XML-bestand.

### <a name="step-3"></a>Stap 3

Gateway toepassingsconfiguratie bijwerken met het nieuwe XML-bestand met behulp van **Set AzureApplicationGatewayConfig**. Hiermee wordt uw toepassingsgateway bijgewerkt met de nieuwe configuratie.

    Set-AzureApplicationGatewayConfig -Name "<application gateway name>" -Configfile "<path to file>"


## <a name="next-steps"></a>Volgende stappen

Als u wilt voor het configureren van Secure Sockets Layer (SSL)-offload, Zie [configureren een toepassingsgateway voor SSL-offload](application-gateway-ssl.md).

Als u configureren een toepassingsgateway wilt gebruiken met een interne taakverdeling, Zie [maken een toepassingsgateway met interne taakverdeling (ILB)](application-gateway-ilb.md).
