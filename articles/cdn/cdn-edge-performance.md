<properties
    pageTitle="Analyseren van de prestaties van de rand in Azure CDN | Microsoft Azure"
    description="Analyseer de prestaties van het knooppunt in Microsoft Azure CDN rand. Rand prestaties Analytics vindt u gedetailleerde informatie verkeer en de bandbreedte gebruik voor de CDN."
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
    ms.topic="article"
    ms.date="07/28/2016"
    ms.author="casoper"/>

# <a name="analyze-edge-node-performance-in-microsoft-azure-cdn"></a>Analyseren van de prestaties van het knooppunt in Microsoft Azure CDN rand

[AZURE.INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]

## <a name="overview"></a>Overzicht
Rand prestaties analytics vindt u gedetailleerde informatie verkeer en de bandbreedte gebruik voor de CDN. Deze informatie kan vervolgens worden gebruikt voor het genereren van statistieken volgen van trends, zodat u inzicht in hoe de activa worden in de cache opgeslagen en geleverd aan de clients. Op zijn beurt Hiermee kunt u een strategie voor het optimaliseren van de levering van de inhoud van uw formulier, en om te bepalen welke problemen moeten worden aangepakt op betere middelen de CDN. Als gevolg hiervan niet alleen wordt u mogelijk gegevens leveringsprestaties te verbeteren, maar worden ook uw CDN-kosten te verlagen.

> [AZURE.NOTE] Alle rapporten maken gebruik van UTC/GMT notatie bij het opgeven van een datum/tijd.

## <a name="reports-and-log-collection"></a>Rapporten en verzamelen

CDN activiteitsgegevens worden verzameld door de rand prestaties Analytics module voordat deze rapporten kunt genereren. Deze collectie gebeurt eenmaal per dag, en heeft betrekking op de activiteit die plaatsvonden tijdens de vorige dag. Dit betekent dat de statistieken van een rapport vertegenwoordigen een steekproef van de statistieken van de dag op het moment dat deze is verwerkt en hoeft niet bevatten de volledige set gegevens voor de huidige dag. De primaire functie van deze rapporten is voor de beoordeling van prestaties. Zij moeten niet worden gebruikt voor facturatie of exacte numerieke statistieken.

> [AZURE.NOTE] De onbewerkte gegevens waaruit de rand prestaties analytische rapporten worden gegenereerd is voor ten minste 90 dagen beschikbaar.

## <a name="dashboard"></a>Dashboard

De rand prestaties Analytics dashboard actuele en historische CDN-verkeer via grafieken en statistieken worden bijgehouden. Dit dashboard gebruiken voor het detecteren van recente en lange termijn trends op de prestaties van CDN verkeer voor uw account.

Dit dashboard bestaat uit:

* Een interactieve grafiek waarmee u de weergave van de belangrijkste statistieken en trends.
* Een tijdlijn met een gevoel van lange termijn patronen voor belangrijke statistieken en trends.
* Belangrijke maatstaven en statistische informatie over het verbetert ons CDN-netwerk verkeer zoals afgemeten aan de algehele prestaties, het gebruik en efficiëntie.

### <a name="accessing-the-edge-performance-dashboard"></a>Toegang tot de rand performance dashboard

1. Klik op de knop **beheren** van de bladeserver CDN-profiel.

    ![Knop CDN profiel blade beheren](./media/cdn-edge-performance/cdn-manage-btn.png)

    De portal CDN management wordt geopend.

2. Wijs het tabblad **Analytics** en vervolgens zweven boven de **Rand leiden Analytics** doel.  Klik op het **Dashboard**.

    De rand knooppunt analytics dashboard wordt weergegeven.

### <a name="chart"></a>Grafiek

Het dashboard bevat een metric wordt bijgehouden over de tijdsperiode die is geselecteerd in de tijdlijn dat direct onder een grafiek.  Een tijdlijn grafieken omhoog naar de laatste twee jaar van CDN activiteit wordt direct onder de grafiek weergegeven.

#### <a name="using-the-chart"></a>Met behulp van de grafiek

* Standaard wordt de efficiëntie van de cache voor de laatste 30 dagen worden uitgezet.
* Deze grafiek wordt gegenereerd op basis van gegevens gesorteerd op een dagelijkse basis.
* Aanwijzen van een dag op de lijngrafiek geeft aan een datum en de waarde van de metrische gegevens op die datum.
* Klik op markeren tijdens het weekend om te schakelen tussen een overlay van lichte grijze verticale balken die tijdens het weekend naar de grafiek vertegenwoordigen. Dit soort overlay is handig voor het identificeren van patronen in het netwerkverkeer via de weekends.
* Klik op weergave één jaar geleden om te schakelen tussen een bedekking van het voorgaande jaar activiteit gedurende dezelfde periode naar de grafiek. Dit type vergelijking geeft inzicht in de lange termijn CDN gebruikspatronen. De bovenste rechterhoek van de grafiek bevat een legenda die de kleurcode voor elke regel grafiek aangeeft.

#### <a name="updating-the-chart"></a>De grafiek wordt bijgewerkt

* Periode: Voer een van de volgende opties:
    * Selecteer de gewenste regio in de tijdlijn. De grafiek wordt bijgewerkt met gegevens die aan de geselecteerde tijdsperiode.
    * Dubbelklik op de grafiek om alle beschikbare historische gegevens tot een maximum van twee jaar weer te geven.
* Metric: Klik op de grafiek-pictogram wordt weergegeven naast de gewenste metric. De grafiek en de tijdlijn met de gegevens voor de metric van de corresponderende vernieuwd.


### <a name="key-metrics-and-statistics"></a>Belangrijke gegevens en statistieken

#### <a name="efficiency-metrics"></a>Efficiëntie metrics

Het doel van deze gegevens is te zien of de efficiëntie van de cache kan worden verbeterd. De belangrijkste voordelen die zijn afgeleid van de efficiëntie van de cache zijn:

* Verminderde werklast voor de oorspronkelijke server, wat tot leiden kan:
    * Betere prestaties van web-server.
    * Lagere operationele kosten.
* Gegevens levering versnelling verbeterd omdat er meer aanvragen rechtstreeks van de CDN worden geserveerd.

Veld | Beschrijving
------|------------
Cache-efficiëntie | Geeft het percentage van de gegevens die worden overgebracht die uit de cache is betekend. Deze metrische maatregelen wanneer een cacheversie van de aangevraagde inhoud rechtstreeks vanaf de CDN (edge-servers) is betekend aan aanvragers (bijvoorbeeld webbrowser)
Frequentie treffers sjablooncache | Geeft het percentage van de aanvragen die zijn opgehaald uit de cache. Deze metrische maatregelen wanneer een cacheversie van de aangevraagde inhoud is betekend of rechtstreeks vanuit de CDN (edge-servers) voor aanvragers (bijv. een webbrowser).
% van de externe Bytes - geen Cache-Config | Geeft het percentage van het verkeer dat van oorsprong servers is betekend aan de CDN (edge-servers) dat niet in de cache door de Cache van de Bypass functie (http-regels-Engine opgeslagen zal).
% van de externe Bytes - Cache is verlopen | Geeft het percentage van het verkeer dat van oorsprong servers is betekend aan de CDN (edge-servers) als gevolg van verouderde inhoud opnieuw te worden gevalideerd.

#### <a name="usage-metrics"></a>Gebruik cijfers

Het doel van deze gegevens is inzicht geven in de volgende kostenbesparende maatregelen:

* Minimaliseren van operationele kosten door de CDN.
* Degressieve uitgaven CDN via cache efficiëntie en compressie.

> [AZURE.NOTE] Verkeer volume nummers staan voor verkeer dat is gebruikt in de berekeningen van de verhoudingen en percentages en een gedeelte van het totale verkeer kan alleen worden weergegeven voor klanten met hoge volumes.

Veld | Beschrijving
------|------------
Ave verzonden Bytes | Geeft het gemiddelde aantal bytes dat is overgedragen voor elk verzoek van de CDN (edge-servers) bediend aan de aanvrager (bijv. een webbrowser).
Geen Cache Config Byte koers | Geeft het percentage van het verkeer van de CDN (edge-servers) bediend aan de aanvrager die niet in de cache door de Cache van de Bypass functie opgeslagen zal (bijvoorbeeld een webbrowser).
Gecomprimeerde Byte tarief | Geeft het percentage van het verkeer dat wordt verzonden vanuit de CDN (edge-servers) voor aanvragers (bijv. een webbrowser) in een gecomprimeerde indeling.
Verzonden bytes | Hiermee geeft de hoeveelheid gegevens in bytes, die van de CDN (edge-servers) zijn geleverd aan de aanvrager (bijv. een webbrowser).  
Bytes In | Geeft u de hoeveelheid gegevens in bytes van de aanvragers (bijv. een webbrowser) verzonden aan de CDN (edge-servers).
Extern bytes | Hiermee geeft de hoeveelheid gegevens in bytes, van oorsprong-servers CDN en de klant wordt verzonden naar de CDN (edge-servers).

#### <a name="performance-metrics"></a>Prestatiegegevens

Het doel van deze gegevens is voor het bijhouden van prestaties CDN voor het verkeer.

Veld | Beschrijving
------|------------
Overdrachtssnelheid | Geeft de gemiddelde snelheid waarmee inhoud van de CDN is overgedragen aan de aanvrager.
Duur | Geeft de gemiddelde tijd in milliseconden die nodig is voor het leveren van een actief aan de aanvrager (bijv. een webbrowser).
Gecomprimeerde aanvraagsnelheid | Geeft het percentage van treffers die vanuit de CDN (edge-servers) zijn geleverd aan de aanvrager (bijv. een webbrowser) in een gecomprimeerde indeling.
4XX Foutfrequentie | Geeft het percentage van treffers die de statuscode 4xx gegenereerd.
5XX Foutfrequentie | Geeft het percentage van treffers die een 5xx statuscode gegenereerd.
Treffers | Hiermee geeft het aantal aanvragen voor inhoud CDN.

#### <a name="secure-traffic-metrics"></a>Beveiligd verkeer statistieken

Het doel van deze gegevens is voor het bijhouden van prestaties van CDN voor HTTPS-verkeer.

Veld | Beschrijving
------|------------
Cache beveiligen efficiëntie | Geeft het percentage van de gegevens uit de cache voor HTTPS-aanvragen die zijn overgedragen. Deze metrische maatregelen wanneer een cacheversie van de aangevraagde inhoud rechtstreeks vanaf de CDN (edge-servers) voor aanvragers (bijv. een webbrowser) via HTTPS is betekend.
Beveiligde overdrachtssnelheid | Geeft de gemiddelde snelheid waarmee inhoud werd overgedragen van de CDN (edge-servers) voor aanvragers (bijvoorbeeld voor webservers) via HTTPS.
Gemiddelde duur van beveiligde | Geeft de gemiddelde tijd in milliseconden die nodig is voor het leveren van een actief aan de aanvrager (bijv. een webbrowser) via HTTPS.
Beveiligde treffers | Geeft het aantal aanvragen voor inhoud CDN HTTPS.
Veilig verzonden Bytes | Geeft aan dat het bedrag van HTTPS-verkeer, in bytes, die van de CDN (edge-servers) zijn geleverd aan de aanvrager (bijv. een webbrowser).

## <a name="reports"></a>Rapporten

Elk rapport in deze module grafieken en statistieken over het gebruik van bandbreedte en verkeer voor verschillende soorten gegevens bevat (bijvoorbeeld HTTP-statuscodes, cache statuscodes, aanvraag-URL, enz.). Deze informatie kan worden gebruikt om dieper dieper hoe inhoud wordt aangeleverd voor de clients en om te stemmen gedrag CDN levering van gegevens versnellen.

### <a name="accessing-the-edge-performance-reports"></a>Toegang tot de voortgangsrapporten van de rand

1. Klik op de knop **beheren** van de bladeserver CDN-profiel.

    ![Knop CDN profiel blade beheren](./media/cdn-edge-performance/cdn-manage-btn.png)

    De portal CDN management wordt geopend.

2. Wijs het tabblad **Analytics** en vervolgens zweven boven de **Rand leiden Analytics** doel.  Klik op **http-groot Object**.

    Het scherm edge knooppunt analytics-rapporten weergegeven.

Rapport | Beschrijving
-------|------------
Dagelijkse samenvatting | Kunt u dagelijks verkeer trends weergeven over een opgegeven periode. Elke staaf in deze grafiek vertegenwoordigt een bepaalde datum. De grootte van de balk geeft het totale aantal treffers die op die datum plaatsgevonden.
Samenvatting per uur | U kunt bekijken per uur verkeer trends gedurende een opgegeven periode. Elke staaf in deze grafiek vertegenwoordigt één uur op een bepaalde datum. De grootte van de balk geeft het totale aantal treffers die tijdens dat uur.
Protocollen | Geeft de verdeling van verkeer tussen de HTTP- en HTTPS-protocollen. Een cirkelgrafiek geeft het percentage van treffers die voor elk type protocol opgetreden.
HTTP-methoden | Hiermee kunt u een snelle idee van waarmee HTTP-methoden worden gebruikt voor het aanvragen van uw gegevens. Meestal zijn de meest voorkomende HTTP-verzoek methoden GET, HEAD en POST. Een cirkelgrafiek geeft het percentage van treffers die voor elk type HTTP-aanvraagmethode opgetreden.
URL 's | Een grafiek waarin de top 10 aangevraagde URL's bevat. Een balk wordt voor elke URL weergegeven. De hoogte van de balk geeft aan hoeveel treffers dat bepaalde URL is gegenereerd via de tijdspanne waarvoor de lijst. Statistieken voor de top 100 van de aangevraagde dat URL 's direct onder deze grafiek worden weergegeven.
CNAME-records | Bevat een grafiek waarin de top 10 CNAME-records gebruikt voor het aanvragen van activa op het tijdstip van een rapport beslaan. Statistieken voor de top 100 van de aangevraagde dat CNAME-records direct onder deze grafiek worden weergegeven.
Oorsprong | Bevat een grafiek waarin de top 10 CDN of klant oorsprong servers waarvan de activa gedurende een opgegeven periode zijn aangevraagd. Statistieken voor de top 100 van de aangevraagde CDN of klant oorsprong-servers direct onder deze grafiek worden weergegeven. Klant oorsprong servers worden geïdentificeerd door de naam van de optie naam van de map.
Geo-POP 's | Geeft aan hoeveel van uw verkeer wordt omgeleid via een bepaalde point-of-presence (POP). De afkorting van drie letters vertegenwoordigt een POP in ons CDN-netwerk.
Clients | Bevat een grafiek waarin de top 10 clients die aangevraagd van activa over een bepaalde periode worden weergegeven. Voor de toepassing van dit rapport worden alle aanvragen die afkomstig van hetzelfde IP-adres zijn worden beschouwd als van dezelfde client. Statistieken voor de top 100 clients worden direct onder deze grafiek weergegeven. Dit rapport is handig voor het bepalen van patronen voor download activiteit voor uw belangrijkste klanten.
Cache status | Geeft een gedetailleerde opsplitsing van cache gedrag die benaderingen onthullen kan voor het verbeteren van de algehele ervaring van de eindgebruiker. Aangezien de snelste prestaties afkomstig zijn van treffers in de cache, kunt u gegevens levering snelheden door Minimalisatie van het aantal Cachemissers en verlopen cachetreffers optimaliseren.
GEEN Details | Bevat een grafiek waarin de 10 URL's voor activa die cache inhoud FRISHEID niet is gecontroleerd gedurende een opgegeven periode. Statistieken voor de 100 URL's voor deze typen elementen worden direct onder deze grafiek weergegeven.
CONFIG_NOCACHE-Details | Een grafiek waarin de top 10 URL's voor activa die zijn niet in de cache aan CDN-configuratie van de klant bevat. Deze typen elementen zijn rechtstreeks vanaf de oorspronkelijke server verzonden. Statistieken voor de 100 URL's voor deze typen elementen worden direct onder deze grafiek weergegeven.
UNCACHEABLE Details | Een grafiek waarin de 10 URL's voor activa die niet in de cache door de verzoek-header-gegevens bevat. Statistieken voor de 100 URL's voor deze typen elementen worden direct onder deze grafiek weergegeven.
TCP_HIT-Details | Bevat een grafiek waarin de 10 URL's voor activa die direct bereikbaar zijn vanuit de cache. Statistieken voor de 100 URL's voor deze typen elementen worden direct onder deze grafiek weergegeven.
TCP_MISS-Details | Een grafiek waarin de 10 URL's voor activa met een status van de cache van de TCP_MISS bevat. Statistieken voor de 100 URL's voor deze typen elementen worden direct onder deze grafiek weergegeven.
TCP_EXPIRED_HIT-Details | Bevat een grafiek waarin de 10 URL's voor verouderde activa die zijn rechtstreeks van de POP wordt weergegeven. Statistieken voor de 100 URL's voor deze typen elementen worden direct onder deze grafiek weergegeven.
TCP_EXPIRED_MISS-Details | Een grafiek waarin de 10 URL's voor verouderde activa waarvoor een nieuwe versie moesten worden opgehaald van de oorspronkelijke server bevat. Statistieken voor de 100 URL's voor deze typen elementen worden direct onder deze grafiek weergegeven.
TCP_CLIENT_REFRESH_MISS-Details | Bevat een staafdiagram waarin de top 10-URL's voor de activa van een originele server vanwege een aanvraag van de client voor Nee-cache zijn opgehaald. Statistieken voor de 100 URL's voor dit soort aanvragen worden direct onder deze grafiek weergegeven.
Client-aanvraagtypen | Geeft het type van de aanvragen die zijn gemaakt met de HTTP-clients (bijvoorbeeld browsers). Dit rapport bevat een cirkelgrafiek waarmee een idee over hoe aanvragen nog worden verwerkt. Bandbreedte en verkeer informatie voor elk aanvraagtype wordt onder de grafiek weergegeven.
Gebruikersagent | Een staafdiagram weergeven top 10 gebruikersagenten voor het aanvragen van uw inhoud via onze CDN bevat. Een gebruikersagent is normaal gesproken een webbrowser, MediaPlayer, of een mobiele telefoon browser. Statistieken voor de top 100 gebruikersagenten worden direct onder deze grafiek weergegeven.
Verwijzende sites | Een staafdiagram weergeven, de top 10 verwijzende sites via onze CDN inhoud bevat. Een referrer is meestal de URL van de webpagina of de bron die is gekoppeld aan de inhoud. Hieronder de grafiek vindt u gedetailleerde informatie voor de top 100 van verwijzende sites.
De compressietypen | Bevat een cirkelgrafiek die uitsplitsing van activa aangevraagde of ze zijn gecomprimeerd door onze servers van de rand. Het percentage van de gecomprimeerde activa is uitgesplitst naar het type compressie gebruikt. Hieronder de grafiek vindt u gedetailleerde informatie voor elk compressietype en status.
Bestandstypen | Een staafdiagram waarin de top 10 typen die zijn aangevraagd via onze CDN voor uw account bevat. Voor de toepassing van dit rapport een bestandstype dat is gedefinieerd door de bestandsextensie van het actief en het Internet-mediatype (bijvoorbeeld HTML \[tekst/html\], .htm \[tekst/html\], .aspx \[tekst/html\], enz.). Hieronder de grafiek vindt u gedetailleerde informatie voor de top 100 bestandstypen.
Unieke bestanden | Bevat een grafiek die het totale aantal unieke activa die zijn aangevraagd op een bepaalde dag over een bepaalde periode worden uitgezet.
Token-verificatie-overzicht | Een cirkeldiagram met een snel overzicht over of opgegeven activa zijn beschermd door verificatie op basis van het Token bevat. Beveiligde elementen worden weergegeven in de grafiek op basis van de resultaten van hun poging tot verificatie.
Token Auth weigeren Details | Bevat een staafdiagram kunt u de top 10 aanvragen die zijn geweigerd vanwege een op tokens gebaseerde verificatie weergeven.
HTTP responscodes | Geeft een opsplitsing van de HTTP-statuscodes (bijvoorbeeld 200 OK, 403 verboden, 404 niet gevonden, enzovoort) die voor uw HTTP-clients zijn geleverd door onze servers van de rand. Een cirkeldiagram kunt u snel nagaan hoe uw activa zijn opgehaald. Gedetailleerde statistische gegevens worden verstrekt voor elke reactie die onder de grafiek.
404-fouten | Bevat een staafgrafiek weer die u kunt bekijken van de top 10 aanvragen dat heeft geresulteerd in een 404 reactiecode niet gevonden.
403 fouten | Bevat een staafdiagram kunt u de top 10 aanvragen waardoor reactiecode 403 weergeven. 403 reactiecode vindt plaats wanneer een aanvraag wordt geweigerd door de server van een klant oorsprong of een edge-server op onze POP.
4XX fouten | Bevat een staafgrafiek weer die u kunt weergeven van de top 10 aanvragen dat heeft geresulteerd in een reactie in het bereik van 400. 403 uitgesloten van dit rapport zijn niet gevonden en 404 responscodes. 4xx reactiecode treedt meestal op wanneer een aanvraag wordt geweigerd als gevolg van een fout bij de client.
504 fouten | Bevat een staafdiagram kunt u de top 10 aanvragen dat heeft geresulteerd in een 504 Gateway time-out antwoordcode weergeven. 504 Gateway time-out reactiecode vindt plaats wanneer een time-out optreedt wanneer een HTTP-proxyserver probeert te communiceren met een andere server. In het geval van onze CDN, reactiecode 504 Gateway time-out treedt meestal op wanneer een edge-server kan geen communicatie met de server van een klant oorsprong is.
502 fouten | Bevat een staafdiagram kunt u de top 10 aanvragen dat heeft geresulteerd in een 502 Ongeldige Gateway-antwoordcode weergeven. 502 Ongeldige Gateway reactiecode treedt op wanneer een protocol HTTP-fout tussen een server en een HTTP-proxy optreedt. In het geval van onze CDN, 502 Ongeldige Gateway reactiecode treedt meestal op wanneer een klant oorsprong server een ongeldige reactie op een edge-server retourneert. Een antwoord is ongeldig als niet kan worden geanalyseerd of onvolledig.
5XX-fouten | Bevat een staafdiagram kunt u de top 10 aanvragen dat heeft geresulteerd in een reactie in het bereik 500 weergeven.  Uitgesloten van dit rapport zijn 502 Ongeldige Gateway en 504 Gateway time-out responscodes.

## <a name="see-also"></a>Zie ook
* [Azure CDN-overzicht](cdn-overview.md)
* [Real-time statistieken in Microsoft Azure CDN](cdn-real-time-stats.md)
* [Standaardgedrag HTTP met behulp van regels-engine](cdn-rules-engine.md)
* [Geavanceerde HTTP-rapporten](cdn-advanced-http-reports.md)
