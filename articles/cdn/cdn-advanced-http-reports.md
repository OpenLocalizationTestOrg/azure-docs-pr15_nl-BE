<properties
    pageTitle="Azure CDN geavanceerde HTTP-rapporten | Microsoft Azure"
    description="Geavanceerde HTTP-rapporten in Microsoft Azure CDN. Deze rapporten bevatten gedetailleerde informatie over CDN activiteit."
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

# <a name="advanced-http-reports-in-microsoft-azure-cdn"></a>Geavanceerde HTTP-rapporten in Microsoft Azure CDN

## <a name="overview"></a>Overzicht

In dit document worden de geavanceerde HTTP-rapportage in Microsoft Azure CDN. Deze rapporten bevatten gedetailleerde informatie over CDN activiteit.

[AZURE.INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]

## <a name="accessing-advanced-http-reports"></a>Toegang tot geavanceerde HTTP-rapporten

1. Klik op de knop **beheren** van de bladeserver CDN-profiel.

    ![Knop CDN profiel blade beheren](./media/cdn-advanced-http-reports/cdn-manage-btn.png)

    De portal CDN management wordt geopend.

2. Wijs het tabblad **Analytics** en vervolgens zweven over het doel van **Geavanceerde HTTP-rapporten** .  Klik op **http-grote Platform**.

    ![CDN management portal - menu Geavanceerde rapporten](./media/cdn-advanced-http-reports/cdn-advanced-reports.png)

    Opties worden weergegeven.

## <a name="geography-reports-map-based"></a>Geografie rapporten (gebaseerd op kaart)

Er zijn vijf rapporten die gebruik maken van een kaart om aan te geven van de regio's waarvan de inhoud wordt aangevraagd. Deze rapporten zijn wereldkaart, kaart van de Verenigde Staten, Canada kaart, kaart van Europa en Azië-Pacific-kaart.

Elk rapport op basis van het overzicht gerangschikt op geografische entiteiten (dat wil zeggen, landen, Staten en provincies) op het percentage van treffers die afkomstig zijn uit die regio. Bovendien is een kaart die bij het visualiseren van de locaties van waaruit de inhoud wordt aangevraagd. Is het kunnen doen door de kleurcodering van elke regio op basis van het bedrag van de vraag in die regio ervaren. Lichtere gearceerde gebieden geven lagere vraag naar uw inhoud, terwijl de donkerste gedeelten geven hogere niveaus van de vraag naar de inhoud.

Direct onder de kaart vindt u gedetailleerde informatie in verkeer en de bandbreedte voor elke regio. Hierdoor kunt u het totale aantal treffers, het percentage van treffers, de totale hoeveelheid gegevens overgedragen (in GB) en het percentage van de gegevens voor elke regio wordt overgedragen. Een beschrijving van elk van deze statistieken weergeven. Ten slotte, wanneer u de muisaanwijzer over een gebied (dat wil zeggen, land, staat of provincie), de naam en het percentage van treffers die heeft plaatsgevonden in de regio wordt weergegeven als knopinfo.

Hieronder vindt u een korte beschrijving van elk type rapport geografische kaart is gebaseerd.

De naam van rapport | Beschrijving
------------|------------
Wereldkaart | Dit rapport kunt u de wereldwijde vraag naar uw CDN-inhoud weergeven. Elk land is kleur op de wereldkaart om aan te geven van het percentage van treffers die afkomstig van die regio zijn.
Kaart van Verenigde Staten | Dit rapport kunt u de vraag naar de inhoud van uw CDN weergeven in de Verenigde Staten. Elke staat is voorzien van een kleurcode op deze kaart om aan te geven van het percentage van treffers die afkomstig van die regio zijn.
Kaart van Canada | Dit rapport kunt u de vraag naar de inhoud van uw CDN weergeven in Canada. Elke provincie is voorzien van een kleurcode op deze kaart om aan te geven van het percentage van treffers die afkomstig van die regio zijn.
Kaart van Europa | Dit rapport kunt u de vraag naar de inhoud van uw CDN weergeven in Europa. Elk land is voorzien van een kleurcode op deze kaart om aan te geven van het percentage van treffers die afkomstig van die regio zijn.
Azië-Pacific kaart | Dit rapport kunt u de vraag naar de inhoud van uw CDN weergeven in Azië. Elk land is voorzien van een kleurcode op deze kaart om aan te geven van het percentage van treffers die afkomstig van die regio zijn.

## <a name="geography-reports-bar-charts"></a>Geografie rapporten (staafdiagrammen)

Er zijn twee aanvullende rapporten die bepalen op basis van Geografie, boven steden en landen boven. Deze rapporten rangorde van steden en landen, respectievelijk overeenkomstig het aantal treffers dat afkomstig van deze regio's is. Bij dit type rapport wordt gegenereerd, wordt een staafdiagram geven aan de top 10 steden of landen die inhoud aangevraagd over een specifiek platform. Deze balk grafiek kunt u snel inzicht in de regio's die het hoogste aantal aanvragen voor uw inhoud te genereren.

De linkerkant van de grafiek (y-as) geeft aan hoe vaak opgetreden in de opgegeven regio. Direct onder de grafiek (x-as) vindt u een label voor elk van de top 10 regio's.

### <a name="using-the-bar-charts"></a>Met behulp van de staafdiagrammen

* Als u de muisaanwijzer op een balk, worden de naam en het totale aantal treffers dat is opgetreden in de regio als knopinfo weergegeven.
* De knopinfo voor het rapport boven steden identificeert een stad door de naam, provincie en Landafkorting.
* Als de plaats of regio (dat wil zeggen, provincie) waarvan een aanvraag afkomstig is niet kan worden vastgesteld, wordt deze vervolgens aangegeven dat onbekend zijn. Als het land is onbekend en vervolgens twee vraagtekens (dat wil zeggen??), worden weergegeven.
* Een rapport kan bevatten metrics voor 'Europa' of "Azië/Pacific." Deze items zijn niet bedoeld voor statistische informatie op alle IP-adressen in deze regio's. In plaats daarvan wordt deze alleen van toepassing op aanvragen die afkomstig zijn van IP-adressen die zijn verdeeld over Europa of Azië in plaats van naar een bepaalde plaats of land.

De gegevens die is gebruikt voor het genereren van het staafdiagram kunnen eronder worden weergegeven. Er vindt u het totale aantal treffers, het percentage van treffers, de hoeveelheid gegevens overgedragen (in GB), en het percentage van de gegevens overgedragen voor de regio's van de top 250. Een beschrijving van elk van deze statistieken weergeven.

Voor beide typen rapporten hieronder vindt u een korte beschrijving.

De naam van rapport | Beschrijving
------------|------------
Top steden | In dit rapport worden gerangschikt op steden volgens het aantal treffers dat afkomstig is uit die regio.
Top landen | In dit rapport worden gerangschikt op landen overeenkomstig het aantal treffers dat afkomstig is uit die regio.

## <a name="daily-summary"></a>Dagelijkse samenvatting

De dagelijkse samenvatting rapport kunt u het totale aantal treffers en de gegevens die worden overgebracht via een bepaald platform op een dagelijkse basis te bekijken. Deze gegevens kunnen worden gebruikt om snel CDN activiteit patronen te onderscheiden. Bijvoorbeeld, kunt in deze lijst u vaststellen welke dagen ervaren hoger of lager dan verwachte verkeer.

Bij dit type rapport wordt gegenereerd, biedt een staafdiagram een visuele indicatie dat het bedrag van de platform-specifieke vraag ervaren op een dagelijkse basis gedurende de periode waarvoor de lijst. Daartoe zal door een balk voor elke dag in het rapport weer te geven. Bijvoorbeeld 'Afgelopen Week' de periode selecteren aangeroepen, genereert een staafdiagram met zeven balken. Elke balk geeft aan het totale aantal treffers op die dag ervaren.

De linkerkant van de grafiek (y-as) geeft aan hoe vaak opgetreden op de opgegeven datum. Direct onder de grafiek (x-as), zult u een label die de datum aangeeft (notatie: jjjj-MM-DD) voor elke dag in het rapport opgenomen.

> [AZURE.TIP] Als u de muisaanwijzer op een balk, wordt het totale aantal treffers dat is opgetreden op die datum weergegeven als knopinfo.

De gegevens die is gebruikt voor het genereren van het staafdiagram kunnen eronder worden weergegeven. Er vindt u het totale aantal treffers en de hoeveelheid gegevens die worden overgebracht (in GB) voor elke dag waarvoor de lijst.

## <a name="by-hour"></a>Per uur

Het rapport per uur kunt u het totale aantal treffers en de gegevens die worden overgebracht via een bepaald platform op uurbasis weergeven. Deze gegevens kunnen worden gebruikt om snel CDN activiteit patronen te onderscheiden. Bijvoorbeeld, kunt dit rapport u de perioden gedurende de dag die zich hoger of lager dan verwachte verkeer te detecteren.

Bij dit type rapport wordt gegenereerd, wordt een staafdiagram bieden een visuele indicatie dat het bedrag van de platform-specifieke vraag ervaren op uurbasis gedurende de periode waarvoor de lijst. Daartoe zal door een balk voor elk uur waarvoor de lijst weer te geven. Bijvoorbeeld voor het selecteren van een 24-uurs periode genereert een staafdiagram met vierentwintig balken. Elke balk geeft aan het totale aantal treffers ervaren tijdens dat uur.

De linkerkant van de grafiek (y-as) geeft aan hoe vaak opgetreden op het tijdstip dat u opgeeft. Direct onder de grafiek (x-as), zult u een label met de datum en tijd aangeeft (notatie: jjjj-MM-DD hh: mm) voor elk uur in de lijst opgenomen. Tijd in 24-uurs notatie wordt aangegeven en dat is opgegeven met de tijdzone UTC/GMT.

> [AZURE.TIP] Als u de muisaanwijzer op een werkbalk, wordt het totale aantal treffers die tijdens dat uur weergegeven als knopinfo.

De gegevens die is gebruikt voor het genereren van het staafdiagram kunnen eronder worden weergegeven. Er vindt u het totale aantal treffers en de hoeveelheid gegevens die worden overgebracht (in GB) voor elk uur waarvoor de lijst.

## <a name="by-file"></a>Door het bestand

Het rapport per bestand kunt u het bedrag van de vraag en het verkeer via een bepaald platform voor de meest gevraagde activa ontstaan. Bij dit type rapport wordt gegenereerd, wordt op de top 10 meest aangevraagde activa gedurende de opgegeven periode een staafdiagram worden gegenereerd.

> [AZURE.NOTE] Voor de toepassing van dit rapport worden rand CNAME-URL's geconverteerd naar hun equivalent CDN-URL. U kunt een nauwkeurige tally voor het totale aantal treffers dat is gekoppeld aan een actief CDN of CNAME-URL die wordt gebruikt voor het aanvragen van deze rand.

De linkerkant van de grafiek (y-as) geeft aan dat het aantal aanvragen voor elk activum gedurende de opgegeven periode. Direct onder de grafiek (x-as) zult u een label met de naam van het bestand voor elk van de top 10 gevraagde activa.

De gegevens die is gebruikt voor het genereren van het staafdiagram kunnen eronder worden weergegeven. Hier vindt u de volgende informatie voor elk van de top 250 gevraagde elementen: het relatieve pad, het totale aantal treffers, het percentage van treffers, de hoeveelheid gegevens overgedragen (in GB), en het percentage van de gegevens overgedragen.

## <a name="by-file-detail"></a>Door het bestand details

Het rapport door bestand details kunt u het bedrag van de vraag en het verkeer via een bepaald platform voor een bepaald activum ontstaan. Is de optie voor meer informatie in het bestand op de menubalk van dit rapport. Deze optie biedt een overzicht van de meest gevraagde activa op het geselecteerde platform. Om een Detail van het bestand te genereren, moet u de gewenste activa te selecteren via de optie bestand Details voor. Hierna moet een staafdiagram wordt gemeld dat het bedrag van de dagelijkse vraag die deze gegenereerd in de opgegeven periode.

Geeft het totale aantal aanvragen dat een actief op een bepaalde dag opgetreden aan de linkerkant van de grafiek (y-as). Direct onder de grafiek (x-as), zult u een label die de datum aangeeft (notatie: jjjj-MM-DD) voor welke CDN vraag naar het activum is gemeld.

De gegevens die is gebruikt voor het genereren van het staafdiagram kunnen eronder worden weergegeven. Er vindt u het totale aantal treffers en de hoeveelheid gegevens die worden overgebracht (in GB) voor elke dag waarvoor de lijst.

## <a name="by-file-type"></a>Op bestandstype

Het rapport door het bestandstype kunt u om het bedrag van de vraag en het verkeer ontstaan door het bestandstype weer te geven. Bij dit type rapport wordt gegenereerd, wordt een cirkelgrafiek geven het percentage van treffers die worden gegenereerd door de bovenste 10 bestandstypen.

> [AZURE.TIP] Als u de muisaanwijzer op een segment in de cirkelgrafiek, mediatype het Internet van dat bestandstype wordt weergegeven als knopinfo.

De gegevens die is gebruikt voor het genereren van de cirkelgrafiek worden eronder weergegeven. Er vindt u het bestand naam uitbreiding/Internet-mediatype, het totale aantal treffers, het percentage van treffers, de hoeveelheid gegevens overgedragen (in GB) en het percentage van de gegevens voor elk van de typen boven 250 overgebracht.

## <a name="by-directory"></a>Door Directory

Het rapport door de Directory kunt u het bedrag van de vraag en het verkeer ontstaan op een bepaald platform voor inhoud van een specifieke map. Bij dit type rapport wordt gegenereerd, wordt een staafdiagram geven het totale aantal treffers gegenereerd door de inhoud in de top 10.

### <a name="using-the-bar-chart"></a>Met behulp van het staafdiagram

* De muisaanwijzer op een balk om het relatieve pad naar de betreffende map weer te geven.
* Inhoud die is opgeslagen in een submap van een map telt niet mee bij de berekening van vraag door directory. Deze berekening uitsluitend gebruikmaakt van het aantal aanvragen voor inhoud die is opgeslagen in de werkelijke map gegenereerd.
* Voor de toepassing van dit rapport worden rand CNAME-URL's geconverteerd naar hun equivalent CDN-URL. U kunt een nauwkeurige tally voor alle statistieken die zijn gekoppeld aan een actief CDN of CNAME-URL die wordt gebruikt voor het aanvragen van deze rand.

De linkerkant van de grafiek (y-as) geeft het totaal aantal aanvragen voor de inhoud die is opgeslagen in de mappen van uw top 10. Elke staaf in de grafiek vertegenwoordigt een map. Gebruik de kleurcodering schema aan een balk naar een map die wordt vermeld in de sectie boven 250 volledige mappen.

De gegevens die is gebruikt voor het genereren van het staafdiagram kunnen eronder worden weergegeven. Hier vindt u de volgende informatie voor elk van de mappen boven 250: het relatieve pad, het totale aantal treffers, het percentage van treffers, de hoeveelheid gegevens overgedragen (in GB), en het percentage van de gegevens overgedragen.

## <a name="by-browser"></a>Door de Browser

Het rapport door de Browser, kunt u bekijken welke browsers zijn gebruikt voor het aanvragen van inhoud. Bij dit type rapport wordt gegenereerd, wordt een cirkeldiagram geven het percentage aanvragen dat is verwerkt door de top 10 van browsers.

### <a name="using-the-pie-chart"></a>Met behulp van het cirkeldiagram

* De muisaanwijzer op een segment van het cirkeldiagram om de naam en versie van een browser weer te geven.
* Voor de toepassing van dit rapport wordt elke combinatie van unieke browserversie wordt beschouwd als een andere browser.
* Het segment 'Overig' genoemd, geeft het percentage aanvragen dat is verwerkt door andere browsers en versies.

De gegevens die is gebruikt voor het genereren van het cirkeldiagram worden eronder weergegeven. Er vindt u het browser/versienummer, het totale aantal treffers en het percentage van treffers voor elk van de browsers top 250.

## <a name="by-referrer"></a>Door verwijzende site

Het rapport verwijzende site door kunt u de meest bezochte verwijzende sites naar inhoud weergeven op het geselecteerde platform. Een verwijzende site geeft de naam van de host waarvoor een aanvraag is gegenereerd. Bij dit type rapport wordt gegenereerd, wordt een staafdiagram aangegeven van het bedrag van de vraag (dat wil zeggen, hits) gegenereerd door de bovenste 10 verwijzende sites.

De linkerkant van de grafiek (y-as) geeft het totale aantal aanvragen dat een actief voor elke verwijzende site ervaren. Elke balk in de grafiek vertegenwoordigt een verwijzende site. Gebruik de kleurcodering schema aan een balk een vermeld in de sectie boven 250 Referrer referrer.

De gegevens die is gebruikt voor het genereren van het staafdiagram kunnen eronder worden weergegeven. Er vindt u de URL, het totale aantal treffers en het percentage van treffers uit elk van de top 250 verwijzingen gegenereerd.

## <a name="by-download"></a>Door te downloaden

Het rapport door te downloaden kunt u analyseren, patronen voor de meest aangevraagde inhoud downloaden. De bovenkant van het rapport bevat een staafdiagram dat vergelijkt downloads met voltooide downloads voor de bovenste 10 gevraagde activa geprobeerd. Elke staaf is naargelang het om een poging tot downloaden (blauw) of een voltooide download (groen) gekleurd.

> [AZURE.NOTE] Voor de toepassing van dit rapport worden rand CNAME-URL's geconverteerd naar hun equivalent CDN-URL. U kunt een nauwkeurige tally voor alle statistieken die zijn gekoppeld aan een actief CDN of CNAME-URL die wordt gebruikt voor het aanvragen van deze rand.

De linkerkant van de grafiek (y-as) geeft de naam van het bestand voor elk van de top 10 gevraagde activa. Direct onder de grafiek (x-as) vindt u de labels die aangeven van het totale aantal downloads geprobeerd kan niet worden voltooid.

Direct onder het staafdiagram, de volgende informatie weergegeven voor de top 250 gevraagde activa: relatief pad (inclusief de bestandsnaam), het aantal keren dat het volledig is gedownload, het aantal keren dat deze is aangevraagd en het percentage aanvragen dat heeft geresulteerd in een volledige download van.

> [AZURE.TIP] Onze CDN is niet op de hoogte gesteld door een HTTP-client (d.w.z. browser) wanneer een activum volledig is gedownload. Als gevolg hiervan hebben we berekenen of een activum volledig is gedownload volgens statuscodes en bereik aan bytes aanvragen. Het eerste wat dat we zoeken bij deze berekening is of de aanvraag resulteert in een statuscode van 200 OK. Als dat zo is, kijken we op verzoeken om ervoor te zorgen dat zij betrekking hebben op de gehele activa byte-bereik. Ten slotte vergelijken we de hoeveelheid gegevens die worden overgebracht naar de grootte van de aangevraagde actief. Als gegevens die gelijk aan of groter is dan de bestandsgrootte is en het bereik aan bytes aanvragen geschikt voor de activa zijn, zullen de bezoekersteller worden geteld als een volledige download.
>
>Door de interpretatie aard van dit verslag Houd u rekening met de volgende punten die de consistentie en de nauwkeurigheid van dit verslag kunnen wijzigen.
>
>* Patronen in het netwerkverkeer kunnen niet nauwkeurig worden voorspeld bij gebruikersagenten zich anders gedragen. Deze resultaten voltooide downloaden die groter dan 100 zijn %.
>* Activa die van het HTTP-progressieve Download gebruikmaken kunnen niet nauwkeurig worden weergegeven door dit rapport. Dit komt door de gebruikers die op verschillende posities in een video.

## <a name="by-404-errors"></a>Met 404-fouten

Het rapport met 404-fouten kunt u bepalen welk type inhoud die het meest aantal 404 niet gevonden statuscodes worden gegenereerd. De bovenkant van het rapport bevat een staafdiagram voor de top 10 activa waarvoor een statuscode 404 niet gevonden is geretourneerd. Deze balk grafiek vergelijkt het totale aantal aanvragen dat heeft geresulteerd in een 404 niet gevonden-statuscode voor deze activa aanvragen. Elke staaf is gekleurd. Een gele balk wordt gebruikt om aan te geven dat de aanvraag heeft geresulteerd in een statuscode 404 niet gevonden. Een rode balk wordt gebruikt om aan te geven van het totale aantal aanvragen voor het activum.

> [AZURE.NOTE] Voor de toepassing van dit rapport, Let op het volgende:
>
>* Elke aanvraag voor een activum, ongeacht de statuscode staat voor een treffer.
>* Rand CNAME-URL's worden geconverteerd naar hun equivalent CDN-URL. U kunt een nauwkeurige tally voor alle statistieken die zijn gekoppeld aan een actief CDN of CNAME-URL die wordt gebruikt voor het aanvragen van deze rand.

De linkerkant van de grafiek (y-as) geeft de naam van het bestand voor elk van de top 10, gevraagde activa die in een statuscode 404 niet gevonden resulteerde. Direct onder de grafiek (x-as) vindt u de labels die aangeven van het totale aantal aanvragen en het aantal aanvragen dat heeft geresulteerd in een statuscode 404 niet gevonden.

Direct onder het staafdiagram, de volgende informatie weergegeven voor de top 250 gevraagde activa: relatieve pad (inclusief de bestandsnaam), het aantal aanvragen dat heeft geresulteerd in een statuscode 404 niet gevonden, het totale aantal keren dat het activum is aangevraagd en het percentage aanvragen dat heeft geresulteerd in een statuscode 404 niet gevonden.

## <a name="see-also"></a>Zie ook
* [Azure CDN-overzicht](cdn-overview.md)
* [Real-time statistieken in Microsoft Azure CDN](cdn-real-time-stats.md)
* [Standaardgedrag HTTP met behulp van regels-engine](cdn-rules-engine.md)
* [Prestaties analyseren](cdn-edge-performance.md)
