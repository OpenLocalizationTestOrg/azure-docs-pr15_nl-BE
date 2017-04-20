<properties
   pageTitle="VS-Code gebruiken met sjablonen Resource Manager | Microsoft Azure"
   description="Laat zien hoe Visual Studio-Code instellen om Azure Resource Manager-sjablonen te maken."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="cmatskas"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/26/2016"
   ms.author="chmatsk;tomfitz"/>

# <a name="working-with-azure-resource-manager-templates-in-visual-studio-code"></a>Werken met sjablonen voor Azure Resource Manager in Visual Studio Code

Azure Resource Manager-sjablonen zijn JSON bestanden waarin een resource en de bijbehorende afhankelijkheden. Deze bestanden kunnen soms vrij groot zijn en dus belangrijk gereedschap ondersteuning is ingewikkeld. Visual Studio Code is een nieuw, lichtgewicht, open source, cross-platform code-editor. Het ondersteunt maken en bewerken van sjablonen met een [nieuwe extensie](https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools)Resource Manager. VS Code overal wordt uitgevoerd en toegang tot het Internet niet nodig, tenzij u ook wilt implementeren uw templates Resource Manager.

Als u nog geen Code van de VS, kunt u deze installeren op [https://code.visualstudio.com/](https://code.visualstudio.com/).

## <a name="install-the-resource-manager-extension"></a>Uitbreiding voor de Resource Manager installeren

Met de JSON-sjablonen in de VS Code wilt werken, moet u een extensie te installeren. De volgende stappen downloaden en installeren van de ondersteuning voor bronbeheer JSON-sjablonen:

1. VS Code starten 
2. Open snel openen (Ctrl + P) 
3. Voer de volgende opdracht: 

        ext install azurerm-vscode-tools

4. VS-Code wanneer u wordt gevraagd om de extensie opnieuw te starten. 

 Werk gedaan!

## <a name="set-up-resource-manager-snippets"></a>Resource Manager-codefragmenten instellen

De vorige stappen de tooling ondersteuning geïnstalleerd, maar nu moeten we VS Code voor het gebruik van fragmenten van JSON sjabloon configureren.

1. De inhoud van het bestand in de opslagplaats [azure-xplat-arm tooling](https://raw.githubusercontent.com/Azure/azure-xplat-arm-tooling/master/VSCode/armsnippets.json) naar het Klembord kopiëren.
2. VS Code starten 
3. In VS Code, kunt u het bestand JSON-fragmenten openen door beide navigeren naar een **bestand** -> **Voorkeuren** -> **Gebruiker fragmenten** -> **JSON**, of door **F1** te selecteren en te typen **Voorkeuren** totdat kunt u **Voorkeuren: fragmenten**.

    ![voorkeur fragmenten](./media/resource-manager-vs-code/preferences-snippets.png)

    Selecteer de opties voor **JSON**.

    ![Selecteer json](./media/resource-manager-vs-code/select-json.png)

4. Plakt u de inhoud van het bestand in stap 1 in uw bestand fragmenten voordat het uiteindelijke "}" 
5. Controleer of de JSON ziet er OK en er zijn geen tijdens overal het typen. 
6. Opslaan en sluiten van het bestand fragmenten.

Dat is alles dat nodig is om te starten met behulp van de fragmenten Resource Manager. Vervolgens plaatst we deze instelling aan de test.

## <a name="work-with-template-in-vs-code"></a>Werken met de sjabloon in de Code van de VS

De eenvoudigste manier om te werken met een sjabloon is een van de snelle Start sjablonen beschikbaar op [Github](https://github.com/Azure/azure-quickstart-templates) pak of een eigen. U kunt eenvoudig [exporteren van een sjabloon](resource-manager-export-template.md) voor de bronnengroepen via de portal. 

1. Als u een sjabloon uit een resourcegroep hebt geëxporteerd, opent u de uitgepakte bestanden in de Code van de VS.

    ![bestanden weergeven](./media/resource-manager-vs-code/show-files.png)

2. Open het bestand template.json dat u kunt bewerken en toevoegen van enkele extra bronnen. Na het **'bronnen': [** druk op enter om een nieuwe regel beginnen. Als u de **arm**typt, zult u worden gepresenteerd met een lijst met opties. Deze opties zijn de sjabloon fragmenten die u hebt geïnstalleerd. Het ziet er zo uit: 

    ![codefragmenten weergeven](./media/resource-manager-vs-code/type-snippets.png)

3. Kies het gewenste fragment. Voor dit artikel, **arm-ip** voor het maken van een nieuwe openbare IP-adres moet opgeven. Plaats een komma na de vierkante haak sluiten "}" van de zojuist gemaakte bron om te controleren of uw sjabloon syntaxis is ongeldig.

     ![punt toevoegen](./media/resource-manager-vs-code/add-comma.png)

4. Code van de VS heeft ingebouwde IntelliSense. Als u uw sjablonen bewerken, stelt VS Code voor beschikbare waarden. Bijvoorbeeld, u een sectie variabelen toevoegen aan uw sjabloon toevoegen **""** (twee dubbele aanhalingstekens) en selecteert u **Ctrl + spatie** tussen de aanhalingstekens. U krijgt opties met inbegrip van **variabelen**.

    ![variabelen toevoegen](./media/resource-manager-vs-code/add-variables.png)

5. IntelliSense biedt ook beschikbare waarden of functies. Als u een eigenschap instelt op een waarde voor de parameter, een expressie te maken met **'[]'** en **Ctrl + spatiebalk**. U kunt beginnen met het typen van de naam van een functie. Klik op **Tab** als u de gewenste functie hebt gevonden.

    ![parameter toevoegen](./media/resource-manager-vs-code/select-parameters.png)

6. Opnieuw **Ctrl + spatiebalk** selecteert in de functie voor een overzicht van de beschikbare parameters in de sjabloon.

    ![parameter toevoegen](./media/resource-manager-vs-code/select-avail-parameters.png)

7. Als u alle validatieproblemen schema in de sjabloon, ziet u de vertrouwde tijdens het typen in de editor. U kunt de lijst met fouten en waarschuwingen weergeven door **Ctrl + Shift + M** te typen of selecteren de glyphs in de onderste statusbalk van links.

    ![fouten](./media/resource-manager-vs-code/errors.png)

    Validatie van de sjabloon, kunt u syntaxis opsporen; u kunt echter ook fouten die u kunt negeren zien. In sommige gevallen is de editor vergelijken met uw sjabloon tegen een schema die niet up-to-date is en dus een fout rapporteert, hoewel u weet dat deze juist is. Stel bijvoorbeeld dat een functie is onlangs toegevoegd aan de Resource Manager, maar het schema is niet bijgewerkt. De editor meldt een fout, ondanks het feit dat de functie goed tijdens de implementatie werkt.

    ![foutbericht](./media/resource-manager-vs-code/unrecognized-function.png)

## <a name="deploy-your-new-resources"></a>Implementeren van nieuwe bronnen

Wanneer de sjabloon klaar is, kunt u de nieuwe resources de onderstaande instructies te implementeren: 

### <a name="windows"></a>Windows

1. Open een opdrachtprompt PowerShell 
2. Login-type: 

        Login-AzureRmAccount 

3. Als er meerdere abonnementen, krijgt u een overzicht van de abonnementen met:

        Get-AzureRmSubscription

    En selecteer het abonnement te gebruiken.
   
        Select-AzureRmSubscription -SubscriptionId <Subscription Id>

4. De parameters in het bestand parameters.json bijwerken
5. De Deploy.ps1 voor de implementatie van uw sjabloon op Azure uitvoeren

### <a name="osxlinux"></a>OSX/Linux

1. Een terminal-venster openen 
2. Login-type:

        azure login 

3. Als er meerdere abonnementen, selecteer het juiste abonnement:

        azure account set <subscriptionNameOrId> 

4. De parameters in het bestand parameters.json bijwerken.
5. Voor de implementatie van de sjabloon worden uitgevoerd:

        azure group deployment create -f <PathToTemplate> 

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over sjablonen, [sjablonen ontwerpen Azure Resource Manager](resource-group-authoring-templates.md).
- Meer informatie over de functies van de sjabloon, ziet [de bronnenbeheerder Azure sjabloon functies](resource-group-template-functions.md).
- Zie voor meer voorbeelden van het werken met Visual Studio Code [cloud-apps bouwen met Visual Studio-Code](https://github.com/Microsoft/HealthClinic.biz/wiki/Build-cloud-apps-with-Visual-Studio-Code) van de [HealthClinic.biz](https://github.com/Microsoft/HealthClinic.biz) 2015 Connect [demo](https://blogs.msdn.microsoft.com/visualstudio/2015/12/08/connectdemos-2015-healthclinic-biz/). Zie voor meer QuickStart uit de HealthClinic.biz-demo [Azure Developer Tools QuickStart](https://github.com/Microsoft/HealthClinic.biz/wiki/Azure-Developer-Tools-Quickstarts).
