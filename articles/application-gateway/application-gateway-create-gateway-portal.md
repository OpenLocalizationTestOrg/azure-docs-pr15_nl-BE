<properties
   pageTitle="Maak een toepassingsgateway met behulp van de portal | Microsoft Azure"
   description="Informatie over het maken van een Gateway-toepassing met behulp van de portal"
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

# <a name="create-an-application-gateway-by-using-the-portal"></a>Een toepassingsgateway maken met behulp van de portal

> [AZURE.SELECTOR]
- [Azure portal](application-gateway-create-gateway-portal.md)
- [Azure PowerShell voor bronbeheer](application-gateway-create-gateway-arm.md)
- [Azure klassieke PowerShell](application-gateway-create-gateway.md)
- [Sjabloon voor Azure Resource Manager](application-gateway-create-gateway-arm-template.md)
- [Azure CLI](application-gateway-create-gateway-cli.md)

Azure Application Gateway is een laag 7 load balancer. Biedt failover, HTTP-verzoeken prestaties routering tussen verschillende servers, of ze nu op de cloud of op locatie. Application Gateway biedt vele functies van Application Delivery Controller (ADC) met inbegrip van HTTP-taakverdeling, op basis van een cookie sessie affiniteit, Secure Sockets Layer (SSL)-offload, aangepaste gezondheid sondes, ondersteuning voor meerdere locaties en vele andere. Ga voor een volledige lijst van ondersteunde functies naar [Gateway: overzicht](application-gateway-introduction.md)

## <a name="scenario"></a>Scenario

In dit scenario wordt informatie over het maken van een toepassingsgateway met behulp van de portal Azure.

In dit scenario wordt:

- Maak een toepassingsgateway gemiddeld met twee exemplaren.
- Maak een virtueel netwerk met de naam AdatumAppGatewayVNET met een gereserveerd blok CIDR van 10.0.0.0/16.
- Maak een subnet genaamd Appgatewaysubnet dat 10.0.0.0/28 als de CIDR-blok gebruikt.
- Een certificaat configureren voor SSL-offload.

![Voorbeeld scenario][scenario]

>[AZURE.IMPORTANT] Aanvullende configuratie van de toepassingsgateway, met inbegrip van aangepaste gezondheid sondes, back-end-groep met adressen en aanvullende regels zijn geconfigureerd nadat de toepassingsgateway is geconfigureerd en niet tijdens de eerste installatie.

## <a name="before-you-begin"></a>Voordat u begint

Azure Application Gateway vereist zijn eigen subnet. Bij het maken van een virtueel netwerk, ervoor te zorgen dat u laat voldoende adresruimte als u wilt dat meerdere subnetten. Zodra u een toepassingsgateway met een subnet implementeert, kunnen alleen extra Toepassingsgateways worden toegevoegd aan het subnet.

## <a name="create-the-application-gateway"></a>De toepassingsgateway maken

### <a name="step-1"></a>Stap 1

Ga naar de portal Azure, klikt u op **Nieuw** > **netwerk** > **Application Gateway**

![Application Gateway maken][1]

### <a name="step-2"></a>Stap 2

Vervolgens vult u de basisgegevens voor de application gateway. Klik op **OK** wanneer voltooid

Is de informatie die nodig is voor de basisinstellingen:

- **Name** - de naam van de toepassingsgateway.
- **Laag** - dit is de laag van de toepassingsgateway. Twee lagen zijn beschikbaar, **WAF** en **standaard**. WAF kunnen de web application firewall-functie.
- **Grootte SKU** - met deze instelling wordt de grootte van de toepassingsgateway, de beschikbare opties zijn (**klein**, **middelgroot**en **groot**). *Kleine is niet beschikbaar als WAF laag wordt gekozen.*
- **Instantie het aantal** - het aantal exemplaren, deze waarde moet een getal tussen 2 en 10.
- **Groep** - de resourcegroep voor de toepassingsgateway kan bestaan uit een bestaande resourcegroep of een nieuwe.
- **Locatie** - de regio voor de toepassingsgateway is dezelfde locatie op de resourcegroep. *De locatie is belangrijk als het virtuele netwerk en openbare IP moet op dezelfde locatie als de gateway*.

![Basisinstellingen voor blade weergeven][2]

>[AZURE.NOTE] Een aantal exemplaar 1 kan worden gekozen voor testdoeleinden. Het is belangrijk te weten dat elk exemplaar onder twee exemplaren wordt niet gedekt door de SLA en worden daarom niet aanbevolen. Kleine gateways zijn voor dev test en niet voor productiedoeleinden worden gebruikt.

### <a name="step-3"></a>Stap 3

Zodra de basisinstellingen zijn gedefinieerd, is de volgende stap voor het definiëren van het virtuele netwerk moet worden gebruikt. Het virtuele netwerk ook de toepassing die de toepassingsgateway load balancing voor nieuwste.

Klik op **een virtueel netwerk kiezen** om het virtuele netwerk configureren.

![Blade weergegeven met instellingen voor toepassingsgateway][3]

### <a name="step-4"></a>Stap 4

Klik op **Nieuw** in het blad *Virtueel netwerk kiezen*

Terwijl dat niet in dit scenario wordt uitgelegd, kan een bestaande virtuele netwerk op dit moment worden geselecteerd.  Als u een bestaande virtuele netwerk wordt gebruikt, is het belangrijk om te weten dat het virtuele netwerk moet een lege subnet of een subnet van alleen toepassing gateway resources worden gebruikt.

![blade virtuele netwerk kiezen][4]

### <a name="step-5"></a>Stap 5

Het netwerkgegevens invult in het blade **Virtuele netwerken maken** zoals wordt beschreven in de beschrijving van de voorgaande [Scenario](#scenario) .

![Blade virtuele netwerken maken met gegevens die zijn ingevoerd][5]

### <a name="step-6"></a>Stap 6

Als het virtuele netwerk is gemaakt, is de volgende stap voor het definiëren van de front-IP voor de toepassingsgateway. Op dit moment is de keuze tussen een openbare of een particuliere IP-adres van de front-end. De keuze hangt de toepassing internet gerichte of interne alleen. In dit scenario wordt ervan uitgegaan dat een openbaar IP-adres. Als u een particulier IP-adres, kan de **particulier** worden geklikt. Een automatisch toegewezen IP-adres is gekozen, of kunt u het selectievakje **Kies een specifiek particulier IP-adres** als u een handmatig.

### <a name="step-7"></a>Stap 7

Klik op **een openbaar IP-adres kiezen**. Als een bestaande openbare IP-adres beschikbaar is op dit moment kunt kiezen, in dit scenario maakt u een nieuwe openbare IP-adres. Klik op **Nieuw**

![Kies blade van openbare IP-adres][6]

### <a name="step-8"></a>Stap 8

Vervolgens geeft het openbare IP-adres een beschrijvende naam en klik op **OK**

![Blade van openbare IP-adres maken][7]

### <a name="step-9"></a>Stap 9

De laatste instelling configureren bij het maken van een toepassingsgateway is de listener-configuratie.  Als u **http** gebruikt, blijft er niets als u wilt configureren en **OK** kan worden geklikt. Voor het gebruik van **https** verdere is configuratie vereist.

Een certificaat is vereist voor het gebruik van **https**. De persoonlijke sleutel van het certificaat is vereist, zodat een .pfx-export van het certificaat en het wachtwoord moet worden opgegeven.

### <a name="step-10"></a>Stap 10

**HTTPS**, klik op **het mappictogram naast het **certificaat uploaden PFX** textbox** .
Ga naar het certificaat pfx-bestand in het bestandssysteem. Als deze optie is geselecteerd, het certificaat een beschrijvende naam geven en typ het wachtwoord voor het pfx-bestand.

Klik op **OK** om de instellingen voor de Application Gateway als voltooid.

![Listener-configuratiesectie op instellingen blade][9]

### <a name="step-11"></a>Stap 11

Controleer de pagina Samenvatting en klik op **OK**.  Nu is de toepassingsgateway in de wachtrij geplaatst en gemaakt.

### <a name="step-12"></a>Stap 12

Zodra de toepassingsgateway is gemaakt, gaan in de portal te gaan met de configuratie van de toepassingsgateway.

![Application Gateway resourceweergave][10]

Deze stappen maken een basistoepassing-gateway met de standaardinstellingen voor de listener, back-end-toepassingen, back-end HTTP-instellingen en regels. U kunt deze instellingen aanpassen aan uw implementatie zodra het inrichten voltooid is wijzigen.

## <a name="next-steps"></a>Volgende stappen

In dit scenario maakt een standaardgateway voor de toepassing. De volgende stappen zijn voor het configureren van de toepassingsgateway door leden van de groep toe te voegen, te wijzigen en aan te passen regels in de gateway voor het goed te laten werken.

Informatie over het maken van aangepaste gezondheid sondes via [een sonde met aangepaste gezondheid maken](application-gateway-create-probe-portal.md)

Informatie over het configureren van SSL-Offloading en nemen de dure SSL-decodering uit uw webservers via [SSL-Offload configureren](application-gateway-ssl-portal.md)

Informatie over het beveiligen van uw toepassingen met [Web Application Firewall](application-gateway-webapplicationfirewall-overview.md) een functie van application gateway.

<!--Image references-->
[1]: ./media/application-gateway-create-gateway-portal/figure1.png
[2]: ./media/application-gateway-create-gateway-portal/figure2.png
[3]: ./media/application-gateway-create-gateway-portal/figure3.png
[4]: ./media/application-gateway-create-gateway-portal/figure4.png
[5]: ./media/application-gateway-create-gateway-portal/figure5.png
[6]: ./media/application-gateway-create-gateway-portal/figure6.png
[7]: ./media/application-gateway-create-gateway-portal/figure7.png
[8]: ./media/application-gateway-create-gateway-portal/figure8.png
[9]: ./media/application-gateway-create-gateway-portal/figure9.png
[10]: ./media/application-gateway-create-gateway-portal/figure10.png
[scenario]: ./media/application-gateway-create-gateway-portal/scenario.png
