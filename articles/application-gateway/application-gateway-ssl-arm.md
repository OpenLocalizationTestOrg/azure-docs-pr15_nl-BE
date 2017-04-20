<properties
   pageTitle="Een toepassingsgateway voor offload van SSL configureren met behulp van bronbeheer Azure | Microsoft Azure"
   description="Deze pagina bevat instructies voor het maken van een toepassingsgateway met SSL via Azure Resource Manager-offload"
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
   ms.date="09/09/2016"
   ms.author="gwallace"/>

# <a name="configure-an-application-gateway-for-ssl-offload-by-using-azure-resource-manager"></a>Een toepassingsgateway voor offload van SSL configureren met behulp van bronbeheer Azure

> [AZURE.SELECTOR]
-[Azure Portal](application-gateway-ssl-portal.md)
-[Azure PowerShell voor Resource Manager](application-gateway-ssl-arm.md)
-[Azure klassieke PowerShell](application-gateway-ssl.md)

 Azure toepassingsgateway kan worden geconfigureerd om de Secure Sockets Layer (SSL)-sessie op de gateway om te voorkomen dat kostbare SSL-decodering taken te gebeuren op de web-farm te beëindigen. SSL-offload vereenvoudigt ook de front-end-server setup en het beheer van de webtoepassing.

## <a name="before-you-begin"></a>Voordat u begint

1. Installeer de nieuwste versie van de Azure PowerShell-cmdlets met behulp van de Web Platform Installer. U kunt downloaden en installeren van de meest recente versie van de **Windows PowerShell** -sectie van de [pagina met Downloads](https://azure.microsoft.com/downloads/).
2. U maakt een virtueel netwerk en subnet voor de toepassingsgateway. Controleer of geen virtuele machines of cloud implementaties het subnet worden gebruikt. Toepassingsgateway moet zelf in een virtueel netwerk subnet.
3. De servers die u kunt configureren voor het gebruik van de toepassingsgateway moeten bestaan of de eindpunten ervan gemaakt in het virtuele netwerk of met een openbaar IP-/ VIP hebt toegewezen.

## <a name="what-is-required-to-create-an-application-gateway"></a>Wat is vereist voor het maken van een toepassingsgateway?


- **Back-end server-groep:** De lijst met IP-adressen van de back-end servers. De IP-adressen ofwel moeten behoren tot het virtuele netwerk subnet of moeten een openbaar IP/VIP.
- **Instellingen voor back-end server toepassingen:** Elke groep heeft als poort, protocol en affiniteit op basis van een cookie-instellingen. Deze instellingen zijn gekoppeld aan een groep en worden toegepast op alle servers in de groep.
- **Front-poort:** Dit wordt de openbare poort op de toepassingsgateway wordt geopend. Verkeer treft deze poort en wordt vervolgens omgeleid naar een van de back-end servers.
- **Listener:** De listener is een front-end-poort, een protocol (Http of Https, deze instellingen zijn hoofdlettergevoelig), en de SSL-certificaat (als het configureren van SSL-offload).
- **Regel:** De regel wordt de listener en de groep met back-end-server en wordt gedefinieerd welke het verkeer moet worden besteed aan een bepaalde listener wanneer deze groep met back-end-server. Op dit moment wordt *alleen de basisregel* ondersteund. *De basisregel* is de verdeling van de belasting van round-robin.

**Aanvullende opmerkingen**

Voor het configureren van SSL-certificaten moet het protocol in de **HttpListener** veranderen in *Https* (hoofdlettergevoelig). Het element **SslCertificate** is toegevoegd aan **HttpListener** met de waarde van de variabele voor het SSL-certificaat is geconfigureerd. De front-poort moet worden bijgewerkt op 443.

**Inschakelen van de affiniteit op basis van cookie**: een toepassingsgateway kan worden geconfigureerd om ervoor te zorgen dat een aanvraag van een clientsessie altijd naar dezelfde VM in de farm wordt gestuurd. In dit scenario wordt gedaan door injectie van een sessiecookie waarmee de gateway naar directe verkeer op de juiste manier. Instellen zodat op basis van een cookie affiniteit **CookieBasedAffinity** *ingeschakeld* in het element **BackendHttpSettings** .


## <a name="create-an-application-gateway"></a>Een toepassingsgateway maken

Het verschil tussen het gebruik van de klassieke Azure implementatiemodel en Azure Resource Manager is de volgorde die u maakt een toepassingsgateway en de artikelen die moeten worden geconfigureerd.

Met Resource Manager alle onderdelen van een toepassingsgateway worden afzonderlijk geconfigureerd en deze vervolgens samen voor het maken van een gateway-toepassingsbron.


Hier zijn de stappen die nodig zijn voor het maken van een toepassingsgateway:

1. Een resourcegroep maken voor bronbeheer
2. Virtueel netwerk subnet en openbare IP-voor de toepassingsgateway maken
3. Maakt een object application gateway configureren
4. Maken van een gateway-toepassingsbron


## <a name="create-a-resource-group-for-resource-manager"></a>Een resourcegroep maken voor bronbeheer

Zorg ervoor dat u PowerShell modus de Azure Resource Manager-cmdlets gebruiken als u wilt overschakelen. Meer info vindt u op [Met behulp van Windows PowerShell met Resource Manager](../powershell-azure-resource-manager.md).

### <a name="step-1"></a>Stap 1

    Login-AzureRmAccount

### <a name="step-2"></a>Stap 2

Controleer de abonnementen voor de account.

    Get-AzureRmSubscription

U wordt gevraagd te verifiëren met uw referenties.<BR>

### <a name="step-3"></a>Stap 3

Kies welke van uw Azure-abonnementen te gebruiken. <BR>


    Select-AzureRmSubscription -Subscriptionid "GUID of subscription"


### <a name="step-4"></a>Stap 4

Een resourcegroep (overslaan deze stap als u een bestaande bronnengroep) maken.

    New-AzureRmResourceGroup -Name appgw-rg -Location "West US"

Azure Resource Manager vereist dat alle bronnengroepen van een locatie opgeven. Deze instelling wordt gebruikt als de standaardlocatie voor resources in die groep. Zorg ervoor dat alle opdrachten voor het maken van een application-gateway maakt gebruik van dezelfde resourcegroep.

In het bovenstaande voorbeeld wij een resourcegroep "appgw RG" en de locatie 'West ons' genoemd.

## <a name="create-a-virtual-network-and-a-subnet-for-the-application-gateway"></a>Een virtueel netwerk en subnet voor de toepassingsgateway maken

In het volgende voorbeeld ziet u hoe een virtueel netwerk maken met behulp van Resource Manager:

### <a name="step-1"></a>Stap 1

    $subnet = New-AzureRmVirtualNetworkSubnetConfig -Name subnet01 -AddressPrefix 10.0.0.0/24

In dit voorbeeld wordt het adres bereik 10.0.0.0/24 toegewezen aan een variabele subnet moet worden gebruikt voor het maken van een virtueel netwerk.

### <a name="step-2"></a>Stap 2

    $vnet = New-AzureRmVirtualNetwork -Name appgwvnet -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $subnet

In dit voorbeeld wordt een virtueel netwerk met de naam 'appgwvnet' in de resource-groep 'appgw-rg' voor de regio West VS is met het voorvoegsel 10.0.0.0/16 met subnetmasker 10.0.0.0/24.

### <a name="step-3"></a>Stap 3

    $subnet = $vnet.Subnets[0]

In dit voorbeeld wordt het subnetobject toegewezen aan de variabele $subnet voor de volgende stappen.

## <a name="create-a-public-ip-address-for-the-front-end-configuration"></a>Maak een openbaar IP-adres voor de front-configuratie

    $publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-rg -name publicIP01 -location "West US" -AllocationMethod Dynamic

In dit voorbeeld wordt een openbaar IP-bron in een resource-groep 'appgw-rg' voor de regio West VS ' publicIP01'.


## <a name="create-an-application-gateway-configuration-object"></a>Maakt een object application gateway configureren

### <a name="step-1"></a>Stap 1

    $gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $subnet

In dit voorbeeld wordt een toepassing IP-configuratie van de standaardgateway met de naam 'gatewayIP01'. Als Gateway-toepassing wordt gestart, neemt een IP-adres van het subnet is geconfigureerd en netwerkverkeer doorsturen naar de IP-adressen in de back-end-IP-adresgroep. Houd er rekening mee dat elk exemplaar één IP-adres wordt.

### <a name="step-2"></a>Stap 2

    $pool = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 134.170.185.46, 134.170.188.221,134.170.185.50

In dit voorbeeld wordt de back-end-IP-adresgroep met de naam 'pool01' met IP-adressen geconfigureerd "134.170.185.46, 134.170.188.221,134.170.185.50." Deze waarden zijn de IP-adressen die het netwerkverkeer dat afkomstig van de front-IP-eindpunt is worden weergegeven. De IP-adressen uit het voorgaande voorbeeld vervangen door de IP-adressen van de eindpunten van de web-toepassingen.

### <a name="step-3"></a>Stap 3

    $poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name poolsetting01 -Port 80 -Protocol Http -CookieBasedAffinity Enabled

In dit voorbeeld wordt geconfigureerd application gateway instelling 'poolsetting01' voor taakverdeling van netwerkverkeer in de back-end-toepassingen.

### <a name="step-4"></a>Stap 4

    $fp = New-AzureRmApplicationGatewayFrontendPort -Name frontendport01  -Port 443

In dit voorbeeld wordt geconfigureerd voor de front-end-IP-poort met de naam 'frontendport01' voor het openbare IP-eindpunt.

### <a name="step-5"></a>Stap 5

    $cert = New-AzureRmApplicationGatewaySslCertificate -Name cert01 -CertificateFile <full path for certificate file> -Password ‘<password>’

In dit voorbeeld wordt het certificaat voor SSL-verbinding geconfigureerd. Het certificaat moet in de PFX-indeling en het wachtwoord moet tussen de 4 tot 12 tekens.

### <a name="step-6"></a>Stap 6

    $fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name fipconfig01 -PublicIPAddress $publicip

In dit voorbeeld maakt de front-IP-configuratie met de naam 'fipconfig01' en wordt het openbare IP-adres gekoppeld aan de front-end-IP-configuratie.

### <a name="step-7"></a>Stap 7

    $listener = New-AzureRmApplicationGatewayHttpListener -Name listener01  -Protocol Https -FrontendIPConfiguration $fipconfig -FrontendPort $fp -SslCertificate $cert


In dit voorbeeld maakt de listener de naam "listener01" en de front-poort op de front-end-IP-configuratie en het certificaat koppelt.

### <a name="step-8"></a>Stap 8

    $rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name rule01 -RuleType Basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool

In dit voorbeeld wordt de load balancer regel routering met de naam 'rule01', waarmee het gedrag van load balancer wordt geconfigureerd.

### <a name="step-9"></a>Stap 9

    $sku = New-AzureRmApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2

In dit voorbeeld wordt de grootte van het exemplaar van de application-gateway geconfigureerd.

>[AZURE.NOTE]  De standaardwaarde voor *InstanceCount* is 2, met een maximale waarde van 10. De standaardwaarde voor *GatewaySize* is normaal. U kunt kiezen tussen Standard_Small, Standard_Medium en Standard_Large.

## <a name="create-an-application-gateway-by-using-new-azureapplicationgateway"></a>Een toepassingsgateway maken met nieuwe AzureApplicationGateway

    $appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku -SslCertificates $cert

In dit voorbeeld wordt een toepassingsgateway met alle configuratie-items uit de voorgaande stappen. In het voorbeeld wordt de toepassingsgateway 'appgwtest' genoemd.

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

Als u wilt configureren een toepassingsgateway met interne taakverdeling (ILB), Zie [maken een toepassingsgateway met interne taakverdeling (ILB)](application-gateway-ilb.md).

Als u meer informatie over het algemeen balancing opties laden, Zie:

- [Azure Load Balancer](https://azure.microsoft.com/documentation/services/load-balancer/)
- [Azure verkeer Manager](https://azure.microsoft.com/documentation/services/traffic-manager/)
