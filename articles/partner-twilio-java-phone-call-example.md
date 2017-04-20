<properties 
    pageTitle="Het maken van een telefoongesprek van Twilio (Java) | Microsoft Azure" 
    description="Informatie over het maken van een telefoongesprek van een webpagina met behulp van Twilio in een Java-toepassing op Azure." 
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

# <a name="how-to-make-a-phone-call-using-twilio-in-a-java-application-on-azure"></a>Het maken van een telefoongesprek met Twilio in een Java-toepassing op Azure 

In het volgende voorbeeld ziet u hoe u kunt Twilio bellen vanaf een webpagina die wordt gehost in Azure. De uiteindelijke toepassing wordt de gebruiker voor het telefoongesprek waarden, zoals in de volgende schermafdruk.

![Azure Call-formulier met behulp van Twilio en Java][twilio_java]

U moet voor het gebruik van de code in dit onderwerp als volgt:

1. Verwerving van een Twilio account en verificatie token. Evalueren om te beginnen met Twilio, prijzen op [http://www.twilio.com/pricing][twilio_pricing]. U kunt zich aanmelden op [https://www.twilio.com/try-twilio][try_twilio]. Zie voor informatie over de API die door Twilio [http://www.twilio.com/api][twilio_api].
2. Haal de Twilio JAR. Op [https://github.com/twilio/twilio-java][twilio_java_github], de bronnen van GitHub downloaden en maak uw eigen POTJES of een vooraf gebouwde JAR (met of zonder de afhankelijkheden) downloaden.
De code in dit onderwerp is geschreven met behulp van de vooraf ontworpen TwilioJava 3.3.8 met afhankelijkheden JAR.
3. Het oppervlak aan de Java-bouwpad toevoegen.
4. Als u Eclips deze Java-toepassing maken, opnemen de JAR-Twilio in uw implementatie toepassingsbestand (WAR) met de Eclips implementatie assembly-functie. Als u niet deze Java-toepassing maken Eclips, zorgen de JAR-Twilio is opgenomen in Azure dezelfde rol als de Java-toepassing en toegevoegd aan het klassepad van uw toepassing.
5. Controleer uw cacerts keystore bevat het certificaat van de CA Secure Equifax met MD5-vingerafdruk 67:CB:9 D: C0:13:24:8A:82:9B:B2:17:1E:D1:1B:EC:D4 (het serienummer is 35:DE:F4:CF en de SHA1-vingerafdruk is D2:32:09:AD:23:D3:14:23:21:74:E4:0 D: 7F:9 D: 62:13:97:86:63:3A). Dit is het certificaat van de certificate authority (CA) voor de [https://api.twilio.com] [ twilio_api_service] -service, die wordt aangeroepen wanneer u Twilio APIs. Zie [een certificaat voor de Java CA-certificaatarchief toe te voegen]voor meer informatie over het toevoegen van deze CA-certificaat op van de JDK cacert winkel[add_ca_cert].

Ook bekend zijn met de informatie in het [maken van een Hello World toepassing met behulp van de Toolkit Azure voor Eclips][azure_java_eclipse_hello_world], of met andere technieken voor het hosten van Java-toepassingen in Azure als u geen Eclips, wordt sterk aanbevolen.

## <a name="create-a-web-form-for-making-a-call"></a>Een webformulier voor een verbinding maken

De volgende code ziet u hoe een webformulier maken voor het ophalen van gegevens voor een verbinding van de gebruiker. Voor de toepassing van dit voorbeeld wordt een nieuwe dynamische webproject met de naam **TwilioCloud**, is gemaakt en **callform.jsp** is toegevoegd als een JSP-bestand.

    <%@ page language="java" contentType="text/html; charset=ISO-8859-1"
        pageEncoding="ISO-8859-1" %>
    <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
    <html>
    <head>
    <meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
    <title>Automated call form</title>
    </head>
    <body>
     <p>Fill in all fields and click <b>Make this call</b>.</p>
     <br/>
      <form action="makecall.jsp" method="post">
       <table>
         <tr>
           <td>To:</td>
           <td><input type="text" size=50 name="callTo" value="" />
           </td>
         </tr>
         <tr>
           <td>From:</td>
           <td><input type="text" size=50 name="callFrom" value="" />
           </td>
         </tr>
         <tr>
           <td>Call message:</td>
           <td><input type="text" size=400 name="callText" value="Hello. This is the call text. Good bye." />
           </td>
         </tr>
         <tr>
           <td colspan=2><input type="submit" value="Make this call" />
           </td>
         </tr>
       </table>
     </form>
     <br/>
    </body>
    </html>

## <a name="create-the-code-to-make-the-call"></a>Maken van de code om te kunnen bellen
De volgende code wordt aangeroepen wanneer de gebruiker het formulier dat wordt weergegeven door de callform.jsp is voltooid, maakt het bericht van de aanroep en de aanroep genereert. Voor de toepassing van dit voorbeeld wordt het JSP-bestand met de naam **makecall.jsp** en is toegevoegd aan het project **TwilioCloud** . (Gebruik uw Twilio account en verificatie in plaats van de tijdelijke aanduiding waarden toegewezen aan **accountSID** en **authToken** in de onderstaande code token).

    <%@ page language="java" contentType="text/html; charset=ISO-8859-1"
    import="java.util.*"
    import="com.twilio.*"
    import="com.twilio.sdk.*"
    import="com.twilio.sdk.resource.factory.*"
    import="com.twilio.sdk.resource.instance.*"
    pageEncoding="ISO-8859-1" %>
    <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
    <html>
    <head>
    <meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
    <title>Call processing happens here</title>
    </head>
    <body>
        <b>This is my make call page.</b><p/>
     <%
    try 
    {
         // Use your account SID and authentication token instead
         // of the placeholders shown here.
         String accountSID = "your_twilio_account";
         String authToken = "your_twilio_authentication_token";
     
         // Instantiate an instance of the Twilio client.     
         TwilioRestClient client;
         client = new TwilioRestClient(accountSID, authToken);

         // Retrieve the account, used later to retrieve the CallFactory.
         Account account = client.getAccount();

         // Display the client endpoint. 
         out.println("<p>Using Twilio endpoint " + client.getEndpoint() + ".</p>");
     
         // Display the API version.
         String APIVERSION = TwilioRestClient.DEFAULT_VERSION;
         out.println("<p>Twilio client API version is " + APIVERSION + ".</p>");
    
         // Retrieve the values entered by the user.
         String callTo = request.getParameter("callTo");  
         // The Outgoing Caller ID, used for the From parameter,
         // must have previously been verified with Twilio.
         String callFrom = request.getParameter("callFrom");
         String userText = request.getParameter("callText");
     
         // Replace spaces in the user's text with '%20', 
         // to make the text suitable for a URL.
         userText = userText.replace(" ", "%20");
     
         // Create a URL using the Twilio message and the user-entered text.
         String Url="http://twimlets.com/message";
         Url = Url + "?Message%5B0%5D=" + userText;
     
         // Display the message URL.
         out.println("<p>");
         out.println("The URL is " + Url);
         out.println("</p>");
    
         // Place the call From, To and URL values into a hash map. 
         HashMap<String, String> params = new HashMap<String, String>();
         params.put("From", callFrom);
         params.put("To", callTo);
         params.put("Url", Url);
     
         CallFactory callFactory = account.getCallFactory();
         Call call = callFactory.create(params);
         out.println("<p>Call status: " + call.getStatus()  + "</p>"); 
    } 
    catch (TwilioRestException e) 
    {
        out.println("<p>TwilioRestException encountered: " + e.getMessage() + "</p>");
        out.println("<p>StackTrace: " + e.getStackTrace().toString() + "</p>");
    }
    catch (Exception e) 
    {
        out.println("<p>Exception encountered: " + e.getMessage() + "");
        out.println("<p>StackTrace: " + e.getStackTrace().toString() + "</p>");
    }
    %>
    </body>
    </html>

Makecall.jsp geeft de oproep te maken, zodat het eindpunt Twilio, API-versie en de status van serviceoproep. Een voorbeeld is de volgende schermafdruk:

![Azure oproep reactie met Twilio en Java][twilio_java_response]

## <a name="run-the-application"></a>De toepassing wordt uitgevoerd
Hieronder volgen de stappen op hoog niveau voor het starten van uw toepassing. Details voor deze stappen u in het [maken van een Hello World toepassing met behulp van de Toolkit Azure voor Eclips vinden kunnen][azure_java_eclipse_hello_world].

1. De WAR TwilioCloud naar de map Azure **approot** exporteren. 
2. **Startup.cmd** om uw TwilioCloud WAR unzip wijzigen.
3. Compileer de toepassing voor de compute-emulator.
4. Start de distributie in de compute-emulator.
5. Open een browser en **http://localhost:8080/TwilioCloud/callform.jsp**worden uitgevoerd.
6. Waarden invoeren in het formulier, klikt u op **bellen**en vervolgens de resultaten bekijken in makecall.jsp.

Wanneer u klaar bent om te implementeren op Azure, recompile voor distributie naar de cloud, naar Azure implementeren en http://*your_hosted_name*.cloudapp.net/TwilioCloud/callform.jsp in de browser worden uitgevoerd (vervangen door de waarde voor *your_hosted_name*).

## <a name="next-steps"></a>Volgende stappen
Deze code is aangegeven, zodat u met behulp van Twilio in Java op Azure basisfunctionaliteit geleverd. Voordat u implementeert voor Azure in productie, kunt u foutafhandeling meer of andere functies toe te voegen. Bijvoorbeeld:

* In plaats van een webformulier, kunt u opslag Azure BLOB's of SQL-Database opgeslagen telefoonnummers en tekst. Zie voor meer informatie over het gebruik van opslag Azure BLOB's in Java, [het gebruik van de Blob Storage-Service van Java][howto_blob_storage_java]. Zie voor meer informatie over het gebruik van SQL-Database in Java [Met behulp van SQL-Database in Java][howto_sql_azure_java].
* Met **RoleEnvironment.getConfigurationSettings** kunt u de Twilio-ID ophalen en verificatietoken van de implementatie van de configuratie-instellingen, in plaats van harde codering de waarden in makecall.jsp. Zie voor informatie over de klasse **RoleEnvironment** , [De Azure-runtimebibliotheek in JSP] [ azure_runtime_jsp] en de documentatie van het pakket Azure Service Runtime op [http://dl.windowsazure.com/javadoc][azure_javadoc].
* De code makecall.jsp wijst een URL opgegeven voor Twilio, [http://twimlets.com/message][twimlet_message_url], naar de **Url** -variabele. Deze URL bevat een antwoord Twilio Markup Language (TwiML) Twilio te informeren hoe verder te gaan met de aanroep. Bijvoorbeeld, de TwiML die wordt geretourneerd kan bevatten een ** &lt;zeg&gt; ** bewerking waardoor de tekst die naar de ontvanger wordt gesproken. In plaats van de geleverde Twilio URL, kan samenstellen van uw eigen service om te reageren op de aanvraag van Twilio; Zie voor meer informatie, [het gebruik van Twilio voor spraak- en SMS-mogelijkheden in Java][howto_twilio_voice_sms_java]. Meer informatie over TwiML vindt u op [http://www.twilio.com/docs/api/twiml][twiml], en meer informatie over ** &lt;zeg&gt; ** en andere Twilio bewerkingen kunnen u vinden op [http://www.twilio.com/docs/api/twiml/say][twilio_say].
* Lees de richtlijnen op [https://www.twilio.com/docs/security]Twilio[twilio_docs_security].

Zie voor meer informatie over Twilio, [https://www.twilio.com/docs][twilio_docs].

## <a name="see-also"></a>Zie ook
* [Het gebruik van Twilio voor spraak- en SMS-mogelijkheden in Java][howto_twilio_voice_sms_java]
* [Een certificaat toevoegen aan het CA-certificaatarchief voor Java][add_ca_cert]

[twilio_pricing]: http://www.twilio.com/pricing
[try_twilio]: http://www.twilio.com/try-twilio
[twilio_api]: http://www.twilio.com/api
[verify_phone]: https://www.twilio.com/user/account/phone-numbers/verified#
[twilio_java_github]: http://github.com/twilio/twilio-java
[twimlet_message_url]: http://twimlets.com/message
[twiml]: http://www.twilio.com/docs/api/twiml
[twilio_api_service]: http://api.twilio.com
[add_ca_cert]: java-add-certificate-ca-store.md
[azure_java_eclipse_hello_world]: http://msdn.microsoft.com/library/windowsazure/hh690944.aspx
[howto_twilio_voice_sms_java]: partner-twilio-java-how-to-use-voice-sms.md
[howto_blob_storage_java]: http://www.windowsazure.com/develop/java/how-to-guides/blob-storage/
[howto_sql_azure_java]: http://msdn.microsoft.com/library/windowsazure/hh749029.aspx
[azure_runtime_jsp]: http://msdn.microsoft.com/library/windowsazure/hh690948.aspx
[azure_javadoc]: http://dl.windowsazure.com/javadoc
[twilio_docs_security]: http://www.twilio.com/docs/security
[twilio_docs]: http://www.twilio.com/docs
[twilio_say]: http://www.twilio.com/docs/api/twiml/say
[twilio_java]: ./media/partner-twilio-java-phone-call-example/WA_TwilioJavaCallForm.jpg
[twilio_java_response]: ./media/partner-twilio-java-phone-call-example/WA_TwilioJavaMakeCall.jpg
