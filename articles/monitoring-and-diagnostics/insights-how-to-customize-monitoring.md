<properties
    pageTitle="Overzicht van parameters in Microsoft Azure | Microsoft Azure"
    description="Informatie over het aanpassen van de controle van de grafieken in Azure."
    authors="rboucher"
    manager="carolz"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/08/2015"
    ms.author="robb"/>

# <a name="overview-of-metrics-in-microsoft-azure"></a>Overzicht van parameters in Microsoft Azure

Alle Azure services volgen sleutel waarmee u de gezondheid, prestaties, beschikbaarheid en gebruik van de services te controleren. U kunt deze gegevens bekijken in de portal voor Azure en kun je de [REST API](https://msdn.microsoft.com/library/azure/dn931930.aspx) of [.NET SDK](https://www.nuget.org/packages/Microsoft.Azure.Insights/) om via programmacode toegang krijgen tot de volledige set parameters.

Voor bepaalde services moet u diagnostics inschakelen om parameters te zien. Voor anderen, zoals virtuele machines, krijgt u een eenvoudige reeks maatstaven, maar nodig hebt om de volledige hoge frequentie metrics. Zie [controle en diagnose inschakelen](insights-how-to-use-diagnostics.md) voor meer informatie.

## <a name="using-monitoring-charts"></a>Werken met grafieken van monitoring

U kunt een van de statistieken grafiek ze over een periode die u kiest.

1. Klik op **Bladeren**en vervolgens een bron die u geïnteresseerd bent in controle in de [Portal Azure](https://portal.azure.com/).

2. **De controlesectie** bevat de meest belangrijke metrische gegevens voor elke resource Azure. Een web app heeft bijvoorbeeld **aanvragen en fouten**, waar het als een virtuele machine zou hebben op het **percentage van de CPU** en **schijf lezen en schrijven**:  ![Monitoring lens](./media/insights-how-to-customize-monitoring/Insights_MonitoringChart.png)

3. Te klikken op de grafiek ziet u de **Metric** blade. Op het blad, naast het diagram, is een tabel waarin aggregaties bedrijfsparameters (zoals gemiddelde, minimum en maximum, gedurende de periode die u hebt gekozen). Onder die zijn de regels voor de resource.
    ![Metrische blade](./media/insights-how-to-customize-monitoring/Insights_MetricBlade.png)

4. Voor het aanpassen van de lijnen die worden weergegeven, klikt u op de knop **bewerken** in de grafiek of de opdracht **grafiek bewerken** in de metrische blade.

5. Op het blad Query bewerken kunt u drie dingen doen:
    - Het tijdsbereik wijzigen
    - Overschakelen van de weergave tussen de balk en lijn
    - Kies verschillende metics ![Query bewerken](./media/insights-how-to-customize-monitoring/Insights_EditQuery.png)

6. Wijzigen van het tijdsbereik is net zo eenvoudig als een ander bereik (bijvoorbeeld het **Afgelopen uur**) te selecteren en te klikken op **Opslaan** onder aan het blad. U kunt ook **aangepaste**, waarmee u een periode in de afgelopen twee weken te kiezen. Zo ziet u de gehele twee weken of, slechts 1 uur van gisteren. Typ in het tekstvak Geef een ander uur.
    ![Aangepaste periode](./media/insights-how-to-customize-monitoring/Insights_CustomTime.png)

7. Onder het tijdsbereik kiest u kanaal een willekeurig aantal cijfers in de grafiek weergeven.

8. Wanneer u opslaan op worden uw wijzigingen opgeslagen voor die resource. Bijvoorbeeld, als u beschikt over twee virtuele machines en een grafiek op een wijzigt, niet van invloed het andere.

## <a name="creating-side-by-side-charts"></a>Side-by-side grafieken maken

Met de krachtige aanpassingsmogelijkheden in de portal kunt u zo veel grafieken als u wilt toevoegen.

1. Klik in het menu aan de bovenkant van het blad van **...** op de **tegels toevoegen**:  
    ![Menu toevoegen](./media/insights-how-to-customize-monitoring/Insights_AddMenu.png)
2. Vervolgens, u kunt selecteren selecteert u een grafiek in de **Galerie** op de rechterkant van het scherm:  ![galerie](./media/insights-how-to-customize-monitoring/Insights_Gallery.png)
3. Als u de metric die u niet ziet, kunt u altijd toevoegen een van de vooraf ingestelde metrics en **bewerken** van de grafiek wilt weergeven van de metric die u nodig hebt.

## <a name="monitoring-usage-quotas"></a>Gebruik van quota's controleren

De meeste zien u trends in de tijd, maar bepaalde gegevens, zoals het gebruik van quota's point-in-time informatie aan een drempelwaarde zijn.

U ziet ook gebruik van quota op het blad voor resources die quota's hebt:

![Gebruik](./media/insights-how-to-customize-monitoring/Insights_UsageChart.png)

Net als met metrics, kunt u de [REST API](https://msdn.microsoft.com/library/azure/dn931963.aspx) of de [SDK voor .NET](https://www.nuget.org/packages/Microsoft.Azure.Insights/) via programmering toegang krijgen tot de volledige set van het gebruik van quota.

## <a name="next-steps"></a>Volgende stappen

* [Meldingen ontvangen](insights-receive-alert-notifications.md) wanneer er een metric een drempel overschrijdt.
* [Inschakelen, controle en diagnose](insights-how-to-use-diagnostics.md) voor het verzamelen van gedetailleerde statistieken van hoge frequentie op uw service.
* [Aantal exemplaar automatisch geschaald](insights-how-to-scale.md) om ervoor te zorgen dat de service beschikbaar is en reageert.
* [Prestaties van de toepassing controleren](../application-insights/app-insights-azure-web-apps.md) als u wilt weten precies hoe uw code uitvoeren in de cloud.
* [Toepassing inzichten voor JavaScript-toepassingen en webpagina's](../application-insights/app-insights-web-track-usage.md) gebruiken om analytics client over de browsers die webpagina's bezoekt.
* [Beschikbaarheid van de monitor en de reactiesnelheid van elke webpagina](../application-insights/app-insights-monitor-web-app-availability.md) met toepassing inzichten, zodat u of uw pagina controleren kunt is niet beschikbaar.
