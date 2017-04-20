<properties
   pageTitle="Een toepassingsgateway voor offload van SSL configureren met behulp van klassieke implementatie | Microsoft Azure"
   description="Dit artikel bevat instructies voor het maken van een toepassingsgateway met SSL-offload met behulp van het model Klassiek Azure-implementatie."
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

# <a name="configure-an-application-gateway-for-ssl-offload-by-using-the-classic-deployment-model"></a>Een toepassingsgateway voor offload van SSL configureren met behulp van het implementatiemodel klassiek

> [AZURE.SELECTOR]
-[Azure portal](application-gateway-ssl-portal.md)
-[Azure Resource Manager PowerShell](application-gateway-ssl-arm.md)
-[Klassieke PowerShell Azure](application-gateway-ssl.md)

Azure toepassingsgateway kan worden geconfigureerd om de Secure Sockets Layer (SSL)-sessie op de gateway om te voorkomen dat kostbare SSL-decodering taken te gebeuren op de web-farm te beëindigen. SSL-offload vereenvoudigt ook de front-end-server setup en het beheer van de webtoepassing.

## <a name="before-you-begin"></a>Voordat u begint

1. Installeer de nieuwste versie van de Azure PowerShell-cmdlets met behulp van de Web Platform Installer. U kunt downloaden en installeren van de meest recente versie van de **Windows PowerShell** -sectie van de [pagina met Downloads](https://azure.microsoft.com/downloads/).
2. Controleer of u een werkende virtuele netwerk met een geldig subnetmasker. Controleer of geen virtuele machines of cloud implementaties het subnet worden gebruikt. De toepassingsgateway moet zijn op zichzelf in een virtueel netwerk subnet.
3. De servers die u configureert voor het gebruik van de toepassingsgateway moeten bestaan of de eindpunten ervan gemaakt in het virtuele netwerk of met een openbaar IP-/ VIP hebt toegewezen.

Offload SSL configureren op een toepassingsgateway, voer de volgende stappen in de aangegeven volgorde:

1. [Een toepassingsgateway maken](#create-an-application-gateway)
2. [Uploaden SSL-certificaten](#upload-ssl-certificates)
3. [De gateway configureren](#configure-the-gateway)
4. [De gatewayconfiguratie instellen](#set-the-gateway-configuration)
5. [Start de gateway](#start-the-gateway)
6. [Controleer of de status van de gateway](#verify-the-gateway-status)


## <a name="create-an-application-gateway"></a>Een toepassingsgateway maken

U kunt de gateway maken met de cmdlet **New-AzureApplicationGateway** , de waarden te vervangen door uw eigen. Facturering van de gateway op dit moment niet worden gestart. Facturering begint in een latere stap de gateway is gestart.

    New-AzureApplicationGateway -Name AppGwTest -VnetName testvnet1 -Subnets @("Subnet-1")

Om te valideren dat de gateway is gemaakt, kunt u de cmdlet **Get-AzureApplicationGateway** .

In het voorbeeld, *Beschrijving*, *InstanceCount*en *GatewaySize* zijn optionele parameters. De standaardwaarde voor *InstanceCount* is 2, met een maximale waarde van 10. De standaardwaarde voor *GatewaySize* is normaal. Kleine en grote andere waarden beschikbaar zijn. *VirtualIPs* en de *DNS-naam* worden weergegeven als lege omdat de gateway is nog niet begonnen. Deze waarden worden gemaakt zodra de gateway actief is.

    Get-AzureApplicationGateway AppGwTest

## <a name="upload-ssl-certificates"></a>Uploaden SSL-certificaten

**Add AzureApplicationGatewaySslCertificate** certificaat van de server in de *pfx* -indeling uploaden naar de toepassingsgateway gebruiken. De naam van de naam van een gebruiker zijn gekozen en moet uniek zijn binnen de toepassingsgateway. Dit certificaat wordt verwezen door deze naam in alle beheersbewerkingen in certificaat op de toepassingsgateway.

In dit volgende voorbeeld wordt de cmdlet, de waarden in het voorbeeld vervangen door uw eigen.

    Add-AzureApplicationGatewaySslCertificate  -Name AppGwTest -CertificateName GWCert -Password <password> -CertificateFile <full path to pfx file>

Vervolgens wordt het certificaat uploaden valideren. Gebruik de cmdlet **Get-AzureApplicationGatewayCertificate** .

Dit voorbeeld ziet u de cmdlet op de eerste regel, gevolgd door de uitvoer.

    Get-AzureApplicationGatewaySslCertificate AppGwTest

    VERBOSE: 5:07:54 PM - Begin Operation: Get-AzureApplicationGatewaySslCertificate
    VERBOSE: 5:07:55 PM - Completed Operation: Get-AzureApplicationGatewaySslCertificate
    Name           : SslCert
    SubjectName    : CN=gwcert.app.test.contoso.com
    Thumbprint     : AF5ADD77E160A01A6......EE48D1A
    ThumbprintAlgo : sha1RSA
    State..........: Provisioned

>[AZURE.NOTE] Het wachtwoord voor het certificaat is tussen de 4 tot 12 tekens, letters of getallen. Speciale tekens worden niet geaccepteerd.

## <a name="configure-the-gateway"></a>De gateway configureren

Een gateway Toepassingsconfiguratie bestaat uit meerdere waarden. De waarden kunnen worden verbonden bij elkaar om samen te stellen van de configuratie.

De waarden zijn:

- **Back-end server-groep:** De lijst met IP-adressen van de back-end servers. De IP-adressen ofwel moeten behoren tot het virtuele netwerk subnet of moeten een openbaar IP/VIP.
- **Instellingen voor back-end server toepassingen:** Elke groep heeft als poort, protocol en affiniteit op basis van een cookie-instellingen. Deze instellingen zijn gekoppeld aan een groep en worden toegepast op alle servers in de groep.
- **Front-poort:** Dit wordt de openbare poort op de toepassingsgateway wordt geopend. Verkeer treft deze poort en wordt vervolgens omgeleid naar een van de back-end servers.
- **Listener:** De listener is een front-end-poort, een protocol (Http of Https, deze waarden zijn hoofdlettergevoelig), en de SSL-certificaat (als het configureren van SSL-offload).
- **Regel:** De regel wordt de listener en de groep met back-end-server en wordt gedefinieerd welke het verkeer moet worden besteed aan een bepaalde listener wanneer deze groep met back-end-server. Op dit moment wordt *alleen de basisregel* ondersteund. *De basisregel* is de verdeling van de belasting van round-robin.

**Aanvullende opmerkingen**

Voor het configureren van SSL-certificaten moet het protocol in de **HttpListener** veranderen in *Https* (hoofdlettergevoelig). Het element **SslCert** is toegevoegd aan de **HttpListener** met de waarde ingesteld op dezelfde naam als in het uploaden van de voorgaande sectie voor SSL-certificaten. De front-poort moet worden bijgewerkt op 443.

**Inschakelen van de affiniteit op basis van cookie**: een toepassingsgateway kan worden geconfigureerd om ervoor te zorgen dat een aanvraag van een clientsessie altijd naar dezelfde VM in de farm wordt gestuurd. In dit scenario wordt gedaan door injectie van een sessiecookie waarmee de gateway naar directe verkeer op de juiste manier. Instellen zodat op basis van een cookie affiniteit **CookieBasedAffinity** *ingeschakeld* in het element **BackendHttpSettings** .



U kunt uw configuratie maken door een configuratieobject te maken of met behulp van een XML-configuratiebestand.
Om de configuratie maken met behulp van een XML-configuratiebestand, gebruikt u in het volgende voorbeeld:

**Configuratie van XML-voorbeeld**

    <?xml version="1.0" encoding="utf-8"?>
    <ApplicationGatewayConfiguration xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/windowsazure">
        <FrontendIPConfigurations />
        <FrontendPorts>
            <FrontendPort>
                <Name>FrontendPort1</Name>
                <Port>443</Port>
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
                <Protocol>Https</Protocol>
                <SslCert>GWCert</SslCert>
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

Stel vervolgens de toepassingsgateway. U kunt de cmdlet **Set-AzureApplicationGatewayConfig** met een configuratieobject of een XML-configuratiebestand.

    Set-AzureApplicationGatewayConfig -Name AppGwTest -ConfigFile D:\config.xml

## <a name="start-the-gateway"></a>Start de gateway

Na de configuratie van de gateway met de **Start-AzureApplicationGateway** -cmdlet kunt starten de gateway. Facturering van een toepassingsgateway begint nadat de gateway is gestart.


**Opmerking:** De cmdlet **Start AzureApplicationGateway** duurt maximaal 15-20 minuten duren.

    Start-AzureApplicationGateway AppGwTest

## <a name="verify-the-gateway-status"></a>Controleer of de status van de gateway

De cmdlet **Get-AzureApplicationGateway** gebruiken om de status van de gateway. **Start AzureApplicationGateway** is voltooid in de vorige stap, *staat* moet worden uitgevoerd als *VirtualIPs* en de *DNS-naam* moet geldig posten hebben.

In dit voorbeeld ziet u een toepassingsgateway, actief, en is klaar om verkeer te.

    Get-AzureApplicationGateway AppGwTest

    Name          : AppGwTest2
    Description   :
    VnetName      : testvnet1
    Subnets       : {Subnet-1}
    InstanceCount : 2
    GatewaySize   : Medium
    State         : Running
    VirtualIPs    : {23.96.22.241}
    DnsName       : appgw-4c960426-d1e6-4aae-8670-81fd7a519a43.cloudapp.net


## <a name="next-steps"></a>Volgende stappen


Als u meer informatie over het algemeen balancing opties laden, Zie:

- [Azure Load Balancer](https://azure.microsoft.com/documentation/services/load-balancer/)
- [Azure verkeer Manager](https://azure.microsoft.com/documentation/services/traffic-manager/)