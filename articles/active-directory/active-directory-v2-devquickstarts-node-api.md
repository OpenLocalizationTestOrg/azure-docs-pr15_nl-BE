<properties
    pageTitle="Azure AD v2.0 NodeJS Web API | Microsoft Azure"
    description="Het bouwen van een Web-API NodeJS accepteert tokens van beide persoonlijke Microsoft-Account en rekeningen voor werk of school."
    services="active-directory"
    documentationCenter="nodejs"
    authors="brandwe"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="javascript"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="brandwe"/>

# <a name="secure-a-web-api-using-nodejs"></a>Een Web API met node.js beveiligen

> [AZURE.NOTE]
    Niet alle scenario's Azure Active Directory en functies worden ondersteund door het eindpunt v2.0.  Om te bepalen als u het eindpunt v2.0 moet gebruiken, Lees over [v2.0 beperkingen](active-directory-v2-limitations.md).

Kunt u een Web-API gebruikt [OAuth 2.0](active-directory-v2-protocols.md#oauth2-authorization-code-flow) toegangstokens, zodat gebruikers met persoonlijke Microsoft-account en werk beschermen met Azure Active Directory het eindpunt v2.0 of school rekeningen voor veilig toegang tot uw Web-API.

**Passport** is verificatie middleware voor Node.js. Zeer flexibel en modulair, Passport kan onopvallend wegvallen voor elk Express- of Resitify van de webtoepassing. Een uitgebreide set van strategieën voor ondersteuning van verificatie met behulp van een gebruikersnaam en wachtwoord, Facebook en Twitter. We hebben een strategie ontwikkeld voor Microsoft Azure Active Directory. We in deze module te installeren en vervolgens Microsoft Azure Active Directory toevoegen `passport-azure-ad` plug-in.

## <a name="download"></a>Downloaden
De code voor deze zelfstudie wordt onderhouden [op GitHub](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-nodejs).  Als u wilt volgen, kunt u [downloaden van de app skelet als een ZIP-](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-nodejs/archive/skeleton.zip) of het skelet klonen:

```git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-nodejs.git```

De voltooide toepassing wordt geleverd aan het einde van deze zelfstudie ook.


## <a name="1-register-an-app"></a>1. een app registreren
Maak een nieuwe app op [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)of Volg deze [gedetailleerde stappen](active-directory-v2-app-registration.md).  Zorg ervoor dat:

- Kopiëren naar beneden de **Toepassings-Id** toegewezen aan uw app, moet u deze snel.
- Het **mobiele** platform voor uw app toevoegen.
- Kopiëren naar de **URI omgeleid** via de portal. Moet u de standaardwaarde van `urn:ietf:wg:oauth:2.0:oob`.


## <a name="2-download-nodejs-for-your-platform"></a>2: node.js voor uw platform downloaden
In dit voorbeeld is gebruikt, hebt u een werkende installatie van Node.js.

Node.js installeren vanaf [http://nodejs.org](http://nodejs.org).

## <a name="3-install-mongodb-on-to-your-platform"></a>3: MongoDB installeren op uw platform

In dit voorbeeld is gebruikt, hebt u een werkende installatie van MongoDB. MongoDB gebruiken we onze persistant REST API om verschillende exemplaren van de server.

MongoDB installeren vanaf [http://mongodb.org](http://www.mongodb.org).

> [AZURE.NOTE] In dit scenario wordt ervan uitgegaan dat u de standaard installatie- en eindpunten voor MongoDB, namelijk op het moment van schrijven van dit: mongodb://localhost

## <a name="4-install-the-restify-modules-in-to-your-web-api"></a>4: de Restify-modules in installeren op uw Web-API

We gebruiken Resitfy bouwen onze REST API. Restify afkomstig is van een minimale en flexibele Node.js application framework Express met een robuuste set van functies voor het maken van REST API's op verbinding maken.

### <a name="install-restify"></a>Restify installeren

Wijzigen vanaf de opdrachtregel mappen naar de map azuread. Als de map **azuread** niet bestaat, maken.

`cd azuread`- of -`mkdir azuread;`

Typ de volgende opdracht:

`npm install restify`

Met deze opdracht installeert Restify.

#### <a name="did-you-get-an-error"></a>Kom je een fout?

Wanneer u npm op enkele besturingssystemen gebruikt, kan er een foutbericht fout: EPERM, type chmod '/ usr/lokale/bin /..' en een verzoek om te proberen de account uitvoeren als beheerder. Als dit gebeurt, gebruikt u de opdracht sudo npm op een hoger bevoegdhedenniveau wordt uitgevoerd.

#### <a name="did-you-get-an-error-regarding-dtrace"></a>Hebt u een fout met betrekking tot DTrace hier?

U ziet iets dergelijks bij het installeren van Restify:

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


Restify biedt een krachtig mechanisme voor de REST traceren met behulp van DTrace. Echter, veel besturingssystemen geen DTrace beschikbaar. U kunt deze fouten negeren.


De uitvoer van deze opdracht ziet er ongeveer als volgt:


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
    └── bunyan@0.22.0(mv@0.0.5)


## <a name="5-install-passportjs-into-your-web-api"></a>5: Passport.js installeren in uw Web-API

[Passport](http://passportjs.org/) is verificatie middleware voor Node.js. Zeer flexibel en modulair, Passport kan onopvallend wegvallen voor elk Express- of Resitify van de webtoepassing. Een uitgebreide set van strategieën voor ondersteuning van verificatie met behulp van een gebruikersnaam en wachtwoord, Facebook en Twitter. We hebben een strategie ontwikkeld voor Azure Active Directory. Wij installeren deze module en voeg vervolgens de invoegtoepassing Azure Active Directory-strategie.

Wijzigen vanaf de opdrachtregel mappen naar de map azuread.

Voer de volgende opdracht installeren passport.js

`npm install passport`

De uitvoer van de opdracht ziet er ongeveer als volgt:

    passport@0.1.17 node_modules\passport
    ├── pause@0.0.1
    └── pkginfo@0.2.3

## <a name="6-add-passport-azure-ad-to-your-web-api"></a>6: Passport-Azure-advertentie toevoegen aan uw Web-API

Vervolgens gaat toevoegen we de strategie OAuth met passport-azuread, een suite van strategieën die Azure Active Directory met Passport-account verbinden. We gebruiken deze strategie voor Tokens aan toonder in dit voorbeeld Rest API.

> [AZURE.NOTE] Maar OAuth2 een kader waarin een onbekend type token kan worden uitgegeven biedt, kunnen alleen bepaalde token typen wide spread gebruik hebben opgedaan. Voor de beveiliging van eindpunten, die heeft bleek aan toonder Tokens. Aan toonder tokens zijn het meest algemeen type token in OAuth2 uitgegeven en veel implementaties wordt ervan uitgegaan dat aan toonder tokens het enige type token uitgegeven zijn.

Vanaf de opdrachtregel wijzigen in mappen de map azuread

Typ de volgende opdracht om een Passport.js passport-azure-ad module te installeren:

`npm install passport-azure-ad`

De uitvoer van de opdracht ziet er ongeveer als volgt:

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

## <a name="7-add-mongodb-modules-to-your-web-api"></a>7: MongoDB-modules toevoegen aan uw Web-API

We gebruiken MongoDB als onze datastore om die reden, moeten we de installatie van zowel de gebruikte invoegtoepassing voor het beheren van modellen en schema's genaamd Mongoose als het stuurprogramma voor MongoDB, ook wel MongoDB.


* `npm install mongoose`
* `npm install mongodb`

## <a name="8-install-additional-modules"></a>8: aanvullende modules installeren

Vervolgens zullen we de resterende vereiste modules installeren.


Wijzig mappen naar de map **azuread** als dit niet al er vanaf de opdrachtregel:

`cd azuread`


Voer de volgende opdrachten de volgende modules te installeren in de map node_modules:

* `npm install crypto`
* `npm install assert-plus`
* `npm install posix-getopt`
* `npm install util`
* `npm install path`
* `npm install connect`
* `npm install xml-crypto`
* `npm install xml2js`
* `npm install xmldom`
* `npm install async`
* `npm install request`
* `npm install underscore`
* `npm install grunt-contrib-jshint@0.1.1`
* `npm install grunt-contrib-nodeunit@0.1.2`
* `npm install grunt-contrib-watch@0.2.0`
* `npm install grunt@0.4.1`
* `npm install xtend@2.0.3`
* `npm install bunyan`
* `npm update`


## <a name="9-create-a-serverjs-with-your-dependencies"></a>9: een server.js maken met de afhankelijkheden

Het bestand server.js wordt het merendeel van onze functionaliteit bieden voor onze server Web-API. We wilt de meeste van onze code toevoegen aan dit bestand. U zou de functionaliteit in kleinere bestanden, zoals verschillende routes en controllers refactoring voor productiedoeleinden. Voor de toepassing van deze demo gebruiken we server.js voor deze functionaliteit.

Wijzig mappen naar de map **azuread** als dit niet al er vanaf de opdrachtregel:

`cd azuread`

Maak een `server.js` bestand in onze favoriete editor en voeg de volgende gegevens:

```Javascript
'use strict';
/**
* Module dependencies.
*/
var util = require('util');
var assert = require('assert-plus');
var mongoose = require('mongoose/');
var bunyan = require('bunyan');
var restify = require('restify');
var config = require('./config');
var passport = require('passport');
var OIDCBearerStrategy = require('passport-azure-ad').OIDCStrategy;
```

Sla het bestand. Wij zijn terug te kort.

## <a name="10-create-a-config-file-to-store-your-azure-ad-settings"></a>10: maken van een configuratiebestand voor het opslaan van uw instellingen Azure AD

Dit codebestand geeft de parameters voor de configuratie van uw Azure Active Directory Portal aan Passport.js. U kunt deze configuratiewaarden gemaakt wanneer u de Web-API toegevoegd aan de portal in het eerste deel van de procedure. Wordt uitgelegd wat er in de waarden van deze parameters, nadat u de code hebt gekopieerd.


Wijzig mappen naar de map **azuread** als dit niet al er vanaf de opdrachtregel:

`cd azuread`

Maak een `config.js` bestand in onze favoriete editor en voeg de volgende gegevens:

```Javascript
// Don't commit this file to your public repos. This config is for first-run
exports.creds = {
mongoose_auth_local: 'mongodb://localhost/tasklist', // Your mongo auth uri goes here
issuer: 'https://sts.windows.net/**<your application id>**/',
audience: '<your redirect URI>',
identityMetadata: 'https://login.microsoftonline.com/common/.well-known/openid-configuration' // For using Microsoft you should never need to change this.
};

```



### <a name="required-values"></a>Vereiste waarden

*IdentityMetadata*: dit is waar passport-azure-ad zoekt de configuratiegegevens voor de IdP, alsmede de sleutels om de JWT tokens te valideren. U wilt waarschijnlijk niet als met Azure Active Directory wijzigen.

*doelgroep*: het omleiden van URI van de portal.

> [AZURE.NOTE]
We rollen onze sleutels met regelmatige tussenpozen. Zorg ervoor dat u altijd van de URL 'openid_keys' zijn trekken en dat de app toegang krijgen het internet tot.


## <a name="11-add-configuration-to-your-serverjs-file"></a>11: configuratie toevoegen aan het bestand server.js

We moeten deze waarden in het Config-bestand dat u zojuist hebt gemaakt in onze toepassing lezen. Dit kunt u doen we gewoon het .config-bestand toevoegen als een vereiste resource in onze toepassing en stelt u globale variabelen die in het document config.js

Wijzig mappen naar de map **azuread** als dit niet al er vanaf de opdrachtregel:

`cd azuread`

Open uw `server.js` bestand in onze favoriete editor en voeg de volgende gegevens:

```Javascript
var config = require('./config');
```
Voegt een nieuwe sectie op `server.js` met de volgende code:

```Javascript
// We pass these options in to the ODICBearerStrategy.
var options = {
// The URL of the metadata document for your app. We will put the keys for token validation from the URL found in the jwks_uri tag of the in the metadata.
identityMetadata: config.creds.identityMetadata,
issuer: config.creds.issuer,
audience: config.creds.audience
};
// array to hold logged in users and the current logged in user (owner)
var users = [];
var owner = null;
// Our logger
var log = bunyan.createLogger({
name: 'Microsoft Azure Active Directory Sample'
});
```

## <a name="step-12-add-the-mongodb-model-and-schema-information-using-moongoose"></a>Stap 12: De MongoDB-Model en Moongoose met Schema-informatie toevoegen

Alle deze voorbereiding zal nu gaan betalen uit als we liquidatie deze drie bestanden samen een REST API service.

Voor dit scenario zullen we MongoDB gebruiken voor het opslaan van onze taken zoals beschreven in ***stap 4***.

Als u vanuit de config.js-bestand die in stap 11 is gemaakt, we onze database *tasklist*genoemd als dat is wat we aan het einde van onze mogoose_auth_local verbindings-URL opnemen. U hoeft niet vooraf maken van deze database in MongoDB, het maakt dit voor ons eerste uitvoering van onze server-toepassing (als deze nog niet bestaat).

Nu dat we hebben u de server verteld welke MongoDB database willen we gebruiken, moeten we enkele extra code schrijven om te maken van het model en het schema voor taken van onze server.

#### <a name="discussion-of-the-model"></a>Bespreking van het model

Ons Schema model is zeer eenvoudig en daarop waar nodig.

NAME - de naam van die aan de taak is toegewezen. Een ***tekenreeks***

TAAK - de taak zelf. Een ***tekenreeks***

-De datum waarop de taak vervalt. Een ***datum/tijd***

INGEVULD - als de taak is voltooid of niet. Een ***Boole-waarde***

#### <a name="creating-the-schema-in-the-code"></a>Een schema maken in de code


Wijzig mappen naar de map **azuread** als dit niet al er vanaf de opdrachtregel:

`cd azuread`

Open uw `server.js` bestand in onze favoriete editor en voeg de volgende informatie onder de configuratie-item:

```Javascript
// MongoDB setup
// Setup some configuration
var serverPort = process.env.PORT || 8080;
var serverURI = (process.env.PORT) ? config.creds.mongoose_auth_mongohq : config.creds.mongoose_auth_local;
// Connect to MongoDB
global.db = mongoose.connect(serverURI);
var Schema = mongoose.Schema;
log.info('MongoDB Schema loaded');
```
Hiermee verbinding maken met de server MongoDB en opnieuw in handen een schemaobject aan ons.

#### <a name="using-the-schema-create-our-model-in-the-code"></a>Maak ons model met behulp van het Schema in de code

Hieronder de code die u hierboven hebt geschreven, voeg de volgende code:

```Javascript
// Here we create a schema to store our tasks and users. Pretty simple schema for now.
var TaskSchema = new Schema({
owner: String,
task: String,
completed: Boolean,
date: Date
});
// Use the schema to register a model
mongoose.model('Task', TaskSchema);
var Task = mongoose.model('Task');
```
Zoals u in de code ziet, wij maken ons Schema en maakt u een model-object we onze gegevens overal in de code opslaan gebruiken wanneer we onze ***Routes***definieert.

## <a name="step-13-add-our-routes-for-our-task-rest-api-server"></a>Stap 13: Onze Routes voor onze taak REST API-server toevoegen

Nu hebben we een databasemodel werken met, laten we toevoegen de routes die we voor onze server REST API gebruiken.

### <a name="about-routes-in-restify"></a>Over Routes in Restify

Routes zijn in exact dezelfde manier dan met behulp van de Express-stack in Restify werken. U definieert de URI die u verwacht dat de clienttoepassingen aan te roepen met routes. U definieert de routes meestal in een apart bestand. Voor onze doeleinden, zullen we onze routes plaatsen in het bestand server.js. Wij raden u aan dat deze factor in hun eigen bestand voor gebruik in productie.

Er is een typisch patroon voor een Restify-Route:

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


Dit is het patroon op het meest elementaire niveau. Resitfy (en Express) bieden veel diepere functionaltiy zoals toepassingstypen definiëren en complexe routeren tussen verschillende eindpunten te doen. Voor onze doeleinden blijven we deze routes erg eenvoudig.

#### <a name="add-default-routes-to-our-server"></a>Standaardroutes naar onze server toevoegen

We nu toevoegen basic CRUD routes van het maken, ophalen, bijwerken en verwijderen.

Wijzig mappen naar de map **azuread** als dit niet al er vanaf de opdrachtregel:

`cd azuread`

Open uw `server.js` bestand in onze favoriete editor en voeg de volgende informatie onder de database-items hierboven:

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
if (!req.params.task) {
req.log.warn({
params: p
}, 'createTodo: missing task');
next(new MissingTaskError());
return;
}
_task.owner = owner;
_task.task = req.params.task;
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
// Delete a task by name
function removeTask(req, res, next) {
Task.remove({
task: req.params.task,
owner: owner
}, function(err) {
if (err) {
req.log.warn(err,
'removeTask: unable to delete %s',
req.params.task);
next(err);
} else {
log.info('Deleted task:', req.params.task);
res.send(204);
next();
}
});
}
// Delete all tasks
function removeAll(req, res, next) {
Task.remove();
res.send(204);
return next();
}
// Get a specific task based on name
function getTask(req, res, next) {
log.info('getTask was called for: ', owner);
Task.find({
owner: owner
}, function(err, data) {
if (err) {
req.log.warn(err, 'get: unable to read %s', owner);
next(err);
return;
}
res.json(data);
});
return next();
}
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

### <a name="add-some-error-handling-for-the-routes"></a>Sommige voor de routes voor foutafhandeling toevoegen

Is het zinvol om toe te voegen sommige fouten afhandelen zodat we kunnen communiceren om het probleem dat er is opgetreden op een manier terug naar de client deze kan begrijpen.

Voeg de volgende code onder de code die u hierboven hebt geschreven:

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


## <a name="step-14-create-your-server"></a>Stap 14: Maak uw Server!

We hebben onze database gedefinieerd, hebben we onze routes in de plaats en het laatste wat te doen is onze server-exemplaar waarmee onze gesprekken toevoegen.

Restify (en Express) diep aanpassingen kunt u een server API REST doen veel, maar nogmaals we de meest eenvoudige instelling gebruiken voor onze doelstellingen.

```Javascript
/**
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
}));
```
## <a name="15-adding-the-routes-without-authentication-for-now"></a>15: de routes (zonder verificatie nu) toe te voegen

```Javascript
/// Now the real handlers. Here we just CRUD
/**
/*
/* Each of these handlers are protected by our OIDCBearerStrategy by invoking 'oidc-bearer'
/* in the pasport.authenticate() method. We set 'session: false' as REST is stateless and
/* we don't need to maintain session state. You can experiement removing API protection
/* by removing the passport.authenticate() method like so:
/*
/* server.get('/tasks', listTasks);
/*
**/
server.get('/tasks', listTasks);
server.get('/tasks', listTasks);
server.get('/tasks/:owner', getTask);
server.head('/tasks/:owner', getTask);
server.post('/tasks/:owner/:task', createTask);
server.post('/tasks', createTask);
server.del('/tasks/:owner/:task', removeTask);
server.del('/tasks/:owner', removeTask);
server.del('/tasks', removeTask);
server.del('/tasks', removeAll, function respond(req, res, next) {
res.send(204);
next();
});
// Register a default '/' handler
server.get('/', function root(req, res, next) {
var routes = [
'GET /',
'POST /tasks/:owner/:task',
'POST /tasks (for JSON body)',
'GET /tasks',
'PUT /tasks/:owner',
'GET /tasks/:owner',
'DELETE /tasks/:owner/:task'
];
res.send(200, routes);
next();
});
server.listen(serverPort, function() {
var consoleMessage = '\n Microsoft Azure Active Directory Tutorial';
consoleMessage += '\n +++++++++++++++++++++++++++++++++++++++++++++++++++++';
consoleMessage += '\n %s server is listening at %s';
consoleMessage += '\n Open your browser to %s/tasks\n';
consoleMessage += '+++++++++++++++++++++++++++++++++++++++++++++++++++++ \n';
consoleMessage += '\n !!! why not try a $curl -isS %s | json to get some ideas? \n';
consoleMessage += '+++++++++++++++++++++++++++++++++++++++++++++++++++++ \n\n';
});
```
## <a name="16-before-we-add-oauth-support-lets-run-the-server"></a>16: voordat we OAuth ondersteuning toe te voegen, laten we de server uitvoeren.

Testen van de server voordat we verificatie toe te voegen

De eenvoudigste manier om dit te doen is met behulp van de krul in een opdrachtregel. Voordat we dat doen, moeten we een eenvoudig hulpprogramma waarmee wij de uitvoer als JSON parseren. Hiertoe door het hulpprogramma json te installeren, zoals de onderstaande voorbeelden die gebruiken.

`$npm install -g jsontool`

Dit wordt het hulpprogramma JSON globaal geïnstalleerd. Nu dat we die bereikt – laten we spelen met de server:

Controleer eerst of uw isntance monogoDB actief is...

`$sudo mongod`

Vervolgens Ga naar de map en start curling...

`$ cd azuread`
`$ node server.js`

`$ curl -isS http://127.0.0.1:8080 | json`

```Shell
HTTP/1.1 2.0OK
Connection: close
Content-Type: application/json
Content-Length: 171
Date: Tue, 14 Jul 2015 05:43:38 GMT
[
"GET /",
"POST /tasks/:owner/:task",
"POST /tasks (for JSON body)",
"GET /tasks",
"PUT /tasks/:owner",
"GET /tasks/:owner",
"DELETE /tasks/:owner/:task"
]
```

Vervolgens, we kunnen een taak toevoegen op deze manier:

`$ curl -isS -X POST http://127.0.0.1:8888/tasks/brandon/Hello`

Het antwoord moet zijn:

```Shell
HTTP/1.1 201 Created
Connection: close
Access-Control-Allow-Origin: *
Access-Control-Allow-Headers: X-Requested-With
Content-Type: application/x-www-form-urlencoded
Content-Length: 5
Date: Tue, 04 Feb 2014 01:02:26 GMT
Hello
```
En we kunnen een lijst met taken voor Brandon op deze manier:

`$ curl -isS http://127.0.0.1:8080/tasks/brandon/`

Als dit werkt, zijn we OAuth aan de REST API-server toevoegen.

**U hebt een server REST API met MongoDB!**

## <a name="17-add-authentication-to-our-rest-api-server"></a>17: verificatie met onze REST API-Server toevoegen

Nu dat we een actieve REST API hebben (Gefeliciteerd, btw!) laten we gaan naar nuttige tegen AD Azure.

Wijzig mappen naar de map **azuread** als dit niet al er vanaf de opdrachtregel:

`cd azuread`

### <a name="1-use-the-oidcbearerstrategy-that-is-included-with-passport-azure-ad"></a>1: Gebruik de oidcbearerstrategy die is opgenomen in een passport-azure-ad

Tot nu toe hebben we een typische REST TODO server zonder vergunning gebouwd. Dit is waar we beginnen het samenstellen.

Eerst moet aangeven dat we Passport gebruiken. Plaats dit recht na de serverconfiguratie:

```Javascript
// Let's start using Passport.js

server.use(passport.initialize()); // Starts passport
server.use(passport.session()); // Provides session support
```

> [AZURE.TIP]
Bij het schrijven van API's moet u altijd de gegevens koppelen aan een unieke van het token dat door de gebruiker kan niet vervalsen. Als deze server TODO items opgeslagen, slaat ze op basis van de abonnement-ID van de gebruiker in het token (de zogenaamde via token.sub) die we in het veld 'eigenaar plaatsen'. Dit zorgt ervoor dat alleen die gebruiker toegang heeft tot zijn TODOs en niemand anders toegang de TODOs ingevoerd tot. Er is geen blootstelling in de API van de "eigenaar" van een externe gebruiker van andere TODOs kunt aanvragen, zelfs als deze zijn geverifieerd.

Vervolgens gebruiken we de strategie van Open-ID koppelen aan toonder die wordt geleverd met passport-azure-ad. Kijk eens naar de code nu, leg ik uit dat binnenkort. Plaats dit na wat u pated hierboven:

```Javascript
/**
/*
/* Calling the OIDCBearerStrategy and managing users
/*
/* Passport pattern provides the need to manage users and info tokens
/* with a FindorCreate() method that must be provided by the implementor.
/* Here we just autoregister any user and implement a FindById().
/* You'll want to do something smarter.
**/
var findById = function(id, fn) {
for (var i = 0, len = users.length; i < len; i++) {
var user = users[i];
if (user.sub === id) {
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
log.info('User was added automatically as they were new. Their sub is: ', token.sub);
users.push(token);
owner = token.sub;
return done(null, token);
}
owner = token.sub;
return done(null, user, token);
});
}
);
passport.use(oidcStrategy);
```

Een soortgelijk patroon Passport gebruikt voor alle die strategieën (Twitter, Facebook, enz.) die voor alle schrijvers van de strategie worden aangehouden is. Kijken naar de strategie er geven we een function() met een token en een gereed als parameters. De strategie zal plichtsgetrouw terugkomen naar ons zodra het doet al het werk. Als dat zo is willen we voor het opslaan van de gebruiker en het token niet initialiseren zodat we hoeft aan te vragen om het opnieuw.

> [AZURE.IMPORTANT]
De bovenstaande code wordt elke gebruiker die met onze server te verifiëren. Dit staat bekend als de automatische registratie. Productieservers wilt u wouldn't laat iedereen zonder eerste ze via een registratieproces die u beslist. Dit is meestal het patroon dat u ziet in de consument apps die u kunnen u aanmelden bij Facebook maar vraagt u om extra informatie in te vullen. Als dit niet een programma vanaf de opdrachtregel, kan hebben we alleen het e-mailbericht uitgepakt uit het token-object dat wordt geretourneerd en wordt vervolgens gevraagd om aanvullende informatie invullen. Aangezien dit een testserver we gewoon de in-memory database toevoegen.

### <a name="2-finally-protect-some-endpoints"></a>2. tot slot enkele eindpunten te beschermen

U beschermen eindpunten door op te geven van de aanroep van passport.authenticate() met het protocol dat u wilt gebruiken.

Laten we onze route in onze servercode iets interessanter te doen bewerken:

```Javascript
server.get('/tasks', passport.authenticate('oidc-bearer', {
session: false
}), listTasks);
server.get('/tasks', passport.authenticate('oidc-bearer', {
session: false
}), listTasks);
server.get('/tasks/:owner', passport.authenticate('oidc-bearer', {
session: false
}), getTask);
server.head('/tasks/:owner', passport.authenticate('oidc-bearer', {
session: false
}), getTask);
server.post('/tasks/:owner/:task', passport.authenticate('oidc-bearer', {
session: false
}), createTask);
server.post('/tasks', passport.authenticate('oidc-bearer', {
session: false
}), createTask);
server.del('/tasks/:owner/:task', passport.authenticate('oidc-bearer', {
session: false
}), removeTask);
server.del('/tasks/:owner', passport.authenticate('oidc-bearer', {
session: false
}), removeTask);
server.del('/tasks', passport.authenticate('oidc-bearer', {
session: false
}), removeTask);
server.del('/tasks', passport.authenticate('oidc-bearer', {
session: false
}), removeAll, function respond(req, res, next) {
res.send(204);
next();
});
```

## <a name="18-run-your-server-application-again-and-ensure-it-rejects-you"></a>18: de servertoepassing opnieuw uit en zorg ervoor dat u weigert

We gebruiken `curl` opnieuw om te controleren of we nu OAuth2 bescherming tegen onze eindpunten. We zullen dit doen voordat gestart van onze client SDK's op basis van dit eindpunt. De headers teruggestuurd moet genoeg om ons te laten dat wij u het juiste pad zijn.

Controleer eerst of uw isntance monogoDB actief is...

    $sudo mongod

Vervolgens Ga naar de map en start curling...

    $ cd azuread
    $ node server.js

Probeer een eenvoudige boeken:

`$ curl -isS -X POST http://127.0.0.1:8080/tasks/brandon/Hello`

```Shell
HTTP/1.1 401 Unauthorized
Connection: close
WWW-Authenticate: Bearer realm="Users"
Date: Tue, 14 Jul 2015 05:45:03 GMT
Transfer-Encoding: chunked
```

Een 401 is het antwoord dat u zoekt, zoals die wordt aangegeven dat de laag Passport probeert om te leiden naar het eindpunt van de machtigen, die is precies wat u wilt.


## <a name="congratulations-you-have-a-rest-api-service-using-oauth2"></a>Gefeliciteerd! U hebt een REST API Service met behulp van OAuth2!

U hebt bezocht, voorzover kunt u met deze server zonder een compatibele client OAuth2. U moet een aanvullende procedure doorlopen.

Als u alleen informatie over het implementeren van een REST API met behulp van Restify en OAuth2 zoekt, hebt u meer dan voldoende code voor het ontwikkelen van uw service en leren hoe u kunt bouwen op dit voorbeeld.

## <a name="next-steps"></a>Volgende stappen

Voor een verwijzing naar kunt het ingevulde voorbeeld (zonder de waarden van de systeemconfiguratie) [wordt geleverd als een .zip hier](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-nodejs/archive/complete.zip)of u klonen het van GitHub:

```git clone --branch complete https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-nodejs.git```

U kunt nu verplaatsen naar de meer geavanceerde onderwerpen.  U kunt proberen:

[Een Node.js web app met behulp van het eindpunt v2.0 Secure >>](active-directory-v2-devquickstarts-node-web.md)

Aanvullende bronnen voor uitchecken:
- [De handleiding voor ontwikkelaars van versie 2.0 >>](active-directory-appmodel-v2-overview.md)
- [StackOverflow 'azure active directory'-code >>](http://stackoverflow.com/questions/tagged/azure-active-directory)

## <a name="get-security-updates-for-our-products"></a>Beveiligingsupdates voor onze producten

We raden u meldingen wanneer beveiligingsincidenten plaatsvinden door [deze pagina](https://technet.microsoft.com/security/dd252948) te bezoeken en abonneren op waarschuwingen Raadgevend ophalen.
