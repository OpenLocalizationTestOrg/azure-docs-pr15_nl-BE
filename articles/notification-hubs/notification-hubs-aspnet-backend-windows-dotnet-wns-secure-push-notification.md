<properties
    pageTitle="Push-melding Azure Hubs veilig"
    description="Informatie over het verzenden van beveiligde push-meldingen in Azure. Voorbeelden van code is geschreven in C# .NET API gebruiken."
    documentationCenter="windows"
    authors="ysxu"
    manager="erikre"
    editor=""
    services="notification-hubs"/>

<tags
    ms.service="notification-hubs" 
    ms.workload="mobile"
    ms.tgt_pltfrm="windows"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="06/29/2016"
    ms.author="yuaxu"/>

#<a name="azure-notification-hubs-secure-push"></a>Push-melding Azure Hubs veilig

> [AZURE.SELECTOR]
- [Universele Windows](notification-hubs-aspnet-backend-windows-dotnet-wns-secure-push-notification.md)
- [iOS](notification-hubs-aspnet-backend-ios-push-apple-apns-secure-notification.md)
- [Android](notification-hubs-aspnet-backend-android-secure-google-gcm-push-notification.md)


##<a name="overview"></a>Overzicht

Ondersteuning voor push-berichten in Microsoft Azure hebt u toegang tot een gemakkelijk te gebruiken, meerdere platforms, schaalvergroting push-infrastructuur aanzienlijk de implementatie van push-meldingen voor consumenten- en enterprise-toepassingen voor mobiele platforms vereenvoudigt.

Vanwege wettelijke beveiligingsbeperkingen, soms een toepassing wil wellicht iets opnemen in het bericht dat niet kan worden verzonden via de standaard push notification-infrastructuur. In deze zelfstudie wordt beschreven hoe u de dezelfde ervaring door het verzenden van gevoelige informatie via een beveiligd, geverifieerd verbinding tussen het apparaat en de back-end app te bereiken.

Op een hoog niveau is de stroom als volgt uit:

1. App back-end:
    - Beveiligde payload winkels in back-enddatabase.
    - De ID van deze melding stuurt naar het apparaat (geen beveiligde gegevens verzonden).
2. De app op het apparaat bij de ontvangst van de kennisgeving:
    - Het apparaat contact op met de back-end de nettolading beveiligde aanvragen.
    - De app kan de nettolading weergeven als een melding op het apparaat.

Het is belangrijk te weten dat in de voorgaande stroom (en in deze zelfstudie), nemen we aan dat het apparaat een verificatietoken in lokale opslag opgeslagen nadat de gebruiker zich aanmeldt. Dit garandeert een volledig naadloze oplossing, zoals het apparaat kunt ophalen van de kennisgeving secure payload deze token gebruikt. Als uw toepassing niet-verificatietokens worden opgeslagen op het apparaat, of als deze tokens verlopen de app apparaat na ontvangst van de kennisgeving een algemene kennisgeving de gebruiker hierom te vragen voor het starten van de app moet worden weergegeven. De app vervolgens verifieert de gebruiker en de nettolading van de melding wordt weergegeven.

Deze Secure Push zelfstudie laat zien hoe veilig een push-bericht verzenden. De zelfstudie bouwt voort op de zelfstudie [Gebruikers waarschuwen](notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md) , dus u moet eerst de stappen in deze zelfstudie uitvoeren.

> [AZURE.NOTE] In deze zelfstudie wordt ervan uitgegaan dat u hebt gemaakt en uw melding hub configureren zoals beschreven in het [Aan de slag met melding Hubs (Windows Store)](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md).
Onthoud ook dat Windows Phone 8.1 is vereist voor Windows (niet Windows Phone)-referenties en achtergrondtaken werken niet op Windows Phone 8.0 of Silverlight 8.1. Archief van Windows-toepassingen, u kunt meldingen ontvangen via een achtergrondtaak alleen als de app-scherm vergrendelen ingeschakeld (Klik op het selectievakje in het Appmanifest).

[AZURE.INCLUDE [notification-hubs-aspnet-backend-securepush](../../includes/notification-hubs-aspnet-backend-securepush.md)]

## <a name="modify-the-windows-phone-project"></a>De Windows Phone-Project wijzigen

1. Voeg de volgende code naar App.xaml.cs taak op de achtergrond push registreren in het project **NotifyUserWindowsPhone** . Voeg de volgende regel code toe aan het einde van de `OnLaunched()` methode:

        RegisterBackgroundTask();

2. Nog in App.xaml.cs, voeg de volgende code direct na de `OnLaunched()` methode:

        private async void RegisterBackgroundTask()
        {
            if (!Windows.ApplicationModel.Background.BackgroundTaskRegistration.AllTasks.Any(i => i.Value.Name == "PushBackgroundTask"))
            {
                var result = await BackgroundExecutionManager.RequestAccessAsync();
                var builder = new BackgroundTaskBuilder();

                builder.Name = "PushBackgroundTask";
                builder.TaskEntryPoint = typeof(PushBackgroundComponent.PushBackgroundTask).FullName;
                builder.SetTrigger(new Windows.ApplicationModel.Background.PushNotificationTrigger());
                BackgroundTaskRegistration task = builder.Register();
            }
        }

3. Voeg de volgende `using` instructies boven aan het bestand App.xaml.cs:

        using Windows.Networking.PushNotifications;
        using Windows.ApplicationModel.Background;

4. Klik op **Alles opslaan**in het menu **bestand** in Visual Studio.

## <a name="create-the-push-background-component"></a>Het onderdeel Push achtergrond maken

De volgende stap is om de achtergrond push component te maken.

1. Klik met de rechtermuisknop op het knooppunt op het hoogste niveau van de oplossing (**SecurePush van de oplossing** in dit geval) in de Solution Explorer, klikt u op **toevoegen**en klik op **Nieuw Project**.

2. **Winkel Apps**, vouw vervolgens **Windows Phone Apps**Klik **Windows Runtime-onderdeel (Windows Phone)**. Geef het project de **PushBackgroundComponent**en klik op **OK** om het project te maken.

    ![][12]

3. In de Solution Explorer met de rechtermuisknop op het project **PushBackgroundComponent (Windows Phone 8.1)** en vervolgens klikt u op **toevoegen**en klik vervolgens op **klasse**. Naam van de nieuwe klasse **PushBackgroundTask.cs**. Klik op **toevoegen** om de klasse te genereren.

4. Vervangen door de volgende code wordt vervangen door de tijdelijke aanduiding voor de gehele inhoud van de definitie van de naamruimte **PushBackgroundComponent** `{back-end endpoint}` met het eindpunt van de back-end verkregen bij het implementeren van uw back-end:

        public sealed class Notification
            {
                public int Id { get; set; }
                public string Payload { get; set; }
                public bool Read { get; set; }
            }

            public sealed class PushBackgroundTask : IBackgroundTask
            {
                private string GET_URL = "{back-end endpoint}/api/notifications/";

                async void IBackgroundTask.Run(IBackgroundTaskInstance taskInstance)
                {
                    // Store the content received from the notification so it can be retrieved from the UI.
                    RawNotification raw = (RawNotification)taskInstance.TriggerDetails;
                    var notificationId = raw.Content;

                    // retrieve content
                    BackgroundTaskDeferral deferral = taskInstance.GetDeferral();
                    var httpClient = new HttpClient();
                    var settings = ApplicationData.Current.LocalSettings.Values;
                    httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Basic", (string)settings["AuthenticationToken"]);

                    var notificationString = await httpClient.GetStringAsync(GET_URL + notificationId);

                    var notification = JsonConvert.DeserializeObject<Notification>(notificationString);

                    ShowToast(notification);

                    deferral.Complete();
                }

                private void ShowToast(Notification notification)
                {
                    ToastTemplateType toastTemplate = ToastTemplateType.ToastText01;
                    XmlDocument toastXml = ToastNotificationManager.GetTemplateContent(toastTemplate);
                    XmlNodeList toastTextElements = toastXml.GetElementsByTagName("text");
                    toastTextElements[0].AppendChild(toastXml.CreateTextNode(notification.Payload));
                    ToastNotification toast = new ToastNotification(toastXml);
                    ToastNotificationManager.CreateToastNotifier().Show(toast);
                }
            }

5. In de Solution Explorer met de rechtermuisknop op het project **PushBackgroundComponent (Windows Phone 8.1)** en klik vervolgens op **Beheren NuGet pakketten**.

6. Klik op **on line**aan de linkerkant.

7. Typ in het vak **Zoeken** de **Http-Client**.

8. Klik op **Microsoft http-Client Libraries**in de resultatenlijst en klik op **installeren**. Voltooi de installatie.

9. Typ in het vak NuGet **Zoeken** **Json.net**. Installeer het pakket **Json.NET** en sluit vervolgens het venster NuGet Package Manager.

10. Voeg de volgende `using` instructies boven aan het bestand **PushBackgroundTask.cs** :

        using Windows.ApplicationModel.Background;
        using Windows.Networking.PushNotifications;
        using System.Net.Http;
        using Windows.Storage;
        using System.Net.Http.Headers;
        using Newtonsoft.Json;
        using Windows.UI.Notifications;
        using Windows.Data.Xml.Dom;

11. Klik met de rechtermuisknop op **References**in de Solution Explorer, klikt u in het project **NotifyUserWindowsPhone (Windows Phone 8.1)** en klikt u op **Verwijzing toevoegen**. Schakel het selectievakje **PushBackgroundComponent**in het dialoogvenster Reference Manager en klik vervolgens op **OK**.

12. Dubbelklik op **Package.appxmanifest** in het **NotifyUserWindowsPhone (Windows Phone 8.1)** project in de Solution Explorer. **Meldingen**, stel onder **Toast staat** op **Ja**.

    ![][3]

13. Klik in het menu **aangiften** bij de bovenkant in **Package.appxmanifest**, nog steeds. In de vervolgkeuzelijst met **Beschikbare declaraties** op **Achtergrondtaken**en klik vervolgens op **toevoegen**.

14. Controleer de **Push-melding**in **Package.appxmanifest**onder **Eigenschappen**.

15. Typ **PushBackgroundComponent.PushBackgroundTask** in het veld **Invoerhaven** in de **Package.appxmanifest**onder **De instellingen App**.

    ![][13]

16. Klik op **Alles opslaan**in het menu **bestand** .

## <a name="run-the-application"></a>De toepassing wordt uitgevoerd

Als de toepassing wordt uitgevoerd, het volgende doen:

1. Voer de toepassing van de Web-API **AppBackend** in Visual Studio. Een ASP.NET-webpagina wordt weergegeven.

2. Voer de Windows Phone-app van **NotifyUserWindowsPhone (Windows Phone 8.1)** in Visual Studio. De Windows Phone emulator wordt uitgevoerd en wordt de app automatisch geladen.

3. Geef een gebruikersnaam en wachtwoord in de app **NotifyUserWindowsPhone** UI. Dit kunnen elke tekenreeks zijn, maar zij moeten dezelfde waarde.

4. Klik in de app **NotifyUserWindowsPhone** UI **Log in en registreer**. Klik vervolgens op **verzenden push**.

[3]: ./media/notification-hubs-aspnet-backend-windows-dotnet-secure-push/notification-hubs-secure-push3.png
[12]: ./media/notification-hubs-aspnet-backend-windows-dotnet-secure-push/notification-hubs-secure-push12.png
[13]: ./media/notification-hubs-aspnet-backend-windows-dotnet-secure-push/notification-hubs-secure-push13.png
