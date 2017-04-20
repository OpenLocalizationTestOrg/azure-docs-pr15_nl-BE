<properties
    pageTitle="Inleiding tot de functies van de Stream Analytics venster | Microsoft Azure"
    description="Meer informatie over de drie functies venster in Analytics Stream (tumbling, hopping, schuiven)."
    keywords="venster schuiven, venster, venster hopping tumbling"
    documentationCenter=""
    services="stream-analytics"
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"
/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="09/26/2016"
    ms.author="jeffstok"
/>


# <a name="introduction-to-stream-analytics-window-functions"></a>Inleiding tot de functies van de Stream Analytics venster

In veel real-time streaming van scenario's, is het noodzakelijk voor het uitvoeren van bewerkingen op de gegevens in de tijdelijke windows alleen. Native ondersteuning voor windowing functies is een belangrijke functie van Azure Stream Analytics die de naald op de productiviteit van ontwikkelaars beweegt in de verwerking van de stroom van complexe projecten ontwerpen. Stream Analytics kan ontwikkelaars temporal bewerkingen uitvoeren op gegevens van streaming met [**Tumbling**](https://msdn.microsoft.com/library/dn835055.aspx), [**Hopping**](https://msdn.microsoft.com/library/dn835041.aspx) en [**beweegbare**](https://msdn.microsoft.com/library/dn835051.aspx) vensters. Het is vermeldenswaardig dat alle bewerkingen van het [venster](https://msdn.microsoft.com/library/dn835019.aspx) resultaten aan het **einde** van het venster uitvoer. De uitvoer van het venster worden enkele gebeurtenis op basis van de statistische functie gebruikt. De gebeurtenis is het tijdstempel van het einde van het venster en alle functies van het venster worden gedefinieerd met een vaste lengte. Ten slotte is het belangrijk te weten dat alle functies van het venster moeten worden gebruikt in een [**GROUP BY**](https://msdn.microsoft.com/library/dn835023.aspx) -component.

![Stream Analytics venster werkt concepten](media/stream-analytics-window-functions/stream-analytics-window-functions-conceptual.png)

## <a name="tumbling-window"></a>Venster gewor

Tumbling-functies worden gebruikt voor een gegevensstroom segmenteren in afzonderlijke tijd segmenten en het uitvoeren van een functie tegen hen, zoals in het volgende voorbeeld venster. De belangrijkste differentiators van een venster gewor zijn dat ze herhalen, elkaar niet overlappen en een gebeurtenis kan geen deel uitmaken van meer dan één gewor-venster.

![Stream Analytics venster functies tumbling intro](media/stream-analytics-window-functions/stream-analytics-window-functions-tumbling-intro.png)

## <a name="hopping-window"></a>Venster Hopping

Hopping venster functies hop vooruit in de tijd met een vaste periode. Kan het zijn gemakkelijk te zien ze als gewor windows die kunnen overlappen zodat gebeurtenissen deel van meer dan een resultaatset van de Hopping-venster uitmaken kunnen. Om een venster Hopping hetzelfde als een daling geeft venster één gewoon de hop grootte gelijk zijn aan de grootte van het venster. 

![Stream Analytics venster werkt hopping intro](media/stream-analytics-window-functions/stream-analytics-window-functions-hopping-intro.png)

## <a name="sliding-window"></a>Schuifvenster

Verschuivende functies venster, in tegenstelling tot windows gewor of Hopping, produceren een uitvoer **alleen** wanneer een gebeurtenis plaatsvindt. Elk venster heeft ten minste één gebeurtenis en het venster voortdurend wordt voorwaarts verplaatst door een € (epsilon). Als Windows Hopping kunnen gebeurtenissen behoren tot meer dan één venster schuiven.

![Stream Analytics venster werkt verschuivende intro](media/stream-analytics-window-functions/stream-analytics-window-functions-sliding-intro.png)

## <a name="getting-help-with-window-functions"></a>Help opvragen bij functies venster

Probeer onze [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics) voor verdere ondersteuning

## <a name="next-steps"></a>Volgende stappen

- [Inleiding tot Analytics Azure Stream](stream-analytics-introduction.md)
- [Aan de slag met Azure Stream Analytics](stream-analytics-get-started.md)
- [Schaal Azure Stream Analytics taken](stream-analytics-scale-jobs.md)
- [Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure Stream Analytics beheer REST API: naslag](https://msdn.microsoft.com/library/azure/dn835031.aspx)
