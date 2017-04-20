<properties
    pageTitle="CDN cachebeleid in Media Services-uitbreiding"
    description="Dit onderwerp geeft een overzicht van een CDN cachebeleid in Media Services-extensie."
    services="media-services,cdn"
    documentationCenter=".NET"
    authors="juliako"
    manager="erikre"
    editor=""/>

<tags
    ms.service="media-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/19/2016"
    ms.author="juliako"/>
 
#<a name="cdn-caching-policy-in-media-services-extension"></a>CDN cachebeleid in Media Services-uitbreiding

Azure Media Services biedt dat HTTP gebaseerde adaptieve Streaming en progressief downloaden. HTTP-gebaseerde streaming is uiterst schaalbare met voordelen van caching in proxy en CDN lagen en caching aan clientzijde. Streaming eindpunten bevat algemene streaming mogelijkheden en configuratie voor HTTP-cache headers. Eindpunten stroomsgewijze HTTP-header Cache-Control ingesteld: maximum leeftijd en verloopt headers. U krijgt meer informatie voor HTTP-cache headers van [W3.org](http://www.w3.org/Protocols/rfc2616/rfc2616-sec13.html).

##<a name="default-caching-headers"></a>Standaard headers cache

Streaming-eindpunten toepassing standaard 3-daagse cache kopteksten voor op aanvraag stroomsgewijs verzenden van gegevens (fragmenten werkelijke media/chunks) en manifest(playlist). Voor live streaming streaming eindpunten toepassing 3-daagse cache kopteksten voor gegevens (fragmenten werkelijke media/chunks) en 2 seconden in de cache-header voor manifest(playlist) aanvragen. Wanneer live programma (live archief) vraag wordt toepassen op verzoek streaming cache koppen.

##<a name="azure-cdn-integration"></a>Azure CDN-integratie

Azure Media Services biedt [geïntegreerde CDN](https://azure.microsoft.com/updates/azure-media-services-now-fully-integrated-with-azure-cdn/) voor streaming eindpunten. Headers Cache-control is van toepassing op dezelfde manier als streaming eindpunten aan CDN ingeschakeld streaming eindpunten. Azure CDN gebruikt streaming eindpunt geconfigureerd cachewaarden om de levensduur van de intern opgeslagen objecten definiëren en deze waarde ook wordt de levering stelt de headers cache. Als met behulp van CDN ingeschakeld streaming eindpunten niet verdient kleine cachewaarden instellen. Kleine waarden verlagen de prestaties en het voordeel van CDN verminderen. Het is niet toegestaan de headers cache kleiner dan 600 seconden voor CDN ingeschakeld streaming eindpunten instellen.

>[AZURE.IMPORTANT] Integratie van de Azure Media Services met Azure CDN is geïmplementeerd op **Azure CDN van Verizon**.  Als u gebruik wenst te **Azure CDN van Akamai** Azure Media Services, moet u [het handmatig configureren van het eindpunt](cdn-create-new-endpoint.md).  Zie voor meer informatie over functies van Azure CDN [CDN-overzicht](cdn-overview.md).

##<a name="configuring-cache-headers-with-azure-media-services"></a>Headers cache met Azure Media Services configureren

U kunt Azure Management portal of Azure Media Services API's headerwaarden cache configureren.

1. Cache-kop met beheer configureren portal Zie [Streaming eindpunten beheren](../media-services/media-services-portal-manage-streaming-endpoints.md) sectie configureren van het eindpunt voor Streaming.
2. Azure Media Services REST API, [StreamingEndpoint](https://msdn.microsoft.com/library/azure/dn783468.aspx#StreamingEndpointCacheControl).
3. Azure Media Services SDK voor .NET, [StreamingEndpointCacheControl eigenschappen](http://go.microsoft.com/fwlink/?LinkId=615302).

##<a name="cache-configuration-precedence-order"></a>Voorkeursvolgorde voor cache-configuratie

1. Azure Media Services geconfigureerd cachewaarde heeft prioriteit boven de standaardwaarde.
2. Als er geen handmatige configuratie, geldt de standaardwaarden.
3. Door standaard 2 seconden cache headers is van toepassing op live streaming manifest(playlist) ongeacht de configuratie van Azure Media of Azure opslag en opheffing van deze waarde is niet beschikbaar.
