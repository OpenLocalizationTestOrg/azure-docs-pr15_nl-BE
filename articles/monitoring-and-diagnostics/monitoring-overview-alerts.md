<properties
    pageTitle="Overzicht van waarschuwingen in Microsoft Azure | Microsoft Azure"
    description="Waarschuwingen kunt u resource Azure metrics, gebeurtenissen of logboeken controleren en de hoogte gesteld wanneer een bepaalde voorwaarde is voldaan."
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
    ms.date="09/24/2016"
    ms.author="robb"/>

# <a name="overview-of-alerts-in-microsoft-azure"></a>Overzicht van waarschuwingen in Microsoft Azure


In dit artikel wordt beschreven welke waarschuwingen zijn hun voordelen, aan de slag met het gebruik van deze.  

## <a name="what-are-alerts"></a>Wat zijn meldingen?
Waarschuwingen zijn een methode van controle Azure resource maatstaven, gebeurtenissen, of zich aanmeldt en vervolgens wordt gewaarschuwd wanneer een bepaalde voorwaarde wordt voldaan.

U kunt waarschuwingen op basis van ontvangen:

- **Metrische waarden**: deze waarschuwing wordt geactiveerd wanneer de waarde van een opgegeven metric kruist een drempel die u in beide richtingen toewijst. Dat wil zeggen, er wordt zowel als eerst aan de voorwaarde is voldaan en klik daarna als die voorwaarde is niet langer wordt voldaan.
- **De gebeurtenissen in activiteit**: deze waarschuwing kan worden geactiveerd bij elke gebeurtenis of alleen wanneer een bepaald aantal gebeurtenissen plaatsvinden.

## <a name="alerts-in-different-azure-services"></a>Waarschuwingen in verschillende Azure services

Waarschuwingen zijn beschikbaar naar verschillende services, met inbegrip van:

- **Inzichten van toepassing**: Hiermee test en metric waarschuwingen voor websites. Zie [waarschuwingen in toepassing inzichten](../application-insights/app-insights-alerts.md) en [beschikbaarheid van de Monitor en de reactiesnelheid van een website](../application-insights/app-insights-monitor-web-app-availability.md).
- **Logboek Analytics (Operations Management Suite)**: de routering van diagnostische logboeken logboek Analytics. Operations Management Suite kunnen metric, het logboek en andere typen waarschuwingen. Zie [waarschuwingen in logboek Analytics](../log-analytics/log-analytics-alerts.md)voor meer informatie.  
- **Azure Monitor**: kunt u waarschuwingen op basis van metrische waarden en de gebeurtenissen in activiteit. Azure-Monitor biedt de [Azure Monitor REST API](https://msdn.microsoft.com/library/dn931943.aspx).  Zie voor meer informatie [via de portal Azure, PowerShell, of de interface met opdrachtregel om waarschuwingen te maken](insights-alerts-portal.md).

## <a name="alert-actions"></a>Acties bij waarschuwingen
U kunt een waarschuwing als volgt configureren:

- E-mailberichten verzenden naar de servicebeheerder, CO-beheerders of extra e-mailadressen die u opgeeft.
- Bel een webhook waarmee u extra automatisering acties starten.

 ![Signalen toegelicht](./media/monitoring-overview-alerts/AlertsOverviewResource3.png)


## <a name="next-steps"></a>Volgende stappen

Informatie ophalen over waarschuwingsregels en configureren met:

- [Azure portal](insights-alerts-portal.md)
- [PowerShell](insights-alerts-powershell.md)
- [Opdrachtregelinterface (CLI)](insights-alerts-command-line-interface.md)
- [Azure Monitor REST API](https://msdn.microsoft.com/library/azure/dn931945.aspx)
