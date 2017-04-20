<properties
    pageTitle="Visualiseren en oplossen taken Stream Analytics | Microsoft Azure"
    description="Informatie over het visualiseren van een pijpleiding stroom Analytics taak voor het gebruik van de functie van diagnostische gegevens van het diagram zelf oplossen."
    keywords=""
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


# <a name="visualize-and-troubleshoot-stream-analytics-jobs"></a>Visualiseren en Stream Analytics taken oplossen

In Analytics Stream, net als bij andere cloud-gebaseerde technologieën, is het oplossen van problemen soms nodig te onderzoeken waarom een taak niet levert de verwachte output (of elke uitvoer wat dat betreft). Stream Analytics biedt met dit doel voor ogen, de mogelijkheid voor het visualiseren van een streaming-project. Dit is ook handig als een hulpmiddel voor het modelleren en heeft het voordeel aan de zijkant voor deze documentatie die van hun werk.

In het deelvenster weergave worden de ingangen zichtbaar en de query wordt uitgevoerd en vervolgens alle de uitgangen geconfigureerd. Problemen met verbindingen of de configuratie kunnen duidelijker worden en kan ook het handig zijn om te zien een visuele weergave van uw configuratie.

## <a name="using-the-diagnosis-diagram-tool"></a>Het hulpprogramma voor de diagnose diagram

Als u deze visualizer, klikt u op de knop 'Diagram diagnose' in het blad 'Instellingen' van de van de Stream Analytics taak.

![Stream-Analytics-Troubleshoot-visualization-diagnosis-diagram](./media/stream-analytics-troubleshoot-visualization/stream-analytics-troubleshoot-visualization-diagnosis-diagram1.png)

Alle invoer en uitvoer is van een kleur voorzien om aan te geven van de huidige status van dat onderdeel, zoals hieronder wordt weergegeven.

![Stream-Analytics-Troubleshoot-visualization-Input-map](./media/stream-analytics-troubleshoot-visualization/stream-analytics-troubleshoot-visualization-input-map.png)

Wanneer de gebruiker wil kijken query tussenliggende stappen om inzicht in de patronen van de gegevensstroom binnen een project, biedt de visualisatie tool een weergave van de verdeling van de query in de stappen van de component en de volgorde van de stroom. Op elke stap van de query te klikken, wordt de bijbehorende sectie in een query bewerken venster zoals weergegeven. 

![Stream-Analytics-Troubleshoot-visualization-Intermediate-Steps](./media/stream-analytics-troubleshoot-visualization/stream-analytics-troubleshoot-visualization-intermediate-steps.png)




## <a name="next-steps"></a>Volgende stappen

- [Inleiding tot Analytics Azure Stream](stream-analytics-introduction.md)
- [Aan de slag met Azure Stream Analytics](stream-analytics-get-started.md)
- [Schaal Azure Stream Analytics taken](stream-analytics-scale-jobs.md)
- [Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure Stream Analytics beheer REST API: naslag](https://msdn.microsoft.com/library/azure/dn835031.aspx)
