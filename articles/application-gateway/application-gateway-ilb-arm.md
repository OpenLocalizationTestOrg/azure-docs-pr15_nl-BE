<properties
   pageTitle="Maken en configureren van een toepassingsgateway met interne taakverdeling (ILB) met behulp van bronbeheer Azure | Microsoft Azure"
   description="Deze pagina bevat instructies voor het maken, configureren, starten en een Azure toepassingsgateway met interne taakverdeling (ILB) voor Azure Resource Manager verwijderen"
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
   ms.date="08/19/2016"
   ms.author="gwallace"/>


# <a name="create-an-application-gateway-with-an-internal-load-balancer-ilb-by-using-azure-resource-manager"></a>Een toepassingsgateway met interne taakverdeling (ILB) maken met behulp van bronbeheer Azure

> [AZURE.SELECTOR]
- [Azure klassieke werk](application-gateway-ilb.md)
- [Resource Manager PowerShell stappen](application-gateway-ilb-arm.md)

Azure toepassingsgateway kan worden geconfigureerd met een VIP verbonden met Internet of met een interne eindpunt dat niet is blootgesteld aan Internet, ook wel bekend als een interne load balancer (ILB) eindpunt. De gateway configureren met een ILB is nuttig voor interne line-of-business-toepassingen die niet zijn blootgesteld aan Internet. Het is ook handig voor services en lagen van een toepassing met meerdere niveaus die zich bevinden in een beveiligingsgrens die niet wordt blootgesteld aan Internet maar moeten nog round-robin distributie, sessie kleverigheid of beëindiging van Secure Sockets Layer (SSL) laden.

Dit artikel begeleidt u door de stappen voor het configureren van een toepassingsgateway met een ILB.

## <a name="before-you-begin"></a>Voordat u begint

1. Installeer de nieuwste versie van de Azure PowerShell-cmdlets met behulp van de Web Platform Installer. U kunt downloaden en installeren van de meest recente versie van de **Windows PowerShell** -sectie van de [pagina met Downloads](https://azure.microsoft.com/downloads/).
2. U maken een virtueel netwerk en een subnet voor Application Gateway. Controleer of geen virtuele machines of cloud implementaties het subnet worden gebruikt. Toepassingsgateway moet zelf in een virtueel netwerk subnet.
3. De servers die u configureert voor het gebruik van de toepassingsgateway moeten bestaan of de eindpunten ervan gemaakt in het virtuele netwerk of met een openbaar IP-/ VIP hebt toegewezen.

## <a name="what-is-required-to-create-an-application-gateway"></a>Wat is vereist voor het maken van een toepassingsgateway?


- **Back-end server-groep:** De lijst met IP-adressen van de back-end servers. Het weergegeven IP-adressen ofwel moeten behoren tot het virtuele netwerk, maar in een ander subnet voor de toepassingsgateway of een openbaar IP/VIP moeten worden.
- **Instellingen voor back-end server toepassingen:** Elke groep heeft als poort, protocol en affiniteit op basis van een cookie-instellingen. Deze instellingen zijn gekoppeld aan een groep en worden toegepast op alle servers in de groep.
- **Front-poort:** Dit wordt de openbare poort op de toepassingsgateway wordt geopend. Verkeer treft deze poort en wordt vervolgens omgeleid naar een van de back-end servers.
- **Listener:** De listener is een front-end-poort, een protocol (Http of Https, dit zijn hoofdlettergevoelig), en de SSL-certificaat (als het configureren van SSL-offload).
- **Regel:** De regel wordt de listener en de groep met back-end-server en wordt gedefinieerd welke het verkeer moet worden besteed aan een bepaalde listener wanneer deze groep met back-end-server. Op dit moment wordt *alleen de basisregel* ondersteund. *De basisregel* is de verdeling van de belasting van round-robin.



## <a name="create-an-application-gateway"></a>Een toepassingsgateway maken

Het verschil tussen het gebruik van klassieke Azure en Azure Resource Manager is de volgorde waarin u de toepassingsgateway en de artikelen die moeten worden geconfigureerd.
Met Resource Manager alle items in een toepassingsgateway wordt afzonderlijk geconfigureerd en deze vervolgens samen met de toepassingsbron gateway maken.


Hier zijn de stappen die nodig zijn voor het maken van een toepassingsgateway:

1. Een resourcegroep maken voor bronbeheer
2. Een virtueel netwerk en subnet voor de toepassingsgateway maken
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

Maak een nieuwe resourcegroep (overslaan deze stap als u een bestaande resourcegroep).

    New-AzureRmResourceGroup -Name appgw-rg -location "West US"

Azure Resource Manager vereist dat alle bronnengroepen van een locatie opgeven. Dit wordt gebruikt als de standaardlocatie voor resources in die groep. Zorg ervoor dat alle opdrachten voor het maken van een application-gateway maakt gebruik van dezelfde resourcegroep.

In het bovenstaande voorbeeld wij een resourcegroep "appgw rg" en de locatie 'West ons' genoemd.

## <a name="create-a-virtual-network-and-a-subnet-for-the-application-gateway"></a>Een virtueel netwerk en subnet voor de toepassingsgateway maken

In het volgende voorbeeld ziet u hoe een virtueel netwerk maken met behulp van Resource Manager:

### <a name="step-1"></a>Stap 1

    $subnetconfig = New-AzureRmVirtualNetworkSubnetConfig -Name subnet01 -AddressPrefix 10.0.0.0/24

Dit het adres bereik 10.0.0.0/24 toegewezen aan een variabele subnet moet worden gebruikt voor het maken van een virtueel netwerk.

### <a name="step-2"></a>Stap 2

    $vnet = New-AzureRmVirtualNetwork -Name appgwvnet -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $subnetconfig

Hiermee maakt u een virtueel netwerk met de naam 'appgwvnet' in de resource-groep 'appgw-rg' voor de regio West VS is met het voorvoegsel 10.0.0.0/16 met subnetmasker 10.0.0.0/24.

### <a name="step-3"></a>Stap 3

    $subnet = $vnet.subnets[0]

Dit wordt het subnetobject toegewezen aan de variabele $subnet voor de volgende stappen.

## <a name="create-an-application-gateway-configuration-object"></a>Maakt een object application gateway configureren

### <a name="step-1"></a>Stap 1

    $gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $subnet

Hiermee maakt u een toepassing IP-configuratie van de standaardgateway met de naam 'gatewayIP01'. Als Gateway-toepassing wordt gestart, neemt een IP-adres van het subnet is geconfigureerd en netwerkverkeer doorsturen naar de IP-adressen in de back-end-IP-adresgroep. Houd er rekening mee dat elk exemplaar één IP-adres wordt.


### <a name="step-2"></a>Stap 2

    $pool = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 134.170.185.46, 134.170.188.221,134.170.185.50

Hiermee configureert u de back-end-IP-adresgroep, 'pool01' met IP-adressen met de naam "134.170.185.46, 134.170.188.221,134.170.185.50". Dit zijn de IP-adressen die het netwerkverkeer dat afkomstig van de front-IP-eindpunt is worden weergegeven. U vervangt de hierboven om het toevoegen van uw eigen eindpunten toepassing IP-adres IP-adressen.

### <a name="step-3"></a>Stap 3

    $poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name poolsetting01 -Port 80 -Protocol Http -CookieBasedAffinity Disabled

Hiermee wordt verkeer application gateway instelling 'poolsetting01' voor de belasting in evenwicht in de back-end-toepassingen.

### <a name="step-4"></a>Stap 4

    $fp = New-AzureRmApplicationGatewayFrontendPort -Name frontendport01  -Port 80

Hiermee configureert u de front-IP-poort met de naam 'frontendport01' van het ILB.

### <a name="step-5"></a>Stap 5

    $fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name fipconfig01 -Subnet $subnet

Dit maakt de front-IP-configuratie 'fipconfig01' genoemd en koppelt u deze aan een particulier IP-adres van de huidige virtuele netwerk subnet.

### <a name="step-6"></a>Stap 6

    $listener = New-AzureRmApplicationGatewayHttpListener -Name listener01  -Protocol Http -FrontendIPConfiguration $fipconfig -FrontendPort $fp

Dit maakt de listener 'listener01' genoemd en de IP-configuratie van de front-front-poort wordt gekoppeld.

### <a name="step-7"></a>Stap 7

    $rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name rule01 -RuleType Basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool

Hiermee maakt u de load balancer regel routering genaamd 'rule01', waarmee het gedrag van load balancer wordt geconfigureerd.

### <a name="step-8"></a>Stap 8

    $sku = New-AzureRmApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2

Hiermee wordt de grootte van het exemplaar van de application gateway.

>[AZURE.NOTE]  De standaardwaarde voor *InstanceCount* is 2, met een maximale waarde van 10. De standaardwaarde voor *GatewaySize* is normaal. U kunt kiezen tussen Standard_Small, Standard_Medium en Standard_Large.

## <a name="create-an-application-gateway-by-using-new-azureapplicationgateway"></a>Een toepassingsgateway maken met nieuwe AzureApplicationGateway

Hiermee maakt een toepassingsgateway met alle configuratie-items uit de bovenstaande stappen. In dit voorbeeld wordt de toepassingsgateway 'appgwtest' genoemd.


    $appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku

Hiermee maakt u een toepassingsgateway met alle configuratie-items uit de bovenstaande stappen. In het voorbeeld wordt de toepassingsgateway 'appgwtest' genoemd.


## <a name="delete-an-application-gateway"></a>Een toepassingsgateway verwijderen

U verwijdert een toepassingsgateway, moet u de volgende handelingen in volgorde:

1. Gebruik de cmdlet **Stop AzureRmApplicationGateway** om te stoppen met de gateway.
2. Gebruik de cmdlet **Verwijderen AzureRmApplicationGateway** voor het verwijderen van de gateway.
3. Controleer of de gateway is verwijderd met behulp van de cmdlet **Get-AzureApplicationGateway** .


### <a name="step-1"></a>Stap 1

Het object application gateway ophalen en deze te koppelen aan een variabele "$getgw".

    $getgw =  Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg

### <a name="step-2"></a>Stap 2

**Stop AzureRmApplicationGateway** de toepassingsgateway niet meer gebruiken. Dit voorbeeld ziet u de cmdlet **Stop AzureRmApplicationGateway** op de eerste regel, gevolgd door de uitvoer.

    PS C:\> Stop-AzureRmApplicationGateway -ApplicationGateway $getgw  

    VERBOSE: 9:49:34 PM - Begin Operation: Stop-AzureApplicationGateway
    VERBOSE: 10:10:06 PM - Completed Operation: Stop-AzureApplicationGateway
    Name       HTTP Status Code     Operation ID                             Error
    ----       ----------------     ------------                             ----
    Successful OK                   ce6c6c95-77b4-2118-9d65-e29defadffb8

Zodra de toepassingsgateway een gestopt is, gebruikt u de cmdlet **Verwijderen AzureRmApplicationGateway** om de service te verwijderen.


    PS C:\> Remove-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Force

    VERBOSE: 10:49:34 PM - Begin Operation: Remove-AzureApplicationGateway
    VERBOSE: 10:50:36 PM - Completed Operation: Remove-AzureApplicationGateway
    Name       HTTP Status Code     Operation ID                             Error
    ----       ----------------     ------------                             ----
    Successful OK                   055f3a96-8681-2094-a304-8d9a11ad8301

>[AZURE.NOTE] De **-force** schakeloptie onderdrukt u het bevestigingsbericht voor verwijderen kan worden gebruikt.


Als u wilt controleren of de service is verwijderd, kunt u de cmdlet **Get-AzureRmApplicationGateway** . Deze stap is niet vereist.


    PS C:\>Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg

    VERBOSE: 10:52:46 PM - Begin Operation: Get-AzureApplicationGateway

    Get-AzureApplicationGateway : ResourceNotFound: The gateway does not exist.
    .....

## <a name="next-steps"></a>Volgende stappen

Als u wilt voor het configureren van SSL-offload, Zie [configureren een toepassingsgateway voor SSL-offload](application-gateway-ssl.md).

Als u configureren een toepassingsgateway wilt gebruiken met een ILB, Zie [maken een toepassingsgateway met interne taakverdeling (ILB)](application-gateway-ilb.md).

Als u meer informatie over het algemeen balancing opties laden, Zie:

- [Azure Load Balancer](https://azure.microsoft.com/documentation/services/load-balancer/)
- [Azure verkeer Manager](https://azure.microsoft.com/documentation/services/traffic-manager/)
