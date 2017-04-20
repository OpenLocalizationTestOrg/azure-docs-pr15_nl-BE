<properties
    pageTitle="Hoe u een telefoongesprek van Twilio (PHP) | Microsoft Azure"
    description="Informatie over het bellen en verzenden van een SMS-bericht met de API Twilio service op Azure. Voorbeelden zijn voor PHP-toepassing."
    documentationCenter="php"
    services=""
    authors="devinrader"
    manager="twilio"
    editor="mollybos"/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="PHP"
    ms.topic="article"
    ms.date="11/25/2014"
    ms.author="microsofthelp@twilio.com"/>

# <a name="how-to-make-a-phone-call-using-twilio-in-a-php-application-on-azure"></a>Het maken van een telefoongesprek met Twilio in een PHP-toepassing op Azure

In het volgende voorbeeld ziet u hoe u Twilio kunt gebruiken om een oproep van een PHP-webpagina in Azure. De uiteindelijke toepassing wordt de gebruiker voor het telefoongesprek waarden, zoals in de volgende schermafdruk.

![Azure Call-formulier met behulp van Twilio en PHP][twilio_php]

U moet voor het gebruik van de code in dit onderwerp als volgt:

1. Verwerving van een Twilio account en verificatie token. Evalueren om te beginnen met Twilio, prijzen op [http://www.twilio.com/pricing][twilio_pricing]. U kunt zich aanmelden voor een proef-account op [https://www.twilio.com/try-twilio][try_twilio]. Zie voor informatie over de API die door Twilio [http://www.twilio.com/api][twilio_api].
2. De [bibliotheek voor PHP Twilio](https://github.com/twilio/twilio-php) verkrijgen of installeren als een pakket PERENBOMEN. Raadpleeg het [Leesmij-bestand](https://github.com/twilio/twilio-php/blob/master/README.md)voor meer informatie.
3. De Azure SDK voor PHP installeren. Zie [de SDK Azure voor PHP instellen]voor een overzicht van de SDK en de instructies voor het installeren van het,[setup_php_sdk].

## <a name="create-a-web-form-for-making-a-call"></a>Een webformulier voor een verbinding maken

De volgende HTML-code ziet u hoe u een webpagina (**callform.html**) die gegevens voor een verbinding van de gebruiker opgehaald:

    <html>
    <head>
        <title>Automated call form</title>
    </head>
    <body>
    <h1>Automated Call Form</h1>
    <p>Fill in all fields and click <b>Make this call</b>.</p>
    <form action="makecall.php" method="post">
    <table>
        <tr>
            <td>To:</td>
            <td><input type="text" size=50 name="callTo" value=""></td>
        </tr>
        <tr>
            <td>From:</td>
            <td><input type="text" size=50 name="callFrom" value=""></td>
        </tr>
        <tr>
            <td>Call message:</td>
            <td><input type="text" size=100 name="callText" value="Hello. This is the call text. Good bye." /></td>
        </tr>
        <tr>
            <td colspan=2><input type="submit" value="Make this call"></td>
        </tr>
    </table>
    </form>
    <br/>
    </body>
    </html>

## <a name="create-the-code-to-make-the-call"></a>Maken van de code om te kunnen bellen
De volgende code toont hoe u een webpagina (**makecall.php**) die wordt aangeroepen wanneer de gebruiker het formulier dat wordt weergegeven door de **callform.html**indient. Onderstaande code maakt het bericht aanroep en de aanroep genereert. (Gebruik uw Twilio account en verificatie in plaats van de tijdelijke aanduiding waarden toegewezen aan **$sid** en **$token** in de onderstaande code token).

    <html>
    <head><title>Making call...</title></head>
    <body>
    <p>Your call is being made.</p>

    <?php
    require_once 'Services/Twilio.php';

    $sid = "your_account_sid";
    $token = "your_authentication_token";

    $from_number = $_POST['callFrom']; // Calls must be made from a registered Twilio number.
    $to_number = $_POST['callTo'];
    $message = $_POST['callText'];

    $client = new Services_Twilio($sid, $token, "2010-04-01");

    $call = $client->account->calls->create(
        $from_number,
        $to_number,
        'http://twimlets.com/message?Message='.urlencode($message)
    );

    echo "Call status: ".$call->status."<br />";
    echo "URI resource: ".$call->uri."<br />";
    ?>
    </body>
    </html>

**Makecall.php** geeft de oproep te maken, zodat sommige oproep metagegevens (zoals weergegeven in de screenshot hieronder). Zie voor meer informatie over metagegevens oproep, [https://www.twilio.com/docs/api/rest/call#instance-properties][twilio_call_properties].

![Azure oproep reactie met Twilio en PHP][twilio_php_response]

## <a name="run-the-application"></a>De toepassing wordt uitgevoerd
De volgende stap is het implementeren van uw toepassing naar Azure Websites. De volgende artikelen bevatten informatie voor een website maken en implementeren van uw code met Git, FTP- of WebMatrix (hoewel niet alle informatie in elk artikel is van toepassing):

* [Een PHP-MySQL Azure-website maken en implementeren met behulp van Git][website-git]
* [Een PHP-MySQL Azure-website maken en implementeren met behulp van FTP][website-ftp]

## <a name="next-steps"></a>Volgende stappen
Deze code is opgegeven met Twilio in PHP op Azure basisfunctionaliteit weergeven. Voordat u implementeert voor Azure in productie, kunt u foutafhandeling meer of andere functies toe te voegen. Bijvoorbeeld:

* In plaats van een webformulier, kunt u opslag Azure BLOB's of SQL-Database opgeslagen telefoonnummers en tekst. Zie voor meer informatie over het gebruik van opslag Azure BLOB's in PHP [Azure-opslag met PHP toepassingen met behulp van][howto_blob_storage_php]. Zie voor informatie over het gebruik van SQL-Database in PHP [Met behulp van SQL-Database met PHP toepassingen][howto_sql_azure_php].
* URL van Twilio maakt gebruik van de code **makecall.php** ([http://twimlets.com/message][twimlet_message_url]) een Twilio Markup Language (TwiML) van Twilio te informeren hoe verder te gaan met het gesprek antwoord te geven. Bijvoorbeeld, de TwiML die wordt geretourneerd kan bevatten een `<Say>` bewerking waardoor de tekst die naar de ontvanger wordt gesproken. In plaats van de geleverde Twilio URL, kan samenstellen van uw eigen service om te reageren op de aanvraag van Twilio; Zie voor meer informatie, [het gebruik van Twilio voor spraak- en SMS-mogelijkheden van PHP][howto_twilio_voice_sms_php]. Meer informatie over TwiML vindt u op [http://www.twilio.com/docs/api/twiml][twiml], en meer informatie over `<Say>` en andere Twilio bewerkingen kunnen u vinden op [http://www.twilio.com/docs/api/twiml/say][twilio_say].
* Lees de richtlijnen op [https://www.twilio.com/docs/security]Twilio[twilio_docs_security].

Zie voor meer informatie over Twilio, [https://www.twilio.com/docs][twilio_docs].

## <a name="see-also"></a>Zie ook
* [Het gebruik van Twilio voor spraak- en SMS-mogelijkheden in PHP](partner-twilio-php-how-to-use-voice-sms.md)

[twilio_pricing]: http://www.twilio.com/pricing
[try_twilio]: http://www.twilio.com/try-twilio
[twilio_api]: http://www.twilio.com/api
[verify_phone]: https://www.twilio.com/user/account/phone-numbers/verified#
[setup_php_sdk]: http://azurephp.interoperabilitybridges.com/articles/setup-the-windows-azure-sdk-for-php
[twimlet_message_url]: http://twimlets.com/message
[twiml]: http://www.twilio.com/docs/api/twiml
[twilio_api_service]: http://api.twilio.com
[build_php_azure_app]: http://azurephp.interoperabilitybridges.com/articles/build-and-deploy-a-windows-azure-php-application
[howto_twilio_voice_sms_php]: partner-twilio-php-how-to-use-voice-sms.md
[howto_blob_storage_php]: http://azure.microsoft.com/documentation/articles/storage-php-how-to-use-blobs/
[howto_sql_azure_php]: http://azure.microsoft.com/documentation/articles/sql-database-php-how-to-use/
[twilio_call_properties]: https://www.twilio.com/docs/api/rest/call#instance-properties
[twilio_docs_security]: http://www.twilio.com/docs/security
[twilio_docs]: http://www.twilio.com/docs
[twilio_say]: http://www.twilio.com/docs/api/twiml/say
[ssl_validation]: http://readthedocs.org/docs/twilio-php/en/latest/usage/rest.html
[twilio_php]: ./media/partner-twilio-php-make-phone-call/WA_TwilioPHPCallForm.jpg
[twilio_php_response]: ./media/partner-twilio-php-make-phone-call/WA_TwilioPHPMakeCall.jpg
[website-git]: ./web-sites/web-sites-php-mysql-deploy-use-git.md
[website-ftp]: ./web-sites/web-sites-php-mysql-deploy-use-ftp.md
[twilio_php_github]: https://github.com/twilio/twilio-php
