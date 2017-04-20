<properties
    pageTitle="Melding Hubs gebruiken voor het verzenden van laatste nieuws (Universal Windows)"
    description="Azure melding Hubs met tags in de registratie voor het laatste nieuws verzenden naar een universele Windows app gebruiken."
    services="notification-hubs"
    documentationCenter="windows"
    authors="ysxu"
    manager="erikre"
    editor=""/>


<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="06/29/2016"
    ms.author="yuaxu"/>

# <a name="use-notification-hubs-to-send-breaking-news"></a>Melding Hubs gebruiken voor het verzenden van laatste nieuws


[AZURE.INCLUDE [notification-hubs-selector-breaking-news](../../includes/notification-hubs-selector-breaking-news.md)]


##<a name="overview"></a>Overzicht

In dit onderwerp wordt beschreven hoe u Azure melding Hubs gebruiken voor het uitzenden van recente nieuws meldingen naar een Windows Store of app voor Windows Phone 8.1 (niet-Silverlight). Als u Windows Phone 8.1 Silverlight, Zie de [Windows Phone](notification-hubs-windows-phone-push-xplat-segmented-mpns-notification.md) -versie. Als alles klaar is, u kunnen registreren voor het breken van nieuwscategorieën waarin u geïnteresseerd bent, en alleen push meldingen ontvangen voor deze categorieën. Dit scenario is een gemeenschappelijk patroon voor veel toepassingen waarbij meldingen worden verzonden naar groepen gebruikers die eerder interesse in hen, zoals RSS-lezer, apps voor muziek ventilatoren, hebben verklaard enzovoort hebben. 

Broadcast-scenario's zijn ingeschakeld door een of meer _tags_ bij het maken van een inschrijving in de kennisgeving hub. Wanneer berichten worden verzonden naar een tag, ontvangt alle apparaten die zijn geregistreerd voor de code de melding. Omdat codes gewoon tekenreeksen zijn, hoeven niet van tevoren worden ingericht. Raadpleeg [melding Hubs en Tag expressies](notification-hubs-tags-segment-push-message.md)voor meer informatie over tags.

##<a name="prerequisites"></a>Vereisten

Dit onderwerp is gebaseerd op de toepassing die u hebt gemaakt in het [aan de slag met Hubs melding][get-started]. Voordat u deze zelfstudie begint, moet reeds hebt voltooid [aan de slag met Hubs melding][get-started].

##<a name="add-category-selection-to-the-app"></a>Categorieselectie toevoegen aan de app.

De eerste stap is de gebruikersinterface-elementen toevoegen aan uw bestaande hoofdpagina waarmee de gebruiker kan de categorieën selecteren om te registreren. De categorieën die zijn geselecteerd door een gebruiker zijn op het apparaat opgeslagen. Wanneer de toepassing wordt gestart, wordt de apparaatregistratie van een in uw melding hub met geselecteerde categorieën als codes gemaakt.

1. Open het projectbestand MainPage.xaml en kopieer de volgende code in het element **raster** :

        <Grid>
            <Grid.RowDefinitions>
                <RowDefinition/>
                <RowDefinition/>
                <RowDefinition/>
                <RowDefinition/>
                <RowDefinition/>
            </Grid.RowDefinitions>
            <Grid.ColumnDefinitions>
                <ColumnDefinition/>
                <ColumnDefinition/>
            </Grid.ColumnDefinitions>
            <TextBlock Grid.Row="0" Grid.Column="0" Grid.ColumnSpan="2"  TextWrapping="Wrap" Text="Breaking News" FontSize="42" VerticalAlignment="Top" HorizontalAlignment="Center"/>
            <ToggleSwitch Header="World" Name="WorldToggle" Grid.Row="1" Grid.Column="0" HorizontalAlignment="Center"/>
            <ToggleSwitch Header="Politics" Name="PoliticsToggle" Grid.Row="2" Grid.Column="0" HorizontalAlignment="Center"/>
            <ToggleSwitch Header="Business" Name="BusinessToggle" Grid.Row="3" Grid.Column="0" HorizontalAlignment="Center"/>
            <ToggleSwitch Header="Technology" Name="TechnologyToggle" Grid.Row="1" Grid.Column="1" HorizontalAlignment="Center"/>
            <ToggleSwitch Header="Science" Name="ScienceToggle" Grid.Row="2" Grid.Column="1" HorizontalAlignment="Center"/>
            <ToggleSwitch Header="Sports" Name="SportsToggle" Grid.Row="3" Grid.Column="1" HorizontalAlignment="Center"/>
            <Button Name="SubscribeButton" Content="Subscribe" HorizontalAlignment="Center" Grid.Row="4" Grid.Column="0" Grid.ColumnSpan="2" Click="SubscribeButton_Click"/>
        </Grid>


2. Klik met de rechtermuisknop het **gedeelde** project en een nieuwe klasse met de naam **meldingen**, toevoegen de optie **public** toevoegen aan de klassendefinitie van de en vervolgens toevoegen met de volgende instructies voor het **gebruik van** het nieuwe bestand met code:

        using Windows.Networking.PushNotifications;
        using Microsoft.WindowsAzure.Messaging;
        using Windows.Storage;
        using System.Threading.Tasks;

3. De volgende code kopiëren naar de nieuwe klasse van **meldingen** :

        private NotificationHub hub;

        public Notifications(string hubName, string listenConnectionString)
        {
            hub = new NotificationHub(hubName, listenConnectionString);
        }

        public async Task<Registration> StoreCategoriesAndSubscribe(IEnumerable<string> categories)
        {
            ApplicationData.Current.LocalSettings.Values["categories"] = string.Join(",", categories);
            return await SubscribeToCategories(categories);
        }

        public IEnumerable<string> RetrieveCategories()
        {
            var categories = (string) ApplicationData.Current.LocalSettings.Values["categories"];
            return categories != null ? categories.Split(','): new string[0];
        }

        public async Task<Registration> SubscribeToCategories(IEnumerable<string> categories = null)
        {
            var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

            if (categories == null)
            {
                categories = RetrieveCategories();
            }

            // Using a template registration to support notifications across platforms.
            // Any template notifications that contain messageParam and a corresponding tag expression
            // will be delivered for this registration.

            const string templateBodyWNS = "<toast><visual><binding template=\"ToastText01\"><text id=\"1\">$(messageParam)</text></binding></visual></toast>";

            return await hub.RegisterTemplateAsync(channel.Uri, templateBodyWNS, "simpleWNSTemplateExample",
                    categories);
        }

    Deze klasse wordt de lokale opslag gebruikt voor het opslaan van de categorieën van het nieuws dat dit apparaat heeft ontvangen. Houd er rekening mee dat in plaats van het aanroepen van de methode *RegisterNativeAsync* noemen we *RegisterTemplateAsync* om te registreren voor de registratie van een sjabloon met categorieën. 
    
    We bieden ook een naam voor de sjabloon ("simpleWNSTemplateExample"), omdat we handig is voor het registreren van meer dan een sjabloon (bijvoorbeeld een toast meldingen) en één voor tegels en we ze een naam moeten om te werken of te verwijderen.

    Houd er rekening mee dat als u een apparaat registreert meerdere sjablonen met dezelfde tag, een binnenkomend bericht Doelitems van het type label in dat meerdere meldingen geleverd aan het apparaat (één voor elke sjabloon). Dit gedrag is handig wanneer het bericht met dezelfde logische heeft leiden tot meerdere visuele meldingen, bijvoorbeeld met zowel een badge en een toast in een archief van Windows-toepassing.

    Zie voor meer informatie over sjablonen, [sjablonen](notification-hubs-templates-cross-platform-push-messages.md).




4. In het projectbestand App.xaml.cs toevoegen de volgende eigenschap aan de klasse **App** :

        public Notifications notifications = new Notifications("<hub name>", "<connection string with listen access>");

    Deze eigenschap wordt gebruikt voor het maken en openen van een exemplaar van de **meldingen** .

    In de bovenstaande code wordt vervangen door de `<hub name>` en `<connection string with listen access>` tijdelijke aanduidingen met de naam van de hub van uw melding en de verbindingsreeks voor de *DefaultListenSharedAccessSignature* die u eerder hebt gekregen.

    > [AZURE.NOTE] Omdat de referenties die worden gedistribueerd met een client-app niet in het algemeen veilig zijn, moet u de sleutel voor het luisteren naar access alleen met uw app client distribueren. Access maakt de app te registreren voor meldingen, maar bestaande registraties kan niet worden gewijzigd luisteren en meldingen kunnen niet worden verzonden. De volledige toegang tot de sleutel wordt gebruikt in een beveiligde back-end-service voor het verzenden van meldingen en bestaande registraties wijzigen.

5. In de MainPage.xaml.cs, voeg de volgende regel:

        using Windows.UI.Popups;

6. In het projectbestand MainPage.xaml.cs, voegt u de volgende methode:

        private async void SubscribeButton_Click(object sender, RoutedEventArgs e)
        {
            var categories = new HashSet<string>();
            if (WorldToggle.IsOn) categories.Add("World");
            if (PoliticsToggle.IsOn) categories.Add("Politics");
            if (BusinessToggle.IsOn) categories.Add("Business");
            if (TechnologyToggle.IsOn) categories.Add("Technology");
            if (ScienceToggle.IsOn) categories.Add("Science");
            if (SportsToggle.IsOn) categories.Add("Sports");

            var result = await ((App)Application.Current).notifications.StoreCategoriesAndSubscribe(categories);

            var dialog = new MessageDialog("Subscribed to: " + string.Join(",", categories) + " on registration Id: " + result.RegistrationId);
            dialog.Commands.Add(new UICommand("OK"));
            await dialog.ShowAsync();
        }

    Deze methode maakt een lijst met categorieën en de **meldingen** klasse gebruikt de lijst opslaan in de lokale opslag en de corresponderende codes registreren met de hub van uw melding. Wanneer categorieën worden gewijzigd, wordt de registratie opnieuw gemaakt met de nieuwe categorieën.

Uw app kan nu een set categorieën worden opgeslagen in lokale opslag op het apparaat en registreren met de hub melding wanneer de gebruiker de selectie van categorieën wijzigt.

##<a name="register-for-notifications"></a>Aanmelden voor meldingen

Deze stappen registreren met de hub melding bij het opstarten met behulp van de categorieën die zijn opgeslagen in lokale opslag.

> [AZURE.NOTE] Omdat het kanaal toegewezen door Windows Notification Service (WNS) URI op elk gewenst moment wijzigen kunt, dient u te registreren voor meldingen regelmatig om te voorkomen dat storingen melding. In dit voorbeeld registreert voor melding elke keer dat de toepassing wordt gestart. Voor toepassingen die vaak worden uitgevoerd, kunt meer dan één keer per dag, u waarschijnlijk overslaan registratie om bandbreedte te besparen als minder dan een dag is verstreken sinds de vorige registratie.

1. Open het bestand App.xaml.cs en bijwerken van de methode **InitNotificationsAsync** de `notifications` klasse om u te abonneren op basis van categorieën.

        // *** Remove or comment out these lines *** 
        //var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();
        //var hub = new NotificationHub("your hub name", "your listen connection string");
        //var result = await hub.RegisterNativeAsync(channel.Uri);
    
        var result = await notifications.SubscribeToCategories();

    Dit zorgt ervoor dat telkens wanneer de toepassing wordt gestart wordt opgehaald van de categorieën van lokale opslag en een registeration voor deze categorieën vraagt. De methode **InitNotificationsAsync** is gemaakt als onderdeel van de [aan de slag met Hubs melding] [ get-started] zelfstudie.

3. Voeg de volgende code de methode *OnNavigatedTo* in het projectbestand MainPage.xaml.cs:

        protected override void OnNavigatedTo(NavigationEventArgs e)
        {
            var categories = ((App)Application.Current).notifications.RetrieveCategories();

            if (categories.Contains("World")) WorldToggle.IsOn = true;
            if (categories.Contains("Politics")) PoliticsToggle.IsOn = true;
            if (categories.Contains("Business")) BusinessToggle.IsOn = true;
            if (categories.Contains("Technology")) TechnologyToggle.IsOn = true;
            if (categories.Contains("Science")) ScienceToggle.IsOn = true;
            if (categories.Contains("Sports")) SportsToggle.IsOn = true;
        }

    Hiermee werkt u de hoofdpagina op basis van de status van eerder opgeslagen categorieën.

De app is nu voltooid en een set categorieën kan worden opgeslagen in de lokale opslag van apparaat gebruikt om u te registreren bij de hub melding wanneer de gebruiker de selectie van categorieën wijzigt. Vervolgens definiëren we een backend die categorie berichten naar deze toepassing verzenden kan.

##<a name="sending-tagged-notifications"></a>Gecodeerde berichten verzenden

[AZURE.INCLUDE [notification-hubs-send-categories-template](../../includes/notification-hubs-send-categories-template.md)]

##<a name="run-the-app-and-generate-notifications"></a>De toepassing uitvoeren en meldingen genereren

1. Druk op F5 om te compileren en start de toepassing in Visual Studio.

    ![][1]

    Opmerking dat de UI-app biedt een aantal in-en uitschakelen waarmee u de categorieën wilt abonneren op kiezen.

2. Schakel een of meer categorieën in-of uitschakelen en klik op **abonneren**.

    De app wordt de geselecteerde categorieën omgezet in codes en vraagt om een nieuwe registratie voor de geselecteerde codes van de hub van de kennisgeving. De geregistreerde categorieën worden geretourneerd en weergegeven in een dialoogvenster.

    ![][19]

4. Een nieuw bericht verzenden vanuit de back-end in een van de volgende manieren:

    + **Console app:** start de console app.

    + **Java/PHP:** uw app/script uitvoeren.

    Meldingen voor de geselecteerde categorieën weergegeven als toast meldingen.

    ![][14]

##<a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd hoe u de laatste nieuws uitzending per categorie. Houd rekening met voltooien van een van de volgende zelfstudies die andere geavanceerde scenario's voor melding Hubs markeren:

+ [Melding Hubs gebruiken voor het uitzenden van gelokaliseerde laatste nieuws]

    Informatie over het uitbreiden van het laatste nieuws app zodat gelokaliseerde verzenden van meldingen.



<!-- Anchors. -->
[Add category selection to the app]: #adding-categories
[Register for notifications]: #register
[Send notifications from your back-end]: #send
[Run the app and generate notifications]: #test-app
[Next Steps]: #next-steps

<!-- Images. -->
[1]: ./media/notification-hubs-windows-store-dotnet-send-breaking-news/notification-hub-breakingnews-win1.png

[14]: ./media/notification-hubs-windows-store-dotnet-send-breaking-news/notification-hub-windows-toast-2.png


[19]: ./media/notification-hubs-windows-store-dotnet-send-breaking-news/notification-hub-windows-reg-2.png

<!-- URLs.-->
[get-started]: /manage/services/notification-hubs/getting-started-windows-dotnet/
[Melding Hubs gebruiken voor het uitzenden van gelokaliseerde laatste nieuws]: /manage/services/notification-hubs/breaking-news-localized-dotnet/
[Notify users with Notification Hubs]: /manage/services/notification-hubs/notify-users
[Mobile Service]: /develop/mobile/tutorials/get-started/
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/jj927170.aspx
[Notification Hubs How-To for Windows Store]: http://msdn.microsoft.com/library/jj927172.aspx
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253

[wns object]: http://go.microsoft.com/fwlink/p/?LinkId=260591
