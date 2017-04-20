<properties 
    pageTitle="Het gebruik van Twilio voor spraak- en SMS (Java) | Microsoft Azure" 
    description="Informatie over het bellen en verzenden van een SMS-bericht met de API Twilio service op Azure. Voorbeelden van code is geschreven in Java." 
    services="" 
    documentationCenter="java" 
    authors="devinrader" 
    manager="twilio" 
    editor="mollybos"/>

<tags 
    ms.service="multiple" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="Java" 
    ms.topic="article" 
    ms.date="11/25/2014" 
    ms.author="microsofthelp@twilio.com"/>

# <a name="how-to-use-twilio-for-voice-and-sms-capabilities-in-java"></a>Het gebruik van Twilio voor spraak- en SMS-mogelijkheden in Java

Deze handleiding wordt beschreven hoe algemene programmeertaken met de API Twilio-service uitvoeren op Azure. De scenario's die zijn te bellen en verzenden van een bericht Short Message Service (SMS). Zie de sectie [Volgende stappen](#NextSteps) voor meer informatie over Twilio en het gebruik van spraak en SMS in uw toepassingen.

## <a id="WhatIs"></a>Wat is Twilio?
Twilio is een web service-API voor telefonie kunt u uw bestaande web-talen en vaardigheden via spraak- en SMS-toepassingen bouwen. Twilio is een service van derden (niet een Azure-functie en niet in een Microsoft-product).

**Twilio spraak** kunt uw toepassingen en telefoongesprekken ontvangen. **Twilio SMS** kunt uw toepassingen en ontvangen van SMS-berichten. **Twilio-Client** kunt uw toepassingen gesproken communicatie met bestaande Internet-verbindingen, met inbegrip van mobiele verbindingen inschakelen.

## <a id="Pricing"></a>Twilio prijzen en speciale aanbiedingen
Informatie over prijzen Twilio is beschikbaar op de [Prijzen van Twilio] [twilio_pricing]. Azure-klanten krijgen een [speciale aanbieding][special_offer]: een gratis krediet van 1000 teksten of 1000 minuten inkomend. Aanmelden voor dit aanbod of meer informatie, Ga naar [http://ahoy.twilio.com/azure][special_offer].  

## <a id="Concepts"></a>Concepten
De Twilio-API is een RESTful API die spraak- en SMS-functionaliteit voor toepassingen biedt. Clientbibliotheken zijn beschikbaar in meerdere talen; Zie voor een lijst, [API-bibliotheken Twilio] [twilio_libraries].

Belangrijke aspecten van de Twilio-API zijn bewerkingen van Twilio en Twilio Markup Language (TwiML).

### <a id="Verbs"></a>Twilio woorden
De API wordt gebruik gemaakt van Twilio woorden; bijvoorbeeld, de ** &lt;zeg&gt; ** opdracht geïnstrueerd Twilio hoorbaar een bericht te bezorgen in een gesprek. 

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
Wanneer u klaar bent om een Twilio-account, aanmelden bij [Twilio probeer] [try_twilio]. U kunt beginnen met een gratis account en uw account later bijwerken.

Wanneer u zich voor een Twilio-account aanmeldt, ontvangt u een account-ID en een verificatietoken. Beide moesten Twilio API-aanroepen. Als u wilt voorkomen dat onbevoegde toegang tot uw account, uw verificatietoken veilig houden. Uw account-ID en verificatie tokens kunnen worden weergegeven op de [pagina Twilio] [twilio_account], in de velden met de naam **SID rekening** en **AUTH TOKEN**, respectievelijk.

## <a id="create_app"></a>Een Java-toepassing maken
1. De JAR-Twilio downloaden en toe te voegen aan uw bouwpad Java en uw implementatie WAR assembly. Op [https://github.com/twilio/twilio-java][twilio_java], de bronnen van GitHub downloaden en maak uw eigen POTJES of een vooraf gebouwde JAR (met of zonder de afhankelijkheden) downloaden.
2. Waarborgen van de JDK **cacerts** keystore bevat het certificaat van de CA Secure Equifax met MD5-vingerafdruk 67:CB:9 D: C0:13:24:8A:82:9B:B2:17:1E:D1:1B:EC:D4 (het serienummer is 35:DE:F4:CF en de SHA1-vingerafdruk is D2:32:09:AD:23:D3:14:23:21:74:E4:0 D: 7F:9 D: 62:13:97:86:63:3A). Dit is het certificaat van de certificate authority (CA) voor de [https://api.twilio.com] [ twilio_api_service] -service, die wordt aangeroepen wanneer u Twilio APIs. Voor meer informatie over het waarborgen van de JDK **cacerts** keystore de juiste CA-certificaat bevat, raadpleegt u [een certificaat in het certificaatarchief van de Java-CA toe te voegen][add_ca_cert].

Gedetailleerde instructies voor het gebruik van de bibliotheek Twilio client for Java zijn beschikbaar op [het maken van een telefoongesprek met behulp van Twilio in een Java-toepassing op Azure][howto_phonecall_java].

## <a id="configure_app"></a>Uw toepassing gebruik van bibliotheken Twilio configureren
In uw code, kunt u **importeren** instructies toevoegen aan de bovenkant van de bronbestanden voor de pakketten Twilio of klassen die u wilt gebruiken in uw toepassing. 

Voor de bronbestanden van Java:

    import com.twilio.*;
    import com.twilio.sdk.*;
    import com.twilio.sdk.resource.factory.*;
    import com.twilio.sdk.resource.instance.*;

Bronbestanden van Java Server-pagina (JSP):

    import="com.twilio.*"
    import="com.twilio.sdk.*"
    import="com.twilio.sdk.resource.factory.*"
    import="com.twilio.sdk.resource.instance.*"
Afhankelijk van welke pakketten Twilio of klassen die u gebruiken wilt, afwijken uw afschriften **importeren** .

## <a id="howto_make_call"></a>Procedure: een uitgaand gesprek
Hieronder ziet u hoe u een uitgaande oproep met behulp van de klasse **CallFactory** . Een Twilio opgegeven site deze code ook gebruikt om het antwoord Twilio Markup Language (TwiML). Vervang de waarden voor de telefoonnummers **van** en **naar** en zorg ervoor dat u controleert of het telefoonnummer **van** voor uw account Twilio voor het uitvoeren van de code.

    // Use your account SID and authentication token instead
    // of the placeholders shown here.
    String accountSID = "your_twilio_account";
    String authToken = "your_twilio_authentication_token";

    // Create an instance of the Twilio client.
    TwilioRestClient client;
    client = new TwilioRestClient(accountSID, authToken);

    // Retrieve the account, used later to create an instance of the CallFactory.
    Account account = client.getAccount();

    // Use the Twilio-provided site for the TwiML response.
    String Url="http://twimlets.com/message";
    Url = Url + "?Message%5B0%5D=Hello%20World";

    // Place the call From, To and URL values into a hash map. 
    HashMap<String, String> params = new HashMap<String, String>();
    params.put("From", "NNNNNNNNNN"); // Use your own value for the second parameter.
    params.put("To", "NNNNNNNNNN");   // Use your own value for the second parameter.
    params.put("Url", Url);

    // Create an instance of the CallFactory class.
    CallFactory callFactory = account.getCallFactory();

    // Make the call.
    Call call = callFactory.create(params);

Zie voor meer informatie over de parameters die worden doorgegeven aan de methode **CallFactory.create** [http://www.twilio.com/docs/api/rest/making-calls][twilio_rest_making_calls].

Zoals gezegd, wordt deze code een Twilio opgegeven site het antwoord TwiML. U kan uw eigen site gebruiken om het TwiML antwoord; voor meer informatie, Zie [TwiML antwoorden bieden in een Java-toepassing op Azure](#howto_provide_twiml_responses).

## <a id="howto_send_sms"></a>Procedure: een SMS-bericht verzenden
Hieronder ziet u hoe u een SMS-bericht verzenden met de klasse **SmsFactory** . **Vanaf** nummer, **4155992671**, wordt geleverd door de Twilio voor evaluatie accounts voor het verzenden van SMS-berichten. **Het nummer** moet worden gecontroleerd voor uw Twilio account voordat u de code uitvoert.

    // Use your account SID and authentication token instead
    // of the placeholders shown here.
    String accountSID = "your_twilio_account";
    String authToken = "your_twilio_authentication_token";

    // Create an instance of the Twilio client.
    TwilioRestClient client;
    client = new TwilioRestClient(accountSID, authToken);

    // Retrieve the account, used later to create an instance of the SmsFactory.
    Account account = client.getAccount();

    // Send an SMS message.
    MessageFactory messageFactory = account.getMessageFactory();
    
    List<NameValuePair> params = new ArrayList<NameValuePair>();
    params.add(new BasicNameValuePair("To", "+14159352345")); // Replace with a valid phone number for your account.
    params.add(new BasicNameValuePair("From", "+14158141829")); // Replace with a valid phone number for your account.
    params.add(new BasicNameValuePair("Body", "Where's Wallace?"));
    
    Message sms = messageFactory.create(params);
        
Zie voor meer informatie over de parameters die worden doorgegeven aan de methode **SmsFactory.create** [http://www.twilio.com/docs/api/rest/sending-sms][twilio_rest_sending_sms].

## <a id="howto_provide_twiml_responses"></a>Procedure: bieden van antwoorden van de TwiML van uw eigen Website
Wanneer uw toepassing een aanroep van de API Twilio initieert, stuurt bijvoorbeeld via de methode **CallFactory.create** Twilio uw aanvraag voor een URL die wordt verwacht dat het resultaat van TwiML. In het bovenstaande voorbeeld wordt de URL van Twilio [http://twimlets.com/message][twimlet_message_url]. (TwiML is ontworpen voor gebruik door webservices, ziet u de TwiML in uw browser. Klik bijvoorbeeld op [http://twimlets.com/message] [ twimlet_message_url] voor een overzicht van een lege ** &lt;antwoord&gt; ** -element; Klik bijvoorbeeld op [http://twimlets.com/message?Message%5B0%5D=Hello%20World] [ twimlet_message_url_hello_world] voor een overzicht van een ** &lt;antwoord&gt; ** element met een ** &lt;zeg&gt; ** element.)

Niet vertrouwen op de geleverde Twilio URL, kunt u uw eigen URL site die HTTP-antwoorden als resultaat geeft. U kunt de site maken in elke taal die HTTP-antwoorden retourneert; in dit onderwerp wordt ervan uitgegaan dat u zult als host fungeren voor de URL in een JSP-pagina.

De volgende JSP-pagina resulteert in een TwiML reactie die **Hallo wereld** op het gesprek zegt.

    <%@ page contentType="text/xml" %>
    <Response> 
        <Say>Hello World</Say>
    </Response>

De volgende JSP-pagina resulteert in een TwiML reactie die zegt wat tekst en informatie over de API Twilio-versie en de naam van de Azure zegt heeft verschillende pauzes.


    <%@ page contentType="text/xml" %>
    <Response> 
        <Say>Hello from Azure</Say>
        <Pause></Pause>
        <Say>The Twilio API version is <%= request.getParameter("ApiVersion") %>.</Say>
        <Say>The Azure role name is <%= System.getenv("RoleName") %>.</Say>
        <Pause></Pause>
        <Say>Good bye.</Say>
    </Response>

De parameter **ApiVersion** is beschikbaar in Twilio voice aanvragen (geen SMS aanvragen). De parameters van het verzoek beschikbaar voor Twilio spraak en SMS aanvragen, Zie <https://www.twilio.com/docs/api/twiml/twilio_request> en <https://www.twilio.com/docs/api/twiml/sms/twilio_request>, respectievelijk. De omgevingsvariabele **rolnaam** is beschikbaar als onderdeel van een Azure-implementatie. (Zie de sectie van de omgeving variabelen op [Diverse rol configuratie-instellingen]als u aangepaste omgevingsvariabelen toevoegen wilt zodat zij kunnen worden opgehaald uit de **System.getenv**,[misc_role_config_settings].)

Zodra u uw JSP-pagina ingesteld op TwiML antwoorden geven hebt, gebruikt u de URL van de JSP-pagina als de URL doorgegeven aan de methode **CallFactory.create** . Bijvoorbeeld als u een webtoepassing met de naam MyTwiML op een Azure geïmplementeerd gehoste service, en de naam van de JSP-pagina mytwiml.jsp, de URL kan worden doorgegeven aan **CallFactory.create** zoals in het volgende:

    // Place the call From, To and URL values into a hash map. 
    HashMap<String, String> params = new HashMap<String, String>();
    params.put("From", "NNNNNNNNNN");
    params.put("To", "NNNNNNNNNN");
    params.put("Url", "http://<your_hosted_service>.cloudapp.net/MyTwiML/mytwiml.jsp");

    CallFactory callFactory = account.getCallFactory();
    Call call = callFactory.create(params);

Er is een andere optie voor het beantwoorden van TwiML via de klasse **TwiMLResponse** , die beschikbaar in het pakket **com.twilio.sdk.verbs is** .

Zie voor meer informatie over het gebruik van Twilio in Azure met Java, [het maken van een telefoongesprek met behulp van Twilio in een Java-toepassing op Azure][howto_phonecall_java].

## <a id="AdditionalServices"></a>Procedure: extra Twilio Services gebruiken
Naast de voorbeelden die hier worden weergegeven, biedt Twilio web-API's waarmee u kunt profiteren van extra functionaliteit van de Twilio van uw toepassing Azure. Zie de [Twilio API-documentatie]voor volledige details [twilio_api_documentation].

## <a id="NextSteps"></a>Volgende stappen
U hebt geleerd dat de basisbeginselen van de Twilio-service, klik op deze koppelingen voor meer informatie:

* [Richtlijnen voor beveiliging van Twilio] [twilio_security_guidelines]
* [Van de procedure Twilio en voorbeeldcode] [twilio_howtos]
* [Twilio Quickstart zelfstudies][twilio_quickstarts] 
* [Twilio op GitHub] [twilio_on_github]
* [Neem contact op met ondersteuning voor Twilio] [twilio_support]

[twilio_java]: https://github.com/twilio/twilio-java
[twilio_api_service]: https://api.twilio.com
[add_ca_cert]: java-add-certificate-ca-store.md
[howto_phonecall_java]: partner-twilio-java-phone-call-example.md
[misc_role_config_settings]: http://msdn.microsoft.com/library/windowsazure/hh690945.aspx
[twimlet_message_url]: http://twimlets.com/message
[twimlet_message_url_hello_world]: http://twimlets.com/message?Message%5B0%5D=Hello%20World
[twilio_rest_making_calls]: http://www.twilio.com/docs/api/rest/making-calls
[twilio_rest_sending_sms]: http://www.twilio.com/docs/api/rest/sending-sms
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
