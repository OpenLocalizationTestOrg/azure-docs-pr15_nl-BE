<properties
    pageTitle="Overzicht van parameters in Microsoft Azure | Microsoft Azure"
    description="Overzicht van parameters en hun toepassingen in Microsoft Azure"
    authors="kamathashwin"
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
    ms.date="09/26/2016"
    ms.author="ashwink"/>

# <a name="overview-of-metrics-in-microsoft-azure"></a>Overzicht van parameters in Microsoft Azure 

Dit artikel wordt beschreven wat zijn in Microsoft Azure, hun voordelen en hoe aan de slag met hen.  

## <a name="what-are-metrics"></a>Wat zijn de cijfers?

Azure Monitor kunt u telemetrie te verkrijgen van inzicht in de prestaties en de gezondheid van uw werkbelasting op Azure verbruiken. De belangrijkste Azure telemetrie data zijn cijfers over de (ook wel items genoemd) dat door de meeste Azure bronnen. Azure Monitor biedt verschillende manieren voor het configureren en gebruiken deze gegevens voor het toezicht en het oplossen van problemen.


## <a name="what-can-you-do-with-metrics"></a>Wat kunt u doen met statistieken

Parameters zijn een waardevolle bron van Telemetrie en kunt u het volgende doen:

- **Bijhouden van de prestaties** van uw bron (zoals een VM, Website of App logica) door de parameters in een portal grafiek uitzetten en vastmaken die grafiek aan een dashboard.
- **Ontvang een bericht over een probleem** invloed op de prestaties van de resource als een metric een bepaalde drempel overschrijdt.
- **Configureren automatische acties**, zoals autoscaling, een resource of een runbook wordt geactiveerd wanneer een metric een bepaalde drempel overschrijdt.
- **Geavanceerde analytics uitvoeren** of rapporteren van trends prestaties of het gebruik van de resource (s).
- **Archief** van de prestaties of de gezondheid van de geschiedenis van uw bron **voor het controleren van** conformiteit/doeleinden.

##  <a name="metric-characteristics"></a>Metrische kenmerken
Parameters hebben de volgende kenmerken:

- Alle metrics hebben **frequentie van 1 minuut**. U ontvangt een metrische waarde elke minuut van de bron, zodat u in de buurt van real-time inzicht in de staat en de gezondheid van de bron.
- Statistieken zijn **beschikbaar out-of-the-box-zonder opt-in** of het instellen van aanvullende diagnostische gegevens.
- Toegang tot **30 dagen** voor elke maateenheid. U kunt snel de recente en maandelijkse trends in de prestaties of de gezondheid van de bron bekijken.

U kunt:

- Eenvoudig ontdekken, toegang en **alle statistieken bekijken** via de Azure portal wanneer u een resource selecteren en deze in een grafiek uitzetten. 
- Een metrische **waarschuwingsregel die stuurt een melding of geautomatiseerde actie wordt** configureren wanneer de metric kruist de drempel die u hebt ingesteld. Automatisch schalen is een speciale automatische actie waarmee u de schaal van de resource om te voldoen aan de inkomende aanvragen of de belasting op uw website of resources te berekenen. U kunt een regel van de instelling automatisch schalen om out in te schalen op basis van een metric is een drempel overschrijden.
- **Archief** metrics voor meer of ze voor het melden van off line gebruiken. De maatstaven voor het blob-opslag wanneer u diagnostische instellingen voor de resource configureren kunt u terecht.
- Een Hub gebeurtenis, zodat u deze vervolgens doorsturen naar Azure Stream Analytics of aangepaste toepassingen voor analyse in de buurt van real-time **Stream** maatstelsel. U kunt doen met de diagnostische instellingen.
- **Route** alle maatstelsel OMS logboek Analytics analytics instant search en aangepaste waarschuwingen op metrische gegevens van uw resources te ontgrendelen.
- **Verbruiken** de metrics via nieuwe Azure Monitor REST API's.
- **Query** -parameters met de PowerShell-Cmdlets of Cross-Platform REST API.

 ![Routering van statistieken in Azure Monitor](./media/monitoring-overview-metrics/MetricsOverview0.png)

## <a name="access-metrics-via-portal"></a>Statistieken van de toegang via de portal
Hier is een snel overzicht van het maken van een grafiek metrische via Azure-portal

### <a name="view-metrics-after-creating-a-resource"></a>Cijfers na het maken van een bron weergeven
1. Open Azure portal
2. Maak een nieuwe App-Service - website.
3. Nadat u een website hebt gemaakt, gaat u naar het blad Overzicht van de website.
4. U kunt nieuwe maatstaven weergeven als een tegel 'Monitoring'. U kunt de tegel bewerken en kiest u Metrisch voor meer

 ![Maatstaven voor een bron in Azure Monitor](./media/monitoring-overview-metrics/MetricsOverview1.png)    

### <a name="access-all-metrics-in-a-single-place"></a>Toegang tot alle gegevens op één plaats
1. Open de Azure portal 
2. Ga naar het nieuwe tabblad Monitor en selecteer de optie maatstelsel onder het 
3. U kunt uw abonnement, resourcegroep en de naam van de resource uit de vervolgkeuzelijst selecteren. 
4. U kunt nu de lijst met beschikbare parameters weergeven. 
5. Selecteer de metric die u van belang zijn en het uitzetten. 
6. U kunt het vastmaken aan het dashboard door op de PIN-code in de rechterbovenhoek te klikken.

 ![Toegang tot alle gegevens op één plaats in Azure Monitor](./media/monitoring-overview-metrics/MetricsOverview2.png) 


>[AZURE.NOTE] Toegankelijk op hostniveau maatstaven van VMs (Azure resourcemanager gebaseerd) en VM schaal ingesteld zonder eventuele aanvullende diagnostische instellingen. Deze nieuwe gegevens van de host-niveau zijn beschikbaar voor Windows en Linux exemplaren. Deze gegevens mogen niet worden verward met de Gast OS niveau maatstelsel dat u toegang tot hebt wanneer u Azure diagnostische gegevens op de VMs- of VMSS. Voor meer informatie over het configureren van Azure diagnostische gegevens, Zie [Wat is Microsoft Azure diagnostische gegevens](../azure-diagnostics.md).

## <a name="access-metrics-via-rest-api"></a>Maatstaven voor toegang via REST API
Azure Metrics kan worden benaderd via Azure Monitor API's. Er zijn twee API's die u helpen bij het opsporen en metrics openen. Gebruik de: 

- [Azure Monitor Metric definities REST API](https://msdn.microsoft.com/library/mt743621.aspx) voor toegang tot de lijst met parameters die beschikbaar zijn voor een service.
- [Azure Monitor Metrics REST API](https://msdn.microsoft.com/library/mt743622.aspx) voor toegang tot de gegevens van de werkelijke cijfers

>[AZURE.NOTE] Dit artikel heeft betrekking op de metrische gegevens via de [nieuwe API voor metrieken](https://msdn.microsoft.com/library/dn931930.aspx) voor Azure resources. De API-versie voor de nieuwe definities van metrische API is 03-01-2016 en de versie voor metrische API 09-01-2016. De oude metrische definities en parameters is toegankelijk via de api-versie 04-01-2014.

De Azure Monitor REST API's, Zie voor een meer gedetailleerde scenario met [Azure Monitor REST API scenario](monitoring-rest-api-walkthrough.md).

## <a name="export-options-for-metrics"></a>Exportopties voor statistieken
U gaat u naar de diagnostische logboeken blade onder het tabblad Monitor en de exportopties voor metrieken weergeven. Kunt u maatstaven (en diagnostische logboeken) moet worden doorgestuurd naar Blob-opslag gebeurtenis Hubs of OMS logboek Analytics voor gebruiksvoorbeelden eerder in dit artikel worden vermeld. 

 ![Exportopties voor metrieken in Azure Monitor](./media/monitoring-overview-metrics/MetricsOverview3.png)   

U kunt dit via de bronnenbeheerder sjablonen, [PowerShell](insights-powershell-samples.md), [CLI](insights-cli-samples.md) of [REST API's](https://msdn.microsoft.com/library/dn931943.aspx). 

## <a name="take-action-on-metrics"></a>Actie ondernemen Metrics
U kunt meldingen ontvangen of metrische gegevens van geautomatiseerde acties ondernemen. U kunt regels of instellingen automatisch schalen om dit te doen.

### <a name="alert-rules"></a>Waarschuwingsregels
U kunt regels configureren op metrics. Deze regels kunnen controleren als metric een bepaalde drempel heeft overschreden en ontvangt u een melding via e-mail of een webhook die vervolgens kan worden gebruikt voor het uitvoeren van een aangepast script gestart. U kunt ook de webhook 3e product integraties configureren.

 ![Maatstaven en waarschuwingsregels in Azure Monitor](./media/monitoring-overview-metrics/MetricsOverview4.png)

### <a name="autoscale"></a>Automatisch schalen
Sommige bronnen Azure ondersteunen meerdere exemplaren te schalen of voor het verwerken van uw werkbelasting. Automatisch schalen van toepassing is op de App (Web apps), Virtual Machine schaal Sets (VMSS) en klassieke Cloud diensten. U kunt de regels automatisch schalen of schaalt wanneer een bepaalde metriek beïnvloeden uw werkbelasting snijdt een drempel die u opgeeft. Zie [overzicht van autoscaling](monitoring-overview-autoscale.md)voor meer informatie.

 ![Maatstaven en automatisch schalen in Azure Monitor](./media/monitoring-overview-metrics/MetricsOverview5.png)

## <a name="supported-services-and-metrics"></a>Ondersteunde Services en statistieken
Azure Monitor is een infrastructuur met nieuwe maatstaven. Het biedt ondersteuning voor de volgende Azure services in de Azure portal en de nieuwe versie van de API Azure Monitor:

- VMs (Azure resourcemanager gebaseerd)
- VM schaal Sets
- Batch
- Gebeurtenis Hub naamruimte 
- Service Bus naamruimte (alleen premium SKU)
- SQL (versie 12)
- Elastische SQL-toepassingen
- Websites
- Webserver-Farms
- Logica Apps
- IoT Hubs
- Cache bestand Vgx.
- Netwerken: Toepassingsgateways
- Zoeken

U vindt een een gedetailleerde lijst met ondersteunde services en hun statistieken op [Azure Monitor metrics - ondersteunde parameters per resourcetype](monitoring-supported-metrics.md). 


## <a name="next-steps"></a>Volgende stappen

Raadpleeg de koppelingen in dit artikel. Bovendien informatie over:  

- over [gemeenschappelijke maatstaven voor autoscaling](insights-autoscale-common-metrics.md)
- [waarschuwingsregels](insights-alerts-portal.md) maken




