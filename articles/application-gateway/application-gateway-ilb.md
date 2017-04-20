<properties 
   pageTitle="Maken en configureren van een toepassingsgateway met interne taakverdeling (ILB) in een virtueel netwerk | Microsoft Azure"
   description="Deze pagina bevat instructies voor een toepassingsgateway Azure configureren met een interne Netwerktaakverdeling eindpunt"
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
   ms.date="08/19/2016"
   ms.author="gwallace"/>

# <a name="create-an-application-gateway-with-an-internal-load-balancer-ilb"></a>Een toepassingsgateway maken met een interne taakverdeling (ILB)

> [AZURE.SELECTOR]
- [Azure klassieke werk](application-gateway-ilb.md)
- [Resource Manager Powershell stappen](application-gateway-ilb-arm.md)

Toepassingsgateway kan worden geconfigureerd met een virtuele IP toegankelijk via internet of met een interne eindpunt niet blootgesteld aan internet, ook wel bekend als eindpunt van interne Load Balancer (ILB). De gateway configureren met een ILB is nuttig voor interne line-of-business-toepassingen niet blootgesteld aan internet. Het is ook handig voor services/lagen in een toepassing met meerdere niveaus, die zich bevindt in een beveiligingsgrens niet blootgesteld aan internet, maar moet nog round robin verdeling van de belasting, sessie kleverigheid of SSL-beëindiging. Dit artikel begeleidt u door de stappen voor het configureren van een toepassingsgateway met een ILB.

## <a name="before-you-begin"></a>Voordat u begint

1. Installeer de nieuwste versie van de via het Web Platform Installer Azure PowerShell-cmdlets. U kunt downloaden en installeren van de meest recente versie van de **Windows PowerShell** -sectie van de [pagina te downloaden](https://azure.microsoft.com/downloads/).
2. Controleer of u een werkende virtuele netwerk met een geldig subnetmasker.
3. Controleer of u een back-end-servers hebt in het virtuele netwerk of met een openbaar IP-/ VIP toegewezen.


U maakt een toepassingsgateway, moet u de volgende stappen uitvoeren in de aangegeven volgorde. 

1. [Een toepassingsgateway maken](#create-a-new-application-gateway)
2. [De gateway configureren](#configure-the-gateway)
3. [De gatewayconfiguratie instellen](#set-the-gateway-configuration)
4. [Start de gateway](#start-the-gateway)
4. [Controleer of de gateway](#verify-the-gateway-status)



## <a name="create-an-application-gateway"></a>Maak een toepassingsgateway:

**Voor het maken van de gateway**, gebruik de `New-AzureApplicationGateway` cmdlet, vervangen door de waarden zelf. Houd er rekening mee dat facturering van de gateway niet op dit moment wordt gestart. Facturering begint in een latere stap de gateway is gestart.

    PS C:\> New-AzureApplicationGateway -Name AppGwTest -VnetName testvnet1 -Subnets @("Subnet-1")

    VERBOSE: 4:31:35 PM - Begin Operation: New-AzureApplicationGateway 
    VERBOSE: 4:32:37 PM - Completed Operation: New-AzureApplicationGateway
    Name       HTTP Status Code     Operation ID                             Error 
    ----       ----------------     ------------                             ----
    Successful OK                   55ef0460-825d-2981-ad20-b9a8af41b399

**Om te valideren** dat de gateway is gemaakt, kunt u de `Get-AzureApplicationGateway` cmdlet. 

In het voorbeeld, *Beschrijving*, *InstanceCount*en *GatewaySize* zijn optionele parameters. De standaardwaarde voor *InstanceCount* is 2, met een maximale waarde van 10. De standaardwaarde voor *GatewaySize* is normaal. Kleine en grote andere waarden beschikbaar zijn. *VIP* - en *DNS-naam* worden weergegeven als lege omdat de gateway is nog niet begonnen. Deze worden gemaakt zodra de gateway actief is. 

    PS C:\> Get-AzureApplicationGateway AppGwTest

    VERBOSE: 4:39:39 PM - Begin Operation:
    Get-AzureApplicationGateway VERBOSE: 4:39:40 PM - Completed 
    Operation: Get-AzureApplicationGateway
    Name: AppGwTest 
    Description: 
    VnetName: testvnet1 
    Subnets: {Subnet-1} 
    InstanceCount: 2 
    GatewaySize: Medium 
    State: Stopped 
    VirtualIPs: 
    DnsName:


## <a name="configure-the-gateway"></a>De gateway configureren

Een gateway Toepassingsconfiguratie bestaat uit meerdere waarden. De waarden kunnen worden verbonden bij elkaar om samen te stellen van de configuratie.
 
De waarden zijn:

- **Back-end server-groep:** De lijst met IP-adressen van de back-end servers. Het weergegeven IP-adressen moeten hetzij deel uitmaken van het subnet VNet, of moeten een openbaar IP/VIP. 
- **Instellingen voor back-end server toepassingen:** Elke groep heeft als poort, protocol en affiniteit op basis van een cookie-instellingen. Deze instellingen zijn gekoppeld aan een groep en worden toegepast op alle servers in de groep.
- **Frontend-poort:** Dit wordt de openbare poort op de toepassingsgateway geopend. Verkeer treft deze poort en wordt vervolgens omgeleid naar een van de back-end servers.
- **Listener:** De listener is een front-end-poort, een protocol (Http of Https, dit zijn hoofdlettergevoelig), en de SSL-certificaat (als het configureren van SSL-offload). 
- **Regel:** De regel wordt de listener en de groep met back-end server gebonden en definieert welke het verkeer moet worden besteed aan wanneer deze een bepaalde listener backend server-groep. Op dit moment wordt *alleen de basisregel* ondersteund. *De basisregel* is de verdeling van de belasting van round-robin.

U kunt de configuratie maken door een configuratieobject te maken, of met behulp van een XML-configuratiebestand. Als u wilt uw configuratie samenstellen met behulp van een XML-configuratiebestand, gebruik het onderstaande voorbeeld.



Let op het volgende:


- Het element *FrontendIPConfigurations* beschrijving van de ILB-gegevens relevant voor Application Gateway configureren met een ILB. 

- De Frontend IP- *Type* moet worden ingesteld op 'Privé'

- De *StaticIPAddress* moet worden ingesteld op de gewenste interne IP-adres waarop de gateway verkeer ontvangt. Houd er rekening mee dat het element *StaticIPAddress* optioneel is. Als dit niet is ingesteld, een beschikbare interne IP van de geïmplementeerde subnet wordt gekozen. 

- De waarde van het *naam* -element dat is opgegeven in *FrontendIPConfiguration* moet worden gebruikt in de HTTPListener van *FrontendIP* -element om te verwijzen naar de FrontendIPConfiguration.

 **Configuratie van XML-voorbeeld**

 

        <?xml version="1.0" encoding="utf-8"?>
        <ApplicationGatewayConfiguration xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/windowsazure">
            <FrontendIPConfigurations>
                <FrontendIPConfiguration>
                    <Name>fip1</Name> 
                    <Type>Private</Type> 
                    <StaticIPAddress>10.0.0.10</StaticIPAddress> 
                </FrontendIPConfiguration>
            </FrontendIPConfigurations>
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
                    <FrontendIP>fip1</FrontendIP>
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
    


## <a name="set-the-gateway-configuration"></a>De gatewayconfiguratie instellen

Vervolgens stelt u de toepassingsgateway. U kunt de `Set-AzureApplicationGatewayConfig` cmdlet met configuratie-object, of een XML-configuratiebestand. 

    PS C:\> Set-AzureApplicationGatewayConfig -Name AppGwTest -ConfigFile D:\config.xml

    VERBOSE: 7:54:59 PM - Begin Operation: Set-AzureApplicationGatewayConfig 
    VERBOSE: 7:55:32 PM - Completed Operation: Set-AzureApplicationGatewayConfig
    Name       HTTP Status Code     Operation ID                             Error 
    ----       ----------------     ------------                             ----
    Successful OK                   9b995a09-66fe-2944-8b67-9bb04fcccb9d

## <a name="start-the-gateway"></a>Start de gateway

Zodra de gateway is geconfigureerd, gebruikt u de `Start-AzureApplicationGateway` cmdlet starten de gateway. Facturering van een toepassingsgateway begint nadat de gateway is gestart. 


> [AZURE.NOTE] De `Start-AzureApplicationGateway` cmdlet kan maximaal 15-20 minuten in beslag nemen. 
   
    PS C:\> Start-AzureApplicationGateway AppGwTest 

    VERBOSE: 7:59:16 PM - Begin Operation: Start-AzureApplicationGateway 
    VERBOSE: 8:05:52 PM - Completed Operation: Start-AzureApplicationGateway
    Name       HTTP Status Code     Operation ID                             Error 
    ----       ----------------     ------------                             ----
    Successful OK                   fc592db8-4c58-2c8e-9a1d-1c97880f0b9b

## <a name="verify-the-gateway-status"></a>Controleer of de status van de gateway

Gebruik de `Get-AzureApplicationGateway` cmdlet te controleren van de status van de gateway. *Start AzureApplicationGateway* is voltooid in de vorige stap, de staat moet worden *uitgevoerd*als de DNS-naam en de Vip moeten geldige waarden hebben. Dit voorbeeld ziet u de cmdlet op de eerste regel, gevolgd door de uitvoer. In dit voorbeeld wordt de gateway wordt uitgevoerd en gereed is voor verkeer. 

> [AZURE.NOTE] De toepassingsgateway is geconfigureerd voor het accepteren van verkeer op het eindpunt van de geconfigureerde ILB van 10.0.0.10 in dit voorbeeld.

    PS C:\> Get-AzureApplicationGateway AppGwTest 

    VERBOSE: 8:09:28 PM - Begin Operation: Get-AzureApplicationGateway 
    VERBOSE: 8:09:30 PM - Completed Operation: Get-AzureApplicationGateway
    Name          : AppGwTest
    Description   : 
    VnetName      : testvnet1
    Subnets       : {Subnet-1}
    InstanceCount : 2
    GatewaySize   : Medium
    State         : Running
    VirtualIPs    : {10.0.0.10}
    DnsName       : appgw-b2a11563-2b3a-4172-a4aa-226ee4c23eed.cloudapp.net

## <a name="next-steps"></a>Volgende stappen


Als u meer informatie over het algemeen balancing opties laden, Zie:

- [Azure Load Balancer](https://azure.microsoft.com/documentation/services/load-balancer/)
- [Azure verkeer Manager](https://azure.microsoft.com/documentation/services/traffic-manager/)
