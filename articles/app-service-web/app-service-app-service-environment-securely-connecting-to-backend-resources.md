<properties 
    pageTitle="Veilig verbinding maken met back-end bronnen uit de omgeving van een App-Service" 
    description="Meer informatie over veilig verbinding maken met back-end van een App-omgeving." 
    services="app-service" 
    documentationCenter="" 
    authors="stefsch" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/04/2016" 
    ms.author="stefsch"/>   

# <a name="securely-connecting-to-backend-resources-from-an-app-service-environment"></a>Veilig verbinding maken met back-end bronnen uit de omgeving van een App-Service #

## <a name="overview"></a>Overzicht ##
Aangezien een App-omgeving wordt altijd gemaakt in **beide** een virtueel netwerk Azure Resource Manager, **of** een klassieke distributie model [virtueel netwerk][virtualnetwork], uitgaande verbindingen van een App-omgeving met andere bronnen backend uitsluitend via het virtuele netwerk kunnen stromen.  Met een recente wijziging in juni 2016, kan ASEs ook worden geïmplementeerd in de virtuele netwerken die gebruikmaken van openbare adresbereiken of RFC1918-adresruimten (dat wil zeggen particuliere adressen).  

Zo kan er een SQL-Server op een cluster van virtuele machines met poort 1433 vergrendeld.  Het eindpunt is mogelijk ACLd alleen toegang te verlenen uit andere bronnen op het virtuele netwerk.  

Een ander voorbeeld gevoelige eindpunten op ruimten kan uitvoeren en worden verbonden met Azure via een [Site naar Site] [ SiteToSite] of [Azure ExpressRoute] [ ExpressRoute] verbindingen.  Alleen de resources in de virtuele netwerken die zijn verbonden met de website of ExpressRoute tunnels worden daardoor toegang kunnen krijgen tot de eindpunten op de ruimten.

Voor al deze scenario's worden apps op een App-omgeving veilig verbinding maken met de verschillende servers en bronnen.  Uitgaand verkeer vanuit apps uitgevoerd in een omgeving van de Service App aan particuliere eindpunten in hetzelfde virtuele netwerk (of aangesloten op hetzelfde netwerk als virtuele) alleen stroom wordt via het virtuele netwerk.  Uitgaand verkeer naar persoonlijke eindpunten zal niet lopen via het openbare Internet.

Een voorbehoud is van toepassing op uitgaand verkeer uit een App-omgeving aan de eindpunten binnen een virtueel netwerk.  App Service omgevingen eindpunten van virtuele machines die zich in **hetzelfde** subnet als de App-omgeving kunnen niet bereiken.  Dit moet normaal niet een probleem zijn als App serviceomgevingen worden geïmplementeerd in een subnet dat is gereserveerd voor exclusief gebruik door alleen het App-omgeving.

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

## <a name="outbound-connectivity-and-dns-requirements"></a>Uitgaande verbindingen en DNS-vereisten ##
Voor een App-omgeving te laten functioneren, moet de uitgaande toegang tot verschillende eindpunten. Een volledige lijst van de externe eindpunten die wordt gebruikt door een ASE is in de sectie 'Netwerkverbinding vereist' van de [Netwerkconfiguratie voor ExpressRoute](app-service-app-service-environment-network-configuration-expressroute.md#required-network-connectivity) artikel.

App Service omgevingen moeten een geldige DNS-infrastructuur is geconfigureerd voor het virtuele netwerk.  Als die de DNS-configuratie wordt gewijzigd nadat een App-omgeving is gemaakt, bijvoorbeeld ontwikkelaars dat een App-omgeving om op te halen de nieuwe DNS-configuratie afdwingen kunnen.  Rolling omgeving opnieuw opstarten met behulp van het pictogram 'Opnieuw' aan de bovenkant van het blad App-omgeving beheren in de portal te activeren, wordt de omgeving om op te halen de nieuwe DNS-configuratie.

Het is eveneens aanbevolen aangepaste DNS-servers op de vnet instellingen tijdig een App-omgeving te maken.  Als een virtueel netwerk DNS-configuratie wordt gewijzigd terwijl een App-omgeving wordt gemaakt, die leidt tot het mislukken van de App-omgeving maken proces.  Een soortgelijke vein als een aangepaste DNS-server op het andere uiteinde van een VPN-gateway bestaat en de DNS-server niet bereikbaar is of niet beschikbaar is, wordt het proces voor het maken van App-omgeving ook niet.

## <a name="connecting-to-a-sql-server"></a>Verbinding maken met een SQL-Server
Een veelgebruikte configuratie van SQL Server is een eindpunt luisteren op poort 1433:

![SQL Server-Endpoint][SqlServerEndpoint]

Er zijn twee benaderingen voor het beperken van verkeer naar dit eindpunt:


- [Network Access Control Lists] [ NetworkAccessControlLists] (netwerk ACL's)

- [Netwerk-beveiligingsgroepen][NetworkSecurityGroups]


## <a name="restricting-access-with-a-network-acl"></a>Beperken van toegang met een ACL-netwerk

Poort 1433 kan worden beveiligd met behulp van een network access control list.  In het voorbeeld hieronder whitelists client afkomstig zijn van in een virtueel netwerk adressen en blokkeert de toegang tot alle andere clients.

![Voorbeeld van Network Access Control][NetworkAccessControlListExample]

Alle toepassingen die in App-omgeving in hetzelfde virtuele netwerk als de SQL-Server verbinding maken met de SQL Server-exemplaar met behulp van de **VNet interne** IP-adres voor de virtuele machine van SQL Server kan worden.  

De verbindingsreeks voorbeeld verwijst naar de SQL-Server met behulp van het particuliere IP-adres.

    Server=tcp:10.0.1.6;Database=MyDatabase;User ID=MyUser;Password=PasswordHere;provider=System.Data.SqlClient

Hoewel de virtuele machine ook een openbare eindpunt heeft, wordt pogingen om verbinding met het openbare IP-adres afgewezen door de ACL-netwerk. 

## <a name="restricting-access-with-a-network-security-group"></a>Beperken van toegang aan een beveiligingsgroep netwerk
Een alternatieve methode voor het beveiligen van toegang is met een netwerk-beveiligingsgroep.  Beveiligingsgroepen netwerk kunnen worden toegepast op afzonderlijke virtuele machines of op een subnet met virtuele machines.

Een beveiligingsgroep netwerk moet eerst worden gemaakt:

    New-AzureNetworkSecurityGroup -Name "testNSGexample" -Location "South Central US" -Label "Example network security group for an app service environment"

Toegang beperken tot alleen VNet het interne verkeer is zeer eenvoudig met een beveiligingsgroep netwerk.  De standaardregels in een beveiligingsgroep netwerk alleen toestaan toegang vanaf andere netwerkclients in hetzelfde virtuele netwerk.

Hierdoor toegang tot SQL Server te vergrendelen is net zo eenvoudig als een netwerk-beveiligingsgroep met de standaardregels toepassen op beide de virtuele machines met SQL Server of het subnet waarin de virtuele machines.

Het onderstaande voorbeeld van een beveiligingsgroep netwerk toepassing naar het subnet waarin:

    Get-AzureNetworkSecurityGroup -Name "testNSGExample" | Set-AzureNetworkSecurityGroupToSubnet -VirtualNetworkName 'testVNet' -SubnetName 'Subnet-1'
    
Het eindresultaat is een verzameling van regels voor externe toegang, terwijl VNet interne toegang blokkeren:

![Standaardregels netwerk beveiliging][DefaultNetworkSecurityRules]


## <a name="getting-started"></a>Aan de slag
Alle artikelen en hoe-aan de App serviceomgevingen zijn beschikbaar in het [Leesmij-bestand voor Application Service-omgevingen](../app-service/app-service-app-service-environments-readme.md).

Zie [Inleiding tot de App-omgeving] aan de slag met App Service-omgevingen[IntroToAppServiceEnvironment]

Om binnenkomend verkeer op uw App-omgeving beheren Zie [binnenkomend verkeer op een App-omgeving beheren][ControlInboundASE]

Zie voor meer informatie over het platform Azure App Service [Azure App Service][AzureAppService].

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]
 

<!-- LINKS -->
[virtualnetwork]: https://azure.microsoft.com/documentation/articles/virtual-networks-faq/
[ControlInboundTraffic]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-control-inbound-traffic/
[SiteToSite]: https://azure.microsoft.com/documentation/articles/vpn-gateway-site-to-site-create/
[ExpressRoute]: http://azure.microsoft.com/services/expressroute/
[NetworkAccessControlLists]: https://azure.microsoft.com/documentation/articles/virtual-networks-acl/
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[IntroToAppServiceEnvironment]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/ 
[ControlInboundASE]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-control-inbound-traffic/ 

<!-- IMAGES -->
[SqlServerEndpoint]: ./media/app-service-app-service-environment-securely-connecting-to-backend-resources/SqlServerEndpoint01.png
[NetworkAccessControlListExample]: ./media/app-service-app-service-environment-securely-connecting-to-backend-resources/NetworkAcl01.png
[DefaultNetworkSecurityRules]: ./media/app-service-app-service-environment-securely-connecting-to-backend-resources/DefaultNetworkSecurityRules01.png 
