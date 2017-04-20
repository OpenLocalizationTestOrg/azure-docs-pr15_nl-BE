<properties
    pageTitle="Gebruik van gegevens in logboek Analytics analyseren | Microsoft Azure"
    description="Kunt u de pagina gebruik in Analytics logboek bekijken hoeveel gegevens wordt verzonden naar de OMS-service."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/11/2016"
    ms.author="banders"/>

# <a name="analyze-data-usage-in-log-analytics"></a>Gebruik van gegevens in logboek Analytics analyseren

Logboek Analytics Operations Management Suite (OMS) worden gegevens verzameld en verzonden naar de OMS-service periodiek.  De pagina **Gebruik** kunt u bekijken hoeveel gegevens wordt verzonden naar de OMS-service. De pagina **Gebruik** ziet u ook hoeveel gegevens dagelijks wordt gestuurd door oplossingen en hoe vaak uw servers verzenden gegevens.

>[AZURE.NOTE] Als u een gratis account gemaakt met de [OMS-website](http://www.microsoft.com/oms)hebt, bent u beperkt tot 500 MB aan gegevens naar de OMS-service dagelijks verzonden. Als u de dagelijkse limiet bereikt, wordt gegevensanalyse stoppen en hervatten aan het begin van de volgende dag. Je hebt niet is aanvaard of verwerkt door OMS gegevens verzenden.

U kunt uw gebruik via de tegel voor **Gebruik** op het dashboard **Overzicht** in OMS weergeven.

![Gebruik naast elkaar](./media/log-analytics-usage/usage-tile.png)

Als u uw dagelijkse gebruikslimiet overschreden hebt of als u in de buurt van de limiet, kunt u desgewenst een oplossing voor het verminderen van de hoeveelheid gegevens die u naar de OMS-service verzendt verwijderen. Zie voor meer informatie over het verwijderen van oplossingen, [oplossingen voor logboek Analytics toevoegen uit de galerie met oplossingen](log-analytics-add-solutions.md).

![Gebruik dashboard](./media/log-analytics-usage/usage-dashboard.png)

De pagina **Gebruik** bevat de volgende informatie:

- Gemiddeld gebruik per dag
- Gebruik van gegevens voor elke oplossing van de laatste 30 dagen
- Hoeveel servers in uw omgeving zijn het verzenden van gegevens naar de OMS-service gedurende de afgelopen 30 dagen
- Uw gegevens plan prijzen laag en de geschatte kosten
- Informatie over de serviceovereenkomst (SLA), met inbegrip van hoe lang het duurt OMS om uw gegevens te verwerken

## <a name="to-work-with-usage-data"></a>Werken met gegevens over het gebruik

1. Klik op de tegel voor **Gebruik** op de pagina **Overzicht** .
2. Op de pagina **gebruik van** het van gebruikscategorieën weergeven die gebieden weergeven als dat u bang bent.
3. Hebt u een oplossing die te veel van uw dagelijkse quotum voor uploaden verbruikt, kunt u overwegen deze oplossing verwijderen.

## <a name="to-view-your-estimated-cost-and-billing-information"></a>De geschatte kosten en factureringsgegevens weergeven
1. Klik op de tegel voor **Gebruik** op de pagina **Overzicht** .
2. Klik op de pagina **Gebruik** onder **Gebruik**op de dubbele punthaken (**>**) naast de **geschatte kosten**.
3. In de uitgebreide details van **uw data-abonnement** , kunt u de geschatte kosten per maand bekijken.  
    ![Uw data-abonnement](./media/log-analytics-usage/usage-data-plan.png)
4. Als u uw factuurgegevens bekijken wilt, klikt u op **Mijn factuur weergeven** om uw abonnementsgegevens weer te geven.
    - Klik op de pagina abonnementen weergeven, details en een lijst artikel op de regel van het gebruik van uw abonnement.  
        ![abonnement](./media/log-analytics-usage/usage-sub01.png)
    - Op de overzichtspagina voor uw abonnement kunt u allerlei taken te beheren en meer details weergeven over uw abonnement uitvoeren.  
        ![details van abonnement](./media/log-analytics-usage/usage-sub02.png)

## <a name="to-view-data-batches-for-your-sla"></a>Batches voor uw SLA gegevens weergeven
1. Klik op de tegel voor **Gebruik** op de pagina **Overzicht** .
2. Klik op **Download SLA details**onder **Service Level Agreement**.
3. Een Excel XLSX-bestand is gedownload kunt bekijken.  
    ![SLA details](./media/log-analytics-usage/usage-sla-details.png)

## <a name="next-steps"></a>Volgende stappen

- Zie [zoekacties in Analytics logboek logboek](log-analytics-log-searches.md) gedetailleerde informatie verzameld door oplossingen te bekijken.
