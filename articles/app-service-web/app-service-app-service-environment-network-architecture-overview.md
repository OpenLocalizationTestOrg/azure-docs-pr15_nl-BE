<properties 
    pageTitle="Netwerk architectuur-overzicht van App-Service-omgevingen" 
    description="Overzicht van de architectuur van netwerk topologie ofApp Service-omgevingen." 
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

# <a name="network-architecture-overview-of-app-service-environments"></a>Netwerk architectuur-overzicht van App-Service-omgevingen

## <a name="introduction"></a>Inleiding ##
App serviceomgevingen worden altijd gemaakt binnen een subnet van een [virtueel netwerk] [ virtualnetwork] -toepassingen uitgevoerd in een omgeving van App-Service kunnen communiceren met particuliere eindpunten zich binnen dezelfde virtuele netwerktopologie.  Aangezien klanten onderdelen van hun virtuele netwerkinfrastructuur vergrendelen mogelijk, is het belangrijk dat u weet welke soorten netwerk communicatie overdrachten die worden uitgevoerd in een omgeving van App-Service.

## <a name="general-network-flow"></a>Algemene netwerk stroom ##
 
Wanneer een openbare virtueel IP-adres (VIP) App Service omgeving (ASE) voor toepassingen gebruikt, wordt al het binnenkomende verkeer binnenkomt op die openbare VIP.  Dit omvat de HTTP- en HTTPS-verkeer voor apps, maar ook het andere verkeer voor FTP-functionaliteit voor foutopsporing op afstand en Azure beheertaken uit te voeren.  Raadpleeg het artikel over het [beheren van binnenkomend verkeer] voor een volledige lijst van de specifieke poorten (zowel vereist als optioneel) die beschikbaar zijn op de openbare VIP[ controllinginboundtraffic] voor een App-omgeving. 

App Service omgevingen ondersteunen ook de actieve apps die afhankelijk zijn van alleen een virtueel netwerk intern adres, ook wel genoemd een adres ILB (interne taakverdeling).  Op een ILB ingeschakelde ASE, HTTP en HTTPS-verkeer voor apps zoals externe foutopsporing oproepen, op het adres van het ILB aankomt.  Voor de meest voorkomende configuraties ILB ASE aankomt FTP/FTPS verkeer ook op het adres van het ILB.  Azure beheer wordt nog steeds op poorten 454/455 op de openbare VIP van een ILB ingeschakeld maar ASE.

Het onderstaande diagram ziet u een overzicht van de verschillende netwerk voor binnenkomende en uitgaande stromen voor een App-omgeving waarin de toepassingen afhankelijk zijn van een openbare virtueel IP-adres:

![Algemene netwerk stromen][GeneralNetworkFlows]

Een App-omgeving kan communiceren met een groot aantal eindpunten particuliere klant.  Apps in de App-omgeving uitgevoerd kunnen bijvoorbeeld verbinding met database (s) waarop IaaS virtuele machines in dezelfde virtuele netwerktopologie.

>[AZURE.IMPORTANT] Het netwerkdiagram worden weergegeven, worden de 'andere berekenen Resources' in een ander subnet bevindt dan de App-omgeving geïmplementeerd. Implementeren van bronnen in hetzelfde Subnet met het ASE blokkeert ASE verbinding hebben met deze bronnen (met uitzondering van bepaalde intra-ASE routering). Implementeer in plaats daarvan naar een ander Subnet (in het hetzelfde VNET). De App-omgeving zijn vervolgens verbinding maken. Er is geen aanvullende configuratie nodig.

App Service omgevingen communiceren ook met Sql DB en Azure opslag middelen die noodzakelijk zijn voor het beheren en een App-omgeving werken.  Enkele van de Sql- en bronnen die een App-omgeving met communiceert bevinden zich in hetzelfde gebied, als de App-omgeving, terwijl anderen zich in externe Azure regio's bevinden.  Als gevolg hiervan is uitgaande verbinding met het Internet altijd vereist voor een App-omgeving te laten functioneren. 

Aangezien een App-omgeving in een subnet wordt geïmplementeerd, kunnen netwerk-beveiligingsgroepen worden gebruikt om binnenkomend verkeer op het subnet.  Zie voor meer informatie over het beheren van binnenkomend verkeer op een App-omgeving, het volgende [artikel][controllinginboundtraffic].

Raadpleeg voor informatie over het toestaan van uitgaande Internet-verbinding van een App-omgeving, het volgende artikel over het werken met [Express Route][ExpressRoute].  Dezelfde aanpak als beschreven in het artikel is van toepassing als u werkt met verbinding van Site naar Site, en met behulp van gedwongen tunneling.

## <a name="outbound-network-addresses"></a>Uitgaande netwerkadressen ##
Wanneer een Service App omgeving uitgaande gesprekken, wordt een IP-adres altijd gekoppeld aan de uitgaande gesprekken.  Het specifieke IP-adres dat wordt gebruikt, is afhankelijk van of het eindpunt wordt aangeroepen zich in de virtuele netwerktopologie, of buiten de virtuele netwerktopologie bevindt.

Als het eindpunt wordt aangeroepen **buiten** de virtuele netwerktopologie, is het uitgaande adres (aka de uitgaande NAT-adres) wordt gebruikt de openbare VIP van het App-omgeving.  Dit adres vindt u in de gebruikersinterface van de portal voor de App-omgeving in eigenschappen blade.
 
![Uitgaande IP-adres][OutboundIPAddress]

Dit adres kan ook worden bepaald voor ASEs met een openbare VIP alleen door het maken van een app in de App-omgeving en vervolgens een *nslookup* uit te voeren op het adres van de app. Het resulterende IP-adres is zowel de openbare VIP, als de App-omgeving uitgaande NAT-adres.

Als het eindpunt wordt aangeroepen **binnen** de virtuele netwerktopologie, is het uitgaand adres van de aanroepende toepassing het interne IP-adres van de afzonderlijke compute resource waarop de toepassing wordt uitgevoerd.  Er is echter geen permanente toewijzing van virtueel netwerk interne IP-adressen aan apps.  Apps kunnen navigeren tussen verschillende compute resources en de pool van beschikbare compute bronnen in een App-omgeving veranderen kunnen als gevolg van schalen bewerkingen.

Echter, aangezien een App-omgeving altijd in een subnet bevinden is, wordt gegarandeerd dat het interne IP-adres van een compute-bron met een app de CIDR-adresbereik van het subnet altijd zal liggen.  Als gevolg hiervan wanneer toegesneden ACL's of netwerk-beveiligingsgroepen worden gebruikt voor het beveiligen van de toegang tot andere eindpunten in het virtuele netwerk, moet het subnetbereik met de App-omgeving toegang worden verleend.

In het volgende diagram ziet u deze concepten in meer detail:

![Uitgaande netwerkadressen][OutboundNetworkAddresses]

In het bovenstaande diagram:

- Aangezien de openbare VIP van het App-omgeving is 192.23.1.2, dat is de uitgaande IP-adres dat is gebruikt bij het aanroepen van "Internet" eindpunten.
- De CIDR-bereik van het subnet waarin voor de App-omgeving is 10.0.1.0/26.  Andere eindpunten binnen dezelfde virtuele netwerkinfrastructuur zien als van oorsprong uit ergens binnen het adresbereik van dit aanroepen vanuit apps.

## <a name="calls-between-app-service-environments"></a>Gesprekken tussen Service App omgevingen ##
Een meer complexe scenario kan zich voordoen als u meerdere App Service-omgevingen in hetzelfde virtuele netwerk implementeren en maken van uitgaande oproepen vanuit één App-omgeving naar een andere App-omgeving.  Deze soorten cross-gesprekken, ook worden behandeld als 'Internet' aanroepen van App-omgeving.

In het volgende diagram ziet u een voorbeeld van een gelaagde architectuur met apps op een App-omgeving (bij voorbeeld "Voordeur" web apps) apps aanroept voor een tweede App-omgeving (bijv. interne back-end API apps niet bedoeld om te worden toegankelijk vanaf het Internet). 

![Gesprekken tussen Service App omgevingen][CallsBetweenAppServiceEnvironments] 

In het bovenstaande voorbeeld heeft de App-omgeving "ASE één" een uitgaand IP-adres van de 192.23.1.2.  Als op deze toepassing wordt App-omgeving maakt een uitgaand gesprek naar een toepassing uitvoert op een tweede App Service-omgeving ("ASE twee") zich in hetzelfde virtuele netwerk, uitgaande gesprekken worden behandeld als een aanroep van "Internet".  Hierdoor het netwerkverkeer die binnenkomen op de tweede App-omgeving worden weergegeven als gevolg van 192.23.1.2 (dat wil zeggen niet het subnet-adresbereik van het eerste App-omgeving).

Hoewel aanroepen tussen verschillende omgevingen voor App-Service worden behandeld als 'Internet' gesprekken, wanneer beide omgevingen App Service bevinden zich in de Azure regio verkeer blijft in het regionale netwerk Azure en fysiek niet zal lopen via het openbare Internet.  Hierdoor kunt u een beveiligingsgroep netwerk op het subnet van de tweede App-omgeving alleen toestaan dat inkomende oproepen van de eerste App-omgeving (met uitgaande IP-adres is 192.23.1.2), zodat beveiligde communicatie tussen de Service App omgevingen.

## <a name="additional-links-and-information"></a>Als u meer koppelingen en informatie ##
Alle artikelen en hoe-aan de App serviceomgevingen zijn beschikbaar in het [Leesmij-bestand voor Application Service-omgevingen](../app-service/app-service-app-service-environments-readme.md).

Meer informatie over binnenkomende poorten die worden gebruikt door de App serviceomgevingen en inkomend verkeer regelen met behulp van beveiligingsgroepen voor netwerk is beschikbaar [hier][controllinginboundtraffic].

Gegevens over de gebruiker gedefinieerde routes toe te staan uitgaande Internet-toegang tot omgevingen van App-Service is beschikbaar in dit [artikel][ExpressRoute]. 


<!-- LINKS -->
[virtualnetwork]: http://azure.microsoft.com/services/virtual-network/
[controllinginboundtraffic]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-control-inbound-traffic/
[ExpressRoute]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-network-configuration-expressroute/

<!-- IMAGES -->
[GeneralNetworkFlows]: ./media/app-service-app-service-environment-network-architecture-overview/NetworkOverview-1.png
[OutboundIPAddress]: ./media/app-service-app-service-environment-network-architecture-overview/OutboundIPAddress-1.png
[OutboundNetworkAddresses]: ./media/app-service-app-service-environment-network-architecture-overview/OutboundNetworkAddresses-1.png
[CallsBetweenAppServiceEnvironments]: ./media/app-service-app-service-environment-network-architecture-overview/CallsBetweenEnvironments-1.png

