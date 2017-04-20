<properties
    pageTitle="Ontvangen van meldingen voor Azure services | Microsoft Azure"
    description="Een melding wanneer waarschuwingsregels voorwaarden wordt voldaan."
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

# <a name="receive-alert-notifications"></a>Meldingen ontvangen

U krijgt een waarschuwing op basis van controle metrics voor of gebeurtenissen op uw Azure services.

Voor een waarschuwingsregel op een metrische waarde wanneer de waarde van een opgegeven metric een drempel is toegewezen kruist, de waarschuwingsregel actief wordt en een bericht kunt verzenden. Een regel kan een melding sturen op *elke* gebeurtenis, of, wanneer een bepaald aantal gebeurtenissen plaatsvinden voor een waarschuwingsregel van gebeurtenissen.

Wanneer u een waarschuwingsregel maakt, kunt u opties voor het verzenden van een e-mailbericht naar de beheerder en CO-beheerders of een andere beheerder die u kunt opgeven. Een e-mailmelding wordt verzonden wanneer de regel wordt geactiveerd en wanneer een waarschuwing voorwaarde is opgelost.

U kunt de [REST API](https://msdn.microsoft.com/library/azure/dn931945.aspx) of de [SDK voor .NET](https://www.nuget.org/packages/Microsoft.Azure.Insights/) configureren en informatie opvragen over waarschuwingsregels via programmacode.

## <a name="create-an-alert-rule"></a>Een waarschuwingsregel maken

1. Klik op **Bladeren**en vervolgens een bron die u geïnteresseerd bent in controle in de [portal](https://portal.azure.com/).

2. Klik op de tegel **waarschuwingsregels** in de lens **bewerkingen** .

3. Klik op de opdracht **melding toevoegen** .

    ![Een waarschuwing toevoegen](./media/insights-receive-alert-notifications/Insights_AddAlert.png)

4. U kunt uw Waarschuwing regel een naam en kies een beschrijving die wordt weergegeven in de e-mailmelding.

5. Wanneer u kiest u **Metrisch** kiest u een voorwaarde en waarde drempel voor de metric. Dit is de periode waarin Azure voor monitor en uitgezet alert activiteit.

    ![Voorwaarde en drempel](./media/insights-receive-alert-notifications/Insights_ConditionAndThreshold.png)

6. U kunt ook de **gebeurtenissen**kiezen en u een melding krijgen wanneer een bepaalde gebeurtenis plaatsvindt.

    ![Gebeurtenissen](./media/insights-receive-alert-notifications/Insights_Events.png)

7. Ten slotte kunt u e-mailberichten verzenden naar beheerders die verantwoordelijk zijn.

Nadat u op **Opslaan**klikt, binnen enkele minuten zult u op de hoogte wanneer de metric die u kiest de drempelwaarde wordt overschreden.

## <a name="managing-your-alert-rules"></a>Waarschuwingsregels beheren

Als u een waarschuwingsregel hebt gemaakt, kunt u een voorvertoning van uw melding bekijken drempel ten opzichte van de metric van de vorige dag.

![Gebeurtenissen](./media/insights-receive-alert-notifications/Insights_EditAlert.png)


Natuurlijk kunt u **deze waarschuwingsregel, en- of **uitschakelen** dat als u tijdelijk niet langer meldingen over het** bewerken.

## <a name="next-steps"></a>Volgende stappen

* [Webhooks op je meldingen configureren](insights-webhooks-alerts.md) voor meldingen van verschillende kanalen route
* [Maatstaven voor monitor-service](insights-how-to-customize-monitoring.md) om te controleren of dat de service beschikbaar is en reageert.
* [Inschakelen, controle en diagnose](insights-how-to-use-diagnostics.md) voor het verzamelen van gedetailleerde statistieken van hoge frequentie op uw service.
* [Beschikbaarheid van de monitor en de reactiesnelheid van elke webpagina](../application-insights/app-insights-monitor-web-app-availability.md) met toepassing inzichten, zodat u of uw pagina controleren kunt is niet beschikbaar.
* [Prestaties van de toepassing controleren](../application-insights/app-insights-azure-web-apps.md) als u wilt weten precies hoe uw code uitvoeren in de cloud.
* [Gebeurtenissen bekijken en controleren van Logboeken](insights-debugging-with-events.md) voor meer informatie over alles dat is er gebeurd met uw service.
* [Track service gezondheid](insights-service-health.md) om erachter te komen wanneer Azure heeft ondervonden prestaties afbraak- of -service-onderbrekingen.
