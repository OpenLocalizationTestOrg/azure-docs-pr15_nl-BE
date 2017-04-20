<properties 
    pageTitle="Het gebruik van de e-mailservice van SendGrid (.NET) | Microsoft Azure" 
    description="Meer informatie over hoe verzend ik e-mail met de SendGrid e-mailservice op Azure. Monsters die zijn geschreven in C#-code en de .NET API gebruiken." 
    services="app-service\web" 
    documentationCenter=".net" 
    authors="thinkingserious" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="app-service-web" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="01/14/2016" 
    ms.author="team-pi@sendgrid.com"/>





# <a name="how-to-send-email-using-sendgrid-with-azure"></a>Het verzenden van E-mail met SendGrid met Azure


## <a name="overview"></a>Overzicht

Deze handleiding wordt beschreven hoe algemene programmeertaken met de SendGrid e-service uitvoeren op Azure. De monsters zijn geschreven in C\#
en de .NET API gebruiken. De scenario's die zijn **e-mailadres maken**, **verzenden van** **bijlagen toevoegen**en **met behulp van filters**. Zie de sectie [volgende stappen][] voor meer informatie over SendGrid en verzenden van e-mail.

## <a name="what-is-the-sendgrid-email-service"></a>Wat is de SendGrid e-service?

SendGrid is een [cloud-gebaseerde e-maildienst] betrouwbare [transactionele e-mails aankomen], schaalbaarheid en real-time analytics samen met flexibele API's die aangepaste integratie te vereenvoudigen. Veelvoorkomende gebruiksscenario's voor SendGrid zijn:

-   Automatisch verzenden van ontvangstbewijzen aan klanten.
-   Distributielijsten beheren voor het verzenden van klanten maandelijkse e-Folders en speciale aanbiedingen bevat.
-   Het verzamelen van realtime metrics voor geblokkeerde e-mailadres en de reactiesnelheid van de klant.
-   Genereren van rapporten om u te helpen bij het identificeren van trends.
-   Het doorsturen van vragen van klanten.
-   Verwerking van binnenkomende e-mailberichten.

Zie voor meer informatie, [https://sendgrid.com](https://sendgrid.com) of onze [bibliotheek C#][sendgrid-csharp]

## <a name="create-a-sendgrid-account"></a>Maak een account SendGrid

[AZURE.INCLUDE [sendgrid-sign-up](../../includes/sendgrid-sign-up.md)]

## <a name="reference-the-sendgrid-net-class-library"></a>Verwijst naar de bibliotheek van de klasse SendGrid .NET

Het [pakket SendGrid NuGet](https://www.nuget.org/packages/Sendgrid) is de eenvoudigste manier om de SendGrid-API en het configureren van uw toepassing met alle afhankelijkheden. NuGet is een Visual Studio-uitbreiding opgenomen met het Visual Studio 2015 Microsoft, waardoor het gemakkelijk te installeren en bijwerken van hulpprogramma's en bibliotheken. 

> [AZURE.NOTE] NuGet installeren als u werkt met een oudere versie van Visual Studio dan Visual Studio 2015, gaat u naar [http://www.nuget.org](http://www.nuget.org)en klik op de knop **NuGet installeren** .

De SendGrid NuGet om pakket te installeren in uw toepassing, het volgende doen:

1.  Een nieuw project gemaakt.

    ![Een nieuw project maken][create-new-project]

2.  Selecteer een sjabloon.

    ![Selecteer een sjabloon][select-a-template]

3.  In de **Solution Explorer**met de rechtermuisknop op **References**, klik op **Beheren NuGet pakketten**.

4.  Zoeken naar **SendGrid** en selecteer de **SendGrid** in de lijst met resultaten.

    ![SendGrid NuGet package][SendGrid-NuGet-package]

5.  Klik op **installeren** om de installatie te voltooien en sluit dit dialoogvenster.

.NET-klassenbibliotheek van SendGrid wordt **SendGridMail**genoemd. Het bevat de volgende naamruimten:

-   **SendGridMail** voor het maken en werken met e-items.
-   **SendGridMail.Transport** voor het verzenden van e-mail via het **SMTP-** protocol of de HTTP 1.1-protocol met **Web/REST**.

De volgende code naamruimtedeclaraties toevoegen aan de bovenkant van een C\# bestand waarin u wilt via programmering toegang tot de service van de e-SendGrid.
**System.Net** en **System.Net.Mail** zijn .NET Framework-naamruimten die opgenomen zijn omdat zij omvatten de typen die u vaak met de APIs SendGrid gebruikt.

    using System;
    using System.Net;
    using System.Net.Mail;
    using SendGrid;

## <a name="how-to-create-an-email"></a>Procedure: een e-mailbericht maken

Het **SendGridMessage** -object gebruiken voor het maken van een e-mailbericht. Zodra het berichtobject is gemaakt, kunt u de eigenschappen en methoden, met inbegrip van het e-mailadres afzender, geadresseerde, het voorwerp en de hoofdtekst van het e-mailbericht instellen.

In het volgende voorbeeld ziet u hoe een object volledig ingevulde e-mailadres:

    // Create the email object first, then add the properties.
    var myMessage = new SendGridMessage();

    // Add the message properties.
    myMessage.From = new MailAddress("john@example.com");

    // Add multiple addresses to the To field.
    List<String> recipients = new List<String>
    {
        @"Jeff Smith <jeff@example.com>",
        @"Anna Lidman <anna@example.com>",
        @"Peter Saddow <peter@example.com>"
    };

    myMessage.AddTo(recipients);

    myMessage.Subject = "Testing the SendGrid Library";

    //Add the HTML and Text bodies
    myMessage.Html = "<p>Hello World!</p>";
    myMessage.Text = "Hello World plain text!";

Zie voor meer informatie over alle eigenschappen en methoden die worden ondersteund door het type **SendGrid** , [sendgrid-csharp][] op GitHub.

## <a name="how-to-send-an-email"></a>Procedure: een e-mailbericht verzenden

Na het maken van een e-mailbericht, kunt u verzenden met behulp van de Web-API die door SendGrid. U kunt ook [gebruiken. Ingebouwde bibliotheek van NET](https://sendgrid.com/docs/Code_Examples/csharp.html).

E-mail is vereist dat u uw SendGrid referenties (gebruikersnaam en wachtwoord) of SendGrid API sleutel opgeven. API-sleutel is de aanbevolen methode. Als u meer informatie over het configureren van de sleutels van de API, gaat u naar onze [documentatie](https://sendgrid.com/docs/Classroom/Send/api_keys.html)

U kan deze referenties via de Portal Azure opslaan door op configureren te klikken en de sleutel/waarde-paren onder "app instellingen" toe te voegen.

 ![Azure app instellingen][azure_app_settings]

 Vervolgens, u hebt toegang tot deze als volgt: 
    
    var username = System.Environment.GetEnvironmentVariable("SENDGRID_USERNAME"); 
    var pswd = System.Environment.GetEnvironmentVariable("SENDGRID_PASSWORD");
    var apiKey = System.Environment.GetEnvironmentVariable("SENDGRID_APIKEY");

Met behulp van referenties:
    
    // Create network credentials to access your SendGrid account
    var username = "your_sendgrid_username";
    var pswd = "your_sendgrid_password";

    var credentials = new NetworkCredential(username, pswd);
    // Create an Web transport for sending email.
    var transportWeb = new Web(credentials);

Met behulp van API-sleutel:

    var apiKey = "your_sendgrid_api_key";  
    // create a Web transport, using API Key
    var transportWeb = new Web(apiKey);


De volgende voorbeelden laten zien hoe een bericht verzenden met de Web-API.

    // Create the email object first, then add the properties.
    SendGridMessage myMessage = new SendGridMessage();
    myMessage.AddTo("anna@example.com");
    myMessage.From = new MailAddress("john@example.com", "John Smith");
    myMessage.Subject = "Testing the SendGrid Library";
    myMessage.Text = "Hello World!";

    // Create credentials, specifying your user name and password.
    var credentials = new NetworkCredential("username", "password");

    // Create an Web transport for sending email.
    var transportWeb = new Web(credentials);

    // Send the email, which returns an awaitable task.
    transportWeb.DeliverAsync(myMessage);

    // If developing a Console Application, use the following
    // transportWeb.DeliverAsync(mail).Wait();

## <a name="how-to-add-an-attachment"></a>Procedure: een bijlage toevoegen

Bijlagen kunnen worden toegevoegd aan een bericht door het aanroepen van de methode **AddAttachment** en de naam en het pad van het bestand dat u wilt bijvoegen op te geven.
U kunt meerdere bijlagen opnemen door het aanroepen van deze methode als u voor elk bestand dat u wilt koppelen. In het volgende voorbeeld ziet u een bijlage aan een bericht toe te voegen:

    SendGridMessage myMessage = new SendGridMessage();
    myMessage.AddTo("anna@example.com");
    myMessage.From = new MailAddress("john@example.com", "John Smith");
    myMessage.Subject = "Testing the SendGrid Library";
    myMessage.Text = "Hello World!";

    myMessage.AddAttachment(@"C:\file1.txt");
    
U kunt ook bijlagen toevoegen van de gegevens in de **Stream**. Kan worden gedaan door het aanroepen van dezelfde methode als hierboven, **AddAttachment**, maar door doorgeven in de stroom van de gegevens en de bestandsnaam die moet worden weergegeven als in het bericht. In dit geval moet u de System.IO-bibliotheek toevoegen.

    SendGridMessage myMessage = new SendGridMessage();
    myMessage.AddTo("anna@example.com");
    myMessage.From = new MailAddress("john@example.com", "John Smith");
    myMessage.Subject = "Testing the SendGrid Library";
    myMessage.Text = "Hello World!";

    using (var attachmentFileStream = new FileStream(@"C:\file.txt", FileMode.Open))
    {
        myMessage.AddAttachment(attachmentFileStream, "My Cool File.txt");
    }


## <a name="how-to-use-apps-to-enable-footers-tracking-and-analytics"></a>Procedure: apps gebruiken om te schakelen van voetteksten, bijhouden en analytics

SendGrid biedt extra e-functionaliteit door het gebruik van apps. Dit zijn instellingen die kunnen worden toegevoegd aan een e-mailbericht om specifieke functionaliteit zoals Klik tracking, Google analytics, abonnement bijhouden, enzovoort. Zie voor een volledige lijst van apps in [App instellingen][].

Apps kunnen worden toegepast op **SendGrid** e-mailberichten met behulp van methoden die worden geïmplementeerd als onderdeel van de klasse **SendGrid** .

De volgende voorbeelden demonstreren de voettekst en klik op filters bijhouden:

### <a name="footer"></a>Voettekst

    // Create the email object first, then add the properties.
    SendGridMessage myMessage = new SendGridMessage();
    myMessage.AddTo("anna@example.com");
    myMessage.From = new MailAddress("john@example.com", "John Smith");
    myMessage.Subject = "Testing the SendGrid Library";
    myMessage.Text = "Hello World!";

    // Add a footer to the message.
    myMessage.EnableFooter("PLAIN TEXT FOOTER", "<p><em>HTML FOOTER</em></p>");

### <a name="click-tracking"></a>Klik op bijhouden

    // Create the email object first, then add the properties.
    SendGridMessage myMessage = new SendGridMessage();
    myMessage.AddTo("anna@example.com");
    myMessage.From = new MailAddress("john@example.com", "John Smith");
    myMessage.Subject = "Testing the SendGrid Library";
    myMessage.Html = "<p><a href=\"http://www.example.com\">Hello World Link!</a></p>";
    myMessage.Text = "Hello World!";
    
    // true indicates that links in plain text portions of the email 
    // should also be overwritten for link tracking purposes. 
    myMessage.EnableClickTracking(true);

## <a name="how-to-use-additional-sendgrid-services"></a>Procedure: extra SendGrid services gebruiken

SendGrid biedt een web-API's en webhooks kunt u profiteren van extra functionaliteit van de SendGrid van uw toepassing Azure. Zie de [SendGrid API-documentatie][]voor meer informatie.

## <a name="next-steps"></a>Volgende stappen

U hebt geleerd dat de basisbeginselen van de e-SendGrid-service, klik op deze koppelingen voor meer informatie.

*   C SendGrid\# library repo: [sendgrid-csharp][]
*   SendGrid API-documentatie: <https://sendgrid.com/docs>
*   Speciale aanbieding voor klanten Azure SendGrid: [https://sendgrid.com](https://sendgrid.com)

  [Volgende stappen]: #next-steps
  [What is the SendGrid Email Service?]: #whatis
  [Create a SendGrid Account]: #createaccount
  [Reference the SendGrid .NET Class Library]: #reference
  [How to: Create an Email]: #createemail
  [How to: Send an Email]: #sendemail
  [How to: Add an Attachment]: #addattachment
  [How to: Use Filters to Enable Footers, Tracking, and Analytics]: #usefilters
  [How to: Use Additional SendGrid Services]: #useservices
  
  [special offer]: https://www.sendgrid.com/windowsazure.html
  
  [create-new-project]: ./media/sendgrid-dotnet-how-to-send-email/create_new_project.png
  [select-a-template]: ./media/sendgrid-dotnet-how-to-send-email/select_a_template.png
  [SendGrid-NuGet-package]: ./media/sendgrid-dotnet-how-to-send-email/sendgrid_nuget.png
  [azure_app_settings]: ./media/sendgrid-dotnet-how-to-send-email/app_settings.png
  [sendgrid-csharp]: https://github.com/sendgrid/sendgrid-csharp
  [SMTP vs. Web API]: https://sendgrid.com/docs/Integrate/index.html
  [App instellingen]: https://sendgrid.com/docs/API_Reference/SMTP_API/apps.html
  [SendGrid API-documentatie]: https://sendgrid.com/docs
  
  [cloud-gebaseerde e-maildienst]: https://sendgrid.com/email-solutions
  [transactionele e-mails aankomen]: https://sendgrid.com/transactional-email
 
