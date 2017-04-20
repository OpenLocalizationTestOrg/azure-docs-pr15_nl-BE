<properties
    pageTitle="Node.js API in Azure App Service app | Microsoft Azure"
    description="Informatie over het maken van een Node.js RESTful API en deze implementeren in een app API in Azure App-Service."
    services="app-service\api"
    documentationCenter="node"
    authors="bradygaster"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-api"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="node"
    ms.topic="get-started-article"
    ms.date="05/26/2016"
    ms.author="rachelap"/>

# <a name="build-a-nodejs-restful-api-and-deploy-it-to-an-api-app-in-azure"></a>Een RESTful API Node.js samenstellen en deze implementeren in een app API in Azure

[AZURE.INCLUDE [app-service-api-get-started-selector](../../includes/app-service-api-get-started-selector.md)]

Deze zelfstudie laat zien hoe een eenvoudige [Node.js](http://nodejs.org) API maken en deze met behulp van [Git](http://git-scm.com)implementeren in een [API app](app-service-api-apps-why-best-platform.md) in [Azure App-Service](../app-service/app-service-value-prop-what-is.md) . U kunt elk besturingssysteem dat Node.js kunt uitvoeren en u zult al uw werk met behulp van opdrachtregelprogramma's, zoals cmd.exe of bash.

## <a name="prerequisites"></a>Vereisten

1. Microsoft Azure account ([open hier een gratis account](https://azure.microsoft.com/pricing/free-trial/))
1. [Node.js](http://nodejs.org) geïnstalleerd (in dit voorbeeld wordt ervan uitgegaan dat u beschikt over versie 4.2.2 Node.js)
2. [Git](https://git-scm.com/) geïnstalleerd
1. [GitHub](https://github.com/) -account

App-Service ondersteunt veel manieren uw code naar een app API implementeren, toont de methode Git en wordt ervan uitgegaan dat u de basiskennis van het werken met Git deze zelfstudie. Zie voor meer informatie over andere implementatiemethoden van [uw app Azure App service implementeren](../app-service-web/web-sites-deploy.md).

## <a name="get-the-sample-code"></a>De voorbeeldcode verkrijgen

1. Open een opdrachtregelinterface die Node.js en Git opdrachten kunt uitvoeren.

1. Navigeer naar een map die u om een lokale Git repository en kloon de [GitHub opslagplaats met de voorbeeldcode gebruiken kunt](https://github.com/Azure-Samples/app-service-api-node-contact-list).

        git clone https://github.com/Azure-Samples/app-service-api-node-contact-list.git

    De monster-API biedt twee eindpunten: een Get-verzoek naar `/contacts` geeft als resultaat een lijst met namen en e-mailadressen in JSON-indeling, terwijl `/contacts/{id}` retourneert alleen de geselecteerde contactpersoon.

## <a name="scaffold-auto-generate-nodejs-code-based-on-swagger-metadata"></a>Scaffold (automatisch gegenereerd) Node.js code op basis van metagegevens van Swagger

[Swagger](http://swagger.io/) is een indeling voor metagegevens die een RESTful API beschrijft. Azure App-Service heeft [ingebouwde ondersteuning voor metagegevens van Swagger](app-service-api-metadata.md). Dit gedeelte van de zelfstudie een API ontwikkeling workflow waarin u eerst Swagger metagegevens maken en die via scaffold modellen (automatisch gegenereerd) servercode voor de API. 

>[AZURE.NOTE] Als u niet wilt om te leren hoe u de code van een bestand met metagegevens Swagger Node.js scaffold, kunt u deze sectie overslaan. Als u wilt gewoon voorbeeldcode naar een nieuwe API app implementeren, gaat u rechtstreeks naar de sectie [een API in Azure app maken](#createapiapp) .

### <a name="install-and-execute-swaggerize"></a>Installeren en uitvoeren van Swaggerize

1. De volgende opdrachten de **yo** **generator swaggerize** NPM modules en globaal installeren uitvoeren.

        npm install -g yo
        npm install -g generator-swaggerize

    Swaggerize is een hulpprogramma waarmee u servercode voor een API die wordt beschreven door een bestand met metagegevens Swagger genereert. De Swagger-bestand dat u gaat gebruiken met de naam *api.json* en bevindt zich in de map *start* van de opslagplaats die u gekloond.

2. Ga naar de map *start* en vervolgens uitvoeren van de `yo swaggerize` opdracht. Swaggerize zal vragen een aantal vragen.  Voer "ContactList", voor het **pad naar het document swagger**, 'api.json' voor **Wat dit project aanroepen**, en voor **Express, tevreden, of Restify**, voert u 'express'.

        yo swaggerize

    ![Swaggerize vanaf de opdrachtregel](media/app-service-api-nodejs-api-app/swaggerize-command-line.png)
    
    **Opmerking**: als u een fout in deze stap, de volgende stap wordt uitgelegd hoe u het probleem oplossen.

    Swaggerize maakt een map application, scaffolds-handlers en configuratiebestanden en genereert u een bestand **package.json** . De snelle weergave-engine wordt gebruikt voor het genereren van de Swagger help-pagina.  

3. Als de `swaggerize` opdracht niet werkt met een 'Onverwachte token' of "Ongeldige escape-reeks" fout, de oorzaak van de fout corrigeren door de gegenereerde *package.json* -bestand te bewerken. In de `regenerate` krachtens regel `scripts`, de backslash die voorafgaat aan de *api.json* met een slash zodanig wijzigen dat de regel in het volgende voorbeeld ziet:

        "regenerate": "yo swaggerize --only=handlers,models,tests --framework express --apiPath config/api.json"

1. Navigeer naar de map waarin de scaffolded code (in dit geval de submap */start/ContactList* ).

1. Uitvoeren van `npm install`.
    
        npm install
        
2. Installeer de module **jsonpath** NPM. 

        npm install --save jsonpath
        
    ![Jsonpath installeren](media/app-service-api-nodejs-api-app/jsonpath-install.png)

1. Installeer de module **swaggerize ui** NPM. 

        npm install --save swaggerize-ui
        
    ![Swaggerize Ui installeren](media/app-service-api-nodejs-api-app/swaggerize-ui-install.png)

### <a name="customize-the-scaffolded-code"></a>De code van de scaffolded aanpassen

1. De **lib** map van de map **start** kopiëren naar de map **ContactList** is gemaakt door de scaffolder. 

1. De code in het bestand **handlers/contacts.js** wordt vervangen door de volgende code. 

    Deze code gebruikt de JSON-gegevens opgeslagen in het bestand **lib/contacts.json** die wordt aangeboden door **lib/contactRepository.js**. De nieuwe contacts.js code reageert op HTTP-aanvragen naar alle contactpersonen op te vragen en deze een JSON-nettolading. 

        'use strict';
        
        var repository = require('../lib/contactRepository');
        
        module.exports = {
            get: function contacts_get(req, res) {
                res.json(repository.all())
            }
        };

1. De code in het bestand **handlers/contacts/{id}.js** wordt vervangen door de code fofllowing. 

        'use strict';

        var repository = require('../../lib/contactRepository');
        
        module.exports = {
            get: function contacts_get(req, res) {
                res.json(repository.get(req.params['id']));
            }    
        };

1. De code in **server.js** met de volgende code vervangen. 

    De wijzigingen in het bestand server.js worden genoemd door opmerkingen te gebruiken zodat u de wijzigingen kunt zien. 

        'use strict';

        var port = process.env.PORT || 8000; // first change

        var http = require('http');
        var express = require('express');
        var bodyParser = require('body-parser');
        var swaggerize = require('swaggerize-express');
        var swaggerUi = require('swaggerize-ui'); // second change
        var path = require('path');

        var app = express();

        var server = http.createServer(app);

        app.use(bodyParser.json());

        app.use(swaggerize({
            api: path.resolve('./config/api.json'), // third change
            handlers: path.resolve('./handlers'),
            docspath: '/swagger' // fourth change
        }));

        // change four
        app.use('/docs', swaggerUi({
          docs: '/swagger'  
        }));

        server.listen(port, function () { // fifth and final change
        });

### <a name="test-with-the-api-running-locally"></a>Met de API voor lokaal uitgevoerde test

1. De server met de opdrachtregel executable Node.js activeren. 

        node server.js

1. Wanneer u naar **http://localhost:8000/contactpersonen bladert**, ziet u de JSON-uitvoer van de lijst met contactpersonen (of wordt u gevraagd deze te downloaden, afhankelijk van uw browser). 

    ![Alle contactpersonen Api-aanroep](media/app-service-api-nodejs-api-app/all-contacts-api-call.png)

1. Wanneer u naar **contactpersonen-http://localhost:8000/2 bladert**, ziet u de contactpersoon die wordt vertegenwoordigd door dit id-waarde.

    ![Specifieke contactpersoon Api-aanroep](media/app-service-api-nodejs-api-app/specific-contact-api-call.png)

1. De Swagger JSON-gegevens wordt via het **eindpunt/swagger** aangeboden:

    ![Contactpersonen Swagger Json](media/app-service-api-nodejs-api-app/contacts-swagger-json.png)

1. De gebruikersinterface van Swagger wordt aangeboden via het eindpunt van het **product** . In de gebruikersinterface van Swagger, kunt u de uitgebreide functies van HTML-client voor het testen van de API.

    ![Swagger Ui](media/app-service-api-nodejs-api-app/swagger-ui.png)

## <a id="createapiapp"></a>Maak een nieuwe API App

In deze sectie u de Azure portal maakt een nieuwe API App in Azure. Deze API app vertegenwoordigt de compute-bronnen die Azure biedt u uw programmacode uitvoeren. In latere secties zult u uw code naar de toepassing van de nieuwe API implementeren.

1. Ga naar de [Portal Azure](https://portal.azure.com/). 

1. Klik op **Nieuw > Web + Mobile > API App**. 

    ![Nieuwe API-app in de portal](media/app-service-api-nodejs-api-app/new-api-app-portal.png)

4. Voer een **naam** die uniek is in het domein *azurewebsites.net* , zoals NodejsAPIApp plus een getal uniek te maken. 

    Als de naam bijvoorbeeld `NodejsAPIApp`, de URL van de `nodejsapiapp.azurewebsites.net`.

    Als u een naam die iemand anders al heeft gebruikt opgeeft, ziet u een rood uitroepteken rechts.

6. Klikt u op **Nieuw**en voer vervolgens in **naam van nieuwe resourcegroep** 'NodejsAPIAppGroup' of een andere naam als u liever in de vervolgkeuzelijst **Resourcegroep** . 

    Een [resourcegroep](../azure-resource-manager/resource-group-overview.md) is een verzameling van Azure bronnen, zoals toepassingen, databases en VMs API. Voor deze zelfstudie is het beste een nieuwe resourcegroep maken omdat die gemakkelijk te verwijderen in één stap de Azure bronnen die u voor de zelfstudie maakt.

4. **App plan/locatie**op en klik vervolgens op **Nieuw**.

    ![App Service plan maken](./media/app-service-api-nodejs-api-app/newappserviceplan.png)

    In de volgende stappen maakt u een App Service plan voor de nieuwe resourcegroep. Een serviceplan App geeft de compute-bronnen die compatibel is met uw app API. Bijvoorbeeld als u de vrij laag, de API-app wordt uitgevoerd op gedeelde VMs, terwijl voor sommige betaalde lagen deze wordt uitgevoerd op specifieke VMs. Voor meer informatie over App serviceplannen overzicht [App Service plannen](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).

5. Voer "NodejsAPIAppPlan" of een andere naam als u liever in de blade **App Service Plan** .

5. Kies de locatie die zich het dichtst bij u in de vervolgkeuzelijst **locatie** .

    Deze instelling bepaalt welke Azure datacenter in uw toepassing wordt uitgevoerd. Voor deze zelfstudie kunt u elk gebied en het won't merkbaar verschil maken. Maar voor een productie-app u uw server moet zo dicht mogelijk aan de clients die gebruikmaken van deze [Latentie](http://www.bing.com/search?q=web%20latency%20introduction&qs=n&form=QBRE&pq=web%20latency%20introduction&sc=1-24&sp=-1&sk=&cvid=eefff99dfc864d25a75a83740f1e0090)minimaliseren.

5. Klik op **prijzen laag > Alles > F1 gratis**.

    Voor deze zelfstudie biedt de prijzen vrij laag voldoende prestaties.

    ![Vrije prijzen laag selecteren](./media/app-service-api-nodejs-api-app/selectfreetier.png)

6. Klik op **OK**in het blad **App Service Plan** .

7. Klik op **maken**in de blade **API App** .

## <a name="set-up-your-new-api-app-for-git-deployment"></a>Instellen van uw nieuwe API app voor Git distributie

U zult uw code implementeren API App doorvoeracties duwen om een Git repository in Azure App-Service. In deze sectie van de handleiding maakt u de referenties en Git repository in Azure waarmee u voor de implementatie.  

1. Nadat uw app API is gemaakt, klikt u op **App Services > {uw app API}** vanaf de introductiepagina van de portal. 

    De portal wordt de blades **API App** en **-Instellingen** weergegeven.

    ![Portal API app en de instellingen voor bladeservers](media/app-service-api-nodejs-api-app/portalapiappblade.png)

1. Ga naar de sectie **publiceren** in de blade **Instellingen** en klik op **referenties voor implementatie**.
 
3. Geef een gebruikersnaam en wachtwoord in de blade **implementatie referenties instellen** en klik vervolgens op **Opslaan**.

    U gebruikt deze referenties voor het publiceren van uw code Node.js naar uw app API. 

    ![Implementatie van referenties](media/app-service-api-nodejs-api-app/deployment-credentials.png)

1. Klik op in het blad **Instellingen** **distributiebron > Kies bron > lokale Git Repository**, klikt u op **OK**.

    ![Git Repo maken](media/app-service-api-nodejs-api-app/create-git-repo.png)

1. Zodra de Git repository is gemaakt de blade wijzigingen zodat u uw active-implementaties. Aangezien de opslagplaats nieuw is, hebt u geen actieve installaties in de lijst. 

    ![Geen actieve installaties](media/app-service-api-nodejs-api-app/no-active-deployments.png)

1. Kopieer de URL Git repository. Hiertoe gaat u naar het blad voor uw nieuwe API App en bekijk de **Essentials** -sectie van het blad. U ziet de **Git clone URL** in de sectie **Essentials** . Wanneer u de muisaanwijzer op deze URL, ziet u een pictogram aan de rechterzijde wordt de URL naar het Klembord kopiëren. Klik op dit pictogram om de URL te kopiëren.

    ![De Git Url ophalen vanaf de Portal](media/app-service-api-nodejs-api-app/get-the-git-url-from-the-portal.png)

    **Opmerking**: U moet de URL in het volgende gedeelte dus zorg ervoor dat het op te slaan ergens voor het moment dat Git-kloon.

Nu dat u een API App met een Git repository maken van back-hebt, kunt u push-code in de opslagplaats voor de implementatie van de code aan de API-app. 

## <a name="deploy-your-api-code-to-azure"></a>Uw code API implementeren op Azure

In deze sectie maakt u een lokaal Git repository met uw servercode voor de API en vervolgens u push-de code uit die bibliotheek naar de bibliotheek in Azure die u eerder hebt gemaakt.

1. Kopie de `ContactList` map naar een locatie die u voor een nieuwe lokale Git repository gebruiken kunt. Als u het eerste gedeelte van de zelfstudie heeft, kopieert u `ContactList` van de `start` map. kopiëren, `ContactList` van de `end` map.

1. Ga naar de nieuwe map in het opdrachtregelprogramma vervolgens uitvoeren met de volgende opdracht maakt u een nieuwe lokale Git repository. 

        git init

     ![Nieuwe lokale Git Repo](media/app-service-api-nodejs-api-app/new-local-git-repo.png)

1. De volgende opdracht om een extern voor de opslagplaats van uw app API Git toevoegen uitvoeren. 

        git remote add azure YOUR_GIT_CLONE_URL_HERE

    **Opmerking**: de tekenreeks "YOUR_GIT_CLONE_URL_HERE" vervangen door uw eigen Git clone URL die u eerder hebt gekopieerd. 

1. De volgende opdrachten voor het maken van een commit waarin alle van uw code uitgevoerd. 

        git add .
        git commit -m "initial revision"

    ![GIT Commit-uitvoer](media/app-service-api-nodejs-api-app/git-commit-output.png)

1. Uitvoering van de opdracht om de code naar Azure. Wanneer u wordt gevraagd om een wachtwoord, voer die u eerder in de portal Azure gemaakt.

        git push azure master

    Dit veroorzaakt een implementatie van de API-app.  

1. Terug te keren naar het blade **implementaties** voor uw app API in uw browser, en u ziet dat de implementatie plaatsvindt. 

    ![Implementatie gebeurt](media/app-service-api-nodejs-api-app/deployment-happening.png)

    De command line interface geeft de status van uw implementatie tegelijk, terwijl deze plaatsvindt. 

    ![Knooppunt Js implementatie gebeurt](media/app-service-api-nodejs-api-app/node-js-deployment-happening.png)

    Nadat de installatie is voltooid, geeft de blade **implementaties** de geslaagde implementatie van uw codewijzigingen in uw App API. 

## <a name="test-with-the-api-running-in-azure"></a>Met de API uitvoert in Azure testen
 
3. Kopieer de **URL** in de **Essentials** -sectie van uw App API blade. 

    ![De implementatie is voltooid](media/app-service-api-nodejs-api-app/deployment-completed.png)

1. Met een REST API-client zoals Postman of Fiddler (of uw webbrowser), geef de URL van uw contactpersonen API-aanroep, wordt de `/contacts` eindpunt van uw app API. De URL van de`https://{your API app name}.azurewebsites.net/contacts`

    Wanneer u een GET-verzoek naar dit eindpunt verzendt, krijgt u de JSON-uitvoer van uw app API.

    ![Postman-Api raken](media/app-service-api-nodejs-api-app/postman-hitting-api.png)

2. In een browser, Ga naar de `/docs` eindpunt van de gebruikersinterface Swagger uitproberen als dit wordt uitgevoerd in Azure.

Nu dat er continue levering van wired, kunt u code wijzigen en deze vervolgens implementeert op Azure door doorvoeracties aan uw Azure Git repository pushen.

## <a name="next-steps"></a>Volgende stappen

U hebt nu met succes een API App gemaakt en Node.js API code geïmplementeerd op deze. In de volgende zelfstudie ziet hoe [API apps van JavaScript-clients met behulp van CORS](app-service-api-cors-consume-javascript.md)in beslag neemt.
