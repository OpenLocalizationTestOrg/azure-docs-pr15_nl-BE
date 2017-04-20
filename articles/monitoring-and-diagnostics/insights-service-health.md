<properties
    pageTitle="Azure service gezondheid met Azure Monitor activiteitenlogboeken bijhouden | Microsoft Azure"
    description="Uitzoeken wanneer Azure heeft ondervonden prestaties afbraak- of -service-onderbrekingen. "
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
    ms.date="10/20/2016"
    ms.author="robb"/>

# <a name="track-azure-service-health-using-azure-monitor-activity-logs"></a>Azure service gezondheid met Azure Monitor activiteitenlogboeken bijhouden

Azure publicizes telkens wanneer er een service wordt onderbroken of de prestaties afbraak is. Kunt u deze gebeurtenissen in de portal Azure en kun je de [REST API](https://msdn.microsoft.com/library/azure/dn931927.aspx) of de [SDK voor .NET](https://www.nuget.org/packages/Microsoft.Azure.Insights/) via programmering toegang krijgen tot de volledige reeks gebeurtenissen.

## <a name="browse-the-service-health-logs-for-your-subscription"></a>Door de health service, logboeken voor uw abonnement bladeren

1. Aanmelden bij de [Azure Portal](https://portal.azure.com/).

2. Op de **introductiepagina** ziet u een tegel **health Service**genoemd. Klik op het.

    ![Home](./media/insights-service-health/Insights_Home.png)

3. U ziet een lijst met alle regio's in Azure. Klik op een regio om het logboek voor faxactiviteit query waarin service incidenten die van invloed zijn geweest op een van uw abonnementen in de afgelopen 24 uur.

    ![Activiteit logboek abonnement Service gezondheid](./media/insights-service-health/AzureActivityLogServiceHealth3.png)

4. Door te klikken op die gebeurtenis in de tabel ziet u de details van een individuele incidenten.

5. Wijzig de **Timespan** klikken voor een langere periode.

## <a name="next-steps"></a>Volgende stappen

* [Beschikbaarheid van de monitor en de reactiesnelheid van elke webpagina](../application-insights/app-insights-monitor-web-app-availability.md) met toepassing inzichten, zodat u of uw pagina controleren kunt is niet beschikbaar.
