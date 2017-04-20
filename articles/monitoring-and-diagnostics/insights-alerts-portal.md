<properties
    pageTitle="Azure portal gebruiken voor het maken van waarschuwingen voor Azure services | Microsoft Azure"
    description="Via de portal Azure Azure waarschuwingen die meldingen of automatisering activeren kunnen als de opgegeven voorwaarden is voldaan."
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
    ms.date="09/23/2016"
    ms.author="robb"/>

# <a name="use-azure-portal-to-create-alerts-for-azure-services"></a>Azure portal gebruiken voor het maken van waarschuwingen voor Azure services

> [AZURE.SELECTOR]
- [Portal](insights-alerts-portal.md)
- [PowerShell](insights-alerts-powershell.md)
- [CLI](insights-alerts-command-line-interface.md)

## <a name="overview"></a>Overzicht

In dit artikel wordt beschreven hoe u Azure meldingen met de Azure portal wilt instellen.   

U krijgt een waarschuwing op basis van controle metrics voor of gebeurtenissen op uw Azure services.

- **Metrische waarden** - triggers waarschuwen wanneer de waarde van een opgegeven metric kruist een drempel die u in beide richtingen toewijst. Dat wil zeggen, er wordt zowel als eerst aan de voorwaarde is voldaan en klik daarna als die voorwaarde is niet langer wordt voldaan.    
- **De gebeurtenissen in activiteit** - een waarschuwing kan worden geactiveerd bij *elke* gebeurtenis of, wanneer een bepaald aantal gebeurtenissen zich voordoen.


U kunt het volgende doen wanneer er wordt een waarschuwing:

- e-mailberichten verzenden naar de beheerder en CO-beheerders
- e-mail verzenden naar extra e-mails die u opgeeft.
- Bel een webhook
- Start de uitvoering van een Azure runbook (alleen vanuit Azure portal)

U kunt configureren en informatie over regels met

- [Azure portal](insights-alerts-portal.md)
- [PowerShell](insights-alerts-powershell.md)
- [opdrachtregelinterface (CLI)](insights-alerts-command-line-interface.md)
- [Azure Monitor REST API](https://msdn.microsoft.com/library/azure/dn931945.aspx)


## <a name="create-an-alert-rule-on-a-metric-with-the-azure-portal"></a>Een waarschuwingsregel maken op een waarde met de Azure portal

1. Zoek in de [portal](https://portal.azure.com/), de resource die u wilt controleren en selecteer deze.

2. Selecteer **waarschuwingen** of **waarschuwingsregels** onder controle. De tekst en het pictogram kunnen enigszins variëren voor verschillende bronnen.  

    ![Monitoring](./media/insights-alerts-portal/AlertRulesButton.png)


3. Selecteer de opdracht **melding toevoegen** en vul de velden in.

    ![Een waarschuwing toevoegen](./media/insights-alerts-portal/AddAlertOnlyParamsPage.png)

4. **De naam** van de waarschuwing regel en kies een **Beschrijving**, waarin ook in e-mailberichten.
5. Selecteer de **Metric** die u wilt controleren en kies vervolgens een waarde **en de **drempel** ** voor de metric. Ook de **periode** waarin de metrische regel moet worden voldaan voordat de waarschuwing triggers gekozen. Dus bijvoorbeeld als u de periode van 'PT5M' en de waarschuwing wordt gezocht naar de CPU boven de 80%, de waarschuwing wordt geactiveerd wanneer de CPU voortdurend boven 80% gedurende 5 minuten is. Zodra de eerste trigger wordt wordt er opnieuw gegenereerd wanneer de CPU onder 80% gedurende 5 minuten blijft. De CPU-meting vindt plaats elke 1 minuut.   

6. **... E-eigenaren** controleren als u wilt dat beheerders en CO-beheerders worden gestuurd wanneer de waarschuwing wordt geactiveerd.

7. Als u extra e-mailberichten worden geïnformeerd wanneer de waarschuwing wordt geactiveerd, kunt u deze in het veld **beheerder aanvullende email(s)** toevoegen. Meerdere e-mailberichten te scheiden met puntkomma's-*email@contoso.com;email2@contoso.com*

8. In een geldige URI in het veld **Webhook** plaatsen als u wilt dat deze wordt aangeroepen wanneer de waarschuwing wordt geactiveerd.

9. Als u met automatisering Azure, kunt u een Runbook moet worden uitgevoerd wanneer de waarschuwing wordt geactiveerd.

10. Selecteer **OK** wanneer u klaar bent voor het maken van de waarschuwing.   

Binnen enkele minuten de waarschuwing is actief en triggers zoals eerder is beschreven.

## <a name="managing-your-alerts"></a>Uw waarschuwingen beheren

Als u een waarschuwing hebt gemaakt, kunt u deze selecteren en:

- Een grafiek waarin de metrische drempel en de werkelijke waarden van de vorige dag weergeven.
- Bewerken of verwijderen.
- **Uitschakelen** of **inschakelen** dat als u tijdelijk stoppen of hervatten ontvangst van meldingen voor deze waarschuwing.



## <a name="next-steps"></a>Volgende stappen

* [Een overzicht van Azure monitoring](monitoring-overview.md) met inbegrip van de soorten informatie te verzamelen en controleren.
* Meer informatie over het [configureren van de webhooks in de waarschuwingen](insights-webhooks-alerts.md).
* Meer informatie over [Azure automatisering Runbooks](..\automation\automation-starting-a-runbook.md).
* Een [overzicht van de diagnostische logboeken](monitoring-overview-of-diagnostic-logs.md) en het verzamelen van gedetailleerde statistieken van hoge frequentie op uw service.
* Een [overzicht van de collectie parameters](insights-how-to-customize-monitoring.md) om ervoor te zorgen dat de service is beschikbaar en responsieve opvragen.
