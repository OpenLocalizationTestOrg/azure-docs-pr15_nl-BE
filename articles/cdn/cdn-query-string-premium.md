<properties
    pageTitle="Azure CDN premie van Verizon caching gedrag van querytekenreeksen aanvragen beheren | Microsoft Azure"
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

#<a name="controlling-caching-behavior-of-cdn-requests-with-query-strings---premium"></a>Bestuurt caching gedrag van CDN aanvragen met querytekenreeksen - Premium

> [AZURE.SELECTOR]
- [Standaard](cdn-query-string.md)
- [Azure CDN premie van Verizon](cdn-query-string-premium.md)

##<a name="overview"></a>Overzicht

Hoe bestanden worden opgeslagen moeten wanneer ze de query-tekenreeks bevatten besturingselementen caching query-tekenreeks.

> [AZURE.IMPORTANT] De Standard en Premium CDN-producten bieden dezelfde querytekenreeks cache-functionaliteit, maar verschilt van de gebruikersinterface.  Dit document beschrijft de interface voor **Azure CDN premie van Verizon**.  Zie voor query-tekenreeks met **Azure CDN standaard van Akamai** - en **Azure CDN van Verizon**caching, [Controlling cachinggedrag CDN aanvragen met de query-tekenreeks](cdn-query-string.md).

Er zijn drie modi beschikbaar:

- **standaard-cache**: dit is de standaardmodus.  Het knooppunt CDN rand wordt doorgegeven de query-tekenreeks van de aanvrager de oorsprong voor de eerste aanvraag en de cache het activum.  Alle volgende aanvragen voor de activa die bereikbaar zijn vanaf de randknooppunt wordt de query-tekenreeks genegeerd totdat de activa in de cache is verlopen.
- **Nee-cache**: In deze modus querytekenreeksen aanvragen worden niet in de cache op het knooppunt CDN rand.  Het randknooppunt opgehaald van de activa direct vanaf de oorsprong en wordt doorgegeven aan de aanvrager bij elke aanvraag.
- **unieke cache**: in deze modus elke aanvraag met een queryreeks behandelt als een unieke activa met de eigen cache.  De reactie van de oorsprong voor een aanvraag voor *foo.ashx?q=bar* zou bijvoorbeeld worden in cache bij het randknooppunt en voor de volgende caches met die dezelfde querytekenreeks geretourneerd.  Een aanvraag voor *foo.ashx?q=somethingelse* zou in de cache worden opgeslagen als een afzonderlijk actief met een eigen tijd op live.

##<a name="changing-query-string-caching-settings-for-premium-cdn-profiles"></a>Query-tekenreeks opslaan in de cache-instellingen voor premium CDN profielen wijzigen

1. Klik op de knop **beheren** van de bladeserver CDN-profiel.

    ![Knop CDN profiel blade beheren](./media/cdn-query-string-premium/cdn-manage-btn.png)

    De portal CDN management wordt geopend.

2. Wijs het tabblad **Http-groot** en vervolgens zweven over het doel van de **Cache-instellingen** .  Klik op de **Query-tekenreeks in de cache opslaan**.

    Cacheopties voor query-tekenreeks worden weergegeven.

    ![Query-tekenreeks CDN cacheopties](./media/cdn-query-string-premium/cdn-query-string.png)

3. Nadat u uw keuze hebt, klikt u op de knop **bijwerken** .


> [AZURE.IMPORTANT] Wijzigingen in instellingen zijn mogelijk niet onmiddellijk zichtbaar als het duurt om de inschrijving doorgeven via de CDN.  <b>Azure CDN van Verizon</b> profielen, het doorgeven van meestal voltooit binnen 90 minuten, maar in sommige gevallen kan langer duren.
