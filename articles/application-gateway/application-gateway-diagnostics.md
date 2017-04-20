<properties 
   pageTitle="Toegangs- en logbestanden en statistieken controleren voor Application Gateway | Microsoft Azure"
   description="Meer informatie over het inschakelen en beheren van Logboeken voor toegang en prestaties voor Application Gateway"
   services="application-gateway"
   documentationCenter="na"
   authors="amitsriva"
   manager="rossort"
   editor="tysonn"
   tags="azure-resource-manager"
/>
<tags 
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/26/2016"
   ms.author="amitsriva" />

# <a name="diagnostics-logging-and-metrics-for-application-gateway"></a>Diagnostische gegevens vastleggen en Metrics voor Application Gateway

Azure biedt de mogelijkheid voor het controleren van de resource met de registratie en statistieken

[**Logboekregistratie**](#enable-logging-with-powershell) - logboekregistratie kunt u prestaties, toegang en andere logboeken worden opgeslagen of van een resource voor controledoeleinden verbruikt.

[**Statistieken**](#metrics) - toepassingsgateway momenteel heeft een metric. Deze meetmethode kan de doorvoer van de toepassingsgateway in Bytes per seconde.

U kunt verschillende typen logboeken in Azure beheren en oplossen van problemen met Toepassingsgateways. Sommige van deze logboeken kunt openen via de portal en alle logboeken kunnen worden opgehaald uit een Azure blobopslag, en in andere hulpprogramma's, zoals [Analytics logboek](../log-analytics/log-analytics-azure-networking-analytics.md), Excel- en PowerBI weergegeven. U kunt meer informatie over de verschillende soorten logboeken in de volgende lijst:

- **Controlelogboekbestanden:** [Controlelogboeken Azure](../monitoring-and-diagnostics/insights-debugging-with-events.md) (voorheen bekend als operationele Logboeken) kunt u alle bewerkingen wordt ingediend bij het abonnement Azure en hun status weergeven. Controlelogboeken worden standaard ingeschakeld en kunnen worden weergegeven in de voorvertoning Azure-portal.
- **Toegang tot logboeken:** U kunt dit logboek application gateway toegangstijden weergeven en analyseren belangrijke informatie met inbegrip van beller IP, gevraagde URL reactie latentie, retourcode, bytes in-en uitchecken. Logboekbestanden verzameld om de 300 seconden. Dit logboek bevat één record per exemplaar van application gateway. Het toepassingsexemplaar gateway kan worden geïdentificeerd door de eigenschap 'instanceId'.
- **Prestatielogboeken:** U kunt dit logboek bekijken hoe het uitvoeren van exemplaren van application gateway. Dit logboek worden vastgelegd prestatiegegevens per exemplaar aanvraag totaalbedrag aangeboden, met inbegrip van de doorvoer in bytes, totaal aantal aanvragen die worden bediend, mislukte aanvraag count, telling van gezonde en beschadigde back-end-exemplaar. Prestatielogboek verzameld om de 60 seconden.
- **Firewall logs:** U kunt dit logboek bekijken van de aanvragen die zijn vastgelegd door de modus voor detectie of voorkoming van een toepassingsgateway die is geconfigureerd met web application firewall.

>[AZURE.WARNING] Logboeken zijn alleen beschikbaar voor resources in het implementatiemodel bronnenbeheerder geïmplementeerd. Kunt u zich aanmeldt voor bronnen in het klassieke implementatiemodel. Voor een beter begrip van de twee modellen, verwijzen naar het artikel [Wat Resource Manager-implementatie en klassieke implementatie](../resource-manager-deployment-model.md) .

## <a name="enable-logging-with-powershell"></a>Schakel logboekregistratie met PowerShell

Logboekregistratie is standaard ingeschakeld voor elke resource Resource Manager. U moet toegang en prestaties vastleggen om te beginnen met het verzamelen van de gegevens die beschikbaar zijn via deze logboeken inschakelen. Als logboekregistratie wilt inschakelen, raadpleegt u de volgende stappen uit: 

1. Opmerking van de account van uw opslag Resource-ID, waar de logboekgegevens worden opgeslagen. Dit zou zijn van het formulier: /subscriptions/\<subscriptionId\>/resourceGroups/\<Resourcegroepnaam\>/providers/Microsoft.Storage/storageAccounts/\<opslag accountnaam\>. Een account voor opslag in uw abonnement kan worden gebruikt. De voorbeeld-portal kunt u deze informatie vinden.

    ![Portal - Application Gateway diagnostische gegevens bekijken](./media/application-gateway-diagnostics/diagnostics1.png)

2. Opmerking de toepassingsgateway Resource-ID voor het registreren is ingeschakeld. Dit zou zijn van het formulier: /subscriptions/\<subscriptionId\>/resourceGroups/\<Resourcegroepnaam\>/providers/Microsoft.Network/applicationGateways/\<gateway toepassingsnaam\>. De voorbeeld-portal kunt u deze informatie vinden.

    ![Portal - Application Gateway diagnostische gegevens bekijken](./media/application-gateway-diagnostics/diagnostics2.png)

3. Diagnostische gegevens vastleggen met behulp van de volgende powershell-cmdlet inschakelen:

        Set-AzureRmDiagnosticSetting  -ResourceId /subscriptions/<subscriptionId>/resourceGroups/<resource group name>/providers/Microsoft.Network/applicationGateways/<application gateway name> -StorageAccountId /subscriptions/<subscriptionId>/resourceGroups/<resource group name>/providers/Microsoft.Storage/storageAccounts/<storage account name> -Enabled $true  

>[AZURE.INFORMATION] controlelogboeken niet nodig een aparte opslag-account hebben. Het gebruik van opslag voor toegang en registratie administratiekosten in rekening worden gebracht.

## <a name="enable-logging-with-azure-portal"></a>Schakel logboekregistratie met Azure portal

### <a name="step-1"></a>Stap 1

Ga naar de bron in de portal Azure. Klik op **Diagnostische logboeken**. Als dit de eerste keer dat diagnostische gegevens configureren ziet de bladeserver in de volgende afbeelding:

Toepassingsgateway zijn 3 zich aanmeldt beschikbaar.

- Toegang logboek
- Prestatielogboek
- Logboek firewall

Klik op **Diagnostische gegevens inschakelen** als u wilt beginnen met het verzamelen van gegevens.

![diagnostische instelling blade][1]

### <a name="step-2"></a>Stap 2

Op het blad **Diagnostische instellingen** de instellingen voor de manier waarop de diagnostische logboeken worden ingesteld. Logboek analytics wordt in dit voorbeeld wordt gebruikt voor het opslaan van de logboekbestanden. Klik op **configureren** onder **Logboek Analytics** uw werkruimte configureren. Gebeurtenis hubs en een account voor de opslag kunnen worden gebruikt om op te slaan van de diagnostische gegevens zich aanmeldt.

![Diagnostische gegevens blade][2]

### <a name="step-3"></a>Stap 3

Kies een bestaande werkruimte voor OMS of maak een nieuwe. In dit voorbeeld wordt een bestaande gebruikt.

![OMS werkruimten][3]

### <a name="step-4"></a>Stap 4

Als alles klaar is, bevestig de instellingen en klik op **Opslaan** om het opslaan van de instellingen.

![Bevestig de selectie][4]

## <a name="audit-log"></a>Controlelogboek

Dit logboek (voorheen bekend als 'operationele log') wordt gegenereerd door Azure.  De logboeken worden 90 dagen bewaard in Azure van gebeurtenislogboeken winkel. Meer informatie over deze logboeken door het lezen van het artikel [weergeven van gebeurtenissen en logboeken controleren](../monitoring-and-diagnostics/insights-debugging-with-events.md) .

## <a name="access-log"></a>Toegang logboek

Dit logboek alleen wordt gegenereerd als u deze hebt ingeschakeld op een per Application Gateway-basis zoals aangegeven in de voorgaande stappen. De gegevens worden opgeslagen in de opslag-account opgegeven wanneer u de logboekregistratie ingeschakeld. Elke toegang van toepassingsgateway vastgelegd in JSON-indeling, zoals in het volgende voorbeeld:

    {
        "resourceId": "/SUBSCRIPTIONS/<subscription id>/RESOURCEGROUPS/<resource group name>/PROVIDERS/MICROSOFT.NETWORK/APPLICATIONGATEWAYS/<application gateway name>",
        "operationName": "ApplicationGatewayAccess",
        "time": "2016-04-11T04:24:37Z",
        "category": "ApplicationGatewayAccessLog",
        "properties": {
            "instanceId":"ApplicationGatewayRole_IN_0",
            "clientIP":"37.186.113.170",
            "clientPort":"12345",
            "httpMethod":"HEAD",
            "requestUri":"/xyz/portal",
            "requestQuery":"",
            "userAgent":"-",
            "httpStatus":"200",
            "httpVersion":"HTTP/1.0",
            "receivedBytes":"27",
            "sentBytes":"202",
            "timeTaken":"359",
            "sslEnabled":"off"
        }
    }

## <a name="performance-log"></a>Prestatielogboek

Dit logboek wordt alleen gegenereerd als deze is ingeschakeld op een per Application Gateway-basis zoals aangegeven in de voorgaande stappen. De gegevens worden opgeslagen in de opslag-account opgegeven wanneer u de logboekregistratie ingeschakeld. De volgende gegevens vastgelegd:

    {
        "resourceId": "/SUBSCRIPTIONS/<subscription id>/RESOURCEGROUPS/<resource group name>/PROVIDERS/MICROSOFT.NETWORK/APPLICATIONGATEWAYS/<application gateway name>",
        "operationName": "ApplicationGatewayPerformance",
        "time": "2016-04-09T00:00:00Z",
        "category": "ApplicationGatewayPerformanceLog",
        "properties": 
        {
            "instanceId":"ApplicationGatewayRole_IN_1",
            "healthyHostCount":"4",
            "unHealthyHostCount":"0",
            "requestCount":"185",
            "latency":"0",
            "failedRequestCount":"0",
            "throughput":"119427"
        }
    }


## <a name="firewall-log"></a>Logboek firewall

Dit logboek wordt alleen gegenereerd als deze is ingeschakeld op een per toepassing gateway als bedoeld in de voorgaande stappen. Dit logboek moet ook op een toepassingsgateway web application firewall worden geconfigureerd. De gegevens worden opgeslagen in de opslag-account opgegeven wanneer u de logboekregistratie ingeschakeld. De volgende gegevens vastgelegd:

    {
        "resourceId": "/SUBSCRIPTIONS/<subscriptionId>/RESOURCEGROUPS/<resourceGroupName>/PROVIDERS/MICROSOFT.NETWORK/APPLICATIONGATEWAYS/<applicationGatewayName>",
        "operationName": "ApplicationGatewayFirewall",
        "time": "2016-09-20T00:40:04.9138513Z",
        "category": "ApplicationGatewayFirewallLog",
        "properties":     {
            "instanceId":"ApplicationGatewayRole_IN_0",
            "clientIp":"108.41.16.164",
            "clientPort":1815,
            "requestUri":"/wavsep/active/RXSS-Detection-Evaluation-POST/",
            "ruleId":"OWASP_973336",
            "message":"XSS Filter - Category 1: Script Tag Vector",
            "action":"Logged",
            "site":"Global",
            "message":"XSS Filter - Category 1: Script Tag Vector",
            "details":{"message":" Warning. Pattern match "(?i)(<script","file":"/owasp_crs/base_rules/modsecurity_crs_41_xss_attacks.conf","line":"14"}}
    }

## <a name="view-and-analyze-the-audit-log"></a>Weergeven en analyseren van het controlelogboek

U kunt weergeven en analyseren van logboekgegevens controleren met behulp van een van de volgende methoden:

- **Extra azure:** Informatie ophalen uit de controlelogboeken via Azure PowerShell, Azure Command Line Interface (CLI), de REST Azure API of de Azure preview portal.  Stapsgewijze instructies voor elke methode worden gedetailleerde in het artikel [controleverrichtingen met Resource Manager](../resource-group-audit.md) .
- **Power BI:** Als u niet al een [Power BI](https://powerbi.microsoft.com/pricing) -account hebt, kunt u deze gratis proberen. Met behulp van de [controlelogboeken Azure content pack voor Power BI](https://powerbi.microsoft.com/en-us/documentation/powerbi-content-pack-azure-audit-logs/) kunt u uw gegevens met vooraf geconfigureerde dashboards die u als gebruiken kunt analyseren-, of aanpassen.

## <a name="view-and-analyze-the-access-performance-and-firewall-log"></a>Weergeven en analyseren van het logboek toegang, prestaties en firewall

Azure [Logboek Analytics](../log-analytics/log-analytics-azure-networking-analytics.md) de teller kunt verzamelen en gebeurtenislogboek bestanden van uw account Blob-opslag en visualisaties en krachtige zoekfuncties voor het analyseren van de logboeken bevat.

U kunt ook verbinding maken met uw account voor opslag en ophalen van de JSON-logboekvermeldingen voor toegangs- en Logboeken. Nadat u de JSON-bestanden downloaden, kunt u deze converteren CSV en weergave in Excel, PowerBI of een andere data visualisatie tool.

>[AZURE.TIP] Als u bekend met Visual Studio en de basisconcepten bent van de waarden van de constanten en variabelen in C#, kunt u [logboek conversieprogramma voor hulpprogramma's](https://github.com/Azure-Samples/networking-dotnet-log-converter) beschikbaar via Github.

## <a name="metrics"></a>Statistieken

Metrics is een functie voor bepaalde Azure bronnen waarin u de prestatiemeteritems in de portal kunt weergeven. Toepassingsgateway is een metric beschikbaar op het moment van schrijven van dit artikel. Deze metriek doorvoer is en kan worden gezien in de portal. Ga naar een toepassingsgateway en klikt u op **parameters**.  Selecteer de doorvoer in de sectie **parameters beschikbaar** om de waarden weer te geven. In de volgende afbeelding ziet u een voorbeeld met de filters die kunnen worden gebruikt om de gegevens weergeven in een andere tijd bereiken.

Een overzicht van de huidige statistieken ondersteunen, gaat u naar [ondersteunde parameters met Azure Monitor](../monitoring-and-diagnostics/monitoring-supported-metrics.md)

![metrische weergeven][5]

## <a name="alert-rules"></a>Waarschuwingsregels

Waarschuwingsregels kunnen worden gestart op basis van van maatstaven voor een bron. Dit betekent voor de toepassingsgateway, een waarschuwing roept een webhook of een beheerder een e-mail als de doorvoer van de toepassingsgateway boven, onder of op een drempel voor een bepaalde periode is.

In het volgende voorbeeld helpt u bij het maken van een waarschuwingsregel die een e-mailbericht naar een beheerder, verzendt nadat een doorvoer-drempel is nagekomen.

### <a name="step-1"></a>Stap 1

Klik op de **metrische waarschuwing toevoegen** te starten. Deze blade kan ook worden bereikt vanaf de blade maatstaven.

![waarschuwingsregels blade][6]

### <a name="step-2"></a>Stap 2

Vul de naam van de voorwaarde, en secties in kennis in het blad **regel toevoegen** en klikt u op **OK** wanneer u klaar bent.

De selector **voorwaarde** kan 4 waarden, **groter dan**, **groter dan of gelijk**, **kleiner dan**of **kleiner dan of gelijk aan**.

De selector **periode** kunnen voor het picken van een periode van 5 minuten tot 6 uur.

Door **e-eigenaren, medewerkers, en lezers** te selecteren kan het e-mailbericht zijn dynamisch gebaseerd op de gebruikers die toegang tot die bron hebben. Anders kan een door komma's gescheiden lijst van gebruikers worden geleverd in het tekstvak **beheerder aanvullende email(s)** .

![blade regel toevoegen][7]

Als de drempelwaarde is nagekomen, wordt een e-mailbericht binnenkomt vergelijkbaar zijn met die in de volgende afbeelding:

![e-mailadres drempel nagekomen][8]

Een lijst met waarschuwingen wordt weergegeven als een metric is gemaakt en een overzicht van de waarschuwingsregels biedt.

![waarschuwingsregel weergeven][9]

Bezoek voor meer informatie over meldingen, [waarschuwingen ontvangen](../monitoring-and-diagnostics/insights-receive-alert-notifications.md)

Om te begrijpen meer informatie over webhooks en hoe u ze kunt gebruiken met waarschuwingen, gaat u naar [een webhook op een metrische Azure waarschuwing configureren](../monitoring-and-diagnostics/insights-webhooks-alerts.md)

## <a name="next-steps"></a>Volgende stappen

- Teller en gebeurtenislogboeken met [Analytics logboek](../log-analytics/log-analytics-azure-networking-analytics.md) visualiseren 
- [Visualiseren de controlelogboeken Azure met Power BI](http://blogs.msdn.com/b/powerbi/archive/2015/09/30/monitor-azure-audit-logs-with-power-bi.aspx) blogbericht.
- [Weergeven en analyseren van controlelogboeken Azure in Power BI en meer](https://azure.microsoft.com/blog/analyze-azure-audit-logs-in-powerbi-more/) blogbericht.

[1]: ./media/application-gateway-diagnostics/figure1.png
[2]: ./media/application-gateway-diagnostics/figure2.png
[3]: ./media/application-gateway-diagnostics/figure3.png
[4]: ./media/application-gateway-diagnostics/figure4.png
[5]: ./media/application-gateway-diagnostics/figure5.png
[6]: ./media/application-gateway-diagnostics/figure6.png
[7]: ./media/application-gateway-diagnostics/figure7.png
[8]: ./media/application-gateway-diagnostics/figure8.png
[9]: ./media/application-gateway-diagnostics/figure9.png