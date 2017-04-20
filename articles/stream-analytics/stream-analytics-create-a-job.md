<properties 
    pageTitle="Het maken van een taak voor verwerking analytics voor Stream Analytics | Microsoft Azure" 
    description="Maak een taak voor verwerking analytics voor Stream Analytics | leren padsegment."
    keywords="analytics gegevensverwerking"
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

# <a name="how-to-create-a-data-analytics-processing-job-for-stream-analytics"></a>Het maken van een taak voor verwerking analytics voor Stream Analytics

De resource op het hoogste niveau in Azure Stream Analytics is een Stream Analytics-taak.  Het bestaat uit een of meer input gegevensbronnen, een query waarin de gegevenstransformatie en een of meer output doelen die resultaten naar worden geschreven. Bij elkaar op die manier kunnen de gebruiker uit te voeren gegevens analytics voor het streamen van scenario's voor gegevens verwerken.

Om te streamen Analytics gebruiken, beginnen door een nieuwe gegevensstroom Analytics taak maakt.  Houd er rekening mee dat deze actie geen factuuradres gevolgen heeft totdat de taak is gestart.

1.  Log in op het online [Azure klassieke portal](http://manage.windowsazure.com) of de [Azure portal](https://portal.azure.com/).
2.  In de portal: **Klik op Nieuw**, klikt u op **Gegevens** of **Gegevens Analytics** afhankelijk van uw portal en klik op **Azure Stream Analytics** of **Stream Analytics** en vervolgens **Snel maken**.

    ![De wizard taak analytics gegevensverwerking](./media/stream-analytics-create-a-job/1-stream-analytics-create-a-job.png)  

    ![Data analytics taakverwerking maken](./media/stream-analytics-create-a-job/4-stream-analytics-create-a-job.png)  

3.  Geef de gewenste configuratie voor het project Stream Analytics.
    - Voer in het vak **Naam** een naam voor de taak Stream Analytics. Wanneer de **Naam van de taak** wordt gevalideerd, verschijnt er een groen vinkje in het vak naam van de taak. De **Naam** mag alleen alfanumerieke tekens bevatten en de '-' teken, en moet tussen 3 en 63 tekens.
    - **Regio** in Azure portal of **locatie** in Azure portal gebruiken om op te geven van de geografische locatie waar u de taak uit te voeren.
    - Als de portal Azure, selecteren of maken van een account opslag te gebruiken als de **Regionale controle opslag Account**. Deze opslag rekening wordt gebruikt voor het opslaan van controlegegevens voor alle Analytics Stream projecten in dit gebied worden uitgevoerd.
    - Als de Azure portal, een nieuwe of bestaande **Bronnengroep** opgeven voor het opslaan van verwante bronnen voor uw toepassing.

4.  Nadat de nieuwe Stream Analytics Taakopties zijn geconfigureerd, klikt u op de **Stream Analytics-taak maken**. Het kan enkele minuten duren voordat de Stream Analytics taak moet worden gemaakt. U kunt de status controleren, kunt u de voortgang in de hub meldingen controleren.

    ![Data analytics verwerking taak notfications hub](./media/stream-analytics-create-a-job/2-stream-analytics-create-a-job.png)  

    ![Azure portal gegevens analytics taakverwerking maken taak](./media/stream-analytics-create-a-job/5-stream-analytics-create-a-job.png)  

5.  Het nieuwe project wordt weergegeven met de status **gemaakt**. U ziet dat de knop **Start** is uitgeschakeld. Moet u de functie input, query- en uitvoer voordat de taak start.

    ![Analytics taak verwerkingstaak Status](./media/stream-analytics-create-a-job/3-stream-analytics-create-a-job.png)  

    ![Azure portal gegevens analytics verwerking status van taak](./media/stream-analytics-create-a-job/6-stream-analytics-create-a-job.png)  

## <a name="get-help"></a>Help-informatie opvragen
Probeer onze [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics) voor verdere ondersteuning

## <a name="next-steps"></a>Volgende stappen

- [Inleiding tot Analytics Azure Stream](stream-analytics-introduction.md)
- [Aan de slag met Azure Stream Analytics](stream-analytics-get-started.md)
- [Schaal Azure Stream Analytics taken](stream-analytics-scale-jobs.md)
- [Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure Stream Analytics beheer REST API: naslag](https://msdn.microsoft.com/library/azure/dn835031.aspx)
