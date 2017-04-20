<properties 
    pageTitle="Store-sendgrid-Java-How-to-Send-email-example" 
    description="Het verzenden van e-mail met de SendGrid van Java in een Azure-implementatie" 
    services="" 
    documentationCenter="java" 
    authors="thinkingserious" 
    manager="sendgrid" 
    editor="mollybos"/>

<tags 
    ms.service="multiple" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="Java" 
    ms.topic="article" 
    ms.date="10/30/2014" 
    ms.author="vibhork;dominic.may@sendgrid.com;elmer.thomas@sendgrid.com"/>

# <a name="how-to-send-email-using-sendgrid-from-java-in-an-azure-deployment"></a>Het verzenden van E-mail met de SendGrid van Java in Azure-implementatie

In het volgende voorbeeld ziet u hoe u SendGrid kunt gebruiken om e-mailberichten verzenden vanuit een webpagina in Azure. De uiteindelijke toepassing wordt de gebruiker voor e-waarden, zoals in de volgende schermafdruk.

![E-mailformulier][emailform]

Het resulterende e-mailbericht ziet er ongeveer als de volgende schermafdruk.

![E-mailbericht][emailsent]

U moet voor het gebruik van de code in dit onderwerp als volgt:

1. Ophalen met de potten van javax.mail, bijvoorbeeld <http://www.oracle.com/technetwork/java/javamail/index.html>.
2. De potten toevoegen aan uw Java-bouwpad.
3. Als u Eclips maken deze Java-toepassing gebruikt, kunt u de bibliotheken SendGrid opnemen in uw implementatie toepassingsbestand (WAR) met de Eclips implementatie assembly functie. Als u niet deze Java-toepassing maken Eclips, Controleer de bibliotheken zijn opgenomen in Azure dezelfde rol als de Java-toepassing en toegevoegd aan het klassepad van uw toepassing.


Ook moet u uw eigen SendGrid gebruikersnaam en wachtwoord naar het e-mailbericht verzenden. Zie [het verzenden van e-mail via de SendGrid van Java](store-sendgrid-java-how-to-send-email.md)om te beginnen met SendGrid.

Ook wordt bekend zijn met de informatie in het [maken van een Hello World-toepassing voor Azure in Eclips](http://msdn.microsoft.com/library/windowsazure/hh690944)of andere technieken voor het hosten van Java-toepassingen in Azure als u geen Eclips, sterk aanbevolen.

## <a name="create-a-web-form-for-sending-email"></a>Maak een webformulier voor het verzenden van e-mail

De volgende code ziet u hoe een webformulier maken voor het ophalen van gegevens voor het verzenden van e-mailadres van de gebruiker. Voor de toepassing van deze inhoud, het JSP-bestand met de naam **emailform.jsp**.

    <%@ page language="java" contentType="text/html; charset=ISO-8859-1"
        pageEncoding="ISO-8859-1" %>
    <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
    <html>
    <head>
    <meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
    <title>Email form</title>
    </head>
    <body>
     <p>Fill in all fields and click <b>Send this email</b>.</p>
     <br/>
      <form action="sendemail.jsp" method="post">
       <table>
         <tr>
           <td>To:</td>
           <td><input type="text" size=50 name="emailTo">
           </td>
         </tr>
         <tr>
           <td>From:</td>
           <td><input type="text" size=50 name="emailFrom">
           </td>
         </tr>
         <tr>
           <td>Subject:</td>
           <td><input type="text" size=100 name="emailSubject" value="My email subject">
           </td>
         </tr>
         <tr>
           <td>Text:</td>
           <td><input type="text" size=400 name="emailText" value="Hello,<p>This is my message.</p>Thank you." />
           </td>
         </tr>
         <tr>
           <td>SendGrid user name:</td>
           <td><input type="text" name="sendGridUser">
           </td>
         </tr>
         <tr>
           <td>SendGrid password:</td>
           <td><input type="password" name="sendGridPassword">
           </td>
         </tr>
         <tr>
           <td colspan=2><input type="submit" value="Send this email">
           </td>
         </tr>
       </table>
     </form>
     <br/>
    </body>
    </html>

## <a name="create-the-code-to-send-the-email"></a>De code voor het verzenden van het e-mailbericht maken

De volgende code wordt aangeroepen wanneer het formulier in de emailform.jsp is voltooid, wordt het e-mailbericht gemaakt en verzonden. Voor de toepassing van deze inhoud, het JSP-bestand met de naam **sendemail.jsp**.

    <%@ page language="java" contentType="text/html; charset=ISO-8859-1"
        pageEncoding="ISO-8859-1" import="javax.activation.*, javax.mail.*, javax.mail.internet.*, java.util.Date, java.util.Properties" %>
    <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
    <html>
    <head>
    <meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
    <title>Email processing happens here</title>
    </head>
    <body>
        <b>This is my send mail page.</b><p/>
     <%
     
     final String sendGridUser = request.getParameter("sendGridUser");
     final String sendGridPassword = request.getParameter("sendGridPassword");
     
     class SMTPAuthenticator extends Authenticator
     {
       public PasswordAuthentication getPasswordAuthentication()
       {
            String username = sendGridUser;
            String password = sendGridPassword;
          
            return new PasswordAuthentication(username, password);   
       }
     }
     try
     {
         
         // The SendGrid SMTP server.
         String SMTP_HOST_NAME = "smtp.sendgrid.net";
    
         Properties properties;
        
         properties = new Properties();
         
         // Specify SMTP values.
         properties.put("mail.transport.protocol", "smtp");
         properties.put("mail.smtp.host", SMTP_HOST_NAME);
         properties.put("mail.smtp.port", 587);
         properties.put("mail.smtp.auth", "true");
         
         // Display the email fields entered by the user. 
         out.println("Value entered for email Subject: " + request.getParameter("emailSubject") + "<br/>");        
         out.println("Value entered for email      To: " + request.getParameter("emailTo") + "<br/>");
         out.println("Value entered for email    From: " + request.getParameter("emailFrom") + "<br/>");
         out.println("Value entered for email    Text: " + "<br/>" + request.getParameter("emailText") + "<br/>");
    
         // Create the authenticator object.
         Authenticator authenticator = new SMTPAuthenticator();
         
         // Create the mail session object.
         Session mailSession;
         mailSession = Session.getDefaultInstance(properties, authenticator);
         
         // Display debug information to stdout, useful when using the
         // compute emulator during development.
         mailSession.setDebug(true);
    
         // Create the message and message part objects.
         MimeMessage message;
         Multipart multipart;
         MimeBodyPart messagePart; 
         
         message = new MimeMessage(mailSession);
         
         multipart = new MimeMultipart("alternative");
         messagePart = new MimeBodyPart();
         messagePart.setContent(request.getParameter("emailText"), "text/html");
         multipart.addBodyPart(messagePart);            
    
         // Specify the email To, From, Subject and Content. 
         message.setFrom(new InternetAddress(request.getParameter("emailFrom")));
         message.addRecipient(Message.RecipientType.TO, new InternetAddress(request.getParameter("emailTo")));
         message.setSubject(request.getParameter("emailSubject")); 
         message.setContent(multipart);
         
         // Uncomment the following if you want to add a footer.
         // message.addHeader("X-SMTPAPI", "{\"filters\": {\"footer\": {\"settings\": {\"enable\":1,\"text/html\": \"<html>This is my <b>email footer</b>.</html>\"}}}}");
    
         // Uncomment the following if you want to enable click tracking.
         // message.addHeader("X-SMTPAPI", "{\"filters\": {\"clicktrack\": {\"settings\": {\"enable\":1}}}}");
         
         Transport transport;
         transport = mailSession.getTransport();
         // Connect the transport object.
         transport.connect();
         // Send the message.
         transport.sendMessage(message,  message.getRecipients(Message.RecipientType.TO));
         // Close the connection.
         transport.close();
     
        out.println("<p>Email processing completed.</p>");
         
     }
     catch (Exception e)
     {
         out.println("<p>Exception encountered: " + 
                            e.getMessage()     +
                            "</p>");   
     }
    %>
    
    </body>
    </html>

Naast het e-mailbericht verzendt, biedt emailform.jsp een resultaat voor de gebruiker. een voorbeeld is de volgende schermafdruk:

![Resultaat van e-mailberichten verzenden][emailresult]

## <a name="next-steps"></a>Volgende stappen

Implementeren van uw toepassing de compute-emulator en emailform.jsp in een browser worden uitgevoerd, voert u waarden in het formulier klikt u op **Dit e-mailbericht te verzenden**en vervolgens Zie resultaten in sendemail.jsp.

Deze code is aangegeven, zodat u het gebruik van SendGrid in Java op Azure geleverd. Voordat u implementeert voor Azure in productie, kunt u foutafhandeling meer of andere functies toe te voegen. Bijvoorbeeld: 

* U kunt opslag Azure BLOB's of SQL-Database voor het opslaan van e-mailadressen en e-mailberichten, in plaats van via een formulier. Zie voor meer informatie over het gebruik van opslag Azure BLOB's in Java, [het gebruik van de Blob Storage-Service van Java](https://azure.microsoft.com/develop/java/how-to-guides/blob-storage/). Zie voor meer informatie over het gebruik van SQL-Database in Java [Met behulp van SQL-Database in Java](https://azure.microsoft.com/develop/java/how-to-guides/using-sql-azure-in-java/).
* U kunt `RoleEnvironment.getConfigurationSettings` SendGrid gebruikersnaam en wachtwoord ophalen van de implementatie van de configuratie-instellingen, in plaats van via het formulier op te halen die waarden. Voor meer informatie over de `RoleEnvironment` klasse, Zie [De Azure-runtimebibliotheek in JSP](http://msdn.microsoft.com/library/windowsazure/hh690948) en de documentatie van het pakket Azure Service Runtime op <http://dl.windowsazure.com/javadoc>.
* Zie voor meer informatie over het gebruik van SendGrid in Java, [het verzenden van e-mail via de SendGrid van Java](store-sendgrid-java-how-to-send-email.md).

[emailform]: ./media/store-sendgrid-java-how-to-send-email-example/SendGridJavaEmailform.jpg
[emailsent]: ./media/store-sendgrid-java-how-to-send-email-example/SendGridJavaEmailSent.jpg
[emailresult]: ./media/store-sendgrid-java-how-to-send-email-example/SendGridJavaResult.jpg
