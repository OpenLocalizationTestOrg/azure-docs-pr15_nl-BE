<properties
   pageTitle="Activiteiten, gebeurtenissen en prestatiemeteritems controleren op NSGs | Microsoft Azure"
   description="Informatie over het inschakelen van items, gebeurtenissen en operationele logboek voor NSGs"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn"
   tags="azure-resource-manager"
/>
<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="07/14/2016"
   ms.author="jdial" />

#<a name="log-analytics-for-network-security-groups-nsgs"></a>Logboek analytics voor beveiligingsgroepen netwerk (NSGs)

U kunt verschillende typen logboeken in Azure beheren en oplossen van problemen met NSGs. Sommige van deze logboeken kunt openen via de portal en alle logboeken kunnen worden opgehaald uit een Azure blobopslag, en in andere hulpprogramma's, zoals [Analytics logboek](../log-analytics/log-analytics-azure-networking-analytics.md), Excel- en PowerBI weergegeven. U kunt meer informatie over de verschillende soorten logboeken in de lijst hieronder.

- **Controlelogboekbestanden:** [Controlelogboeken Azure](../monitoring-and-diagnostics/insights-debugging-with-events.md) (voorheen bekend als operationele Logboeken) kunt u alle bewerkingen worden ingediend bij uw abonnement(en) Azure en hun status weergeven. Controlelogboeken worden standaard ingeschakeld en kunnen worden weergegeven in de voorvertoning Azure-portal.
- **Gebeurtenislogboeken:** U kunt dit logboek bekijken welke NSG regels worden toegepast op VMs en exemplaar rollen op basis van MAC-adres. De status van deze regels wordt elke 60 seconden verzameld.
- **Logs teller:** U kunt dit logboek bekijken hoe vaak elke NSG-regel is toegepast om te weigeren of toestaan van verkeer.

>[AZURE.WARNING] Logboeken zijn alleen beschikbaar voor resources in het implementatiemodel bronnenbeheerder geïmplementeerd. Kunt u zich aanmeldt voor bronnen in het klassieke implementatiemodel. Voor een beter begrip van de twee modellen, verwijzen naar het artikel [Wat Resource Manager-implementatie en klassieke implementatie](../resource-manager-deployment-model.md) .

##<a name="enable-logging"></a>Logboekregistratie inschakelen
Het controlelogboek wordt automatisch ingeschakeld op alle tijden voor elke resource Resource Manager. U moet de gebeurtenis en prestatiemeteritems registreren om te starten met het verzamelen van de gegevens die beschikbaar zijn via deze logboeken inschakelen. Als logboekregistratie wilt inschakelen, volgt u de onderstaande stappen uit.

1.  Aanmelden op de [portal Azure](https://portal.azure.com). Als u niet al een bestaande beveiligingsgroep voor netwerk, [een NSG maken](virtual-networks-create-nsg-arm-ps.md) voordat u verdergaat.

2.  Klik op **Bladeren**in het preview portal >> **netwerk beveiligingsgroepen**.

    ![Preview portal - netwerk beveiligingsgroepen](./media/virtual-network-nsg-manage-log/portal-enable1.png)

3. Selecteer een bestaande beveiligingsgroep voor het netwerk.

    ![Preview portal - groepsinstellingen voor netwerkbeveiliging](./media/virtual-network-nsg-manage-log/portal-enable2.png)

4. In het blad **Instellingen** Klik op **Diagnostische gegevens**en klik vervolgens in het deelvenster naast **Status** **Diagnostische gegevens** op **in**
5. Klik op **Account voor opslag**in de blade **Instellingen** en selecteert u een bestaande opslag account of maak een nieuwe.  

>[AZURE.INFORMATION] controlelogboeken niet nodig een aparte opslag-account hebben. Het gebruik van opslag voor de gebeurtenis en de registratie van de regel worden de kosten in rekening gebracht.

6. Selecteer in de vervolgkeuzelijst onder **Opslag Account**of u wilt melden, gebeurtenissen of items en klik op **Opslaan**.

    ![Preview portal - diagnostische logboeken](./media/virtual-network-nsg-manage-log/portal-enable3.png)

## <a name="audit-log"></a>Controlelogboek
Dit logboek (voorheen bekend als 'operationele log') wordt gegenereerd door Azure.  De logboeken worden 90 dagen bewaard in Azure van gebeurtenislogboeken winkel. Meer informatie over deze logboeken door het lezen van het artikel [weergeven van gebeurtenissen en logboeken controleren](../monitoring-and-diagnostics/insights-debugging-with-events.md) .

## <a name="counter-log"></a>Logboek voor prestatiemeteritems
Dit logboek is alleen gegenereerd als u deze hebt ingeschakeld op basis van per NSG zoals beschreven. De gegevens worden opgeslagen in de opslag-account opgegeven wanneer u de logboekregistratie ingeschakeld. Elke regel toegepast op resources wordt geregistreerd in JSON-indeling, zoals hieronder wordt weergegeven.

    {
        "time": "2015-09-11T23:14:22.6940000Z",
        "systemId": "e22a0996-e5a7-4952-8e28-4357a6e8f0c5",
        "category": "NetworkSecurityGroupRuleCounter",
        "resourceId": "/SUBSCRIPTIONS/D763EE4A-9131-455F-8C5E-876035455EC4/RESOURCEGROUPS/INSIGHTOBONRP/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/NSGINSIGHTOBONRP",
        "operationName": "NetworkSecurityGroupCounters",
        "properties": {
            "vnetResourceGuid":"{DD0074B1-4CB3-49FA-BF10-8719DFBA3568}",
            "subnetPrefix":"10.0.0.0/24",
            "macAddress":"001517D9C43C",
            "ruleName":"DenyAllOutBound",
            "direction":"Out",
            "type":"block",
            "matchedConnections":0
            }
    }

## <a name="event-log"></a>Gebeurtenislogboek
Dit logboek is alleen gegenereerd als u deze hebt ingeschakeld op basis van per NSG zoals beschreven. De gegevens worden opgeslagen in de opslag-account opgegeven wanneer u de logboekregistratie ingeschakeld. De volgende gegevens vastgelegd:

    {
        "time": "2015-09-11T23:05:22.6860000Z",
        "systemId": "e22a0996-e5a7-4952-8e28-4357a6e8f0c5",
        "category": "NetworkSecurityGroupEvent",
        "resourceId": "/SUBSCRIPTIONS/D763EE4A-9131-455F-8C5E-876035455EC4/RESOURCEGROUPS/INSIGHTOBONRP/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/NSGINSIGHTOBONRP",
        "operationName": "NetworkSecurityGroupEvents",
        "properties": {
            "vnetResourceGuid":"{DD0074B1-4CB3-49FA-BF10-8719DFBA3568}",
            "subnetPrefix":"10.0.0.0/24",
            "macAddress":"001517D9C43C",
            "ruleName":"AllowVnetOutBound",
            "direction":"Out",
            "priority":65000,
            "type":"allow",
            "conditions":{
                "destinationPortRange":"0-65535",
                "sourcePortRange":"0-65535",
                "destinationIP":"10.0.0.0/8,172.16.0.0/12,169.254.0.0/16,192.168.0.0/16,168.63.129.16/32",
                "sourceIP":"10.0.0.0/8,172.16.0.0/12,169.254.0.0/16,192.168.0.0/16,168.63.129.16/32"
            }
        }
    }

## <a name="view-and-analyze-the-audit-log"></a>Weergeven en analyseren van het controlelogboek
U kunt weergeven en analyseren van logboekgegevens controleren met behulp van een van de volgende methoden:

- **Extra azure:** Informatie ophalen uit de controlelogboeken via Azure PowerShell, Azure Command Line Interface (CLI), de REST Azure API of de Azure preview portal.  Stapsgewijze instructies voor elke methode worden gedetailleerde in het artikel [controleverrichtingen met Resource Manager](../resource-group-audit.md) .
- **Power BI:** Als u niet al een [Power BI](https://powerbi.microsoft.com/pricing) -account hebt, kunt u deze gratis proberen. Met behulp van de [controlelogboeken Azure content pack voor Power BI](https://powerbi.microsoft.com/documentation/powerbi-content-pack-azure-audit-logs/) kunt u uw gegevens met vooraf geconfigureerde dashboards die u als gebruiken kunt analyseren-, of aanpassen.

## <a name="view-and-analyze-the-counter-and-event-log"></a>Weergeven en analyseren van de teller en gebeurtenislogboek

Azure [Logboek Analytics](../log-analytics/log-analytics-azure-networking-analytics.md) de teller kunt verzamelen en gebeurtenislogboek bestanden van uw account Blob-opslag en visualisaties en krachtige zoekfuncties voor het analyseren van de logboeken bevat.

U kunt ook verbinding maken met uw account voor de opslag en de JSON logboekvermeldingen van gebeurtenis-en teller ophalen. Nadat u de JSON-bestanden downloaden, kunt u deze converteren CSV en weergave in Excel, PowerBI of een andere data visualisatie tool.

>[AZURE.TIP] Als u bekend met Visual Studio en de basisconcepten bent van de waarden van de constanten en variabelen in C#, kunt u [logboek conversieprogramma voor hulpprogramma's](https://github.com/Azure-Samples/networking-dotnet-log-converter) beschikbaar via Github.

## <a name="next-steps"></a>Volgende stappen

- Teller en gebeurtenislogboeken met [Analytics logboek](../log-analytics/log-analytics-azure-networking-analytics.md) visualiseren
- [Visualiseren de controlelogboeken Azure met Power BI](http://blogs.msdn.com/b/powerbi/archive/2015/09/30/monitor-azure-audit-logs-with-power-bi.aspx) blogbericht.
- [Weergeven en analyseren van controlelogboeken Azure in Power BI en meer](https://azure.microsoft.com/blog/analyze-azure-audit-logs-in-powerbi-more/) blogbericht.
