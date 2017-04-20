<properties
    pageTitle="Cross-platform Command Line Interface (CLI) gebruiken voor het maken van waarschuwingen voor Azure services | Microsoft Azure"
    description="Gebruik de command line interface maken Azure waarschuwingen die meldingen of automatisering activeren kunnen als de opgegeven voorwaarden is voldaan."
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
    ms.date="10/24/2016"
    ms.author="robb"/>

# <a name="use-the-cross-platform-command-line-interface-cli-to-create-alerts-for-azure-services"></a>Cross-platform Command Line Interface (CLI) gebruiken voor het maken van waarschuwingen voor Azure services

> [AZURE.SELECTOR]
- [Portal](insights-alerts-portal.md)
- [PowerShell](insights-alerts-powershell.md)
- [CLI](insights-alerts-command-line-interface.md)

## <a name="overview"></a>Overzicht

In dit artikel wordt beschreven hoe u Azure meldingen met behulp van de Command Line Interface (CLI) wilt instellen.

>[AZURE.NOTE] Azure Monitor is de nieuwe naam voor de zogenoemde "Azure inzichten" tot 25 september 2016. Echter bevatten de naamruimten en dus ook de onderstaande opdrachten de 'inzichten'.

U krijgt een waarschuwing op basis van controle metrics voor of gebeurtenissen op uw Azure services.

- **Metrische waarden** - triggers waarschuwen wanneer de waarde van een opgegeven metric kruist een drempel die u in beide richtingen toewijst. Dat wil zeggen, er wordt zowel als eerst aan de voorwaarde is voldaan en klik daarna als die voorwaarde is niet langer wordt voldaan.    
- **De gebeurtenissen in activiteit** - een waarschuwing kan worden geactiveerd bij *elke* gebeurtenis of, wanneer een bepaald aantal gebeurtenissen zich voordoen.

U kunt het volgende doen wanneer er wordt een waarschuwing:

- e-mailberichten verzenden naar de beheerder en CO-beheerders
- e-mail verzenden naar extra e-mails die u opgeeft.
- Bel een webhook
- Start de uitvoering van een Azure runbook (alleen vanuit Azure portal op dit moment)

U kunt configureren en informatie over regels met

- [Azure portal](insights-alerts-portal.md)
- [PowerShell](insights-alerts-powershell.md)
- [opdrachtregelinterface (CLI)](insights-alerts-command-line-interface.md)
- [Azure Monitor REST API](https://msdn.microsoft.com/library/azure/dn931945.aspx)


U kunt de help voor de opdrachten altijd ontvangen door een opdracht te typen en stellen - help aan het einde. Bijvoorbeeld:

    ```console
    azure insights alerts -help
    azure insights alerts actions email create -help
    ```

## <a name="create-alert-rules-using-the-cli"></a>Met behulp van de CLI waarschuwingsregels maken

1. De voorwaarden en meld u aan bij Azure uitvoeren. Zie [voorbeelden van Azure Monitor CLI](insights-cli-samples.md). Kortom, de CLI installeren en uitvoeren van deze opdrachten. Zij krijgen u aangemeld, weergeven welke abonnementen u gebruikt en bereid u de Monitor Azure opdrachten kunt uitvoeren.


    ```console
    azure login
    azure account show
    azure config mode arm

    ```

2.  U kunt bestaande regels op een resourcegroep gebruiken het volgende formulier **azure inzichten waarschuwingen lijst regel** *[opties] &lt;resourceGroup&gt; *

    ```console
    azure insights alerts rule list myresourcegroupname

    ```
3. Als u een regel maakt, moet u eerst belangrijke stukjes informatie hebben.
    - De **Resource-ID** voor de resource die u wilt een waarschuwing instellen voor
    - De **metrische definities** beschikbaar voor die bron

    Een manier om de Resource-ID is de Azure portal te gebruiken. Als dat de resource al is gemaakt, selecteert u deze in de portal. In het volgende blad, klik op *Eigenschappen* in de sectie *Instellingen* . De *RESOURCE-ID* is een veld in het volgende blad. Een andere manier is [Azure Resource Explorer](https://resources.azure.com/).

    Een voorbeeld van de resource-id voor een web app is

    ```console
    /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename
    ```

    Als u een lijst van de beschikbare statistieken en eenheden voor deze parameters in het vorige voorbeeld van de resource, gebruikt de volgende CLI-opdracht:  

    ```console
    azure insights metrics list /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename PT1M
    ```

    _PT1M_ is de granulatie van de beschikbare meting (1 minuut intervallen). Met verschillende wisselkoersmodellen hebt u verschillende opties voor metrische.


4. U kunt een metric-waarschuwingsregel maken met een opdracht van de volgende vorm:

    **Azure inzichten waarschuwingen regel metric instellen** *[opties] &lt;ruleName&gt; &lt;locatie&gt; &lt;resourceGroup&gt; &lt;WindowSize,&gt; &lt;operator&gt; &lt;drempel&gt; &lt;targetResourceId&gt; &lt;metricName&gt; &lt;timeAggregationOperator&gt; *

    In het volgende voorbeeld wordt een bericht op de website van resource. De waarschuwing triggers wanneer er voortdurend al het verkeer ontvangen voor 5 minuten en opnieuw wanneer er geen verkeer ontvangen gedurende 5 minuten.

    ```console
    azure insights alerts rule metric set myrule eastus myreasourcegroup PT5M GreaterThan 2 /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename BytesReceived Total

    ```

5. Webhook maken of e-mailbericht verzenden wanneer er een waarschuwing wordt gestart, moet u eerst het e-mailadres en/of webhooks maken. Maak de regel onmiddellijk daarna. Kunt u niet koppelen webhook of e-mailberichten met regels met behulp van de CLI al gemaakt.

    ```console
    azure insights alerts actions email create --customEmails myemail@contoso.com

    azure insights alerts actions webhook create https://www.contoso.com

    azure insights alerts rule metric set myrulewithwebhookandemail eastus myreasourcegroup PT5M GreaterThan 2 /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename BytesReceived Total
    ```


6. Als u wilt een waarschuwing die wordt geactiveerd op een bepaalde voorwaarde in het logboek voor faxactiviteit maakt, kunt u het formulier gebruiken:

    **Meld u inzichten waarschuwingen regel instellen** *[opties] &lt;ruleName&gt; &lt;locatie&gt; &lt;resourceGroup&gt; &lt;operationName&gt; *

    Bijvoorbeeld

    ```console
    azure insights alerts rule log set myActivityLogRule eastus myresourceGroupName Microsoft.Storage/storageAccounts/listKeys/action
    ```

    De operationName komt overeen met het gebeurtenistype voor een vermelding in het logboek voor faxactiviteit. Voorbeelden zijn *Microsoft.Compute/virtualMachines/delete* en *microsoft.insights/diagnosticSettings/write*.

    De PowerShell-opdracht [Get-AzureRmProviderOperation](https://msdn.microsoft.com/library/mt603720.aspx) kunt u een lijst met mogelijke operationNames. Ook kunt u de portal Azure query van het gebeurtenissenlogboek en zoeken naar specifieke bewerkingen die u wilt maken van een waarschuwing voor het verleden. De bewerkingen in de logboekweergave grafische van de beschrijvende naam weergegeven. Kijk in de JSON voor de vermelding en de waarde OperationName uitlichten.   

7. U kunt controleren dat de waarschuwingen goed zijn gemaakt door te kijken op een afzonderlijke regel.

    ```console
    azure insights alerts rule list myresourcegroup --ruleName myrule
    ```

8. U regels verwijderen met een opdracht van het formulier:

    **inzichten waarschuwingen regel verwijderen** [opties] &lt;resourceGroup&gt; &lt;ruleName&gt;

    Deze opdrachten verwijdert u de regels die eerder in dit artikel hebt gemaakt.

    ```console
    azure insights alerts rule delete myresourcegroup myrule
    azure insights alerts rule delete myresourcegroup myrulewithwebhookandemail
    azure insights alerts rule delete myresourcegroup myActivityLogRule
    ```



## <a name="next-steps"></a>Volgende stappen

* [Een overzicht van Azure monitoring](monitoring-overview.md) met inbegrip van de soorten informatie te verzamelen en controleren.
* Meer informatie over het [configureren van de webhooks in de waarschuwingen](insights-webhooks-alerts.md).
* Meer informatie over [Azure automatisering Runbooks](..\automation\automation-starting-a-runbook.md).
* Een [overzicht van het verzamelen van diagnostische logboeken](monitoring-overview-of-diagnostic-logs.md) voor het verzamelen van gedetailleerde statistieken van hoge frequentie op uw service te krijgen.
* Een [overzicht van de collectie parameters](insights-how-to-customize-monitoring.md) om ervoor te zorgen dat de service is beschikbaar en responsieve opvragen.
