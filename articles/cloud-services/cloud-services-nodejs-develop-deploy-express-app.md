<properties 
    pageTitle="Web App met Express (Node.js) | Microsoft Azure" 
    description="Een zelfstudie die is gebaseerd op de service cloud zelfstudie en laat zien hoe u de module Express." 
    services="cloud-services" 
    documentationCenter="nodejs" 
    authors="rmcmurray" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="cloud-services" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="na" 
    ms.devlang="nodejs" 
    ms.topic="article" 
    ms.date="08/11/2016" 
    ms.author="robmcm"/>






# <a name="build-a-nodejs-web-application-using-express-on-an-azure-cloud-service"></a>Bouwen van een webtoepassing voor Node.js met Express op een Azure Cloud-Service

Node.js bevat een minimale set van functies in de kern-runtime.
Ontwikkelaars gebruiken vaak 3de partij modules bieden extra functionaliteit bij het ontwikkelen van een toepassing Node.js. In deze handleiding maakt u een nieuwe toepassing met behulp van de module [Express][] een MVC kader biedt voor het maken van webtoepassingen Node.js.

Een schermafbeelding van de voltooide toepassing lager is dan:

![Een webbrowser weergeven Express in Azure, Welkom](./media/cloud-services-nodejs-develop-deploy-express-app/node36.png)

##<a name="create-a-cloud-service-project"></a>Maak een Cloud Service Project

Voer de volgende stappen uit om een nieuwe wolk service project met de naam 'expressapp' te maken:

1. Zoeken in het **Menu Start** of het **Startscherm van** **Windows PowerShell**. Ten slotte met de rechtermuisknop op **Windows PowerShell** en selecteer **Als Administrator uitvoeren**.

    ![Azure PowerShell-pictogram](./media/cloud-services-nodejs-develop-deploy-express-app/azure-powershell-start.png)

    [AZURE.INCLUDE [install-dev-tools](../../includes/install-dev-tools.md)]

2. Eerst naar de **c:\\knooppunt** directory en voer vervolgens de volgende opdrachten voor het maken van een nieuwe oplossing met de naam **expressapp** en de Webrol van een met de naam **WebRole1**:

        PS C:\node> New-AzureServiceProject expressapp
        PS C:\Node\expressapp> Add-AzureNodeWebRole
        PS C:\Node\expressapp> Set-AzureServiceProjectRole WebRole1 Node 0.10.21

    > [AZURE.NOTE] **Toevoegen-AzureNodeWebRole** standaard in een oudere versie van Node.js. De instructie **Set AzureServiceProjectRole** hiervoor opdracht Azure v0.10.21 van knooppunt gebruikt.  Opmerking dat de parameters zijn hoofdlettergevoelig.  U kunt controleren of dat de juiste versie van Node.js is geselecteerd door de eigenschap **motoren** in **WebRole1\package.json**te controleren.

##<a name="install-express"></a>Express installeren

1. De generator Express installeren door de volgende opdracht:

        PS C:\node\expressapp> npm install express-generator -g

    De uitvoer van de opdracht npm moet uitzien het resultaat hieronder. 

    ![Windows PowerShell voor het weergeven van de uitvoer van de npm installeren uitdrukkelijke opdracht.](./media/cloud-services-nodejs-develop-deploy-express-app/express-g.png)

2. Mappen wijzigen in de map **WebRole1** en de uitdrukkelijke opdracht voor het genereren van een nieuwe toepassing:

        PS C:\node\expressapp\WebRole1> express

    U wordt gevraagd uw eerdere toepassing overschrijven. Typ **j** of **Ja** om door te gaan. Express, genereert het bestand app.js en een mappenstructuur voor het bouwen van uw toepassing.

    ![De uitvoer van de opdracht uitdrukkelijk](./media/cloud-services-nodejs-develop-deploy-express-app/node23.png)


5.  Extra afhankelijkheden die zijn gedefinieerd in het bestand package.json installeren, voer de volgende opdracht:

        PS C:\node\expressapp\WebRole1> npm install

    ![De uitvoer van de npm opdracht install](./media/cloud-services-nodejs-develop-deploy-express-app/node26.png)

6.  Gebruik de volgende opdracht als u de **opslaglocatie/www** -bestand te kopiëren naar **server.js**. Dit is de service cloud vindt het ingangspunt voor deze toepassing.

        PS C:\node\expressapp\WebRole1> copy bin/www server.js

    Nadat deze opdracht is voltooid, hebt u een **server.js** -bestand in de map WebRole1.

7.  Wijzig de **server.js** als u wilt verwijderen op een van de '.' tekens uit de volgende regel.

        var app = require('../app');

    Na deze wijziging de regel moet worden weergegeven als volgt.

        var app = require('./app');

    Deze wijziging is vereist omdat we (voorheen **bin/www**,) van het bestand verplaatst naar dezelfde map als het app-bestand vereist is. Na het aanbrengen van deze wijziging, slaat u het bestand **server.js** .

8.  Gebruik de volgende opdracht als u de toepassing wilt uitvoeren in de Azure-emulator:

        PS C:\node\expressapp\WebRole1> Start-AzureEmulator -launch

    ![Een webpagina met Welkom bij express.](./media/cloud-services-nodejs-develop-deploy-express-app/node28.png)

## <a name="modifying-the-view"></a>De weergave wijzigen

Nu wilt weergeven van het bericht 'Welkom naar Express in Azure' wijzigen.

1.  Voer de volgende opdracht in het bestand index.jade te openen:

        PS C:\node\expressapp\WebRole1> notepad views/index.jade

    ![De inhoud van het bestand index.jade.](./media/cloud-services-nodejs-develop-deploy-express-app/getting-started-19.png)

    Jade is de standaard weergave-engine die wordt gebruikt door toepassingen Express. Zie [http://jade-lang.com][]voor meer informatie op de motor Jade weergeven.

2.  De laatste regel van de tekst wijzigen door **in Azure**toe te voegen.

    ![Het bestand index.jade de laatste regel leest: p-Welkom bij \#{titel} in Azure](./media/cloud-services-nodejs-develop-deploy-express-app/node31.png)

3.  Sla het bestand op en sluit Kladblok af.

4.  Vernieuw uw browser en u ziet uw wijzigingen.

    ![Een browservenster bevat de pagina Welkom bij Express in Azure](./media/cloud-services-nodejs-develop-deploy-express-app/node32.png)

Gebruik de cmdlet **Stop AzureEmulator** de emulator stoppen na het testen van de toepassing.

##<a name="publishing-the-application-to-azure"></a>De toepassing op Azure publiceren

In het venster Azure PowerShell gebruikt u de cmdlet **Publiceren AzureServiceProject** voor de implementatie van de toepassing naar een cloud-service

    PS C:\node\expressapp\WebRole1> Publish-AzureServiceProject -ServiceName myexpressapp -Location "East US" -Launch

Nadat de bewerking voor de implementatie is voltooid, wordt uw browser openen en wordt de webpagina weergegeven.

![Een webbrowser de pagina Express weergeven. De URL geeft aan dat het nu wordt gehost op Azure.](./media/cloud-services-nodejs-develop-deploy-express-app/node36.png)

## <a name="next-steps"></a>Volgende stappen

Zie [Node.js Developer Center](/develop/nodejs/)voor meer informatie.

  [Node.js Web Application]: http://www.windowsazure.com/develop/nodejs/tutorials/getting-started/
  [Express]: http://expressjs.com/
  [http://Jade-lang.com]: http://jade-lang.com

 
