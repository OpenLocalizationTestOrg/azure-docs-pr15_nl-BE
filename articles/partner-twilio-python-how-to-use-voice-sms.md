<properties
    pageTitle="Het gebruik van Twilio voor spraak- en SMS (PHP) | Microsoft Azure"
    description="Informatie over het bellen en verzenden van een SMS-bericht met de API Twilio service op Azure. Voorbeelden van code is geschreven in PHP."
    services=""
    documentationCenter="python"
    authors="devinrader"
    manager="twilio"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="python" 
    ms.topic="article"
    ms.date="02/19/2015"
    ms.author="MicrosoftHelp@twilio.com"/>





# <a name="how-to-use-twilio-for-voice-and-sms-capabilities-in-php"></a>Het gebruik van Twilio voor spraak- en SMS-mogelijkheden in PHP
Deze handleiding wordt beschreven hoe algemene programmeertaken met de API Twilio-service uitvoeren op Azure. De scenario's die zijn te bellen en verzenden van een bericht Short Message Service (SMS). Zie de sectie [Volgende stappen](#NextSteps) voor meer informatie over Twilio en het gebruik van spraak en SMS in uw toepassingen.

## <a id="WhatIs"></a>Wat is Twilio?
Twilio is dat de toekomst van zakelijke communicatie, waarmee ontwikkelaars ingesloten gesproken, VoIP, berichten en in toepassingen. Ze virtualiseren alle infrastructuur die nodig zijn in een cloud-gebaseerde globale omgeving, weer via het platform Twilio communicatie API. Toepassingen zijn eenvoudig te maken en schaalbaar. Geniet van flexibiliteit met betalen-als-je prijzen gaan en profiteren van de betrouwbaarheid van de wolk.

**Twilio spraak** kunt uw toepassingen en telefoongesprekken ontvangen. **Twilio SMS** kan uw toepassing te verzenden en ontvangen van SMS-berichten. **Twilio-Client** kunt u VoIP-oproepen vanuit een telefoon, Tablet PC of een browser en WebRTC ondersteunt.

## <a id="Pricing"></a>Twilio prijzen en speciale aanbiedingen

Azure klanten ontvangen [speciale aanbieding] $10 Twilio Credit wanneer u een upgrade uitvoert van uw Twilio Account. Deze Twilio Credit kan worden toegepast op elk gebruik van Twilio ($10 credit gelijk aan maar liefst 1000 SMS-berichten verzenden of ontvangen van maximaal 1000 inkomende Voice minuten, afhankelijk van de locatie van uw telefoon en bericht of oproep bestemming). Dit krediet Twilio verzilveren en op slag: [ahoy.twilio.com/azure].

Twilio is een "pay-as-you-go"-service. Er zijn geen abonnementskosten instellen en u kunt uw account op elk moment sluiten. U kunt meer informatie vinden op [Twilio prijzen] [twilio_pricing].

## <a id="Concepts"></a>Concepten
De Twilio-API is een RESTful API die spraak- en SMS-functionaliteit voor toepassingen biedt. Clientbibliotheken zijn beschikbaar in meerdere talen; Zie voor een lijst, [API-bibliotheken Twilio] [twilio_libraries].

Belangrijke aspecten van de Twilio-API zijn bewerkingen van Twilio en Twilio Markup Language (TwiML).

### <a id="Verbs"></a>Twilio woorden
De API wordt gebruik gemaakt van Twilio woorden; bijvoorbeeld, de ** &lt;zeg&gt; ** opdracht geïnstrueerd Twilio hoorbaar een bericht te bezorgen in een gesprek.

Hier volgt een lijst met bewerkingen voor Twilio. Meer informatie over de andere opdrachten en mogelijkheden via [documentatie Twilio Markup Language] [http://www.twilio.com/docs/api/twiml].

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

### <a id="TwiML"></a>TwiML
TwiML is een set XML-gebaseerde instructies op basis van de Twilio woorden die in de Twilio van het verwerken van een oproep of SMS op de hoogte.

Als u bijvoorbeeld zou het volgende TwiML omzetten in de tekst **Hallo wereld** spraak.

    <?xml version="1.0" encoding="UTF-8" ?>
    <Response>
       <Say>Hello World</Say>
    </Response>

Wanneer uw toepassing de Twilio-API aanroept, is een van de parameters van de API de URL die het antwoord TwiML wordt. Voor ontwikkelingsdoeleinden, kunt u URL's Twilio geleverd om de antwoorden van de TwiML die wordt gebruikt door uw toepassingen. U kan ook host uw eigen URL's voor de productie van de antwoorden van de TwiML en een andere optie is het **TwiMLResponse** -object gebruiken.

Zie voor meer informatie over de Twilio woorden, hun kenmerken en TwiML [TwiML] [twiml]. Zie voor meer informatie over de API Twilio [API Twilio] [twilio_api].

## <a id="CreateAccount"></a>Maak een Account Twilio
Wanneer u klaar bent om een Twilio-account, aanmelden bij [Probeer Twilio] [try_twilio]. U kunt beginnen met een gratis account en uw account later bijwerken.

Wanneer u zich voor een Twilio-account aanmeldt, ontvangt u een account-ID en een verificatietoken. Beide moesten Twilio API-aanroepen. Als u wilt voorkomen dat onbevoegde toegang tot uw account, uw verificatietoken veilig houden. Uw account-ID en verificatie tokens kunnen worden weergegeven op de [pagina Twilio] [twilio_account], in de velden met de naam **SID rekening** en **AUTH TOKEN**, respectievelijk.

## <a id="create_app"></a>Een PHP-toepassing maken
Een PHP-toepassing die gebruikmaakt van de service Twilio en wordt uitgevoerd in Azure is niet anders dan een andere PHP-toepassing die gebruikmaakt van de service Twilio. Terwijl Twilio services REST gebaseerd zijn op verschillende manieren kunnen worden aangeroepen vanuit PHP, in dit artikel zal zich richten op Twilio services gebruiken met [bibliotheek voor PHP van GitHub Twilio][twilio_php]. Zie voor meer informatie over het gebruik van de Twilio-bibliotheek voor PHP [http://readthedocs.org/docs/twilio-php/en/latest/index.html][twilio_lib_docs].

Gedetailleerde instructies voor het maken en implementeren van een Twilio/PHP-toepassing naar Azure zijn beschikbaar op [het maken van een telefoongesprek met behulp van Twilio in een PHP-toepassing op Azure][howto_phonecall_php].

## <a id="configure_app"></a>Uw toepassing gebruik van bibliotheken Twilio configureren
U kunt uw toepassing de Twilio-bibliotheek voor PHP op twee manieren configureren:

1. Downloaden van de Twilio-bibliotheek voor PHP van GitHub ([https://github.com/twilio/twilio-php][twilio_php]) en de map **Services** toevoegen aan uw toepassing.

    -OF-

2. De Twilio-bibliotheek voor PHP installeren als een pakket PERENBOMEN. Het kan worden geïnstalleerd met de volgende opdrachten:

        $ pear channel-discover twilio.github.com/pear
        $ pear install twilio/Services_Twilio

Zodra u de Twilio bibliotheek voor PHP hebt geïnstalleerd, kunt u een instructie **require_once** vervolgens toevoegen aan de bovenkant van de PHP-bestanden als u verwijst naar de bibliotheek:

        require_once 'Services/Twilio.php';

Voor meer informatie, Zie [https://github.com/twilio/twilio-php/blob/master/README.md][twilio_github_readme].

## <a id="howto_make_call"></a>Procedure: een uitgaand gesprek
Hieronder ziet u hoe u een uitgaande oproep met behulp van de klasse **Services_Twilio** . Een Twilio opgegeven site deze code ook gebruikt om het antwoord Twilio Markup Language (TwiML). Vervang de waarden voor de telefoonnummers **van** en **naar** en zorg ervoor dat u controleert of het telefoonnummer **van** voor uw account Twilio voor het uitvoeren van de code.

    // Include the Twilio PHP library.
    require_once 'Services/Twilio.php';

    // Library version.
    $version = "2010-04-01";

    // Set your account ID and authentication token.
    $sid = "your_twilio_account_sid";
    $token = "your_twilio_authentication_token";

    // The number of the phone initiating the the call.
    // (Must be previously validated with Twilio.)
    $from_number = "NNNNNNNNNNN";

    // The number of the phone receiving call.
    $to_number = "NNNNNNNNNNN";

    // Use the Twilio-provided site for the TwiML response.
    $url = "http://twimlets.com/message";

    // The phone message text.
    $message = "Hello world.";

    // Create the call client.
    $client = new Services_Twilio($sid, $token, $version);

    //Make the call.
    try
    {
        $call = $client->account->calls->create(
            $from_number,
            $to_number,
            $url.'?Message='.urlencode($message)
        );
    }
    catch (Exception $e)
    {
        echo 'Error: ' . $e->getMessage();
    }

Zoals gezegd, wordt deze code een Twilio opgegeven site het antwoord TwiML. U kan uw eigen site gebruiken om het TwiML antwoord; voor meer informatie, Zie [TwiML antwoorden bieden op uw eigen website](#howto_provide_twiml_responses).


- **Opmerking**: Zie [http://readthedocs.org/docs/twilio-php/en/latest/usage/rest.html] voor het oplossen van fouten met gegevensvalidatie SSL certificaat[ssl_validation]


## <a id="howto_send_sms"></a>Procedure: een SMS-bericht verzenden
Hieronder ziet u hoe u een SMS-bericht verzenden met de klasse **Services_Twilio** . Het nummer **van** wordt geleverd door de Twilio voor evaluatie accounts voor het verzenden van SMS-berichten. **Het nummer** moet worden gecontroleerd voor uw Twilio account voordat u de code uitvoert.

    // Include the Twilio PHP library.
    require_once 'Services/Twilio.php';

    // Library version.
    $version = "2010-04-01";

    // Set your account ID and authentication token.
    $sid = "your_twilio_account_sid";
    $token = "your_twilio_authentication_token";


    $from_number = "NNNNNNNNNNN"; // With trial account, texts can only be sent from your Twilio number.
    $to_number = "NNNNNNNNNNN";
    $message = "Hello world.";

    // Create the call client.
    $client = new Services_Twilio($sid, $token, $version);

    // Send the SMS message.
    try
    {
        $client->account->sms_messages->create($from_number, $to_number, $message);
    }
    catch (Exception $e)
    {
        echo 'Error: ' . $e->getMessage();
    }

## <a id="howto_provide_twiml_responses"></a>Procedure: bieden van antwoorden van de TwiML van uw eigen Website
Wanneer uw toepassing een aanroep van de API Twilio initieert, wordt Twilio uw aanvraag verzenden naar een URL die wordt verwacht dat het resultaat van TwiML. In het bovenstaande voorbeeld wordt de URL van Twilio [http://twimlets.com/message][twimlet_message_url]. (TwiML is ontworpen voor gebruik door Twilio, ziet u de it in uw browser. Klik bijvoorbeeld op [http://twimlets.com/message] [ twimlet_message_url] voor een overzicht van een lege `<Response>` element; Klik bijvoorbeeld op [http://twimlets.com/message?Message%5B0%5D=Hello%20World] [ twimlet_message_url_hello_world] voor een overzicht van een `<Response>` element met een `<Say>` element.)

Niet vertrouwen op de geleverde Twilio URL, kunt u uw eigen site die HTTP-antwoorden als resultaat geeft. U kunt de site maken in elke taal die XML-antwoorden retourneert; in dit onderwerp wordt ervan uitgegaan dat u PHP gebruikt voor het maken van de TwiML.

De volgende PHP-pagina resulteert in een TwiML reactie die **Hallo wereld** op het gesprek zegt.

    <?php
        header("content-type: text/xml");
        echo "<?xml version=\"1.0\" encoding=\"UTF-8\"?>\n";
    ?>
    <Response>
        <Say>Hello world.</Say>
    </Response>

Als u in het bovenstaande voorbeeld zien kunt, is het antwoord TwiML gewoon een XML-document. De Twilio-bibliotheek voor PHP bevat klassen die TwiML voor u genereert. In het volgende voorbeeld levert het gelijkwaardige antwoord zoals hierboven, maar gebruikt de **Services\_Twilio\_Twiml** klasse in de bibliotheek Twilio voor PHP:

    require_once('Services/Twilio.php');

    $response = new Services_Twilio_Twiml();
    $response->say("Hello world.");
    print $response;

Zie voor meer informatie over TwiML, [https://www.twilio.com/docs/api/twiml][twiml_reference].

Zodra u uw PHP-pagina ingesteld op TwiML antwoorden geven hebt, de URL van de PHP-pagina gebruiken als de URL doorgegeven aan de `Services_Twilio->account->calls->create` methode. Bijvoorbeeld, als u een webtoepassing met de naam **MyTwiML** geïmplementeerd op een Azure gehoste service, en de naam van de PHP-pagina **mytwiml.php**, de URL kan worden doorgegeven aan **Services_Twilio -> account -> oproepen -> maken** zoals in het volgende voorbeeld wordt getoond:

    require_once 'Services/Twilio.php';

    $sid = "your_twilio_account_sid";
    $token = "your_twilio_authentication_token";
    $from_number = "NNNNNNNNNNN";
    $to_number = "NNNNNNNNNNN";
    $url = "http://<your_hosted_service>.cloudapp.net/MyTwiML/mytwiml.php";

    // The phone message text.
    $message = "Hello world.";

    $client = new Services_Twilio($sid, $token, "2010-04-01");

    try
    {
        $call = $client->account->calls->create(
            $from_number,
            $to_number,
            $url.'?Message='.urlencode($message)
        );
    }
    catch (Exception $e)
    {
        echo 'Error: ' . $e->getMessage();
    }

Zie voor meer informatie over het gebruik van Twilio in Azure met PHP, [het maken van een telefoongesprek met behulp van Twilio in een PHP-toepassing op Azure][howto_phonecall_php].

## <a id="AdditionalServices"></a>Procedure: extra Twilio Services gebruiken
Naast de voorbeelden die hier worden weergegeven, biedt Twilio web-API's waarmee u kunt profiteren van extra functionaliteit van de Twilio van uw toepassing Azure. Zie de [Twilio API-documentatie]voor volledige details [twilio_api_documentation].

## <a id="NextSteps"></a>Volgende stappen
U hebt geleerd dat de basisbeginselen van de Twilio-service, klik op deze koppelingen voor meer informatie:

* [Richtlijnen voor beveiliging van Twilio] [twilio_security_guidelines]
* [Twilio procedure hulplijnen en voorbeeldcode] [twilio_howtos]
* [Twilio Quickstart zelfstudies][twilio_quickstarts]
* [Twilio op GitHub] [twilio_on_github]
* [Neem contact op met ondersteuning voor Twilio] [twilio_support]

[twilio_php]: https://github.com/twilio/twilio-php
[twilio_lib_docs]: http://readthedocs.org/docs/twilio-php/en/latest/index.html
[twilio_github_readme]: https://github.com/twilio/twilio-php/blob/master/README.md
[ssl_validation]: http://readthedocs.org/docs/twilio-php/en/latest/usage/rest.html

[howto_phonecall_php]: http://windowsazure.com/documentation/articles/partner-twilio-php-make-phone-call
[twimlet_message_url]: http://twimlets.com/message
[twimlet_message_url_hello_world]: http://twimlets.com/message?Message%5B0%5D=Hello%20World
[twiml_reference]: https://www.twilio.com/docs/api/twiml
[twilio_pricing]: http://www.twilio.com/pricing

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
