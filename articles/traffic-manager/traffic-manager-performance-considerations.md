<properties
    pageTitle="Overwegingen bij prestaties voor Azure verkeer Manager | Microsoft Azure"
    description="Inzicht in prestaties op het beheer van verkeer en het testen van de prestaties van uw website wanneer u met beheer van verkeer"
    services="traffic-manager"
    documentationCenter=""
    authors="sdwheeler"
    manager="carmonm"
    editor=""
/>
<tags
    ms.service="traffic-manager"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="10/11/2016"
    ms.author="sewhee"
/>

# <a name="performance-considerations-for-traffic-manager"></a>Overwegingen bij prestaties van Manager voor verkeer

Deze pagina wordt uitgelegd met behulp van beheer van netwerkverkeer overwegingen bij prestaties. Neem het volgende scenario:

U hebt de WestUS- en Oost-Aziatische gebieden exemplaren van uw website. Een van de instanties is de serverstatus controleren voor de sonde verkeer manager mislukt. Verkeer van toepassing is gericht op de gezonde regio. Deze failover wordt verwacht maar prestaties kan een probleem op basis van de latentie van het verkeer nu onderweg naar een afgelegen gebied.

## <a name="how-traffic-manager-works"></a>De werking van beheer van netwerkverkeer

De enige invloed op de prestaties die het beheer van verkeer op uw website kunnen hebben, is de eerste DNS-zoekopdracht. Een DNS-aanvraag voor de naam van uw serviceprofiel verkeer wordt verwerkt door de Microsoft DNS-basisserver die fungeert als host voor de zone trafficmanager.net. Beheer van verkeer wordt gevuld en regelmatig bijgewerkt, van de Microsoft DNS-basisservers op basis van het beleid voor beheer van netwerkverkeer en de resultaten van de sonde. Dus ook tijdens de eerste DNS-zoekopdracht geen DNS-query's verzonden naar verkeer Manager.

Beheer van verkeer bestaat uit verschillende onderdelen: DNS-naam servers, een API service, de opslaglaag en een eindpunt-service te controleren. Als een onderdeel van de service Manager verkeer mislukt, is er geen effect op de DNS-naam die is gekoppeld aan uw serviceprofiel verkeer. De records in de Microsoft DNS-servers blijven ongewijzigd. Eindpunt controleren en bijwerken van DNS-echter niet gebeuren. Daarom kan verkeer Manager niet voor het bijwerken van DNS om te verwijzen naar de failover-site als uw primaire site down gaat.

DNS-naamomzetting is snel en de resultaten in de cache zijn opgeslagen. De client wordt gebruikt voor het herleiden van DNS-servers is afhankelijk van de snelheid van de eerste DNS-zoekopdracht. Een client kan een DNS-zoekopdracht binnen ~ 50 ms meestal worden voltooid. De resultaten van de zoekopdracht opgeslagen in het cachegeheugen voor de duur van de DNS-Time-to-live (TTL). De standaard-TTL voor verkeer Manager is 300 seconden.

Verkeer doorloopt via beheer van verkeer niet. Als de DNS-zoekopdracht is voltooid, is de client een IP-adres voor een exemplaar van uw website. De client maakt rechtstreeks verbinding met dat adres en niet doorgeeft via beheer van verkeer. Het beleid voor beheer van verkeer u heeft geen invloed op de prestaties van DNS. Prestaties routing-methode kan echter een negatief effect hebben de toepassingservaring. Bijvoorbeeld, als uw beleid wordt omgeleid naar een exemplaar dat wordt gehost in Azië-verkeer van Noord-Amerika, mogelijk de netwerkvertraging voor deze sessies een prestatieprobleem.

## <a name="measuring-traffic-manager-performance"></a>Meten van de prestaties van de Manager van verkeer

Er zijn verschillende websites die kunt u inzicht in de prestaties en het gedrag van een serviceprofiel verkeer. Veel van deze sites zijn gratis, maar kunnen beperkingen hebben. Sommige sites bieden een verbeterde controle en rapportage voor een vergoeding.

De hulpprogramma's op deze sites maatregel DNS vertragingstijden en weergave van de omgezette IP-adressen voor client-locaties over de hele wereld. De meeste van deze hulpprogramma's niet in de resultaten van de DNS-cache. Daarom tonen de hulpprogramma's voor de volledige DNS-zoekopdracht telkens als die een test wordt uitgevoerd. Wanneer u vanaf uw eigen client test, worden alleen de volledige DNS-lookup prestaties eenmaal tijdens de duur van de TTL optreden.

## <a name="sample-tools-to-measure-dns-performance"></a>Hulpmiddelen voor het meten van de prestaties van DNS-voorbeeld

- [SolveDNS](http://www.solvedns.com/dns-comparison/)

    SolveDNS biedt een groot aantal hulpprogramma's voor prestaties. De DNS-bestandsvergelijkingsprogramma wordt weergegeven hoe lang het duurt om de DNS-naam omzetten en hoe die zich verhoudt tot andere DNS-service-providers.

- [WebSitePulse](http://www.websitepulse.com/help/tools.php)

    Een van de eenvoudigste's is WebSitePulse. Voer de URL Zie DNS-tijd, de eerste Byte, laatste Byte en andere prestatiestatistieken. U kunt kiezen uit drie verschillende test locaties. In dit voorbeeld ziet u dat de eerste uitvoering blijkt dat DNS-lookup 0.204 seconde heeft.

    ![pulse1](./media/traffic-manager-performance-considerations/traffic-manager-web-site-pulse.png)

    Omdat de resultaten in cache worden opgeslagen, de tweede proef voor hetzelfde eindpunt van het beheer van verkeer de DNS-zoekopdracht wordt 0.002 sec.

    ![pulse2](./media/traffic-manager-performance-considerations/traffic-manager-web-site-pulse2.png)

- [CA App synthetische Monitor](https://asm.ca.com/en/checkit.php)

    Voorheen bekend als het hulpprogramma Watchmouse selectievakje Website, deze site u de DNS-resolutie tijd weergeven uit meerdere geografische regio's tegelijkertijd. Voer de URL Zie DNS-omzetting tijd, tijd en snelheid van verschillende geografische locaties. Deze test gebruiken om te zien welke gehoste service voor verschillende vestigingen over de hele wereld wordt geretourneerd.

    ![pulse1](./media/traffic-manager-performance-considerations/traffic-manager-web-site-watchmouse.png)

- [Pingdom](http://tools.pingdom.com/)

    Dit hulpprogramma biedt prestatiestatistieken voor elk element van een webpagina. Het tabblad pagina-analyse toont het percentage van de tijd besteed aan DNS-zoekopdrachten.

- [Wat is mijn DNS?](http://www.whatsmydns.net/)

    Deze site is een DNS-lookup uit 20 verschillende locaties en de resultaten worden weergegeven op een kaart.

- [Graven van Web-Interface](http://www.digwebinterface.com)

    Deze site bevat dat meer gedetailleerde informatie van DNS-met inbegrip van CNAME-records en A-records. Zorg ervoor dat u het vullen met kleur-uitvoer en 'Statistieken' onder Opties, en selecteer 'Alle' onder de Nameservers.

## <a name="next-steps"></a>Volgende stappen

[Over het beheer van verkeer verkeer routeringsmethoden](traffic-manager-routing-methods.md)

[Test de instellingen voor beheer van netwerkverkeer](traffic-manager-testing-settings.md)

[Bewerkingen op verkeer Manager (REST API Reference)](http://go.microsoft.com/fwlink/?LinkId=313584)

[Azure verkeer Manager-Cmdlets](http://go.microsoft.com/fwlink/p/?LinkId=400769)
