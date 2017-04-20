<properties
    pageTitle="Azure overheid documentatie | Microsoft Azure"
    description="Dit zorgt voor een vergelijking van functies en hulp op het ontwikkelen van toepassingen voor de overheid Azure"
    services="Azure-Government"
    cloud="gov" 
    documentationCenter=""
    authors="scooxl"
    manager="zakramer"
    editor=""/>
<tags
    ms.service="multiple"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="azure-government"
    ms.date="10/25/2016"
    ms.author="scooxl"/>
#  <a name="azure-government-management-and-security"></a>Azure overheid beheer en beveiliging

## <a name="automation"></a>Automatisering

Automatisering is in Azure overheid in het algemeen beschikbaar.

### <a name="variations"></a>Variaties

De volgende Automation-functies zijn momenteel niet beschikbaar in Azure regering.

+ Het maken van een Service beginsel referenties voor verificatie

Zie [openbare automatisering-documentatie](../automation/automation-intro.md)voor meer informatie.


##  <a name="key-vault"></a>Sleutel kluis
Zie voor meer informatie over deze service en het gebruik ervan, de <a href="https://azure.microsoft.com/documentation/services/key-vault">openbare documentatie Azure sleutel kluis.</a>
### <a name="data-considerations"></a>Overwegingen met betrekking tot gegevens
De volgende informatie geeft aan de rand van de overheid Azure voor Azure sleutel kluis:

| Geregeld/gecontroleerde gegevens toegestaan | Geregeld/gecontroleerde gegevens niet toegestaan |
|--------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Alle gegevens versleuteld met een sleutel Azure sleutel kluis kan Regulated/gecontroleerde gegevens bevatten. | Azure sleutel kluis metagegevens mag niet exporteren gecontroleerde gegevens bevatten. Deze metagegevens omvatten alle configuratiegegevens hebt opgegeven bij het maken en onderhouden van uw sleutel kluis.  Voer geen gegevens Regulated/geregeld in de volgende velden: groep resourcenamen, sleutel kluis namen, de naam van abonnement |

Sleutel kluis is in Azure overheid in het algemeen beschikbaar. Als in het openbaar is er geen uitbreiding, sleutel kluis is alleen beschikbaar via PowerShell en CLI.
## <a name="log-analytics"></a>Logboek Analytics
Logboek Analytics is over het algemeen beschikbaar in Azure regering. 

### <a name="variations"></a>Variaties

Het volgende logboek analytische functies en oplossingen zijn momenteel niet beschikbaar in Azure regering. Deze lijst wordt bijgewerkt wanneer de status van functies / wijzigingen oplossingen.

+ Oplossingen die in het voorbeeld in openbare Azure, met inbegrip van:
  - Network Monitoring solution
  - Controle van toepassingen afhankelijkheid
  - Office 365-oplossing
  - Windows 10 Upgrade Analytics oplossing
  - Inzichten van toepassing
  - Azure toegang Analytics oplossing
  - Analytics Azure automatisering
  - Sleutel kluis Analytics
+ Oplossingen en functies waarvoor u updates voor software voor gebouwen, met inbegrip van
  - Integratie met System Center Operations Manager 2016 (eerdere versies van Operations Manager worden ondersteund)
  - Computers, groepen van System Center Configuration Manager
  - Surface Hub-oplossing
+ Functies die in het voorbeeld in openbare Azure, met inbegrip van
  - Export van gegevens naar PowerBI
+ Azure maatstaven en Azure diagnostische gegevens
+ OMS mobiele toepassingen

De URL's voor logboek Analytics zijn in Azure regering anders:

| Azure publiek | Azure overheid | Notities |
|--------------|------------------|-------|
| MMS.Microsoft.com | OMS.Microsoft.us | Logboek Analytics portal |
| *workspaceId*. ods.opinsights.azure.com | *workspaceId*. ods.opinsights.azure.us | [Gegevensverzamelaarset API](../log-analytics/log-analytics-data-collector-api.md) 
| \*. ods.opinsights.azure.com | \*. ods.opinsights.azure.us | Agent-communicatie - [instellingen van de firewall configureren](../log-analytics/log-analytics-proxy-firewall.md) |
| \*. oms.opinsights.azure.com | \*. oms.opinsights.azure.us | Agent-communicatie - [instellingen van de firewall configureren](../log-analytics/log-analytics-proxy-firewall.md) |
| \*. blob.core.windows.net | \*. blob.core.usgovcloudapi.net | Agent-communicatie - [instellingen van de firewall configureren](../log-analytics/log-analytics-proxy-firewall.md) |


De volgende logboek Analytics-functies hebben verschillende gedrag in Azure overheid:

+ De Windows-agent moet worden gedownload vanaf de [portal logboek Analytics](https://oms.microsoft.us) voor Azure regering.
+ Verbinding van uw management System Center Operations Manager server logboek Analytics, moet u downloaden en importeren van bijgewerkte beheerpakketten.
  1. Download en sla de [beheerpakketten bijgewerkt](http://go.microsoft.com/fwlink/?LinkId=828749)
  2. Pak het bestand dat u hebt gedownload
  3. Importeer de beheerpakketten in Operations Manager. Zie het onderwerp voor [het importeren van een Operations Manager Management Pack](http://technet.microsoft.com/library/hh212691.aspx) op de website van Microsoft TechNet voor meer informatie over het importeren van een management pack vanaf een schijf.
  4. Logboek Analytics verbinding van Operations Manager, volg de stappen in [Operations Manager verbinding maken met Analytics logboek](../log-analytics/log-analytics-om-agents.md) 



### <a name="frequently-asked-questions"></a>Veelgestelde vragen

+ Kan ik gegevens migreren van logboek Analytics in openbare Azure-Azure overheid?
  - Nr. Het is niet mogelijk gegevens of uw werkruimte verplaatsen van openbare Azure-Azure overheid.
+ Kan ik overschakelen tussen openbare Azure en Azure regering werkruimten van het portal voor OMS logboek Analytics?
  - Nr. De portals voor openbare Azure en Azure regering gescheiden zijn en geen informatie delen. 

Zie [openbare logboek Analytics-documentatie](../log-analytics/log-analytics-overview.md)voor meer informatie.

## <a name="next-steps"></a>Volgende stappen

Voor aanvullende informatie en updates abonneren op de <a href="https://blogs.msdn.microsoft.com/azuregov/">Microsoft Azure overheid Blog.</a>
 
