<properties
    pageTitle="SSL-beleid en complete SSL configureren met Application Gateway | Microsoft Azure"
    description="In dit artikel wordt beschreven hoe u complete SSL configureren met toepassingsgateway met Azure Resource Manager PowerShell"
    services="application-gateway"
    documentationCenter="na"
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

# <a name="configure-ssl-policy-and-end-to-end-ssl-with-application-gateway-using-powershell"></a>SSL-beleid en complete SSL configureren met toepassingsgateway met PowerShell

## <a name="overview"></a>Overzicht

Toepassingsgateway ondersteunt codering van begin tot eind van het verkeer. Toepassingsgateway doet dit door de SSL-verbinding op de toepassingsgateway wordt beëindigd. De gateway vervolgens regels voor de routering van toepassing is op het verkeer opnieuw decodeert het pakket en stuurt het pakket door naar de juiste back-end op basis van de regels voor routing gedefinieerd. Geen antwoord van de webserver wordt doorlopen hetzelfde proces aan de eindgebruiker.

Een andere functie die toepassingsgateway ondersteunt bepaalde versies van SSL-protocol is uitgeschakeld. Toepassingsgateway ondersteunt het uitschakelen van de versie van het volgende protocol; TLSv1.0, TLSv1.1 en TLSv1.2.

> [AZURE.NOTE] SSL 2.0 en SSL 3.0 zijn standaard uitgeschakeld en kan niet worden ingeschakeld. Ze worden beschouwd als niet beveiligd en kunnen niet worden gebruikt met een Application Gateway

![afbeelding met scenario][scenario]

## <a name="scenario"></a>Scenario

In dit scenario wordt informatie over het maken van een toepassingsgateway via SSL van begin tot eind met PowerShell.

In dit scenario wordt:

- Maak een groep met de naam 'appgw '-rg
- Maak een virtueel netwerk met de naam 'appgwvnet' met een gereserveerd blok CIDR van 10.0.0.0/16.
- Maak twee subnetten 'appgwsubnet' en 'appsubnet' genoemd.
- Maak een kleine toepassingsgateway ondersteuning voor SSL-codering voor complete die bepaalde SSL-protocollen uitgeschakeld.

## <a name="before-you-begin"></a>Voordat u begint

Complete SSL configureren met een toepassingsgateway, een certificaat vereist is voor de gateway en certificaten zijn vereist voor de back-end servers. Het gatewaycertificaat wordt gebruikt voor het coderen en decoderen van het verkeer dat wordt verzonden via SSL. Het gatewaycertificaat moet zich in de indeling Personal Information Exchange (pfx). Deze bestandsindeling kunt voor de persoonlijke sleutel te exporteren die door de toepassingsgateway is vereist voor het uitvoeren van de codering en decodering van verkeer.

Voor complete ssl-codering moet de back-end whitelisted met application gateway. Dit wordt gedaan door het openbare certificaat van de backends uploaden naar de toepassingsgateway. Dit zorgt ervoor dat de toepassingsgateway alleen met bekende backend exemplaren communiceert. Dit verder beveiligt de end-to-end-communicatie.

Dit proces wordt beschreven in de volgende stappen uit:

## <a name="create-the-resource-group"></a>De resourcegroep maken

Deze sectie helpt u bij het maken van een resourcegroep, waarin de toepassingsgateway.

### <a name="step-1"></a>Stap 1

Log in op uw Account Azure.

    Login-AzureRmAccount

### <a name="step-2"></a>Stap 2

Selecteer het abonnement voor dit scenario.

    Select-AzureRmsubscription -SubscriptionName "<Subscription name>"

### <a name="step-3"></a>Stap 3

Een resourcegroep (overslaan deze stap als u een bestaande bronnengroep) maken.

    New-AzureRmResourceGroup -Name appgw-rg -Location "West US"

## <a name="create-a-virtual-network-and-a-subnet-for-the-application-gateway"></a>Een virtueel netwerk en subnet voor de toepassingsgateway maken

Het volgende voorbeeld wordt een virtueel netwerk en twee subnetten. Één subnet wordt gebruikt om de toepassingsgateway. Het andere subnet wordt gebruikt voor het hosten van de webtoepassing backends.

### <a name="step-1"></a>Stap 1

Een adresbereik toewijzen voor het subnet worden gebruikt voor de Gateway-toepassing zelf.

    $gwSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name 'appgwsubnet' -AddressPrefix 10.0.0.0/24

> [AZURE.NOTE] Subnetten voor application gateway geconfigureerd moeten correct worden verkleind. Een toepassingsgateway kan worden geconfigureerd voor maximaal 10 exemplaren. Elke instantie neemt 1 IP-adres van het subnet. Te klein van een subnet gevolgen kan hebben voor een toepassingsgateway schalen.

### <a name="step-2"></a>Stap 2

Een adresbereik moet worden gebruikt voor de back-end-adresgroep toewijzen.

    $nicSubnet = New-AzureRmVirtualNetworkSubnetConfig  -Name 'appsubnet' -AddressPrefix 10.0.2.0/24

### <a name="step-3"></a>Stap 3

Een virtueel netwerk maken met de subnetten die worden gedefinieerd in de voorgaande stappen.

    $vnet = New-AzureRmvirtualNetwork -Name 'appgwvnet' -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $gwSubnet, $nicSubnet

### <a name="step-4"></a>Stap 4

Ophalen in de resource virtueel netwerk en de bronnen van het subnet worden gebruikt in de volgende stappen uit:

    $vnet = Get-AzureRmvirtualNetwork -Name 'appgwvnet' -ResourceGroupName appgw-rg
    $gwSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'appgwsubnet' -VirtualNetwork $vnet
    $nicSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'appsubnet' -VirtualNetwork $vnet

## <a name="create-a-public-ip-address-for-the-front-end-configuration"></a>Maak een openbaar IP-adres voor de front-configuratie

Maak een openbare IP-bron voor de toepassingsgateway moet worden gebruikt. Deze openbare IP-adres wordt gebruikt een volgende stap.

    $publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-rg -Name 'publicIP01' -Location "West US" -AllocationMethod Dynamic

> [AZURE.IMPORTANT] Toepassingsgateway biedt geen ondersteuning voor het gebruik van een openbaar IP-adres dat is gemaakt met een domeinlabel gedefinieerd. Alleen een openbaar IP-adres met een domeinlabel dynamisch gemaakte wordt ondersteund. Als u een beschrijvende DNS-naam voor de toepassingsgateway nodig, verdient het gebruik van een CNAME-record als een alias.

## <a name="create-an-application-gateway-configuration-object"></a>Maakt een object application gateway configureren

U moet alle configuratie-items instellen voordat u de toepassingsgateway maakt. De volgende stappen maakt de configuratie-items die nodig zijn voor een gateway-toepassingsbron.

### <a name="step-1"></a>Stap 1

Een toepassing gateway IP-configuratie te maken, deze instelling configureert u welk subnet de toepassingsgateway wordt gebruikt. Als toepassingsgateway wordt gestart, neemt een IP-adres van het subnet is geconfigureerd en netwerkverkeer routeert naar de IP-adressen in de back-end-IP-adresgroep. Houd er rekening mee dat elk exemplaar één IP-adres wordt.

    $gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name 'gwconfig' -Subnet $gwSubnet

### <a name="step-2"></a>Stap 2

Een front-end-IP-adresconfiguratie te maken, deze instelling wordt een particuliere of openbare IP-adres toegewezen aan de front-end van de toepassingsgateway. Het openbare IP-adres in de vorige stap koppelt de volgende stap aan de front-end-IP-configuratie.

    $fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name 'fip01' -PublicIPAddress $publicip

### <a name="step-3"></a>Stap 3

De back-end-IP-adresgroep configureren met de IP-adressen van de backend-webservers. Deze IP-adressen zijn de IP-adressen die het netwerkverkeer dat afkomstig van de front-IP-eindpunt is worden weergegeven. U vervangt de volgende IP-adressen als u wilt uw eigen eindpunten toepassing IP-adres toevoegen.

    $pool = New-AzureRmApplicationGatewayBackendAddressPool -Name 'pool01' -BackendIPAddresses 1.1.1.1, 2.2.2.2, 3.3.3.3

> [AZURE.NOTE] Een volledig gekwalificeerde domeinnaam (FQDN) is ook een geldige waarde in plaats van een IP-adres voor de back-end servers met de schakeloptie - BackendFqdns.

### <a name="step-4"></a>Stap 4

Configureer de front-end-IP-poort voor het openbare IP-eindpunt. Deze poort is de poort waarmee eindgebruikers verbinding maken.

    $fp = New-AzureRmApplicationGatewayFrontendPort -Name 'port01'  -Port 443

### <a name="step-5"></a>Stap 5

Het certificaat voor de toepassingsgateway configureren. Dit certificaat wordt gebruikt voor het decoderen en het verkeer op de toepassingsgateway opnieuw te coderen.

    $cert = New-AzureRmApplicationGatewaySslCertificate -Name cert01 -CertificateFile <full path to .pfx file> -Password <password for certificate file>

> [AZURE.NOTE] In dit voorbeeld wordt het certificaat voor SSL-verbinding geconfigureerd. Het certificaat moet in de PFX-indeling en het wachtwoord moet tussen de 4 tot 12 tekens.

### <a name="step-6"></a>Stap 6

De HTTP-listener voor de toepassingsgateway maken. Het front-IP-configuratie, poort en ssl certificaat toewijzen.

    $listener = New-AzureRmApplicationGatewayHttpListener -Name listener01 -Protocol Https -FrontendIPConfiguration $fipconfig -FrontendPort $fp -SslCertificate $cert

### <a name="step-7"></a>Stap 7

Het certificaat moet worden gebruikt op de resources ssl is ingeschakeld in de backend uploaden.

> [AZURE.NOTE] De sonde standaard haalt de openbare sleutel van de **standaard** SSL-binding op de back-end-IP-adres en vergelijkt de waarde de openbare sleutel die wordt ontvangen op de openbare sleutel waarde die u hier opgeeft. De opgehaalde openbare sleutel mag niet de gewenste site die verkeer zal lopen **als** u gebruikmaakt van host-headers en SNI op de back-end. Bij twijfel, Ga naar de https://127.0.0.1/ op de back-ends te bevestigen welke certificaat wordt gebruikt om de **standaard** SSL-binding. Gebruik de openbare sleutel van die aanvraag in deze sectie. Als u gebruikmaakt van host-headers en SNI op HTTPS-bindingen niet ontvangt u een reactie en een certificaat van een browseraanvraag handmatig te https://127.0.0.1/ op de back-ends, moet u een standaard SSL-binding op de back-ends instellen. Als u niet doet dit, worden mislukt de sondes en de back-end niet whitelisted.

    $authcert = New-AzureRmApplicationGatewayAuthenticationCertificate -Name 'whitelistcert1' -CertificateFile C:\users\gwallace\Desktop\cert.cer

> [AZURE.NOTE] Het certificaat bedoeld in deze stap moet de openbare sleutel van het pfx-cert aanwezig op de backend. Exporteer het certificaat (niet het basiscertificaat) geïnstalleerd op de back endserver. CER opmaken en gebruiken in deze stap. Deze stap whitelists de backend met de toepassingsgateway.

### <a name="step-8"></a>Stap 8

De instellingen voor HTTP-back-end-gateway configureren. Het uploaden van de vorige stap om de instellingen voor HTTP-certificaat toewijzen.

    $poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name 'setting01' -Port 443 -Protocol Https -CookieBasedAffinity Enabled -AuthenticationCertificates $authcert

### <a name="step-9"></a>Stap 9

Maak een load balancer routing-regel die de load balancer gedrag wordt geconfigureerd. In dit voorbeeld wordt wordt een regel basic round-robin gemaakt.

    $rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name 'rule01' -RuleType basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool

### <a name="step-10"></a>Stap 10

De grootte van het exemplaar van de application-gateway configureren.  De beschikbare formaten worden **Standard\_kleine**, **Standard\_normaal**, en **Standard\_groot**.  Voor de capaciteit zijn de beschikbare waarden 1 tot en met 10.

    $sku = New-AzureRmApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2

>[AZURE.NOTE] Een aantal exemplaar 1 kan worden gekozen voor testdoeleinden. Het is belangrijk te weten dat elk exemplaar onder twee exemplaren wordt niet gedekt door de SLA en worden daarom niet aanbevolen. Kleine gateways zijn voor dev test en niet voor productiedoeleinden worden gebruikt.

### <a name="step-11"></a>Stap 11

Configureer het beleid voor SSL op de Gateway-toepassing worden gebruikt. Toepassingsgateway ondersteunt de mogelijkheid bepaalde versies van SSL-protocol uitschakelen.

De volgende waarden zijn een lijst van protocol-versies die kunnen worden uitgeschakeld.

- **TLSv1_0**
- **TLSv1_1**
- **TLSv1_2**

In het volgende voorbeeld wordt TLSv1\_0.

    $sslPolicy = New-AzureRmApplicationGatewaySslPolicy -DisabledSslProtocols TLSv1_0

## <a name="create-the-application-gateway"></a>De toepassingsgateway maken

Met behulp van de voorgaande stappen, de Gateway-toepassing maken. Het maken van de gateway is een langdurige proces.

    $appgw = New-AzureRmApplicationGateway -Name appgateway -SslCertificates $cert -ResourceGroupName "appgw-rg" -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku -SslPolicy $sslPolicy -AuthenticationCertificates $authcert -Verbose

## <a name="disable-ssl-protocol-versions-on-an-existing-application-gateway"></a>SSL-protocol versies op een bestaande toepassingsgateway uitschakelen

De voorgaande stappen, gaat u door middel van een toepassing maken met ssl van begin tot eind en bepaalde versies van SSL-protocol uit te schakelen. Het volgende voorbeeld wordt een bepaalde SSL-beleid op een bestaande toepassingsgateway.

### <a name="step-1"></a>Stap 1

Ophalen van de toepassingsgateway bij te werken.

    $gw = Get-AzureRmApplicationGateway -Name AdatumAppGateway -ResourceGroupName AdatumAppGatewayRG

### <a name="step-2"></a>Stap 2

Een SSL-beleid definiëren. In het volgende voorbeeld, zijn TLSv1.0 en TLSv1.1 uitgeschakeld.

    Set-AzureRmApplicationGatewaySslPolicy -DisabledSslProtocols TLSv1_0, TLSv1_1 -ApplicationGateway $gw

### <a name="step-3"></a>Stap 3

Ten slotte, de gateway bijwerken. Het is belangrijk te weten dat deze laatste stap een langdurige taak is. Als dit gebeurt, is complete ssl op de toepassingsgateway geconfigureerd.

    $gw | Set-AzureRmApplicationGateway

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

Meer informatie over de beveiliging van uw webtoepassingen met Web Application Firewall via een toepassingsgateway hardening via [Web Application Firewall-overzicht](application-gateway-webapplicationfirewall-overview.md)

[scenario]: ./media/application-gateway-end-to-end-ssl-powershell/scenario.png
