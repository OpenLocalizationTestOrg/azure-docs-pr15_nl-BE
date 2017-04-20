<properties
   pageTitle="Azure Resource groep Visual Studio-projecten | Microsoft Azure"
   description="Gebruik Visual Studio een project Azure resource groep maken en implementeren van de bronnen naar Azure."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn" />
<tags
   ms.service="azure-resource-manager"
   ms.devlang="multiple"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/20/2016"
   ms.author="tomfitz" />

# <a name="creating-and-deploying-azure-resource-groups-through-visual-studio"></a>Maken en implementeren van Azure resourcegroepen via Visual Studio

Met Visual Studio en de [Azure-SDK](https://azure.microsoft.com/downloads/)kunt u een project dat de infrastructuur en de code naar Azure implementeert. U kunt bijvoorbeeld de webhost, website en database voor uw app definiëren en implementeren van deze infrastructuur samen met de code. Of een virtuele Machine, Virtual Network en opslag Account definiëren en implementeren van die infrastructuur met een script dat wordt uitgevoerd op een virtuele Machine. Het project **Resourcegroep Azure** -implementatie kunt u alle benodigde resources in een enkele, herhaalbare bewerking implementeren. Zie [Bronbeheer Azure-overzicht](azure-resource-manager/resource-group-overview.md)voor meer informatie over de implementatie en het beheer van bronnen.

Resourcegroep Azure-projecten bevatten Azure Resource Manager JSON-sjablonen, waarin de bronnen die u op Azure implementeert. Meer informatie over de elementen van de sjabloon Resource Manager, Zie [sjablonen ontwerpen Azure Resource Manager](resource-group-authoring-templates.md). Visual Studio kunt u deze sjablonen bewerken en biedt hulpmiddelen waarmee u eenvoudiger werken met sjablonen.

In dit onderwerp implementeert u een web app en de SQL-Database. De stappen zijn echter vrijwel hetzelfde voor elk type resource. U kunt als eenvoudig implementeren een virtuele Machine en de bijbehorende bronnen. Visual Studio biedt vele sjablonen voor verschillende starter voor de implementatie van algemene scenario's.

Dit artikel bevat Visual Studio 2015 Update 2 en Microsoft Azure SDK voor .NET 2.9. Als u Visual Studio 2013 met Azure SDK 2.9, is uw ervaring grotendeels hetzelfde. U kunt versies van de SDK Azure van 2.6 of hoger; de ervaring van de gebruikersinterface mogelijk anders dan de gebruikersinterface die in dit artikel wordt weergegeven. Wij raden u de meest recente versie van de [Azure SDK](https://azure.microsoft.com/downloads/) installeren voordat u de stappen. 

## <a name="create-azure-resource-group-project"></a>Project Azure resourcegroep maken

In deze procedure maakt u een resourcegroep Azure project met een **Web app + SQL** -sjabloon.

1. In Visual Studio kiest **bestand**, **Nieuw Project**, **C#** of **Visual Basic**. Kies een **wolk**en kies vervolgens de **Resourcegroep Azure** project.

    ![Wolk implementatieproject](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/create-project.png)

1. Kies de sjabloon die u wilt implementeren op Azure Resource Manager. Zoals u ziet, er zijn tal van opties op basis van het type project dat u wilt implementeren. Voor dit onderwerp, het **Web app + SQL** -sjabloon te kiezen.

    ![Een sjabloon kiezen](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/select-project.png)

    De sjabloon die u kiest is slechts een begin; u kunt toevoegen en verwijderen van middelen om te voldoen aan uw scenario.

    >[AZURE.NOTE] Visual Studio haalt een lijst met beschikbare sjablonen online. De lijst kan worden gewijzigd.

    Visual Studio maakt een resource groep implementatieproject voor het web app en de SQL-database.

1. Als u wilt zien wat u hebt gemaakt, vouw de knooppunten in het implementatieproject.

    ![knooppunten weergeven](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-items.png)

    Aangezien we de Web app + SQL-sjabloon voor dit voorbeeld kiest, ziet u de volgende bestanden: 

  	|Bestandsnaam|Beschrijving|
  	|---|---|
  	|Implementatie van AzureResourceGroup.ps1|Een PowerShell script PowerShell-opdrachten aanroept voor de implementatie van Azure Resource Manager.<br />**Opmerking** Visual Studio gebruikt deze PowerShell-script voor de implementatie van uw sjabloon. Wijzigingen die u aanbrengt in dit script implementeren in Visual Studio beïnvloeden, dus wees voorzichtig.|
  	|WebSiteSQLDatabase.json|De bronnenbeheerder sjabloon waarin de infrastructuur u wilt implementeren op Azure en de parameters die u kunt opgeven tijdens de implementatie. Ook Hiermee definieert u de afhankelijkheden tussen de bronnen zodat Resource Manager implementeert u de resources in de juiste volgorde.|
  	|WebSiteSQLDatabase.parameters.json|Een parameterbestand met waarden die nodig zijn voor de sjabloon. U doorgeven parameterwaarden voor het aanpassen van elke implementatie.|

    Alle resource-groep implementatieprojecten bevatten basic-bestanden. Andere projecten bevat mogelijk extra bestanden voor de ondersteuning van andere functies.

## <a name="customize-the-resource-manager-template"></a>De bronnenbeheerder sjabloon aanpassen

U kunt een implementatieproject door aanpassing van de JSON-sjablonen die beschrijven de resources die u wilt implementeren. JSON staat voor JavaScript Object Notation, en wordt een geserialiseerde indeling die gemakkelijk is te werken. Een schema dat u aan het begin van elk bestand dat verwijst naar de JSON-bestanden gebruiken Als u weten over het schema wilt, kunt u downloaden en analyseren. In het schema gedefinieerd welke elementen zijn geldig, de typen en indelingen van velden, mogelijke waarden van de geïnventariseerde waarden, enzovoort. Meer informatie over de elementen van de sjabloon Resource Manager, Zie [sjablonen ontwerpen Azure Resource Manager](resource-group-authoring-templates.md).

**WebSiteSQLDatabase.json**u op uw sjabloon te openen.

De editor van Visual Studio biedt hulpmiddelen om u te helpen met het bewerken van de sjabloon Resource Manager. **JSON** het overzichtsvenster kunt u gemakkelijk voor een overzicht van de elementen die zijn gedefinieerd in de sjabloon.

![JSON-overzicht weergeven](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-json-outline.png)

Een van de elementen te selecteren in het overzicht, gaat u naar dat deel van de sjabloon en markeert de bijbehorende JSON.

![JSON navigeren](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/navigate-json.png)

U kunt een resource toevoegen door op de knop **Resource toevoegen** aan de bovenkant van het venster Overzicht JSON of **resources** met de rechtermuisknop en selecteert u **Add New Resource**.

![resource toevoegen](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-resource.png)

Voor deze zelfstudie **Opslag Account** selecteren en een naam geven. Geef een naam die niet meer dan 11 tekens bestaan en bevat alleen cijfers en kleine letters.

![opslag toevoegen](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-storage.png)

Merk op dat niet alleen is de bron toegevoegd, maar ook een parameter voor de account type opslag en een variabele voor de naam van de account van de opslag.

![overzicht weergeven](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-new-items.png)

De parameter **storageType** is vooraf gedefinieerde met toegestane typen en een standaardtype. U kunt deze waarden laten of ze bewerken voor uw scenario. Als u niet dat iedereen een account **Premium_LRS** opslag via deze sjabloon implementeren wilt, deze verwijderen uit de toegestane typen. 

    "storageType": {
      "type": "string",
      "defaultValue": "Standard_LRS",
      "allowedValues": [
        "Standard_LRS",
        "Standard_ZRS",
        "Standard_GRS",
        "Standard_RAGRS"
      ]
    }

Visual Studio biedt ook intellisense laat zien welke eigenschappen zijn beschikbaar bij het bewerken van de sjabloon. Bijvoorbeeld om de eigenschappen voor uw serviceplan App, Ga naar de resource **HostingPlan** en voegt u een waarde voor de **Eigenschappen**. U ziet dat intellisense ziet u de beschikbare waarden en bevat een beschrijving van die waarde.

![intellisense weergeven](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-intellisense.png)

U kunt **numberOfWorkers** instellen op 1.

    "properties": {
      "name": "[parameters('hostingPlanName')]",
      "numberOfWorkers": 1
    }

## <a name="deploy-the-resource-group-project-to-azure"></a>Het project resourcegroep naar Azure implementeren

U bent nu gereed voor de implementatie van uw project. Wanneer u een resourcegroep Azure project implementeert, kunt u deze implementeren aan een resourcegroep Azure. De resourcegroep is een logische groepering van de resources die de levensduur van een gemeenschappelijke delen.

1. Kies in het snelmenu van het knooppunt voor implementatie, **implementeren** > **Nieuwe distributie**.

    ![Implementeren, implementatie van nieuwe menu-item](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/deploy.png)

    Het dialoogvenster **Deploy resourcegroep** wordt weergegeven.

    ![Dialoogvenster resourcegroep implementeren](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-deployment.png)

1. Kies een bestaande resourcegroep in de vervolgkeuzelijst **resourcegroep** of maak een nieuwe. Een resourcegroep maken, het dropdown **Resourcegroep** openen en kies **Nieuwe maken**.

    ![Dialoogvenster resourcegroep implementeren](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/create-new-group.png)

    Het dialoogvenster **Resourcegroep maken** wordt weergegeven. Geef de groep een naam en locatie en klik op de knop **maken** .

    ![Dialoogvenster resourcegroep maken](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/create-resource-group.png)
   
1. Bewerk de parameters voor de implementatie door de knop **Parameters bewerken** te selecteren.

    ![Knop Parameters bewerken](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/edit-parameters.png)

1. Waarden opgeven voor de parameters leeg en klik op de knop **Opslaan** . De lege parameters zijn **hostingPlanName**, **administratorLogin**, **administratorLoginPassword**en **databasenaam**.

    **hostingPlanName** geeft een naam voor de [App serviceplan](./app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) te maken. 
    
    **administratorLogin** geeft u de gebruikersnaam voor de SQL Server-beheerder. Gebruik geen algemene admin namen als **sa** of **beheerder**. 
    
    De **administratorLoginPassword** geeft een wachtwoord voor de SQL Server-beheerder. De optie **voor het opslaan van wachtwoorden als tekst zonder opmaak in het parameterbestand** is niet veilig; daarom deze optie niet selecteren. Aangezien het wachtwoord niet als tekst zonder opmaak opgeslagen wordt, moet u dit wachtwoord opgeven tijdens de installatie opnieuw. 
    
    **databasenaam** bevat een naam voor de database te maken. 

    ![Het dialoogvenster Parameters bewerken](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/provide-parameters.png)
    
1. Kies de knop **implementeren** voor de implementatie van het project naar Azure. Hiermee opent u een console PowerShell buiten het exemplaar van Visual Studio. Geef het administrator-wachtwoord voor SQL Server in de console PowerShell wanneer daarom wordt gevraagd. **De PowerShell-console kan worden verborgen achter andere objecten of geminimaliseerd in de taakbalk.** Kijk voor deze console en selecteer het het wachtwoord op te geven.

    >[AZURE.NOTE] Visual Studio gevraagd de Azure PowerShell cmdlets installeert. U moet de Azure PowerShell-cmdlets voor het installeren van resourcegroepen. Als u wordt gevraagd, installeert u deze.
    
1. De installatie kan enkele minuten duren. In de windows **Output** ziet u de status van de implementatie. Als de installatie is voltooid, is het laatste bericht geeft aan dat een succesvolle implementatie met een vergelijkbare:

        ... 
        18:00:58 - Successfully deployed template 'c:\users\user\documents\visual studio 2015\projects\azureresourcegroup1\azureresourcegroup1\templates\websitesqldatabase.json' to resource group 'DemoSiteGroup'.


1. In een browser, open de [Azure portal](https://portal.azure.com/) en log op uw account. Overzicht van de resourcegroep, **resourcegroepen** en de resourcegroep die u hebt geïmplementeerd op te selecteren.

    ![Selecteer groep](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/select-group.png)

1. U ziet de gebruikte bronnen. U ziet dat de naam van de account van de opslag niet precies wat u hebt opgegeven is bij het toevoegen van die bron. De opslag-account moet uniek zijn. De sjabloon wordt automatisch een reeks tekens toegevoegd aan de naam die u een unieke naam opgeven. 

    ![resources weergeven](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-deployed-resources.png)

1. Als u wijzigingen aanbrengt en wilt distribueren van uw project, kiest u in het snelmenu van het project Azure resource-groep de bestaande brongroep. **Deploy**kiezen in het snelmenu en kies vervolgens de resourcegroep die u hebt geïmplementeerd.

    ![Azure resourcegroep geïmplementeerd](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/redeploy.png)

## <a name="deploy-code-with-your-infrastructure"></a>De code in uw infrastructuur implementeren

Nu u de infrastructuur voor uw app hebt geïmplementeerd, maar er is geen werkelijke code geïmplementeerd met het project. Dit onderwerp bevat het implementeren van een web app en tabellen van de SQL-Database tijdens de implementatie. Als u een virtuele Machine in plaats van een web app implementeert, wilt u bepaalde programmacode uitvoeren op de computer als onderdeel van de implementatie van. Het proces voor de implementatie van code voor een web app of voor het instellen van een virtuele Machine is bijna hetzelfde.

1. Een project met de Visual Studio-oplossing toevoegen. De oplossing met de rechtermuisknop en selecteer **toevoegen** > **Nieuw Project**.

    ![project toevoegen](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-project.png)

1. Een **ASP.NET-webtoepassing**toevoegen. 

    ![toevoegen web app.](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-app.png)
    
1. **MVC** en schakel het veld voor een **Host in de cloud** omdat het project van de groep resources die taak wordt uitgevoerd.

    ![Selecteer MVC](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/select-mvc.png)
    
1. Als uw web app, Visual Studio maakt, ziet u beide projecten in de oplossing.

    ![projecten weergeven](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-projects.png)

1. Nu, moet u controleren of dat uw project resource group is op de hoogte van het nieuwe project. Ga terug naar de project resource groep (AzureResourceGroup1). Klik met de rechtermuisknop op **References** en selecteer **Toevoegen**.

    ![verwijzing toevoegen](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-new-reference.png)

1. Selecteer het project dat web app die u hebt gemaakt.

    ![verwijzing toevoegen](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-reference.png)
    
    Het project web app koppelen aan het project resource groep door een verwijzing toe te voegen en drie belangrijke eigenschappen automatisch ingesteld. Ziet u deze eigenschappen in het venster **Eigenschappen** voor de verwijzing.

      ![Zie Naslaginformatie](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/see-reference.png)
    
    De eigenschappen zijn:

    - De **Extra eigenschappen** van bevat het webpakket implementatie tijdelijke locatie die wordt geduwd aan de opslag van Azure. Opmerking de map (ExampleApp) en het bestand (package.zip). U krijgt deze waarden als parameters bij het implementeren van de app. 
    - Het **Pad naar het bestand opnemen** bevat het pad waar het pakket is gemaakt. De **Doelen zijn** bevat de opdracht die de implementatie wordt uitgevoerd. 
    - De standaardwaarde van **bouwen; Pakket** kunt u de implementatie te maken van een webpakket implementatie (package.zip).  
    
    U hoeft niet een publicatieprofiel als de implementatie de benodigde gegevens uit de eigenschappen haalt voor het maken van het pakket.
      
1. Een resource toevoegen aan de sjabloon.

    ![resource toevoegen](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-resource-2.png)

1. Deze tijd Selecteer **Webpagina voor Web Apps implementeren**. 

    ![toevoegen web implementeren](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-web-deploy.png)
    
1. Implementeer uw project resource groep aan de resourcegroep. Momenteel zijn er enkele nieuwe parameters. U hoeft geen waarden op te geven voor **_artifactsLocation** of **_artifactsLocationSasToken** omdat deze waarden worden automatisch gegenereerd door Visual Studio. U moet echter de map en de bestandsnaam ingesteld op het pad met het implementatiepakket (weergegeven als **ExampleAppPackageFolder** en **ExampleAppPackageFileName** in de volgende afbeelding). Bevatten de waarden die u eerder in het verwijzen naar eigenschappen (**ExampleApp** en **package.zip**) gezien.

    ![toevoegen web implementeren](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/set-new-parameters.png)
    
    Selecteer voor de **rekening van artefact opslag**, die met deze resourcegroep.
    
1. Nadat de installatie is voltooid, selecteert u uw web app in de portal. Selecteer de URL om naar de site te bladeren.

    ![site te bezoeken](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/browse-site.png)

1. U ziet dat u de standaard ASP.NET-toepassing met succes hebt geïmplementeerd.

    ![gedistribueerde toepassing weergeven](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-deployed-app.png)

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het beheer van bronnen via de portal, Zie [de Azure portal voor het beheren van uw resources Azure](./azure-portal/resource-group-portal.md).
- Zie voor meer informatie over sjablonen, [sjablonen ontwerpen Azure Resource Manager](resource-group-authoring-templates.md).
