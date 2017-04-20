<properties
    pageTitle="Cross-platform berichten verzenden naar gebruikers met melding Hubs (ASP.NET)"
    description="Informatie over het bericht Hubs sjablonen gebruiken voor het verzenden in een enkele aanvraag, een platform agnostic melding die gericht is op alle platforms."
    services="notification-hubs"
    documentationCenter=""
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="10/03/2016" 
    ms.author="yuaxu"/>

# <a name="send-cross-platform-notifications-to-users-with-notification-hubs"></a>Cross-platform meldingen verzenden naar gebruikers met melding Hubs


In de vorige zelfstudie [gebruikers waarschuwen kennisgeving Hubs], hebt u geleerd hoe push-meldingen naar alle apparaten die zijn geregistreerd door een geverifieerde gebruiker. In deze zelfstudie zijn meerdere aanvragen vereist een bericht verzenden naar elke client ondersteund platform. Melding Hubs ondersteunt sjablonen, waarmee u kunnen opgeven hoe een bepaald apparaat wil ontvangen. Dit vereenvoudigt het verzenden van berichten tussen verschillende platforms. Dit onderwerp wordt beschreven hoe u kunt profiteren van de sjablonen te verzenden in een enkele aanvraag, een platform agnostic melding die gericht is op alle platforms. Voor meer informatie over sjablonen, overzicht [Azure melding Hubs][Templates].

> [AZURE.NOTE] Hubs kennisgeving kan een apparaat voor het registreren van meerdere sjablonen met dezelfde tag. In dit geval leidt een binnenkomend bericht is gericht op die tag meerdere meldingen die worden geleverd aan het apparaat, één voor elke sjabloon. Zo kunt u hetzelfde bericht weergeven in meerdere visuele meldingen, bijvoorbeeld zowel een badge en een melding toast in een Windows Store-app.

Voer de volgende stappen voor het verzenden van meldingen van cross-platform met behulp van sjablonen:

1. Vouw de map met **domeincontrollers** in de Solution Explorer in Visual Studio en opent u het bestand RegisterController.cs.

2. Zoek het blok met code in de **Post** -methode die wordt gemaakt van een nieuwe registratie vervangen de `switch` inhoud met de volgende code:

        switch (deviceUpdate.Platform)
        {
            case "mpns":
                var toastTemplate = "<?xml version=\"1.0\" encoding=\"utf-8\"?>" +
                    "<wp:Notification xmlns:wp=\"WPNotification\">" +
                       "<wp:Toast>" +
                            "<wp:Text1>$(message)</wp:Text1>" +
                       "</wp:Toast> " +
                    "</wp:Notification>";
                registration = new MpnsTemplateRegistrationDescription(deviceUpdate.Handle, toastTemplate);
                break;
            case "wns":
                toastTemplate = @"<toast><visual><binding template=""ToastText01""><text id=""1"">$(message)</text></binding></visual></toast>";
                registration = new WindowsTemplateRegistrationDescription(deviceUpdate.Handle, toastTemplate);
                break;
            case "apns":
                var alertTemplate = "{\"aps\":{\"alert\":\"$(message)\"}}";
                registration = new AppleTemplateRegistrationDescription(deviceUpdate.Handle, alertTemplate);
                break;
            case "gcm":
                var messageTemplate = "{\"data\":{\"message\":\"$(message)\"}}";
                registration = new GcmTemplateRegistrationDescription(deviceUpdate.Handle, messageTemplate);
                break;
            default:
                throw new HttpResponseException(HttpStatusCode.BadRequest);
        }

    Deze code roept de methode platformspecifieke om een sjabloon maken in plaats van een eigen registratie. Bestaande registraties moeten niet worden gewijzigd omdat de registraties van de sjabloon worden afgeleid van eigen registraties.

3. Vervang de methode **sendNotification** met de volgende code in de controller **meldingen** :

        public async Task<HttpResponseMessage> Post()
        {
            var user = HttpContext.Current.User.Identity.Name;
            var userTag = "username:" + user;

            var notification = new Dictionary<string, string> { { "message", "Hello, " + user } };
            await Notifications.Instance.Hub.SendTemplateNotificationAsync(notification, userTag);

            return Request.CreateResponse(HttpStatusCode.OK);
        }

    Deze code stuurt een melding naar alle platforms op hetzelfde moment en zonder te geven een native-nettolading. Melding Hubs bouwt en levert de juiste nettolading voor elk apparaat met de waarde van de opgegeven _code_ , zoals aangegeven in de geregistreerde sjablonen.

4. Uw back-end WebApi project opnieuw publiceren.

5. De clienttoepassing opnieuw uit en controleer of de registratie is voltooid.

6. (Optioneel) De clienttoepassing implementeren op een tweede apparaat en voer vervolgens de app.

    Houd er rekening mee dat er een bericht op elk apparaat weergegeven.

## <a name="next-steps"></a>Volgende stappen

Nu dat u deze zelfstudie hebt voltooid, meer informatie over meldingen Hubs en sjablonen in deze onderwerpen:

+ **[Melding Hubs gebruiken voor het verzenden van laatste nieuws]** <br/>Ziet u een ander scenario voor het gebruik van sjablonen

+  **[Azure melding Hubs-overzicht][Templates]**<br/>Overzicht bevat meer gedetailleerde informatie over sjablonen.


<!-- Anchors. -->

<!-- Images. -->




<!-- URLs. -->
[Push to users ASP.NET]: /manage/services/notification-hubs/notify-users-aspnet
[Push to users Mobile Services]: /manage/services/notification-hubs/notify-users/
[Visual Studio 2012 Express for Windows 8]: http://go.microsoft.com/fwlink/?LinkId=257546

[Melding Hubs gebruiken voor het verzenden van laatste nieuws]: notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md
[Azure Notification Hubs]: http://go.microsoft.com/fwlink/p/?LinkId=314257
[Waarschuw gebruikers met melding Hubs]: notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md
[Templates]: http://go.microsoft.com/fwlink/p/?LinkId=317339
[Notification Hub How to for Windows Store]: http://msdn.microsoft.com/library/windowsazure/jj927172.aspx
