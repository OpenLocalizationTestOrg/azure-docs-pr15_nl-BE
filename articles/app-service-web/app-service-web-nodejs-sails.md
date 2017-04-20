<properties
    pageTitle="Een Sails.js web app met Azure App Service implementeren"
    description="Informatie over het implementeren van een toepassing Node.js Azure App-Service. In deze zelfstudie wordt beschreven hoe u een Sails.js web app te implementeren."
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
    ms.topic="article"
    ms.date="09/23/2016"
    ms.author="cephalin"/>

# <a name="deploy-a-sailsjs-web-app-to-azure-app-service"></a>Een Sails.js web app met Azure App Service implementeren

In deze zelfstudie wordt beschreven hoe u een app Sails.js implementeren op Azure App-Service. In het proces, kunt u sommige algemene kennis over het configureren van uw app Node.js uitvoeren in de App vindt. 

Hebt u enige basiskennis van de Sails.js. Deze zelfstudie is niet bedoeld om u te helpen met problemen met betrekking tot het uitvoeren van Sail.js in het algemeen.


## <a name="prerequisites"></a>Vereisten

- [Node.js](https://nodejs.org/)
- [Sails.js](http://sailsjs.org/get-started)
- [GIT](http://www.git-scm.com/downloads)
- [Azure CLI](../xplat-cli-install.md)
- Een account van Microsoft Azure. Als u geen account hebt, kunt u [uw voordelen van Visual Studio abonnee activeren](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F)of [registreren voor een gratis proefversie](/pricing/free-trial/?WT.mc_id=A261C142F) .

>[AZURE.NOTE] Azure App Service in actie voordat u aanmelden voor een account Azure Ga [proberen App](http://go.microsoft.com/fwlink/?LinkId=523751)-service. Daar kunt u direct maken een tijdelijk starter app in App-Service — geen creditcard vereist, geen verplichtingen.

## <a name="step-1-create-a-sailsjs-app-locally"></a>Stap 1: Maak een lokaal app van Sails.js

Snel Maak eerst een Sails.js standaard app in de ontwikkelomgeving door de volgende stappen:

1. Open de terminal opdrachtregelprogramma van uw keuze en `CD` in een werkmap.

2. Maak een Sails.js app en uit te voeren:

        sails new <appname>
        cd <appname>
        sails lift

    Zorg ervoor dat u kunt navigeren naar de standaard-startpagina op http://localhost:1377.

## <a name="step-2-create-the-azure-app-resource"></a>Stap 2: Maak de Azure toepassingsbron

Maak vervolgens de App-servicebron in Azure. U gaat uw app Sails.js later te implementeren.

1. Meld u aan bij Azure lijkt dus:
1. ASM-modus wijzigen in de dezelfde terminal en log in op Azure:

        azure config mode asm
        azure login

    Volg de vraag om te gaan met de aanmelding in een browser met een Microsoft-account met uw abonnement Azure.

2. Zorg ervoor dat u nog steeds in de hoofdmap van het project Sails.js. De App Service app-bron maakt in Azure met de naam van een unieke app met de volgende opdracht. URL van uw webtoepassing is http://&lt;appname >. azurewebsites.net.

        azure site create --git <appname>

    Volg de prompt te implementeren op een Azure-regio selecteren. Als u hebt nooit Git/FTP-implementatie referenties voor uw abonnement op Azure, u ook gevraagd om deze te maken.

    Zodra de App Service toepassingsbron is gemaakt:

    - Sails.js app is Git geïnitialiseerd
    - Uw lokale Git geïnitialiseerd opslagplaats is verbonden met het nieuwe App Service app als een externe, Git toepasselijke naam 'azure', en
    - En iisnode.yml-bestand gemaakt in de hoofdmap. U kunt dit bestand [iisnode](https://github.com/tjanczuk/iisnode), dat App-Service gebruikt voor het uitvoeren van Node.js apps configureren.

## <a name="step-3-configure-and-deploy-your-sailsjs-app"></a>Stap 3: Configureren en implementeren van uw app Sails.js

 Werken met een Sails.js-app in de App-Service bestaat uit drie stappen:

 - Uw app voor het uit te voeren in de App-Service configureren
 - Deze App service implementeren
 - Lees stderr en stdout Logboeken oplossen van eventuele problemen bij de implementatie

Ga als volgt te werk:

1. Het nieuwe iisnode.yml bestand in de hoofddirectory te openen en de volgende twee regels toe te voegen:

        loggingEnabled: true
        logDirectory: iisnode

    Logboekregistratie is nu beschikbaar voor iisnode. Voor meer informatie over hoe dit werkt, Zie  [stdout en stderr logs van iisnode krijgen](app-service-web-nodejs-get-started.md#iisnodelog).

2. Config/env/production.js instellen en configureren van uw productieomgeving Open `port` en `hookTimeout`:

        module.exports = {

            // Use process.env.port to handle web requests to the default HTTP port
            port: process.env.port,
            // Increase hooks timout to 30 seconds
            // This avoids the Sails.js error documented at https://github.com/balderdashy/sails/issues/2691
            hookTimeout: 30000,

            ...
        };

    Documentatie voor deze configuratie-instellingen kunt u vinden in de  [Documentatie van de Sails.js](http://sailsjs.org/documentation/reference/configuration/sails-config).

    Vervolgens moet u controleren of [heidens](https://www.npmjs.com/package/grunt) is compatibel met Azure van netwerkstations. Grunt-versies lager dan 1.0.0 maakt gebruik van verouderde [glob](https://www.npmjs.com/package/glob) -pakket (minder dan 5.0.14) die netwerkstations niet ondersteunt. 

3. Open package.json en wijzig de `grunt` versie `1.0.0` en verwijdert u alle `grunt-*` pakketten. Uw `dependencies` eigenschap ziet er zo uit:

        "dependencies": {
            "ejs": "<leave-as-is>",
            "grunt": "1.0.0",
            "include-all": "<leave-as-is>",
            "rc": "<leave-as-is>",
            "sails": "<leave-as-is>",
            "sails-disk": "<leave-as-is>",
            "sails-sqlserver": "<leave-as-is>"
        },

3. Voeg de volgende in package.json, `engines` eigenschap voor het instellen van de Node.js versie die we willen.

        "engines": {
            "node": "6.6.0"
        },

6. Sla uw wijzigingen op en test uw wijzigingen om ervoor te zorgen dat uw app nog steeds wordt lokaal uitgevoerd. Hiertoe verwijdert u de `node_modules` map en voer:

        npm install
        sails lift

4. Gebruik git nu uw app naar Azure implementeren:

        git add .
        git commit -m "<your commit message>"
        git push azure master

5. Net Start ten slotte uw live Azure app in de browser:

        azure site browse

    U ziet nu de introductiepagina van dezelfde Sails.js.
    
    ![](./media/app-service-web-nodejs-sails/sails-in-azure.png)

## <a name="troubleshoot-your-deployment"></a>Problemen met de implementatie

Als uw toepassing Sails.js is mislukt voor een of andere reden in de App-Service, vindt u de logboeken stderr om te helpen bij het oplossen van dit probleem.
Voor meer informatie Zie [stdout en stderr logs van iisnode krijgen](app-service-web-nodejs-sails.md#iisnodelog).
Als deze is gestart, het logboek stdout moet u de bekende bericht weergeven:

                .-..-.

    Sails              <|    .-..-.
    v0.12.4             |\
                        /|.\
                        / || \
                    ,'  |'  \
                    .-'.-==|/_--'
                    `--'-------' 
    __---___--___---___--___---___--___
    ____---___--___---___--___---___--___-__

    Server lifted in `D:\home\site\wwwroot`
    To see your app, visit http://localhost:\\.\pipe\c775303c-0ebc-4854-8ddd-2e280aabccac
    To shut down Sails, press <CTRL> + C at any time.

U kunt de granulatie van de logboeken stdout in het bestand [config/log.js](http://sailsjs.org/#!/documentation/concepts/Logging) beheren. 

## <a name="connect-to-a-database-in-azure"></a>Verbinding maken met een database in Azure

Als u wilt koppelen aan een database in Azure, maken van de database van uw keuze in Azure zoals Azure SQL-Database, MySQL, MongoDB, Cache Azure (bestand Vgx.), enz., en gebruik de bijbehorende [adapter datastore](https://github.com/balderdashy/sails#compatibility) tot stand te brengen. De stappen in deze sectie hoe u verbinding met een MySQL-database in Azure.

1. Volg de zelfstudie [hier](../store-php-create-mysql-database.md) een MySQL-database maken in Azure.

2. Vanaf de opdrachtregel terminal de MySQL-adapter te installeren:

        npm install sails-mysql --save

3. Open config/connections.js en het volgende object toevoegen aan de lijst: 

        mySql: {
            adapter: 'sails-mysql',
            user: process.env.dbuser,
            password: process.env.dbpassword,
            host: process.env.dbhost, 
            database: process.env.dbname,
            options: {
                encrypt: true
            }
        },

4. Voor elke omgevingsvariabele (`process.env.*`), moet u deze instellen in de App-Service. Voer hiervoor de volgende opdrachten vanaf de terminal. Alle informatie u moet in de portal Azure is (Zie [verbinding maken met de MySQL-database](../store-php-create-mysql-database.md#connect)).

        azure site appsetting add dbuser="<database user>"
        azure site appsetting add dbpassword="<database password>"
        azure site appsetting add dbhost="<database hostname>"
        azure site appsetting add dbname="<database name>"
        
    Plaatsen van uw instellingen in Azure app instellingen zorgt ervoor dat gevoelige gegevens uit uw bron (Git). Vervolgens configureert u uw ontwikkelomgeving voor het gebruik van dezelfde gegevens voor de verbinding.

4. Open config/local.js en de volgende verbindingen object toevoegen:

        connections: {
            mySql: {
                user: "<database user>",
                password: "<database password>",
                host: "<database hostname>", 
                database: "<database name>",
            },
        },
    
    Deze configuratie heeft prioriteit boven de instellingen in het bestand config/connections.js voor de lokale omgeving. Dit bestand wordt uitgesloten door de standaard .gitignore in uw project, zodat deze niet in Git opgeslagen. U bent nu verbinding kunnen maken met de MySQL-database van uw Azure web app en van uw lokale ontwikkelomgeving.

4. Open config/env/production.js voor het configureren van uw productieomgeving en voeg de volgende `models` object:

        models: {
            connection: 'mySql',
            migrate: 'safe'
        },

4. Open config/env/development.js voor het configureren van uw ontwikkelomgeving en voeg de volgende `models` object:

        models: {
            connection: 'mySql',
            migrate: 'alter'
        },

    `migrate: 'alter'`kunt u databasefuncties migratie maken en bijwerken in de MySQL tabellen in uw database gemakkelijk te gebruiken. Echter, `migrate: 'safe'` voor uw omgeving Azure (productie) wordt gebruikt, omdat de Sails.js kunt u niet gebruiken `migrate: 'alter'` in een productieomgeving (Zie de  [Documentatie van de Sails.js](http://sailsjs.org/documentation/concepts/models-and-orm/model-settings)).

4. Vanaf de terminal, [genereren](http://sailsjs.org/documentation/reference/command-line-interface/sails-generate) een Sails.js [blauwdruk API](http://sailsjs.org/documentation/concepts/blueprints) zoals u normaal zou zijn, voert u `sails lift` voor het maken van de database met Sails.js de databasemigratie. Bijvoorbeeld:

         sails generate api mywidget
         sails lift

    De `mywidget` model gegenereerd door deze opdracht is leeg, maar we kunt gebruiken om te tonen dat we databaseverbindingen hebben.
    Bij het uitvoeren van `sails lift`, die daarmee de ontbrekende tabellen voor de modellen uw app wordt gebruikt.

6. Toegang tot de blauwdruk API die u zojuist hebt gemaakt in de browser. Bijvoorbeeld:

        http://localhost:1337/mywidget/create
    
    De API moet het gemaakte item retourneren aan u in het browservenster, wat betekent dat uw database is gemaakt.

        {"id":1,"createdAt":"2016-09-23T13:32:00.000Z","updatedAt":"2016-09-23T13:32:00.000Z"}

5. Nu uw wijzigingen push naar Azure en blader naar uw app om te controleren of dat het nog steeds werkt.

        git add .
        git commit -m "<your commit message>"
        git push azure master
        azure site browse

6. Toegang tot de API-blauwdruk van uw Azure web app. Bijvoorbeeld:

        http://<appname>.azurewebsites.net/mywidget/create

    Als de API resulteert in een andere nieuwe post, is uw Azure web app praten met de MySQL-database.

## <a name="more-resources"></a>Meer bronnen

- [Aan de slag met Node.js web apps in Azure App-Service](app-service-web-nodejs-get-started.md)
- [Met behulp van Node.js Modules met Azure toepassingen](../nodejs-use-node-modules-azure-apps.md)
