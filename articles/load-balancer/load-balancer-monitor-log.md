<properties
   pageTitle="Activiteiten, gebeurtenissen en items controleren voor Load Balancer | Microsoft Azure"
   description="Informatie over het alert gebeurtenissen inschakelen en gezondheid status registratie voor Azure Load Balancer probe"
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor="tysonn"
   tags="azure-resource-manager"
/>
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/24/2016"
   ms.author="sewhee" />

# <a name="log-analytics-for-azure-load-balancer-preview"></a>Logboek analytics voor Azure-taakverdeling (voorbeeld)

U kunt verschillende typen logboeken in Azure beheren en oplossen van problemen met netwerktaakverdeling. Sommige van deze logboeken zijn toegankelijk via de portal. Alle logboeken kunnen worden opgehaald uit een Azure blobopslag, en bekeken in verschillende hulpprogramma's, zoals Excel- en PowerBI. U kunt meer informatie over de verschillende soorten logboeken in de lijst hieronder.

- **Controlelogboekbestanden:** [Controlelogboeken Azure](../../articles/monitoring-and-diagnostics/insights-debugging-with-events.md) (voorheen bekend als operationele Logboeken) kunt u alle bewerkingen worden ingediend bij uw abonnement(en) Azure en hun status weergeven. Controlelogboeken worden standaard ingeschakeld en kunnen worden weergegeven in de portal Azure.
- **Waarschuwing gebeurtenislogboeken:** U kunt dit logboek bekijken welke waarschuwingen voor taakverdeling worden verhoogd. De status voor de verdeling van de belasting wordt elke vijf minuten verzameld. Dit logboek is opgesteld als een load balancer alert-gebeurtenis wordt geactiveerd.
- **Gezondheid sonde logs:** Dit logboek kunt u controleren of sonde selectievakje gezondheidsstatus, hoeveel exemplaren on line zijn in de load balancer back-end en het percentage van virtuele machines netwerkverkeer ontvangen van de taakverdeling. Dit logboek geschreven op de sonde status wijzigen.

>[AZURE.IMPORTANT] Meld u analytics op dit moment werkt alleen voor netwerktaakverdeling toegankelijk via Internet. Logboeken zijn alleen beschikbaar voor resources in het implementatiemodel bronnenbeheerder geïmplementeerd. Kunt u zich aanmeldt voor bronnen in het klassieke implementatiemodel. Zie voor meer informatie over de van implementatiemodellen [Resource Manager voor informatie over implementatie en klassieke implementatie](../../articles/resource-manager-deployment-model.md).

## <a name="enable-logging"></a>Logboekregistratie inschakelen

Logboekregistratie is standaard ingeschakeld voor elke resource Resource Manager. U moet de gebeurtenis en gezondheid sonde registreren om te starten met het verzamelen van de gegevens die beschikbaar zijn via deze logboeken inschakelen. Gebruik de volgende stappen uit om logboekregistratie in te schakelen.

Aanmelden op de [portal Azure](http://portal.azure.com). Als u niet al een taakverdeling [een load balancer maken](load-balancer-get-started-internet-arm-ps.md) voordat u verdergaat.

1. Klik op **Bladeren**in de portal.
2. Schakel **Netwerktaakverdeling**.

    ![Portal - load balancer](./media/load-balancer-monitor-log/load-balancer-browse.png)

3. Selecteer een bestaande taakverdeling >> **Alle instellingen**.
4. Aan de rechterkant van het dialoogvenster onder de naam van de taakverdeling, Ga naar **controleren**, klik op **Diagnostische gegevens**.

    ![Portal - instellingen van een load balancer](./media/load-balancer-monitor-log/load-balancer-settings.png)

5. Selecteer in het venster **diagnostiek** onder **Status** **op**.
6. Klik op **opslag-Account**.
7. Onder **LOGBOEKEN**, selecteert u een bestaande account voor de opslag of een nieuwe maken. Gebruik de schuifregelaar om te bepalen hoeveel dagen gebeurtenis dat wordt bewaard in de gebeurtenislogboeken. 8. Klik op **Opslaan**.

    ![Portal - diagnostische logboeken](./media/load-balancer-monitor-log/load-balancer-diagnostics.png)

>[AZURE.INFORMATION] controlelogboeken niet nodig een aparte opslag-account hebben. Het gebruik van opslag voor gebeurtenis- en logboekregistratie sonde administratiekosten zal oplopen.

## <a name="audit-log"></a>Controlelogboek

Het controlelogboek wordt gegenereerd door de standaard. De logboeken worden 90 dagen bewaard in Azure van gebeurtenislogboeken winkel. Meer informatie over deze logboeken door het lezen van het artikel [weergeven van gebeurtenissen en logboeken controleren](../../articles/monitoring-and-diagnostics/insights-debugging-with-events.md) .

## <a name="alert-event-log"></a>Waarschuwing gebeurtenislogboek

Dit logboek alleen wordt gegenereerd als u deze hebt ingeschakeld op een per load balancer basis. De gebeurtenissen worden vastgelegd in JSON-indeling en opgeslagen in de opslag account die u hebt opgegeven als u de logboekregistratie ingeschakeld. Hier volgt een voorbeeld van een gebeurtenis.

    {
    "time": "2016-01-26T10:37:46.6024215Z",
    "systemId": "32077926-b9c4-42fb-94c1-762e528b5b27",
    "category": "LoadBalancerAlertEvent",
    "resourceId": "/SUBSCRIPTIONS/XXXXXXXXXXXXXXXXX-XXXX-XXXX-XXXXXXXXX/RESOURCEGROUPS/RG7/PROVIDERS/MICROSOFT.NETWORK/LOADBALANCERS/WWEBLB",
    "operationName": "LoadBalancerProbeHealthStatus",
    "properties": {
        "eventName": "Resource Limits Hit",
        "eventDescription": "Ports exhausted",
        "eventProperties": {
            "public ip address": "40.117.227.32"
        }
    }

De uitvoer van JSON toont de *gebeurtenisnaam* eigenschap waarin de reden voor de verdeling van de belasting een waarschuwing gemaakt. In dit geval is de waarschuwing die wordt gegenereerd door uitputting van TCP-poort veroorzaakt per bron IP NAT grenzen (SNAT).

## <a name="health-probe-log"></a>Sonde gezondheidsgegevens

Dit logboek alleen wordt gegenereerd als u deze hebt ingeschakeld op een per per load balancer zoals hierboven. De gegevens worden opgeslagen in de opslag-account opgegeven wanneer u de logboekregistratie ingeschakeld. Een container met de naam 'inzichten logs loadbalancerprobehealthstatus' gemaakt en wordt de volgende gegevens worden geregistreerd:

    {
        "records":
        {
            "time": "2016-01-26T10:37:46.6024215Z",
            "systemId": "32077926-b9c4-42fb-94c1-762e528b5b27",
            "category": "LoadBalancerProbeHealthStatus",
            "resourceId": "/SUBSCRIPTIONS/XXXXXXXXXXXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXX/RESOURCEGROUPS/RG7/PROVIDERS/MICROSOFT.NETWORK/LOADBALANCERS/WWEBLB",
            "operationName": "LoadBalancerProbeHealthStatus",
            "properties": {
                "publicIpAddress": "40.83.190.158",
                "port": "81",
                "totalDipCount": 2,
                "dipDownCount": 1,
                "healthPercentage": 50.000000
            }
        },
        {
            "time": "2016-01-26T10:37:46.6024215Z",
            "systemId": "32077926-b9c4-42fb-94c1-762e528b5b27",
            "category": "LoadBalancerProbeHealthStatus",
            "resourceId": "/SUBSCRIPTIONS/XXXXXXXXXXXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXX/RESOURCEGROUPS/RG7/PROVIDERS/MICROSOFT.NETWORK/LOADBALANCERS/WWEBLB",
            "operationName": "LoadBalancerProbeHealthStatus",
            "properties": {
                "publicIpAddress": "40.83.190.158",
                "port": "81",
                "totalDipCount": 2,
                "dipDownCount": 0,
                "healthPercentage": 100.000000
            }
        }
    }

De JSON-uitvoer wordt weergegeven in het veld eigenschappen de basisgegevens voor de gezondheidsstatus van de sonde. De eigenschap *dipDownCount* bevat het totale aantal exemplaren op de back-end die geen netwerkverkeer als gevolg van defecte sonde antwoorden krijgen.

## <a name="view-and-analyze-the-audit-log"></a>Weergeven en analyseren van het controlelogboek

U kunt weergeven en analyseren van logboekgegevens controleren met behulp van een van de volgende methoden:

- **Extra azure:** Informatie ophalen uit de controlelogboeken via Azure PowerShell, Azure Command Line Interface (CLI), de REST Azure API of de Azure preview portal. Stapsgewijze instructies voor elke methode worden gedetailleerde in het artikel [controleverrichtingen met Resource Manager](../../articles/resource-group-audit.md) .
- **Power BI:** Als u niet een [Power BI](https://powerbi.microsoft.com/pricing) -account hebt nog, kunt u deze gratis proberen. U kunt uw gegevens analyseren met vooraf geconfigureerde dashboards met behulp van de [controlelogboeken Azure content pack voor Power BI](https://powerbi.microsoft.com/documentation/powerbi-content-pack-azure-audit-logs), of kunt u weergaven aanpassen aan uw behoeften aanpassen.

## <a name="view-and-analyze-the-health-probe-and-event-log"></a>Weergeven en analyseren van de sonde voor de gezondheid van gebeurtenislogboek

U moet verbinding maken met uw account voor de opslag en de vermeldingen voor gebeurtenis- en sonde logboeken JSON te halen. Nadat u de JSON-bestanden downloaden, kunt u deze converteren CSV en weergave in Excel, PowerBI of een andere data visualisatie tool.

>[AZURE.TIP] Als u bekend met Visual Studio en de basisconcepten bent van de waarden van de constanten en variabelen in C#, kunt u [logboek conversieprogramma voor hulpprogramma's](https://github.com/Azure-Samples/networking-dotnet-log-converter) beschikbaar via Github.

## <a name="additional-resources"></a>Aanvullende bronnen

- [Visualiseren de controlelogboeken Azure met Power BI](http://blogs.msdn.com/b/powerbi/archive/2015/09/30/monitor-azure-audit-logs-with-power-bi.aspx) blogbericht.
- [Weergeven en analyseren van controlelogboeken Azure in Power BI en meer](https://azure.microsoft.com/blog/analyze-azure-audit-logs-in-powerbi-more/) blogbericht.

## <a name="next-steps"></a>Volgende stappen

[Load balancer sondes begrijpen](load-balancer-custom-probe-overview.md)
