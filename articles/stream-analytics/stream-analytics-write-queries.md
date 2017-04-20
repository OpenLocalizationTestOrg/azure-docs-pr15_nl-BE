<properties 
    pageTitle="Het schrijven van query's in de Stream Analytics | Microsoft Azure" 
    description="Query's in de Stream Analytics en de querygegevens schrijven | leren padsegment."
    keywords="het schrijven van query's, querygegevens, een query, query's schrijven"
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

# <a name="how-to-write-queries-in-stream-analytics"></a>Het schrijven van query's in Analytics Stream

Schrijven van query's voor stream processing logica in Azure Stream Analytics is geïmplementeerd als een "permanente query' die is gedefinieerd, voordat een taak wordt gestart en uitgevoerd op gegevens, zoals het de taak is bereikt. De gegevenstransformatie wordt uitgedrukt in een SQL-achtige querytaal, die grotendeels een subset van T-SQL met bepaalde extensies toegevoegde taal zoals [Windowing](https://msdn.microsoft.com/library/azure/dn835019.aspx) toegepast voor tijdelijke semantiek.

## <a name="writing-queries"></a>Het schrijven van query's: ##

1. Klik op **Query**in uw project Stream Analytics in Azure Management portal.

    ![Selectiequery](./media/stream-analytics-write-queries/1-stream-analytics-write-queries.png)  

    Klik op de **Query**in de Portal Azure.

    ![Selecteer Voorvertoning in Query](./media/stream-analytics-write-queries/query-preview-portal.png)  

2.  Nieuwe taken hebben een querysjabloon waarmee u aan de slag te krijgen. De querysjabloon uitvoert een "pass-through" query die velden alle projecten van invoer gebeurtenissen in de uitvoer.  

    - Als u ten minste één invoer en uitvoer voor de taak hebt gedefinieerd, vervangt u de tijdelijke aanduiding '[YourOutputAlias]' en '[YourInputAlias]' velden met de aliassen van de invoer en uitvoer die u wilt gebruiken eerst. Daarnaast kunt u nog steeds ontwerpen en de query testen in de klassieke Portal Azure zonder ingangen en uitgangen op het project definiëren.
    - Als u meer verwerken dan een eenvoudige Pass Through-query wilt, kunt u de definitie van de query kunt bewerken. Om te beginnen met het ontwerpen van query Bekijk enkele algemene query's worden vastgelegd met patronen [hier](stream-analytics-stream-analytics-query-patterns.md).  
  
    ![Querygegevens venster](./media/stream-analytics-write-queries/2-stream-analytics-write-queries.png)  

## <a name="to-validate-query-data-is-working"></a>Voor het valideren van querygegevens werken: ##

U kunt testen dat uw query zich gedraagt zoals verwacht in de browser worden uitgevoerd via een of meer lokale JSON bestanden met testgegevens. Dit wordt niet de taak start of factuuradres gevolgen hebben.

> [AZURE.NOTE] Testen met query's in de browser wordt momenteel niet ondersteund in de Portal Azure.  

1.  Zorg ervoor dat er geen fouten zijn opgetreden in de query (anders is de knop testen wordt uitgeschakeld) en klik vervolgens op de knop testen.  

    ![Querygegevens Test](./media/stream-analytics-write-queries/3-stream-analytics-write-queries.png)  

2.  U wordt gevraagd om bestanden voor elk van de "inputs" waarnaar wordt verwezen in de query opgeven. In dit voorbeeld wordt de sjabloon query blijft als-is, zodat het dialoogvenster gevraagd voor een invoer met de naam 'yourinputalias'.  

    ![De query gegevens testen](./media/stream-analytics-write-queries/4-stream-analytics-write-queries.png)  

3.  Blader naar een testbestand. Verschillende voorbeeldbestanden zijn beschikbaar op [github](https://github.com/Azure/azure-stream-analytics/tree/master/Sample Data) en kunt u voorbeeldgegevens ook ophalen uit uw eigen data stream invoer via de functie van de voorbeeldgegevens op het tabblad invoer.  

    ![Invoer voor de query](./media/stream-analytics-write-queries/5-stream-analytics-write-queries.png)  

4.  Nadat het dialoogvenster te sluiten, de query wordt uitgevoerd via de testgegevens en de resultaten onder aan de pagina van de Query worden weergegeven.  

    ![Samenvatting van query](./media/stream-analytics-write-queries/6-stream-analytics-write-queries.png)  

## <a name="get-help"></a>Help-informatie opvragen
Probeer onze [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics) voor verdere ondersteuning

## <a name="next-steps"></a>Volgende stappen

- [Inleiding tot Analytics Azure Stream](stream-analytics-introduction.md)
- [Aan de slag met Azure Stream Analytics](stream-analytics-get-started.md)
- [Schaal Azure Stream Analytics taken](stream-analytics-scale-jobs.md)
- [Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure Stream Analytics beheer REST API: naslag](https://msdn.microsoft.com/library/azure/dn835031.aspx)
