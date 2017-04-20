<properties 
    pageTitle="Het starten van streaming projecten in stroom Analytics | Microsoft Azure" 
    description="Hoe een streaming taak uitvoeren in Azure Stream Analytics | leren padsegment."
    keywords="Streaming van taken"
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

# <a name="how-to-run-a-streaming-job-in-azure-stream-analytics"></a>Het uitvoeren van een taak streaming in Azure Stream Analytics

Wanneer een taak invoeren en query uitvoer zijn alle opgegeven dat kunt u de Stream Analytics taak starten.

Uw taak starten:

1.  In de klassieke Azure portal van het project dashboard, klikt u op **Start** onder aan de pagina.

    ![Knop taak starten](./media/stream-analytics-run-a-job/1-stream-analytics-run-a-job.png)  

    Klik op **Start** boven aan de pagina taak in het portal voor Azure.

    ![Azure portal Start Project knop](./media/stream-analytics-run-a-job/4-stream-analytics-run-a-job.png)  

2.  Geef een waarde **Start uitvoer** om te bepalen wanneer u deze taak zal gaan produceren output. De standaardinstelling voor taken die nog niet eerder zijn begonnen is **Begintijd taak**, wat inhoudt dat de taak onmiddellijk verwerking van persoonsgegevens wordt gestart. U kunt ook een **aangepaste** tijd opgeven in het verleden (voor het gebruik van historische gegevens) of in de toekomst (voor verwerking tot een later tijdstip uitstellen). Voor het geval wanneer een taak is eerder gestart en gestopt, de optie **Laatste tijd gestopt** is beschikbaar voor het hervatten van de taak van de laatste keer voor uitvoer en gegevensverlies te voorkomen.  

    ![Streaming-tijd van taak starten](./media/stream-analytics-run-a-job/2-stream-analytics-run-a-job.png)  

    ![Azure portal Start streaming project tijd](./media/stream-analytics-run-a-job/5-stream-analytics-run-a-job.png)  

3.  Bevestig uw selectie. Status van de taak wordt gewijzigd in *eerste* en gaat binnenkort naar *uitgevoerd* zodra de taak is begonnen. U kunt de voortgang van de bewerking **Start** in de **Kennisgeving Hub**controleren:

    ![Streaming van de voortgang van het project](./media/stream-analytics-run-a-job/3-stream-analytics-run-a-job.png)  

    ![Azure portal streaming voortgang van het project](./media/stream-analytics-run-a-job/6-stream-analytics-run-a-job.png)  

## <a name="get-help"></a>Help-informatie opvragen
Probeer onze [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics) voor verdere ondersteuning

## <a name="next-steps"></a>Volgende stappen

- [Inleiding tot Analytics Azure Stream](stream-analytics-introduction.md)
- [Aan de slag met Azure Stream Analytics](stream-analytics-get-started.md)
- [Schaal Azure Stream Analytics taken](stream-analytics-scale-jobs.md)
- [Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure Stream Analytics beheer REST API: naslag](https://msdn.microsoft.com/library/azure/dn835031.aspx)
