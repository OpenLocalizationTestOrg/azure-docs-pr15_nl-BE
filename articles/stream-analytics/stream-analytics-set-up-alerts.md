<properties
    pageTitle="Waarschuwingen instellen voor query's in een Stream Analytics | Microsoft Azure"
    description="Wat stroom Analytics waarschuwen"
    keywords="waarschuwingen instellen"
    services="stream-analytics"
    documentationCenter=""
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


# <a name="set-up-alerts-for-azure-stream-analytics-jobs"></a>Waarschuwingen voor Azure Stream Analytics taken instellen

## <a name="introduction-monitor-page"></a>: Introductiepagina Monitor

U kunt waarschuwingen instellen voor een waarschuwing wanneer een metric bereikt een voorwaarde die u opgeeft.

Bijvoorbeeld, "als uitvoer gebeurtenissen voor de laatste 15 minuten is < 100, e-mailbericht verzenden met e-mail-id: xyz@company.com”.

Regels kunnen worden ingesteld op bijhouden via de portal of via Logboeken gegevens geconfigureerde [programmatisch](https://code.msdn.microsoft.com/windowsazure/Receive-Email-Notifications-199e2c9a) kunnen worden.

## <a name="set-up-alerts-through-the-azure-classic-portal"></a>Waarschuwingen via de klassieke Azure-Portal instellen

Er zijn twee manieren meldingen in de klassieke Azure portal wilt instellen:  

1.  Het tabblad **Monitor** van de Stream Analytics taak  
2.  Het logboek voor bewerkingen in de services Management  

## <a name="set-up-alert-through-the-monitor-tab-of-the-job-in-the-portal"></a>Waarschuwing via het tabblad Monitor van de taak in de portal instellen

1.  De metric op het tabblad monitor te selecteren en klikt u op de knop **Regel toevoegen** in de onderkant van het dashboard en uw regels instellen.  

    ![Dashboard](./media/stream-analytics-set-up-alerts/01-stream-analytics-set-up-alerts.png)  

2.  Definieer de naam en beschrijving van de waarschuwing  

    ![Regel maken](./media/stream-analytics-set-up-alerts/02-stream-analytics-set-up-alerts.png)  

3.  Voer de drempels, venster alert evaluatie en de acties voor de waarschuwing  

    ![Voorwaarden definiëren](./media/stream-analytics-set-up-alerts/03-stream-analytics-set-up-alerts.png)  

## <a name="set-up-alerts-through-the-operations-logs"></a>Waarschuwingen via de logboeken bewerkingen instellen

1.  Ga naar het tabblad **Alerts** in Management Services in de [Klassieke Azure-Portal](https://manage.windowsazure.com).  
2.  Klik op **regel toevoegen**  

    ![Criteria](./media/stream-analytics-set-up-alerts/04-stream-analytics-set-up-alerts.png)  

3.  Definieer de naam en beschrijving van de waarschuwing. Selecteer 'Stream Analytics' als het Type Service en de naam van de taak als de servicenaam.  

    ![Waarschuwing definiëren](./media/stream-analytics-set-up-alerts/05-stream-analytics-set-up-alerts.png)  

## <a name="set-up-alerts-in-the-azure-portal"></a>Waarschuwingen in Azure portal instellen ##

In de Azure portal, Ga naar het Analytics Stream-project dat u geïnteresseerd bent in een waarschuwing op en klik op de sectie **controleren** .  In het blad **Metric** die wordt geopend, klikt u op de opdracht **melding toevoegen** .

  ![Azure portal-instellingen](./media/stream-analytics-set-up-alerts/06-stream-analytics-set-up-alerts.png)  

U kunt uw Waarschuwing regel een naam en kies een beschrijving die wordt weergegeven in de e-mailmelding.

Wanneer u kiest u Metrisch kiest u een voorwaarde en waarde drempel voor de metric.

  ![Azure portal select metric](./media/stream-analytics-set-up-alerts/07-stream-analytics-set-up-alerts.png)  

Zie voor meer informatie over het configureren van meldingen in het portal voor Azure [meldingen ontvangen](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).  

## <a name="get-help"></a>Help-informatie opvragen
Probeer onze [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics) voor verdere ondersteuning

## <a name="next-steps"></a>Volgende stappen

- [Inleiding tot Analytics Azure Stream](stream-analytics-introduction.md)
- [Aan de slag met Azure Stream Analytics](stream-analytics-get-started.md)
- [Schaal Azure Stream Analytics taken](stream-analytics-scale-jobs.md)
- [Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure Stream Analytics beheer REST API: naslag](https://msdn.microsoft.com/library/azure/dn835031.aspx)
