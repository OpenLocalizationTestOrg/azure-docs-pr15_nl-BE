<properties
    pageTitle="Activa op een eindpunt Azure CDN vooraf laden | Microsoft Azure"
    description="Informatie over het vooraf laden van inhoud in cache op een eindpunt CDN."
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

# <a name="pre-load-assets-on-an-azure-cdn-endpoint"></a>Activa op een eindpunt Azure CDN vooraf laden

[AZURE.INCLUDE [cdn-verizon-only](../../includes/cdn-verizon-only.md)]

Standaard activa worden eerst in de cache opgeslagen als ze worden opgevraagd. Dit betekent dat het eerste verzoek van elke regio kan langer duren, omdat de servers van de rand geen de inhoud in de cache opgeslagen en moet de aanvraag doorsturen naar de oorspronkelijke server. Vooraf laden van inhoud voorkomt dit eerste hits latentie.

Naast een betere klantervaring, kan vooraf laden van uw activa in de cache ook verminderd netwerkverkeer op de oorspronkelijke server.

> [AZURE.NOTE] Vooraf laden activa is handig voor grote evenementen of inhoud die tegelijk beschikbaar voor een groot aantal gebruikers, zoals een nieuwe release van de film of een software-update komt.

Deze handleiding helpt u bij het vooraf laden van inhoud in cache op alle knooppunten van Azure CDN rand.

## <a name="walkthrough"></a>Stapsgewijze instructies

1. Ga naar de CDN-profiel met het eindpunt dat u wilt laden vooraf in de [Portal Azure](https://portal.azure.com).  De blade profiel wordt geopend.

2. Klik op het eindpunt in de lijst.  Hiermee opent u het eindpunt blade.

3. Klik op de knop laden van de bladeserver CDN-eindpunt.

    ![CDN eindpunt blade](./media/cdn-preload-endpoint/cdn-endpoint-blade.png)

    De blade laden wordt geopend.

    ![CDN laden blade](./media/cdn-preload-endpoint/cdn-load-blade.png)

4. Geef het volledige pad van elk activum dat u wilt laden (bijv. `/pictures/kitten.png`) in het tekstvak **pad** .

    > [AZURE.TIP] Meer **pad** tekstvakken wordt weergegeven nadat u tekst dat u kunt een lijst samenstellen van meerdere elementen.  U kunt elementen uit de lijst verwijderen door te klikken op de knop met het weglatingsteken (...).
    >
    > Paden moeten een relatieve URL die past bij de volgende [reguliere expressie](https://msdn.microsoft.com/library/az24scfc.aspx): `^(?:\/[a-zA-Z0-9-_.\u0020]+)+$`.  Elk actief moet zijn eigen pad hebben.  Er is geen jokertekens-functie voor het vooraf laden van activa.

    ![Knop laden](./media/cdn-preload-endpoint/cdn-load-paths.png)

5. Klik op de knop **laden** .

    ![Knop laden](./media/cdn-preload-endpoint/cdn-load-button.png)

> [AZURE.NOTE] Er is een beperking van verzoeken per minuut per profiel CDN 10 laden.

## <a name="see-also"></a>Zie ook
- [Een eindpunt Azure CDN leegmaken](cdn-purge-endpoint.md)
- [Azure CDN REST API: naslag - wissen of een eindpunt vooraf laden](https://msdn.microsoft.com/library/mt634451.aspx)
