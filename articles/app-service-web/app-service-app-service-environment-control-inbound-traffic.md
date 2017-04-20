<properties 
    pageTitle="Het beheren van binnenkomend verkeer op een App-omgeving" 
    description="Meer informatie over het configureren van regels voor binnenkomend verkeer naar een Service App omgeving netwerk." 
    services="app-service" 
    documentationCenter="" 
    authors="ccompy" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/02/2016" 
    ms.author="stefsch"/>   

# <a name="how-to-control-inbound-traffic-to-an-app-service-environment"></a>Het beheren van binnenkomend verkeer op een App-omgeving

## <a name="overview"></a>Overzicht ##
Een App-omgeving kunnen worden gemaakt in **beide** een virtueel netwerk Azure Resource Manager, **of** een klassieke implementatie [virtueel netwerk]model[virtualnetwork].  Een nieuw virtueel netwerk en een nieuw subnet kunnen worden gedefinieerd op het moment dat een App-omgeving wordt gemaakt.  U kunt ook kunnen een App-omgeving worden gemaakt in een bestaande virtuele netwerk en bestaande subnet.  Met een recente wijziging in juni 2016, kunnen nu ASEs in virtuele netwerken die gebruikmaken van openbare adresbereiken of RFC1918-adresruimten (dat wil zeggen worden geïmplementeerd particuliere adressen).  Voor meer informatie over het maken van een App-omgeving, [het maken van een App-omgeving]Zie[HowToCreateAnAppServiceEnvironment].

Een App-omgeving moet altijd in een subnet worden gemaakt omdat een subnet de grens van een netwerk dat kan worden gebruikt biedt voor het binnenkomende verkeer achter upstream apparaten en services worden vergrendeld dat HTTP- en HTTPS-verkeer wordt alleen geaccepteerd van upstream IP-adressen.

Inkomende en uitgaande netwerkverkeer op een subnet wordt geregeld met behulp van een [beveiligingsgroep netwerk][NetworkSecurityGroups]. Beheren van binnenkomend verkeer, moet beveiligingsregels voor netwerken in een netwerk-beveiligingsgroep maken en vervolgens de beveiliging van het netwerk toewijzen groep het subnet waarin het App-omgeving.

Zodra een beveiligingsgroep netwerk is toegewezen aan een subnet, binnenkomend verkeer voor apps in de App-omgeving is toegestaan/geblokkeerd op basis van het toestaan en weigeren van regels die zijn gedefinieerd in de netwerkgroep.

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

## <a name="network-ports-used-in-an-app-service-environment"></a>Netwerkpoorten die worden gebruikt in een omgeving met App-Service ##
Voordat een binnenkomend netwerkverkeer met een netwerk-beveiligingsgroep te vergrendelen, is het belangrijk om te weten het instellen van de vereiste en optionele netwerkpoorten die worden gebruikt door een App-omgeving.  Per ongeluk uit het verkeer naar bepaalde poorten sluiten kan leiden tot verlies van functionaliteit in een omgeving van App-Service.

Hier volgt een lijst met poorten die worden gebruikt door een App-omgeving. Alle poorten worden **TCP**, tenzij anders wordt duidelijk aangegeven:

- 454: **poort vereist** Azure infrastructuur voor beheer en onderhoud van omgevingen van App-Service via SSL gebruikt.  Verkeer naar deze poort niet geblokkeerd.  Deze poort is altijd gekoppeld aan de openbare VIP van een ASE.
- 455: **poort vereist** Azure infrastructuur voor beheer en onderhoud van omgevingen van App-Service via SSL gebruikt.  Verkeer naar deze poort niet geblokkeerd.  Deze poort is altijd gekoppeld aan de openbare VIP van een ASE.
- 80: standaard poort voor binnenkomend HTTP-verkeer op apps in App serviceplannen voor in een App-omgeving uitgevoerd.  In een ASE ILB ingeschakeld deze poort afhankelijk is van de ILB-adres van de ASE.
- 443: standaard poort voor inkomend SSL-verkeer te apps in App serviceplannen voor in een App-omgeving uitgevoerd.  In een ASE ILB ingeschakeld deze poort afhankelijk is van de ILB-adres van de ASE.
- 21: besturingskanaal voor FTP.  Deze poort kan veilig worden geblokkeerd als FTP wordt niet gebruikt.  In een ASE ILB ingeschakeld kan deze poort worden gebonden aan het adres van het ILB voor een ASE.
- 990: besturingskanaal voor FTPS.  Deze poort kan veilig worden geblokkeerd als FTPS niet wordt gebruikt.  In een ASE ILB ingeschakeld kan deze poort worden gebonden aan het adres van het ILB voor een ASE.
- 10001 10020: kanalen voor FTP-gegevens.  Net als bij het besturingskanaal kunnen deze poorten worden veilig geblokkeerd als FTP wordt niet gebruikt.  In een ASE ILB ingeschakeld kan deze poort afhankelijk zijn van de ASE ILB adres.
- 4016: voor externe foutopsporing bij Visual Studio 2012 gebruikt.  Deze poort kan veilig worden geblokkeerd als de functie niet wordt gebruikt.  In een ASE ILB ingeschakeld deze poort afhankelijk is van de ILB-adres van de ASE.
- 4018: gebruikt voor externe foutopsporing bij Visual Studio 2013.  Deze poort kan veilig worden geblokkeerd als de functie niet wordt gebruikt.  In een ASE ILB ingeschakeld deze poort afhankelijk is van de ILB-adres van de ASE.
- 4020: gebruikt voor externe foutopsporing bij Visual Studio 2015.  Deze poort kan veilig worden geblokkeerd als de functie niet wordt gebruikt.  In een ASE ILB ingeschakeld deze poort afhankelijk is van de ILB-adres van de ASE.

## <a name="outbound-connectivity-and-dns-requirements"></a>Uitgaande verbindingen en DNS-vereisten ##
Voor een App-omgeving te laten functioneren, moet de uitgaande toegang tot verschillende eindpunten. Een volledige lijst van de externe eindpunten die wordt gebruikt door een ASE is in de sectie 'Netwerkverbinding vereist' van de [Netwerkconfiguratie voor ExpressRoute](app-service-app-service-environment-network-configuration-expressroute.md#required-network-connectivity) artikel.

App Service omgevingen moeten een geldige DNS-infrastructuur is geconfigureerd voor het virtuele netwerk.  Als die de DNS-configuratie wordt gewijzigd nadat een App-omgeving is gemaakt, bijvoorbeeld ontwikkelaars dat een App-omgeving om op te halen de nieuwe DNS-configuratie afdwingen kunnen.  Rolling omgeving opnieuw opstarten met behulp van het pictogram 'Opnieuw opstarten' die zich aan de bovenkant van het blad App-omgeving beheren in [Azure portal] [ NewPortal] , zal de omgeving om op te halen de nieuwe DNS-configuratie.

Het is eveneens aanbevolen aangepaste DNS-servers op de vnet instellingen tijdig een App-omgeving te maken.  Als een virtueel netwerk DNS-configuratie wordt gewijzigd terwijl een App-omgeving wordt gemaakt, die leidt tot het mislukken van de App-omgeving maken proces.  Een soortgelijke vein als een aangepaste DNS-server op het andere uiteinde van een VPN-gateway bestaat en de DNS-server niet bereikbaar is of niet beschikbaar is, wordt het proces voor het maken van App-omgeving ook niet.

## <a name="creating-a-network-security-group"></a>Een beveiligingsgroep netwerk maken ##
Voor alle details over hoe de beveiliging van het netwerk werk groepen Zie de volgende [informatie][NetworkSecurityGroups].  De details hieronder touch op markeert van netwerk-beveiligingsgroepen met een focus op configureren en een netwerkgroep beveiliging toepassen op een subnet dat een App-omgeving bevat.

**Opmerking:** Netwerk-beveiligingsgroepen kunnen worden geconfigureerd met grafisch de [Azure Portal](https://portal.azure.com) of via Azure PowerShell.

Netwerk-beveiligingsgroepen worden gemaakt als een zelfstandige entiteit is gekoppeld aan een abonnement. Omdat netwerk-beveiligingsgroepen worden gemaakt in een regio Azure, ervoor te zorgen dat de beveiligingsgroep netwerk wordt gemaakt in dezelfde regio als de App-omgeving.

Het volgende voorbeeld toont een beveiligingsgroep netwerk maken:

    New-AzureNetworkSecurityGroup -Name "testNSGexample" -Location "South Central US" -Label "Example network security group for an app service environment"

Zodra een beveiligingsgroep netwerk is gemaakt, worden een of meer regels voor netwerk toegevoegd.  Omdat de set regels na verloop van tijd veranderen kan, verdient het nummeringsschema gebruikt voor regel prioriteiten waarmee u aanvullende regels invoegen na verloop van tijd gemakkelijk uitspreiden.

In het volgende voorbeeld ziet u een regel die u expliciet toegang tot de poorten voor beheertaken die nodig zijn voor de Azure-infrastructuur verleent te beheren en onderhouden van een App-omgeving.  Houd er rekening mee dat alle beheer van verkeer via SSL loopt en wordt beveiligd door de clientcertificaten, zodat zelfs als de poorten worden geopend door een andere entiteit dan Azure beheerinfrastructuur ontoegankelijk zijn.


    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "ALLOW AzureMngmt" -Type Inbound -Priority 100 -Action Allow -SourceAddressPrefix 'INTERNET'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '454-455' -Protocol TCP
    

Wanneer toegang tot poort 80 en 443 op een App-omgeving achter upstream apparaten of services 'verbergen' te vergrendelen, moet u de upstream IP-adres op te geven.  Bijvoorbeeld als u een web application firewall (WAF), de WAF heeft een eigen IP-adres (of adressen) die wordt gebruikt wanneer via een proxy verkeer naar een downstream App-omgeving.  U moet dit IP-adres gebruiken in de parameter *SourceAddressPrefix* van een regel voor netwerk.

In het onderstaande voorbeeld wordt is het binnenkomende verkeer van een specifiek IP-adres upstream expliciet toegestaan.  Het adres *1.2.3.4* wordt gebruikt als tijdelijke aanduiding voor het IP-adres van een upstream WAF.  Wijzig de waarde overeenkomt met het adres dat wordt gebruikt door de upstream-apparaat of de service.

    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT HTTP" -Type Inbound -Priority 200 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT HTTPS" -Type Inbound -Priority 300 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '443' -Protocol TCP
    
Als u ondersteuning voor FTP-verbinding, kunnen de volgende regels worden gebruikt als een sjabloon toegang verlenen tot de gegevens en de FTP-besturingspoort channel-poorten.  Aangezien FTP een protocol voor stateful is, kunt u mogelijk geen FTP-verkeer routeren via een traditionele HTTP/HTTPS-proxy of firewall-apparaat.  In dit geval moet u de *SourceAddressPrefix* instellen op een andere waarde - bijvoorbeeld het IP-adresbereik van de ontwikkelaar of implementatie machines op waarmee FTP-clients wordt uitgevoerd. 

    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT FTPCtrl" -Type Inbound -Priority 400 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '21' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT FTPDataRange" -Type Inbound -Priority 500 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '10001-10020' -Protocol TCP

(**Opmerking:** poortbereik van de kanaal kan veranderen tijdens de testperiode.)

Als externe foutopsporing bij Visual Studio wordt gebruikt, de volgende regels (demonstratie) om toegang te verlenen.  Er is een aparte regel voor elke ondersteunde versie van Visual Studio, omdat elke versie een andere poort voor foutopsporing op afstand gebruikt.  Zoals met FTP-toegang, externe foutopsporing verkeer mogelijk niet goed door stromen een traditionele WAF of proxyapparaat.  De *SourceAddressPrefix* kan in plaats daarvan worden ingesteld op het IP-adresbereik van Visual Studio-ontwikkelaars computers.

    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT RemoteDebuggingVS2012" -Type Inbound -Priority 600 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '4016' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT RemoteDebuggingVS2013" -Type Inbound -Priority 700 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '4018' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT RemoteDebuggingVS2015" -Type Inbound -Priority 800 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '4020' -Protocol TCP

## <a name="assigning-a-network-security-group-to-a-subnet"></a>Een beveiligingsgroep netwerk toewijzen aan een Subnet ##
Een beveiligingsgroep netwerk heeft een standaardregel die weigert de toegang tot al het externe verkeer.  Het resultaat van de combinatie van de hierboven beschreven regels voor netwerk en de standaardregel voor binnenkomend verkeer te blokkeren is dat alleen verkeer van adres bereiken die zijn gekoppeld aan een actie voor *toestaan* is het mogelijk om verkeer te verzenden naar toepassingen die worden uitgevoerd in een omgeving van App-Service.

Nadat u een beveiligingsgroep netwerk is gevuld met regels, moet deze worden toegewezen aan het subnet waarin het App-omgeving.  De opdracht toewijzing verwijst naar zowel de naam van het virtuele netwerk waarop de Service App omgeving bevindt, evenals de naam van het subnet waarop de App-omgeving is gemaakt.  

In het volgende voorbeeld ziet u een netwerk-beveiligingsgroep die wordt toegewezen aan een subnet en een virtueel netwerk:


    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityGroupToSubnet -VirtualNetworkName 'testVNet' -SubnetName 'Subnet-test'

Zodra de network security group-toewijzing is voltooid (de toewijzing is een langdurige bewerkingen en kan enkele minuten duren), alleen inkomend verkeer *toestaan* toewijzingsregels zullen apps in de App-omgeving met succes bereiken.

Het volgende voorbeeld wordt voor de volledigheid verwijderen en dus de beveiligingsgroep netwerk van het subnet reke‑ te koppelen:


    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Remove-AzureNetworkSecurityGroupFromSubnet -VirtualNetworkName 'testVNet' -SubnetName 'Subnet-test'

## <a name="special-considerations-for-explicit-ip-ssl"></a>Speciale overwegingen bij het expliciete IP-SSL ##
Als een toepassing wordt geconfigureerd met een expliciete IP-SSL (geldt voor ASEs met alleen een openbare VIP), in plaats van het standaard IP-adres van de App-omgeving, HTTP- en HTTPS-verkeer uitmondt in het subnet over een andere reeks poorten dan de poorten 80 en 443.

Het paar afzonderlijke poorten worden gebruikt door elk adres IP-SSL vindt u in de gebruikersinterface van de portal van de App-omgeving details UX blade.  Selecteer 'alle instellingen'--> 'IP-adressen'.  Het blad 'IP-adressen' ziet u een tabel van alle expliciet geconfigureerde IP-SSL-adressen voor App-omgeving, samen met de twee speciale poort die wordt gebruikt voor het routeren van HTTP- en HTTPS-verkeer dat is gekoppeld aan elk adres IP-SSL.  Dit paar poort die worden gebruikt voor de parameters van de DestinationPortRange moet bij het configureren van regels in een beveiligingsgroep netwerk is.

Wanneer een app in een ASE is geconfigureerd voor het gebruik van IP-SSL, wordt externe klanten wordt niet weergegeven en hoeft zich geen zorgen te maken over het paar speciale poort toewijzen.  Verkeer naar de apps zullen normaal naar de geconfigureerde IP-SSL-adres vloeien.  De vertaling op twee speciale poort automatisch gebeurt intern tijdens de laatste fase van het routeren van verkeer in het subnet waarin de ASE. 

## <a name="getting-started"></a>Aan de slag

Zie [Inleiding tot de App-omgeving] aan de slag met App Service-omgevingen[IntroToAppServiceEnvironment]

Alle artikelen en hoe-aan de App serviceomgevingen zijn beschikbaar in het [Leesmij-bestand voor Application Service-omgevingen](../app-service/app-service-app-service-environments-readme.md).

Zie voor meer informatie over apps veilig verbinding maken met back-end-bron van een App-omgeving [veilig verbinding maken met bronnen van de back-end van een App-omgeving][SecurelyConnecttoBackend]

Zie voor meer informatie over het platform Azure App Service [Azure App Service][AzureAppService].

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- LINKS -->
[virtualnetwork]: https://azure.microsoft.com/documentation/articles/virtual-networks-faq/
[HowToCreateAnAppServiceEnvironment]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-an-app-service-environment/
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/
[IntroToAppServiceEnvironment]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[SecurelyConnecttoBackend]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-securely-connecting-to-backend-resources/
[NewPortal]:  https://portal.azure.com  

<!-- IMAGES -->
 
