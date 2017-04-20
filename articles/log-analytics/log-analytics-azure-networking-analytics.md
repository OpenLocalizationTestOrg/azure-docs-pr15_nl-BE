<properties
    pageTitle="Azure toegang Analytics oplossing in logboek Analytics | Microsoft Azure"
    description="U kunt de Azure toegang Analytics oplossing in logboek Analytics Azure toepassingsgateway logboeken en Azure netwerk groep beveiligingslogboeken bekijken."
    services="log-analytics"
    documentationCenter=""
    authors="richrundmsft"
    manager="jochan"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/05/2016"
    ms.author="richrund"/>

# <a name="azure-networking-analytics-preview-solution-in-log-analytics"></a>Azure toegang Analytics (voorbeeld) oplossing in logboek Analytics

>[AZURE.NOTE] Dit is een [voorbeeld van oplossing](log-analytics-add-solutions.md#log-analytics-preview-solutions-and-features).

U kunt de Azure toegang Analytics oplossing in logboek Analytics Azure toepassingsgateway logboeken en Azure netwerk groep beveiligingslogboeken bekijken.

U kunt logboekregistratie voor Azure toepassingsgateway logboeken en beveiligingsgroepen Azure netwerk inschakelen. Deze logboeken worden geschreven naar een Blob-opslag waar ze kunnen vervolgens worden geïndexeerd door logboek Analytics voor zoek- en analyse.

De volgende logboekbestanden worden voor Toepassingsgateways ondersteund:

+ ApplicationGatewayAccessLog
+ ApplicationGatewayPerformanceLog

De volgende logboekbestanden worden voor netwerk-beveiligingsgroepen ondersteund:

+ NetworkSecurityGroupEvent
+ NetworkSecurityGroupRuleCounter

## <a name="install-and-configure-the-solution"></a>Installeren en configureren van de oplossing

Gebruik de volgende instructies te installeren en configureren van de oplossing Azure toegang Analytics:

1.  Diagnostische logboekregistratie inschakelen voor de resources die u wilt controleren:
  + [Application Gateway](../application-gateway/application-gateway-diagnostics.md)
  + [Netwerk-beveiligingsgroep](../virtual-network/virtual-network-nsg-manage-log.md)
2.  Logboek Analytics om te lezen van de logboeken van Blob-opslag met behulp van het proces beschreven in [JSON bestanden in de blobopslag](../log-analytics/log-analytics-azure-storage-json.md)configureren.
3.  De Azure toegang Analytics oplossing inschakelen met behulp van het proces beschreven in [oplossingen logboek Analytics toevoegen uit de galerie met oplossingen](log-analytics-add-solutions.md).  

Als u de registratie van diagnostische gegevens voor een bepaald brontype niet inschakelt, wordt de blades dashboard voor die resource niet leeg zijn.

## <a name="review-azure-networking-analytics-data-collection-details"></a>Azure toegang Analytics data collectie details bekijken

De oplossing Azure toegang Analytics verzamelt diagnostische logboeken van Azure Blob-opslag voor Azure Toepassingsgateways en netwerk-beveiligingsgroepen.
Geen agent is vereist voor het verzamelen van gegevens.

De volgende tabel bevat methoden van de collectie gegevens en andere informatie over de manier waarop gegevens worden verzameld voor Azure toegang Analytics.

| Platform | Directe agent | Systems Center Operations Manager (SCOM)-agent | Azure opslag | SCOM nodig? | SCOM agent gegevens verzonden via management groep | Frequentie van verzameling |
|---|---|---|---|---|---|---|
|Azure|![Nee](./media/log-analytics-azure-networking/oms-bullet-red.png)|![Nee](./media/log-analytics-azure-networking/oms-bullet-red.png)|![Ja](./media/log-analytics-azure-networking/oms-bullet-green.png)|            ![Nee](./media/log-analytics-azure-networking/oms-bullet-red.png)|![Nee](./media/log-analytics-azure-networking/oms-bullet-red.png)| 10 minuten|

## <a name="use-azure-networking-analytics"></a>Azure netwerken Analytics gebruiken

Nadat u de oplossing, ziet u het overzicht van de client en server-fouten voor de gecontroleerde Toepassingsgateways met behulp van de **Azure toegang Analytics** naast elkaar op de pagina **Overzicht** in logboek Analytics.

![afbeelding van Azure toegang Analytics tegel](./media/log-analytics-azure-networking/log-analytics-azurenetworking-tile.png)

Nadat u op de tegel **Overzicht** , kunt u de samenvattingen van de logboeken weergeven en vervolgens analyseren voor de volgende categorieën:

+ Application Gateway toegang tot logboeken
  - Client en server fouten voor Application Gateway-Logboeken
  - Aanvragen per uur voor de Gateway voor elke toepassing
  - Mislukte aanvragen per uur voor de Gateway voor elke toepassing
  - Fouten door de gebruikersagent voor Toepassingsgateways
+ Gateway toepassingsprestaties
  - Gezondheid voor toepassingsgateway host
  - Maximum- en 95e percentiel voor toepassingsgateway mislukte aanvragen
+ Beveiligingsgroep netwerk stromen geblokkeerd
  - Groep beveiligingsregels voor netwerken met geblokkeerde stromen
  - MAC-adressen met geblokkeerde stromen
+ Beveiligingsgroep netwerk stromen toegestaan
  - Groep beveiligingsregels voor netwerken met toegestane stromen
  - MAC-adressen met toegestane stromen


![afbeelding van Azure toegang Analytics dashboard](./media/log-analytics-azure-networking/log-analytics-azurenetworking01.png)

![afbeelding van Azure toegang Analytics dashboard](./media/log-analytics-azure-networking/log-analytics-azurenetworking02.png)

![afbeelding van Azure toegang Analytics dashboard](./media/log-analytics-azure-networking/log-analytics-azurenetworking03.png)

![afbeelding van Azure toegang Analytics dashboard](./media/log-analytics-azure-networking/log-analytics-azurenetworking04.png)

### <a name="to-view-details-for-any-log-summary"></a>Om voor een overzicht te bekijken

1. Klik op de pagina **Overzicht** op de tegel **Azure toegang Analytics** .
2. Neem de samenvattingsinformatie in een van de bladen op het dashboard **Azure toegang Analytics** , en klik op een om gedetailleerde informatie weergeven over het op de pagina aanmelden.

    U kunt de resultaten door de tijd, de gedetailleerde resultaten en de zoekgeschiedenis logboek bekijken op een logboek zoekpagina's. U kunt ook filteren op facetten om de resultaten te beperken.

## <a name="next-steps"></a>Volgende stappen

- [Zoekopdrachten in Analytics logboek logboek](log-analytics-log-searches.md) gebruiken om gedetailleerde Azure toegang Analytics-gegevens weer te geven.
