<properties
    pageTitle="Aan de slag met Node.js web apps in Azure App-Service"
    description="Informatie over het implementeren van een toepassing Node.js naar een web app in Azure App-Service."
    services="app-service\web"
    documentationCenter="nodejs"
    authors="cephalin"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="nodejs"
    ms.topic="get-started-article"
    ms.date="07/01/2016"
    ms.author="cephalin"/>

# <a name="get-started-with-nodejs-web-apps-in-azure-app-service"></a>Aan de slag met Node.js web apps in Azure App-Service

[AZURE.INCLUDE [tabs](../../includes/app-service-web-get-started-nav-tabs.md)]

Deze zelfstudie laat zien hoe een eenvoudige [Node.js] toepassing maken en implementeren met [Azure App Service] vanaf een opdrachtregel omgeving, zoals cmd.exe of bash. De instructies in deze handleiding kunnen worden gevolgd op elk besturingssysteem dat Node.js kunt uitvoeren.

>[AZURE.INCLUDE [app-service-linux](../../includes/app-service-linux.md)] 

<a name="prereq"></a>
## <a name="prerequisites"></a>Vereisten

- [Node.js]
- [Bower]
- [Yeoman]
- [GIT]
- [Azure CLI]
- Een account van Microsoft Azure. Als u geen account hebt, kunt u [uw voordelen van Visual Studio abonnee activeren]of [registreren voor een gratis proefversie] .

## <a name="create-and-deploy-a-simple-nodejs-web-app"></a>Maken en implementeren van een eenvoudige Node.js web app.

1. Open de terminal opdrachtregelprogramma van uw keuze en de [generator voor Yeoman Express]installeren.

        npm install -g generator-express

2. `CD`naar een werkmap en het genereren van een express-app met de volgende syntaxis:

        yo express
        
    Kies de volgende opties wanneer u wordt gevraagd:  

    `? Would you like to create a new directory for your project?`**Ja**  
    `? Enter directory name`**{appname}**  
    `? Select a version to install:`**MVC**  
    `? Select a view engine to use:`**Jade**  
    `? Select a css preprocessor to use (Sass Requires Ruby):`**Geen**  
    `? Select a database to use:`**Geen**  
    `? Select a build tool to use:`**Grunt**

3. `CD`naar de hoofdmap van uw nieuwe app en om ervoor te zorgen dat deze wordt uitgevoerd in uw ontwikkelomgeving te starten:

        npm start

    Ga naar <http://localhost:3000> om ervoor te zorgen dat u de introductiepagina Express kunt zien in de browser. Zodra u uw app werkt goed hebt gecontroleerd, gebruikt u `Ctrl-C` te stoppen.
    
1. ASM-modus wijzigen en zich aanmelden bij Azure (u moet [Azure CLI](#prereq)):

        azure config mode asm
        azure login

    Volg de vraag om te gaan met de aanmelding in een browser met een Microsoft-account met uw abonnement Azure.

2. Zorg ervoor dat u nog steeds in de hoofdmap van uw app en maak vervolgens de resource App Service app in Azure met de naam van een unieke app met de volgende opdracht. Bijvoorbeeld: http://{appname}.azurewebsites.net

        azure site create --git {appname}

    Volg de prompt te implementeren op een Azure-regio selecteren. Als u hebt nooit Git/FTP-implementatie referenties voor uw abonnement op Azure, u ook gevraagd om deze te maken.

3. Open het bestand./config/config.js in de hoofdmap van de toepassing en de productie-poort wijzigen `process.env.port`; uw `production` eigenschap in het `config` object ziet er als volgt uit:

        production: {
            root: rootPath,
            app: {
                name: 'express1'
            },
            port: process.env.port,
        }

    Hiermee kunt uw Node.js app reageren als u wilt dat iisnode het web aanvragen op de standaardpoort luistert.
    
4. ./Package.json Open en voeg de `engines` eigenschap geeft u [de gewenste versie van Node.js](#version).

        "engines": {
            "node": "6.6.0"
        }, 

4. Sla uw wijzigingen vervolgens git gebruiken voor de implementatie van uw app naar Azure:

        git add .
        git commit -m "{your commit message}"
        git push azure master

    De generator Express biedt al een .gitignore bestand, dus de `git push` niet bandbreedte probeert te uploaden van de node_modules / Active directory.

5. Ten slotte uw live Azure app in de browser starten:

        azure site browse

    U ziet nu uw Node.js web app met live in Azure App-Service.
    
    ![Voorbeeld van de gedistribueerde toepassing bladeren.][deployed-express-app]

## <a name="update-your-nodejs-web-app"></a>Bijwerken van uw web app Node.js.

Als u updates naar uw Node.js web app in de App-Service wordt uitgevoerd, alleen uitvoeren `git add`, `git commit`, en `git push` als toen u uw web app voor het eerst geïmplementeerd.
     
## <a name="how-app-service-deploys-your-nodejs-app"></a>App-Service implementeert u hoe uw app Node.js

[Iisnode] Azure App-Service gebruikt voor het uitvoeren van Node.js apps. De CLI Azure en de Kudu-engine (Git deployment) werken samen zodat u een gestroomlijnde ervaring bij het ontwikkelen en implementeren van Node.js apps vanaf de opdrachtregel. 

- `azure site create --git`herkent de gemeenschappelijke Node.js patroon van server.js of app.js en een iisnode.yml gemaakt in de hoofdmap. U kunt dit bestand iisnode aanpassen.
- Op `git push azure master`, Kudu automatiseert de volgende implementatietaken:

    - Uitgevoerd als package.json in de hoofdmap van de opslagplaats, `npm install --production`.
    - Genereer een Web.config voor iisnode die naar het script start in package.json (bv. server.js of app.js verwijst).
    - Aanpassen van Web.config om uw app voor het debuggen van knooppunt Inspector klaar.
    
## <a name="use-a-nodejs-framework"></a>Een Node.js framework gebruiken

Als u een populaire Node.js framework, zoals [Sails.js] [ SAILSJS] of [MEAN.js] [ MEANJS] voor het ontwikkelen van toepassingen, kunt u die service App implementeren. Gebruikte frameworks voor Node.js hebben hun specifieke quirks en behouden hun pakketafhankelijkheden bijgewerkt. App-Service heeft echter de logboeken stdout en stderr beschikbaar, zodat u kunt weten precies wat er gebeurt met uw app en dienovereenkomstig wijzigen. Voor meer informatie Zie [stdout en stderr logs van iisnode krijgen](#iisnodelog).

Het volgende zelfstudiemateriaal wordt het werken met een specifieke kader in App-Service:

- [Een Sails.js web app met Azure App Service implementeren]
- [Maak een chatprogramma Node.js met Socket.IO in Azure App-Service]
- [Het gebruik van io.js met Azure App Service Web Apps]

<a name="version"></a>
## <a name="use-a-specific-nodejs-engine"></a>Een specifieke Node.js engine gebruiken

In de doorsnee werkstroom zien u App Service aan een specifieke Node.js engine gebruiken zoals u gewend in package.json bent.
Bijvoorbeeld:

    "engines": {
        "node": "6.6.0"
    }, 

De installatie-engine Kudu bepaalt welke engine Node.js gebruikt in de volgende volgorde:

- Bekijk eerst de iisnode.yml om te zien of `nodeProcessCommandLine` is opgegeven. Als Ja, kunt u die.
- Bekijk vervolgens de package.json om te zien of `"node": "..."` is opgegeven in de `engines` object. Als Ja, kunt u die.
- Kies een standaard Node.js versie standaard.

>[AZURE.NOTE] Het wordt aanbevolen dat de door u gewenste engine Node.js expliciet definiëren. De standaardversie van Node.js kunt wijzigen en mogelijk dat u fouten in uw Azure web app omdat de standaardversie Node.js niet geschikt voor uw app is.

<a name="iisnodelog"></a>
## <a name="get-stdout-and-stderr-logs-from-iisnode"></a>Stdout en stderr ophalen van iisnode

Ga als volgt te werk om te lezen iisnode logs.

> [AZURE.NOTE] Nadat u deze stappen uitvoert, logboekbestanden bestaat mogelijk niet totdat er een fout optreedt.

1. Open het bestand iisnode.yml waarmee de CLI Azure.

2. De twee volgende parameters instellen: 

        loggingEnabled: true
        logDirectory: iisnode
    
    Samen iisnode vertelt in App-Service voor de uitvoer stdout en stderror op de D:\home\site\wwwroot\**iisnode** directory.

3. Sla uw wijzigingen vervolgens uw wijzigingen naar Azure met de volgende opdrachten voor Git push:

        git add .
        git commit -m "{your commit message}"
        git push azure master
   
    iisnode is nu geconfigureerd. De volgende stappen laten zien hoe deze logboeken te openen.
     
4. In de browser toegang tot de Foutopsporingsconsole Kudu voor uw app, is op:

        https://{appname}.scm.azurewebsites.net/DebugConsole 

    Deze URL verschilt van de URL van de web app door toevoeging van '*.scm.*" de DNS-naam. Als u niet deze toevoeging aan de URL opgeeft, krijgt u een 404-fout.

5. Ga naar D:\home\site\wwwroot\iisnode

    ![Navigeren naar de locatie van de logboekbestanden van iisnode.][iislog-kudu-console-find]

6. Klik op het pictogram **bewerken** voor het logboek dat u wilt lezen. U kunt ook klikken op **downloaden** of **verwijderen** als u wilt.

    ![Het openen van een bestand iisnode.][iislog-kudu-console-open]

    Nu kunt u bekijken in het logboek kunt u fouten opsporen in uw implementatie App-Service.
    
    ![Een logboekbestand iisnode onderzoeken.][iislog-kudu-console-read]

## <a name="debug-your-app-with-node-inspector"></a>Fouten opsporen in uw app met knooppunt Inspector

Als u met uw apps Node.js debug knooppunt inspecteur, kunt u deze voor uw live app in App-Service. Knooppunt Inspector is vooraf geïnstalleerd in de installatie van de iisnode voor App-Service. En als u via Git implementeert, automatisch gegenereerde Web.config uit Kudu al bevat de configuratie die u moet inschakelen knooppunt Inspector.

Schakel knooppunt itemvenster door de volgende stappen uit:

1. Open iisnode.yml in de hoofdmap van uw bibliotheek en de volgende parameters opgeven: 

        debuggingEnabled: true
        debuggerExtensionDll: iisnode-inspector.dll

3. Sla uw wijzigingen vervolgens uw wijzigingen naar Azure met de volgende opdrachten voor Git push:

        git add .
        git commit -m "{your commit message}"
        git push azure master
   
4. Nu gewoon Ga naar van uw app start bestand zoals aangegeven door het script start in de package.json, met toegevoegd aan de URL/Debug. Bijvoorbeeld:

        http://{appname}.azurewebsites.net/server.js/debug
    
    Of,
    
        http://{appname}.azurewebsites.net/app.js/debug

## <a name="more-resources"></a>Meer bronnen

- [Een Node.js versie op te geven in een toepassing Azure](../nodejs-specify-node-version-azure-apps.md)
- [Beste praktijken en de troubleshooting guide for Node.js toepassingen op Azure](app-service-web-nodejs-best-practices-and-troubleshoot-guide.md)
- [Hoe kunt u fouten opsporen in een Node.js in Azure App Service web app](web-sites-nodejs-debug.md)
- [Met behulp van Node.js Modules met Azure toepassingen](../nodejs-use-node-modules-azure-apps.md)
- [Azure App Service Web Apps: Node.js](http://blogs.msdn.com/b/silverlining/archive/2012/06/14/windows-azure-websites-node-js.aspx)
- [Node.js Developer Center](/develop/nodejs/)
- [Aan de slag met web apps in Azure App-Service](app-service-web-get-started.md)
- [De Super geheime Kudu Foutopsporingsconsole verkennen]

<!-- URL List -->

[Azure CLI]: ../xplat-cli-install.md
[Azure App-Service]: ../app-service/app-service-value-prop-what-is.md
[uw voordelen van Visual Studio abonnement activeren]: http://go.microsoft.com/fwlink/?LinkId=623901
[Bower]: http://bower.io/
[Maak een chatprogramma Node.js met Socket.IO in Azure App-Service]: ./web-sites-nodejs-chat-app-socketio.md
[Een Sails.js web app met Azure App Service implementeren]: ./app-service-web-nodejs-sails.md
[De Super geheime Kudu Foutopsporingsconsole verkennen]: /documentation/videos/super-secret-kudu-debug-console-for-azure-web-sites/
[Generator voor Yeoman Express]: https://github.com/petecoop/generator-express
[GIT]: http://www.git-scm.com/downloads
[Het gebruik van io.js met Azure App Service Web Apps]: ./web-sites-nodejs-iojs.md
[iisnode]: https://github.com/tjanczuk/iisnode/wiki
[MEANJS]: http://meanjs.org/
[Node.js]: http://nodejs.org
[SAILSJS]: http://sailsjs.org/
[aanmelden voor een gratis proefversie]: http://go.microsoft.com/fwlink/?LinkId=623901
[web app]: ./app-service-web-overview.md
[Yeoman]: http://yeoman.io/

<!-- IMG List -->

[deployed-express-app]: ./media/app-service-web-nodejs-get-started/deployed-express-app.png
[iislog-kudu-console-find]: ./media/app-service-web-nodejs-get-started/iislog-kudu-console-navigate.png
[iislog-kudu-console-open]: ./media/app-service-web-nodejs-get-started/iislog-kudu-console-open.png
[iislog-kudu-console-read]: ./media/app-service-web-nodejs-get-started/iislog-kudu-console-read.png
