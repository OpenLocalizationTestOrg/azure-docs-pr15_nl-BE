<properties 
    pageTitle="Inleiding tot de App-omgeving" 
    description="Meer informatie over de functie App-omgeving die beveiligde, deel uitmaakt van een VNet, speciale schaaleenheden biedt voor het uitvoeren van uw toepassingen." 
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

# <a name="introduction-to-app-service-environment"></a>Inleiding tot de App-omgeving

## <a name="overview"></a>Overzicht ##
Een App-omgeving is een [Premium] [ PremiumTier] service plan optie van Azure App-Service een volledig geïsoleerd en exclusieve omgeving biedt voor het veilig uitvoeren van apps Azure App Service op hoog niveau, met inbegrip van [Web Apps][WebApps], [Mobile Apps][MobileApps], en een [API Apps][APIApps].  

App Service omgevingen zijn ideaal voor de werkbelasting van toepassingen vereisen:

- Zeer hoge schaal
- Isolatie en veilige netwerktoegang

Klanten kunnen meerdere App serviceomgevingen binnen één enkel Azure, maar ook tussen meerdere Azure regio's maken.  Hierdoor App Service omgeving ideaal voor het horizontaal schalen staat minder lagen bij hoge RPS werkbelasting.

App Service omgevingen zijn beperkt tot slechts één klant toepassingen en altijd in een virtueel netwerk worden geïmplementeerd.  Klanten beschikken over verfijnde controle over zowel de toepassing inkomende en uitgaande netwerkverkeer en toepassingen snelle beveiligde verbindingen kunnen maken via virtuele netwerken tot bedrijfsresources voor gebouwen.

Alle artikelen en hoe-aan de over App serviceomgevingen zijn beschikbaar in het [Leesmij-bestand voor Application Service-omgevingen](../app-service/app-service-app-service-environments-readme.md).

Een overzicht van hoe App serviceomgevingen hoge schaal inschakelen en beveiliging van netwerktoegang, kunt u de [Grondige Kennismaking AzureCon] [ AzureConDeepDive] op App serviceomgevingen!

Voor een uitvoerige op horizontaal schalen met omgevingen met meerdere App Service Zie het artikel op een [oppervlak verdeeld geo app]instellen[GeodistributedAppFootprint].

Als u wilt zien hoe de beveiligingsarchitectuur weergegeven in het AzureCon diep Kennismaking is geconfigureerd, Zie het artikel over het implementeren van een [gelaagde beveiligingsarchitectuur](app-service-app-service-environment-layered-security.md) met App Service-omgevingen.

Apps op App serviceomgevingen hebben hun toegang geregeld door de upstream-apparaten zoals web application firewalls (WAF).  In dit scenario heeft betrekking op het artikel over het [configureren van een WAF voor App-Service-omgevingen](app-service-app-service-environment-web-application-firewall.md) . 

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

## <a name="dedicated-compute-resources"></a>Speciale Compute-bronnen ##
Alle bronnen in een App-omgeving compute zijn speciaal bestemd zijn voor een enkel abonnement en een App-omgeving kan worden geconfigureerd met maximaal vijftig (50) compute bronnen voor exclusief gebruik door een toepassing.

Een App-omgeving bestaat uit een resourcegroep front-end compute als één tot drie werknemer compute resourcegroepen. 

De front-end-toepassingen bevat compute resources die verantwoordelijk is voor de SSL-beëindiging als ook automatische taakverdeling van app verzoeken binnen een App-omgeving. 

Elke werknemer toepassingen bevat compute resources toegewezen aan [Serviceplannen voor App][AppServicePlan], die op zijn beurt een of meer Service voor Azure App apps bevatten.  Omdat er maximaal drie groepen van de andere werknemer in een App-omgeving, kunt u de flexibiliteit om te berekenen voor verschillende resources voor elke werknemer van toepassingen kiezen hebben.  

Bijvoorbeeld, kunt zo u één werknemer toepassingen maken met minder krachtige compute bronnen voor App plannen bedoeld voor ontwikkel- of apps.  Een tweede (of zelfs derde) werknemer toepassingen kunt krachtiger compute middelen die bestemd zijn voor App serviceplannen voor productie toepassingen uitgevoerd.

Zie voor meer informatie over de hoeveelheid compute-bronnen beschikbaar voor groepen van de front-end- en de werknemer [een App-omgeving configureren hoe][HowToConfigureanAppServiceEnvironment].  

Voor meer informatie over de beschikbare bron formaten ondersteund in een omgeving van App-Service te berekenen, raadpleegt u de [Serviceprijzen App] [ AppServicePricing] pagina en bekijk de beschikbare opties voor Service-omgevingen App in de Premium prijzen laag.

## <a name="virtual-network-support"></a>Ondersteuning voor virtuele netwerken ##
Een App-omgeving kunnen worden gemaakt in **beide** een virtueel netwerk Azure Resource Manager, **of** een klassieke distributie model virtueel netwerk ([meer informatie over virtuele netwerken][MoreInfoOnVirtualNetworks]).  Aangezien een App-omgeving altijd in een virtueel netwerk, en meer precies binnen een subnet van een virtueel netwerk bestaat, kunt u gebruikmaken van de beveiligingsfuncties van virtuele netwerken beheren van zowel binnenkomende als uitgaande netwerkcommunicatie.  

Een App-omgeving is een internetverbinding met een openbaar IP-adres of interne gericht met alleen een adres Azure interne Load Balancer (ILB).

U kunt [netwerk-beveiligingsgroepen] [ NetworkSecurityGroups] te beperken inkomende netwerkcommunicatie op het subnet waarop een App-omgeving bevindt.  Zo kunt u apps achter upstream apparaten en services zoals web application firewalls en SaaS netwerkproviders uitvoeren.

Apps moeten vaak ook toegang krijgen tot bedrijfsnetwerken zoals interne databases en webservices.  Vaak wordt deze eindpunten alleen aan interne netwerkverkeer dat binnen een Azure virtueel netwerk beschikbaar te stellen.  Zodra een App-omgeving is verbonden met hetzelfde virtuele netwerk als de interne services, apps in de omgeving er toegang toe hebben, met inbegrip van de eindpunten die bereikbaar is via de [Site naar Site] [ SiteToSite] en [Azure ExpressRoute] [ ExpressRoute] verbindingen.

Voor meer informatie over de werking van App serviceomgevingen met virtuele netwerken en netwerken in gebouwen Neem contact op met de volgende artikelen op de [Netwerkarchitectuur][NetworkArchitectureOverview], [Binnenkomend verkeer regelen][ControllingInboundTraffic], en [Veilig verbinding te maken met de Backends][SecurelyConnectingToBackends]. 

## <a name="getting-started"></a>Aan de slag

Zie [Hoe te maken een App-omgeving] aan de slag met App Service-omgevingen.[HowToCreateAnAppServiceEnvironment]

Alle artikelen en hoe-aan de App serviceomgevingen zijn beschikbaar in het [Leesmij-bestand voor Application Service-omgevingen](../app-service/app-service-app-service-environments-readme.md).

Zie voor meer informatie over het platform Azure App Service [Azure App Service][AzureAppService].

Zie [Network Architecture-overzicht] voor een overzicht van de netwerkarchitectuur App-omgeving[ NetworkArchitectureOverview] artikel.

Zie voor meer informatie over het gebruik van een App-omgeving met ExpressRoute, het volgende artikel in de [Express-Route]en App serviceomgevingen[NetworkConfigDetailsForExpressRoute].

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- LINKS -->
[PremiumTier]: http://azure.microsoft.com/pricing/details/app-service/
[MoreInfoOnVirtualNetworks]: https://azure.microsoft.com/documentation/articles/virtual-networks-faq/
[AppServicePlan]: http://azure.microsoft.com/documentation/articles/azure-web-sites-web-hosting-plans-in-depth-overview/
[HowToCreateAnAppServiceEnvironment]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-an-app-service-environment/
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/
[WebApps]: http://azure.microsoft.com/documentation/articles/app-service-web-overview/
[MobileApps]: http://azure.microsoft.com/documentation/articles/app-service-mobile-value-prop-preview/
[APIApps]: http://azure.microsoft.com/documentation/articles/app-service-api-apps-why-best-platform/
[LogicApps]: http://azure.microsoft.com/documentation/articles/app-service-logic-what-are-logic-apps/
[AzureConDeepDive]:  https://azure.microsoft.com/documentation/videos/azurecon-2015-deploying-highly-scalable-and-secure-web-and-mobile-apps/
[GeodistributedAppFootprint]:  https://azure.microsoft.com/documentation/articles/app-service-app-service-environment-geo-distributed-scale/
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[SiteToSite]: https://azure.microsoft.com/documentation/articles/vpn-gateway-site-to-site-create/
[ExpressRoute]: http://azure.microsoft.com/services/expressroute/
[HowToConfigureanAppServiceEnvironment]:  http://azure.microsoft.com/documentation/articles/app-service-web-configure-an-app-service-environment/
[ControllingInboundTraffic]:  https://azure.microsoft.com/documentation/articles/app-service-app-service-environment-control-inbound-traffic/
[SecurelyConnectingToBackends]:  https://azure.microsoft.com/documentation/articles/app-service-app-service-environment-securely-connecting-to-backend-resources/
[NetworkArchitectureOverview]:  https://azure.microsoft.com/documentation/articles/app-service-app-service-environment-network-architecture-overview/
[NetworkConfigDetailsForExpressRoute]:  https://azure.microsoft.com/documentation/articles/app-service-app-service-environment-network-configuration-expressroute/
[AppServicePricing]: http://azure.microsoft.com/pricing/details/app-service/ 

<!-- IMAGES -->

 
