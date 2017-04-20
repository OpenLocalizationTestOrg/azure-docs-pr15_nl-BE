<properties
   pageTitle="Web Application Firewall configureren op nieuwe of bestaande toepassingsgateway | Microsoft Azure"
   description="Dit artikel biedt u informatie over het gebruik van web application firewall op een bestaande of nieuwe application gateway."
   documentationCenter="na"
   services="application-gateway"
   authors="georgewallace"
   manager="carmonm"
   editor="tysonn"/>
<tags
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/25/2016"
   ms.author="gwallace"/>

# <a name="configure-web-application-firewall-on-a-new-or-existing-application-gateway"></a>Web Application Firewall configureren op een nieuwe of bestaande Application Gateway

> [AZURE.SELECTOR]
- [Azure portal](application-gateway-web-application-firewall-portal.md)
- [Azure PowerShell voor bronbeheer](application-gateway-web-application-firewall-powershell.md)

Webtoepassingen beschermt web application firewall (WAF) in Azure toepassingsgateway tegen veelvoorkomende aanvallen zoals SQL injection aanvallen via cross-site scripting en sessie hijacks.

Azure Application Gateway is een laag 7 load balancer. Biedt failover, HTTP-verzoeken prestaties routering tussen verschillende servers, of ze nu op de cloud of op locatie. Toepassing biedt vele functies van Application Delivery Controller (ADC) met inbegrip van HTTP-taakverdeling, op basis van een cookie sessie affiniteit, Secure Sockets Layer (SSL)-offload, aangepaste gezondheid sondes, ondersteuning voor meerdere locaties en vele andere. Ga voor een volledige lijst van ondersteunde functies naar Gateway: overzicht

Het volgende artikel wordt getoond hoe [een toepassingsgateway bestaande web application firewall toevoegen](#add-web-application-firewall-to-an-existing-application-gateway) en [maakt een toepassingsgateway dat web application firewall gebruikt](#create-an-application-gateway-with-web-application-firewall).

![afbeelding met scenario][scenario]

## <a name="waf-configuration-differences"></a>WAF configuratie verschillen

Als u [een toepassingsgateway met PowerShell maken](application-gateway-create-gateway-arm.md)hebt gelezen, kunt u de SKU-instellingen configureren tijdens het maken van een toepassingsgateway begrijpen. WAF biedt extra instellingen op te geven bij het configureren van de SKU voor een toepassingsgateway. Er zijn geen extra wijzigingen die u op de toepassingsgateway zelf aanbrengt.

**SKU** - een normale toepassingsgateway zonder WAF ondersteunt **Standard\_kleine**, **Standard\_gemiddeld**, en **Standard\_groot** formaten. Met de introductie van WAF, er zijn twee extra SKU's **WAF\_normaal** en **WAF\_groot**. WAF wordt niet ondersteund op een kleine Toepassingsgateways.

**Laag** - de beschikbare waarden zijn **standaard** of **WAF**. Bij het gebruik van Web Application Firewall moet **WAF** worden gekozen.

**Modus** - deze instelling is de WAF-modus. toegestane waarden zijn **detecteren** en **tegenhouden**. Als WAF in de detectie-modus is ingesteld, worden alle bedreigingen in een logboekbestand opgeslagen. Gebeurtenissen worden steeds vastgelegd in de modus voor preventie, maar de aanvaller 403 onbevoegde antwoord ontvangt van de toepassingsgateway.

## <a name="add-web-application-firewall-to-an-existing-application-gateway"></a>Web application firewall toevoegen aan een bestaande toepassingsgateway

Let erop dat u de nieuwste versie van Azure PowerShell. Meer info vindt u op [Met behulp van Windows PowerShell met Resource Manager](../powershell-azure-resource-manager.md).

### <a name="step-1"></a>Stap 1

Log in op uw Account Azure.

    Login-AzureRmAccount

### <a name="step-2"></a>Stap 2

Selecteer het abonnement voor dit scenario.

    Select-AzureRmSubscription -SubscriptionName "<Subscription name>"

### <a name="step-3"></a>Stap 3

De gateway die u Web Application Firewall te toevoegt halen.

    $gw = Get-AzureRmApplicationGateway -Name "AdatumGateway" -ResourceGroupName "MyResourceGroup"


### <a name="step-4"></a>Stap 4

De sku web application firewall configureren. De beschikbare formaten worden **WAF\_grote** en **WAF\_normaal**. Wanneer u web application firewall gebruikt moet de laag **WAF**, de capaciteit bij het instellen van de sku moet worden bevestigd.

    $gw | Set-AzureRmApplicationGatewaySku -Name WAF_Large -Tier WAF -Capacity 2

### <a name="step-5"></a>Stap 5

De WAF-instellingen configureren, zoals gedefinieerd in het volgende voorbeeld:

De beschikbare waarden zijn voor de instelling van de **WafMode** , preventie en detectie.

    $gw | Set-AzureRmApplicationGatewayWebApplicationFirewallConfiguration -Enabled $true -FirewallMode Prevention

### <a name="step-6"></a>Stap 6

De toepassingsgateway bijwerken met de instellingen die zijn gedefinieerd in de vorige stap.

    Set-AzureRmApplicationGateway -ApplicationGateway $gw

Deze opdracht werkt u de toepassingsgateway Web Application firewall. Het is raadzaam [Application Gateway diagnostische gegevens](application-gateway-diagnostics.md) om te begrijpen hoe u Logboeken voor uw toepassingsgateway weergeven. Vanwege de aard van de beveiliging van WAF moeten de logboeken regelmatig opnieuw worden bezien om te begrijpen van de houding van de beveiliging van uw webtoepassingen.

## <a name="create-an-application-gateway-with-web-application-firewall"></a>Een Application Gateway met Firewall voor Internet-toepassing maken

De volgende stappen gaat u door het hele proces van begin tot eind voor het maken van een toepassingsgateway Web Application firewall.

Let erop dat u de nieuwste versie van Azure PowerShell. Meer info vindt u op [Met behulp van Windows PowerShell met Resource Manager](../powershell-azure-resource-manager.md).

### <a name="step-1"></a>Stap 1

Log in op Azure

    Login-AzureRmAccount

U wordt gevraagd te verifiëren met uw referenties.

### <a name="step-2"></a>Stap 2

Controleer de abonnementen voor de account.

    Get-AzureRmSubscription

### <a name="step-3"></a>Stap 3

Kies welke van uw Azure-abonnementen te gebruiken.

    Select-AzureRmsubscription -SubscriptionName "<Subscription name>"

### <a name="step-4"></a>Stap 4

Een resourcegroep (overslaan deze stap als u een bestaande bronnengroep) maken.

    New-AzureRmResourceGroup -Name appgw-rg -Location "West US"

Azure Resource Manager vereist dat alle bronnengroepen van een locatie opgeven. Deze locatie wordt gebruikt als de standaardlocatie voor resources in die groep. Zorg ervoor dat alle opdrachten voor het maken van een application-gateway maakt gebruik van dezelfde resourcegroep.

In het voorgaande voorbeeld wij een resourcegroep "appgw RG" en de locatie 'West ons' genoemd.

>[AZURE.NOTE] Als u een aangepaste sonde voor uw toepassingsgateway configureren, Zie [maken een toepassingsgateway met aangepaste sondes met PowerShell](application-gateway-create-probe-ps.md). Bekijk de [aangepaste sondes en de statuscontrole](application-gateway-probe-overview.md) voor meer informatie.

### <a name="step-5"></a>Stap 5

Een adresbereik toewijzen voor het subnet worden gebruikt voor de Gateway-toepassing zelf.

    $gwSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name 'appgwsubnet' -AddressPrefix 10.0.0.0/24

> [AZURE.NOTE] Een subnet voor een toepassing moet minimaal 28 maskerbits hebben. Deze waarde laat 10 beschikbare adressen in het subnet voor een toepassingsgateway exemplaren. Met een kleinere subnet u mogelijk niet in de toekomst meer exemplaar van de toepassingsgateway van uw toevoegen.

### <a name="step-6"></a>Stap 6

Een adresbereik moet worden gebruikt voor de back-end-adresgroep toewijzen.

    $nicSubnet = New-AzureRmVirtualNetworkSubnetConfig  -Name 'appsubnet' -AddressPrefix 10.0.2.0/24

### <a name="step-7"></a>Stap 7

Een virtueel netwerk met de voorgaande subnetten in de resourcegroep die is gemaakt in stap maken: [de resourcegroep maken](#create-the-resource-group)

    $vnet = New-AzureRmvirtualNetwork -Name 'appgwvnet' -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $gwSubnet, $nicSubnet

### <a name="step-8"></a>Stap 8

Ophalen in de resource virtueel netwerk en de bronnen van het subnet worden gebruikt in de volgende stappen uit:

    $vnet = Get-AzureRmvirtualNetwork -Name 'appgwvnet' -ResourceGroupName appgw-rg
    $gwSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'appgwsubnet' -VirtualNetwork $vnet
    $nicSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'appsubnet' -VirtualNetwork $vnet

### <a name="step-9"></a>Stap 9

Maak een openbare IP-bron voor de toepassingsgateway moet worden gebruikt. Deze openbare IP-adres wordt gebruikt in een van de volgende stappen uit:

    $publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-rg -name 'appgwpip' -Location "West US" -AllocationMethod Dynamic

> [AZURE.IMPORTANT] Toepassingsgateway biedt geen ondersteuning voor het gebruik van een openbaar IP-adres dat is gemaakt met een domeinlabel gedefinieerd. Alleen een openbaar IP-adres met een domeinlabel dynamisch gemaakte wordt ondersteund. Als u een beschrijvende DNS-naam voor de toepassingsgateway nodig, verdient het gebruik van een CNAME-record als een alias.

### <a name="step-10"></a>Stap 10

U moet alle configuratie-items instellen voordat u de toepassingsgateway maakt. De volgende stappen maakt de configuratie-items die nodig zijn voor een gateway-toepassingsbron.

Een toepassing gateway IP-configuratie maakt, welk subnet maakt gebruik van de Gateway toepassing Hiermee worden. Als Gateway-toepassing wordt gestart, neemt een IP-adres van het subnet is geconfigureerd en netwerkverkeer routeert naar de IP-adressen in de back-end-IP-adresgroep. Houd er rekening mee dat elk exemplaar één IP-adres wordt.

    $gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name 'gwconfig' -Subnet $gwSubnet

### <a name="step-11"></a>Stap 11

De back-end-IP-adresgroep configureren met de IP-adressen van de backend-webservers. Deze IP-adressen zijn de IP-adressen die het netwerkverkeer dat afkomstig van de front-IP-eindpunt is worden weergegeven. U vervangt de volgende IP-adressen als u wilt uw eigen eindpunten toepassing IP-adres toevoegen.

    $pool = New-AzureRmApplicationGatewayBackendAddressPool -Name 'pool01' -BackendIPAddresses 1.1.1.1, 2.2.2.2, 3.3.3.3

### <a name="step-12"></a>Stap 12

Het certificaat moet worden gebruikt op de resources ssl is ingeschakeld in de backend uploaden.

    $authcert = New-AzureRmApplicationGatewayAuthenticationCertificate -Name 'whitelistcert1' -CertificateFile <full path to .cer file>

### <a name="step-13"></a>Stap 13

De instellingen voor HTTP-back-end-gateway configureren. Het uploaden van de vorige stap om de instellingen voor HTTP-certificaat toewijzen.

    $poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name 'setting01' -Port 443 -Protocol Https -CookieBasedAffinity Enabled -AuthenticationCertificates $authcert

### <a name="step-14"></a>Stap 14

Configureer de front-end-IP-poort voor het openbare IP-eindpunt. Deze poort is de poort waarmee eindgebruikers verbinding maken.

    $fp = New-AzureRmApplicationGatewayFrontendPort -Name 'port01'  -Port 443

### <a name="step-15"></a>Stap 15

Een front-end-IP-adresconfiguratie te maken, deze instelling wordt een particuliere of openbare IP-adres toegewezen aan de front-end van de toepassingsgateway. Het openbare IP-adres in de vorige stap koppelt de volgende stap aan de front-end-IP-configuratie.

    $fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name 'fip01' -PublicIPAddress $publicip

### <a name="step-16"></a>Stap 16

Het certificaat voor de toepassingsgateway configureren. Dit certificaat wordt gebruikt voor het decoderen en het verkeer op de toepassingsgateway opnieuw te coderen.

    $cert = New-AzureRmApplicationGatewaySslCertificate -Name cert01 -CertificateFile <full path to .pfx file> -Password <password for certificate file>

### <a name="step-17"></a>Stap 17

De HTTP-listener voor de toepassingsgateway maken. Het front-IP-configuratie, poort en ssl certificaat toewijzen.

    $listener = New-AzureRmApplicationGatewayHttpListener -Name listener01 -Protocol Https -FrontendIPConfiguration $fipconfig -FrontendPort $fp -SslCertificate $cert

### <a name="step-18"></a>Stap 18

Maak een load balancer routing-regel die de load balancer gedrag wordt geconfigureerd. In dit voorbeeld wordt wordt een regel basic round-robin gemaakt.

    $rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name 'rule01' -RuleType basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool
   
### <a name="step-19"></a>Stap 19

De grootte van het exemplaar van de application-gateway configureren.

    $sku = New-AzureRmApplicationGatewaySku -Name WAF_Medium -Tier WAF -Capacity 2

>[AZURE.NOTE]  U kunt kiezen tussen **WAF\_normaal** en **WAF\_groot**, de laag als altijd **WAF**WAF gebruik is. Capaciteit is een getal tussen 1 en 10.

### <a name="step-20"></a>Stap 20

Configureren van de modus voor WAF, acceptabele waarden zijn **preventie** en **detectie**.

    $config = New-AzureRmApplicationGatewayWafConfig -Enabled $true -WafMode "Prevention"

### <a name="step-21"></a>Stap 21

Maak een toepassingsgateway met alle configuratie-items uit de voorgaande stappen. In dit voorbeeld wordt de toepassingsgateway 'appgwtest' genoemd.

    $appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku -WebApplicationFirewallConfig $config -SslCertificates $cert -AuthenticationCertificates $authcert

## <a name="get-application-gateway-dns-name"></a>Toepassing-gateway, DNS-naam verkrijgen

Zodra de gateway is gemaakt, is de volgende stap voor het configureren van de front-end voor communicatie. Als u een openbaar IP-adres gebruikt, moet toepassingsgateway een dynamisch toegewezen DNS-naam, niet milieuvriendelijk is. Om eindgebruikers de toepassingsgateway een CNAME-record kunt vertrekken kan worden gebruikt om te verwijzen naar het openbare eindpunt van de toepassingsgateway. [Een aangepaste domeinnaam in Azure configureren](../cloud-services/cloud-services-custom-domain-name-portal.md). U doet dit door details van de toepassingsgateway en de bijbehorende IP/DNS-naam met behulp van het element PublicIPAddress is gekoppeld aan de toepassingsgateway worden opgehaald. DNS-naam van de van de toepassingsgateway moet worden gebruikt voor het maken van een CNAME-record die naar deze DNS-naam de twee toepassingen verwijst. Het gebruik van A-records wordt niet aanbevolen, omdat de VIP bij opnieuw opstarten van application gateway kan veranderen.
    
    Get-AzureRmPublicIpAddress -ResourceGroupName appgw-RG -Name publicIP01
        
    Name                     : publicIP01
    ResourceGroupName        : appgw-RG
    Location                 : westus
    Id                       : /subscriptions/<subscription_id>/resourceGroups/appgw-RG/providers/Microsoft.Network/publicIPAddresses/publicIP01
    Etag                     : W/"00000d5b-54ed-4907-bae8-99bd5766d0e5"
    ResourceGuid             : 00000000-0000-0000-0000-000000000000
    ProvisioningState        : Succeeded
    Tags                     : 
    PublicIpAllocationMethod : Dynamic
    IpAddress                : xx.xx.xxx.xx
    PublicIpAddressVersion   : IPv4
    IdleTimeoutInMinutes     : 4
    IpConfiguration          : {
                                 "Id": "/subscriptions/<subscription_id>/resourceGroups/appgw-RG/providers/Microsoft.Network/applicationGateways/appgwtest/frontendIP
                               Configurations/frontend1"
                               }
    DnsSettings              : {
                                 "Fqdn": "00000000-0000-xxxx-xxxx-xxxxxxxxxxxx.cloudapp.net"
                               }

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het vastleggen van diagnostische gegevens, de gebeurtenissen die zijn aangetroffen of kon Meld Web Application firewall via [Application Gateway diagnostische gegevens](application-gateway-diagnostics.md) configureren


[scenario]: ./media/application-gateway-web-application-firewall-powershell/scenario.png