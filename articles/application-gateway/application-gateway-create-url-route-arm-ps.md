<properties
   pageTitle="Een toepassingsgateway met URL-regels voor routing maken | Microsoft Azure"
   description="Deze pagina bevat instructies voor het maken, een Azure toepassingsgateway met URL-regels voor routing configureren"
   documentationCenter="na"
   services="application-gateway"
   authors="georgewallace"
   manager="jdial"
   editor="tysonn"/>
<tags
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/25/2016"
   ms.author="gwallace"/>


# <a name="create-an-application-gateway-using-path-based-routing"></a>Een toepassingsgateway met routering op basis van een pad maken 

> [AZURE.SELECTOR]
- [Azure portal](application-gateway-create-url-route-portal.md)
- [Azure PowerShell voor bronbeheer](application-gateway-create-url-route-arm-ps.md)

URL-pad gebaseerde routering kunt u routes op basis van het URL-pad van een HTTP-aanvraag koppelen. Het wordt gecontroleerd of er een route naar een back-end-toepassingen voor de lijsten URL in Application Gateway geconfigureerd is en het netwerkverkeer te verzenden naar de gedefinieerde groep met back-end. Vaak gebruikt voor routering op basis van een URL worden geladen aanvragen voor verschillende typen inhoud aan verschillende back-end-server-toepassingen.

Op URL gebaseerde routering wordt een nieuw type application gateway geïntroduceerd. Toepassingsgateway heeft twee typen: basic en PathBasedRouting. Basisregel type round robin-service voor de back-end-toepassingen tijdens het PathBasedRouting naast de round-robin distributie, ook rekening pad patroon van de aanvraag-URL bij het kiezen van de back-end-toepassingen.

>[AZURE.IMPORTANT] PathPattern: De lijst met patronen pad aan. Elk moet beginnen met / en de enige plaats waar een "\*" mag zich aan het einde. Voorbeelden van geldige zijn/xyz, / XYZ* of /xyz/*. De tekenreeks die is ingevoerd in het pad matcher geen tekst bevatten na de eerste "?" of "#" en deze tekens zijn niet toegestaan. 

## <a name="scenario"></a>Scenario
In het volgende voorbeeld fungeert toepassingsgateway verkeer voor contoso.com met twee back-end-server-toepassingen: video server-groep en image server-groep.

Aanvragen voor http://contoso.com/image* worden gerouteerd naar image server-groep (pool1), en http://contoso.com/video* worden gerouteerd naar een video server-groep (pool2). Als geen van de patronen pad overeenkomt met een standaard server-groep (pool1) geselecteerd.

![URL-route](./media/application-gateway-create-url-route-arm-ps/figure1.png)

## <a name="before-you-begin"></a>Voordat u begint

1. Installeer de nieuwste versie van de Azure PowerShell-cmdlets met behulp van de Web Platform Installer. U kunt downloaden en installeren van de meest recente versie van de **Windows PowerShell** -sectie van de [pagina met Downloads](https://azure.microsoft.com/downloads/).
2. Maakt u een virtueel netwerk en subnet voor Application Gateway. Controleer of geen virtuele machines of cloud implementaties het subnet worden gebruikt. De toepassingsgateway moet zijn op zichzelf in een virtueel netwerk subnet.
3. De servers die zijn toegevoegd aan de groep back-end gebruiken de toepassingsgateway moeten bestaan of zijn de eindpunten ervan gemaakt in het virtuele netwerk of met een openbaar IP-/ VIP toegewezen.



## <a name="what-is-required-to-create-an-application-gateway"></a>Wat is vereist voor het maken van een toepassingsgateway?


- **Back-end server-groep:** De lijst met IP-adressen van de back-end servers. De IP-adressen ofwel moeten behoren tot het virtuele netwerk subnet of moeten een openbaar IP/VIP.
- **Instellingen voor back-end server toepassingen:** Elke groep heeft als poort, protocol en affiniteit op basis van een cookie-instellingen. Deze instellingen zijn gekoppeld aan een groep en worden toegepast op alle servers in de groep.
- **Front-poort:** Dit wordt de openbare poort op de toepassingsgateway wordt geopend. Verkeer treft deze poort en wordt vervolgens omgeleid naar een van de back-end servers.
- **Listener:** De listener is een front-end-poort, een protocol (Http of Https, dit zijn hoofdlettergevoelig), en de SSL-certificaat (als het configureren van SSL-offload).
- **Regel:** De regel wordt de listener van de groep back-end-server gebonden en definieert welke het verkeer moet worden besteed aan een bepaalde listener wanneer deze groep met back-end-server.

## <a name="create-an-application-gateway"></a>Een toepassingsgateway maken

Het verschil tussen het gebruik van klassieke Azure en Azure Resource Manager is de volgorde waarin u de toepassingsgateway en de artikelen die moeten worden geconfigureerd.

Met Resource Manager alle items in een toepassingsgateway worden afzonderlijk geconfigureerd en deze vervolgens samen met de toepassingsbron gateway maken.


Hier zijn de stappen die nodig zijn voor het maken van een toepassingsgateway:

1. Een resourcegroep maken voor Resource Manager.
2. Een virtueel netwerk, subnet, en openbare IP-voor de toepassingsgateway maken.
3. Het object application gateway configuratie gemaakt.
4. Maak een toepassingsbron gateway.

## <a name="create-a-resource-group-for-resource-manager"></a>Een resourcegroep maken voor bronbeheer

Let erop dat u de nieuwste versie van Azure PowerShell. Meer info vindt u op [Met behulp van Windows PowerShell met Resource Manager](../powershell-azure-resource-manager.md).

### <a name="step-1"></a>Stap 1

Log in op Azure

    Login-AzureRmAccount

U wordt gevraagd te verifiëren met uw referenties.<BR>

### <a name="step-2"></a>Stap 2

Controleer de abonnementen voor de account.

    Get-AzureRmSubscription

### <a name="step-3"></a>Stap 3

Kies welke van uw Azure-abonnementen te gebruiken. <BR>


    Select-AzureRmSubscription -Subscriptionid "GUID of subscription"

### <a name="step-4"></a>Stap 4

Een resourcegroep (overslaan deze stap als u een bestaande bronnengroep) maken.

    New-AzureRmResourceGroup -Name appgw-RG -Location "West US"

U kunt ook ook labels voor een bronnengroep voor toepassingsgateway maken:
    
    $resourceGroup = New-AzureRmResourceGroup -Name appgw-RG -Location "West US" -Tags @{Name = "testtag"; Value = "Application Gateway URL routing"} 

Azure Resource Manager vereist dat alle bronnengroepen van een locatie opgeven. Dit wordt gebruikt als de standaardlocatie voor resources in die groep. Zorg ervoor dat alle opdrachten voor het maken van een toepassingsgateway dezelfde resourcegroep gebruiken.

In het bovenstaande voorbeeld wij een resourcegroep "appgw RG" en de locatie 'West ons' genoemd.

>[AZURE.NOTE] Als u een aangepaste sonde voor uw toepassingsgateway configureren, Zie [maken een toepassingsgateway met aangepaste sondes met PowerShell](application-gateway-create-probe-ps.md). Bekijk de [aangepaste sondes en de statuscontrole](application-gateway-probe-overview.md) voor meer informatie.

## <a name="create-a-virtual-network-and-a-subnet-for-the-application-gateway"></a>Een virtueel netwerk en subnet voor de toepassingsgateway maken

In het volgende voorbeeld ziet u hoe een virtueel netwerk maken met behulp van bronbeheer.

### <a name="step-1"></a>Stap 1

Het adres bereik 10.0.0.0/24 toewijzen aan de variabele subnet moet worden gebruikt voor het maken van een virtueel netwerk.

    $subnet = New-AzureRmVirtualNetworkSubnetConfig -Name subnet01 -AddressPrefix 10.0.0.0/24


### <a name="step-2"></a>Stap 2

Maak een virtueel netwerk met de naam 'appgwvnet' in de resource-groep 'appgw-rg' voor de regio West VS is met het voorvoegsel 10.0.0.0/16 met subnetmasker 10.0.0.0/24.

    $vnet = New-AzureRmVirtualNetwork -Name appgwvnet -ResourceGroupName appgw-RG -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $subnet


### <a name="step-3"></a>Stap 3

Een subnet-variabele voor de volgende stappen die een toepassingsgateway maakt toewijzen.

    $subnet=$vnet.Subnets[0]

## <a name="create-a-public-ip-address-for-the-front-end-configuration"></a>Maak een openbaar IP-adres voor de front-configuratie

Maak een openbaar IP-bron "publicIP01" in de resource-groep 'appgw-rg' voor de regio West VS.

    $publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-RG -name publicIP01 -location "West US" -AllocationMethod Dynamic

Een IP-adres is toegewezen aan de toepassingsgateway wanneer de service wordt gestart.

## <a name="create-application-gateway-configuration"></a>Toepassingsconfiguratie-gateway maken

Alle configuratie-items moeten worden ingesteld voordat u de toepassingsgateway maakt. De volgende stappen maakt de configuratie-items die nodig zijn voor een gateway-toepassingsbron.

### <a name="step-1"></a>Stap 1

Een toepassing IP-configuratie van de standaardgateway met de naam 'gatewayIP01' maken. Als Gateway-toepassing wordt gestart, neemt een IP-adres van het subnet is geconfigureerd en netwerkverkeer doorsturen naar de IP-adressen in de back-end-IP-adresgroep. Houd er rekening mee dat elk exemplaar één IP-adres wordt.


    $gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $subnet


### <a name="step-2"></a>Stap 2

De back-end-IP-adresgroep met de naam 'pool01' en 'pool2' met IP-adressen configureren "134.170.185.46, 134.170.188.221,134.170.185.50" voor "pool1" en "134.170.186.46, 134.170.189.221,134.170.186.50" voor "pool2".


    $pool1 = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 134.170.185.46, 134.170.188.221,134.170.185.50

    $pool2 = New-AzureRmApplicationGatewayBackendAddressPool -Name pool02 -BackendIPAddresses 134.170.186.46, 134.170.189.221,134.170.186.50

In dit voorbeeld zijn er twee back-end-toepassingen voor het routeren van netwerkverkeer op basis van het URL-pad. Een groep van toepassingen ontvangt verkeer van URL-pad '/ video' en verkeer wordt ontvangen van het pad "/ image" van andere toepassingen. Vervangen door het vorige IP-adressen als u wilt uw eigen eindpunten toepassing IP-adres toevoegen. 

### <a name="step-3"></a>Stap 3

Application gateway instelling 'poolsetting01' en 'poolsetting02' voor het netwerkverkeer verdeeld in de back-end-toepassingen configureren. In dit voorbeeld kunt u instellingen voor verschillende back-end-toepassingen voor de back-end-toepassingen configureren. Elke back-end-toepassingen hebben een eigen back-end-toepassingen instellen.

    $poolSetting01 = New-AzureRmApplicationGatewayBackendHttpSettings -Name "besetting01" -Port 80 -Protocol Http -CookieBasedAffinity Disabled -RequestTimeout 120

    $poolSetting02 = New-AzureRmApplicationGatewayBackendHttpSettings -Name "besetting02" -Port 80 -Protocol Http -CookieBasedAffinity Enabled -RequestTimeout 240

### <a name="step-4"></a>Stap 4

De front-IP configureren met het openbare IP-eindpunt.

    $fipconfig01 = New-AzureRmApplicationGatewayFrontendIPConfig -Name "frontend1" -PublicIPAddress $publicip

### <a name="step-5"></a>Stap 5 

De front-poort voor een application-gateway configureren.

    $fp01 = New-AzureRmApplicationGatewayFrontendPort -Name "fep01" -Port 80
### <a name="step-6"></a>Stap 6

Configureer de listener. In deze stap configureert u de listener voor het openbare IP-adres en de poort die wordt gebruikt voor het ontvangen van binnenkomend netwerkverkeer. 
 
    $listener = New-AzureRmApplicationGatewayHttpListener -Name "listener01" -Protocol Http -FrontendIPConfiguration $fipconfig01 -FrontendPort $fp01

### <a name="step-7"></a>Stap 7 

URL-paden regel voor de back-end-toepassingen configureren. In deze stap configureert u het relatieve pad door toepassingsgateway gebruikt voor de toewijzing van URL-pad welke back-end-groep is toegewezen voor het verwerken van binnenkomend verkeer te definiëren.

In het volgende voorbeeld maakt u twee regels: één voor '/ afbeelding /' path routing verkeer naar back-end 'pool1' en een "/ video /" pad routeren van verkeer naar de back-end 'pool2'.
    
    $imagePathRule = New-AzureRmApplicationGatewayPathRuleConfig -Name "pathrule1" -Paths "/image/*" -BackendAddressPool $pool1 -BackendHttpSettings $poolSetting01

    $videoPathRule = New-AzureRmApplicationGatewayPathRuleConfig -Name "pathrule2" -Paths "/video/*" -BackendAddressPool $pool2 -BackendHttpSettings $poolSetting02

De regel pad map-configuratie ook een standaard back-end-adresgroep wordt geconfigureerd als het pad niet overeenkomt met een van de vooraf gedefinieerde padregels. 

    $urlPathMap = New-AzureRmApplicationGatewayUrlPathMapConfig -Name "urlpathmap" -PathRules $videoPathRule, $imagePathRule -DefaultBackendAddressPool $pool1 -DefaultBackendHttpSettings $poolSetting02

### <a name="step-8"></a>Stap 8

De instelling van een regel maken. In deze stap configureert u de toepassingsgateway voor het gebruik van URL-pad gebaseerde routering.

    $rule01 = New-AzureRmApplicationGatewayRequestRoutingRule -Name "rule1" -RuleType PathBasedRouting -HttpListener $listener -UrlPathMap $urlPathMap

### <a name="step-9"></a>Stap 9

Het aantal exemplaren en de grootte voor de toepassingsgateway configureren.

    $sku = New-AzureRmApplicationGatewaySku -Name "Standard_Small" -Tier Standard -Capacity 2

## <a name="create-application-gateway"></a>Application Gateway maken

Een toepassingsgateway maken met alle configuratieobjecten van de voorgaande stappen.

    $appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-RG -Location "West US" -BackendAddressPools $pool1,$pool2 -BackendHttpSettingsCollection $poolSetting01, $poolSetting02 -FrontendIpConfigurations $fipconfig01 -GatewayIpConfigurations $gipconfig -FrontendPorts $fp01 -HttpListeners $listener -UrlPathMaps $urlPathMap -RequestRoutingRules $rule01 -Sku $sku

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

Als u leren van Secure Sockets Layer (SSL)-offload wilt, Zie [configureren een toepassingsgateway voor SSL-offload](application-gateway-ssl-arm.md).