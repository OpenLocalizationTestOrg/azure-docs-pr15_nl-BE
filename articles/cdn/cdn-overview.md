<properties
    pageTitle="Azure CDN overzicht | Microsoft Azure"
    description="Informatie over wat de Azure inhoud Delivery Network (CDN) is en hoe u kunt gebruiken voor het leveren van inhoud met hoge bandbreedte door caching BLOB's en statische inhoud."
    services="cdn"
    documentationCenter=""
    authors="camsoper"
    manager="erikre"
    editor=""/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="09/30/2016"
    ms.author="casoper"/>

# <a name="overview-of-the-azure-content-delivery-network-cdn"></a>Overzicht van de Azure Content Delivery Network (CDN)

> [AZURE.NOTE] Dit document wordt beschreven wat de Azure Content Delivery Network (CDN) is en hoe het werkt en de functies van elk product Azure CDN.  Als u wilt deze informatie overslaan en ga direct naar een zelfstudie over het maken van een CDN eindpunt Zie [Azure CDN gebruiken](cdn-create-new-endpoint.md).  Als u een overzicht van de huidige CDN knooppunt locaties, Zie [Azure CDN POP locaties](cdn-pop-locations.md).

Azure inhoud Delivery Network (CDN) slaat de statische webinhoud op strategisch geplaatste locaties voor maximale doorvoer voor het leveren van inhoud aan gebruikers.  De CDN biedt ontwikkelaars een globale oplossing voor het leveren van inhoud met hoge bandbreedte door de inhoud op de fysieke knooppunten in cache plaatsen over de hele wereld. 

De voordelen van het gebruik van de CDN cache website activa omvatten:

- Betere prestaties en de gebruiker ervaring voor eindgebruikers, met name bij het gebruik van toepassingen waarbij meerdere omweg nodig zijn om inhoud te laden.
- Grote schaal beter verwerken momentane hoge belasting, zoals aan het begin van een evenement product.
- Distribueren van aanvragen van gebruikers en inhoud van edge-servers, wordt minder verkeer verzonden naar de oorsprong.


## <a name="how-it-works"></a>Hoe het werkt

![CDN-overzicht](./media/cdn-overview/cdn-overview.png)

1. (Els) van een gebruiker een bestand (ook wel een actief) aanvraagt met behulp van een URL met een speciale domeinnaam, zoals `<endpointname>.azureedge.net`.  DNS stuurt de aanvraag naar de beste locatie voor Point of Presence (POP) uitvoeren.  Meestal is dit de POP die geografisch die het dichtst bij de gebruiker.

2. Als de servers van de rand in het POP-het bestand in de cache geen hebt, vraagt de server zijde het bestand vanaf de oorsprong.  De oorsprong is een Web App van Azure, Azure Cloud Service, Azure opslag account of een openbaar toegankelijke webserver.

3. De oorsprong wordt het bestand naar de server rand, met inbegrip van optionele HTTP-headers, met een beschrijving van het bestand Time-to-Live (TTL).

4. Edge-server slaat het bestand en het bestand terug naar de oorspronkelijke aanvrager (Alice).  Het bestand blijft in de cache op de server van de rand totdat de TTL verlopen.  Als de oorsprong niet een TTL-waarde opgeeft, wordt de standaard-TTL zeven dagen.

5. Extra gebruikers kunnen vervolgens hetzelfde bestand dat dezelfde URL met vragen en ook worden doorgestuurd naar dezelfde POP.

6. Als de TTL-waarde voor het bestand niet is verlopen, retourneert de server van de rand het bestand uit de cache.  Dit resulteert in een snellere, meer responsieve gebruikerservaring.


## <a name="azure-cdn-features"></a>Azure CDN-functies

Er zijn drie Azure CDN producten: **Azure CDN standaard van Akamai** **Azure CDN standaard van Verizon**en **Azure CDN premie van Verizon**.  De volgende tabel worden de beschikbare functies bij elk product.

|       | Standaard Akamai | Standaard Verizon | Premium Verizon |
|-------|-----------------|------------------|-----------------|
| Eenvoudige integratie met Azure services zoals [opslag](cdn-create-a-storage-account-with-cdn.md), [Cloud Services](cdn-cloud-service-with-cdn.md), [Web Apps](../app-service-web/cdn-websites-with-cdn.md)en [Media Services](../media-services/media-services-portal-manage-streaming-endpoints.md) | **& #x 2713;** | **& #x 2713;** | **& #x 2713;**|
| Beheer via de [REST API](https://msdn.microsoft.com/library/mt634456.aspx), [.NET](./cdn-app-dev-net.md), [Node.js](./cdn-app-dev-node.md)of [PowerShell](./cdn-manage-powershell.md). | **& #x 2713;** | **& #x 2713;** | **& #x 2713;** |
| HTTPS-ondersteuning | **& #x 2713;** | **& #x 2713;** | **& #x 2713;** |
| Taakverdeling | **& #x 2713;** | **& #x 2713;** | **& #x 2713;** |
| [DDOS](https://www.us-cert.gov/ncas/tips/ST04-015) bescherming | **& #x 2713;** | **& #x 2713;** | **& #x 2713;** |
| IPv4/IPv6 dual-stack | **& #x 2713;** | **& #x 2713;** | **& #x 2713;** |
| [Ondersteuning voor aangepaste domein naam](cdn-map-content-to-custom-domain.md) | **& #x 2713;** | **& #x 2713;** | **& #x 2713;** |
| [Caching van de query-tekenreeks](cdn-query-string.md) | **& #x 2713;** | **& #x 2713;** | **& #x 2713;** |
| [Geo-filtering](cdn-restrict-access-by-country.md) |  | **& #x 2713;** | **& #x 2713;** |
| [Snel wissen](cdn-purge-endpoint.md) | **& #x 2713;** | **& #x 2713;** | **& #x 2713;** |
| [Vooraf laden van activa](cdn-preload-endpoint.md) |  | **& #x 2713;** | **& #x 2713;** |
| [Core analytics](cdn-analyze-usage-patterns.md) |  | **& #x 2713;** | **& #x 2713;** |
| [Ondersteuning voor HTTP/2](https://msdn.microsoft.com/library/mt762901.aspx) | **& #x 2713;**  |  |  |
| [Geavanceerde HTTP-rapporten](cdn-advanced-http-reports.md) | | | **& #x 2713;** |
| [Real-time statistieken](cdn-real-time-stats.md) | | | **& #x 2713;** |
| [Real-time waarschuwingen](cdn-real-time-alerts.md) | | | **& #x 2713;** |
| [Aanpasbare, op regels gebaseerde content delivery engine](cdn-rules-engine.md) | | | **& #x 2713;** |
| Instellingen voor cache-/ veldnamenbestand (met behulp van [regels-engine](cdn-rules-engine.md))  | | | **& #x 2713;** |
| URL omleiding/herschrijven (met behulp van [regels-engine](cdn-rules-engine.md)) | | | **& #x 2713;** |
| Regels voor mobiel apparaat (met behulp van [regels-engine](cdn-rules-engine.md))  | | | **& #x 2713;** |

>[AZURE.TIP] Is er een functie die u zou willen zien in Azure CDN?  [Geef ons feedback](https://feedback.azure.com/forums/169397-cdn)! 

## <a name="next-steps"></a>Volgende stappen

Zie aan de slag met CDN [Via Azure CDN](./cdn-create-new-endpoint.md).

Als u een bestaande CDN-klant bent, kunt u nu uw CDN eindpunten of via de [portal van Microsoft Azure](https://portal.azure.com) [PowerShell](cdn-manage-powershell.md)beheren.

Overzicht van de CDN in actie, check de [video van onze sessie Build 2016](https://azure.microsoft.com/documentation/videos/build-2016-leveraging-the-new-azure-cdn-apis-to-build-wicked-fast-applications/).

Informatie over het automatiseren van Azure CDN met [.NET](./cdn-app-dev-net.md) of [Node.js](./cdn-app-dev-node.md).

Voor prijzen informatie, Zie [CDN prijzen](https://azure.microsoft.com/pricing/details/cdn/).
