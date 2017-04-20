<properties
    pageTitle="Analyseren van gebruikspatronen Azure CDN | Microsoft Azure"
    description="U kunt gebruikspatronen voor uw CDN met behulp van de volgende rapporten weergeven: bandbreedte, gegevens overgebracht, Hits, status van de Cache, Cache Hit Ratio, IPV4/IPv6-gegevens overgedragen."
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

# <a name="analyze-azure-cdn-usage-patterns"></a>Analyseren van gebruikspatronen Azure CDN

[AZURE.INCLUDE [cdn-verizon-only](../../includes/cdn-verizon-only.md)]

U kunt de gebruikspatronen weergeven voor uw CDN met behulp van de volgende rapporten:

- Bandbreedte
- Gegevens die worden overgebracht
- Treffers
- Cache status
- Verhouding tussen treffers in de cache
- IPv4/IPv6-gegevens die worden overgebracht

## <a name="accessing-advanced-http-reports"></a>Toegang tot geavanceerde HTTP-rapporten

1. Klik op de knop **beheren** van de bladeserver CDN-profiel.

    ![Knop CDN profiel blade beheren](./media/cdn-reports/cdn-manage-btn.png)

    De portal CDN management wordt geopend.

2. Zweven via het tabblad **Analytics** en wijs de doel **Core rapporten** .  Klik op het gewenste rapport in het menu.

    ![CDN management portal - menu Core rapporten](./media/cdn-reports/cdn-core-reports.png)


## <a name="bandwidth"></a>Bandbreedte

De bandbreedte rapport bestaat uit een grafiek- en tabel die het bandbreedtegebruik voor HTTP en HTTPS gedurende een bepaalde periode aangeeft. U kunt het bandbreedtegebruik weergeven over alle pop CDN's of een bepaalde POP. Hiermee kunt u het verkeer spikes en verdeling over CDN POP's in Mbps weergeven.

- Selecteer alle rand knooppunten Zie verkeer uit alle knooppunten of kies een specifieke regio of een bepaald knooppunt in de vervolgkeuzelijst.
- Datumbereik selecteren om te bekijken van gegevens voor vandaag/deze week/deze maand, enz. of aangepaste datums opgeven en vervolgens klikt u op 'go' om te controleren of dat de selectie wordt bijgewerkt.
- U kunt exporteren en de gegevens op het excel-blad pictogram naast 'go' te downloaden.

Het rapport wordt elke 5 minuten bijgewerkt.

![Rapport van bandbreedte](./media/cdn-reports/cdn-bandwidth.png)

## <a name="data-transferred"></a>Gegevens die worden overgebracht

Dit rapport bestaat uit een grafiek- en tabel die het gebruik van het verkeer voor HTTP en HTTPS gedurende een bepaalde periode aangeeft. Het gebruik van het verkeer weergeven over alle pop CDN's of bepaalde POP 's Hiermee kunt u het verkeer spikes en verdeling over CDN POP's in GB weergeven.

- Alle knooppunten van de rand-verkeer van alle notities zien of een bepaalde regio of knooppunt kiezen uit de vervolgkeuzelijst selecteren.
- Datumbereik selecteren om te bekijken van gegevens voor vandaag/deze week/deze maand, enz. of aangepaste datums opgeven en vervolgens klikt u op 'go' om te controleren of dat de selectie wordt bijgewerkt.
- U kunt exporteren en de gegevens op het excel-blad pictogram naast 'go' te downloaden.

Het rapport wordt elke 5 minuten bijgewerkt.

![Gegevens overgedragen rapport](./media/cdn-reports/cdn-data-transferred.png)

## <a name="hits-status-codes"></a>Treffers (statuscodes)

Dit rapport beschrijft de verdeling van de codes voor de inhoud van uw verzoek. Alle aanvragen voor inhoud genereert een HTTP-statuscode. De statuscode beschrijft hoe rand POP's verwerkt de aanvraag. 2xx statuscodes bijvoorbeeld aangeven dat de aanvraag werd aangeboden aan een client, terwijl een statuscode 4xx geeft aan dat een fout is opgetreden. Zie voor meer informatie over HTTP-statuscode [statuscodes](https://en.wikipedia.org/wiki/List_of_HTTP_status_codes).

- Datumbereik selecteren om te bekijken van gegevens voor vandaag/deze week/deze maand, enz. of aangepaste datums opgeven en vervolgens klikt u op 'go' om te controleren of dat de selectie wordt bijgewerkt.
- U kunt exporteren en de gegevens door te klikken op het excel-blad naast 'go' te downloaden.

![Hits verslag](./media/cdn-reports/cdn-hits.png)

## <a name="cache-statuses"></a>Cache status

Dit rapport beschrijft de verdeling van de cachetreffers en missers in de cache voor de clientaanvraag. Aangezien de snelste prestaties afkomstig zijn van treffers in de cache, kunt u gegevens levering snelheden door Minimalisatie van het aantal Cachemissers en verlopen cachetreffers optimaliseren. Missers in cache kunnen worden verminderd door het configureren van de oorsprong server om te voorkomen dat 'no-cache' antwoordheaders toewijzen door te vermijden query-tekenreeks in de cache opslaan, behalve indien strikt noodzakelijk is en door het vermijden van niet-cache responscodes. Verlopen cache treffers kunnen worden vermeden door middel van een actief's maximale leeftijd zo lang mogelijk tot een minimum beperken van het aantal aanvragen met de oorspronkelijke server.

![Cache status rapport](./media/cdn-reports/cdn-cache-statuses.png)

### <a name="main-cache-statuses-include"></a>Belangrijkste cache statussen zijn:

- TCP_HIT: Van rand bediend. Het object is in de cache en de max-leeftijd niet had overschreden.
- TCP_MISS: Van oorsprong worden bediend. Het object is niet in de cache en het antwoord was in de oorsprong.
- TCP_EXPIRED _MISS: na opnieuw valideren met de oorsprong van oorsprong bediend. Het object is in de cache, maar de max-leeftijd had overschreden. Een gevalideerd met oorsprong heeft geresulteerd in het cache-object wordt vervangen door een nieuwe reactie van oorsprong.
- TCP_EXPIRED _HIT: na opnieuw valideren met de oorsprong van rand bediend. Het object is in de cache, maar de max-leeftijd had overschreden. Een gevalideerd met de oorspronkelijke server heeft geresulteerd in het cache-object wordt niet gewijzigd.

- Datumbereik selecteren om te bekijken van gegevens voor vandaag/deze week/deze maand, enz. of aangepaste datums opgeven en vervolgens klikt u op 'go' om te controleren of dat de selectie wordt bijgewerkt.
- U kunt exporteren en de gegevens op het excel-blad pictogram naast 'go' te downloaden.

### <a name="full-list-of-cache-statuses"></a>Volledige lijst met cache status

- TCP_HIT - deze status wordt vermeld als een aanvraag naar de client rechtstreeks van de POP wordt bediend. Een actief wordt direct van een POP wanneer het wordt in de cache op de POP zich het dichtst bij de client en een geldige time-to-live heeft, of de TTL bediend. TTL-waarde wordt bepaald door de volgende antwoordheaders:

    - Cache-Control: s-maxage
    - Cache-Control: max-age
    - Verloopt

- TCP_MISS - deze status geeft aan dat een cacheversie van het aangevraagde actief niet is gevonden op de POP zich het dichtst bij de client. De activa wordt van een originele server of een bronserver schild gevraagd. Als de oorspronkelijke server of de oorspronkelijke shield server een actief retourneert, wordt het naar de client verzonden en in de cache op de client en de server van de rand. Anders een statuscode van niet-200 (bijv. 403 verboden, 404 niet gevonden, enz.) wordt geretourneerd.

- TCP_EXPIRED _HIT - deze status wordt vermeld als een aanvraag die een actief met een verlopen TTL-waarde, zoals wanneer de activa van max-age is verlopen, gericht aan de client rechtstreeks vanuit de POP is betekend.

    Een verlopen aanvraag resulteert gewoonlijk in een validatieaanvraag met de oorspronkelijke server. Om een TCP_EXPIRED-_HIT optreden, moet de oorspronkelijke server aangeven dat een nieuwere versie van het activum niet bestaat. Dit soort situaties werkt meestal het Cache-Control en Expires headers van het activum.

- TCP_EXPIRED _MISS - deze status wordt vermeld als een nieuwere versie van een verlopen actief in de cache op de client van de POP wordt bediend. Dit gebeurt wanneer de TTL voor een activum in de cache is verlopen (bijv. max leeftijd verlopen) en de oorspronkelijke server retourneert een nieuwere versie van dat actief. Deze nieuwe versie van het activum moet worden verzonden naar de client in plaats van de versie in de cache. Ook dit wordt in de cache op de client en de server van de rand.

- CONFIG_NOCACHE - deze status geeft aan dat een klant-specifieke configuratie aan onze zijde POP voorkomen dat het activum in de cache worden opgeslagen.

- GEEN - deze status geeft aan dat een cache versheid van inhoud is niet gecontroleerd.

- TCP_ CLIENT_REFRESH _MISS - deze status wordt vermeld als een HTTP-client (bijv. browser) zorgt ervoor dat een rand POP voor het ophalen van een nieuwe versie van een activum verlopen vanaf de oorspronkelijke server.

    Standaard onze servers te voorkomen dat een HTTP-client dwingen onze servers van de rand voor het ophalen van een nieuwe versie van de activa van de originele server.

- TCP_ PARTIAL_HIT - deze status wordt vermeld wanneer een treffer voor een activum gedeeltelijk in cache leidt tot de aanvraag van een byte-bereik. Het bereik van aangevraagde bytes wordt naar de client direct van de POP bediend.

- UNCACHEABLE - deze status wordt vermeld wanneer de headers Cache-Control en Expires van het activum aangeeft dat het mag niet worden opgeslagen in een POP- of door de HTTP-client. Dergelijke verzoeken worden bediend vanaf de oorspronkelijke server

## <a name="cache-hit-ratio"></a>Verhouding tussen treffers in de cache

Dit rapport geeft het percentage aanvragen dat in de cache die rechtstreeks van de cache zijn.

Het rapport bevat de volgende gegevens:

- De aangevraagde inhoud in de cache is op de POP zich het dichtst bij de aanvrager.
- De aanvraag is betekend of rechtstreeks vanaf de rand van ons netwerk.
- Het verzoek werd niet moet worden gevalideerd met de originele server.

Niet opgenomen in de lijst:

- Aanvragen die zijn geweigerd vanwege land filteropties.
- Aanvragen voor activa waarvan de koptekst aangeven dat ze mag niet worden opgeslagen. Bijvoorbeeld Cache-Control: private, Cache-Control: Nee-cache of Pragma: Nee-cache-headers wordt voorkomen dat een actief in cache wordt opgeslagen.
- Byte range-aanvragen voor inhoud gedeeltelijk in de cache.

De formule is: (TCP_ HIT / (TREFFER TCP_ + TCP_MISS)) * 100

- Datumbereik selecteren om te bekijken van gegevens voor vandaag/deze week/deze maand, enz. of aangepaste datums opgeven en vervolgens klikt u op 'go' om te controleren of dat de selectie wordt bijgewerkt.
- U kunt exporteren en de gegevens op het excel-blad pictogram naast 'go' te downloaden.


![Rapport van de verhouding tussen treffers in cache](./media/cdn-reports/cdn-cache-hit-ratio.png)

## <a name="ipv4ipv6-data-transferred"></a>IPv4/IPv6-gegevens die worden overgebracht

Dit rapport toont de verdeling van verkeer gebruik in IPV4 en IPV6.

![IPv4/IPv6-gegevens die worden overgebracht](./media/cdn-reports/cdn-ipv4-ipv6.png)

- Datumbereik selecteren om te bekijken van gegevens voor vandaag/deze week/deze maand, enz. of aangepaste datums invoeren.
- Klik vervolgens op 'go' om te controleren of dat de selectie wordt bijgewerkt.


## <a name="considerations"></a>Overwegingen met betrekking tot

Rapporten kunnen alleen worden gegenereerd in de afgelopen 18 maanden.
