<properties 
    pageTitle="Bouwen van de logica Apps in Visual Studio | Microsoft Azure" 
    description="Een project maken in Visual Studio maken en implementeren van uw app logica." 
    authors="jeffhollan" 
    manager="erikre" 
    editor="" 
    services="logic-apps" 
    documentationCenter=""/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="jehollan"/> 
    
# <a name="build-and-deploy-logic-apps-in-visual-studio"></a>Bouw en implementeer logica Apps in Visual Studio

Hoewel de [Azure Portal](https://portal.azure.com/) u een uitstekende manier om te ontwerpen en beheren van uw apps logica biedt, kunt u ook ontwerpen en implementeren van uw app logica van Visual Studio in plaats daarvan.  Logica Apps komt configureren met een rijke Visual Studio set hulpmiddelen waarmee u kunt bouwen een logica app met designer, implementatie en automatisering sjablonen en implementeren in elke omgeving.  

## <a name="installation-steps"></a>Installatiestappen

Hieronder volgen de stappen voor het installeren en configureren van de Visual Studio tools voor Apps logica.

### <a name="prerequisites"></a>Vereisten

- [Visual Studio 2015](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx)
- [Laatste Azure SDK](https://azure.microsoft.com/downloads/) (2.9.1 of hoger)
- [Azure PowerShell] (https://github.com/Azure/azure-powershell#installation)
- Toegang tot het web wanneer u de ingesloten ontwerpfunctie

### <a name="install-visual-studio-tools-for-logic-apps"></a>Visual Studio tools voor logica Apps installeren

Zodra u de vereisten hebt geïnstalleerd, heb 

1. Open Visual Studio 2015 aan het menu **Extra** en selecteer **uitbreidingen en Updates**
1. Selecteer de categorie **Online** online zoeken
1. Zoeken naar **Logica Apps** weer te geven van de **Hulpprogramma's voor Apps Azure logica voor Visual Studio**
1. Klik op de knop **downloaden** om te downloaden en installeren van de uitbreiding
1. Visual Studio starten na de installatie

> [AZURE.NOTE] U kunt ook de extensie rechtstreeks vanaf [deze koppeling](https://visualstudiogallery.msdn.microsoft.com/e25ad307-46cf-412e-8ba5-5b555d53d2d9) downloaden

Eenmaal geïnstalleerd, is het mogelijk de resourcegroep Azure project met de logica App Designer gebruiken.

## <a name="create-a-project"></a>Een project maken

1. Ga naar het menu **bestand** en selecteer **Nieuw** >  **Project** (of u kunt gaan **toevoegen** en selecteer vervolgens een **Nieuw project** toe te voegen aan een bestaande oplossing):  ![menu bestand](./media/app-service-logic-deploy-from-vs/filemenu.png)

1. **Wolk**zoeken in het dialoogvenster en selecteer **De resourcegroep Azure**. Typ een **naam** en klik vervolgens op **OK**.
    ![Nieuw project toevoegen](./media/app-service-logic-deploy-from-vs/addnewproject.png)

1. Selecteer de sjabloon **app logica** . Hiermee maakt u een sjabloon voor lege logica app implementatie beginnen.
    ![Azure sjabloon selecteren](./media/app-service-logic-deploy-from-vs/selectazuretemplate.png)

1. Wanneer u de **sjabloon**hebt geselecteerd, klik op **OK**.

    Uw project logica app wordt nu toegevoegd aan uw oplossing. U ziet de implementatie-bestand in de Solution Explorer:  

    ![Implementatie](./media/app-service-logic-deploy-from-vs/deployment.png)

## <a name="using-the-logic-app-designer"></a>Met behulp van de logica App Designer

Wanneer u een resourcegroep Azure project met een app logica hebt, kunt u de ontwerpfunctie binnen Visual Studio om u te helpen bij het maken van de werkstroom openen.  De ontwerper een internetverbinding nodig om de aansluitingen voor de beschikbare eigenschappen en gegevens opvragen (bijvoorbeeld als de Dynamics CRM Online-connector, de ontwerper vraagt uw exemplaar van CRM om een lijst van beschikbare aangepaste en de standaardeigenschappen).

1. Klik met de rechtermuisknop op de `<template>.json` -bestand en selecteer **openen met logica App Designer** (of `Ctrl+L`)
1. Kies het abonnement, resourcegroep en locatie op voor de sjabloon voor de implementatie
    - Het is belangrijk te weten dat het ontwerpen van een app logica **API verbinding** resources om te zoeken op Eigenschappen tijdens het ontwerpen maakt.  De geselecteerde resourcegroep worden de resourcegroep die wordt gebruikt voor het maken van deze verbindingen tijdens ontwerptijd.  U kunt bekijken of wijzigen van een API-verbindingen door te gaan naar de Portal Azure en zoeken naar **Verbindingen API**.
    ![Abonnement kiezen](./media/app-service-logic-deploy-from-vs/designer_picker.png)
1. De ontwerper moet worden weergegeven op basis van de definitie in de `<template>.json` bestand.
1. Nu maken en ontwerpen van uw app logica en wijzigingen in de sjabloon voor de implementatie worden bijgewerkt.
    ![Designer in Visual Studio](./media/app-service-logic-deploy-from-vs/designer_in_vs.png)

Ziet u ook `Microsoft.Web/connections` resources worden toegevoegd aan het bronbestand voor alle verbindingen die nodig zijn voor de toepassing van de logica van functie.  Eigenschappen voor deze verbinding kunnen instellen wanneer u implementeren en nadat u hebt geïmplementeerd in **API-verbindingen** in de Portal Azure beheerd.

### <a name="switching-to-the-json-code-view"></a>Overschakelen naar de codeweergave JSON

U kunt het tabblad **Codeweergave** aan de onderkant van de ontwerper om te schakelen naar de JSON representatie van de logica app selecteren.  Als u wilt overschakelen naar de volledige bron JSON, met de rechtermuisknop op de `<template>.json` -bestand en selecteer **openen**.

### <a name="saving-the-logic-app"></a>Opslaan van de logica-app

U kunt de app logica op elk gewenst moment via de knop **Opslaan** opslaan of `Ctrl+S`.  Als er fouten met uw app logica op het moment dat u opslaat, wordt deze weergegeven in het venster **uitvoer** van Visual Studio.

## <a name="deploying-your-logic-app"></a>Uw app logica te implementeren

Ten slotte, nadat u uw app hebt geconfigureerd, kunt u implementeren rechtstreeks vanuit Visual Studio in een paar stappen. 

1. Klik met de rechtermuisknop op het project in de Solution Explorer en **distribueren**naar > **Nieuwe distributie...** 
     ![Nieuwe distributie](./media/app-service-logic-deploy-from-vs/newdeployment.png)

2. Moet u zich aanmelden bij uw abonnement(en) Azure. 

3. Nu moet u de details van de resourcegroep die u wilt implementeren de app logica om te kiezen. 
    ![Resourcegroep implementeren](./media/app-service-logic-deploy-from-vs/deploytoresourcegroup.png)

     > [AZURE.NOTE]    Zorg ervoor dat de juiste sjabloon parameters bestanden en de resourcegroep (bijvoorbeeld als u naar een productieomgeving implementeert zult u kiest u het bestand met de productie) te selecteren. 
4. Klik op de knop implementeren
 
    
6. De status van de installatie wordt weergegeven in het venster **uitvoer** (wellicht moet u kiezen **Azure inrichten**. 
    ![Uitvoer](./media/app-service-logic-deploy-from-vs/output.png)

U kunt in de toekomst uw app logica in Bronbeheer herzien en Visual Studio gebruiken voor de implementatie van nieuwe versies. 

> [AZURE.NOTE] Als u de definitie in de Portal Azure rechtstreeks wijzigt, wordt de volgende keer dat u deze wijzigingen implementeren met behulp van Visual Studio overschreven.

## <a name="next-steps"></a>Volgende stappen

- Volg de zelfstudie voor het [maken van een App logica](app-service-logic-create-a-logic-app.md) om te beginnen met logica Apps.  
- [Veelvoorkomende scenario's en voorbeelden weergeven](app-service-logic-examples-and-scenarios.md)
- [U kunt automatiseren van bedrijfsprocessen met logica Apps](http://channel9.msdn.com/Events/Build/2016/T694) 
- [Leren hoe u uw systemen integreren met logica Apps](http://channel9.msdn.com/Events/Build/2016/P462)