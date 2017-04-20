<properties 
    pageTitle="App-omgeving | Microsoft Azure" 
    description="Wat is een Azure App-omgeving? Een inleiding tot de App-omgeving." 
    keywords="app Azure-omgeving virtueel netwerk netwerken beveiligen"
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

# <a name="app-service-environment-documentation"></a>De Service App omgeving documentatie

Een App-omgeving is een [Premium] [ PremiumTier] service plan optie van Azure App-Service een volledig geïsoleerd en exclusieve omgeving biedt voor het veilig uitvoeren van apps Azure App Service op hoog niveau, met inbegrip van [Web Apps][WebApps], [Mobile Apps][MobileApps], en een [API Apps][APIApps].  

App Service omgevingen zijn ideaal voor de werkbelasting van toepassingen vereisen:

- Zeer hoge schaal
- Isolatie en veilige netwerktoegang

Klanten kunnen meerdere App serviceomgevingen binnen één enkel Azure, maar ook tussen meerdere Azure regio's maken.  Hierdoor App Service omgeving ideaal voor het horizontaal schalen staat minder lagen bij hoge RPS werkbelasting.

App Service omgevingen zijn beperkt tot slechts één klant toepassingen en altijd in een virtueel netwerk worden geïmplementeerd.  Klanten beschikken over verfijnde controle over beide toepassing voor binnenkomend en uitgaand netwerkverkeer met [beveiligingsgroepen netwerk][NetworkSecurityGroups].  Toepassingen kunnen ook snelle beveiligde verbindingen via virtuele netwerken tot bedrijfsresources voor gebouwen maken.

Apps moeten vaak toegang krijgen tot bedrijfsnetwerken zoals interne databases en webservices.  Apps App serviceomgevingen waarop toegang tot bronnen die bereikbaar is via de [Site naar Site] [ SiteToSite] VPN en [Azure ExpressRoute] [ ExpressRoute] verbindingen.

* [Wat is een App-omgeving?](../app-service-web/app-service-app-service-environment-intro.md)
* [Maken van een App-omgeving](../app-service-web/app-service-web-how-to-create-an-app-service-environment.md)
* [Apps maken in een App-omgeving](../app-service-web/app-service-web-how-to-create-a-web-app-in-an-ase.md)
* [Maken en gebruiken van een interne taakverdeling met App Service-omgevingen](../app-service-web/app-service-environment-with-internal-load-balancer.md)
* [Een omgeving van App-Service configureren](../app-service-web/app-service-web-configure-an-app-service-environment.md) 
* [Apps schalen in een App-omgeving](../app-service-web/app-service-web-scale-a-web-app-in-an-app-service-environment.md)
* [Beveiliging van netwerken en architectuur](../app-service-web/app-service-app-service-environment-network-architecture-overview.md)

## <a name="how-tos"></a>Hoe de

[AZURE.INCLUDE [app-service-blueprint-app-service-environment](../../includes/app-service-blueprint-app-service-environment.md)]


## <a name="videos"></a>Video 's
[AZURE.VIDEO azurecon-2015-deploying-highly-scalable-and-secure-web-and-mobile-apps]

[AZURE.VIDEO microsoft-ignite-2015-running-enterprise-web-and-mobile-apps-on-azure-app-service]


<!-- LINKS -->
[PremiumTier]: http://azure.microsoft.com/pricing/details/app-service/
[WebApps]: http://azure.microsoft.com/documentation/articles/app-service-web-overview/
[MobileApps]: http://azure.microsoft.com/documentation/articles/app-service-mobile-value-prop-preview/
[APIApps]: http://azure.microsoft.com/documentation/articles/app-service-api-apps-why-best-platform/
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[SiteToSite]: https://azure.microsoft.com/documentation/articles/vpn-gateway-site-to-site-create/
[ExpressRoute]: http://azure.microsoft.com/services/expressroute/
