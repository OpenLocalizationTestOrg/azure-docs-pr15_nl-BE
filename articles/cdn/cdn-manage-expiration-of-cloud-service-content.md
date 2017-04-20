<properties
 pageTitle="Het beheer van vervaldatums van Azure Web Apps/Cloud Services, ASP.NET en IIS inhoud in Azure CDN | Microsoft Azure"
 description="Hierin wordt beschreven hoe u de vervaldatum van cloud service in Azure CDN beheren"
 services="cdn"
 documentationCenter=".NET"
 authors="camsoper"
 manager="erikre"
 editor=""/>
<tags
 ms.service="cdn"
 ms.workload="media"
 ms.tgt_pltfrm="na"
 ms.devlang="dotnet"
 ms.topic="article"
 ms.date="09/19/2016"
 ms.author="casoper"/>

# <a name="how-to-manage-expiration-of-azure-web-appscloud-services-aspnet-or-iis-content-in-azure-cdn"></a>Het beheer van vervaldatums van Azure Web Apps/Cloud Services of ASP.NET IIS inhoud in Azure CDN

> [AZURE.SELECTOR]
- [Azure Web Apps/Cloud Services, ASP.NET en IIS](cdn-manage-expiration-of-cloud-service-content.md)
- [Azure blob Storage-service](cdn-manage-expiration-of-blob-content.md)

Bestanden van een willekeurige webserver openbaar toegankelijke oorsprong opgeslagen kunnen worden in Azure CDN totdat de time-to-live (TTL) is verstreken.  De TTL-waarde wordt bepaald door de [header *Cache-Control* ](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9) in de HTTP-reactie vanaf de oorspronkelijke server.  In dit artikel wordt beschreven hoe u in te stellen `Cache-Control` headers voor Azure Web Apps, Azure Cloud Services, ASP.NET-toepassingen en Internet Information Services-sites, die allemaal op dezelfde manier zijn geconfigureerd.

>[AZURE.TIP] U kunt geen TTL-waarde instellen voor een bestand.  In dit geval past Azure CDN u een standaard-TTL van zeven dagen.
>
>Zie de [Azure CDN-overzicht](./cdn-overview.md)voor meer informatie over de werking van Azure CDN snelheid van toegang tot bestanden en andere bronnen.

## <a name="setting-cache-control-headers-in-configuration"></a>De Headers Cache-Control instellen in configuratie

Voor statische inhoud, zoals afbeeldingen en opmaakmodellen, kunt u de updatefrequentie bepalen door de **applicationHost.config** - of **web.config** -bestanden voor uw webtoepassing wijzigen.  Het **system.webServer\staticContent\clientCache** -element in het configuratiebestand stelt de `Cache-Control` koptekst voor uw inhoud. Voor **web.config**, de configuratie-instellingen heeft gevolgen voor alles wat in de map en alle submappen, tenzij opgeheven op het niveau van de submap.  U kunt bijvoorbeeld een standaardwaarde instellen time-to-live in de hoofdmap zijn alle statische inhoud in de cache opgeslagen gedurende 3 dagen, maar hebben een submap die meer variabele inhoud met een instelling van 6 uur.  **ApplicationHost.config**, alle toepassingen op de site worden beïnvloed, maar kunnen worden overschreven in de **web.config** -bestanden in de toepassingen.

De volgende XML-programma's en voorbeeld van instelling **clientCache** geeft u een maximale duur van 3 dagen:  

```xml
<configuration>
    <system.webServer>
        <staticContent>
            <clientCache cacheControlMode="UseMaxAge" cacheControlMaxAge="3.00:00:00" />
        </staticContent>
    </system.webServer>
</configuration>
```

Toegevoegd **UseMaxAge** geven een `Cache-Control: max-age=<nnn>` kop aan de respons op basis van de waarde die is opgegeven in het kenmerk **CacheControlMaxAge** . De indeling van het interval is voor het kenmerk **cacheControlMaxAge** is <days>. <hours>:<min>:<sec>. Zie voor meer informatie op het knooppunt **clientCache** [clientcache <clientCache> ](http://www.iis.net/ConfigReference/system.webServer/staticContent/clientCache).  

## <a name="setting-cache-control-headers-in-code"></a>De Headers Cache-Control instellen in Code

U kunt de CDN caching gedrag via een programma met de eigenschap **HttpResponse.Cache** instellen voor ASP.NET-toepassingen. Zie voor meer informatie over de eigenschap **HttpResponse.Cache** [De eigenschap HttpResponse.Cache](http://msdn.microsoft.com/library/system.web.httpresponse.cache.aspx) en [HttpCachePolicy-klasse](http://msdn.microsoft.com/library/system.web.httpcachepolicy.aspx).  

Als u via programmacode in een ASP.NET-toepassingsinhoud in de cache, controleert u of de inhoud is gemarkeerd als cache door HttpCacheability in te stellen op *openbare*. Zorg er bovendien voor dat de validatie van een cache is ingesteld. Validatie van de cache het laatst gewijzigd kan worden ingesteld door het aanroepen van SetLastModified of een waarde van etag tijdstempel instellen door het aanroepen van SetETag. Desgewenst kunt u ook een verlooptijd voor cache opgeven door het aanroepen van SetExpires of u kunt vertrouwen op de standaard cache methodiek die eerder in dit document worden beschreven.  

Bijvoorbeeld voor cache-inhoud voor één uur, Voeg het volgende toe:  

```csharp
// Set the caching parameters.
Response.Cache.SetExpires(DateTime.Now.AddHours(1));
Response.Cache.SetCacheability(HttpCacheability.Public);
Response.Cache.SetLastModified(DateTime.Now);
```

## <a name="next-steps"></a>Volgende stappen

- [Details over het element **clientCache** lezen](http://www.iis.net/ConfigReference/system.webServer/staticContent/clientCache)
- [Raadpleeg de documentatie van de eigenschap **HttpResponse.Cache**](http://msdn.microsoft.com/library/system.web.httpresponse.cache.aspx) 
- [Lees de documentatie van de **Klasse HttpCachePolicy**](http://msdn.microsoft.com/library/system.web.httpcachepolicy.aspx).  
