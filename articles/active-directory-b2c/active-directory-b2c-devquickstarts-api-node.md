<properties
    pageTitle="Azure AD B2C: Een web API beveiligen met Node.js | Microsoft Azure"
    description="Het bouwen van een web Node.js API die tokens van een huurder B2C accepteert"
    services="active-directory-b2c"
    documentationCenter=""
    authors="brandwe"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="javascript"
    ms.topic="hero-article"
    ms.date="08/30/2016"
    ms.author="brandwe"/>

# <a name="azure-ad-b2c-secure-a-web-api-by-using-nodejs"></a>Azure AD B2C: Een web API beveiligen met Node.js

<!-- TODO [AZURE.INCLUDE [active-directory-b2c-devquickstarts-web-switcher](../../includes/active-directory-b2c-devquickstarts-web-switcher.md)]-->

Met B2C Azure Active Directory (AD Azure), kunt u een web API kunt beveiligen met behulp van toegangstokens OAuth 2.0. Deze tokens kunnen uw clienttoepassingen die Azure AD B2C gebruiken om de API te verifiëren. In dit artikel wordt beschreven hoe u een 'takenlijst' API waarmee gebruikers toe te voegen en een lijst met taken maken. De web-API is beveiligd met Azure AD B2C en kan alleen geverifieerde gebruikers voor het beheren van hun taak-overzicht.

> [AZURE.NOTE]  In dit voorbeeld is geschreven met behulp van onze [iOS B2C-voorbeeldtoepassing](active-directory-b2c-devquickstarts-ios.md)verbonden zijn. Het huidige overzicht eerst doen en volg vervolgens samen met het monster.

**Passport** is verificatie middleware voor Node.js. Flexibele en modulaire, Passport onopvallend kan worden geïnstalleerd in een Express-gebaseerde of Restify van de webtoepassing. Een uitgebreide reeks strategieën ondersteunt verificatie met behulp van een gebruikersnaam en wachtwoord, Facebook en Twitter. We hebben een strategie ontwikkeld voor Azure Active Directory (AD Azure). U deze module hebt geïnstalleerd en vervolgens de Azure AD `passport-azure-ad` plug-in.

Als u dit voorbeeld wilt, moet u:

1. Een toepassing met Azure Active Directory registreren.
2. Instellen van uw toepassing voor het gebruik van Passport `azure-ad-passport` plug-in.
3. Een clienttoepassing configureren voor het web 'takenlijst' API aanroepen.


## <a name="get-an-azure-ad-b2c-directory"></a>Een AD-B2C Azure map ophalen

Voordat u AD-B2C Azure gebruiken kunt, moet u een map maken of pachters.  Een directory is een container voor alle gebruikers, toepassingen en groepen.  Als je niet hebt al verder [een B2C-map maken](active-directory-b2c-get-started.md) voordat u.

## <a name="create-an-application"></a>Een toepassing maken

Vervolgens moet u voor het maken van een app in de B2C-map die Azure AD enige informatie die nodig is geeft om veilig communiceren met uw app. In dit geval worden de clienttoepassing en de web-API vertegenwoordigd door een enkele **Aanvraag-ID**omdat ze bestaan uit één logische app. Als u wilt een app maken, volg [de instructies](active-directory-b2c-app-registration.md). Zorg ervoor dat:

- Inclusief een **web app/web api** in de toepassing
- Voer `http://localhost/TodoListService` als een **antwoord op URL**. Het is de standaard-URL voor deze voorbeeldcode.
- Een **geheim van toepassing** voor uw toepassing te maken en te kopiëren. U deze gegevens later nodig hebt. Houd er rekening mee dat deze waarde worden [XML-escaped moet](https://www.w3.org/TR/2006/REC-xml11-20060816/#dt-escape) voordat u deze kunt gebruiken.
- Kopieer de **Toepassings-ID** die is toegewezen aan uw app. U deze gegevens later nodig hebt.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a>Uw beleid maken

In Azure AD B2C, wordt elke gebruikerservaring gedefinieerd door een [beleid](active-directory-b2c-reference-policies.md). Deze app bevat twee identiteit ervaringen: aanmelden en aanmelden. Moet u voor het maken van een beleid voor elk type, zoals beschreven in het [artikel beleid](active-directory-b2c-reference-policies.md#how-to-create-a-sign-up-policy).  Als u de drie beleidsregels maakt, moet u:

- Kies de **weergegeven naam** en andere kenmerken van de aanmelding in uw aanmelding beleid.
- Kies de **weergavenaam** en de **Object-ID** application claims in elk beleid.  U kunt ook andere vorderingen.
- De **naam** van elk beleid kopiëren nadat u deze hebt gemaakt. Er is het voorvoegsel `b2c_1_`.  Moet u later namen beleid.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

Nadat u de drie beleidsregels hebt gemaakt, bent u klaar om uw app te bouwen.

Meer informatie over de werking van beleid in Azure AD B2C, start met de [zelfstudie .NET web app aan de slag](active-directory-b2c-devquickstarts-web-dotnet.md).

## <a name="download-the-code"></a>De code downloaden

De code voor deze zelfstudie [wordt onderhouden op GitHub](https://github.com/AzureADQuickStarts/B2C-WebAPI-NodeJS). Voor het opbouwen van het monster als u gaat, kunt u [een skelet project als een .zip-bestand downloaden](https://github.com/AzureADQuickStarts/B2C-WebAPI-NodeJS/archive/skeleton.zip). U kunt ook het skelet klonen:

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/B2C-WebAPI-NodeJS.git
```

De voltooide app is ook [beschikbaar als een ZIP-bestand](https://github.com/AzureADQuickStarts/B2C-WebAPI-NodeJS/archive/complete.zip) of op de `complete` tak van de dezelfde opslagplaats.

## <a name="download-nodejs-for-your-platform"></a>Node.js voor uw platform downloaden

In dit voorbeeld is gebruikt, moet u een werkende installatie van Node.js. 

Node.js installeren vanaf [nodejs.org](http://nodejs.org).

## <a name="install-mongodb-for-your-platform"></a>MongoDB installeren voor uw platform

In dit voorbeeld is gebruikt, moet u een werkende installatie van MongoDB. We MongoDB gebruiken om de REST API permanente verschillende exemplaren van de server.

MongoDB installeren vanaf [mongodb.org](http://www.mongodb.org).

> [AZURE.NOTE] Dit overzicht wordt ervan uitgegaan dat u de standaard installatie- en eindpunten voor MongoDB, namelijk op het moment van schrijven van dit `mongodb://localhost`.

## <a name="install-the-restify-modules-in-your-web-api"></a>De Restify-modules te installeren op uw web-API

We Restify bouwen uw REST-API gebruiken. Restify afkomstig is van een minimale en flexibele Node.js application framework Express. Het heeft een krachtige verzameling functies voor het bouwen van REST API's op verbinding maken.

### <a name="install-restify"></a>Restify installeren

Wijzigen vanaf de opdrachtregel de map `azuread`. Als de `azuread` map niet bestaat, maakt u deze.

`cd azuread`of`mkdir azuread;`

Voer de volgende opdracht:

`npm install restify`

Met deze opdracht installeert Restify.

#### <a name="did-you-get-an-error"></a>Kom je een fout?

In sommige besturingssystemen wanneer u `npm`, wordt het foutbericht `Error: EPERM, chmod '/usr/local/bin/..'` en een verzoek als een beheerder de account worden uitgevoerd. Als dit probleem optreedt, gebruikt u de `sudo` uit te voeren `npm` op een hoger bevoegdhedenniveau.

#### <a name="did-you-get-a-dtrace-error"></a>Kom je een fout DTrace?

Als deze tekst ziet u tijdens de installatie van Restify:

```Shell
clang: error: no such file or directory: 'HD/azuread/node_modules/restify/node_modules/dtrace-provider/libusdt'
make: *** [Release/DTraceProviderBindings.node] Error 1
gyp ERR! build error
gyp ERR! stack Error: `make` failed with exit code: 2
gyp ERR! stack     at ChildProcess.onExit (/usr/local/lib/node_modules/npm/node_modules/node-gyp/lib/build.js:267:23)
gyp ERR! stack     at ChildProcess.EventEmitter.emit (events.js:98:17)
gyp ERR! stack     at Process.ChildProcess._handle.onexit (child_process.js:789:12)
gyp ERR! System Darwin 13.1.0
gyp ERR! command "node" "/usr/local/lib/node_modules/npm/node_modules/node-gyp/bin/node-gyp.js" "rebuild"
gyp ERR! cwd /Volumes/Development HD/azuread/node_modules/restify/node_modules/dtrace-provider
gyp ERR! node -v v0.10.11
gyp ERR! node-gyp -v v0.10.0
gyp ERR! not ok
npm WARN optional dep failed, continuing dtrace-provider@0.2.8
```

Restify biedt een krachtig mechanisme voor het traceren van REST roept met behulp van DTrace. Echter, veel besturingssystemen geen DTrace beschikbaar. U kunt deze fouten negeren.

De uitvoer van de opdracht lijkt nu op deze tekst:

    restify@2.6.1 node_modules/restify
    ├── assert-plus@0.1.4
    ├── once@1.3.0
    ├── deep-equal@0.0.0
    ├── escape-regexp-component@1.0.2
    ├── qs@0.6.5
    ├── tunnel-agent@0.3.0
    ├── keep-alive-agent@0.0.1
    ├── lru-cache@2.3.1
    ├── node-uuid@1.4.0
    ├── negotiator@0.3.0
    ├── mime@1.2.11
    ├── semver@2.2.1
    ├── spdy@1.14.12
    ├── backoff@2.3.0
    ├── formidable@1.0.14
    ├── verror@1.3.6 (extsprintf@1.0.2)
    ├── csv@0.3.6
    ├── http-signature@0.10.0 (assert-plus@0.1.2, asn1@0.1.11, ctype@0.5.2)
    └── bunyan@0.22.0 (mv@0.0.5)

## <a name="install-passport-in-your-web-api"></a>Passport installeren in uw web-API

Wijzigen vanaf de opdrachtregel de map `azuread`, als deze nog niet aanwezig.

Passport met de volgende opdracht installeren:

`npm install passport`

De uitvoer van de opdracht moet er ongeveer als deze tekst:

    passport@0.1.17 node_modules\passport
    ├── pause@0.0.1
    └── pkginfo@0.2.3

## <a name="add-passport-azuread-to-your-web-api"></a>Passport-azuread toevoegen aan uw web-API

Voeg vervolgens de OAuth-strategie met behulp van `passport-azuread`, een suite van strategieën die Azure AD aan Passport koppelen. Pas deze strategie voor tokens in de steekproef REST API aan toonder.

> [AZURE.NOTE] Maar OAuth2 een kader waarin een onbekend type token kan worden uitgegeven biedt, kunnen alleen bepaalde token typen wijdverbreide gebruik hebben opgedaan. De tokens voor de bescherming van de eindpunten zijn aan toonder tokens. Deze soorten tokens zijn het meest algemeen afgegeven OAuth2. Veel implementaties wordt ervan uitgegaan dat aan toonder tokens het enige type token uitgegeven zijn.

Wijzigen vanaf de opdrachtregel de map `azuread`, als deze nog niet aanwezig.

Het paspoort installeren `passport-azure-ad` module met de volgende opdracht:

`npm install passport-azure-ad`

De uitvoer van de opdracht moet er ongeveer als deze tekst:

``
passport-azure-ad@1.0.0 node_modules/passport-azure-ad
├── xtend@4.0.0
├── xmldom@0.1.19
├── passport-http-bearer@1.0.1 (passport-strategy@1.0.0)
├── underscore@1.8.3
├── async@1.3.0
├── jsonwebtoken@5.0.2
├── xml-crypto@0.5.27 (xpath.js@1.0.6)
├── ursa@0.8.5 (bindings@1.2.1, nan@1.8.4)
├── jws@3.0.0 (jwa@1.0.1, base64url@1.0.4)
├── request@2.58.0 (caseless@0.10.0, aws-sign2@0.5.0, forever-agent@0.6.1, stringstream@0.0.4, tunnel-agent@0.4.1, oauth-sign@0.8.0, isstream@0.1.2, extend@2.0.1, json-stringify-safe@5.0.1, node-uuid@1.4.3, qs@3.1.0, combined-stream@1.0.5, mime-types@2.0.14, form-data@1.0.0-rc1, http-signature@0.11.0, bl@0.9.4, tough-cookie@2.0.0, hawk@2.3.1, har-validator@1.8.0)
└── xml2js@0.4.9 (sax@0.6.1, xmlbuilder@2.6.4)
``

## <a name="add-mongodb-modules-to-your-web-api"></a>MongoDB-modules toevoegen aan uw web-API

In dit voorbeeld gebruikt MongoDB als het gegevensarchief. Voor die Mongoose, een veelgebruikte installeren plug-in voor het beheer van modellen en schema's.

* `npm install mongoose`

## <a name="install-additional-modules"></a>Extra modules installeren

Installeer vervolgens de resterende vereiste modules.

Wijzigen vanaf de opdrachtregel de map `azuread`, als deze nog niet aanwezig:

`cd azuread`

Installeren van de modules in uw `node_modules` map:

* `npm install assert-plus`
* `npm install ejs`
* `npm install ejs-locals`
* `npm install express`
* `npm install bunyan`


## <a name="create-a-serverjs-file-with-your-dependencies"></a>Maak een server.js met de afhankelijkheden

De `server.js` -bestand bevat het merendeel van de functionaliteit voor uw server voor Web-API. 

Wijzigen vanaf de opdrachtregel de map `azuread`, als deze nog niet aanwezig:

`cd azuread`

Maak een `server.js` bestand in een editor. Voeg de volgende gegevens:

```Javascript
'use strict';
/**
* Module dependencies.
*/
var fs = require('fs');
var path = require('path');
var util = require('util');
var assert = require('assert-plus');
var mongoose = require('mongoose/');
var bunyan = require('bunyan');
var restify = require('restify');
var config = require('./config');
var passport = require('passport');
var OIDCBearerStrategy = require('passport-azure-ad').BearerStrategy;
```

Sla het bestand. U terugkeert er later naar.

## <a name="create-a-configjs-file-to-store-your-azure-ad-settings"></a>Maak een bestand config.js voor het opslaan van uw instellingen Azure AD

Dit codebestand geeft de parameters voor de configuratie van uw Azure AD-Portal voor de `Passport.js` bestand. U kunt deze configuratiewaarden gemaakt wanneer u het web API toegevoegd aan de portal in het eerste deel van het overzicht. We uitleggen wat er in de waarden van deze parameters, nadat u de code hebt gekopieerd.

Wijzigen vanaf de opdrachtregel de map `azuread`, als deze nog niet aanwezig:

`cd azuread`

Maak een `config.js` bestand in een editor. Voeg de volgende gegevens:

```Javascript
// Don't commit this file to your public repos. This config is for first-run
exports.creds = {
clientID: <your client ID for this Web API you created in the portal>
mongoose_auth_local: 'mongodb://localhost/tasklist', // Your mongo auth uri goes here
audience: '<your audience URI>', // the Client ID of the application that is calling your API, usually a web API or native client
identityMetadata: 'https://login.microsoftonline.com/<tenant name>/.well-known/openid-configuration', // Make sure you add the B2C tenant name in the <tenant name> area
tenantName:'<tenant name>', 
policyName:'b2c_1_<sign in policy name>' // This is the policy you'll want to validate against in B2C. Usually this is your Sign-in policy (as users sign in to this API)
passReqToCallback: false // This is a node.js construct that lets you pass the req all the way back to any upstream caller. We turn this off as there is no upstream caller.
};

```

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)]

### <a name="required-values"></a>Vereiste waarden

`clientID`: De client-ID van de toepassing van de Web-API.

`IdentityMetadata`: Dit is waar `passport-azure-ad` de configuratiegegevens voor de identiteitsprovider zoekt. Ook zoekt de sleutels om de JSON-web-tokens te valideren. 

`audience`: De uniform resource identifier (URI) van de portal die identificatie van de oproepende toepassing. 

`tenantName`: De naam huurder (bijvoorbeeld **contoso.onmicrosoft.com**).

`policyName`: Het beleid dat u wilt valideren de tokens die aan de server. Dit beleid moet hetzelfde beleid die u op de clienttoepassing voor het aanmelden.

> [AZURE.NOTE] Voor ons voorbeeld B2C de hetzelfde beleid te gebruiken op zowel client als server setup. Als u al een overzicht voltooid en dit beleid wordt gemaakt, hoeft u niet opnieuw doen. Omdat u het overzicht hebt voltooid, moet u niet moet nieuw beleid voor de zelfstudies client ingesteld op de site.

## <a name="add-configuration-to-your-serverjs-file"></a>Configuratie toevoegen aan het bestand server.js

Voor het lezen van de waarden van de `config.js` bestand dat u hebt gemaakt, toevoegen het `.config` -bestand als een vereiste resource in uw toepassing, en stelt u globale variabelen die in de `config.js` document.

Wijzigen vanaf de opdrachtregel de map `azuread`, als deze nog niet aanwezig:

`cd azuread`

Open de `server.js` bestand in een editor. Voeg de volgende gegevens:

```Javascript
var config = require('./config');
```
Een nieuwe sectie wilt toevoegen `server.js` die de volgende code bevat:

```Javascript
// We pass these options in to the ODICBearerStrategy.

var options = {
    // The URL of the metadata document for your app. We put the keys for token validation from the URL found in the jwks_uri tag of the in the metadata.
    identityMetadata: config.creds.identityMetadata,
    clientID: config.creds.clientID,
    tenantName: config.creds.tenantName,
    policyName: config.creds.policyName,
    validateIssuer: config.creds.validateIssuer,
    audience: config.creds.audience,
    passReqToCallback: config.creds.passReqToCallback

};
```

Vervolgens gaan we tijdelijke aanduidingen voor de gebruikers die we van onze aanroepende toepassingen ontvangen toevoegen.

```Javascript
// array to hold logged in users and the current logged in user (owner)
var users = [];
var owner = null;
```

We gaan nu en onze logger te maken.

```Javascript
// Our logger
var log = bunyan.createLogger({
    name: 'Microsoft Azure Active Directory Sample'
});
```

## <a name="add-the-mongodb-model-and-schema-information-by-using-mongoose"></a>De MongoDB model en schema-informatie toevoegen met behulp van Mongoose

De voorbereiding van de eerdere betaalt uit als u deze drie bestanden samen in een REST API service brengen.

Gebruik voor dit overzicht, MongoDB voor het opslaan van uw taken, zoals eerder is besproken.

In de `config.js` -bestand, dat u uw database **tasklist**genoemd. Die naam is ook u plaatsen aan het einde van de `mongoose_auth_local` verbindings-URL. U hoeft niet vooraf maakt van deze database in MongoDB. Dit maakt de database voor u de eerste sessie van de server-toepassing.

Nadat u de server laten welke database MongoDB weten wilt gebruiken, moet u enkele extra code schrijven om te maken van het model en het schema voor de servertaken van uw.

### <a name="expand-the-model"></a>Vouw het model

Dit model schema is eenvoudig. Indien nodig kunt u deze uitvouwen.

`owner`: Die is toegewezen aan de taak. Dit object is een **string**.  

`Text`: De taak zelf. Dit object is een **string**.

`date`: De datum waarop de taak moet voltooid zijn. Dit object is een **datum/tijd**.

`completed`: Als de taak voltooid is. Dit object is een **Boole-waarde**.

### <a name="create-the-schema-in-the-code"></a>Het schema in de code maken

Wijzigen vanaf de opdrachtregel de map `azuread`, als deze nog niet aanwezig:

`cd azuread`

Open de `server.js` bestand in een editor. De volgende informatie onder de configuratie-item toevoegen:

```Javascript
// MongoDB setup
// Setup some configuration
var serverPort = process.env.PORT || 3000; // Note we are hosting our API on port 3000
var serverURI = (process.env.PORT) ? config.creds.mongoose_auth_mongohq : config.creds.mongoose_auth_local;

// Connect to MongoDB
global.db = mongoose.connect(serverURI);
var Schema = mongoose.Schema;
log.info('MongoDB Schema loaded');

// Here we create a schema to store our tasks and users. Pretty simple schema for now.
var TaskSchema = new Schema({
    owner: String,
    Text: String,
    completed: Boolean,
    date: Date
});

// Use the schema to register a model
mongoose.model('Task', TaskSchema);
var Task = mongoose.model('Task');
```
Maakt u eerst het schema en maakt u een model-object dat u gebruikt voor het opslaan van uw gegevens overal in de code wanneer u uw **routes**definieert.

## <a name="add-routes-for-your-rest-api-task-server"></a>Routes voor de REST API taak server toevoegen

Nu hebt u een databasemodel werken met, toevoegen de routes die u kunt voor uw server REST API gebruiken.

### <a name="about-routes-in-restify"></a>Over routes in Restify

Routes werken in Restify op dezelfde manier waarop ze werken wanneer ze de stack Express gebruiken. U kunt routes definiëren met behulp van de URI die u verwacht dat de clienttoepassingen aan te roepen. 

Er is een typisch patroon voor een Restify-route:

```Javascript
function createObject(req, res, next) {
// do work on Object
_object.name = req.params.object; // passed value is in req.params under object
///...
return next(); // keep the server going
}
....
server.post('/service/:add/:object', createObject); // calls createObject on routes that match this.
```

Restify en Express kan veel diepere functionaliteit bieden, zoals het definiëren van de toepassingstypen en complexe routeren tussen verschillende eindpunten doen. Voor de toepassing van deze zelfstudie houden we deze routes eenvoudig.

#### <a name="add-default-routes-to-your-server"></a>Standaardroutes aan uw server toevoegen.

U toevoegen de elementaire CRUD routes **maken** en de **lijst** voor onze REST API. Andere routes kunnen u vinden in de `complete` tak van de steekproef.

Wijzigen vanaf de opdrachtregel de map `azuread`, als deze nog niet aanwezig:

`cd azuread`

Open de `server.js` bestand in een editor. De database-items hierboven toevoegen onder de volgende informatie:

```Javascript
/**
 *
 * APIs for our REST Task server
 */

// Create a task

function createTask(req, res, next) {

    // Resitify currently has a bug which doesn't allow you to set default headers
    // This headers comply with CORS and allow us to mongodbServer our response to any origin

    res.header("Access-Control-Allow-Origin", "*");
    res.header("Access-Control-Allow-Headers", "X-Requested-With");

    // Create a new task model, fill it up and save it to Mongodb
    var _task = new Task();

    if (!req.params.Text) {
        req.log.warn({
            params: req.params
        }, 'createTodo: missing task');
        next(new MissingTaskError());
        return;
    }

    _task.owner = owner;
    _task.Text = req.params.Text;
    _task.date = new Date();

    _task.save(function(err) {
        if (err) {
            req.log.warn(err, 'createTask: unable to save');
            next(err);
        } else {
            res.send(201, _task);

        }
    });

    return next();

}
```

```Javascript
/// Simple returns the list of TODOs that were loaded.

function listTasks(req, res, next) {
    // Resitify currently has a bug which doesn't allow you to set default headers
    // This headers comply with CORS and allow us to mongodbServer our response to any origin

    res.header("Access-Control-Allow-Origin", "*");
    res.header("Access-Control-Allow-Headers", "X-Requested-With");

    log.info("listTasks was called for: ", owner);

    Task.find({
        owner: owner
    }).limit(20).sort('date').exec(function(err, data) {

        if (err)
            return next(err);

        if (data.length > 0) {
            log.info(data);
        }

        if (!data.length) {
            log.warn(err, "There is no tasks in the database. Add one!");
        }

        if (!owner) {
            log.warn(err, "You did not pass an owner when listing tasks.");
        } else {

            res.json(data);

        }
    });

    return next();
}
```


#### <a name="add-error-handling-for-the-routes"></a>Voor de routes voor foutafhandeling toevoegen

Voeg enkele fouten afhandelen zodat u eventuele problemen kunt communiceren terug naar de client op een manier die kan worden herkend.

Voeg de volgende code toe:

```Javascript
///--- Errors for communicating something interesting back to the client
function MissingTaskError() {
restify.RestError.call(this, {
statusCode: 409,
restCode: 'MissingTask',
message: '"task" is a required parameter',
constructorOpt: MissingTaskError
});
this.name = 'MissingTaskError';
}
util.inherits(MissingTaskError, restify.RestError);
function TaskExistsError(owner) {
assert.string(owner, 'owner');
restify.RestError.call(this, {
statusCode: 409,
restCode: 'TaskExists',
message: owner + ' already exists',
constructorOpt: TaskExistsError
});
this.name = 'TaskExistsError';
}
util.inherits(TaskExistsError, restify.RestError);
function TaskNotFoundError(owner) {
assert.string(owner, 'owner');
restify.RestError.call(this, {
statusCode: 404,
restCode: 'TaskNotFound',
message: owner + ' was not found',
constructorOpt: TaskNotFoundError
});
this.name = 'TaskNotFoundError';
}
util.inherits(TaskNotFoundError, restify.RestError);
```


## <a name="create-your-server"></a>Uw server maken

U hebt nu uw database gedefinieerd en leggen de routes. Het laatste wat u moet doen is het toevoegen van het serverexemplaar dat uw gesprekken beheert.

Restify en Express bieden een grondige aanpassing voor een server met REST API, maar gebruiken we de meest elementaire instellingen hier. 

```Javascript

**
 * Our Server
 */


var server = restify.createServer({
    name: "Microsoft Azure Active Directroy TODO Server",
    version: "2.0.1"
});

// Ensure we don't drop data on uploads
server.pre(restify.pre.pause());

// Clean up sloppy paths like //todo//////1//
server.pre(restify.pre.sanitizePath());

// Handles annoying user agents (curl)
server.pre(restify.pre.userAgentConnection());

// Set a per request bunyan logger (with requestid filled in)
server.use(restify.requestLogger());

// Allow 5 requests/second by IP, and burst to 10
server.use(restify.throttle({
    burst: 10,
    rate: 5,
    ip: true,
}));

// Use the common stuff you probably want
server.use(restify.acceptParser(server.acceptable));
server.use(restify.dateParser());
server.use(restify.queryParser());
server.use(restify.gzipResponse());
server.use(restify.bodyParser({
    mapParams: true
})); // Allows for JSON mapping to REST
server.use(restify.authorizationParser()); // Looks for authorization headers

// Let's start using Passport.js

server.use(passport.initialize()); // Starts passport
server.use(passport.session()); // Provides session support


```
## <a name="add-the-routes-to-the-server-without-authentication"></a>De routes toevoegen aan de server (zonder verificatie)

```Javascript
server.get('/api/tasks', passport.authenticate('oauth-bearer', {
    session: false
}), listTasks);
server.get('/api/tasks', passport.authenticate('oauth-bearer', {
    session: false
}), listTasks);
server.get('/api/tasks/:owner', passport.authenticate('oauth-bearer', {
    session: false
}), getTask);
server.head('/api/tasks/:owner', passport.authenticate('oauth-bearer', {
    session: false
}), getTask);
server.post('/api/tasks/:owner/:task', passport.authenticate('oauth-bearer', {
    session: false
}), createTask);
server.post('/api/tasks', passport.authenticate('oauth-bearer', {
    session: false
}), createTask);
server.del('/api/tasks/:owner/:task', passport.authenticate('oauth-bearer', {
    session: false
}), removeTask);
server.del('/api/tasks/:owner', passport.authenticate('oauth-bearer', {
    session: false
}), removeTask);
server.del('/api/tasks', passport.authenticate('oauth-bearer', {
    session: false
}), removeTask);
server.del('/api/tasks', passport.authenticate('oauth-bearer', {
    session: false
}), removeAll, function respond(req, res, next) {
    res.send(204);
    next();
});


// Register a default '/' handler

server.get('/', function root(req, res, next) {
    var routes = [
        'GET     /',
        'POST    /api/tasks/:owner/:task',
        'POST    /api/tasks (for JSON body)',
        'GET     /api/tasks',
        'PUT     /api/tasks/:owner',
        'GET     /api/tasks/:owner',
        'DELETE  /api/tasks/:owner/:task'
    ];
    res.send(200, routes);
    next();
});
```

```Javascript

server.listen(serverPort, function() {

    var consoleMessage = '\n Microsoft Azure Active Directory Tutorial';
    consoleMessage += '\n +++++++++++++++++++++++++++++++++++++++++++++++++++++';
    consoleMessage += '\n %s server is listening at %s';
    consoleMessage += '\n Open your browser to %s/api/tasks\n';
    consoleMessage += '+++++++++++++++++++++++++++++++++++++++++++++++++++++ \n';
    consoleMessage += '\n !!! why not try a $curl -isS %s | json to get some ideas? \n';
    consoleMessage += '+++++++++++++++++++++++++++++++++++++++++++++++++++++ \n\n';

    //log.info(consoleMessage, server.name, server.url, server.url, server.url);

});

``` 

## <a name="add-authentication-to-your-rest-api-server"></a>Verificatie met de REST API-server toevoegen

Nu dat er een actieve REST API-server, kunt u deze tegen Azure AD nuttig maken.

Wijzigen vanaf de opdrachtregel de map `azuread`, als deze nog niet aanwezig:

`cd azuread`

### <a name="use-the-oidcbearerstrategy-that-is-included-with-passport-azure-ad"></a>Gebruik de OIDCBearerStrategy die is opgenomen in een passport-azure-ad


> [AZURE.TIP]
Wanneer u API's schrijft, moet u altijd de gegevens koppelen aan een unieke van het token dat door de gebruiker kan niet vervalsen. Wanneer de ToDo-items op de server opgeslagen, gebeurt dus op basis van de **oid** van de gebruiker in het token (de zogenaamde via token.oid), dat in het veld 'eigenaar'. Deze waarde zorgt ervoor dat alleen de gebruiker die toegang hebben tot hun eigen ToDo-items. Er is geen blootstelling in de API van 'eigenaar', zodat een externe gebruiker anderen ToDo items aanvragen kan, zelfs als deze zijn geverifieerd.

Gebruik vervolgens de strategie aan toonder die wordt geleverd met `passport-azure-ad`.

```Javascript
var findById = function(id, fn) {
    for (var i = 0, len = users.length; i < len; i++) {
        var user = users[i];
        if (user.oid === id) {
            log.info('Found user: ', user);
            return fn(null, user);
        }
    }
    return fn(null, null);
};


var oidcStrategy = new OIDCBearerStrategy(options,
    function(token, done) {
        log.info('verifying the user');
        log.info(token, 'was the token retreived');
        findById(token.sub, function(err, user) {
            if (err) {
                return done(err);
            }
            if (!user) {
                // "Auto-registration"
                log.info('User was added automatically as they were new. Their sub is: ', token.oid);
                users.push(token);
                owner = token.oid;
                return done(null, token);
            }
            owner = token.sub;
            return done(null, user, token);
        });
    }
);

passport.use(oidcStrategy);
```

Hetzelfde patroon Passport gebruikt voor alle bijbehorende strategieën. Als u gebruikmaakt van een `function()` die is `token` en `done` als parameters. De strategie komt naar u terug nadat het al het werk heeft gedaan. Vervolgens de gebruiker worden opgeslagen en u de token opslaan zodat u niet hoeft te vragen om het opnieuw.

> [AZURE.IMPORTANT]
De bovenstaande code wordt elke gebruiker die gebeurt om te verifiëren met de server. Dit proces staat bekend als autoregistration. In productieservers, laat niet in alle gebruikers toegang tot de API zonder eerst deze registratieprocedure doorlopen. Dit is meestal het patroon dat u in de consument apps waarmee u ziet te registreren via Facebook maar vraagt u om extra informatie in te vullen. Als dit programma is niet een programma vanaf de opdrachtregel, kan hebben we het e-mailbericht afkomstig uit het token-object dat wordt geretourneerd en wordt vervolgens gevraagd de gebruikers om aanvullende informatie in te vullen. Dit is een monster, toevoegen we deze aan een database in het geheugen.



## <a name="run-your-server-application-to-verify-that-it-rejects-you"></a>De servertoepassing wordt uitgevoerd om te controleren of deze weigert u

U kunt `curl` om te controleren of u nu OAuth2 bescherming tegen de eindpunten. De headers teruggestuurd moet genoeg om aan te geven dat u op het juiste pad zijn.

Zorg ervoor dat uw MongoDB is gestart:

    $sudo mongodb

Ga naar de map en start de server:

    $ cd azuread
    $ node server.js

Uitgevoerd in een nieuw terminal venster`curl`

Probeer een eenvoudige boeken:

`$ curl -isS -X POST http://127.0.0.1:3000/api/tasks/brandon/Hello`

```Shell
HTTP/1.1 401 Unauthorized
Connection: close
WWW-Authenticate: Bearer realm="Users"
Date: Tue, 14 Jul 2015 05:45:03 GMT
Transfer-Encoding: chunked
```

Een 401-fout is het antwoord dat u wilt. Hiermee wordt aangegeven dat de laag Passport probeert om te leiden naar het eindpunt van de machtigen.


## <a name="you-now-have-a-rest-api-service-that-uses-oauth2"></a>U hebt nu een REST API service die gebruikmaakt van OAuth2

U hebt een REST API geïmplementeerd met behulp van Restify en OAuth! U hebt nu voldoende code zodat u kunt doorgaan met uw service te ontwikkelen en bouwen op dit voorbeeld. U hebt nu zover u met deze server kunt zonder gebruik van een OAuth2-compatibele client. Gebruik een extra overzicht zoals onze [verbinding maken met een web API met behulp van iOS met B2C](active-directory-b2c-devquickstarts-ios.md) -scenario voor die stap.


## <a name="next-steps"></a>Volgende stappen

U kunt nu verplaatsen naar meer geavanceerde onderwerpen, zoals:

[Verbinding maken met een web API via iOS met B2C](active-directory-b2c-devquickstarts-ios.md)