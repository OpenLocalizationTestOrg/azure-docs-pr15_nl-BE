<properties
    pageTitle="Een eindpunt CDN Azure verwijderen | Microsoft Azure"
    description="Informatie over het wissen van alle opgeslagen inhoud vanuit het eindpunt van een CDN."
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

# <a name="purge-an-azure-cdn-endpoint"></a>Een eindpunt Azure CDN leegmaken

## <a name="overview"></a>Overzicht

Azure CDN rand knooppunten cache activa tot de activa van de time-to-live (TTL) verloopt.  Nadat de TTL van het vaste activum is verlopen, wanneer een client hierom de activa van het randknooppunt vraagt, een nieuwe, bijgewerkte kopie van de activa dienen de aanvraag van de client worden opgehaald door het randknooppunt en winkel de cache vernieuwen.

Soms wilt u mogelijk inhoud in cache wissen van alle knooppunten van de rand en zorgen dat ze alle nieuwe bijgewerkte activa ophalen.  Het kan zijn dat de updates naar uw webtoepassing of aan snel update activa met onjuiste gegevens bevatten.

> [AZURE.TIP] Houd er rekening mee dat alleen de inhoud in de cache op de rand CDN servers verwijderen wissen.  Elke downstream caches, zoals proxyservers en caches van lokale, kunnen een kopie van het bestand nog steeds houdt.  Het is belangrijk om dit te onthouden als u een bestand van de time-to-live.  U kunt forceren dat een downstream-client voor het aanvragen van de meest recente versie van het bestand door een unieke naam geven telkens wanneer u deze bijwerken of door gebruik te maken van [caching van de query-tekenreeks](cdn-query-string.md).  

Deze zelfstudie doorloopt u elementen verwijderen uit alle knooppunten van de rand van een eindpunt.

## <a name="walkthrough"></a>Stapsgewijze instructies

1. Ga naar de CDN-profiel met het eindpunt dat u wilt verwijderen in de [Portal Azure](https://portal.azure.com).

2. Klik op de knop wissen van het blad CDN-profiel.

    ![CDN profiel blade](./media/cdn-purge-endpoint/cdn-profile-blade.png)

    Hiermee opent u het blad verwijderen.

    ![CDN leegmaken blade](./media/cdn-purge-endpoint/cdn-purge-blade.png)

3. Selecteer het adres van de te verwijderen uit de vervolgkeuzelijst URL op het blad verwijderen.

    ![Formulier leegmaken](./media/cdn-purge-endpoint/cdn-purge-form.png)

    > [AZURE.NOTE] Ook kun je naar het blad verwijderen door te klikken op de knop **verwijderen** op het eindpunt CDN blade.  In dat geval wordt het **URL** -veld worden vooraf gevuld met het adres van dat specifieke eindpunt.

4. Selecteer welke activa u wilt verwijderen uit de knooppunten van de rand.  Als u wissen van alle activa wilt, klikt u op het selectievakje **Alle verwijderen** .  Anders, typ het volledige pad van elk activum dat u wilt verwijderen (bijvoorbeeld, `/pictures/kitten.png`) in het tekstvak **pad** .

    > [AZURE.TIP] Meer **pad** tekstvakken wordt weergegeven nadat u tekst dat u kunt een lijst samenstellen van meerdere elementen.  U kunt elementen uit de lijst verwijderen door te klikken op de knop met het weglatingsteken (...).
    >
    > Paden moeten een relatieve URL die passen bij de volgende [reguliere expressie](https://msdn.microsoft.com/library/az24scfc.aspx): `^\/(?:[a-zA-Z0-9-_.\u0020]+\/)*\*$";`.  Voor **Azure CDN van Verizon** (Standard en Premium), sterretje (\*) kan worden gebruikt als een jokerteken (bijv. `/music/*`).  Jokertekens en **wissen alle** mogen niet met **Azure CDN van Akamai**.
    
5. Klik op de knop **verwijderen** .

    ![Knop verwijderen](./media/cdn-purge-endpoint/cdn-purge-button.png)

> [AZURE.IMPORTANT] Purge aanvragen nemen ongeveer 2-3 minuten om te verwerken met **Azure CDN van Verizon** (Standard en Premium) en ongeveer 7 minuten met **Azure CDN van Akamai**.  Azure CDN heeft een limiet van 50 gelijktijdige aanvragen op elk gewenst moment verwijderen. 

## <a name="see-also"></a>Zie ook
- [Activa op een eindpunt Azure CDN vooraf laden](cdn-preload-endpoint.md)
- [Azure CDN REST API: naslag - wissen of een eindpunt vooraf laden](https://msdn.microsoft.com/library/mt634451.aspx)
