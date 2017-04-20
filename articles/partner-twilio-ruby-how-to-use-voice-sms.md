<properties 
    pageTitle="Het gebruik van Twilio voor spraak- en SMS (Ruby) | Microsoft Azure" 
    description="Informatie over het bellen en verzenden van een SMS-bericht met de API Twilio service op Azure. Voorbeelden van code geschreven in Ruby." 
    services="" 
    documentationCenter="ruby" 
    authors="devinrader" 
    manager="twilio" 
    editor=""/>

<tags 
    ms.service="multiple" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="ruby" 
    ms.topic="article" 
    ms.date="11/25/2014" 
    ms.author="MicrosoftHelp@twilio.com"/>





# <a name="how-to-use-twilio-for-voice-and-sms-capabilities-in-ruby"></a>Het gebruik van Twilio voor spraak- en SMS-mogelijkheden in Ruby
Deze handleiding wordt beschreven hoe algemene programmeertaken met de API Twilio-service uitvoeren op Azure. De scenario's die zijn te bellen en verzenden van een bericht Short Message Service (SMS). Zie de sectie [Volgende stappen](#NextSteps) voor meer informatie over Twilio en het gebruik van spraak en SMS in uw toepassingen.

## <a id="WhatIs"></a>Wat is Twilio?
Twilio is een web service-API voor telefonie kunt u uw bestaande web-talen en vaardigheden via spraak- en SMS-toepassingen bouwen. Twilio is een service van derden (niet een Azure-functie en niet in een Microsoft-product).

**Twilio spraak** kunt uw toepassingen en telefoongesprekken ontvangen. **Twilio SMS** kunt uw toepassingen en ontvangen van SMS-berichten. **Twilio-Client** kunt uw toepassingen gesproken communicatie met bestaande Internet-verbindingen, met inbegrip van mobiele verbindingen inschakelen.

## <a id="Pricing"></a>Twilio prijzen en speciale aanbiedingen
Informatie over prijzen Twilio is beschikbaar op de [Prijzen van Twilio] [twilio_pricing]. Azure-klanten krijgen een [speciale aanbieding][special_offer]: een gratis krediet van 1000 teksten of 1000 minuten inkomend. Aanmelden voor dit aanbod of meer informatie, Ga naar [http://ahoy.twilio.com/azure][special_offer].  

## <a id="Concepts"></a>Concepten
De Twilio-API is een RESTful API die spraak- en SMS-functionaliteit voor toepassingen biedt. Clientbibliotheken zijn beschikbaar in meerdere talen; Zie voor een lijst, [API-bibliotheken Twilio] [twilio_libraries].

### <a id="TwiML"></a>TwiML
TwiML is een set XML-instructies die Twilio van het verwerken van een oproep of SMS op de hoogte.

Als u bijvoorbeeld zou het volgende TwiML omzetten in de tekst **Hallo wereld** spraak.

    <?xml version="1.0" encoding="UTF-8" ?>
    <Response>
       <Say>Hello World</Say>
    </Response>

Alle TwiML documenten hebben `<Response>` als hun root-element. Van daaruit kunt u Twilio bewerkingen bepalen het gedrag van uw toepassing.

### <a id="Verbs"></a>TwiML woorden
Twilio-bewerkingen zijn XML-codes die Twilio wat meer te **doen**. Bijvoorbeeld, de ** &lt;zeg&gt; ** opdracht geïnstrueerd Twilio hoorbaar een bericht te bezorgen in een gesprek. 

Hier volgt een lijst met bewerkingen voor Twilio.

* ** &lt;Externe&gt;**: de beller een verbinding maakt met een andere telefoon.
* ** &lt;Verzamelen&gt;**: verzamelt cijfers op het toetsenblok van de telefoon is ingevoerd.
* ** &lt;Ophangen&gt;**: een oproep wordt beëindigd.
* ** &lt;Spelen&gt;**: een audiobestand wordt afgespeeld.
* ** &lt;Onderbreken&gt;**: stil wacht op een opgegeven aantal seconden.
* ** &lt;Record&gt;**: de stem van de beller wordt vastgelegd en geeft als resultaat een URL van een bestand met de opname.
* ** &lt;Redirect&gt;**: controle van een gesprek of een SMS stuurt naar de TwiML op een andere URL.
* ** &lt;Weigeren&gt;**: weigert een binnenkomend gesprek naar het nummer van uw Twilio zonder u facturering
* ** &lt;Zeg&gt;**: converteert tekst naar spraak op een oproep is gedaan.
* ** &lt;Sms&gt;**: een SMS-bericht verzonden.

Zie voor meer informatie over de Twilio woorden, hun kenmerken en TwiML [TwiML] [twiml]. Zie voor meer informatie over de API Twilio [API Twilio] [twilio_api].

## <a id="CreateAccount"></a>Maak een Account Twilio
Wanneer u klaar bent om een Twilio-account, aanmelden bij [Twilio probeer] [try_twilio]. U kunt beginnen met een gratis account en uw account later bijwerken.

Wanneer u zich voor een account Twilio aanmeldt, krijgt u een gratis telefoonnummer voor uw toepassing. U ontvangt ook een account en een token voor verificatie. Beide moesten Twilio API-aanroepen. Als u wilt voorkomen dat onbevoegde toegang tot uw account, uw verificatietoken veilig houden. Uw account en auth-tokens kunnen worden weergegeven op de [pagina Twilio][twilio_account], in de velden met de naam **SID rekening** en **AUTH TOKEN**, respectievelijk.

### <a id="VerifyPhoneNumbers"></a>Controleer of de telefoonnummers
Naast het nummer krijgt u door Twilio, kunt u ook controleren of u de controle (dat wil zeggen uw mobiele telefoon of huis telefoonnummer) nummers voor gebruik in uw toepassingen. 

Zie voor meer informatie over het controleren van een nummer, [Nummers beheren] [verify_phone].

## <a id="create_app"></a>Een Ruby-toepassing maken
Een Ruby-toepassing die gebruikmaakt van de service Twilio en wordt uitgevoerd in Azure is niet anders dan andere Ruby toepassing die gebruikmaakt van de service Twilio. Terwijl Twilio services RESTful zijn en op verschillende manieren kunnen worden aangeroepen vanuit Ruby, in dit artikel zal zich richten op Twilio services gebruiken met [Twilio helper library voor Ruby][twilio_ruby].

Eerste, [een nieuwe Azure Linux VM installeren] [ azure_vm_setup] op te treden als host voor de nieuwe Ruby webtoepassing. De stappen voor het maken van een Rails app, slechts installeren de VM negeren. Zorg ervoor dat u een eindpunt maken met een externe poort 80 en een interne poort van 5000.

In de onderstaande voorbeelden we [Sinatra]gebruikt[sinatra], een zeer eenvoudige web framework voor Ruby. Maar zeker voor kunt u de bibliotheek Twilio helper Ruby met eventuele andere web framework, met inbegrip van Ruby op Rails.

SSH in uw nieuwe VM en maak een map voor de nieuwe toepassing. In die map, maak een bestand met de naam Gemfile en kopieer de volgende code in het:

    source 'https://rubygems.org'
    gem 'sinatra'
    gem 'thin'

Op de opdrachtregel uitvoeren `bundle install`. Hiermee installeert u de bovenstaande afhankelijkheden. Maak een bestand genaamd `web.rb`. Dit is waar de code voor uw web app woont. Plak de volgende code in het:

    require 'sinatra'

    get '/' do
        "Hello Monkey!"
    end

Op dit punt moet mogelijk zijn het uitvoeren van de opdracht `ruby web.rb -p 5000`. Dit zal draaien van een kleine webserver op poort 5000. U moet mogelijk bladeren naar deze toepassing in uw browser via de URL u opzet voor uw Azure VM. Zodra u uw web app in de browser bereiken kan, bent u klaar om te beginnen met het samenstellen van een Twilio-app.

## <a id="configure_app"></a>Uw toepassing configureren voor gebruik van Twilio
Kunt u uw web app bijwerken voor het gebruik van de bibliotheek Twilio de `Gemfile` op deze regel:

    gem 'twilio-ruby'

Voer op de opdrachtregel, `bundle install`. Open nu `web.rb` en deze in de bovenste regel met:

    require 'twilio-ruby'

U bent nu klaar gebruik van de bibliotheek Twilio helper voor Ruby in uw web app.

## <a id="howto_make_call"></a>Procedure: een uitgaand gesprek
Hieronder ziet u hoe u een uitgaande oproep. Belangrijke concepten zijn REST API-aanroepen met de bibliotheek Twilio helper voor Ruby en TwiML weergeven. Vervang de waarden voor de telefoonnummers **van** en **naar** en zorg ervoor dat u controleert of het telefoonnummer **van** voor uw account Twilio voor het uitvoeren van de code.

Deze functie toevoegen `web.md`:

    # Set your account ID and authentication token.
    sid = "your_twilio_account_sid";
    token = "your_twilio_authentication_token";

    # The number of the phone initiating the the call.
    # This should either be a Twilio number or a number that you've verified
    from = "NNNNNNNNNNN";

    # The number of the phone receiving call.
    to = "NNNNNNNNNNN";

    # Use the Twilio-provided site for the TwiML response.
    url = "http://yourdomain.cloudapp.net/voice_url";
      
    get '/make_call' do
      # Create the call client.
      client = Twilio::REST::Client.new(sid, token);
      
      # Make the call
      client.account.calls.create(to: to, from: from, url: url)
    end

    post '/voice_url' do
      "<Response>
         <Say>Hello Monkey!</Say>
       </Response>"
    end
    
Als u open tot `http://yourdomain.cloudapp.net/make_call` in een browser, die de aanroep van de API Twilio de oproep wordt geactiveerd. De eerste twee parameters in `client.account.calls.create` zijn vrij geen uitleg: het nummer van de oproep is `from` en het nummer van de oproep is `to`. 

De derde parameter (`url`) is de URL die Twilio aanvragen voor instructies over wat te doen wanneer de oproep is verbonden. In dit geval we plaatsen een URL (`http://yourdomain.cloudapp.net`) die een eenvoudig TwiML-document als resultaat gegeven en wordt de `<Say>` opdracht voor het doen van bepaalde tekst naar spraak en zeg "Hallo Monkey" aan de persoon die de oproep ontvangt.

## <a id="howto_recieve_sms"></a>Procedure: een SMS-bericht ontvangen
In het vorige voorbeeld wij een **Uitgaand** telefoongesprek gestart. Deze keer, gebruiken we het telefoonnummer dat de Twilio gaf ons tijdens het aanmelden voor het verwerken van **inkomende** SMS-bericht.

Eerste, log in op uw [dashboard Twilio][twilio_account]. Klik op 'Numbers' in de top nav en klik vervolgens op in het Twilio dat u zijn. Hier ziet u twee URL's die u kunt configureren. Aanvraag URL een mondeling verzoek-URL en een SMS-bericht. Dit zijn de URL's die wordt aangeroepen wanneer een telefoongesprek wordt gemaakt of een SMS-bericht wordt verzonden naar het nummer van uw Twilio. De URL's worden ook wel bekend als "web haken".

We willen graag inkomende SMS-berichten, dus laten we werken de URL van `http://yourdomain.cloudapp.net/sms_url`. Ga verder en klik op wijzigingen opslaan onder aan de pagina. Nu, back- `web.rb` we onze toepassing voor het verwerken van dit programma:

    post '/sms_url' do
      "<Response>
         <Message>Hey, thanks for the ping! Twilio and Azure rock!</Message>
       </Response>"
    end

Zorg dat u uw web app opnieuw starten na het aanbrengen van de wijziging. Nu pakt de telefoon en een SMS-bericht verzenden naar het nummer van uw Twilio. Zo spoedig mogelijk ontvangt u een SMS-antwoord dat "Hoi, Bedankt voor de opdracht ping zegt! Twilio en Azure rock! ".

## <a id="additional_services"></a>Procedure: extra Twilio Services gebruiken
Naast de voorbeelden die hier worden weergegeven, biedt Twilio web-API's waarmee u kunt profiteren van extra functionaliteit van de Twilio van uw toepassing Azure. Zie de [Twilio API-documentatie]voor volledige details [twilio_api_documentation].

### <a id="NextSteps"></a>Volgende stappen
U hebt geleerd dat de basisbeginselen van de Twilio-service, klik op deze koppelingen voor meer informatie:

* [Richtlijnen voor beveiliging van Twilio] [twilio_security_guidelines]
* [Twilio HowTos en voorbeeldcode] [twilio_howtos]
* [Twilio Quickstart zelfstudies][twilio_quickstarts] 
* [Twilio op GitHub] [twilio_on_github]
* [Neem contact op met ondersteuning voor Twilio] [twilio_support]

[twilio_ruby]: https://www.twilio.com/docs/ruby/install





[twilio_pricing]: http://www.twilio.com/pricing
[special_offer]: http://ahoy.twilio.com/azure
[twilio_libraries]: https://www.twilio.com/docs/libraries
[twiml]: http://www.twilio.com/docs/api/twiml
[twilio_api]: http://www.twilio.com/api
[try_twilio]: https://www.twilio.com/try-twilio
[twilio_account]:  https://www.twilio.com/user/account
[verify_phone]: https://www.twilio.com/user/account/phone-numbers/verified#
[twilio_api_documentation]: http://www.twilio.com/api
[twilio_security_guidelines]: http://www.twilio.com/docs/security
[twilio_howtos]: http://www.twilio.com/docs/howto
[twilio_on_github]: https://github.com/twilio
[twilio_support]: http://www.twilio.com/help/contact
[twilio_quickstarts]: http://www.twilio.com/docs/quickstart
[sinatra]: http://www.sinatrarb.com/
[azure_vm_setup]: http://www.windowsazure.com/develop/ruby/tutorials/web-app-with-linux-vm/
