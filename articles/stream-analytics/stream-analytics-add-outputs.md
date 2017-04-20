<properties 
    pageTitle="Hiermee kunt u gegevens configureren voor Analytics Stream projecten | Microsoft Azure" 
    description="Uitgangen voor Stream Analytics taken configureren | leren padsegment."
    keywords="output, gegevens verplaatsen van gegevens"
    documentationCenter=""
    services="stream-analytics"
    authors="jeffstokes72" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="stream-analytics" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="data-services" 
    ms.date="09/26/2016" 
    ms.author="jeffstok"/> 

# <a name="how-to-configure-data-outputs-for-stream-analytics-jobs"></a>Hiermee kunt u gegevens configureren voor Stream Analytics taken

Azure Stream Analytics taken kunnen worden aangesloten op een of meer gegevens-uitgangen, die een verbinding met een bestaande gegevens-sink definieert. Als uw baan Analytics Stream verwerkt en binnenkomende gegevens worden omgezet, wordt aan de uitvoer van uw project een stroom van gegevens output gebeurtenissen geschreven.

Analytics-stroomgegevens uitgangen kunnen worden gebruikt om de bron van realtime dashboards of waarschuwingen, gegevensverplaatsing werkstromen starten of alleen gearchiveerd voor later op batch-verwerking. Stream Analytics heeft eerste-klas integratie met verschillende Azure services die hier worden beschreven.

Uitvoer toevoegen aan uw project Analytics Stream:

1. In de klassieke Portal Azure **uitgangen** op en klik op **Uitvoer toevoegen** in uw project Stream Analytics.

    ![Uitgangen toevoegen](./media/stream-analytics-add-outputs/1-stream-analytics-add-outputs.png)  

    Klik op de tegel **uitgangen** in uw project Stream Analytics in Azure portal.

    ![Azure Portal uitgangen toevoegen](./media/stream-analytics-add-outputs/5-stream-analytics-add-outputs.png)

2. Geef het type van de uitvoer:

    ![Verplaatsing van gegevens kiezen](./media/stream-analytics-add-outputs/2-stream-analytics-add-outputs.png)  

    ![Azure portal verplaatsing gegevenstype kiezen](./media/stream-analytics-add-outputs/6-stream-analytics-add-outputs.png)

3. Geef een beschrijvende naam voor deze uitvoer in het vak **Output Alias** . Deze naam kan worden gebruikt in de query van de taak later te verwijzen naar de uitvoer.  
    
    Vul de rest van de vereiste eigenschappen van de verbinding met de uitvoer.  Deze velden is afhankelijk van het uitvoertype en hier nader zijn omschreven.  

    ![Data output-eigenschappen toevoegen](./media/stream-analytics-add-outputs/3-stream-analytics-add-outputs.png)  

4. Afhankelijk van het uitvoertype moet u opgeven hoe de gegevens worden geserialiseerd of opgemaakt. De serialisatie van specifieke instellingen voor elk uitvoertype, worden hier beschreven.

    Vul de rest van de vereiste eigenschappen van de verbinding met de gegevensbron. Deze velden is afhankelijk van het type invoer- en type en worden gedefinieerd in detail [hier](stream-analytics-create-a-job.md).  

    ![Uitvoer toevoegen aan de gebeurtenis hub](./media/stream-analytics-add-outputs/4-stream-analytics-add-outputs.png)  

    ![Azure portal uitvoer op gebeurtenis hub](./media/stream-analytics-add-outputs/7-stream-analytics-add-outputs.png)  

> [Azure.Note] Een output-element toegevoegd aan de taak, moet bestaan voordat de taak is gestart en gebeurtenissen gestart die doorloopt. Bijvoorbeeld als u als output van een Blob storage, wordt de taak niet een opslag-account automatisch gemaakt. Deze moet door de gebruiker worden gemaakt voordat de ASA taak is gestart.

## <a name="get-help"></a>Help-informatie opvragen
Probeer onze [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics) voor verdere ondersteuning

## <a name="next-steps"></a>Volgende stappen

- [Inleiding tot Analytics Azure Stream](stream-analytics-introduction.md)
- [Aan de slag met Azure Stream Analytics](stream-analytics-get-started.md)
- [Schaal Azure Stream Analytics taken](stream-analytics-scale-jobs.md)
- [Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure Stream Analytics beheer REST API: naslag](https://msdn.microsoft.com/library/azure/dn835031.aspx)
