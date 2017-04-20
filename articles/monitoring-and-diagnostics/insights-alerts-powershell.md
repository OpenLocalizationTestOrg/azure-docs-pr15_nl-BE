<properties
    pageTitle="PowerShell gebruiken voor het maken van waarschuwingen voor Azure services | Microsoft Azure"
    description="PowerShell gebruiken voor het maken van Azure waarschuwingen die meldingen of automatisering activeren kunnen als de opgegeven voorwaarden is voldaan."
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

# <a name="use-powershell-to-create-alerts-for-azure-services"></a>PowerShell gebruiken voor het maken van waarschuwingen voor Azure services

> [AZURE.SELECTOR]
- [Portal](insights-alerts-portal.md)
- [PowerShell](insights-alerts-powershell.md)
- [CLI](insights-alerts-command-line-interface.md)

## <a name="overview"></a>Overzicht

In dit artikel wordt beschreven hoe u voor het instellen van Azure waarschuwingen met PowerShell.  

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


Voor meer informatie kunt u altijd typen ```get-help``` en vervolgens de PowerShell-opdracht waarover u help wilt.

## <a name="create-alert-rules-in-powershell"></a>Waarschuwingsregels maken in PowerShell

1. Log in op Azure.   

    ```PowerShell
    Login-AzureRmAccount

    ```

2. Een overzicht van de abonnementen er beschikbaar zijn. Controleer of u werkt met het juiste abonnement. Zo niet, stel deze in op het juiste antwoord met de uitvoer van `Get-AzureRmSubscription`.

    ```PowerShell
    Get-AzureRmSubscription
    Get-AzureRmContext
    Set-AzureRmContext -SubscriptionId <subscriptionid>
    ```

3.  U kunt bestaande regels op een resourcegroep gebruiken met de volgende opdracht:

    ```PowerShell
    Get-AzureRmAlertRule -ResourceGroup <myresourcegroup> -DetailedOutput
    ```

4. Als u een regel maakt, moet u eerst belangrijke stukjes informatie hebben. 
    - De **Resource-ID** voor de resource die u wilt een waarschuwing instellen voor
    - De **metrische definities** beschikbaar voor die bron

    Een manier om de Resource-ID is de Azure portal te gebruiken. Als dat de resource al is gemaakt, selecteert u deze in de portal. In het volgende blad, klik op *Eigenschappen* in de sectie *Instellingen* . De RESOURCE-ID is een veld in het volgende blad. Een andere manier is [Azure Resource Explorer](https://resources.azure.com/).

    Een voorbeeld van de resource-id voor een web app is

    ```
    /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename
    ```

    U kunt `Get-AzureRmMetricDefinition` om de lijst van alle definities van metrische gegevens voor een specifieke resource weer te geven.

    ```PowerShell
    Get-AzureRmMetricDefinition -ResourceId <resource_id>
    ```

    In het volgende voorbeeld wordt een tabel met de naam van de kosten en de eenheid voor deze metrische gegevens gegenereerd.

    ```PowerShell
    Get-AzureRmMetricDefinition -ResourceId <resource_id> | Format-Table -Property Name,Unit

    ```
    Een volledige lijst van beschikbare opties voor Get-AzureRmMetricDefinition is beschikbaar door het uitvoeren van Get-MetricDefinitions.


5. In het volgende voorbeeld wordt een bericht op de website van resource. De waarschuwing triggers wanneer er voortdurend al het verkeer ontvangen voor 5 minuten en opnieuw wanneer er geen verkeer ontvangen gedurende 5 minuten.

    ```PowerShell
    Add-AzureRmMetricAlertRule -Name myMetricRuleWithWebhookAndEmail -Location "East US" -ResourceGroup myresourcegroup -TargetResourceId /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename -MetricName "BytesReceived" -Operator GreaterThan -Threshold 2 -WindowSize 00:05:00 -TimeAggregationOperator Total -Description "alert on any website activity"

    ```

6. Webhook maken of e-mailbericht verzenden wanneer een waarschuwing wordt gegenereerd, maakt u eerst het e-mailadres en/of webhooks. Vervolgens onmiddellijk de regel achteraf met de - tag acties en maken zoals in het volgende voorbeeld wordt weergegeven. Kunt u niet koppelen webhook of e-mailberichten met regels via PowerShell al gemaakt.


    ```PowerShell
    $actionEmail = New-AzureRmAlertRuleEmail -CustomEmail myname@company.com
    $actionWebhook = New-AzureRmAlertRuleWebhook -ServiceUri https://www.contoso.com?token=mytoken

    Add-AzureRmMetricAlertRule -Name myMetricRuleWithWebhookAndEmail -Location "East US" -ResourceGroup myresourcegroup -TargetResourceId /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename -MetricName "BytesReceived" -Operator GreaterThan -Threshold 2 -WindowSize 00:05:00 -TimeAggregationOperator Total -Actions $actionEmail, $actionWebhook -Description "alert on any website activity"
    ```


7. Maakt u een waarschuwing wordt gegenereerd op een bepaalde voorwaarde in het logboek voor faxactiviteit met opdrachten van het volgende formulier

    ```PowerShell
    $actionEmail = New-AzureRmAlertRuleEmail -CustomEmail myname@company.com
    $actionWebhook = New-AzureRmAlertRuleWebhook -ServiceUri https://www.contoso.com?token=mytoken

    Add-AzureRmLogAlertRule -Name myLogAlertRule -Location "East US" -ResourceGroup myresourcegroup -OperationName microsoft.web/sites/start/action -Status Succeeded -TargetResourceGroup resourcegroupbeingmonitored -Actions $actionEmail, $actionWebhook
    ```

    De OperationName - komt overeen met een type gebeurtenis in het logboek voor faxactiviteit. Voorbeelden zijn *Microsoft.Compute/virtualMachines/delete* en *microsoft.insights/diagnosticSettings/write*.

    De PowerShell-opdracht [Get-AzureRmProviderOperation](https://msdn.microsoft.com/library/mt603720.aspx) kunt u een lijst met mogelijke operationNames. Ook kunt u de portal Azure query van het gebeurtenissenlogboek en zoeken naar specifieke bewerkingen die u wilt maken van een waarschuwing voor het verleden. De bewerkingen in de logboekweergave grafische van de beschrijvende naam weergegeven. Kijk in de JSON voor de vermelding en de waarde OperationName uitlichten.   

8. Controleer of dat je meldingen correct zijn gemaakt door te kijken naar de afzonderlijke regels.

    ```PowerShell
    Get-AzureRmAlertRule -Name myMetricRuleWithWebhookAndEmail -ResourceGroup myresourcegroup -DetailedOutput

    Get-AzureRmAlertRule -Name myLogAlertRule -ResourceGroup myresourcegroup -DetailedOutput
    ```

9. Uw waarschuwingen verwijderen. Deze opdrachten verwijdert u de regels die eerder in dit artikel.

    ```PowerShell
    Remove-AzureRmAlertRule -ResourceGroup myresourcegroup -Name myrule
    Remove-AzureRmAlertRule -ResourceGroup myresourcegroup -Name myMetricRuleWithWebhookAndEmail
    Remove-AzureRmAlertRule -ResourceGroup myresourcegroup -Name myLogAlertRule
    ```

## <a name="next-steps"></a>Volgende stappen

* [Een overzicht van Azure monitoring](monitoring-overview.md) met inbegrip van de soorten informatie te verzamelen en controleren.
* Meer informatie over het [configureren van de webhooks in de waarschuwingen](insights-webhooks-alerts.md).
* Meer informatie over [Azure automatisering Runbooks](..\automation\automation-starting-a-runbook.md).
* Een [overzicht van het verzamelen van diagnostische logboeken](monitoring-overview-of-diagnostic-logs.md) voor het verzamelen van gedetailleerde statistieken van hoge frequentie op uw service te krijgen.
* Een [overzicht van de collectie parameters](insights-how-to-customize-monitoring.md) om ervoor te zorgen dat de service is beschikbaar en responsieve opvragen.
