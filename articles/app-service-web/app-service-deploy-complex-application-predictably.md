<properties
    pageTitle="Inrichten en implementeren van microservices voorspelbaar in Azure"
    description="Informatie over het implementeren van een toepassing die bestaat uit microservices in Azure App-Service als geheel en op een voorspelbare wijze met behulp van sjablonen resource JSON en PowerShell-scripts."
    services="app-service"
    documentationCenter=""
    authors="cephalin"
    manager="wpickett"
    editor="jimbe"/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="01/06/2016"
    ms.author="cephalin"/>


# <a name="provision-and-deploy-microservices-predictably-in-azure"></a>Inrichten en implementeren van microservices voorspelbaar in Azure #

Deze zelfstudie laat zien hoe inrichten en implementeren van een toepassing die bestaat uit [microservices](https://en.wikipedia.org/wiki/Microservices) in [Azure App-Service](/services/app-service/) als geheel en op een voorspelbare wijze met behulp van sjablonen resource JSON en PowerShell-scripts. 

Bij het inrichten en implementeren van hoog-toepassingen die zijn samengesteld uit zeer ontkoppeld zijn microservices, herhaalbaarheid en voorspelbaarheid cruciaal voor succes. [Azure App-Service](/services/app-service/) kunt u microservices web apps, mobiele apps, API apps en logica apps te maken. [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) kunt u alle microservices als een eenheid, samen met de afhankelijke bronnen zoals database beheren en instellingen voor de gegevensbron. U kunt nu ook een toepassing met behulp van sjablonen JSON en eenvoudige PowerShell-scripts implementeren. 

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

## <a name="what-you-will-do"></a>Wat u doet ##

In de zelfstudie implementeert u een toepassing die gebruikmaakt van:

-   Twee web-apps (dat wil zeggen twee microservices)
-   Een back-end SQL-Database
-   Instellingen van de App en verbindingsreeksen bronbeheer
-   Toepassing inzichten, waarschuwingen, autoscaling instellingen

## <a name="tools-you-will-use"></a>Hulpprogramma's die u wilt gebruiken ##

In deze zelfstudie gebruikt u de volgende hulpprogramma's. Omdat geen uitgebreide discussie op Extra, ga ik blijven vasthouden aan het end-to-end scenario en gewoon kunt u een korte inleiding op elk, en vindt u meer informatie over het. 

### <a name="azure-resource-manager-templates-json"></a>Azure Resource Manager-sjablonen (JSON) ###
 
Telkens wanneer u een web app in Azure App-Service maakt, bijvoorbeeld een JSON-sjabloon door Azure Resource Manager gebruikt de hele resourcegroep maken met de bronnen van de component. Een complexe sjabloon van [Azure Marketplace](/marketplace) zoals de [Schaalbare WordPress](/marketplace/partners/wordpress/scalablewordpress/) app de MySQL-database, opslag rekeningen, App serviceplan en de web app zelf zijn kan, waarschuwingsregels, app instellingen, instellingen voor automatisch schalen en meer, en alle sjablonen zijn beschikbaar voor u via PowerShell. Zie voor meer informatie over het downloaden en deze sjablonen gebruiken [Met behulp van Azure PowerShell Azure Resource Manager](../powershell-azure-resource-manager.md).

Zie voor meer informatie over de sjablonen Azure Resource Manager, [Azure Resource Manager sjablonen ontwerpen](../resource-group-authoring-templates.md)

### <a name="azure-sdk-26-for-visual-studio"></a>Azure SDK 2.6 voor Visual Studio ###

De nieuwste SDK bevat verbeteringen voor de ondersteuning van de sjabloon Resource Manager in de JSON-editor. U kunt dit snel nieuwe resource groep sjabloon maken of open een bestaande sjabloon JSON (bijvoorbeeld een sjabloon gedownload galerie) gebruiken voor het wijzigen van, het parameterbestand invullen en zelfs de resourcegroep rechtstreeks uit een resourcegroep Azure-oplossing te implementeren.

Zie [2.6 van Azure SDK voor Visual Studio](/blog/2015/04/29/announcing-the-azure-sdk-2-6-for-net/)voor meer informatie.

### <a name="azure-powershell-080-or-later"></a>Azure PowerShell 0.8.0 of hoger ###

Vanaf versie 0.8.0 wordt omvat de Azure PowerShell installatie de module Azure Resource Manager naast de module Azure. Deze nieuwe module kunt u de implementatie van resourcegroepen script.

Zie voor meer informatie, [Met behulp van Azure PowerShell Azure Resource Manager](../powershell-azure-resource-manager.md)

### <a name="azure-resource-explorer"></a>Azure Resource Explorer ###

Dit [hulpprogramma voorbeeld](https://resources.azure.com) kunt u de JSON-definities van de resourcegroepen in uw abonnement en de afzonderlijke resources verkennen. In het hulpprogramma kunt u de JSON-definities van een resource bewerken, verwijderen van de gehele hiërarchie van resources en maken van nieuwe resources.  De informatie gemakkelijk beschikbaar in dit hulpmiddel is erg handig voor het ontwerpen van een sjabloon omdat er wordt aangegeven welke eigenschappen u nodig hebt om in te stellen voor een bepaald type resource, de juiste waarden, enz. U kunt zelfs de bronnengroep maken in de [Portal Azure](https://portal.azure.com/)vervolgens controleren de JSON-definities in de explorer tool waarmee u de resourcegroep templatize.

### <a name="deploy-to-azure-button"></a>Azure knop implementeren ###

Als u GitHub voor het besturingselement gebruikt, kunt u een [distribueren naar Azure knop](/blog/2014/11/13/deploy-to-azure-button-for-azure-websites-2/) plaatsen in uw Leesmij-bestand. MD, waarmee een turn-key implementatie tot Azure. Terwijl u dit voor een eenvoudige web app doen kunt, kunt u hierop als u de implementatie van een hele resource-groep voor de toevoeging van een azuredeploy.json-bestand in de hoofdmap van de bibliotheek uitbreiden. Dit JSON-bestand waarin de sjabloon voor de groep resources wordt gebruikt door de distribueren naar Azure knop voor het maken van de resourcegroep. Zie voor een voorbeeld, het monster [ToDoApp](https://github.com/azure-appservice-samples/ToDoApp) , die u in deze zelfstudie wilt gebruiken.

## <a name="get-the-sample-resource-group-template"></a>De sjabloon voorbeeldgegevens resource groep ##

Nu gaan we direct aan het.

1.  Navigeer naar het App-Service [ToDoApp](https://github.com/azure-appservice-samples/ToDoApp) monster.

2.   In readme.md, klik op **Deploy om Azure**.
 
3.  U bent genomen om de site [naar azure implementeren](https://deploy.azure.com) en implementatie invoerparameters gevraagd. U ziet dat de meeste velden worden gevuld met de naam van de opslagplaats en enkele willekeurige tekenreeksen. U kunt alle velden wijzigen als u wilt, maar de enige dingen die u moet invoeren zijn de administratieve SQL Server-aanmeldingsnaam en het wachtwoord en klik op **volgende**.
 
    ![](./media/app-service-deploy-complex-application-predictably/gettemplate-1-deploybuttonui.png)

4.  Klik vervolgens op **Deploy** om het implementatieproces te starten. Zodra het proces wordt volledig wordt uitgevoerd, klikt u op de http://todoapp*XXXX*. azurewebsites.net koppelen aan de gedistribueerde toepassing te bladeren. 

    ![](./media/app-service-deploy-complex-application-predictably/gettemplate-2-deployprogress.png)

    De gebruikersinterface zou worden een beetje traag wanneer u eerst naar het omdat de apps gewoon worden gestart, maar bewegen zelf is een volledig functionele toepassing.

5.  Klik op de link **beheren** als u wilt zien van de nieuwe toepassing in de Portal Azure terug in de pagina implementeren.

6.  Klik op de koppeling bron groep in de vervolgkeuzelijst **Essentials** . Let ook op de web app is al verbonden met de opslagplaats GitHub onder **Externe Project**. 

    ![](./media/app-service-deploy-complex-application-predictably/gettemplate-3-portalresourcegroup.png)
 
7.  In de groep resources blade, houd er rekening mee dat er nog twee web apps en een SQL-Database in de resourcegroep.

    ![](./media/app-service-deploy-complex-application-predictably/gettemplate-4-portalresourcegroupclicked.png)
 
Alles wat u gezien in een paar korte minuten is een toepassing volledig geïmplementeerde twee-microservice, met alle onderdelen, afhankelijkheden, instellingen, database en continue publishing, ingesteld door een geautomatiseerde orchestration Azure Resource Manager. Dit werd gedaan door twee dingen:

-   De distribueren naar Azure knop
-   azuredeploy.JSON in de hoofdmap van de repo

U kunt deze toepassing implementeren tientallen, honderden of duizenden malen en exact dezelfde configuratie elke keer. De herhaalbaarheid en de voorspelbaarheid van deze benadering kunt u toepassingen voor hoge schaalbaarheid met gemak en vertrouwen implementeren.

## <a name="examine-or-edit-azuredeployjson"></a>AZUREDEPLOY onderzoek (of bewerken). JSON ##

Nu gaan we kijken hoe de opslagplaats GitHub is ingesteld. U zult met de JSON-editor in de Azure .NET SDK, dus als u nog niet hebt geïnstalleerd [Azure .NET SDK 2.6](/downloads/), doet u dit nu.

1.  De [ToDoApp](https://github.com/azure-appservice-samples/ToDoApp) -bibliotheek met uw favoriete git gereedschap klonen. In het screenshot hieronder ben ik hierdoor in de Explorer-Team in Visual Studio 2013.

    ![](./media/app-service-deploy-complex-application-predictably/examinejson-1-vsclone.png)

2.  Open in de hoofdmap van de opslagplaats azuredeploy.json in Visual Studio. Als u het overzichtsvenster JSON niet ziet, moet u Azure .NET SDK installeren.

    ![](./media/app-service-deploy-complex-application-predictably/examinejson-2-vsjsoneditor.png)

Ik ga niet beschrijven elk detail van de JSON-indeling, maar het gedeelte [Meer informatie](#resources) bevat koppelingen voor het leren van de taal van de resource groep sjabloon. Hier, Ga alleen ik tonen de interessante functies die u kunnen helpen bij het maken van uw eigen aangepaste sjabloon voor implementatie van app aan de slag.

### <a name="parameters"></a>Parameters ###

Kijk eens naar de sectie parameters om te zien dat de meeste van deze parameters zijn wat de knop **distribueren naar Azure** gevraagd. De site achter de knop **distribueren naar Azure** vult de invoer met behulp van de parameters die zijn gedefinieerd in azuredeploy.json UI. Deze parameters worden gebruikt in de definities van de resource, zoals namen, waarden, enz.

### <a name="resources"></a>Bronnen ###

In het knooppunt voor resources, kunt u zien dat resources met 4 op het hoogste niveau zijn gedefinieerd, met inbegrip van een exemplaar van SQL Server, een App serviceplan en twee web apps. 

#### <a name="app-service-plan"></a>App-serviceplan ####

Laten we beginnen met een eenvoudige hoofdniveau bron in de JSON. Klik op het plan App-Service met de naam **[hostingPlanName]** de overeenkomstige JSON-code markeren in het overzicht JSON. 

![](./media/app-service-deploy-complex-application-predictably/examinejson-3-appserviceplan.png)

Houd er rekening mee dat de `type` element geeft u de tekenreeks voor een App serviceplan (het is wel een server-farm een lange, lange tijd geleden), en andere elementen en eigenschappen worden ingevuld met behulp van de parameters die zijn gedefinieerd in het bestand JSON en deze bron heeft geen geneste bronnen.

>[AZURE.NOTE] Denk er aan dat de waarde van `apiVersion` wordt aangegeven welke versie van de definitie van de resource JSON met gebruik van de REST-API en het kan invloed hebben op hoe de resource moet worden opgemaakt in Azure de `{}`. 

#### <a name="sql-server"></a>SQL Server ####

Klik op de SQL Server-bron met de naam **SQL Server** in het overzicht JSON.

![](./media/app-service-deploy-complex-application-predictably/examinejson-4-sqlserver.png)
 
Let op het volgende over de gemarkeerde JSON-code:

-   Het gebruik van parameters zorgt ervoor dat de bronnen gemaakt zijn met de naam geconfigureerd op een manier die consistent zijn met elkaar worden gemaakt.
-   De SQL Server-bron heeft twee geneste resources, elk een verschillende waarde voor `type`.
-   Geneste bronnen binnen `“resources”: […]`, waar de database en de firewall-regels worden gedefinieerd, hebben een `dependsOn` -element waarin de resource-ID van de resource hoofdniveau SQL Server. Zo weet Azure Resource Manager, "voordat u deze resource die andere bron moet al bestaan; en als die andere bron is gedefinieerd in de sjabloon, maakt dat een eerste".

    >[AZURE.NOTE] Voor gedetailleerde informatie over het gebruik van de `resourceId()` werken, Zie [Bronbeheer sjabloon van Microsoft Azure](../resource-group-template-functions.md).

-   Het effect van de `dependsOn` -element is dat Azure Resource Manager weet welke bronnen parallel kunnen worden gemaakt en welke bronnen moeten opeenvolgend worden gemaakt. 

#### <a name="web-app"></a>Web app. ####

Nu laten we doorgaan met de werkelijke web-apps zelf, die gecompliceerder zijn. Klik op de web-app [variables('apiSiteName')] in het overzicht JSON de JSON-code markeren. U zult zien dat dingen zijn het veel meer interessant. Voor dit doel, wil ik het hebben over de functies één voor één:

##### <a name="root-resource"></a>Toegangspuntbron #####

De web app is afhankelijk van twee verschillende bronnen. Dit betekent dat Azure Resource Manager de web app maakt nadat het abonnement App- en het exemplaar van SQL Server worden gemaakt.

![](./media/app-service-deploy-complex-application-predictably/examinejson-5-webapproot.png)

##### <a name="app-settings"></a>App instellingen #####

De instellingen van de app zijn ook gedefinieerd als een geneste resource.

![](./media/app-service-deploy-complex-application-predictably/examinejson-6-webappsettings.png)

In de `properties` -element voor `config/appsettings`, twee app instellingen zijn in de notatie `“<name>” : “<value>”`.

-   `PROJECT`is een [instelling van de KUDU](https://github.com/projectkudu/kudu/wiki/Customizing-deployments) waarin Azure-implementatie te gebruiken in een oplossing met meerdere Visual Studio-project. Leest u hoe het besturingselement gegevensbron is geconfigureerd, maar aangezien de code ToDoApp in een project met meerdere Visual Studio-oplossing, moeten we deze instelling.
-   `clientUrl`is gewoon een instelling waarmee de toepassingscode app wordt gebruikt.

##### <a name="connection-strings"></a>Verbindingsreeksen #####

De verbindingsreeks zijn ook gedefinieerd als een geneste resource.

![](./media/app-service-deploy-complex-application-predictably/examinejson-7-webappconnstr.png)

In de `properties` -element voor `config/connectionstrings`, de verbindingsreeks ook wordt gedefinieerd als een combinatie van de naam: waarde, met de specifieke indeling van `“<name>” : {“value”: “…”, “type”: “…”}`. Voor de `type` -element, mogelijke waarden zijn `MySql`, `SQLServer`, `SQLAzure`, en `Custom`.

>[AZURE.TIP] Voor een definitieve lijst van de string verbindingstypen, kunt u de volgende opdracht uitvoeren in Azure PowerShell: \[Enum]::GetNames("Microsoft.WindowsAzure.Commands.Utilities.Websites.Services.WebEntities.DatabaseType")
    
##### <a name="source-control"></a>Besturingselement voor gegevensbron #####

De instellingen voor bronbeheer zijn ook gedefinieerd als een geneste resource. Azure Resource Manager gebruikt deze bron continue publicatie configureren (Zie voorbehoud op `IsManualIntegration` later) en ook te ere van de implementatie van de toepassingscode automatisch tijdens de verwerking van de JSON-bestand.

![](./media/app-service-deploy-complex-application-predictably/examinejson-8-webappsourcecontrol.png)

`RepoUrl`en `branch` moeten heel intuïtief en verwijzen naar de Git repository en de naam van de subsleutel die u wilt publiceren uit. Nogmaals, deze worden gedefinieerd door de invoerparameters. 

Opmerking de `dependsOn` element dat naast de webbron app zelf, `sourcecontrols/web` ook afhankelijk van `config/appsettings` en `config/connectionstrings`. Dit komt omdat zodra `sourcecontrols/web` is geconfigureerd, het proces Azure-implementatie wordt automatisch geprobeerd te implementeren, bouwen en start de toepassingscode. Daarom helpt u ervoor te zorgen dat de toepassing toegang tot de instellingen van de app vereist en verbindingsreeksen heeft vóór het uitvoeren van de code van de toepassing deze afhankelijkheid invoegen. 

>[AZURE.NOTE] Denk er aan dat `IsManualIntegration` is ingesteld op `true`. Deze eigenschap is in deze zelfstudie nodig omdat u de opslagplaats GitHub niet werkelijk de eigenaar en dus kan niet werkelijk leesmachtiging Azure (dat wil zeggen continue publiceren vanaf [ToDoApp](https://github.com/azure-appservice-samples/ToDoApp) configureren push-opslagplaats voor automatische updates naar Azure). U kunt de standaardwaarde `false` voor de opgegeven bibliotheek alleen als u de eigenaar van GitHub referenties hebt geconfigureerd in [Azure portal](https://portal.azure.com/) vóór. Met andere woorden, als u eerder een besturingselement voor de GitHub of BitBucket voor elke toepassing in de [Azure Portal](https://portal.azure.com/) hebt ingesteld, zal met de referenties van de gebruiker vervolgens Azure de referenties onthouden en deze gebruiken wanneer u in de toekomst een app van GitHub of BitBucket implementeren. Echter, als u dit nog niet hebt gedaan, implementatie van de sjabloon JSON mislukt tijdens het configureren van instellingen voor de web-app bronbeheer omdat deze zich niet in GitHub of BitBucket met de referenties van de eigenaar van de opslagplaats aanmelden Azure Resource Manager.

## <a name="compare-the-json-template-with-deployed-resource-group"></a>Vergelijk de JSON-sjabloon met de geïmplementeerde resourcegroep ##

Hier kunt u alle de web app van blades in [Azure Portal](https://portal.azure.com/)doorlopen, maar er is een ander hulpmiddel dat net zo nuttig als niet meer. Ga naar de voorbeeldfunctie [Azure Resource Explorer](https://resources.azure.com) , waarin u een JSON representatie van de resourcegroepen in uw abonnementen die daadwerkelijk zijn in de backend Azure. Ook ziet u hoe de resourcegroep JSON hiërarchie in Azure komt overeen met de hiërarchie in het sjabloonbestand dat wordt gebruikt om deze te maken.

Bijvoorbeeld, wanneer ik ga naar de [Azure Resource Explorer](https://resources.azure.com) tool en vouw de knooppunten in de explorer, ziet ik de resourcegroep en het hoofdniveau resources die onder hun respectieve brontypen worden verzameld.

![](./media/app-service-deploy-complex-application-predictably/ARM-1-treeview.png)

Als u een web app bereiken, moet u kunnen zien web app configuratiedetails vergelijkbaar met de onderstaande schermafbeelding:

![](./media/app-service-deploy-complex-application-predictably/ARM-2-jsonview.png)

Nogmaals, de geneste bronnen moet een hiërarchie, vergelijkbaar met die in het sjabloonbestand JSON en ziet u de app instellingen, tekenreeksen, enzovoort, correct worden weergegeven in het deelvenster JSON. Het ontbreken van instellingen kan duiden op een probleem met uw JSON-bestand en kan helpen bij het oplossen van uw sjabloonbestand JSON.

## <a name="deploy-the-resource-group-template-yourself"></a>Implementatie van de resource groep sjabloon ##

De knop **distribueren naar Azure** is prima, maar kunt u voor de implementatie van de resource groep sjabloon in azuredeploy.json alleen als u al azuredeploy.json naar GitHub hebben geduwd. De Azure .NET SDK biedt ook de hulpmiddelen voor het implementeren van een JSON-sjabloonbestand rechtstreeks vanaf uw lokale computer. Volg hiervoor de onderstaande stappen:

1.  In Visual Studio, klikt u op het **bestand** > **Nieuw** > **Project**.

2.  Klik op **Visual C#** > **wolk** > **Resourcegroep Azure**, klikt u op **OK**.

    ![](./media/app-service-deploy-complex-application-predictably/deploy-1-vsproject.png)

3.  **Lege sjabloon** selecteren in **Azure sjabloon selecteren**en klik op **OK**.

4.  Azuredeploy.json naar **de map van het nieuwe project** slepen.

    ![](./media/app-service-deploy-complex-application-predictably/deploy-2-copyjson.png)

5.  Open het gekopieerde azuredeploy.json in de Solution Explorer.

6.  NET voor de demonstratie, we enkele standaard toepassing inzicht resources toevoegen aan onze JSON-bestand door te klikken op de **Resource toevoegen**. Als u alleen geïnteresseerd bent in de JSON-bestand implementeren, gaat u verder met de stappen voor het implementeren.

    ![](./media/app-service-deploy-complex-application-predictably/deploy-3-newresource.png)

7.  Selecteer **Toepassing inzichten voor Web Apps**, Controleer of een bestaande App Service plannen en web app is geselecteerd en klik vervolgens op **toevoegen**.

    ![](./media/app-service-deploy-complex-application-predictably/deploy-4-newappinsight.png)

    Nu zult u kunt diverse nieuwe bronnen zien die, afhankelijk van de bron en wat het doet, afhankelijk van het abonnement App of de web app. Deze bronnen zijn niet ingeschakeld door de bestaande definitie en u wilt wijzigen.

    ![](./media/app-service-deploy-complex-application-predictably/deploy-5-appinsightresources.png)
 
8.  Klik in het overzicht JSON **appInsights automatisch schalen** om de JSON-code markeren. Dit is de instelling van de schaal voor uw serviceplan App.

9.  Zoek in de gemarkeerde JSON-code, de `location` en `enabled` eigenschappen en deze instellen zoals hieronder wordt weergegeven.

    ![](./media/app-service-deploy-complex-application-predictably/deploy-6-autoscalesettings.png)

10. Klik in het overzicht JSON **CPUHigh appInsights** om de JSON-code markeren. Dit is een waarschuwing.

11. Zoek de `location` en `isEnabled` eigenschappen en deze instellen zoals hieronder wordt weergegeven. Doe hetzelfde voor de andere drie waarschuwingen (paarse bollen).

    ![](./media/app-service-deploy-complex-application-predictably/deploy-7-alerts.png)

12. U bent nu klaar om te implementeren. Klik met de rechtermuisknop op het project en selecteer **distribueren** > **Nieuwe distributie**.

    ![](./media/app-service-deploy-complex-application-predictably/deploy-8-newdeployment.png)

13. Log in op uw Azure-account als u nog niet hebt gedaan.

14. Een bestaande resourcegroep selecteren in uw abonnement of maak een nieuwe, selecteer **azuredeploy.json**en klik op **Parameters bewerken**.

    ![](./media/app-service-deploy-complex-application-predictably/deploy-9-deployconfig.png)

    Nu zult u de parameters die zijn gedefinieerd in het sjabloonbestand dat in een mooi tabel bewerken. Parameters die bepalen de standaardinstellingen voor al hun standaardwaarden en parameters die een van toegestane waarden lijst wordt weergegeven als vervolgkeuzelijsten.

    ![](./media/app-service-deploy-complex-application-predictably/deploy-10-parametereditor.png)

15. Vul de lege parameters en gebruik de [GitHub repo adres voor ToDoApp](https://github.com/azure-appservice-samples/ToDoApp.git) in **repoUrl**. Klik vervolgens op **Opslaan**.
 
    ![](./media/app-service-deploy-complex-application-predictably/deploy-11-parametereditorfilled.png)

    >[AZURE.NOTE] AutoScaling is een functie die wordt aangeboden in de **standaard** niveau of hoger en plan niveau waarschuwingen zijn functies die worden aangeboden in **Basic** laag of hoger, moet u de parameter instellen op de **sku** **Standard** of **Premium** Zie alle uw App inzicht resources oplichten.
    
16. Klik op **installeren**. Als u **wachtwoorden opslaan**hebt ingeschakeld, wordt het wachtwoord opgeslagen in de parameter bestand **als tekst zonder opmaak**. Anders wordt u gevraagd het wachtwoord invoeren tijdens de implementatie.

Dat is alles. Nu moet u Ga naar de [Azure Portal](https://portal.azure.com/) en de [Azure Resource Explorer](https://resources.azure.com) tool voor een overzicht van de nieuwe waarschuwingen en instellingen voor automatisch schalen toegevoegd aan uw JSON toepassing geïmplementeerd.

De stappen in dit gedeelte uitgevoerd hoofdzakelijk het volgende:

1.  Bereid het sjabloonbestand
2.  Gemaakt van een parameterbestand om te gaan met het sjabloonbestand
3.  Het sjabloonbestand met de parameterbestand geïmplementeerd

De laatste stap is gemakkelijk gedaan door een PowerShell-cmdlet. Overzicht van Visual Studio heeft wanneer deze uw toepassing is geïmplementeerd, open Scripts\Deploy AzureResourceGroup.ps1. Er is een heleboel code er maar gewoon ga ik Selecteer de relevante code moet u het sjabloonbestand met de parameterbestand implementeren.

![](./media/app-service-deploy-complex-application-predictably/deploy-12-powershellsnippet.png)

De laatste cmdlet, `New-AzureResourceGroup`, die de actie uitvoert. Dit alles moet worden aangetoond dat aan u, met behulp van gereedschappen is betrekkelijk eenvoudig voor de implementatie van de wolk toepassing voorspelbaar. Telkens wanneer u de cmdlet op dezelfde sjabloon met hetzelfde parameterbestand uitvoert, gaat u krijgt hetzelfde resultaat.

## <a name="summary"></a>Samenvatting ##

Zijn de sleutels tot een geslaagde implementatie van een grote schaal toepassing bestaat uit microservices in DevOps, herhaalbaarheid en voorspelbaarheid. In deze zelfstudie hebt u een twee-microservice toepassing Azure als één resourcegroep met behulp van de sjabloon Azure bronnenbeheerder geïmplementeerd. Hopelijk heeft dat u gegeven de kennis die u nodig hebt om te beginnen uw toepassing in Azure zetten in een sjabloon kunt inrichten en voorspelbare implementeren. 

## <a name="next-steps"></a>Volgende stappen ##

Informatie over het [flexibele methoden toepassen en publiceren voortdurend de toepassing microservices met gemak](app-service-agile-software-development.md) en implementatie van geavanceerde technieken zoals [flighting implementatie](app-service-web-test-in-production-controlled-test-flight.md) eenvoudig.

<a name="resources"></a>
## <a name="more-resources"></a>Meer bronnen ##

-   [Azure taal Resource Manager](../resource-group-authoring-templates.md)
-   [Azure Resource Manager sjablonen ontwerpen](../resource-group-authoring-templates.md)
-   [Functies van de sjabloon Azure Resource Manager](../resource-group-template-functions.md)
-   [Een toepassing met de sjabloon Azure Resource Manager](../resource-group-template-deploy.md)
-   [Met behulp van Azure PowerShell met Azure Resource Manager](../powershell-azure-resource-manager.md)
-   [Problemen met implementaties in Azure resourcegroep](../resource-manager-troubleshoot-deployments-portal.md)




 
