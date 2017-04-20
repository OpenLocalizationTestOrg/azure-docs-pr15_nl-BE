<properties
    pageTitle="Push-meldingen met Azure melding Hubs en Node.js verzenden"
    description="Leren werken met Hubs melding push-meldingen verzenden vanuit een toepassing Node.js."
    keywords="push-bericht, notifications,node.js-push push, ios-push"
    services="notification-hubs"
    documentationCenter="nodejs"
    authors="ysxu"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="javascript"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="yuaxu"/>

# <a name="sending-push-notifications-with-azure-notification-hubs-and-nodejs"></a>Push-meldingen met Azure melding Hubs en Node.js verzenden
[AZURE.INCLUDE [notification-hubs-backend-how-to-selector](../../includes/notification-hubs-backend-how-to-selector.md)]

##<a name="overview"></a>Overzicht

> [AZURE.IMPORTANT] Deze zelfstudie hebt u een actieve account Azure. Als u geen account hebt, kunt u een gratis proefperiode account in een paar minuten. Zie voor meer informatie, [Gratis proefperiode van Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A643EE910&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-nodejs-how-to-use-notification-hubs).

Deze handleiding wordt beschreven hoe u push om meldingen te verzenden met behulp van Azure melding Hubs rechtstreeks vanuit een toepassing Node.js. 

De scenario's die gedekt zijn push-meldingen verzenden naar toepassingen op de volgende platforms:

* Android
* iOS
* Windows Phone
* Universal Windows Platform 

Voor meer informatie over hubs melding, Zie de sectie [Volgende stappen](#next) .

##<a name="what-are-notification-hubs"></a>Wat zijn Hubs melding?

Azure melding Hubs bieden een gemakkelijk te gebruiken, meerdere platforms, schaalbare infrastructuur voor push-meldingen verzenden naar mobiele apparaten. Zie voor meer informatie over de service-infrastructuur, de pagina [Azure melding Hubs](http://msdn.microsoft.com/library/windowsazure/jj927170.aspx) .

##<a name="create-a-nodejs-application"></a>Een Node.js-toepassing maken

De eerste stap in deze zelfstudie is een nieuwe, lege Node.js-toepassing maken. Zie voor meer informatie over het maken van een toepassing Node.js, [maken en implementeren van een toepassing Node.js naar Azure website][nodejswebsite], [Node.js wolk Service] [ Node.js Cloud Service] met behulp van Windows PowerShell of [website met WebMatrix].

##<a name="configure-your-application-to-use-notification-hubs"></a>Uw toepassing configureren voor gebruik van Hubs melding

Voor het gebruik van Azure melding Hubs, moet u downloaden en gebruiken van het Node.js [azure pakket](https://www.npmjs.com/package/azure)omvat een ingebouwde reeks helper bibliotheken die met de push melding overige services communiceren.

### <a name="use-node-package-manager-npm-to-obtain-the-package"></a>Knooppunt Package Manager (NPM) gebruiken voor het verkrijgen van het pakket

1.  Gebruik een opdrachtregelinterface zoals **PowerShell** (Windows), **Terminal** (Mac) of **Bash** (Linux) en navigeer naar de map waarin u de lege toepassing gemaakt.

2.  Typ **npm azure sb installeren** in het opdrachtvenster.

3.  U kunt handmatig uitvoeren met de opdracht **ls** of **dir** om te controleren of die een **knooppunt\_modules** map is gemaakt. In deze map vinden het **azure** -pakket de bibliotheken die u nodig hebt bevat voor toegang tot de melding Hub.

>[AZURE.NOTE] U kunt meer informatie over het installeren van NPM op de officiële [blog van de NPM](http://blog.npmjs.org/post/85484771375/how-to-install-npm). 

### <a name="import-the-module"></a>Importeren van de module

Met behulp van een teksteditor, Voeg het volgende toe boven aan het bestand **server.js** van de toepassing:

    var azure = require('azure');

### <a name="setup-an-azure-notification-hub-connection"></a>Setup een verbinding Azure melding Hub

Het **NotificationHubService** -object kunt u werken met melding hubs. De volgende code wordt een **NotificationHubService** -object voor de hub van de nofication met de naam **hubname**gemaakt. Toe te voegen aan de bovenkant van het bestand **server.js** na de instructie voor het importeren van de module azure:

    var notificationHubService = azure.createNotificationHubService('hubname','connectionstring');

De waarde van de **connectionstring** verbinding kan worden verkregen vanaf de [Portal Azure] door de volgende stappen uit te voeren:

1. Klik op **Bladeren**in het linkernavigatievenster.

2. Selecteer **Melding Hubs**en zoek de hub die u wilt gebruiken voor het monster. U kunt verwijzen naar de [Zelfstudie Windows Store aan de slag](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) als u hulp bij het maken van een nieuwe melding Hub nodig.

3. Selecteer **Instellingen**.

4. Klik op het **-beleid**. Ziet u zowel tekenreeksen voor gedeelde en volledige toegang.

![Azure Portal - melding Hubs](./media/notification-hubs-nodejs-how-to-use-notification-hubs/notification-hubs-portal.png)

> [AZURE.NOTE] Ook kunt u de verbindingsreeks met de cmdlet **Get-AzureSbNamespace** is verstrekt [Azure PowerShell](../powershell-install-configure.md) of de opdracht **show azure sb naamruimte** met de [Azure opdrachtregelinterface (CLI Azure)](../xplat-cli-install.md)ophalen.

##<a name="general-architecture"></a>Algemene architectuur

Het object **NotificationHubService** bevat de volgende objectinstanties voor push-meldingen verzenden naar specifieke apparaten en toepassingen:

* **Android** - gebruik het **GcmService** -object beschikbaar op **notificationHubService.gcm is**
* **iOS** - gebruik het **ApnsService** -object, dat toegankelijk is op **notificationHubService.apns**
* **Windows Phone** - gebruik het **MpnsService** -object beschikbaar op **notificationHubService.mpns is**
* **Universal Windows Platform** - gebruik het **WnsService** -object beschikbaar op **notificationHubService.wns is**

### <a name="how-to-send-push-notifications-to-android-applications"></a>Procedure: push-meldingen verzenden naar Android toepassingen

Het **GcmService** -object biedt de methode **send** push-meldingen verzenden naar Android-toepassingen kan worden gebruikt. De methode **send** accepteert de volgende parameters:

* **Tags** - de label-id. Als er geen argument is opgegeven, wordt de melding al clients worden verzonden.
* **Payload** - JSON of de ruwe string payload van het bericht.
* **Callback** - callback-functie.

Zie voor meer informatie over de indeling van de nettolading van de **nettolading** sectie van het document [GCM-Server implementeren](http://developer.android.com/google/gcm/server.html#payload) .

De **GcmService** instantie die door de **NotificationHubService** wordt de volgende code een push-bericht verzenden naar alle geregistreerde clients.

    var payload = {
      data: {
        message: 'Hello!'
      }
    };
    notificationHubService.gcm.send(null, payload, function(error){
      if(!error){
        //notification sent
      }
    });

### <a name="how-to-send-push-notifications-to-ios-applications"></a>Procedure: push-meldingen verzenden naar iOS toepassingen

Hetzelfde als bij Android toepassingen hierboven, het **ApnsService** -object biedt methode **send** push-meldingen verzenden naar iOS toepassingen kan worden gebruikt. De methode **send** accepteert de volgende parameters:

* **Tags** - de label-id. Als er geen argument is opgegeven, wordt de melding verzonden naar alle clients.
* **Payload** - JSON van het bericht of de nettolading van de tekenreeks.
* **Callback** - callback-functie.

Zie de sectie **Kennisgeving Payload** van de [lokale versie en de Push-melding Programming Guide](http://developer.apple.com/library/ios/#documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/ApplePushService/ApplePushService.html) document voor meer informatie de payload-indeling.

De **ApnsService** instantie die door de **NotificationHubService** wordt de volgende code een waarschuwingsbericht sturen aan alle clients:

    var payload={
        alert: 'Hello!'
      };
    notificationHubService.apns.send(null, payload, function(error){
      if(!error){
        // notification sent
      }
    });

### <a name="how-to-send-push-notifications-to-windows-phone-applications"></a>Procedure: push-meldingen verzenden aan toepassingen voor Windows Phone

Het **MpnsService** -object biedt de methode **send** push-meldingen verzenden naar Windows Phone-toepassingen kan worden gebruikt. De methode **send** accepteert de volgende parameters:

* **Tags** - de label-id. Als er geen argument is opgegeven, wordt de melding verzonden naar alle clients.
* **Payload** - XML-nettolading van het bericht.
* **Doelnaam**  -  `toast` voor meldingen van toast. `token`voor meldingen van de tegel.
* **NotificationClass** - de prioriteit van het bericht. Zie het gedeelte **HTTP-koptekst elementen** van het document [Push-meldingen van een server](http://msdn.microsoft.com/library/hh221551.aspx) voor geldige waarden.
* **Opties** - optionele headers.
* **Callback** - callback-functie.

Uitchecken voor een lijst met geldige opties voor **doelnaam**, **NotificationClass** en koptekst, de [Push-meldingen van een server](http://msdn.microsoft.com/library/hh221551.aspx) -pagina.

De volgende voorbeeldcode wordt de **MpnsService** instantie die door de **NotificationHubService** gebruikt voor het verzenden van een push-bericht toast:

    var payload = '<?xml version="1.0" encoding="utf-8"?><wp:Notification xmlns:wp="WPNotification"><wp:Toast><wp:Text1>string</wp:Text1><wp:Text2>string</wp:Text2></wp:Toast></wp:Notification>';
    notificationHubService.mpns.send(null, payload, 'toast', 22, function(error){
      if(!error){
        //notification sent
      }
    });

### <a name="how-to-send-push-notifications-to-universal-windows-platform-uwp-applications"></a>Procedure: push-meldingen verzenden naar toepassingen Universal Windows Platform (UWP)

Het **WnsService** -object biedt de methode **send** push-meldingen verzenden naar Universal Windows Platform toepassingen kan worden gebruikt.  De methode **send** accepteert de volgende parameters:

* **Tags** - de label-id. Als er geen argument is opgegeven, wordt de melding verzonden aan alle geregistreerde clients.
* **Payload** - de nettolading van XML-bericht.
* **Type** - het meldingstype.
* **Opties** - optionele headers.
* **Callback** - callback-functie.

Zie voor een lijst met geldige typen en aanvraagheaders, [Push notification service aanvraag en het antwoord headers](http://msdn.microsoft.com/library/windows/apps/hh465435.aspx).

De **WnsService** instantie die door de **NotificationHubService** wordt de volgende code een toast push-bericht verzenden naar een UWP app:

    var payload = '<toast><visual><binding template="ToastText01"><text id="1">Hello!</text></binding></visual></toast>';
    notificationHubService.wns.send(null, payload , 'wns/toast', function(error){
      if(!error){
        // notification sent
      }
    });

## <a name="next-steps"></a>Volgende stappen

Het bovenstaande voorbeeld-fragmenten kunnen u gemakkelijk maken service infrastructuur voor het leveren van push-meldingen naar een grote verscheidenheid van apparaten. U hebt geleerd dat de grondbeginselen van het gebruik van Hubs kennisgeving met node.js, klik op deze koppelingen voor meer informatie over hoe u deze mogelijkheden nog verder kunt uitbreiden.

-   Zie de MSDN-verwijzing voor [Azure melding Hubs](https://msdn.microsoft.com/library/azure/jj927170.aspx).
-   Ga naar de opslagplaats [Azure SDK voor knooppunt] op GitHub voor meer voorbeelden en implementatiegegevens.

  [Azure SDK voor knooppunt]: https://github.com/WindowsAzure/azure-sdk-for-node
  [Next Steps]: #nextsteps
  [What are Service Bus Topics and Subscriptions?]: #what-are-service-bus-topics
  [Create a Service Namespace]: #create-a-service-namespace
  [Obtain the Default Management Credentials for the Namespace]: #obtain-default-credentials
  [Create a Node.js Application]: #Create_a_Nodejs_Application
  [Configure Your Application to Use Service Bus]: #Configure_Your_Application_to_Use_Service_Bus
  [How to: Create a Topic]: #How_to_Create_a_Topic
  [How to: Create Subscriptions]: #How_to_Create_Subscriptions
  [How to: Send Messages to a Topic]: #How_to_Send_Messages_to_a_Topic
  [How to: Receive Messages from a Subscription]: #How_to_Receive_Messages_from_a_Subscription
  [How to: Handle Application Crashes and Unreadable Messages]: #How_to_Handle_Application_Crashes_and_Unreadable_Messages
  [How to: Delete Topics and Subscriptions]: #How_to_Delete_Topics_and_Subscriptions
  [1]: #Next_Steps
  [Topic Concepts]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-topics-01.png
  [Azure Classic Portal]: http://manage.windowsazure.com
  [image]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-queues-03.png
  [2]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-queues-04.png
  [3]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-queues-05.png
  [4]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-queues-06.png
  [5]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-queues-07.png
  [SqlFilter.SqlExpression]: http://msdn.microsoft.com/library/windowsazure/microsoft.servicebus.messaging.sqlfilter.sqlexpression.aspx
  [Azure Service Bus Notification Hubs]: http://msdn.microsoft.com/library/windowsazure/jj927170.aspx
  [SqlFilter]: http://msdn.microsoft.com/library/windowsazure/microsoft.servicebus.messaging.sqlfilter.aspx
  [Website met WebMatrix]: /develop/nodejs/tutorials/web-site-with-webmatrix/
  [Node.js Cloud Service]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md
[Previous Management Portal]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/previous-portal.png
  [nodejswebsite]: /develop/nodejs/tutorials/create-a-website-(mac)/
  [Node.js Cloud Service with Storage]: /develop/nodejs/tutorials/web-app-with-storage/
  [Node.js Web Application with Storage]: /develop/nodejs/tutorials/web-site-with-storage/
  [Azure Portal]: https://portal.azure.com
