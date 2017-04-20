<properties 
    pageTitle="Met behulp van Twilio voor spraak, VoIP en SMS-berichten in Azure" 
    description="Informatie over het bellen en verzenden van een SMS-bericht met de API Twilio service op Azure. Voorbeelden van code geschreven in Node.js." 
    services="" 
    documentationCenter="nodejs" 
    authors="devinrader" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="multiple" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="nodejs" 
    ms.topic="article" 
    ms.date="11/25/2014" 
    ms.author="wpickett"/>


# <a name="using-twilio-for-voice-voip-and-sms-messaging-in-azure"></a>Met behulp van Twilio voor spraak, VoIP en SMS-berichten in Azure

In deze handleiding beschreven hoe u toepassingen die met Twilio en node.js op Azure communiceren.

<a id="whatis"/>
## <a name="what-is-twilio"></a>Wat is Twilio?

Twilio is een API-platform waarmee u gemakkelijk voor ontwikkelaars en telefoongesprekken ontvangen, verzenden en ontvangen berichten met tekst, en VoIP gesprekken in de browser en eigen mobiele toepassingen insluiten.  We gaan nu Ga kort in op hoe dit werkt vooraleer.

### <a name="receiving-calls-and-text-messages"></a>Ontvangen van oproepen en tekstberichten

Twilio kunnen ontwikkelaars aan te [schaffen programmeerbare telefoonnummers] [ purchase_phone] die kan worden gebruikt voor het verzenden en ontvangen van oproepen en tekstberichten.  Als een getal Twilio een inkomende oproep of tekst ontvangt, stuurt Twilio uw webtoepassing een HTTP POST of GET-aanvraag, waarin u wordt gevraagd voor instructies over het afhandelen van de oproep of tekst.  De server reageert op HTTP-aanvraag van Twilio met [TwiML][twiml], een eenvoudige set van XML-codes die instructies voor het afhandelen van een gesprek of tekst bevatten.  In even zien we voorbeelden van TwiML.

### <a name="making-calls-and-sending-text-messages"></a>Gesprekken en het verzenden van berichten

Doordat de HTTP-aanvragen naar de Twilio web service API kunnen ontwikkelaars tekstberichten verzenden of uitgaande telefoongesprekken tot stand brengen.  Voor uitgaande gesprekken, de ontwikkelaar moet ook een URL opgeven die als resultaat TwiML instructies voor het uitgaande gesprekken afhandelen gegeven zodra deze is aangesloten.

### <a name="embedding-voip-capabilities-in-ui-code-javascript-ios-or-android"></a>VoIP-functionaliteit insluiten in UI-code (JavaScript, iOS of Android)

Twilio biedt een client-side SDK die een desktop webbrowser, iOS-app of Android app in een VoIP-telefoon omzetten kunt.  In dit artikel gaan we in op het gebruik van VoIP-aanroepen in de browser.  Naast de JavaScript Twilio SDK in de browser wordt uitgevoerd, moet een servertoepassing (onze node.js toepassing) worden gebruikt voor het uitgeven van een token' mogelijkheid' naar de JavaScript-client.  U kunt meer lezen over het gebruik van VoIP met node.js [op de blog van de dev Twilio][voipnode].

<a id="signup"/>
## <a name="sign-up-for-twilio-microsoft-discount"></a>Aanmelden voor Twilio (Microsoft korting)

Twilio services wilt gebruiken, moet u [zich aanmelden voor een account][signup].  Microsoft Azure klanten ontvangen een speciale korting - [Meld u hier moet][signup]!

<a id="azuresite"/>
## <a name="create-and-deploy-a-nodejs-azure-website"></a>Maken en implementeren van een Website node.js Azure

Vervolgens moet u een node.js website waarop Azure te maken.  [De officiële documentatie voor dit hier bevindt zich][azure_new_site].  Op een hoog niveau, zal u worden als volgt:

* Aanmelden voor een account Azure, als u geen sjabloon hebt
* Een nieuwe website maken met behulp van de beheerconsole van Azure
* Source control ondersteuning (we ervan uit dat u gebruikt git) toe te voegen
* Maken van een `server.js` met een eenvoudige node.js webtoepassing
* Deze eenvoudige toepassing Azure implementeren

<a id="twiliomodule"/>
## <a name="configure-the-twilio-module"></a>Configureer de Module Twilio

Vervolgens beginnen wij een eenvoudige node.js toepassing die gebruik maakt van de API Twilio schrijven.  Voordat we beginnen, moeten we onze Twilio accountgegevens configureren.  

### <a name="configuring-twilio-credentials-in-system-environment-variables"></a>Twilio referenties in de systeemomgevingsvariabelen configureren

We moeten onze account en auth token, die dienen als de gebruikersnaam en het wachtwoord ingesteld voor onze Twilio account zodat geverifieerde aanvragen tegen de Twilio back-end. De veiligste manier om te configureren voor gebruik met de module knooppunt in Azure is via de systeemomgevingsvariabelen, die u rechtstreeks in de Azure-beheerconsole kunt instellen.

Selecteer uw node.js-website en klik op de koppeling 'Configureren'.  Als u beneden schuift, ziet u een gebied waarin u de configuratie-eigenschappen voor uw toepassing instellen kunt.  Geef de referenties van de account Twilio ([gevonden op uw dashboard Twilio][twilio_dashboard]) zoals - Zorg ervoor dat deze de naam "TWILIO_ACCOUNT_SID" en "TWILIO_AUTH_TOKEN", respectievelijk:

![Azure-beheerconsole][azure-admin-console]

Wanneer u deze variabelen hebt geconfigureerd, start u de toepassing opnieuw in de console Azure.

### <a name="declaring-the-twilio-module-in-packagejson"></a>Het declareren van de module Twilio in package.json

Vervolgens moet een package.json voor het beheren van onze knooppunt module afhankelijkheden via [npm]maken.  Maak een bestand met de naam 'package.json' op hetzelfde niveau als het "server.js" bestand dat u hebt gemaakt in de zelfstudie voor Azure/node.js.  In dit bestand, plaatst u het volgende:

  {'naam': 'application name', 'versie': '0.0.1', 'Privé': true, 'scripts': {'start': 'server knooppunt'}, 'afhankelijkheden': {'express': "3.1.0", "ejs": "*", "twilio": "*"}}

Dit verklaart de module twilio als een afhankelijkheid, evenals de populaire [express web framework] [ express] en de sjabloon EJS motor.  Goed, nu we bent klaar - we schrijven wat code!

<a id="makecall"/>
## <a name="make-an-outbound-call"></a>Een uitgaand gesprek

We gaan nu een eenvoudig formulier maakt dat zal een gesprek naar een nummer kiezen wij.  Open server.js en voer de volgende code.  Houd rekening met 'CHANGE_ME' - plaatst u de naam van uw website azure er eens:

    // Module dependencies
    var express = require('express'), 
      path = require('path'), 
      http = require('http'), 
      twilio = require('twilio');

    // Create Express web application
    var app = express();

    // Express configuration
    app.configure(function(){
      app.set('port', process.env.PORT || 3000);
      app.set('views', __dirname + '/views');
      app.set('view engine', 'ejs');
      app.use(express.favicon());
      app.use(express.logger('dev'));
      app.use(express.bodyParser());
      app.use(express.methodOverride());
      app.use(app.router);
      app.use(express.static(path.join(__dirname, 'public')));
    });
    app.configure('development', function(){
      app.use(express.errorHandler());
    });

    // Render an HTML user interface for the application's home page
    app.get('/', function(request, response) {
      response.render('index');
    });

    // Handle the form POST to place a call
    app.post('/call', function(request, response) {
      var client = twilio();
      client.makeCall({
          // make a call to this number
          to:request.body.number,

          // Change to a Twilio number you bought - see:
          // https://www.twilio.com/user/account/phone-numbers/incoming
          from:'+15558675309',

          // A URL in our app which generates TwiML
          // Change "CHANGE_ME" to your app's name
          url:'https://CHANGE_ME.azurewebsites.net/outbound_call'
      }, function(error, data) {
          // Go back to the home page
          response.redirect('/');
      });
    });

    // Generate TwiML to handle an outbound call
    app.post('/outbound_call', function(request, response) {
      var twiml = new twilio.TwimlResponse();

      // Say a message to the call's receiver 
      twiml.say('hello - thanks for checking out Twilio and Azure', {
          voice:'woman'
      });

      response.set('Content-Type', 'text/xml');
      response.send(twiml.toString());
    });

    // Start server
    http.createServer(app).listen(app.get('port'), function(){
      console.log("Express server listening on port " + app.get('port'));
    });

Vervolgens maakt u een map met de naam 'weergaven' - in deze map, maak een bestand met de naam 'index.ejs' met de volgende inhoud:

    <!DOCTYPE html>
    <html>
    <head>
      <title>Twilio Test</title>
      <style>
        input { height:20px; width:300px; font-size:18px; margin:5px; padding:5px; }
      </style>
    </head>
    <body>
      <h1>Twilio Test</h1>
      <form action="/call" method="POST">
          <input placeholder="Enter a phone number" name="number"/>
          <br/>
          <input type="submit" value="Call the number above"/>
      </form>
    </body>
    </html>

Nu uw website implementeren op Azure en openen van uw huis.  U moet mogelijk wordt gebeld door het nummer van uw Twilio en voer je telefoonnummer in het tekstveld!

<a id="sendmessage"/>
## <a name="send-an-sms-message"></a>Een SMS-bericht verzenden

Nu we instellen van een gebruikersinterface en een formulier logica hanteren voor het verzenden van een tekstbericht.  'Server.js' te openen en voeg de volgende code na de laatste aanroep 'app.post':

    app.post('/sms', function(request, response) {
      var client = twilio();
      client.sendSms({
          // send a text to this number
          to:request.body.number,

          // A Twilio number you bought - see:
          // https://www.twilio.com/user/account/phone-numbers/incoming
          from:'+15558675309',

          // The body of the text message
          body: request.body.message
          
      }, function(error, data) {
          // Go back to the home page
          response.redirect('/');
      });
    });

In 'views/index.ejs', voegt u een ander formulier onder de eerste die een getal en een bericht verzenden:

    <form action="/sms" method="POST">
      <input placeholder="Enter a phone number" name="number"/>
      <br/>
      <input placeholder="Enter a message to send" name="message"/>
      <br/>
      <input type="submit" value="Send text to the number above"/>
    </form>

Implementeer de toepassing naar Azure en nu moet u dat formulier en zelf (of een van je beste vrienden) een tekstbericht verzenden inzenden!

<a id="nextsteps"/>
## <a name="next-steps"></a>Volgende stappen

U hebt nu geleerd de basisbeginselen van het bouwen van toepassingen die communiceren met node.js en Twilio.  Maar deze voorbeelden nauwelijks krassen veroorzaken op het oppervlak van de mogelijkheden met Twilio en node.js.  Bekijk de volgende bronnen voor meer informatie Twilio met node.js gebruikt:

* [Module van officiële documenten][docs]
* [Zelfstudie over VoIP met node.js toepassingen][voipnode]
* [Votr - een real-time SMS stemmen toepassing met node.js en CouchDB (drie delen)][votr]
* [Combinatie van programmeren in de browser met node.js][pair]

We hopen dat u graag node.js en Twilio hacken op Azure.

[purchase_phone]: https://www.twilio.com/user/account/phone-numbers/available/local
[twiml]: https://www.twilio.com/docs/api/twiml
[signup]: http://ahoy.twilio.com/azure
[azure_new_site]: /app-service-web/web-sites-nodejs-develop-deploy-mac.md
[twilio_dashboard]: https://www.twilio.com/user/account
[npm]: http://npmjs.org
[express]: http://expressjs.com
[voipnode]: http://www.twilio.com/blog/2013/04/introduction-to-twilio-client-with-node-js.html
[docs]: http://twilio.github.io/twilio-node/
[votr]: http://www.twilio.com/blog/2012/09/building-a-real-time-sms-voting-app-part-1-node-js-couchdb.html
[pair]: http://www.twilio.com/blog/2013/06/pair-programming-in-the-browser-with-twilio.html
[azure-admin-console]: ./media/partner-twilio-nodejs-how-to-use-voice-sms/twilio_1.png



