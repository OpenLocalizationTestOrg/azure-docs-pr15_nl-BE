<properties
   pageTitle="Maak een aangepaste sonde met PowerShell in Resource Manager voor Application Gateway | Microsoft Azure"
   description="Informatie over het maken van een aangepaste sonde voor Application Gateway met PowerShell in Resource Manager"
   services="application-gateway"
   documentationCenter="na"
   authors="georgewallace"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags  
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/06/2016"
   ms.author="gwallace" />

# <a name="create-a-custom-probe-for-azure-application-gateway-by-using-powershell-for-azure-resource-manager"></a>Maak een aangepaste sonde met PowerShell voor Azure Resource Manager voor Azure Application Gateway

> [AZURE.SELECTOR]
- [Azure portal](application-gateway-create-probe-portal.md)
- [Azure PowerShell voor bronbeheer](application-gateway-create-probe-ps.md)
- [Azure klassieke PowerShell](application-gateway-create-probe-classic-ps.md)

[AZURE.INCLUDE [azure-probe-intro-include](../../includes/application-gateway-create-probe-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)][klassieke implementatiemodel](application-gateway-create-probe-classic-ps.md).

[AZURE.INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

### <a name="step-1"></a>Stap 1

Login-AzureRmAccount gebruiken om te verifiëren.

    Login-AzureRmAccount

### <a name="step-2"></a>Stap 2

Controleer de abonnementen voor de account.

    Get-AzureRmSubscription

### <a name="step-3"></a>Stap 3

Kies welke van uw Azure-abonnementen te gebruiken. <BR>


    Select-AzureRmSubscription -Subscriptionid "GUID of subscription"


### <a name="step-4"></a>Stap 4

Een resourcegroep (overslaan deze stap als u een bestaande bronnengroep) maken.

    New-AzureRmResourceGroup -Name appgw-rg -Location "West US"

Azure Resource Manager vereist dat alle bronnengroepen van een locatie opgeven. Deze locatie wordt gebruikt als de standaardlocatie voor resources in die groep. Zorg ervoor dat alle opdrachten voor het maken van een toepassingsgateway dezelfde resourcegroep gebruiken.

In het bovenstaande voorbeeld wij een resourcegroep "appgw RG" en de locatie 'West ons' genoemd.

## <a name="create-a-virtual-network-and-a-subnet-for-the-application-gateway"></a>Een virtueel netwerk en subnet voor de toepassingsgateway maken

De volgende stappen maakt een virtueel netwerk en een subnetmasker voor de toepassingsgateway.

### <a name="step-1"></a>Stap 1


Het adres bereik 10.0.0.0/24 toewijzen aan een variabele subnet moet worden gebruikt voor het maken van een virtueel netwerk.

    $subnet = New-AzureRmVirtualNetworkSubnetConfig -Name subnet01 -AddressPrefix 10.0.0.0/24

### <a name="step-2"></a>Stap 2

Maak een virtueel netwerk met de naam 'appgwvnet' in de resource-groep 'appgw-rg' voor de regio West VS is met het voorvoegsel 10.0.0.0/16 met subnetmasker 10.0.0.0/24.

    $vnet = New-AzureRmVirtualNetwork -Name appgwvnet -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $subnet


### <a name="step-3"></a>Stap 3

Een variabele van het subnet voor de volgende stappen die een toepassingsgateway maken toewijzen.

    $subnet = $vnet.Subnets[0]

## <a name="create-a-public-ip-address-for-the-front-end-configuration"></a>Maak een openbaar IP-adres voor de front-configuratie


Maak een openbaar IP-bron "publicIP01" in de resource-groep 'appgw-rg' voor de regio West VS.

    $publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-rg -Name publicIP01 -Location "West US" -AllocationMethod Dynamic


## <a name="create-an-application-gateway-configuration-object-with-a-custom-probe"></a>Een toepassingsobject gateway-configuratie maken met een aangepaste sonde

U instellen alle configuratie-items voordat u de toepassingsgateway maakt. De volgende stappen maakt de configuratie-items die nodig zijn voor een gateway-toepassingsbron.

### <a name="step-1"></a>Stap 1

Een toepassing IP-configuratie van de standaardgateway met de naam 'gatewayIP01' maken. Als Gateway-toepassing wordt gestart, neemt een IP-adres van het subnet is geconfigureerd en netwerkverkeer doorsturen naar de IP-adressen in de back-end-IP-adresgroep. Houd er rekening mee dat elk exemplaar één IP-adres wordt.

    $gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $subnet


### <a name="step-2"></a>Stap 2


De back-end-IP-adresgroep met de naam 'pool01' met IP-adressen configureren "134.170.185.46, 134.170.188.221,134.170.185.50". Deze waarden zijn de IP-adressen die het netwerkverkeer dat afkomstig van de front-IP-eindpunt is worden weergegeven. U vervangt de hierboven om het toevoegen van uw eigen eindpunten toepassing IP-adres IP-adressen.

    $pool = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 134.170.185.46, 134.170.188.221,134.170.185.50


### <a name="step-3"></a>Stap 3


De aangepaste sonde is in deze stap geconfigureerd.

De parameters zijn:

- **Interval** - configureert de controles sonde interval in seconden.
- **Timeout** - definieert de sonde time-out voor een HTTP-antwoord uit.
- **-Hostnaam en pad** - volledige URL-pad dat wordt aangeroepen door de Application Gateway om na te gaan van de gezondheid van de instantie. Bijvoorbeeld als er een http://contoso.com/ website, worden kan de aangepaste sonde geconfigureerd voor 'http://contoso.com/path/custompath.htm' voor de sonde controles hebben een geslaagde HTTP-antwoord.
- **UnhealthyThreshold** - het aantal mislukte HTTP-antwoorden die nodig zijn voor het markeren van de back-end-instantie als *beschadigd*.

<BR>

    $probe = New-AzureRmApplicationGatewayProbeConfig -Name probe01 -Protocol Http -HostName "contoso.com" -Path "/path/path.htm" -Interval 30 -Timeout 120 -UnhealthyThreshold 8

### <a name="step-4"></a>Stap 4

Application gateway instelling "poolsetting01" voor het verkeer in de back-end-toepassingen configureren. Deze stap heeft ook een configuratie met time-outwaarde voor de groep met back-end antwoord op een aanvraag voor gateway. Wanneer u een back-end antwoord raakt een time-out optreedt, annuleert toepassingsgateway verzoek. Deze waarde wijkt af van een time-out van de sonde die uitsluitend bestemd is voor de back-end-reactie op controles probe.

    $poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name poolsetting01 -Port 80 -Protocol Http -CookieBasedAffinity Disabled -Probe $probe -RequestTimeout 80

### <a name="step-5"></a>Stap 5

Configureer de front-end-IP-poort met de naam 'frontendport01' voor het openbare IP-eindpunt.

    $fp = New-AzureRmApplicationGatewayFrontendPort -Name frontendport01 -Port 80

### <a name="step-6"></a>Stap 6

Maken van de front-IP-configuratie met de naam 'fipconfig01' en het openbare IP-adres koppelen aan de front-end-IP-configuratie.


    $fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name fipconfig01 -PublicIPAddress $publicip

### <a name="step-7"></a>Stap 7

Maak de listener naam 'listener01' en koppel de front-poort op de front-end-IP-configuratie.

    $listener = New-AzureRmApplicationGatewayHttpListener -Name listener01  -Protocol Http -FrontendIPConfiguration $fipconfig -FrontendPort $fp

### <a name="step-8"></a>Stap 8

De load balancer regel routering met de naam 'rule01', waarmee de load balancer gedrag maken.

    $rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name rule01 -RuleType Basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool

### <a name="step-9"></a>Stap 9

De grootte van het exemplaar van de application-gateway configureren.

    $sku = New-AzureRmApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2


>[AZURE.NOTE]  De standaardwaarde voor *InstanceCount* is 2, met een maximale waarde van 10. De standaardwaarde voor *GatewaySize* is normaal. U kunt kiezen tussen Standard_Small, Standard_Medium en Standard_Large.

## <a name="create-an-application-gateway-by-using-new-azurermapplicationgateway"></a>Een toepassingsgateway maken met nieuwe AzureRmApplicationGateway

Maak een toepassingsgateway met alle configuratie-items uit de bovenstaande stappen. In dit voorbeeld wordt de toepassingsgateway 'appgwtest' genoemd.

    $appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Location "West US" -BackendAddressPools $pool -Probes $probe -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku

## <a name="add-a-probe-to-an-existing-application-gateway"></a>Een sonde toevoegen aan een bestaande toepassingsgateway

U hebt een aangepaste sonde toevoegen aan een bestaande toepassingsgateway in vier stappen.

### <a name="step-1"></a>Stap 1

De toepassingsbron gateway in een variabele PowerShell laden via de **Get-AzureRmApplicationGateway**.

    $getgw =  Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg

### <a name="step-2"></a>Stap 2

Een sonde toevoegen aan de bestaande configuratie van de standaardgateway.

    $getgw = Add-AzureRmApplicationGatewayProbeConfig -ApplicationGateway $getgw -Name probe01 -Protocol Http -HostName "contoso.com" -Path "/path/custompath.htm" -Interval 30 -Timeout 120 -UnhealthyThreshold 8


In het voorbeeld wordt is de aangepaste sonde geconfigureerd om te controleren of URL-pad contoso.com/path/custompath.htm elke 30 seconden. Een drempel van 120 seconden voor time-out is geconfigureerd met een maximum aantal 8 sonde mislukte aanvragen.

### <a name="step-3"></a>Stap 3

Met de sonde toevoegen aan de configuratie van backend-instelling en time- **-Set-AzureRmApplicationGatewayBackendHttpSettings**.


     $getgw = Set-AzureRmApplicationGatewayBackendHttpSettings -ApplicationGateway $getgw -Name $getgw.BackendHttpSettingsCollection.name -Port 80 -Protocol Http -CookieBasedAffinity Disabled -Probe $probe -RequestTimeout 120

### <a name="step-4"></a>Stap 4

Sla de configuratie op de toepassingsgateway met behulp van **Set AzureRmApplicationGateway**.

    Set-AzureRmApplicationGateway -ApplicationGateway $getgw

## <a name="remove-a-probe-from-an-existing-application-gateway"></a>Een sonde van de toepassingsgateway van een bestaande verwijderen

Hier vindt u de stappen voor het verwijderen van een aangepaste sonde uit een bestaande toepassingsgateway.

### <a name="step-1"></a>Stap 1

De toepassingsbron gateway in een variabele PowerShell laden via de **Get-AzureRmApplicationGateway**.

    $getgw =  Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg


### <a name="step-2"></a>Stap 2

De sonde configuratie van de toepassingsgateway verwijderen met **Software AzureRmApplicationGatewayProbeConfig**.

    $getgw = Remove-AzureRmApplicationGatewayProbeConfig -ApplicationGateway $getgw -Name $getgw.Probes.name

### <a name="step-3"></a>Stap 3

De back-end-toepassingen voor het verwijderen van de sonde en time-outinstelling bijwerken met behulp van **-Set-AzureRmApplicationGatewayBackendHttpSettings**.


     $getgw = Set-AzureRmApplicationGatewayBackendHttpSettings -ApplicationGateway $getgw -Name $getgw.BackendHttpSettingsCollection.name -Port 80 -Protocol http -CookieBasedAffinity Disabled

### <a name="step-4"></a>Stap 4

Sla de configuratie op de toepassingsgateway met behulp van **Set AzureRmApplicationGateway**. 

    Set-AzureRmApplicationGateway -ApplicationGateway $getgw

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

Informatie over het configureren van SSL-offloading via [SSL-Offload configureren](application-gateway-ssl-arm.md)

