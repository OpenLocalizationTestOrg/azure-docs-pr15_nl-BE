<properties
    pageTitle="Verzenden van push-meldingen met Azure melding Hubs op Windows Phone | Microsoft Azure"
    description="In deze zelfstudie leert u Azure melding Hubs gebruiken voor push-meldingen naar een Windows Phone 8 en Windows Phone 8.1 Silverlight-toepassing."
    services="notification-hubs"
    documentationCenter="windows"
    keywords="push-meldingen, push-meldingen, windows telefoon push"
    authors="ysxu"
    manager="erikre"
    editor="erikre"/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows-phone"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="10/03/2016"
    ms.author="yuaxu"/>

# <a name="sending-push-notifications-with-azure-notification-hubs-on-windows-phone"></a>Push-meldingen verzenden met Azure melding Hubs op Windows Phone

[AZURE.INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

##<a name="overview"></a>Overzicht

> [AZURE.NOTE] Deze zelfstudie hebt u een actieve account Azure. Als u geen account hebt, kunt u een gratis proefperiode account in een paar minuten. Zie voor meer informatie, [Gratis proefperiode van Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-windows-phone-get-started%2F).

In deze zelfstudie wordt beschreven hoe u Azure melding Hubs push om meldingen te verzenden naar een Windows Phone 8 en Windows Phone 8.1 Silverlight-toepassing gebruiken. Als u Windows Phone 8.1 (niet-Silverlight), vervolgens verwijzen naar de [Universele Windows](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) versie.
In deze zelfstudie maakt u een lege app voor Windows Phone 8 waarmee push-meldingen ontvangen via Microsoft Push Notification Service (MPNS). Wanneer u klaar bent, zult u uw melding hub gebruiken voor het uitzenden van push-meldingen voor alle apparaten die met uw app.

> [AZURE.NOTE] De kennisgeving Hubs Windows Phone SDK biedt geen ondersteuning voor Windows Push Notification Service (WNS) gebruiken met Windows Phone 8.1 Silverlight-toepassingen. Voer de [Melding Hubs - zelfstudie Windows Phone Silverlight], waarbij de REST API's met Windows Phone 8.1 Silverlight apps met WNS (in plaats van MPNS).

De eenvoudige broadcast scenario bij het gebruik van Hubs melding ziet u de zelfstudie.

##<a name="prerequisites"></a>Vereisten

Deze zelfstudie is het volgende vereist:

+ [Visual Studio 2012 Express voor Windows Phone]of een latere versie.

Voltooien van deze zelfstudie is een vereiste voor alle andere Hubs melding zelfstudies voor apps voor Windows Phone 8.

##<a name="create-your-notification-hub"></a>De hub melding maken

[AZURE.INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

<ol start="6">
<li><p>Klik op de sectie <b>Notification Services</b> (binnen <i>Instellingen</i>), klik op de <b>Windows Phone (MPNS)</b> en klik op het selectievakje <b>niet-geverifieerde push inschakelen</b> .</p>
</li>
</ol>

&emsp;&emsp;![Azure Portal - unathenticated push-meldingen inschakelen](./media/notification-hubs-windows-phone-get-started/azure-portal-unauth.png)

De hub is nu gemaakt en geconfigureerd voor het verzenden van niet-geverifieerde meldingen voor Windows Phone.

> [AZURE.NOTE] In deze zelfstudie wordt MPNS in niet-geverifieerde modus. Niet-geverifieerde modus MPNS wordt geleverd met beperkingen op de meldingen die u naar elk kanaal verzenden kunt. Melding Hubs ondersteunt [MPNS modus geverifieerd](http://msdn.microsoft.com/library/windowsphone/develop/ff941099.aspx) doordat u uw certificaat uploaden.

##<a name="connecting-your-app-to-the-notification-hub"></a>Uw app verbinding met de melding hub

1. Maak in Visual Studio, een nieuwe toepassing voor Windows Phone 8.

    ![Visual Studio - Project nieuwe - Windows Phone-App][13]

    In Visual Studio 2013 Update 2 of hoger, worden in plaats daarvan een Windows Phone Silverlight-toepassing maken.

    ![Visual Studio - Project nieuwe - blanco App - Windows Phone Silverlight][11]

2. Klik met de rechtermuisknop op de oplossing in Visual Studio, en klik vervolgens op **Beheren NuGet pakketten**.

    Het dialoogvenster **NuGet pakketten beheren** wordt weergegeven.

3. Zoeken naar `WindowsAzure.Messaging.Managed` en klik op **installeren**en accepteer de gebruiksvoorwaarden.

    ![Visual Studio - NuGet Package Manager][20]

    Dit downloadt, installeert en een verwijzing toegevoegd naar de bibliotheek Azure Messaging voor Windows met behulp van het <a href="http://nuget.org/packages/WindowsAzure.Messaging.Managed/">pakket WindowsAzure.Messaging.Managed NuGet</a>.

4. Open het bestand App.xaml.cs en voeg de volgende `using` instructies:

        using Microsoft.Phone.Notification;
        using Microsoft.WindowsAzure.Messaging;

5. Voeg de volgende code boven aan de methode **Application_Launching** in App.xaml.cs:

        var channel = HttpNotificationChannel.Find("MyPushChannel");
        if (channel == null)
        {
            channel = new HttpNotificationChannel("MyPushChannel");
            channel.Open();
            channel.BindToShellToast();
        }

        channel.ChannelUriUpdated += new EventHandler<NotificationChannelUriEventArgs>(async (o, args) =>
        {
            var hub = new NotificationHub("<hub name>", "<connection string>");
            var result = await hub.RegisterNativeAsync(args.ChannelUri.ToString());

            System.Windows.Deployment.Current.Dispatcher.BeginInvoke(() =>
            {
                MessageBox.Show("Registration :" + result.RegistrationId, "Registered", MessageBoxButton.OK);
            });
        });

    >[AZURE.NOTE] De waarde **MyPushChannel** is een index die wordt gebruikt voor het opzoeken van een bestaand kanaal in de [HttpNotificationChannel](https://msdn.microsoft.com/library/windows/apps/microsoft.phone.notification.httpnotificationchannel.aspx) -collectie. Als er niet er is, maakt u een nieuwe vermelding met die naam.
    
    Zorg ervoor dat de naam van de hub en de verbindingsreeks met de naam **DefaultListenSharedAccessSignature** die u hebt verkregen in de vorige sectie invoegen.
    Deze code het kanaal URI voor de app opgehaald uit MPNS en registreert vervolgens dat kanaal URI met de hub van uw melding. Het garandeert ook dat het kanaal dat URI is geregistreerd in de hub melding elke keer dat de toepassing wordt gestart.

    >[AZURE.NOTE]Deze zelfstudie stuurt een melding toast naar het apparaat. Wanneer u een tegel melding verzendt, moet u in plaats daarvan de methode **BindToShellTile** aanroepen op het kanaal. Bel voor de ondersteuning van zowel toast en meldingen naast elkaar, zowel **BindToShellTile** als **BindToShellToast**.

6. Vouw in de Solution Explorer, **Eigenschappen**, open het `WMAppManifest.xml` het bestand, klik op het tabblad **mogelijkheden** en zorg ervoor dat de mogelijkheid van **ID_CAP_PUSH_NOTIFICATION** wordt gecontroleerd.

    ![Visual Studio - Windows Phone App mogelijkheden][14]

    Dit zorgt ervoor dat uw app push-meldingen kunt ontvangen. Zonder dit mislukt elke poging een push-bericht verzenden naar de app.

7. Druk op de `F5` voor het starten van de app.

    Een bericht van inschrijving wordt weergegeven in de app.

8. Sluit de app.  

   >[AZURE.NOTE] Als u wilt een toast push-bericht ontvangt, moet de toepassing niet op de voorgrond worden uitgevoerd.

##<a name="send-push-notifications-from-your-backend"></a>Push-meldingen verzenden vanuit uw back-end

Push-meldingen kunt u verzenden met behulp van kennisgeving Hubs vanuit een back-end via de openbare <a href="http://msdn.microsoft.com/library/windowsazure/dn223264.aspx">interface van de REST</a>. In deze zelfstudie kunt u met behulp van een consoletoepassing .NET push-meldingen verzenden. 

Zie voor een voorbeeld van het push-meldingen verzenden vanuit een WebAPI in ASP.NET backend die geïntegreerd met de melding Hubs [Azure melding Hubs gebruikers waarschuwen met .NET back-end](./notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md).  

Voor een voorbeeld van hoe push-meldingen worden verzonden via de [REST API's](https://msdn.microsoft.com/library/azure/dn223264.aspx), [het gebruik van Hubs melding van Java](./notification-hubs-java-push-notification-tutorial.md) en [het gebruik van Hubs melding van PHP](./notification-hubs-php-push-notification-tutorial.md).

1. Met de rechtermuisknop op de oplossing **toevoegen** als **Nieuw Project...**, selecteren en vervolgens klik op **Windows** en **Consoletoepassing**in **Visual C#**, en klikt u op **OK**.

    ![Visual Studio - Project nieuwe - consoletoepassing][6]

    Hiermee voegt u een nieuwe Visual C# consoletoepassing voor de oplossing. U kunt dit ook doen in een afzonderlijke oplossing.

4. Klik op **Extra**, klik op **Library Package Manager**en klik op **Package Manager-Console**.

    De Package Manager-Console worden weergegeven.

5.  Het **project standaard** ingesteld voor de nieuwe console application-project in het consolevenster met **Package Manager** en klik in het consolevenster met de volgende opdracht worden uitgevoerd:

        Install-Package Microsoft.Azure.NotificationHubs

    Hiermee wordt een verwijzing naar de Azure melding Hubs SDK met behulp van het <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">pakket Microsoft.Azure.Notification Hubs NuGet</a>toegevoegd.

6. Open de `Program.cs` -bestand en voeg de volgende `using` instructie:

        using Microsoft.Azure.NotificationHubs;

6. In de `Program` klasse, voegt u de volgende methode:

        private static async void SendNotificationAsync()
        {
            NotificationHubClient hub = NotificationHubClient
                .CreateClientFromConnectionString("<connection string with full access>", "<hub name>");
            string toast = "<?xml version=\"1.0\" encoding=\"utf-8\"?>" +
                "<wp:Notification xmlns:wp=\"WPNotification\">" +
                   "<wp:Toast>" +
                        "<wp:Text1>Hello from a .NET App!</wp:Text1>" +
                   "</wp:Toast> " +
                "</wp:Notification>";
            await hub.SendMpnsNativeNotificationAsync(toast);
        }

    Vervang de `<hub name>` de tijdelijke aanduiding voor de naam van de hub melding die wordt weergegeven in de portal. Ook de connection string tijdelijke aanduiding vervangen door de tekenreeks **DefaultFullSharedAccessSignature** die u hebt gekregen die in de sectie 'Uw melding hub configureren'.

    >[AZURE.NOTE]Zorg ervoor dat u de verbindingsreeks met **volledige** toegang, niet **luisteren** . De tekenreeks listen access geen machtigingen om push-meldingen te verzenden.

4. Voeg de volgende regel in uw `Main` methode:

         SendNotificationAsync();
         Console.ReadLine();

5. Met de Windows Phone emulator uitgevoerd en uw app gesloten, stelt u de console application-project als het standaard opstarten van project en druk vervolgens op de `F5` voor het starten van de app.

    U ontvangt een push-bericht toast. De app te tikken op de banner toast worden geladen.

De mogelijke nettoladingen kunt u vinden in de onderwerpen [toast catalogus] en [naast de catalogus] op MSDN.

##<a name="next-steps"></a>Volgende stappen

In het volgende eenvoudige voorbeeld u push-meldingen uitgezonden naar alle Windows Phone 8-apparaten. 

Verwijzen naar de zelfstudie [Gebruiken kennisgeving Hubs op push-meldingen voor gebruikers] om een specifiek doelpubliek. 

Als u wilt dat uw gebruikers door belangengroepen in segmenten, kunt u [Gebruik melding Hubs voor het verzenden van laatste nieuws]lezen. 

Meer informatie over het gebruik van Hubs kennisgeving in [Richtlijnen voor kennisgeving Hubs].



<!-- Images. -->
[6]: ./media/notification-hubs-windows-phone-get-started/notification-hub-create-console-app.png
[7]: ./media/notification-hubs-windows-phone-get-started/notification-hub-create-from-portal.png
[8]: ./media/notification-hubs-windows-phone-get-started/notification-hub-create-from-portal2.png
[9]: ./media/notification-hubs-windows-phone-get-started/notification-hub-select-from-portal.png
[10]: ./media/notification-hubs-windows-phone-get-started/notification-hub-select-from-portal2.png
[11]: ./media/notification-hubs-windows-phone-get-started/notification-hub-create-wp-silverlight-app.png
[12]: ./media/notification-hubs-windows-phone-get-started/notification-hub-connection-strings.png

[13]: ./media/notification-hubs-windows-phone-get-started/notification-hub-create-wp-app.png
[14]: ./media/notification-hubs-windows-phone-get-started/mobile-app-enable-push-wp8.png
[15]: ./media/notification-hubs-windows-phone-get-started/notification-hub-pushauth.png
[20]: ./media/notification-hubs-windows-phone-get-started/notification-hub-windows-universal-app-install-package.png
[213]: ./media/notification-hubs-windows-phone-get-started/notification-hub-create-console-app.png





<!-- URLs. -->
[Visual Studio 2012 Express voor Windows Phone]: https://go.microsoft.com/fwLink/p/?LinkID=268374
[Richtsnoeren voor kennisgeving Hubs]: http://msdn.microsoft.com/library/jj927170.aspx
[MPNS authenticated mode]: http://msdn.microsoft.com/library/windowsphone/develop/ff941099(v=vs.105).aspx
[Melding Hubs gebruiken voor push-meldingen aan gebruikers]: notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md
[Melding Hubs gebruiken voor het verzenden van laatste nieuws]: notification-hubs-windows-phone-push-xplat-segmented-mpns-notification.md
[toast catalogus]: http://msdn.microsoft.com/library/windowsphone/develop/jj662938(v=vs.105).aspx
[tegel catalogus]: http://msdn.microsoft.com/library/windowsphone/develop/hh202948(v=vs.105).aspx
[Melding Hubs - zelfstudie Windows Phone Silverlight]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/PushToSLPhoneApp

