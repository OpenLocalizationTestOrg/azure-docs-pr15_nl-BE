<properties
    pageTitle="Azure CDN caching gedrag van querytekenreeksen aanvragen beheren | Microsoft Azure"
    description="Azure CDN-queryreeks caching besturingselementen hoe bestanden worden opgeslagen moeten wanneer deze querytekenreeksen bevatten."
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

#<a name="controlling-caching-behavior-of-cdn-requests-with-query-strings"></a>Bestuurt caching gedrag van CDN aanvragen met querytekenreeksen

> [AZURE.SELECTOR]
- [Standaard](cdn-query-string.md)
- [Azure CDN premie van Verizon](cdn-query-string-premium.md)

##<a name="overview"></a>Overzicht

Hoe bestanden worden opgeslagen moeten wanneer ze de query-tekenreeks bevatten besturingselementen caching query-tekenreeks.

> [AZURE.IMPORTANT] De Standard en Premium CDN-producten bieden dezelfde querytekenreeks cache-functionaliteit, maar verschilt van de gebruikersinterface.  Dit document beschrijft de interface voor **Azure CDN standaard van Akamai** - en **Azure CDN van Verizon**.  Zie voor query string caching met **Azure CDN premie van Verizon** [cachinggedrag CDN aanvragen met querytekenreeksen met Premium beheren](cdn-query-string-premium.md).

Er zijn drie modi beschikbaar:

- **Querytekenreeksen negeren**: dit is de standaardmodus.  Het knooppunt CDN rand wordt doorgegeven de query-tekenreeks van de aanvrager de oorsprong voor de eerste aanvraag en de cache het activum.  Alle volgende aanvragen voor de activa die bereikbaar zijn vanaf de randknooppunt wordt de query-tekenreeks genegeerd totdat de activa in de cache is verlopen.
- **Bypass caching voor querytekenreeksen-URL**: In deze modus querytekenreeksen aanvragen worden niet in de cache op het knooppunt CDN rand.  Het randknooppunt opgehaald van de activa direct vanaf de oorsprong en wordt doorgegeven aan de aanvrager bij elke aanvraag.
- **Cache van elke unieke URL**: in deze modus elke aanvraag met een queryreeks behandelt als een unieke activa met de eigen cache.  De reactie van de oorsprong voor een aanvraag voor *foo.ashx?q=bar* zou bijvoorbeeld worden in cache bij het randknooppunt en voor de volgende caches met die dezelfde querytekenreeks geretourneerd.  Een aanvraag voor *foo.ashx?q=somethingelse* zou in de cache worden opgeslagen als een afzonderlijk actief met een eigen tijd op live.

##<a name="changing-query-string-caching-settings-for-standard-cdn-profiles"></a>Query-tekenreeks opslaan in de cache-instellingen voor de standaard CDN profielen wijzigen

1. Klik op het CDN eindpunt dat u wilt beheren vanuit de bladeserver CDN-profiel.

    ![CDN profiel blade eindpunten](./media/cdn-query-string/cdn-endpoints.png)

    Hiermee opent u het eindpunt CDN blade.

2. Klik op de knop **configureren** .

    ![Knop CDN profiel blade beheren](./media/cdn-query-string/cdn-config-btn.png)

    Hiermee opent u de configuratie van CDN blade.

3. Selecteer een instelling in de vervolgkeuzelijst **caching gedrag Query-tekenreeks** .

    ![Query-tekenreeks CDN cacheopties](./media/cdn-query-string/cdn-query-string.png)

4. Nadat u uw keuze hebt, klikt u op de knop **Opslaan** .

> [AZURE.IMPORTANT] Wijzigingen in instellingen zijn mogelijk niet onmiddellijk zichtbaar als het duurt om de inschrijving doorgeven via de CDN.  Doorgifte wordt voor <b>Azure CDN van Akamai</b> profielen, meestal binnen één minuut voltooid.  <b>Azure CDN van Verizon</b> profielen, het doorgeven van meestal voltooit binnen 90 minuten, maar in sommige gevallen kan langer duren.
