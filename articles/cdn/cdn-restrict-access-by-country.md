<properties
    pageTitle="Beperk de toegang tot de inhoud van uw Azure CDN door land | Microsoft Azure"
    description="Informatie over het beperken van toegang tot uw Azure CDN-inhoud met de functie voor het filteren van Geo."
    services="cdn"
    documentationCenter=""
    authors="camsoper, rli"
    manager="erikre"
    editor=""/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/13/2016"
    ms.author="casoper"/>

#<a name="restrict-access-to-your-content-by-country---verizon"></a>Beperk de toegang tot de inhoud van uw land - Verizon

> [AZURE.SELECTOR]
- [Verizon](cdn-restrict-access-by-country.md)
- [Akamai standaard](cdn-restrict-access-by-country-akamai.md)

##<a name="overview"></a>Overzicht

Wanneer een gebruiker de inhoud standaard, wordt de inhoud aangeboden ongeacht waar de gebruiker dit verzoek van gemaakt. In sommige gevallen wilt u mogelijk beperken de toegang tot de inhoud van uw land. In dit onderwerp vindt de functie **Geo-filters** gebruiken om de service te kunnen configureren of de toegang blokkeren door het land.

> [AZURE.IMPORTANT] De Verizon en Akamai producten bieden dezelfde functionaliteit geo filteren, maar verschilt van de gebruikersinterface. Dit document beschrijft de interface voor **Azure CDN standaard van Verizon** en **Azure CDN premie van Verizon**. Zie [toegang tot de inhoud van uw land - Akamai beperken](cdn-restrict-access-by-country-akamai.md)voor geo-filtering met **Azure CDN Standard van Akamai**.

Zie de sectie [overwegingen](cdn-restrict-access-by-country.md#considerations) aan het einde van het onderwerp voor informatie over overwegingen voor het configureren van dit type beperking.  

>[AZURE.NOTE] Nadat de configuratie is ingesteld, wordt toegepast op alle **Azure CDN van Verizon** eindpunten in dit profiel Azure CDN.

![Filteren van land](./media/cdn-filtering/cdn-country-filtering.png)

##<a name="step-1-define-the-directory-path"></a>Stap 1: Het pad naar de map definiëren

Wanneer u een land filter configureert, moet u het relatieve pad naar de locatie waarop gebruikers wel of geen toegang. U kunt toepassen land filtering voor alle bestanden met '/' of mappen geselecteerd door mappaden op te geven.

Voorbeeld van de directory pad filter:

    /                                 
    /Photos/
    /Photos/Strasbourg

##<a name="step-2-define-the-action-block-or-allow"></a>Stap 2: Definieer de actie: blokkeren of toestaan

**Blok:** Gebruikers uit bepaalde landen wordt de toegang geweigerd aan activa van dat pad recursieve gevraagd. Als er geen andere land-filteropties voor die locatie zijn geconfigureerd, vervolgens alle andere gebruikers mogen toegang.

**Toestaan:** Alleen vanuit bepaalde landen kunnen gebruikers toegang tot de activa van dat pad recursieve gevraagd.

##<a name="step-3-define-the-countries"></a>Stap 3: Definieer de landen

Selecteer de landen die u wilt blokkeren of toestaan voor het pad. Zie voor meer informatie [Azure CDN van Verizon landcodes](https://msdn.microsoft.com/library/mt761717.aspx).

De regel van het blokkeren van /Photos/Straatsburg/filtert bijvoorbeeld bestanden met inbegrip van:

    http://<endpoint>.azureedge.net/Photos/Strasbourg/1000.jpg
    http://<endpoint>.azureedge.net/Photos/Strasbourg/Cathedral/1000.jpg


##<a name="country-codes"></a>Landcodes

De functie voor het **Filteren van Geo** gebruikt landcodes de landen waar een aanvraag worden toegestaan of geblokkeerd voor een beveiligde map. U vindt de landcodes in [Azure CDN van Verizon landcodes](https://msdn.microsoft.com/library/mt761717.aspx). Als u "EU" (Europa) of 'AP' (Azië/Stille Oceaan) opgeeft, wordt een subset van IP-adressen die afkomstig van een land in die regio's zijn worden geblokkeerd of toegestaan.


##<a id="considerations"></a>Overwegingen met betrekking tot

- Het duurt maximaal 90 minuten voor wijzigingen in de configuratie van uw land filteren worden pas van kracht.
- Deze functie biedt geen ondersteuning voor jokertekens (bijvoorbeeld ' *').
- Het filteren van de configuratie die is gekoppeld aan het relatieve pad land wordt recursief toegepast op dat pad.
- Slechts één regel kan worden toegepast op hetzelfde relatieve pad (u kunt meerdere land filters die naar hetzelfde relatieve pad verwijzen niet maken. Een map hebben echter meerdere filters van het land. Dit komt door de aard van de recursieve land filters. Met andere woorden, kan dat een submap van een eerder geconfigureerde map het filter van een ander land worden toegewezen.
