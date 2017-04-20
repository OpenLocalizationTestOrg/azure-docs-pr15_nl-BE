<properties
   pageTitle="Resource Manager ondersteund services | Microsoft Azure"
   description="Beschrijving van de resource-providers die Resource Manager, hun schema's en beschikbare versies van de API en de regio's waarop de middelen ondersteunen."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/25/2016"
   ms.author="magoedte;tomfitz"/>

# <a name="resource-manager-providers-regions-api-versions-and-schemas"></a>Resource Manager providers, regio's, API-versies en schema 's

Azure Resource Manager biedt een nieuwe manier om te implementeren en beheren van de services van uw toepassingen. Ondersteuning voor de meeste, maar niet alle services Resource Manager en sommige services ondersteunt bronnenbeheer slechts gedeeltelijk. Dit onderwerp bevat een lijst met ondersteunde resource providers voor Azure Resource Manager.

Wanneer u uw bronnen implementeert, moet u ook weten welke regio's ondersteunen deze resources en welke API-versies zijn beschikbaar voor de resources. De sectie [ondersteunde regio's](#supported-regions) wordt beschreven hoe u wilt weten welke regio's werken voor uw abonnement en resources. [API ondersteunde versies](#supported-api-versions) van de sectie ziet u hoe u kunt bepalen welke u kunt API-versies.

Welke services worden ondersteund in de Azure portal en portal-klassiek, Zie [grafiek voor beschikbaarheid van Azure portal](https://azure.microsoft.com/features/azure-portal/availability/). Welke services bewegende Ondersteuningsbronnen Zie [verplaatsen van bronnen aan de nieuwe resourcegroep of abonnement](resource-group-move-resources.md).

De volgende tabellen bevatten welke Microsoft services ondersteuning-implementatie en beheer via Resource Manager en welke niet. De koppeling in de kolom **Sjablonen Quickstart** stuurt een query naar de opslagplaats Azure Quickstart sjablonen voor de opgegeven resource provider. QuickStart-sjablonen worden toegevoegd en wordt regelmatig bijgewerkt. De aanwezigheid van een koppeling voor een bepaalde service is niet noodzakelijkerwijs dat de query retourneert sjablonen uit de opslagplaats. Er zijn ook veel providers van derden resource die Resource Manager te ondersteunen. U informatie over het weergeven van de resource-providers in de sectie [bron providers en typen](#resource-providers-and-types) .


## <a name="compute"></a>Berekenen

| Service | Bronbeheer is ingeschakeld | REST-API | Schema | QuickStart-sjablonen |
| ------- | ------------------------ |-------- | ------ | ------ |
| Batch   | Ja     | [REST partij](https://msdn.microsoft.com/library/azure/dn820158.aspx) | [12-01-2015](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-12-01/Microsoft.Batch.json)       |  |
|Container | Ja | [Container Service REST](https://msdn.microsoft.com/library/azure/mt711470.aspx) | [2016-03-30](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-03-30/Microsoft.ContainerService.json) | [Microsoft.ContainerService](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.ContainerService%22&type=Code) |
| Dynamics Lifecycle Services | Ja  |      |        |  |
| Schaal instellen | Ja | [Schaal instellen REST](https://msdn.microsoft.com/library/azure/mt705635.aspx) | [08-01-2015](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-08-01/Microsoft.Compute.json) | [virtualMachineScaleSets](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=virtualMachineScaleSets&type=Code) | 
| Service-Fabric | Ja  | [Service Fabric Rest](https://msdn.microsoft.com/library/azure/dn707692.aspx)  |      | [Microsoft.ServiceFabric](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.ServiceFabric%22&type=Code) |
| Virtuele Machines | Ja | [VM-OVERIGE](https://msdn.microsoft.com/library/azure/mt163647.aspx) | [08-01-2015](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-08-01/Microsoft.Compute.json) | [virtualMachines](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Compute%2Fvirtualmachines%22&type=Code) |
| Virtuele Machines (klassiek) | Beperkt | - | - | - |
| Externe App | Nee      | -  | - | - |
| Cloud Services (klassiek) | Beperkt (Zie hieronder) | - | - | - |

Virtuele Machines (klassiek) wordt verwezen naar bronnen die zijn geïmplementeerd door het implementatiemodel Klassiek in plaats van via het implementatiemodel Resource Manager. In het algemeen bewerkingen Resource Manager bieden geen ondersteuning voor deze bronnen, maar er zijn bepaalde bewerkingen die zijn ingeschakeld. Zie voor meer informatie over deze modellen deployment [Resource Manager voor informatie over implementatie en klassieke implementatie](resource-manager-deployment-model.md). 

Cloud Services (klassiek) kunnen worden gebruikt met andere klassieke bronnen; klassieke bronnen doen niet profiteren van alle functies van de Resource Manager en zijn echter niet een goede optie voor toekomstige oplossingen. In plaats daarvan een goed idee om uw infrastructuur met toepassingen voor het gebruik van middelen van de Microsoft.Compute, Microsoft.Storage en Microsoft.Network-naamruimten.


## <a name="networking"></a>Netwerken

| Service | Bronbeheer is ingeschakeld | REST-API | Schema | QuickStart-sjablonen |
| ------- | -------  | -------- | ------ | ------ |
| Application Gateway | Ja | [Toepassingsgateway REST](https://msdn.microsoft.com/library/azure/mt684939.aspx)   |        | [applicationGateways](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Network%2FapplicationGateways%22&type=Code) |
| DNS     | Ja | [DNS-OVERIGE](https://msdn.microsoft.com/library/azure/mt163862.aspx)         | [04-01-2016](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-04-01/Microsoft.Network.json) | [dnsZones](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Network%2FdnsZones%22&type=Code) |
| ExpressRoute | Ja  | [Overige ExpressRoute](https://msdn.microsoft.com/library/azure/mt586720.aspx)  |       | [expressRouteCircuits](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Network%2FexpressRouteCircuits%22&type=Code) |
| Taakverdeling | Ja  | [De belasting voor documentconversies REST](https://msdn.microsoft.com/library/azure/mt163651.aspx) | [08-01-2015](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-08-01/Microsoft.Network.json) | [loadBalancers](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Network%2Floadbalancers%22&type=Code) |
| Beheer van netwerkverkeer | Ja | [Overige verkeer Manager](https://msdn.microsoft.com/library/azure/mt163667.aspx) | [11-01-2015](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-11-01/Microsoft.Network.json)  | [trafficmanagerprofiles](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Network%2Ftrafficmanagerprofiles%22&type=Code) |
| Virtuele netwerken | Ja| [Virtueel netwerk REST](https://msdn.microsoft.com/en-us/library/azure/mt163650.aspx) | [08-01-2015](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-08-01/Microsoft.Network.json) | [virtualNetworks](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Network%2FvirtualNetworks%22&type=Code) |
| VPN-Gateway | Ja | [Netwerkgateway REST](https://msdn.microsoft.com/library/azure/mt163859.aspx) |  | [virtualNetworkGateways](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Network%2FvirtualNetworkGateways%22&type=Code) <br /> [localNetworkGateways](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Network%2FlocalNetworkGateways%22&type=Code) <br />[verbindingen](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Network%2Fconnections%22&type=Code) |


## <a name="storage"></a>Opslag

| Service | Bronbeheer is ingeschakeld | REST-API | Schema | QuickStart-sjablonen |
| ------- | ------- | -------- | ------ | ------- | ------ |
| Opslag | Ja  | [Overige opslag](https://msdn.microsoft.com/library/azure/mt163683.aspx) | [Opslag account](resource-manager-template-storage.md) | [Microsoft.Storage](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Storage%22&type=Code) |
| StorSimple | Ja  |         |        |  |

## <a name="databases"></a>Databases

| Service | Bronbeheer is ingeschakeld | REST-API | Schema | QuickStart-sjablonen |
| ------- | ------- | -------- | ------ | ------- | ------ |
| DocumentDB | Ja  | [Overige DocumentDB](https://msdn.microsoft.com/library/azure/dn781481.aspx) | [2015-04-08](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-04-08/Microsoft.DocumentDB.json)  | [Microsoft.DocumentDB](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.DocumentDb%22&type=Code) |
| Cache bestand Vgx. | Ja |   | [04-01-2016](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-04-01/Microsoft.Cache.json) | [Microsoft.Cache](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Cache%22&type=Code) |
| SQL-Database | Ja | [REST van de SQL-Database](https://msdn.microsoft.com/library/azure/mt163571.aspx) | [2014-04-01-voorbeeld](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2014-04-01-preview/Microsoft.Sql.json) | [Microsoft.Sql](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Sql%22&type=Code) |
| SQL datawarehouse | Ja |   |      |


## <a name="web--mobile"></a>Web & Mobile

| Service | Bronbeheer is ingeschakeld | REST-API | Schema | QuickStart-sjablonen |
| ------- | ------- | -------- | ------ | ------ |
| API Apps | Ja |   | [08-01-2015](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-08-01/Microsoft.Web.json) | [API Apps](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22kind%22%3A+%22apiApp%22&type=Code) |
| API-beheer | Ja  | [Management API REST](https://msdn.microsoft.com/library/azure/dn776326.aspx) | [2016-07-07](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-07-07/Microsoft.ApiManagement.json)       | [Microsoft.ApiManagement](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.ApiManagement%22&type=Code) | 
| Inhoud van de toezichthouder | Ja |   |   |   |
| Functie App | Ja |   |   | [functionApp](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22functionApp%22&type=Code) |
| Logica Apps | Ja   | [Workflow Service REST API](https://msdn.microsoft.com/library/azure/mt643787.aspx) | [06-01-2016](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-06-01/Microsoft.Logic.json) | [Microsoft.Logic](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Logic%22&type=Code) |
| Mobiele toepassingen | Ja |  | [08-01-2015](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-08-01/Microsoft.Web.json)  | [mobileApp](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22mobileApp%22&type=Code)   |
| Mobiel zijn | Ja  |  [Mobiele betrokkenheid bij de REST](https://msdn.microsoft.com/library/azure/mt683754.aspx)        |        | [Microsoft.MobileEngagements](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.MobileEngagement%22&type=Code) |
| Zoeken | Ja  | [REST zoeken](https://msdn.microsoft.com/library/azure/dn798935.aspx) |  | [Microsoft.Search](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Search%22&type=Code) |
| Web Apps | Ja |          | [08-01-2015](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-08-01/Microsoft.Web.json) | [Microsoft.Web](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Web%22&type=Code) |

## <a name="analytics"></a>Analytics

| Service | Bronbeheer is ingeschakeld | REST-API | Schema | QuickStart-sjablonen |
| ------- | -------  | -------- | ------ | ------ |
| Gegevenscatalogus | Ja  | [Gegevenscatalogus REST](https://msdn.microsoft.com/library/azure/mt267595.aspx)  | [2016-03-30](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-03-30/Microsoft.DataCatalog.json) |  |
| Data Factory | Ja | [Data Factory REST](https://msdn.microsoft.com/library/azure/dn906738.aspx) |    | [Microsoft.DataFactory](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.DataFactory%22&type=Code) |
| Gegevens Lake Analytics | Ja |   |   [2015-10-01-voorbeeld](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-10-01-preview/Microsoft.DataLakeAnalytics.json) | [Microsoft.DataLakeAnalytics](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.DataLakeAnalytics%22&type=Code) |
| Lake gegevensarchief | Ja  | [Gegevensarchief meer REST](https://msdn.microsoft.com/library/azure/mt693424.aspx) | [2015-10-01-voorbeeld](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-10-01-preview/Microsoft.DataLakeAnalytics.json) | [Microsoft.DataLakeStore](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.DataLakeStore%22&type=Code) |
| HDInsights | Ja | [Overige HDInsights](https://msdn.microsoft.com/library/azure/mt622197.aspx) |        | [Microsoft.HDInsight](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.HDInsight%22&type=Code) |
| Machine Learning | Ja | [Leren van de REST van de computer](https://msdn.microsoft.com/library/azure/mt767538.aspx)        | [2016-05-01-voorbeeld](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-05-01-preview/Microsoft.MachineLearning.json) |
| Stream Analytics | Ja  | [Stoom Analytics REST](https://msdn.microsoft.com/library/azure/dn835031.aspx)   |        |  |
| Power BI | Ja | [Power BI ingesloten REST](https://msdn.microsoft.com/library/azure/mt712303.aspx) | [2016-01-29](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-01-29/Microsoft.PowerBI.json) |  |

## <a name="intelligence"></a>Bedrijfsinformatie

| Service | Bronbeheer is ingeschakeld | REST-API | Schema | QuickStart-sjablonen |
| ------- | ------- | -------- | ------ | ------ |
| Cognitieve Services | Ja |  | [2016-02-01-voorbeeld](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-02-01-preview/Microsoft.CognitiveServices.json) |   |

## <a name="internet-of-things"></a>Internet van dingen

| Service | Bronbeheer is ingeschakeld | REST-API | Schema | QuickStart-sjablonen |
| ------- | ------- | -------- | ------ | ------ |
| Gebeurtenis Hub | Ja  | [Overige gebeurtenis-Hub](https://msdn.microsoft.com/library/azure/dn790674.aspx) | [08-01-2015](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-08-01/Microsoft.EventHub.json) | [Microsoft.EventHub](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.EventHub%22&type=Code)  |
| IoTHubs | Ja | [IoT Hub REST](https://msdn.microsoft.com/library/azure/mt589014.aspx) | [2016-02-03](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-02-03/Microsoft.Devices.json) | [Microsoft.Devices](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Devices%22&type=Code) |
| Melding Hubs | Ja | [Melding Hub REST](https://msdn.microsoft.com/library/azure/dn495827.aspx) | [04-01-2015](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-04-01/Microsoft.NotificationHubs.json) | [Microsoft.NotificationHubs](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.NotificationHubs%22&type=Code) |

## <a name="media--cdn"></a>Media & CDN

| Service | Bronbeheer is ingeschakeld | REST-API | Schema | QuickStart-sjablonen |
| ------- | ------- | -------- | ------ | ------ |
| CDN | Ja | [REST CDN](https://msdn.microsoft.com/library/azure/mt634456.aspx)  | [2016-04-02](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-04-02/Microsoft.Cdn.json) | [Microsoft.Cdn](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Cdn%22&type=Code) |
| Media Service | Ja | [REST van mediaservices](https://msdn.microsoft.com/library/azure/hh973617.aspx)  | [10-01-2015](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-10-01/Microsoft.Media.json) |


## <a name="hybrid-integration"></a>Integratie van hybride

| Service | Bronbeheer is ingeschakeld | REST-API | Schema | QuickStart-sjablonen |
| ------- | ------- | -------- | ------ | ------ |
| BizTalk-Services | Ja |          | [04-01-2014](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2014-04-01/Microsoft.BizTalkServices.json) |  |
| Recovery-Service | Ja | [Herstel site REST](https://msdn.microsoft.com/library/azure/mt750497.aspx) | [06-01-2016](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-06-01/Microsoft.RecoveryServices.json) | [Microsoft.RecoveryServices](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.RecoveryServices%22&type=Code) |
| Bus service | Ja | [Service Bus REST](https://msdn.microsoft.com/library/azure/mt639375.aspx) | [08-01-2015](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-08-01/Microsoft.ServiceBus.json)       | [Microsoft.ServiceBus](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.ServiceBus%22&type=Code) |

## <a name="identity--access-management"></a>Identity & Access Management 

Azure Active Directory werkt met bronbeheer inschakelen op rollen gebaseerde toegangscontrole voor uw abonnement. Zie informatie over het gebruik van op rollen gebaseerde toegangscontrole en Active Directory, [Azure op rollen gebaseerde toegangscontrole](./active-directory/role-based-access-control-configure.md).

## <a name="developer-services"></a>Services voor ontwikkelaars 

| Service | Bronbeheer is ingeschakeld | REST-API | Schema | QuickStart-sjablonen |
| ------- | ------- | -------- | ------ | ------ |
| Inzichten van toepassing | Ja  | [REST inzichten](https://msdn.microsoft.com/library/azure/dn931943.aspx)  | [04-01-2014](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2014-04-01/Microsoft.Insights.json) | [Microsoft.Insights](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.insights%22&type=Code) |
| Bing Maps | Ja  |          |        |  |
| DevTest Labs | Ja |   | [2016-05-15](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-05-15/Microsoft.DevTestLab.json) | [Microsoft.DevTestLab](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.DevTestLab%22&type=Code)  |
| Visual Studio-account | Ja   |          | [2014-02-26](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2014-02-26/microsoft.visualstudio.json) |  |

## <a name="management-and-security"></a>Beheer en beveiliging

| Service | Bronbeheer is ingeschakeld | REST-API | Schema | QuickStart-sjablonen |
| ------- | ------- | -------- | ------ | ------ |
| Automatisering | Ja | [REST automatisering](https://msdn.microsoft.com/library/azure/mt662285.aspx) | [2015-10-31](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-10-31/Microsoft.Automation.json)       | [Microsoft.Automation](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Automation%22&type=Code) |
| Sleutel kluis | Ja | [Sleutel kluis REST](https://msdn.microsoft.com/library/azure/dn903609.aspx) | [Sleutel kluis](resource-manager-template-keyvault.md)<br />[De geheime sleutel kluis](resource-manager-template-keyvault-secret.md)   | [Microsoft.KeyVault](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.KeyVault%22&type=Code) |
| Operationele inzichten | Ja |          |        |  |
| Scheduler | Ja  | [Scheduler REST](https://msdn.microsoft.com/library/azure/mt629143.aspx)     | [03-01-2016](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-03-01/Microsoft.Scheduler.json) | [Microsoft.Scheduler](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Scheduler%22&type=Code) |
| Beveiliging (voorbeeld) | Ja | [Beveiliging overige](https://msdn.microsoft.com/library/azure/mt704034.aspx)  |  | [Microsoft.Security](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Security%22&type=Code)  |

## <a name="resource-manager"></a>Resource Manager

| Functie | Bronbeheer is ingeschakeld | REST-API | Schema | QuickStart-sjablonen |
| ------- | ------- | -------------- | -------- | ------ | ------ |
| Autorisatie | Ja | [Vergrendelingen beheren](https://msdn.microsoft.com/library/azure/mt204563.aspx)<br >[Toegangsbeheer op basis van rollen](https://msdn.microsoft.com/library/azure/dn906885.aspx)  | [Resource-lock](resource-manager-template-lock.md)<br />[Roltoewijzingen](resource-manager-template-role.md)  | [Microsoft.Authorization](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Authorization%22&type=Code) |
| Bronnen | Ja | [Gekoppelde bronnen](https://msdn.microsoft.com/library/azure/mt238499.aspx) | [Resource-koppelingen](resource-manager-template-links.md) | [Microsoft.Resources](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Resources%22&type=Code) |


## <a name="resource-providers-and-types"></a>Resource-providers en typen

Bij het implementeren van bronnen, moet u vaak voor het ophalen van informatie over de resource providers en typen. U kunt deze informatie met REST API Azure PowerShell en Azure CLI ophalen.

Met de provider voor een resource wilt werken, moet die provider resource aan uw account zijn geregistreerd. Veel providers van resources worden standaard automatisch geregistreerd; u wilt echter bij sommige providers bron handmatig registreren. De volgende voorbeelden wordt aangegeven hoe de registratiestatus van de provider voor een resource en registreren van de provider van de resource, als dat nodig is.

### <a name="portal"></a>Portal

U kunt eenvoudig een overzicht van ondersteunde bronnen providers met de volgende stappen uit:

1. Selecteer **Mijn machtigingen**.

    ![Mijn machtigingen](./media/resource-manager-supported-services/my-permissions.png)

2. Selecteer **provider resourcestatus**

    ![status van providers resource](./media/resource-manager-supported-services/resource-provider-status.png)

3. U ziet de volledige lijst met resources providers voor uw abonnement.

    ![lijst bron providers](./media/resource-manager-supported-services/list-providers.png)

### <a name="rest-api"></a>REST-API

Als u alle beschikbare bron gebruikt dienstverleners, met inbegrip van hun typen, locaties, API-versies en status van de registratie, de bewerking [lijst van alle leveranciers van de resource](https://msdn.microsoft.com/library/azure/dn790524.aspx) . Als u nodig hebt voor het registreren van een resource-provider, Zie [registreren een abonnement bij een provider van de resource](https://msdn.microsoft.com/library/azure/dn790548.aspx).

### <a name="powershell"></a>PowerShell

In het volgende voorbeeld ziet u hoe u alle beschikbare bron providers.

    Get-AzureRmResourceProvider -ListAvailable
    

In het volgende voorbeeld ziet u hoe de brontypen ophalen voor een bepaalde resource provider.

    (Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes
    
De uitvoer is vergelijkbaar met:

    ResourceTypeName                Locations                                         
    ----------------                ---------                                         
    sites/extensions                {Brazil South, East Asia, East US, Japan East...} 
    sites/slots/extensions          {Brazil South, East Asia, East US, Japan East...} .
    ...
    
Voor het registreren van een resource-provider, vindt u de naamruimte:

    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.ApiManagement

### <a name="azure-cli"></a>Azure CLI

In het volgende voorbeeld ziet u hoe u alle beschikbare bron providers.

    azure provider list
    
De uitvoer is vergelijkbaar met:

    info:    Executing command provider list
    + Getting registered providers
    data:    Namespace                        Registered
    data:    -------------------------------  -------------
    data:    Microsoft.ApiManagement          Unregistered
    data:    Microsoft.AppService             Registered
    data:    Microsoft.Authorization          Registered
    ...

U kunt de gegevens voor een bepaalde resource provider opslaan naar een bestand met de volgende opdracht.

    azure provider show Microsoft.Web -vv --json > c:\temp.json

Voor het registreren van een resource-provider, vindt u de naamruimte:

    azure provider register -n Microsoft.ServiceBus

## <a name="supported-regions"></a>Ondersteunde regio 's

Wanneer u bronnen implementeert, moet u meestal een gebied voor de bronnen aan te geven. Resource Manager wordt ondersteund in alle regio's, maar de bronnen die u implementeert niet ondersteund in alle regio's. Bovendien kunnen er beperkingen op uw abonnement die voorkomen dat u met behulp van sommige regio's die ondersteuning bieden voor de resource. Deze beperkingen kunnen worden gerelateerd aan de btw-problemen zijn voor uw land van herkomst of het resultaat van een beleid door de beheerder van uw abonnement voor het gebruik van bepaalde regio's geplaatst. 

Voor een volledige lijst van alle ondersteunde regio's voor alle Azure services Zie [Services per regio](https://azure.microsoft.com/regions/#services); Deze lijst bevatten echter regio's die niet worden ondersteund door uw abonnement. De regio's voor een bepaald brontype dat uw abonnement via de portal, REST API, PowerShell of Azure CLI ondersteunt, kunt u bepalen.

### <a name="portal"></a>Portal

Hier ziet u de ondersteunde regio's voor een brontype via de volgende stappen uit:

1. Selecteer **meer services** > **Resource Explorer**.

    ![resource explorer](./media/resource-manager-supported-services/select-resource-explorer.png)

2. Open het knooppunt **Providers** .

    ![providers selecteren](./media/resource-manager-supported-services/select-providers.png)

3. Selecteer een provider voor de resource en de ondersteunde regio's en de API-versies weergeven.

    ![provider weergeven](./media/resource-manager-supported-services/view-provider.png)

### <a name="rest-api"></a>REST-API

Om te ontdekken welke regio's beschikbaar zijn voor een bepaald brontype op uw abonnement, gebruikt u de [lijst van alle leveranciers van de resource](https://msdn.microsoft.com/library/azure/dn790524.aspx) -bewerking. 

### <a name="powershell"></a>PowerShell

In het volgende voorbeeld ziet u hoe u de ondersteunde regio's voor websites.

    ((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes | Where-Object ResourceTypeName -eq sites).Locations
    
De uitvoer is vergelijkbaar met:

    Brazil South
    East Asia
    East US
    Japan East
    Japan West
    North Central US
    North Europe
    South Central US
    West Europe
    West US
    Southeast Asia
    Central US
    East US 2

### <a name="azure-cli"></a>Azure CLI

In het volgende voorbeeld geeft als resultaat alle ondersteunde locaties voor elk resourcetype.

    azure location list

U kunt ook de resultaten van de locatie met een hulpprogramma zoals [jq](https://stedolan.github.io/jq/)van JSON filteren.

    azure location list --json | jq '.[] | select(.name == "Microsoft.Web/sites")'

Die wordt geretourneerd:

    {
      "name": "Microsoft.Web/sites",
      "location": "Brazil South,East Asia,East US,Japan East,Japan West,North Central US,
            North Europe,South Central US,West Europe,West US,Southeast Asia,Central US,East US 2"
    }

## <a name="supported-api-versions"></a>Ondersteunde API-versies

Wanneer u een sjabloon implementeert, moet u een versie van de API te gebruiken voor het maken van elke resource. De API versie overeenkomt met een versie van de REST API-bewerkingen die zijn uitgebracht door de provider van de resource. Als een resource provider kunt nieuwe functies, geeft het een nieuwe versie van de REST API. Daarom is de versie van de API die u hebt opgegeven in de sjabloon invloed op welke eigenschappen u in de sjabloon opgeven kunt. In het algemeen wilt u bij het maken van sjablonen selecteert u de meest recente versie van de API. U kunt bepalen of u door wilt gaan met een eerdere versie van de API of de sjabloon voor de meest recente versie om te profiteren van nieuwe functies voor het bijwerken van bestaande sjablonen.

### <a name="portal"></a>Portal

U bepaalt de ondersteunde API-versies op dezelfde manier als die u ondersteunde regio's (Zie eerder) vastgesteld.

### <a name="rest-api"></a>REST-API

Om te ontdekken welke API-versies zijn beschikbaar voor verschillende brontypen, gebruikt u de [lijst van alle leveranciers van de resource](https://msdn.microsoft.com/library/azure/dn790524.aspx) -bewerking. 

### <a name="powershell"></a>PowerShell

In het volgende voorbeeld ziet u hoe u de beschikbare versies van de API voor een bepaald brontype.

    ((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes | Where-Object ResourceTypeName -eq sites).ApiVersions
    
De uitvoer is vergelijkbaar met:
    
    2015-08-01
    2015-07-01
    2015-06-01
    2015-05-01
    2015-04-01
    2015-02-01
    2014-11-01
    2014-06-01
    2014-04-01-preview
    2014-04-01

### <a name="azure-cli"></a>Azure CLI

Voor een resource provider naar een bestand met de volgende opdracht kunt u de gegevens (met inbegrip van de beschikbare versies van de API) opslaan.

    azure provider show Microsoft.Web -vv --json > c:\temp.json

U kunt het bestand openen en het element **apiVersions**

## <a name="next-steps"></a>Volgende stappen

- Zie meer informatie over het maken van Resource Manager-sjablonen, [sjablonen ontwerpen Azure Resource Manager](resource-group-authoring-templates.md).
- Zie meer informatie over het implementeren van bronnen, [een toepassing met Azure Resource Manager sjabloon distribueren](resource-group-template-deploy.md).
