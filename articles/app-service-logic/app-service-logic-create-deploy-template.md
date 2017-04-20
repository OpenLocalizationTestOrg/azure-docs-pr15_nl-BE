<properties
   pageTitle="Een sjabloon voor de implementatie logica app maken | Microsoft Azure"
   description="Leren hoe u een sjabloon voor de implementatie logica app maken en deze gebruiken voor het vrijgeven"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="jeffhollan"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="10/18/2016"
   ms.author="jehollan"/>

# <a name="create-a-logic-app-deployment-template"></a>Een sjabloon voor de implementatie logica app maken

Nadat een logica app is gemaakt, is het raadzaam deze als een bronnenbeheerder Azure-sjabloon maken. Op deze manier kunt u gemakkelijk implementeren de app logica aan u deze zult resourcegroep of omgeving. Voor een inleiding tot de bronnenbeheerder sjablonen, worden artikelen op [Azure Resource Manager sjablonen ontwerpen](../resource-group-authoring-templates.md) en [implementeren van bronnen met behulp van sjablonen Azure Resource Manager](../resource-group-template-deploy.md)controleert.

## <a name="logic-app-deployment-template"></a>Sjabloon voor implementatie van logica app

Een app logica bestaat uit drie basisonderdelen:

* **Toepassingsbron logica**. Deze bron bevat informatie over zaken als prijzen plan, de locatie en de definitie van de werkstroom.
* **Definitie van de werkstroom**. Dit is wat wordt weergegeven in de codeweergave. Bevat de definitie van de stappen van de stroom en hoe de motor dient te worden uitgevoerd. Dit is de `definition` eigenschap van de toepassingsbron logica.
* **Verbindingen**. Dit zijn afzonderlijke resources die metagegevens over een connector-verbindingen, zoals een verbindingsreeks en een toegangstoken veilig worden opgeslagen. U verwijst in een app logica in de `parameters` sectie van de logica app resource.

U kunt alle onderdelen voor bestaande logica apps weergeven met behulp van een hulpprogramma zoals [Azure Resource Explorer](http://resources.azure.com).

Als u een sjabloon voor een app logica te gebruiken met een resource groep implementaties, moet u de resources definiëren en desgewenst voorzien. Bijvoorbeeld als u op een ontwikkelings-, test- en productieomgeving, zult u waarschijnlijk willen gebruiken tekenreeksen met andere verbinding met een SQL-database in elke omgeving. Of u wilt implementeren in de verschillende abonnementen of resourcegroepen.  

## <a name="create-a-logic-app-deployment-template"></a>Een sjabloon voor de implementatie logica app maken

De eenvoudigste manier om een sjabloon geldig logica app-implementatie is de [Visual Studio Tools for Logic Apps](./app-service-logic-deploy-from-vs.md)kunt gebruiken.  De Visual Studio tools genereren een geldige implementatiesjabloon die kan worden gebruikt via een abonnement of locatie.

Een paar andere hulpmiddelen kunnen u helpen bij het maken van een sjabloon voor de implementatie logica app. U kunt schrijven met de hand, met behulp van de bronnen al hier besproken parameters maken als nodig is. Een andere mogelijkheid is een [logica app sjabloonauteur](https://github.com/jeffhollan/LogicAppTemplateCreator) PowerShell-module gebruiken. Deze open source-module eerst geëvalueerd en wordt de logica-app en alle verbindingen die het gebruikt en genereert vervolgens een sjabloon resources met de benodigde parameters voor de implementatie. Bijvoorbeeld als er een logica app die een bericht ontvangt van een wachtrij Azure Service Bus en worden gegevens toegevoegd aan een Azure SQL-database, wordt het hulpprogramma behouden alle de logica orchestration en de SQL-Service Bus en verbindingsreeksen voorzien zodat ze kunnen worden ingesteld bij de implementatie.

>[AZURE.NOTE] Verbindingen moeten in dezelfde bronnengroep als de app logica.

### <a name="install-the-logic-app-template-powershell-module"></a>De module logica app sjabloon PowerShell installeren

De eenvoudigste manier om het installeren van de module is via de [PowerShell galerie](https://www.powershellgallery.com/packages/LogicAppTemplate/0.1)met de opdracht `Install-Module -Name LogicAppTemplate`.  

Ook kunt u de PowerShell-module handmatig:

1. Download de nieuwste versie van de [sjabloonauteur logica-app](https://github.com/jeffhollan/LogicAppTemplateCreator/releases).  
1. Pak de map in de map module PowerShell (meestal `%UserProfile%\Documents\WindowsPowerShell\Modules`).

Voor de module om te werken met de huurder en abonnement toegang token, te gebruiken met het opdrachtregelprogramma [ARMClient](https://github.com/projectkudu/ARMClient) .  Deze [blog posten](http://blog.davidebbo.com/2015/01/azure-resource-manager-client.html) worden ARMClient in meer detail besproken.

### <a name="generate-a-logic-app-template-by-using-powershell"></a>Een sjabloon logica app via PowerShell genereren

Als PowerShell is geïnstalleerd, kunt u een sjabloon genereren met de volgende opdracht:

`armclient token $SubscriptionId | Get-LogicAppTemplate -LogicApp MyApp -ResourceGroup MyRG -SubscriptionId $SubscriptionId -Verbose | Out-File C:\template.json`

`$SubscriptionId`is de Azure abonnement-ID. Deze regel voor de eerste keer een token via ARMClient, en vervolgens tot en met de PowerShell-script pijpen en maakt vervolgens de sjabloon in een JSON-bestand.

## <a name="add-parameters-to-a-logic-app-template"></a>Parameters toevoegen aan een sjabloon logica app

Nadat u uw app logica sjabloon maakt, kunt u toevoegen of wijzigen van de parameters die u mogelijk moet blijven. Bijvoorbeeld, als de definitie van een resource-ID voor een Azure functie of geneste workflow die u wilt implementeren in een implementatie omvat, kunt u meer bronnen toevoegen aan uw sjabloon en voorzien van id's wanneer dat nodig is. Hetzelfde geldt voor alle verwijzingen naar aangepaste API's of Swagger eindpunten die u verwacht te implementeren met elke bronnengroep.

## <a name="deploy-a-logic-app-template"></a>Een sjabloon logica app implementeren

U kunt de sjabloon kunt implementeren met behulp van een aantal hulpmiddelen, zoals PowerShell, REST API, Visual Studio Release Management en de implementatie van Azure Portal sjabloon. Zie dit artikel over het [implementeren van bronnen met behulp van bronbeheer Azure-sjablonen](../resource-group-template-deploy.md) voor meer informatie. Ook is het raadzaam dat u een [parameterbestand](../resource-group-template-deploy.md#parameter-file) voor het opslaan van waarden voor de parameter maken.

### <a name="authorize-oauth-connections"></a>OAuth verbindingen toestaan

Na de installatie werkt de app logica end-to-end met geldige parameters. Echter, OAuth verbindingen nog steeds moet worden gemachtigd om een geldig access token te genereren. U kunt dit doen door de logica app openen in de ontwerpfunctie en vervolgens machtiging van verbindingen. Of als u automatiseren wilt, kunt u een script toe te staan van elke verbinding OAuth. Er is een voorbeeldscript op GitHub onder het [LogicAppConnectionAuth](https://github.com/logicappsio/LogicAppConnectionAuth) -project.

## <a name="visual-studio-release-management"></a>Visual Studio Release Management

Gebruik een hulpprogramma zoals Visual Studio Release Management met een sjabloon voor de implementatie logica app is een gangbare scenario voor het implementeren en beheren van een omgeving. Visual Studio Team Services bevat een [Resourcegroep Azure implementeren](https://github.com/Microsoft/vsts-tasks/tree/master/Tasks/DeployAzureResourceGroup) taak toevoegen aan elke build of pijpleiding vrijgeven. Moet u een [service principal](https://blogs.msdn.microsoft.com/visualstudioalm/2015/10/04/automating-azure-resource-group-deployment-using-a-service-principal-in-visual-studio-online-buildrelease-management/) voor een vergunning om te implementeren en vervolgens genereert u de definitie van de release.

1. Release Management, een nieuwe definitie maken, selecteer **leeg** te starten met een definitie van een leeg.

    ![De definitie van een nieuw, leeg maken][1]   

1. U voor deze moet resources kiezen. Dit zal waarschijnlijk de logica app sjabloon handmatig of als deel van het bouwproces gegenereerd.
1. Een **Implementatie van Azure Resource groep** taak toevoegen.
1. Configureren met een [service principal](https://blogs.msdn.microsoft.com/visualstudioalm/2015/10/04/automating-azure-resource-group-deployment-using-a-service-principal-in-visual-studio-online-buildrelease-management/)en verwijst naar de sjabloon en sjabloonparameters bestanden.
1. Doorgaan met het bouwen van de stappen in het release-proces voor het milieu, geautomatiseerde test of fiatteurs indien nodig.

<!-- Image References -->
[1]: ./media/app-service-logic-create-deploy-template/emptyReleaseDefinition.PNG
