<properties
   pageTitle="Maken, starten of een toepassingsgateway verwijderen | Microsoft Azure"
   description="Deze pagina bevat instructies voor het maken, configureren, starten en een toepassingsgateway Azure verwijderen"
   documentationCenter="na"
   services="application-gateway"
   authors="georgewallace"
   manager="carmonm"
   editor="tysonn"/>
<tags
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/25/2016"
   ms.author="gwallace"/>

# <a name="create-start-or-delete-an-application-gateway"></a>Maken, starten of een toepassingsgateway verwijderen

> [AZURE.SELECTOR]
- [Azure Portal](application-gateway-create-gateway-portal.md)
- [Azure PowerShell voor bronbeheer](application-gateway-create-gateway-arm.md)
- [Azure klassieke PowerShell](application-gateway-create-gateway.md)
- [Sjabloon voor Azure Resource Manager](application-gateway-create-gateway-arm-template.md)
- [Azure CLI](application-gateway-create-gateway-cli.md)

Azure Application Gateway is een laag 7 load balancer. Biedt failover, HTTP-verzoeken prestaties routering tussen verschillende servers, of ze nu op de cloud of op locatie. Application Gateway biedt vele functies van Application Delivery Controller (ADC) met inbegrip van HTTP-taakverdeling, op basis van een cookie sessie affiniteit, Secure Sockets Layer (SSL)-offload, aangepaste gezondheid sondes, ondersteuning voor meerdere locaties en vele andere. Ga voor een volledige lijst van ondersteunde functies naar [Gateway: overzicht](application-gateway-introduction.md)

Dit artikel begeleidt u door de stappen voor het maken, configureren, starten en een toepassingsgateway verwijderen.

## <a name="before-you-begin"></a>Voordat u begint

1. Installeer de nieuwste versie van de Azure PowerShell-cmdlets met behulp van de Web Platform Installer. U kunt downloaden en installeren van de meest recente versie van de **Windows PowerShell** -sectie van de [pagina met Downloads](https://azure.microsoft.com/downloads/).
2. Als u een bestaande virtuele netwerk hebt, selecteert u een bestaande lege subnet of een nieuw subnet maken in uw bestaande virtuele netwerk uitsluitend voor gebruik door de toepassingsgateway. U kunt de toepassingsgateway naar een ander virtueel netwerk dan de bronnen die u implementeren achter de toepassingsgateway wilt tenzij vnet peering wordt gebruikt niet implementeren. Ga voor meer informatie naar [Vnet Peering](../virtual-network/virtual-network-peering-overview.md)
3. Controleer of u een werkende virtuele netwerk met een geldig subnetmasker. Controleer of geen virtuele machines of cloud implementaties het subnet worden gebruikt. De toepassingsgateway moet zijn op zichzelf in een virtueel netwerk subnet.
3. De servers die u configureert voor het gebruik van de toepassingsgateway moeten bestaan of de eindpunten ervan gemaakt in het virtuele netwerk of met een openbaar IP-/ VIP hebt toegewezen.

## <a name="what-is-required-to-create-an-application-gateway"></a>Wat is vereist voor het maken van een toepassingsgateway?

Wanneer u de opdracht **Nieuw AzureApplicationGateway** voor het maken van de toepassingsgateway, geen configuratie is nu ingesteld en de zojuist gemaakte bron zijn geconfigureerd met behulp van XML of een configuratieobject.

De waarden zijn:

- **Back-end server-groep:** De lijst met IP-adressen van de back-end servers. De IP-adressen ofwel moeten behoren tot het virtuele netwerk subnet of moeten een openbaar IP/VIP.
- **Instellingen voor back-end server toepassingen:** Elke groep heeft als poort, protocol en affiniteit op basis van een cookie-instellingen. Deze instellingen zijn gekoppeld aan een groep en worden toegepast op alle servers in de groep.
- **Front-poort:** Dit wordt de openbare poort op de toepassingsgateway wordt geopend. Verkeer treft deze poort en wordt vervolgens omgeleid naar een van de back-end servers.
- **Listener:** De listener is een front-end-poort, een protocol (Http of Https, deze waarden zijn hoofdlettergevoelig), en de SSL-certificaat (als het configureren van SSL-offload).
- **Regel:** De regel wordt de listener en de groep met back-end-server en wordt gedefinieerd welke het verkeer moet worden besteed aan een bepaalde listener wanneer deze groep met back-end-server.

## <a name="create-an-application-gateway"></a>Een toepassingsgateway maken

Voor het maken van een toepassingsgateway:

1. Maak een toepassingsbron gateway.
2. Maak een XML-bestand of een configuratieobject.
3. De configuratie van de nieuwe gateway toepassingsbron doorvoeren.

>[AZURE.NOTE] Als u een aangepaste sonde voor uw toepassingsgateway configureren, Zie [maken een toepassingsgateway met aangepaste sondes met PowerShell](application-gateway-create-probe-classic-ps.md). Bekijk de [aangepaste sondes en de statuscontrole](application-gateway-probe-overview.md) voor meer informatie.

![Voorbeeld scenario][scenario]

### <a name="create-an-application-gateway-resource"></a>Maken van een gateway-toepassingsbron

U kunt de gateway maken met de cmdlet **New-AzureApplicationGateway** , de waarden te vervangen door uw eigen. Facturering van de gateway op dit moment niet worden gestart. Facturering begint in een latere stap de gateway is gestart.

In het volgende voorbeeld wordt een toepassingsgateway met behulp van een virtueel netwerk "testvnet1" en een subnet genaamd "subnet 1" genoemd.


    New-AzureApplicationGateway -Name AppGwTest -VnetName testvnet1 -Subnets @("Subnet-1")

    VERBOSE: 4:31:35 PM - Begin Operation: New-AzureApplicationGateway
    VERBOSE: 4:32:37 PM - Completed Operation: New-AzureApplicationGateway
    Name       HTTP Status Code     Operation ID                             Error
    ----       ----------------     ------------                             ----
    Successful OK                   55ef0460-825d-2981-ad20-b9a8af41b399


 *Beschrijving*, *InstanceCount*en *GatewaySize* zijn optionele parameters.

Om te valideren dat de gateway is gemaakt, kunt u de cmdlet **Get-AzureApplicationGateway** .

    Get-AzureApplicationGateway AppGwTest
    Name          : AppGwTest
    Description   :
    VnetName      : testvnet1
    Subnets       : {Subnet-1}
    InstanceCount : 2
    GatewaySize   : Medium
    State         : Stopped
    VirtualIPs    : {}
    DnsName       :

>[AZURE.NOTE]  De standaardwaarde voor *InstanceCount* is 2, met een maximale waarde van 10. De standaardwaarde voor *GatewaySize* is normaal. U kunt kiezen tussen een klein, middelgroot en groot.

*VirtualIPs* en de *DNS-naam* worden weergegeven als lege omdat de gateway is nog niet begonnen. Deze worden gemaakt zodra de gateway actief is.

## <a name="configure-the-application-gateway"></a>De toepassingsgateway configureren

Met behulp van XML of een configuratieobject kunt u de toepassingsgateway configureren.

## <a name="configure-the-application-gateway-by-using-xml"></a>De toepassingsgateway configureren met behulp van XML

In het volgende voorbeeld kunt u een XML-bestand alle application gateway-instellingen configureren en deze de gateway-toepassingsbron vastleggen.  

### <a name="step-1"></a>Stap 1  

Kopieer de volgende tekst naar Kladblok.

    <?xml version="1.0" encoding="utf-8"?>
    <ApplicationGatewayConfiguration xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/windowsazure">
        <FrontendPorts>
            <FrontendPort>
                <Name>(name-of-your-frontend-port)</Name>
                <Port>(port number)</Port>
            </FrontendPort>
        </FrontendPorts>
        <BackendAddressPools>
            <BackendAddressPool>
                <Name>(name-of-your-backend-pool)</Name>
                <IPAddresses>
                    <IPAddress>(your-IP-address-for-backend-pool)</IPAddress>
                    <IPAddress>(your-second-IP-address-for-backend-pool)</IPAddress>
                </IPAddresses>
            </BackendAddressPool>
        </BackendAddressPools>
        <BackendHttpSettingsList>
            <BackendHttpSettings>
                <Name>(backend-setting-name-to-configure-rule)</Name>
                <Port>80</Port>
                <Protocol>[Http|Https]</Protocol>
                <CookieBasedAffinity>Enabled</CookieBasedAffinity>
            </BackendHttpSettings>
        </BackendHttpSettingsList>
        <HttpListeners>
            <HttpListener>
                <Name>(name-of-the-listener)</Name>
                <FrontendPort>(name-of-your-frontend-port)</FrontendPort>
                <Protocol>[Http|Https]</Protocol>
            </HttpListener>
        </HttpListeners>
        <HttpLoadBalancingRules>
            <HttpLoadBalancingRule>
                <Name>(name-of-load-balancing-rule)</Name>
                <Type>basic</Type>
                <BackendHttpSettings>(backend-setting-name-to-configure-rule)</BackendHttpSettings>
                <Listener>(name-of-the-listener)</Listener>
                <BackendAddressPool>(name-of-your-backend-pool)</BackendAddressPool>
            </HttpLoadBalancingRule>
        </HttpLoadBalancingRules>
    </ApplicationGatewayConfiguration>

De waarden tussen haakjes voor de configuratie-items bewerken. Sla het bestand met de extensie .xml.

>[AZURE.IMPORTANT] Het artikel van het protocol Http of Https is hoofdlettergevoelig.

In het volgende voorbeeld ziet u hoe u met een configuratiebestand voor het instellen van de toepassingsgateway. De belasting voorbeeld HTTP-verkeer op poort 80 van de openbare saldi en netwerkverkeer verzendt naar back-end poort 80 tussen twee IP-adressen.

    <?xml version="1.0" encoding="utf-8"?>
    <ApplicationGatewayConfiguration xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/windowsazure">
        <FrontendPorts>
            <FrontendPort>
                <Name>FrontendPort1</Name>
                <Port>80</Port>
            </FrontendPort>
        </FrontendPorts>
        <BackendAddressPools>
            <BackendAddressPool>
                <Name>BackendPool1</Name>
                <IPAddresses>
                    <IPAddress>10.0.0.1</IPAddress>
                    <IPAddress>10.0.0.2</IPAddress>
                </IPAddresses>
            </BackendAddressPool>
        </BackendAddressPools>
        <BackendHttpSettingsList>
            <BackendHttpSettings>
                <Name>BackendSetting1</Name>
                <Port>80</Port>
                <Protocol>Http</Protocol>
                <CookieBasedAffinity>Enabled</CookieBasedAffinity>
            </BackendHttpSettings>
        </BackendHttpSettingsList>
        <HttpListeners>
            <HttpListener>
                <Name>HTTPListener1</Name>
                <FrontendPort>FrontendPort1</FrontendPort>
                <Protocol>Http</Protocol>
            </HttpListener>
        </HttpListeners>
        <HttpLoadBalancingRules>
            <HttpLoadBalancingRule>
                <Name>HttpLBRule1</Name>
                <Type>basic</Type>
                <BackendHttpSettings>BackendSetting1</BackendHttpSettings>
                <Listener>HTTPListener1</Listener>
                <BackendAddressPool>BackendPool1</BackendAddressPool>
            </HttpLoadBalancingRule>
        </HttpLoadBalancingRules>
    </ApplicationGatewayConfiguration>


### <a name="step-2"></a>Stap 2

Stel vervolgens de toepassingsgateway. Gebruik de cmdlet **Set-AzureApplicationGatewayConfig** met een XML-configuratiebestand.


    Set-AzureApplicationGatewayConfig -Name AppGwTest -ConfigFile "D:\config.xml"

    VERBOSE: 7:54:59 PM - Begin Operation: Set-AzureApplicationGatewayConfig
    VERBOSE: 7:55:32 PM - Completed Operation: Set-AzureApplicationGatewayConfig
    Name       HTTP Status Code     Operation ID                             Error
    ----       ----------------     ------------                             ----
    Successful OK                   9b995a09-66fe-2944-8b67-9bb04fcccb9d

## <a name="configure-the-application-gateway-by-using-a-configuration-object"></a>De toepassingsgateway configureren met behulp van een configuratieobject

In het volgende voorbeeld ziet u hoe de toepassingsgateway configureren met configuratie-objecten. Alle configuratie-items moeten afzonderlijk worden geconfigureerd en vervolgens toegevoegd aan een object application gateway configureren. Nadat de configuratie-object is gemaakt, kunt u de opdracht **Set AzureApplicationGateway** doorvoeren van de configuratie van de eerder gemaakte toepassingsbron gateway.

>[AZURE.NOTE] Voordat u een waarde toewijst aan elk configuratieobject, moet u aan te geven welk type object PowerShell wordt gebruikt voor de opslag. De eerste regel voor het maken van de afzonderlijke artikelen bepaalt welke Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model (objectnaam) worden gebruikt.

### <a name="step-1"></a>Stap 1

Alle afzonderlijke configuratie-items maken.

De front-IP maken zoals in het volgende voorbeeld.

    $fip = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.FrontendIPConfiguration
    $fip.Name = "fip1"
    $fip.Type = "Private"
    $fip.StaticIPAddress = "10.0.0.5"

De front-poort maken zoals in het volgende voorbeeld.

    $fep = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.FrontendPort
    $fep.Name = "fep1"
    $fep.Port = 80

De groep met back-end-server maken.

 Definieer de IP-adressen die worden toegevoegd aan de groep back-end-server zoals in het volgende voorbeeld wordt getoond.


    $servers = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendServerCollection
    $servers.Add("10.0.0.1")
    $servers.Add("10.0.0.2")

 Het $server-object gebruiken om de waarden toevoegen aan de groep back-end-object ($pool).

    $pool = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendAddressPool
    $pool.BackendServers = $servers
    $pool.Name = "pool1"

De instelling van de groep back-end-server maken.

    $setting = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendHttpSettings
    $setting.Name = "setting1"
    $setting.CookieBasedAffinity = "enabled"
    $setting.Port = 80
    $setting.Protocol = "http"

De listener maken.

    $listener = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.HttpListener
    $listener.Name = "listener1"
    $listener.FrontendPort = "fep1"
    $listener.FrontendIP = "fip1"
    $listener.Protocol = "http"
    $listener.SslCert = ""

De regel maken.

    $rule = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.HttpLoadBalancingRule
    $rule.Name = "rule1"
    $rule.Type = "basic"
    $rule.BackendHttpSettings = "setting1"
    $rule.Listener = "listener1"
    $rule.BackendAddressPool = "pool1"

### <a name="step-2"></a>Stap 2

Alle afzonderlijke configuratie-items toewijzen aan een object application gateway configuration ($appgwconfig).

De front-IP toevoegen aan de configuratie.

    $appgwconfig = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.ApplicationGatewayConfiguration
    $appgwconfig.FrontendIPConfigurations = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.FrontendIPConfiguration]"
    $appgwconfig.FrontendIPConfigurations.Add($fip)

De front-poort toevoegen aan de configuratie.

    $appgwconfig.FrontendPorts = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.FrontendPort]"
    $appgwconfig.FrontendPorts.Add($fep)

De groep met back-end-server toevoegen aan de configuratie.

    $appgwconfig.BackendAddressPools = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendAddressPool]"
    $appgwconfig.BackendAddressPools.Add($pool)

De instelling van de back-end-toepassingen toevoegen aan de configuratie.

    $appgwconfig.BackendHttpSettingsList = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendHttpSettings]"
    $appgwconfig.BackendHttpSettingsList.Add($setting)

De listener toevoegen aan de configuratie.

    $appgwconfig.HttpListeners = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.HttpListener]"
    $appgwconfig.HttpListeners.Add($listener)

De regel toevoegen aan de configuratie.

    $appgwconfig.HttpLoadBalancingRules = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.HttpLoadBalancingRule]"
    $appgwconfig.HttpLoadBalancingRules.Add($rule)

### <a name="step-3"></a>Stap 3

Het configuratieobject toezeggen aan de toepassingsbron gateway met behulp van **Set AzureApplicationGatewayConfig**.

    Set-AzureApplicationGatewayConfig -Name AppGwTest -Config $appgwconfig

## <a name="start-the-gateway"></a>Start de gateway

Na de configuratie van de gateway met de **Start-AzureApplicationGateway** -cmdlet kunt starten de gateway. Facturering van een toepassingsgateway begint nadat de gateway is gestart.

> [AZURE.NOTE] De cmdlet **Start AzureApplicationGateway** duurt maximaal 15-20 minuten duren.

    Start-AzureApplicationGateway AppGwTest

    VERBOSE: 7:59:16 PM - Begin Operation: Start-AzureApplicationGateway
    VERBOSE: 8:05:52 PM - Completed Operation: Start-AzureApplicationGateway
    Name       HTTP Status Code     Operation ID                             Error
    ----       ----------------     ------------                             ----
    Successful OK                   fc592db8-4c58-2c8e-9a1d-1c97880f0b9b

## <a name="verify-the-gateway-status"></a>Controleer of de status van de gateway

De cmdlet **Get-AzureApplicationGateway** gebruiken om de status van de gateway. **Start AzureApplicationGateway** is voltooid in de vorige stap, *staat* moet worden uitgevoerd als *Vip* en *DNS-naam* moet geldig posten hebben.

In het volgende voorbeeld ziet u een toepassingsgateway die omhoog, actief, en klaar voor het nemen van verkeer dat bestemd is voor `http://<generated-dns-name>.cloudapp.net`.

    Get-AzureApplicationGateway AppGwTest

    VERBOSE: 8:09:28 PM - Begin Operation: Get-AzureApplicationGateway
    VERBOSE: 8:09:30 PM - Completed Operation: Get-AzureApplicationGateway
    Name          : AppGwTest
    Description   :
    VnetName      : testvnet1
    Subnets       : {Subnet-1}
    InstanceCount : 2
    GatewaySize   : Medium
    State         : Running
    Vip           : 138.91.170.26
    DnsName       : appgw-1b8402e8-3e0d-428d-b661-289c16c82101.cloudapp.net


## <a name="delete-an-application-gateway"></a>Een toepassingsgateway verwijderen

Een toepassingsgateway verwijderen:

1. Gebruik de cmdlet **Stop AzureApplicationGateway** om te stoppen met de gateway.
2. Gebruik de cmdlet **Verwijderen AzureApplicationGateway** voor het verwijderen van de gateway.
3. Controleer of de gateway is verwijderd met behulp van de cmdlet **Get-AzureApplicationGateway** .

In het volgende voorbeeld ziet u de cmdlet **Stop AzureApplicationGateway** op de eerste regel, gevolgd door de uitvoer.

    Stop-AzureApplicationGateway AppGwTest

    VERBOSE: 9:49:34 PM - Begin Operation: Stop-AzureApplicationGateway
    VERBOSE: 10:10:06 PM - Completed Operation: Stop-AzureApplicationGateway
    Name       HTTP Status Code     Operation ID                             Error
    ----       ----------------     ------------                             ----
    Successful OK                   ce6c6c95-77b4-2118-9d65-e29defadffb8

Zodra de toepassingsgateway een gestopt is, gebruikt u de cmdlet **Verwijderen AzureApplicationGateway** om de service te verwijderen.


    Remove-AzureApplicationGateway AppGwTest

    VERBOSE: 10:49:34 PM - Begin Operation: Remove-AzureApplicationGateway
    VERBOSE: 10:50:36 PM - Completed Operation: Remove-AzureApplicationGateway
    Name       HTTP Status Code     Operation ID                             Error
    ----       ----------------     ------------                             ----
    Successful OK                   055f3a96-8681-2094-a304-8d9a11ad8301

Als u wilt controleren of de service is verwijderd, kunt u de cmdlet **Get-AzureApplicationGateway** . Deze stap is niet vereist.


    Get-AzureApplicationGateway AppGwTest

    VERBOSE: 10:52:46 PM - Begin Operation: Get-AzureApplicationGateway

    Get-AzureApplicationGateway : ResourceNotFound: The gateway does not exist.
    .....

## <a name="next-steps"></a>Volgende stappen

Als u wilt voor het configureren van SSL-offload, Zie [configureren een toepassingsgateway voor SSL-offload](application-gateway-ssl.md).

Als u configureren een toepassingsgateway wilt gebruiken met een interne taakverdeling, Zie [maken een toepassingsgateway met interne taakverdeling (ILB)](application-gateway-ilb.md).

Als u meer informatie over het algemeen balancing opties laden, Zie:

- [Azure Load Balancer](https://azure.microsoft.com/documentation/services/load-balancer/)
- [Azure verkeer Manager](https://azure.microsoft.com/documentation/services/traffic-manager/)

[scenario]: ./media/application-gateway-create-gateway/scenario.png