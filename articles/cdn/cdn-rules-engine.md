<properties
    pageTitle="Standaardgedrag HTTP in Azure CDN met behulp van regels-engine | Microsoft Azure"
    description="De regels-engine kunt u een cache-beleid definiëren, aanpassen hoe HTTP-aanvragen worden verwerkt door Azure CDN, zoals de levering van bepaalde typen inhoud blokkeren en HTTP-headers wijzigen."
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

# <a name="override-default-http-behavior-using-the-rules-engine"></a>Standaard HTTP-gedrag met behulp van regels-engine wijzigen

[AZURE.INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]

## <a name="overview"></a>Overzicht

De regels-engine kunt u aanpassen hoe HTTP-aanvragen worden verwerkt, zoals de levering van bepaalde typen inhoud blokkeren, een cachebeleid definiëren en HTTP-headers wijzigen.  Deze zelfstudie laat zien een regel maken die het cachinggedrag CDN activa wordt gewijzigd.  Er is ook video-inhoud beschikbaar in de sectie "[Zie ook](#see-also)".

## <a name="tutorial"></a>Zelfstudie

1. Klik op de knop **beheren** van de bladeserver CDN-profiel.

    ![Knop CDN profiel blade beheren](./media/cdn-rules-engine/cdn-manage-btn.png)

    De portal CDN management wordt geopend.

2. Klik op de **Grote HTTP-** tab, gevolgd door een **Regels-Engine**.

    Opties voor een nieuwe regel weergegeven.

    ![Opties voor nieuwe regel CDN](./media/cdn-rules-engine/cdn-new-rule.png)

    >[AZURE.IMPORTANT] De volgorde waarin meerdere regels zijn van invloed op hoe ze worden behandeld. De acties die door een vorige regel opgegeven kan worden overschreven door een volgende regel.
    
3. Voer een naam in de **naam / omschrijving** textbox.

4. Identificatie van het type van de regel van toepassing op aanvragen.  De voorwaarde **altijd** match is standaard ingeschakeld.  U zult **altijd** voor deze zelfstudie gebruiken, dus laat die geselecteerd.

    ![Voorwaarde voor CDN vergelijken](./media/cdn-rules-engine/cdn-request-type.png)

    >[AZURE.TIP] Er zijn veel soorten overeenkomst voorwaarden beschikbaar in de vervolgkeuzelijst.  Op de blauwe informatief pictogram links van de voorwaarde van de overeenkomst te klikken, wordt de geselecteerde voorwaarde in detail uitgelegd.
    >
    >Zie voor de volledige lijst van identieke omstandigheden in detail [regels Engine overeenkomen met voorwaarde en Details van de functie](https://msdn.microsoft.com/library/mt757336.aspx#Anchor_0).

5.  Klik op de **+** knop naast **functies** toe te voegen, een nieuwe functie.  Selecteer in de vervolgkeuzelijst aan de linkerkant, **Geldende interne Max-Age**.  Voer in het tekstvak **300**.  Laat de overige standaardwaarden.

    ![CDN, functie](./media/cdn-rules-engine/cdn-new-feature.png)

    >[AZURE.NOTE] Als met de voorwaarden van de overeenkomst verschijnt te klikken op de blauwe informatief pictogram links van de nieuwe functie meer informatie over deze functie.  In het geval van de **Geldende interne Max-Age**, we doorslaggevende redenen **Cache-Control** en **Expires** headers van het vaste activum om te bepalen wanneer de CDN randknooppunt de activa van de oorsprong wordt vernieuwd.  Ons voorbeeld van 300 seconden betekent dat de CDN randknooppunt cache het activum voor 5 minuten voor het vernieuwen van de activa van de oorsprong ervan.
    >
    >Zie voor de volledige lijst van functies in detail [regels Engine Match voorwaarde en Details van de functie](https://msdn.microsoft.com/library/mt757336.aspx#Anchor_1).

6.  Klik op de knop **toevoegen** om de nieuwe regel opslaan.  De nieuwe regel wordt nu wacht op goedkeuring. Zodra deze is goedgekeurd, wordt de status gewijzigd van **In behandeling zijnde XML** naar **Actieve XML**.

    >[AZURE.IMPORTANT] Wijzigingen van de regels kunnen doorgeven via de CDN 90 minuten duren.

## <a name="see-also"></a>Zie ook
* [Vrijdag azure: Azure CDN krachtige nieuwe Premium-functies](https://azure.microsoft.com/documentation/videos/azure-cdns-powerful-new-premium-features/) (video)
* [Regels-Engine Match voorwaarde en Details van de functie](https://msdn.microsoft.com/library/mt757336.aspx)
