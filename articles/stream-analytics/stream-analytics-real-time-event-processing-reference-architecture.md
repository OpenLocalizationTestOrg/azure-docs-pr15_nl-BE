<properties 
    pageTitle="Real-time gebeurtenis verwerken met de verwerking van de gebeurtenis Stream Analytics | Microsoft Azure" 
    description="Informatie over hoe een aantal Azure services kan samenwerken voor het inschakelen van gebeurtenis real-time verwerking en analyse." 
    keywords="real-time verwerking, verwerking van de gebeurtenis, referentiearchitectuur"
    services="stream-analytics,event-hubs,storage,sql-database" 
    documentationCenter="" 
    authors="jeffstokes72" 
    manager="jhubbard" 
    editor=""/>

<tags 
    ms.service="stream-analytics" 
    ms.workload="big-data" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016" 
    ms.author="jeffstok"/>

# <a name="reference-architecture-real-time-event-processing-with-microsoft-azure-stream-analytics"></a>Verwijzen naar architectuur: Real-time verwerking met Microsoft Azure Stream Analytics van gebeurtenis

De referentiearchitectuur voor real-time gebeurtenis verwerken met Azure Stream Analytics is bedoeld om een algemene blauwdruk voor de implementatie van een platform voor realtime als stream processing service (PaaS) oplossing met Microsoft Azure.

## <a name="summary"></a>Samenvatting

Traditioneel zijn analytics oplossingen gebaseerd op de mogelijkheden zoals ETL (extract, transform, load) en gegevensopslag, waarin gegevens worden opgeslagen voordat de analyse. Veranderende vereisten, met inbegrip van meer snel binnenkomende gegevens, worden deze bestaande model aan de limiet duwen. De mogelijkheid voor het analyseren van gegevens binnen stromen vóór opslag verplaatsen is een mogelijke oplossing en het is niet een nieuwe mogelijkheid, de aanpak niet sterk is vastgesteld over alle industrie verticalen. 

Microsoft Azure biedt een uitgebreide catalogus van analytics-technologieën bieden ondersteuning van een matrix van een oplossing voor verschillende scenario's en vereisten. Selecteren welke Azure services te implementeren voor een end-to-end oplossing kan een uitdaging zijn gezien de breedte van de aanbiedingen. Dit papier is ontworpen voor het beschrijven van de mogelijkheden en de samenwerking van de verschillende Azure services die een oplossing voor gebeurtenis streaming ondersteunen. Ook wordt uitgelegd sommige scenario's waarin klanten van dit soort benadering profiteren kunnen.

## <a name="contents"></a>Inhoud

- Samenvatting
- Inleiding tot Real-Time Analytics
- Toegevoegde waarde van Real-Time gegevens in Azure
- Gebruikelijke scenario's voor Real-Time Analytics
- Architectuur en onderdelen
    - Gegevensbronnen
    - Gegevensintegratie laag
    - Real-time Analytics laag
    - Data-opslaglaag
    - Presentatie / laag verbruik
- Conclusie

**Auteur:** Charles Feddersen, Solution Architect, Datacenter inzichten van topkwaliteit, Microsoft Corporation

**Gepubliceerd:** Januari 2015

**Revisie:** 1.0

**Download:** [Real-Time verwerking met Microsoft Azure Stream Analytics van gebeurtenis](http://download.microsoft.com/download/6/2/3/623924DE-B083-4561-9624-C1AB62B5F82B/real-time-event-processing-with-microsoft-azure-stream-analytics.pdf)


## <a name="get-help"></a>Help-informatie opvragen
Probeer onze [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics) voor verdere ondersteuning

## <a name="next-steps"></a>Volgende stappen

- [Inleiding tot Analytics Azure Stream](stream-analytics-introduction.md)
- [Aan de slag met Azure Stream Analytics](stream-analytics-get-started.md)
- [Schaal Azure Stream Analytics taken](stream-analytics-scale-jobs.md)
- [Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure Stream Analytics beheer REST API: naslag](https://msdn.microsoft.com/library/azure/dn835031.aspx)

 
