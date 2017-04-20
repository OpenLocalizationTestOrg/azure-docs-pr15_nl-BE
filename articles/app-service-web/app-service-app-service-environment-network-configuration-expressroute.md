<properties 
    pageTitle="Gegevens in de netwerkconfiguratie voor het werken met Express-Route" 
    description="Gegevens in de netwerkconfiguratie voor het uitvoeren van Service-omgevingen App in een virtuele netwerken verbonden met een ExpressRoute Circuit." 
    services="app-service" 
    documentationCenter="" 
    authors="stefsch" 
    manager="nirma" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/14/2016" 
    ms.author="stefsch"/>   

# <a name="network-configuration-details-for-app-service-environments-with-expressroute"></a>Gegevens in de netwerkconfiguratie voor omgevingen met ExpressRoute Service App 

## <a name="overview"></a>Overzicht ##
Klanten kunnen een [Azure ExpressRoute] aansluiten[ ExpressRoute] circuit met hun virtueel netwerkinfrastructuur, waardoor hun netwerk op ruimten uitbreiden naar Azure.  Een App-omgeving kan worden gemaakt in een subnet van deze [virtuele netwerk] [ virtualnetwork] infrastructuur.  Apps op de App-omgeving kunnen vervolgens alleen via de ExpressRoute verbinding tot stand brengen beveiligde verbindingen naar back-end bronnen toegankelijk.  

Een App-omgeving kunnen worden gemaakt in **beide** een virtueel netwerk Azure Resource Manager, **of** een klassieke distributie model virtueel netwerk.  Met een recente wijziging in juni 2016, kunnen ASEs nu ook worden geïmplementeerd in de virtuele netwerken die gebruikmaken van openbare adresbereiken of RFC1918-adresruimten (dat wil zeggen particuliere adressen). 

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

## <a name="required-network-connectivity"></a>Verbinding met het netwerk vereist ##
Zijn er vereisten voor connectiviteit voor App-Service-omgevingen die niet in eerste instantie kan worden voldaan in een virtueel netwerk is verbonden met een ExpressRoute.  App Service omgevingen moeten alle volgende goed:


-  Uitgaande netwerkverbindingen met Azure opslag eindpunten wereldwijd op beide poorten 80 en 443.  Dit omvat de eindpunten zich bevinden in hetzelfde gebied, als de App-omgeving, alsmede opslag eindpunten zich in **andere** gebieden Azure.  Azure opslag eindpunten oplossen onder de volgende DNS-domeinen: *table.core.windows.net*, *blob.core.windows.net*, *queue.core.windows.net* en *file.core.windows.net*.  
-  Uitgaande netwerkverbinding met de service bestanden Azure op poort 445.
-  Uitgaande netwerkverbindingen met Sql DB eindpunten zich in hetzelfde gebied, als de App-omgeving.  Eindpunten SQL DB onder het volgende domein oplossen: *database.windows.net*.  Hiervoor moet de toegang tot poorten 1433, 11000 11999 en 14000 14999 openen.  Zie voor meer informatie [dit artikel over het gebruik van Sql-Database V12 poort](../sql-database/sql-database-develop-direct-route-ports-adonet-v12.md).
-  Uitgaande netwerkverbinding met de Azure management vlak eindpunten (ASM zowel ARM eindpunten).  Dit geldt ook voor uitgaande verbindingen met zowel *management.core.windows.net* als *management.azure.com*. 
-  Uitgaande netwerkverbindingen met *ocsp.msocsp.com*, *mscrl.microsoft.com* en *crl.microsoft.com*.  Dit is nodig ter ondersteuning van SSL-functionaliteit.
-  De DNS-configuratie voor het virtuele netwerk van het oplossen van alle domeinen die worden vermeld in de vorige punten en eindpunten kunnen worden.  Eindpunten kunnen niet worden opgelost, App-omgeving maken, niet als bestaande App Service-omgevingen worden gemarkeerd als beschadigd.
-  Uitgaand verkeer op poort 53 is vereist voor communicatie met DNS-servers.
-  Als een aangepaste DNS-server aanwezig is op het andere uiteinde van een VPN-gateway, zijn de DNS-server uit het subnet waarin het App-omgeving bereikbaar. 
-  De uitgaande netwerkpad kan niet via interne corporate proxy's reizen, noch kracht tunnelverbinding tot op de gebouwen worden.  Op die manier verandert het effectieve NAT-adres van het uitgaande netwerkverkeer van het App-omgeving.  De NAT-adres van een App-omgeving uitgaand netwerkverkeer wijzigt, worden fouten tot veel van de hierboven vermelde eindpunten connectiviteit.  Dit resulteert in mislukte pogingen voor App-omgeving maken zoals eerder gezonde App serviceomgevingen wordt gemarkeerd als beschadigd.  
-  Binnenkomend netwerktoegang tot de vereiste poorten voor omgevingen met App-Service moet worden toegestaan, zoals beschreven in dit [artikel][requiredports].

De DNS-vereisten kunnen worden voldaan door te zorgen voor een geldige DNS-infrastructuur is geconfigureerd en onderhouden voor het virtuele netwerk.  Als die de DNS-configuratie wordt gewijzigd nadat een App-omgeving is gemaakt, bijvoorbeeld ontwikkelaars dat een App-omgeving om op te halen de nieuwe DNS-configuratie afdwingen kunnen.  Rolling omgeving opnieuw opstarten met behulp van het pictogram 'Opnieuw opstarten' die zich aan de bovenkant van het blad App-omgeving beheren in [Azure portal] [ NewPortal] , zal de omgeving om op te halen de nieuwe DNS-configuratie.

De netwerkvereisten voor binnenkomende toegang kunnen worden voldaan door het configureren van een [netwerkgroep] [ NetworkSecurityGroups] in de App-omgeving subnet dat de vereiste toegang, zoals beschreven in dit [artikel][requiredports].

## <a name="enabling-outbound-network-connectivity-for-an-app-service-environment"></a>Uitgaande netwerkverbinding wilt inschakelen voor een App-omgeving##
Een zojuist gemaakte ExpressRoute circuit wordt standaard een standaardroute waarmee uitgaande Internet-verbinding.  Met deze configuratie is een App-omgeving verbinding kunnen maken met andere Azure eindpunten.

Een algemene configuratie voor de klant is echter hun eigen standaardroute (0.0.0.0/0), waardoor de uitgaande Internet-verkeer toe in plaats daarvan op ruimten te definiëren.  App serviceomgevingen verbroken dit verkeersstroom altijd omdat het uitgaande verkeer een geblokkeerde op-lokalen wordt of NAT zou een onherkenbare set adressen die niet meer met verschillende Azure eindpunten werken.

De oplossing is voor het definiëren van een (of meer) door de gebruiker gedefinieerd routes (UDRs) op het subnet waarin het App-omgeving.  Een UDR definieert de subnet-specifieke routes die in plaats van de standaardroute wordt ingewilligd.

Indien mogelijk verdient het gebruik van de volgende configuratie:

- De configuratie ExpressRoute 0.0.0.0/0 adverteert en standaard tunnels kracht alle uitgaand verkeer op-ruimten.
- De UDR toegepast op het subnet waarin het App-omgeving bepaalt de 0.0.0.0/0 van het type volgende hop Internet (een voorbeeld hiervan is verder naar beneden in dit artikel).

Het gecombineerde effect van deze stappen is dat subnetniveau UDR heeft voorrang boven de gedwongen ExpressRoute tunneling, zodat uitgaande Internet-toegang vanuit de App-omgeving.

> [AZURE.IMPORTANT] De routes die zijn gedefinieerd in een UDR **moet** worden specifieke voorrang op alle routes die zijn aangekondigd door de configuratie ExpressRoute.  In het volgende voorbeeld gebruikt het adresbereik brede 0.0.0.0/0 en als zodanig kan mogelijk worden per ongeluk overschreven door route-advertisements met meer specifieke adresbereiken.
>
>App-Service-omgevingen worden niet ondersteund met ExpressRoute configuraties die **routes van het openbare peering pad naar de private peering pad cross adverteren**.  ExpressRoute configuraties met openbare peering is geconfigureerd, wordt voor een groot aantal Microsoft Azure IP-adresbereiken route-advertisements ontvangen van Microsoft.  Als de adresbereiken van deze cross geadverteerd op de private peering pad, is het eindresultaat dat alle uitgaande pakketten van het App-omgeving subnet wordt force getunnelde aan infrastructuur in gebouwen van de klant.  Deze overdracht van het netwerk wordt momenteel niet ondersteund met App Service-omgevingen.  Een oplossing voor dit probleem is niet meer cross-reclame routes van het openbare peering pad naar de private peering pad.

Achtergrondinformatie over de gebruiker gedefinieerde routes beschikbaar is in dit [Overzicht][UDROverview].  

Meer informatie over het maken en configureren van routes door de gebruiker gedefinieerd is beschikbaar in deze [Hoe naar handleiding][UDRHowTo].

## <a name="example-udr-configuration-for-an-app-service-environment"></a>Voorbeeld UDR configuratie voor een App-omgeving ##

**Minimumvereisten**

1. Azure Powershell installeren vanaf de [pagina Downloads Azure] [ AzureDownloads] (gedateerd juni 2015 of later).  Onder "Opdrachtregelprogramma's" is er een koppeling 'Installatie' onder 'Windows Powershell' waarmee u de meest recente Powershell cmdlets installeert.

2. Het is raadzaam een uniek subnet wordt gemaakt voor exclusief gebruik door een App-omgeving.  Dit zorgt ervoor dat de UDRs die wordt toegepast op het subnet alleen uitgaand verkeer voor de App-omgeving wordt geopend.
3. **Belangrijk**: Implementeer het App-omgeving niet tot **na** de volgende configuratiestappen worden gevolgd.  Dit zorgt ervoor dat uitgaande netwerkverbinding beschikbaar is voordat u probeert een App-omgeving te implementeren.

**Stap 1: Maak een routetabel met de naam**

Het volgende codefragment maakt een routetabel met de naam 'DirectInternetRouteTable' in de regio West ons Azure:

    New-AzureRouteTable -Name 'DirectInternetRouteTable' -Location uswest

**Stap 2: Maak een of meer routes in de routeringstabel**

U moet een of meer routes toevoegen aan de routetabel uitgaande Internet-toegang mogelijk wilt maken.  

De aanbevolen methode voor het configureren van uitgaande toegang tot het Internet is het definiëren van een route voor 0.0.0.0/0 zoals hieronder wordt weergegeven.
  
    Get-AzureRouteTable -Name 'DirectInternetRouteTable' | Set-AzureRoute -RouteName 'Direct Internet Range 0' -AddressPrefix 0.0.0.0/0 -NextHopType Internet

Houd er rekening mee dat 0.0.0.0/0 is een breed bereik en als zodanig wordt overschreven door de meer specifieke adresbereiken die zijn aangekondigd door de ExpressRoute.  Om de eerder gedane aanbeveling opnieuw doorlopen, moet een UDR met een route 0.0.0.0/0 worden gebruikt in combinatie met een ExressRoute die alleen ook 0.0.0.0/0 adverteert. 

Als alternatief kunt u een complete en bijgewerkte lijst met bereiken door Azure CIDR downloaden.  Het XML-bestand met alle de Azure IP-adresbereiken is beschikbaar via het [Microsoft Download Center][DownloadCenterAddressRanges].  

Maar ziet deze bereiken wijzigen na verloop van tijd die reden periodieke handmatige updates naar de gebruiker gedefinieerde routes om synchroon te houden.  Bovendien moet u de Azure IP-adresbereiken past binnen de limiet van 100 route 'samenvatten' omdat er een standaard bovengrens van 100 routes in een enkele UDR, houden er rekening mee dat UDR routes niet specifieker zijn dan de routes moeten gedefinieerd aangekondigd door uw ExpressRoute.  


**Stap 3: Koppel de routetabel naar het subnet waarin het App-omgeving**

De laatste configuratiestap is het koppelen de routetabel op het subnet waarin het App-omgeving wordt geïmplementeerd.  De volgende opdracht wordt de 'DirectInternetRouteTable' het 'ASESubnet' waarin u uiteindelijk een App-omgeving.

    Set-AzureSubnetRouteTable -VirtualNetworkName 'YourVirtualNetworkNameHere' -SubnetName 'ASESubnet' -RouteTableName 'DirectInternetRouteTable'


**Stap 4: Laatste stappen**

Nadat de tabel is gekoppeld aan het subnet, het verdient aanbeveling eerst testen en het beoogde effect te bevestigen.  Bijvoorbeeld, het implementeren van een virtuele machine in het subnet en bevestigen dat:


- Uitgaand verkeer naar Azure en niet Azure eindpunten eerder in dit artikel **niet** stroomt beneden het circuit ExpressRoute.  Het is zeer belangrijk om te controleren of dit probleem, omdat als uitgaand verkeer uit het subnet is nog steeds gedwongen tunnelverbinding in lokalen, App-omgeving maken altijd mislukt. 
- DNS-lookups voor de eindpunten eerder zijn alle correct kan omzetten. 

Nadat u de bovenstaande stappen zijn bevestigd, moet u de virtuele machine te verwijderen omdat het subnet moet worden 'leeg' op het moment dat de App-omgeving wordt gemaakt.
 
Gaat u verder met het maken van een App-omgeving!

## <a name="getting-started"></a>Aan de slag
Alle artikelen en hoe-aan de App serviceomgevingen zijn beschikbaar in het [Leesmij-bestand voor Application Service-omgevingen](../app-service/app-service-app-service-environments-readme.md).

Zie [Inleiding tot de App-omgeving] aan de slag met App Service-omgevingen[IntroToAppServiceEnvironment]

Zie voor meer informatie over het platform Azure App Service [Azure App Service][AzureAppService].

<!-- LINKS -->
[virtualnetwork]: http://azure.microsoft.com/services/virtual-network/
[ExpressRoute]: http://azure.microsoft.com/services/expressroute/
[requiredports]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-control-inbound-traffic/
[NetworkSecurityGroups]: http://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[UDROverview]: http://azure.microsoft.com/documentation/articles/virtual-networks-udr-overview/
[UDRHowTo]: http://azure.microsoft.com/documentation/articles/virtual-networks-udr-how-to/
[HowToCreateAnAppServiceEnvironment]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-an-app-service-environment/
[AzureDownloads]: http://azure.microsoft.com/en-us/downloads/ 
[DownloadCenterAddressRanges]: http://www.microsoft.com/download/details.aspx?id=41653  
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/
[IntroToAppServiceEnvironment]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[NewPortal]:  https://portal.azure.com
 

<!-- IMAGES -->
