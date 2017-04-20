<properties
   pageTitle="Maak een toepassingsgateway in Resource Manager met behulp van de CLI Azure | Microsoft Azure"
   description="Informatie over het maken van een Gateway-toepassing met behulp van de CLI Azure in Resource Manager"
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
   ms.date="10/25/2016"
   ms.author="gwallace" />

# <a name="create-an-application-gateway-by-using-the-azure-cli"></a>Een toepassingsgateway maken met behulp van de CLI Azure

> [AZURE.SELECTOR]
- [Azure portal](application-gateway-create-gateway-portal.md)
- [Azure PowerShell voor bronbeheer](application-gateway-create-gateway-arm.md)
- [Azure klassieke PowerShell](application-gateway-create-gateway.md)
- [Sjabloon voor Azure Resource Manager](application-gateway-create-gateway-arm-template.md)
- [Azure CLI](application-gateway-create-gateway-cli.md)

Azure Application Gateway is een laag 7 load balancer. Biedt failover, HTTP-verzoeken prestaties routering tussen verschillende servers, of ze nu op de cloud of op locatie. Toepassingsgateway heeft de volgende onderdelen van toepassing levering: http-taakverdeling, op basis van een cookie sessie affiniteit en Secure Sockets Layer (SSL)-offload, aangepaste gezondheid sondes laden en ondersteuning voor meerdere locaties.

## <a name="prerequisite-install-the-azure-cli"></a>Let op: De Azure CLI installeren

Als u de stappen in dit artikel uitvoert, moet u [de opdrachtregelinterface Azure voor Mac, Linux en Windows Azure CLI () installeren](../xplat-cli-install.md) en moet u aan te [melden op Azure](../xplat-cli-connect.md). 

> [AZURE.NOTE] Als u geen Azure account hebt, moet u een. Aanmelden voor een [gratis proefperiode hier](../active-directory/sign-up-organization.md)omhoog gaan.

## <a name="scenario"></a>Scenario

In dit scenario wordt informatie over het maken van een toepassingsgateway met behulp van de portal Azure.

In dit scenario wordt:

- Maak een toepassingsgateway gemiddeld met twee exemplaren.
- Maak een virtueel netwerk met de naam AdatumAppGatewayVNET met een gereserveerd blok CIDR van 10.0.0.0/16.
- Maak een subnet genaamd Appgatewaysubnet dat 10.0.0.0/28 als de CIDR-blok gebruikt.
- Een certificaat configureren voor SSL-offload.

![Voorbeeld scenario][scenario]

>[AZURE.NOTE] Aanvullende configuratie van de toepassingsgateway, met inbegrip van aangepaste gezondheid sondes, back-end-groep met adressen en aanvullende regels zijn geconfigureerd nadat de toepassingsgateway is geconfigureerd en niet tijdens de eerste installatie.

## <a name="before-you-begin"></a>Voordat u begint

Azure Application Gateway vereist zijn eigen subnet. Bij het maken van een virtueel netwerk, ervoor te zorgen dat u laat voldoende adresruimte als u wilt dat meerdere subnetten. Zodra u een toepassingsgateway met een subnet implementeert, kunnen alleen extra Toepassingsgateways worden toegevoegd aan het subnet.

## <a name="log-in-to-azure"></a>Log in op Azure

Open de **opdrachtprompt van Microsoft Azure**en aanmelden. 

    azure login

Als u het bovenstaande voorbeeld typt, krijgt u een code. Ga naar https://aka.ms/devicelogin in een browser om door te gaan met de aanmelding.

![cmd tonen apparaat aanmelden][1]

Voer de code die u hebt ontvangen in de browser. U wordt omgeleid naar een pagina.

![Voer code in browser][2]

Nadat u de code hebt ingevoerd u zich hebt aangemeld, sluit u de browser om door te gaan op met het scenario.

![aangemeld][3]

## <a name="switch-to-resource-manager-mode"></a>Overschakelen naar de modus Resource Manager

    azure config mode arm

## <a name="create-the-resource-group"></a>De resourcegroep maken

Voordat u de toepassingsgateway maakt, is een resourcegroep gemaakt voor de toepassingsgateway. Hieronder ziet u de opdracht.

    azure group create -n AdatumAppGatewayRG -l eastus

## <a name="create-a-virtual-network"></a>Een virtueel netwerk maken

Als de resourcegroep is gemaakt, wordt een virtueel netwerk voor de toepassingsgateway gemaakt.  In het volgende voorbeeld is de adresruimte als 10.0.0.0/16 als omschreven in de bovenstaande scenario's.

    azure network vnet create -n AdatumAppGatewayVNET -a 10.0.0.0/16 -g AdatumAppGatewayRG -l eastus

## <a name="create-a-subnet"></a>Een subnet maken

Nadat het virtuele netwerk is gemaakt, wordt een subnet voor de toepassingsgateway toegevoegd.  Als u van plan bent een toepassingsgateway gebruiken met een web app in hetzelfde virtuele netwerk als de toepassingsgateway wordt gehost, moet u onvoldoende ruimte is voor een ander subnet.

    azure network vnet subnet create -g AdatumAppGatewayRG -n Appgatewaysubnet -v AdatumAppGatewayVNET -a 10.0.0.0/28 

## <a name="create-the-application-gateway"></a>De toepassingsgateway maken

Zodra het virtuele netwerk en subnet zijn gemaakt, zijn de noodzakelijke voorwaarden voor de toepassingsgateway voltooid. Bovendien een certificaat eerder geëxporteerde .pfx-bestand en het wachtwoord voor het certificaat zijn vereist voor de volgende stap: het IP-adressen gebruikt voor de back-end worden de IP-adressen voor de back endserver. Deze waarden kunnen bestaan uit persoonlijke IP-adressen in het virtuele netwerk, openbare IP-adressen of volledig gekwalificeerde domeinnamen voor uw back-end servers.

    azure network application-gateway create -n AdatumAppGateway -l eastus -g AdatumAppGatewayRG -e AdatumAppGatewayVNET -m Appgatewaysubnet -r 134.170.185.46,134.170.188.221,134.170.185.50 -y c:\AdatumAppGateway\adatumcert.pfx -x P@ssw0rd -z 2 -a Standard_Medium -w Basic -j 443 -f Enabled -o 80 -i http -b https -u Standard

> [AZURE.NOTE] Voor een lijst met parameters die u tijdens het maken van de volgende opdracht: **toepassingsgateway azure netwerk maken--help**.

In het volgende voorbeeld wordt een eenvoudige toepassingsgateway gemaakt met de standaardinstellingen voor de listener, back-end-toepassingen, back-end HTTP-instellingen en regels. Het SSL-offload ook geconfigureerd. U kunt deze instellingen aanpassen aan uw implementatie zodra het inrichten voltooid is wijzigen.
Als u al uw webtoepassing die is gedefinieerd met de de back-end-toepassingen in de voorgaande stappen, eenmaal gemaakt, taakverdeling begint.

## <a name="next-steps"></a>Volgende stappen

Informatie over het maken van aangepaste gezondheid sondes via [een sonde met aangepaste gezondheid maken](application-gateway-create-probe-portal.md)

Informatie over het configureren van SSL-Offloading en nemen de dure SSL-decodering uit uw webservers via [SSL-Offload configureren](application-gateway-ssl-arm.md)

<!--Image references-->

[scenario]: ./media/application-gateway-create-gateway-cli/scenario.png
[1]: ./media/application-gateway-create-gateway-cli/figure1.png
[2]: ./media/application-gateway-create-gateway-cli/figure2.png
[3]: ./media/application-gateway-create-gateway-cli/figure3.png