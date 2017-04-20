<properties
    pageTitle="Azure overheid documentatie | Microsoft Azure"
    description="Dit biedt een vergelijking van de functies en de richtlijnen over het ontwikkelen van toepassingen voor de overheid Azure."
    services="Azure-Government"
    cloud="gov"
    documentationCenter=""
    authors="ryansoc"
    manager="zakramer"
    editor=""/>

<tags
    ms.service="multiple"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="azure-government"
    ms.date="10/25/2016"
    ms.author="ryansoc"/>


#  <a name="azure-government-monitoring-and-management"></a>Azure overheid monitoring en beheer

In dit artikel worden de monitoring en management services variaties en overwegingen voor de overheid Azure-omgeving.

## <a name="automation"></a>Automatisering

Automatisering is in Azure overheid in het algemeen beschikbaar.

### <a name="variations"></a>Variaties

De volgende Automation-functies zijn momenteel niet beschikbaar in Azure regering.

+ Het maken van een Service beginsel referenties voor verificatie

Zie [openbare automatisering-documentatie](../automation/automation-intro.md)voor meer informatie.

## <a name="log-analytics"></a>Logboek Analytics

Logboek Analytics is over het algemeen beschikbaar in Azure regering.

### <a name="variations"></a>Variaties

Het volgende logboek analytische functies en oplossingen zijn momenteel niet beschikbaar in Azure regering.

+ Oplossingen die in het voorbeeld in Microsoft Azure, met inbegrip van:
  - Network Monitoring solution
  - Afhankelijkheid Monitoring oplossing
  - Office 365-oplossing
  - Windows 10 Upgrade Analytics oplossing
  - Inzichten oplossing
  - Azure toegang Analytics oplossing
  - Azure automatisering Analytics oplossing
  - Sleutel kluis Analytics oplossing
+ Oplossingen en functies waarvoor u updates voor software voor gebouwen, met inbegrip van:
  - Integratie met System Center Operations Manager 2016 (eerdere versies van Operations Manager worden ondersteund)
  - Computers, groepen van System Center Configuration Manager
  - Surface Hub-oplossing
+ Functies die in het voorbeeld in openbare Azure, met inbegrip van:
  - Export van gegevens naar Power BI
+ Azure maatstaven en Azure diagnostische gegevens
+ Operations Management Suite mobiele toepassingen

De URL's voor logboek Analytics zijn in Azure regering anders:

| Azure publiek | Azure overheid | Notities |
|--------------|------------------|-------|
| MMS.Microsoft.com | OMS.Microsoft.us | Logboek Analytics portal |
| *workspaceId*. ods.opinsights.azure.com | *workspaceId*. ods.opinsights.azure.us | [Gegevensverzamelaarset API](../log-analytics/log-analytics-data-collector-api.md) 
| \*. ods.opinsights.azure.com | \*. ods.opinsights.azure.us | Agent-communicatie - [instellingen van de firewall configureren](../log-analytics/log-analytics-proxy-firewall.md) |
| \*. oms.opinsights.azure.com | \*. oms.opinsights.azure.us | Agent-communicatie - [instellingen van de firewall configureren](../log-analytics/log-analytics-proxy-firewall.md) |
| \*. blob.core.windows.net | \*. blob.core.usgovcloudapi.net | Agent-communicatie - [instellingen van de firewall configureren](../log-analytics/log-analytics-proxy-firewall.md) |


De volgende logboek Analytics functies zich anders gedragen in Azure overheid:

+ De Windows-Agent moet worden gedownload vanaf de [portal logboek Analytics](https://oms.microsoft.us) voor Azure regering.
+ Verbinding van uw management System Center Operations Manager server logboek Analytics, moet u downloaden en importeren van bijgewerkte beheerpakketten.
  1. Download en sla de [beheerpakketten bijgewerkt](http://go.microsoft.com/fwlink/?LinkId=828749).
  2. Pak het bestand dat u hebt gedownload.
  3. Importeer de beheerpakketten in Operations Manager. Zie [informatie over het importeren van een Operations Manager Management Pack](http://technet.microsoft.com/library/hh212691.aspx) op de website van Microsoft TechNet voor meer informatie over het importeren van een management pack vanaf een schijf.
  4. Volg de stappen in [Operations Manager verbinding maken met Analytics logboek](../log-analytics/log-analytics-om-agents.md)logboek Analytics Operations Manager verbinding wordt.


## <a name="frequently-asked-questions"></a>Veelgestelde vragen

+ Kan ik gegevens migreren van Analytics in Microsoft Azure logboek naar Azure overheid?
  - Nr. Het is niet mogelijk gegevens of de werkruimte van Microsoft Azure naar Azure overheid verplaatsen.
+ Kan ik overschakelen tussen Microsoft Azure en Azure regering werkruimten van het Operations Management Suite logboek Analytics portal?
  - Nr. De portals voor Microsoft Azure en Azure regering gescheiden zijn en geen informatie delen.

Zie [openbare logboek Analytics-documentatie](../log-analytics/log-analytics-overview.md)voor meer informatie.

## <a name="next-steps"></a>Volgende stappen

Voor aanvullende informatie en updates abonneren op de <a href="https://blogs.msdn.microsoft.com/azuregov/">Microsoft Azure overheid Blog.</a>
