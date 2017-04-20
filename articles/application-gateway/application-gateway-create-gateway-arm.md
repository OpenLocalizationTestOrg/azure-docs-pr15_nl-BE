<properties
   pageTitle="Maken, starten of een toepassingsgateway verwijderen met behulp van bronbeheer Azure | Microsoft Azure"
   description="Deze pagina bevat instructies voor het maken, configureren, starten en een toepassingsgateway Azure verwijderen met behulp van bronbeheer Azure"
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


# <a name="create-start-or-delete-an-application-gateway-by-using-azure-resource-manager"></a>Maken, starten of een toepassingsgateway verwijderen met behulp van bronbeheer Azure

> [AZURE.SELECTOR]
- [Azure Portal](application-gateway-create-gateway-portal.md)
- [Azure PowerShell voor bronbeheer](application-gateway-create-gateway-arm.md)
- [Azure klassieke PowerShell](application-gateway-create-gateway.md)
- [Sjabloon voor Azure Resource Manager](application-gateway-create-gateway-arm-template.md)
- [Azure CLI](application-gateway-create-gateway-cli.md)

Azure Application Gateway is een laag 7 load balancer. Biedt failover, HTTP-verzoeken prestaties routering tussen verschillende servers, of ze nu op de cloud of op locatie. Application Gateway biedt vele functies van Application Delivery Controller (ADC) met inbegrip van HTTP-taakverdeling, op basis van een cookie sessie affiniteit, Secure Sockets Layer (SSL)-offload, aangepaste gezondheid sondes, ondersteuning voor meerdere locaties en vele andere. Ga voor een volledige lijst van ondersteunde functies naar [Gateway: overzicht](application-gateway-introduction.md)

Dit artikel begeleidt u door de stappen voor het maken, configureren, starten en een toepassingsgateway verwijderen.

>[AZURE.IMPORTANT] Voordat u met Azure resources werken, is het belangrijk te begrijpen Azure heeft momenteel twee implementatiemodellen: Resource Manager en klassiek. Zorg ervoor dat de [distributie modellen en tools](../azure-classic-rm.md) te begrijpen voordat u een bron Azure. U kunt de documentatie bij de hulpprogramma's weergeven door te klikken op de tabs boven aan dit artikel. Dit document heeft betrekking op een toepassingsgateway maken met behulp van bronbeheer Azure. Ga voor het gebruik van de light versie te [maken van een gateway klassieke distributie via PowerShell](application-gateway-create-gateway.md).


## <a name="before-you-begin"></a>Voordat u begint

1. Installeer de nieuwste versie van de Azure PowerShell-cmdlets met behulp van de Web Platform Installer. U kunt downloaden en installeren van de meest recente versie van de **Windows PowerShell** -sectie van de [pagina met Downloads](https://azure.microsoft.com/downloads/).
2. Als u een bestaande virtuele netwerk hebt, selecteert u een bestaande lege subnet of een subnet maken in uw bestaande virtuele netwerk uitsluitend voor gebruik door de toepassingsgateway. U kunt de toepassingsgateway naar een ander virtueel netwerk dan de bronnen die u wilt implementeren achter de toepassingsgateway niet implementeren.
3. De servers die u configureert voor het gebruik van de toepassingsgateway moeten bestaan of de eindpunten ervan gemaakt in het virtuele netwerk of met een openbaar IP-/ VIP hebt toegewezen.

## <a name="what-is-required-to-create-an-application-gateway"></a>Wat is vereist voor het maken van een toepassingsgateway?

- **Back-end server-groep:** De lijst met IP-adressen van de back-end servers. De IP-adressen ofwel moeten behoren tot het virtuele netwerk subnet of moeten een openbaar IP/VIP.
- **Instellingen voor back-end server toepassingen:** Elke groep heeft als poort, protocol en affiniteit op basis van een cookie-instellingen. Deze instellingen zijn gekoppeld aan een groep en worden toegepast op alle servers in de groep.
- **Front-poort:** Dit wordt de openbare poort op de toepassingsgateway wordt geopend. Verkeer treft deze poort en wordt vervolgens omgeleid naar een van de back-end servers.
- **Listener:** De listener is een front-end-poort, een protocol (Http of Https, deze waarden zijn hoofdlettergevoelig), en de SSL-certificaat (als het configureren van SSL-offload).
- **Regel:** De regel wordt de listener van de groep back-end-server gebonden en definieert welke het verkeer moet worden besteed aan een bepaalde listener wanneer deze groep met back-end-server.

## <a name="create-an-application-gateway"></a>Een toepassingsgateway maken

Het verschil tussen het gebruik van klassieke Azure en Azure Resource Manager is de volgorde waarin u de toepassingsgateway en de artikelen die moeten worden geconfigureerd.

Met Resource Manager alle items in een toepassingsgateway worden afzonderlijk geconfigureerd en deze vervolgens samen met de toepassingsbron gateway maken.

De volgende zijn de stappen die nodig zijn voor het maken van een toepassingsgateway.

## <a name="create-a-resource-group-for-resource-manager"></a>Een resourcegroep maken voor bronbeheer

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

    Select-AzureRmSubscription -Subscriptionid "GUID of subscription"

### <a name="step-4"></a>Stap 4

Een resourcegroep (overslaan deze stap als u een bestaande bronnengroep) maken.

    New-AzureRmResourceGroup -Name appgw-rg -Location "West US"

Azure Resource Manager vereist dat alle bronnengroepen van een locatie opgeven. Deze locatie wordt gebruikt als de standaardlocatie voor resources in die groep. Zorg ervoor dat alle opdrachten voor het maken van een application-gateway maakt gebruik van dezelfde resourcegroep.

In het bovenstaande voorbeeld wij een resourcegroep "appgw RG" en de locatie 'West ons' genoemd.

>[AZURE.NOTE] Als u een aangepaste sonde voor uw toepassingsgateway configureren, Zie [maken een toepassingsgateway met aangepaste sondes met PowerShell](application-gateway-create-probe-ps.md). Bekijk de [aangepaste sondes en de statuscontrole](application-gateway-probe-overview.md) voor meer informatie.

## <a name="create-a-virtual-network-and-a-subnet-for-the-application-gateway"></a>Een virtueel netwerk en subnet voor de toepassingsgateway maken

In het volgende voorbeeld ziet u hoe een virtueel netwerk maken met behulp van bronbeheer.

### <a name="step-1"></a>Stap 1

Het adres bereik 10.0.0.0/24 toewijzen aan de variabele subnet moet worden gebruikt voor het maken van een virtueel netwerk.

    $subnet = New-AzureRmVirtualNetworkSubnetConfig -Name subnet01 -AddressPrefix 10.0.0.0/24

### <a name="step-2"></a>Stap 2

Maak een virtueel netwerk met de naam 'appgwvnet' in de resource-groep 'appgw-rg' voor de regio West VS is met het voorvoegsel 10.0.0.0/16 met subnetmasker 10.0.0.0/24.

    $vnet = New-AzureRmVirtualNetwork -Name appgwvnet -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $subnet

### <a name="step-3"></a>Stap 3

Een variabele van het subnet voor de volgende stappen die een toepassingsgateway maken toewijzen.

    $subnet=$vnet.Subnets[0]

## <a name="create-a-public-ip-address-for-the-front-end-configuration"></a>Maak een openbaar IP-adres voor de front-configuratie

Maak een openbaar IP-bron "publicIP01" in de resource-groep 'appgw-rg' voor de regio West VS.

    $publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-rg -name publicIP01 -location "West US" -AllocationMethod Dynamic


## <a name="create-an-application-gateway-configuration-object"></a>Maakt een object application gateway configureren

U moet alle configuratie-items instellen voordat u de toepassingsgateway maakt. De volgende stappen maakt de configuratie-items die nodig zijn voor een gateway-toepassingsbron.

### <a name="step-1"></a>Stap 1

Een toepassing IP-configuratie van de standaardgateway met de naam 'gatewayIP01' maken. Als Gateway-toepassing wordt gestart, neemt een IP-adres van het subnet is geconfigureerd en netwerkverkeer routeert naar de IP-adressen in de back-end-IP-adresgroep. Houd er rekening mee dat elk exemplaar één IP-adres wordt.

    $gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $subnet

### <a name="step-2"></a>Stap 2

De back-end-IP-adresgroep met de naam 'pool01' met IP-adressen configureren "134.170.185.46, 134.170.188.221,134.170.185.50". Deze IP-adressen zijn de IP-adressen die het netwerkverkeer dat afkomstig van de front-IP-eindpunt is worden weergegeven. U vervangt het vorige IP-adressen als u wilt uw eigen eindpunten toepassing IP-adres toevoegen.

    $pool = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 134.170.185.46, 134.170.188.221,134.170.185.50

### <a name="step-3"></a>Stap 3

Application gateway instelling 'poolsetting01' voor het netwerkverkeer verdeeld in de back-end-toepassingen configureren.

    $poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name poolsetting01 -Port 80 -Protocol Http -CookieBasedAffinity Disabled

### <a name="step-4"></a>Stap 4

Configureer de front-end-IP-poort met de naam 'frontendport01' voor het openbare IP-eindpunt.

    $fp = New-AzureRmApplicationGatewayFrontendPort -Name frontendport01  -Port 80

### <a name="step-5"></a>Stap 5

Maken van de front-IP-configuratie met de naam 'fipconfig01' en het openbare IP-adres koppelen aan de front-end-IP-configuratie.

    $fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name fipconfig01 -PublicIPAddress $publicip

### <a name="step-6"></a>Stap 6

Maak de listener naam 'listener01' en koppel de front-poort op de front-end-IP-configuratie.

    $listener = New-AzureRmApplicationGatewayHttpListener -Name listener01  -Protocol Http -FrontendIPConfiguration $fipconfig -FrontendPort $fp

### <a name="step-7"></a>Stap 7

De load balancer regel routering met de naam 'rule01', waarmee de load balancer gedrag maken.

    $rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name rule01 -RuleType Basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool

### <a name="step-8"></a>Stap 8

De grootte van het exemplaar van de application-gateway configureren.

    $sku = New-AzureRmApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2

>[AZURE.NOTE]  De standaardwaarde voor *InstanceCount* is 2, met een maximale waarde van 10. De standaardwaarde voor *GatewaySize* is normaal. U kunt kiezen tussen Standard_Small, Standard_Medium en Standard_Large.

## <a name="create-an-application-gateway-by-using-new-azurermapplicationgateway"></a>Een toepassingsgateway maken met nieuwe AzureRmApplicationGateway

Maak een toepassingsgateway met alle configuratie-items uit de voorgaande stappen. In dit voorbeeld wordt de toepassingsgateway 'appgwtest' genoemd.

    $appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku

### <a name="step-9"></a>Stap 9

DNS- en VIP-details van de toepassingsgateway van de openbare IP-resource is gekoppeld aan de toepassingsgateway ophalen.

    Get-AzureRmPublicIpAddress -Name publicIP01 -ResourceGroupName appgw-rg  

## <a name="delete-an-application-gateway"></a>Een toepassingsgateway verwijderen

U verwijdert een toepassingsgateway, volg deze stappen:

### <a name="step-1"></a>Stap 1

Het object application gateway ophalen en deze te koppelen aan een variabele "$getgw".

    $getgw = Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg

### <a name="step-2"></a>Stap 2

**Stop AzureRmApplicationGateway** de toepassingsgateway niet meer gebruiken.

    Stop-AzureRmApplicationGateway -ApplicationGateway $getgw  

Zodra de toepassingsgateway een gestopt is, gebruikt u de cmdlet **Verwijderen AzureRmApplicationGateway** om de service te verwijderen.

    Remove-AzureRmApplicationGateway -Name $appgwtest -ResourceGroupName appgw-rg -Force

>[AZURE.NOTE] De **-force** schakeloptie onderdrukt u het bevestigingsbericht voor verwijderen kan worden gebruikt.

Als u wilt controleren of de service is verwijderd, kunt u de cmdlet **Get-AzureRmApplicationGateway** . Deze stap is niet vereist.

    Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg

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

Als u wilt voor het configureren van SSL-offload, Zie [configureren een toepassingsgateway voor SSL-offload](application-gateway-ssl.md).

Als u configureren een toepassingsgateway wilt gebruiken met een interne taakverdeling, Zie [maken een toepassingsgateway met interne taakverdeling (ILB)](application-gateway-ilb.md).

Als u meer informatie over het algemeen balancing opties laden, Zie:

- [Azure Load Balancer](https://azure.microsoft.com/documentation/services/load-balancer/)
- [Azure verkeer Manager](https://azure.microsoft.com/documentation/services/traffic-manager/)
