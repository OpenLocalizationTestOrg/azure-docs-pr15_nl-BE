<properties
    pageTitle="Azure AD v2.0 NodeJS Web App | Microsoft Azure"
    description="Het bouwen van een knooppunt JS web app dat gebruikers met beide persoonlijke Microsoft-Account aangemeld en rekeningen voor werk of school."
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

# <a name="add-sign-in-to-a-nodejs-web-app"></a>Aanmelden toevoegen aan een nodeJS Web App


> [AZURE.NOTE]
    Niet alle scenario's Azure Active Directory en functies worden ondersteund door het eindpunt v2.0.  Om te bepalen als u het eindpunt v2.0 moet gebruiken, Lees over [v2.0 beperkingen](active-directory-v2-limitations.md).


We gebruiken hier paspoort aan:

- De gebruiker aanmelden bij het gebruik van Azure AD app en het eindpunt v2.0.
- Sommige informatie weergeven over de gebruiker.
- De gebruiker van de toepassing ondertekenen.

**Passport** is verificatie middleware voor Node.js. Zeer flexibel en modulair, Passport kan onopvallend wegvallen voor elk Express- of Resitify van de webtoepassing. Een uitgebreide set van strategieën voor ondersteuning van verificatie met behulp van een gebruikersnaam en wachtwoord, Facebook en Twitter. We hebben een strategie ontwikkeld voor Microsoft Azure Active Directory. We in deze module te installeren en vervolgens Microsoft Azure Active Directory toevoegen `passport-azure-ad` plug-in.

## <a name="download"></a>Downloaden

De code voor deze zelfstudie wordt onderhouden [op GitHub](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-nodejs).  Als u wilt volgen, kunt u [downloaden van de app skelet als een ZIP-](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-nodejs/archive/skeleton.zip) of het skelet klonen:

```git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-nodejs.git```

De voltooide toepassing wordt geleverd aan het einde van deze zelfstudie ook.

## <a name="1-register-an-app"></a>1. een App registreren
Maak een nieuwe app op [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)of Volg deze [gedetailleerde stappen](active-directory-v2-app-registration.md).  Zorg ervoor dat:

- Kopiëren naar beneden de **Toepassings-Id** toegewezen aan uw app, moet u deze snel.
- Het **Web** platform voor uw app toevoegen.
- Voer de juiste **Redirect URI**. De omleiding URI geeft aan dat Azure AD waarin verificatie reacties moeten worden gericht - de standaardwaarde voor deze zelfstudie is `http://localhost:3000/auth/openid/return`.

## <a name="2-add-pre-requisities-to-your-directory"></a>2. pre-requisities toevoegen aan de directory

Mappen naar de hoofdmap als dit niet al er wijzigen en voert u de volgende opdrachten vanaf de opdrachtregel:

- `npm install express`
- `npm install ejs`
- `npm install ejs-locals`
- `npm install restify`
- `npm install mongoose`
- `npm install bunyan`
- `npm install assert-plus`
- `npm install passport`
- `npm install webfinger`
- `npm install body-parser`
- `npm install express-session`
- `npm install cookie-parser`

- Bovendien is gebruik `passport-azure-ad` in het skelet van de quickstart.

- `npm install passport-azure-ad`


Hiermee installeert u de bibliotheken die passport-azure-ad afhankelijk.

## <a name="3-set-up-your-app-to-use-the-passport-node-js-strategy"></a>3. instellen van uw app gebruiken de passport-knooppunt-js-strategie
Hier zullen we de Express middleware voor het gebruik van het verificatieprotocol OpenID verbinding configureren.  Passport wordt gebruikt voor aanmeldings- en afmeldingstijden aanvragen en informatie over de gebruiker, onder andere gebruikerssessies beheren.

-   Open om te beginnen, de `config.js` in de hoofdmap van het project-bestand en voert u de configuratiewaarden van uw app in de `exports.creds` sectie.
    -   De `clientID:` is de **Toepassings-Id** toegewezen aan uw app in de portal voor registratie.
    -   De `returnURL` wordt de **URI omgeleid** u hebt ingevoerd in de portal.
    - De `clientSecret` is het geheim dat u hebt gegenereerd in de portal.

- De volgende keer opent `app.js` in de hoofdmap van het project-bestand en voeg de aanroep follwing aan te roepen de `OIDCStrategy` strategie die wordt geleverd met`passport-azure-ad`


```JavaScript
var OIDCStrategy = require('passport-azure-ad').OIDCStrategy;

// Add some logging
var log = bunyan.createLogger({
    name: 'Microsoft OIDC Example Web Application'
});
```

- Gebruik daarna de strategie die we zojuist verwezen verwerken van onze login aanvragen

```JavaScript
// Use the OIDCStrategy within Passport. (Section 2)
//
//   Strategies in passport require a `validate` function, which accept
//   credentials (in this case, an OpenID identifier), and invoke a callback
//   with a user object.
passport.use(new OIDCStrategy({
    callbackURL: config.creds.returnURL,
    realm: config.creds.realm,
    clientID: config.creds.clientID,
    clientSecret: config.creds.clientSecret,
    oidcIssuer: config.creds.issuer,
    identityMetadata: config.creds.identityMetadata,
    responseType: config.creds.responseType,
    responseMode: config.creds.responseMode,
    skipUserProfile: config.creds.skipUserProfile
    scope: config.creds.scope
  },
  function(iss, sub, profile, accessToken, refreshToken, done) {
    log.info('Example: Email address we received was: ', profile.email);
    // asynchronous verification, for effect...
    process.nextTick(function () {
      findByEmail(profile.email, function(err, user) {
        if (err) {
          return done(err);
        }
        if (!user) {
          // "Auto-registration"
          users.push(profile);
          return done(null, profile);
        }
        return done(null, user);
      });
    });
  }
));
```
Een soortgelijk patroon Passport gebruikt voor alle die strategieën (Twitter, Facebook, enz.) die voor alle schrijvers van de strategie worden aangehouden is. Kijken naar de strategie er geven we een function() met een token en een gereed als parameters. De strategie zal plichtsgetrouw terugkomen naar ons zodra het doet al het werk. Als dat zo is willen we voor het opslaan van de gebruiker en het token niet initialiseren zodat we hoeft aan te vragen om het opnieuw.

> [AZURE.IMPORTANT]
De bovenstaande code wordt elke gebruiker die met onze server te verifiëren. Dit staat bekend als de automatische registratie. Productieservers wilt u wouldn't laat iedereen zonder eerste ze via een registratieproces die u beslist. Dit is meestal het patroon dat u ziet in de consument apps die u kunnen u aanmelden bij Facebook maar vraagt u om extra informatie in te vullen. Als dit niet een voorbeeldtoepassing is, kan hebben we alleen het e-mailbericht uitgepakt uit het token-object dat wordt geretourneerd en wordt vervolgens gevraagd om aanvullende informatie invullen. Aangezien dit een testserver we gewoon de in-memory database toevoegen.

- Vervolgens voegen we de methoden waarmee wij voor het bijhouden van de aangemelde gebruikers zoals vereist door de Passport-account. Dit omvat serialisatie en deserialisatie van informatie van de gebruiker:

```JavaScript

// Passport session setup. (Section 2)

//   To support persistent login sessions, Passport needs to be able to
//   serialize users into and deserialize users out of the session.  Typically,
//   this will be as simple as storing the user ID when serializing, and finding
//   the user by ID when deserializing.
passport.serializeUser(function(user, done) {
  done(null, user.email);
});

passport.deserializeUser(function(id, done) {
  findByEmail(id, function (err, user) {
    done(err, user);
  });
});

// array to hold logged in users
var users = [];

var findByEmail = function(email, fn) {
  for (var i = 0, len = users.length; i < len; i++) {
    var user = users[i];
    log.info('we are using user: ', user);
    if (user.email === email) {
      return fn(null, user);
    }
  }
  return fn(null, null);
};

```

- Vervolgens voegen we de code voor het laden van de express-engine. U ziet hier gebruiken we de standaard /views en /routes-patroon dat Express biedt.

```JavaScript

// configure Express (Section 2)

var app = express();


app.configure(function() {
  app.set('views', __dirname + '/views');
  app.set('view engine', 'ejs');
  app.use(express.logger());
  app.use(express.methodOverride());
  app.use(cookieParser());
  app.use(expressSession({ secret: 'keyboard cat', resave: true, saveUninitialized: false }));
  app.use(bodyParser.urlencoded({ extended : true }));
  // Initialize Passport!  Also use passport.session() middleware, to support
  // persistent login sessions (recommended).
  app.use(passport.initialize());
  app.use(passport.session());
  app.use(app.router);
  app.use(express.static(__dirname + '/../../public'));
});

```

- Tot slot laten we routes toevoegen, de boeken wordt met de hand uit de werkelijke login aanvragen voor de `passport-azure-ad` motor:

```JavaScript

// Our Auth routes (Section 3)

// GET /auth/openid
//   Use passport.authenticate() as route middleware to authenticate the
//   request.  The first step in OpenID authentication will involve redirecting
//   the user to their OpenID provider.  After authenticating, the OpenID
//   provider will redirect the user back to this application at
//   /auth/openid/return

app.get('/auth/openid',
  passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
  function(req, res) {
    log.info('Authenitcation was called in the Sample');
    res.redirect('/');
  });

// GET /auth/openid/return
//   Use passport.authenticate() as route middleware to authenticate the
//   request.  If authentication fails, the user will be redirected back to the
//   login page.  Otherwise, the primary route function function will be called,
//   which, in this example, will redirect the user to the home page.
app.get('/auth/openid/return',
  passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
  function(req, res) {

    res.redirect('/');
  });

// POST /auth/openid/return
//   Use passport.authenticate() as route middleware to authenticate the
//   request.  If authentication fails, the user will be redirected back to the
//   login page.  Otherwise, the primary route function function will be called,
//   which, in this example, will redirect the user to the home page.

app.post('/auth/openid/return',
  passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
  function(req, res) {

    res.redirect('/');
  });
```

## <a name="4-use-passport-to-issue-sign-in-and-sign-out-requests-to-azure-ad"></a>4. gebruik van Passport op teken- en afmeldingstijden aanvragen op Azure AD

Uw app is nu geconfigureerd om te communiceren met de v2.0 eindpunt met het verificatieprotocol OpenID verbinden.  `passport-azure-ad`van alle lelijke details verificatieberichten samenstellen en onderhouden van de gebruikerssessie tokens van Azure AD valideren is afgehandeld.  Die blijft is om uw gebruikers een manier om het aanmelden, afmelden en aanvullende informatie verzamelen over de aangemelde gebruiker.

- Eerst kunt toevoegen de methoden standaard, login account en meld u af om onze `app.js` bestand:

```JavaScript

//Routes (Section 4)

app.get('/', function(req, res){
  res.render('index', { user: req.user });
});

app.get('/account', ensureAuthenticated, function(req, res){
  res.render('account', { user: req.user });
});

app.get('/login',
  passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
  function(req, res) {
    log.info('Login was called in the Sample');
    res.redirect('/');
});

app.get('/logout', function(req, res){
  req.logout();
  res.redirect('/');
});

```

-   Bekijken we de volgende details:
    -   De `/` route worden omgeleid naar de index.ejs weergave geven van de gebruiker in de aanvraag (indien aanwezig)
    - De `/account` route wordt eerst ***zorgen we worden geverifieerd*** (we implementeren die hieronder) en vervolgens de gebruiker in het verzoek doorgeven zodat we meer informatie over de gebruiker krijgen.
    - De `/login` route afhalen onze verificator azuread openidconnect van `passport-azuread` en als dat zal mislukt de gebruiker omleiden naar /login
    - De `/logout` wordt gewoon de logout.ejs bellen (en route) die cookies worden gewist en vervolgens keert de gebruiker terug naar index.ejs


- Voor het laatste deel van de `app.js`, voegen we de EnsureAuthenticated-methode die wordt gebruikt in `/account` hierboven.

```JavaScript

// Simple route middleware to ensure user is authenticated. (Section 4)

//   Use this route middleware on any resource that needs to be protected.  If
//   the request is authenticated (typically via a persistent login session),
//   the request will proceed.  Otherwise, the user will be redirected to the
//   login page.
function ensureAuthenticated(req, res, next) {
  if (req.isAuthenticated()) { return next(); }
  res.redirect('/login')
}

```

- Tot slot laten we daadwerkelijk maakt de server zelf in `app.js`:

```JavaScript

app.listen(3000);

```


## <a name="5-create-the-views-and-routes-in-express-to-display-our-user-in-the-website"></a>5. Maak de weergaven en routes in express om onze gebruikers in de website weer te geven

We hebben onze `app.js` voltooid. Nu we gewoon moeten toe te voegen, de routes en weergaven, zodat de informatie aan de gebruiker als greep we krijgen de `/logout` en `/login` routes die wij hebben gemaakt.

- Maak de `/routes/index.js` route onder de hoofdmap.

```JavaScript

/*
 * GET home page.
 */

exports.index = function(req, res){
  res.render('index', { title: 'Express' });
};
```

- Maak de `/routes/user.js` route onder de hoofdmap

```JavaScript

/*
 * GET users listing.
 */

exports.list = function(req, res){
  res.send("respond with a resource");
};
```

Deze eenvoudige routes wordt net langs de aanvraag doorgegeven aan onze weergaven, met inbegrip van de gebruiker aanwezig.

- Maak de `/views/index.ejs` weergave onder de hoofdmap. Dit is een eenvoudige pagina die onze aan- en afmelden methoden aanroepen en kunnen we u accountgegevens ophaalt. Aankondiging dat we de voorwaardelijke gebruiken `if (!user)` als de gebruiker in de aanvraag worden doorgegeven via het bewijs hebben we een aangemelde gebruiker is.

```JavaScript
<% if (!user) { %>
    <h2>Welcome! Please log in.</h2>
    <a href="/login">Log In</a>
<% } else { %>
    <h2>Hello, <%= user.displayName %>.</h2>
    <a href="/account">Account Info</a></br>
    <a href="/logout">Log Out</a>
<% } %>
```

- Maakt de `/views/account.ejs` onder de hoofdmap weergeven zodat we extra informatie kunt weergeven die `passport-azuread` in de aanvraag van de gebruiker heeft gezet.

```Javascript
<% if (!user) { %>
    <h2>Welcome! Please log in.</h2>
    <a href="/login">Log In</a>
<% } else { %>
<p>displayName: <%= user.displayName %></p>
<p>givenName: <%= user.name.givenName %></p>
<p>familyName: <%= user.name.familyName %></p>
<p>UPN: <%= user._json.upn %></p>
<p>Profile ID: <%= user.id %></p>
<p>Full Claimes</p>
<%- JSON.stringify(user) %>
<p></p>
<a href="/logout">Log Out</a>
<% } %>
```

- Tot slot, we maken dit uiterlijk vrij door een lay-out toe te voegen. Maak het ' / views/layout.ejs weergave onder de hoofdmap

```HTML

<!DOCTYPE html>
<html>
    <head>
        <title>Passport-OpenID Example</title>
    </head>
    <body>
        <% if (!user) { %>
            <p>
            <a href="/">Home</a> |
            <a href="/login">Log In</a>
            </p>
        <% } else { %>
            <p>
            <a href="/">Home</a> |
            <a href="/account">Account</a> |
            <a href="/logout">Log Out</a>
            </p>
        <% } %>
        <%- body %>
    </body>
</html>
```

Ten slotte bouwen en uitvoeren van uw app!

Uitvoeren van `node app.js` en Ga naar`http://localhost:3000`


Meld u aan met een persoonlijke Microsoft-Account of een account voor werk of school en u ziet hoe de identiteit van de gebruiker wordt weergegeven in de lijst /account.  U hebt nu een web app beveiligd met behulp van industriestandaardprotocollen die gebruikers met hun persoonlijke en werk/school accounts kunnen worden geverifieerd.

##<a name="next-steps"></a>Volgende stappen

Voor een verwijzing naar kunt het ingevulde voorbeeld (zonder de waarden van de systeemconfiguratie) [wordt geleverd als een .zip hier](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-nodejs/archive/complete.zip)of u klonen het van GitHub:

```git clone --branch complete https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-nodejs.git```

U kunt nu verplaatsen naar de meer geavanceerde onderwerpen.  U kunt proberen:

[Een node.js Secure web-api met behulp van het eindpunt v2.0 >>](active-directory-v2-devquickstarts-node-api.md)

Aanvullende bronnen voor uitchecken:
- [De handleiding voor ontwikkelaars van versie 2.0 >>](active-directory-appmodel-v2-overview.md)
- [StackOverflow 'azure active directory'-code >>](http://stackoverflow.com/questions/tagged/azure-active-directory)

## <a name="get-security-updates-for-our-products"></a>Beveiligingsupdates voor onze producten

We raden u meldingen wanneer beveiligingsincidenten plaatsvinden door [deze pagina](https://technet.microsoft.com/security/dd252948) te bezoeken en abonneren op waarschuwingen Raadgevend ophalen.
