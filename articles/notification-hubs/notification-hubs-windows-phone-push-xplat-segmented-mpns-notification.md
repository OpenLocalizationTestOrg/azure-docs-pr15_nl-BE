<properties
    pageTitle="Melding Hubs gebruiken voor het verzenden van laatste nieuws (Windows Phone)"
    description="Azure melding Hubs naar tag in de registraties voor het laatste nieuws verzenden naar een Windows Phone app gebruiken."
    services="notification-hubs"
    documentationCenter="windows"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows-phone"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="06/29/2016" 
    ms.author="yuaxu"/>

# <a name="use-notification-hubs-to-send-breaking-news"></a>Melding Hubs gebruiken voor het verzenden van laatste nieuws

[AZURE.INCLUDE [notification-hubs-selector-breaking-news](../../includes/notification-hubs-selector-breaking-news.md)]


##<a name="overview"></a>Overzicht

In dit onderwerp wordt beschreven hoe u Azure melding Hubs gebruiken voor het uitzenden van recente nieuws meldingen naar een Windows Phone 8.0 8.1/Silverlight app. Als u Windows Store of app voor Windows Phone 8.1, Raadpleeg de [Universele Windows](notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md) versie. Als alles klaar is, u kunnen registreren voor het breken van nieuwscategorieën waarin u geïnteresseerd bent, en alleen push meldingen ontvangen voor deze categorieën. Dit scenario is een gemeenschappelijk patroon voor veel toepassingen waar meldingen moeten worden verzonden naar groepen van gebruikers die eerder gedeclareerd belangstelling voor hen, zoals RSS-lezer, apps voor muziek, ventilatoren, enz.

Broadcast-scenario's zijn ingeschakeld door een of meer _tags_ bij het maken van een inschrijving in de kennisgeving hub. Wanneer berichten worden verzonden naar een tag, ontvangt alle apparaten die zijn geregistreerd voor de code de melding. Omdat codes gewoon tekenreeksen zijn, hoeven niet van tevoren worden ingericht. Raadpleeg [melding Hubs en Tag expressies](notification-hubs-tags-segment-push-message.md)voor meer informatie over tags.

##<a name="prerequisites"></a>Vereisten

Dit onderwerp is gebaseerd op de toepassing die u in [aan de slag met Hubs melding]gemaakt. Voordat u deze zelfstudie begint, moet reeds hebt voltooid [aan de slag met melding Hubs].

##<a name="add-category-selection-to-the-app"></a>Categorieselectie toevoegen aan de app.

De eerste stap is de gebruikersinterface-elementen toevoegen aan uw bestaande hoofdpagina waarmee de gebruiker kan de categorieën selecteren om te registreren. De categorieën die zijn geselecteerd door een gebruiker zijn op het apparaat opgeslagen. Wanneer de toepassing wordt gestart, wordt de apparaatregistratie van een in uw melding hub met geselecteerde categorieën als codes gemaakt.

1. Open het projectbestand MainPage.xaml en vervangt u vervolgens de elementen van het **raster** met de naam `TitlePanel` en `ContentPanel` met de volgende code:

        <StackPanel x:Name="TitlePanel" Grid.Row="0" Margin="12,17,0,28">
            <TextBlock Text="Breaking News" Style="{StaticResource PhoneTextNormalStyle}" Margin="12,0"/>
            <TextBlock Text="Categories" Margin="9,-7,0,0" Style="{StaticResource PhoneTextTitle1Style}"/>
        </StackPanel>

        <Grid Name="ContentPanel" Grid.Row="1" Margin="12,0,12,0">
            <Grid.RowDefinitions>
                <RowDefinition Height="auto"/>
                <RowDefinition Height="auto" />
                <RowDefinition Height="auto" />
                <RowDefinition Height="auto" />
            </Grid.RowDefinitions>
            <Grid.ColumnDefinitions>
                <ColumnDefinition />
                <ColumnDefinition />
            </Grid.ColumnDefinitions>
            <CheckBox Name="WorldCheckBox" Grid.Row="0" Grid.Column="0">World</CheckBox>
            <CheckBox Name="PoliticsCheckBox" Grid.Row="1" Grid.Column="0">Politics</CheckBox>
            <CheckBox Name="BusinessCheckBox" Grid.Row="2" Grid.Column="0">Business</CheckBox>
            <CheckBox Name="TechnologyCheckBox" Grid.Row="0" Grid.Column="1">Technology</CheckBox>
            <CheckBox Name="ScienceCheckBox" Grid.Row="1" Grid.Column="1">Science</CheckBox>
            <CheckBox Name="SportsCheckBox" Grid.Row="2" Grid.Column="1">Sports</CheckBox>
            <Button Name="SubscribeButton" Content="Subscribe" HorizontalAlignment="Center" Grid.Row="3" Grid.Column="0" Grid.ColumnSpan="2" Click="SubscribeButton_Click" />
        </Grid>

2. Een nieuwe klasse met de naam **meldingen**, maken de optie **public** toevoegen aan de klassendefinitie van de in het project, en vervolgens de volgende instructies voor het **gebruik van** het nieuwe bestand met code toevoegen:

        using Microsoft.Phone.Notification;
        using Microsoft.WindowsAzure.Messaging;
        using System.IO.IsolatedStorage;
        using System.Windows;

3. De volgende code kopiëren naar de nieuwe klasse van **meldingen** :

        private NotificationHub hub;

        // Registration task to complete registration in the ChannelUriUpdated event handler
        private TaskCompletionSource<Registration> registrationTask;

        public Notifications(string hubName, string listenConnectionString)
        {
            hub = new NotificationHub(hubName, listenConnectionString);
        }

        public IEnumerable<string> RetrieveCategories()
        {
            var categories = (string)IsolatedStorageSettings.ApplicationSettings["categories"];
            return categories != null ? categories.Split(',') : new string[0];
        }

        public async Task<Registration> StoreCategoriesAndSubscribe(IEnumerable<string> categories)
        {
            var categoriesAsString = string.Join(",", categories);
            var settings = IsolatedStorageSettings.ApplicationSettings;
            if (!settings.Contains("categories"))
            {
                settings.Add("categories", categoriesAsString);
            }
            else
            {
                settings["categories"] = categoriesAsString;
            }
            settings.Save();

            return await SubscribeToCategories();
        }

        public async Task<Registration> SubscribeToCategories()
        {
            registrationTask = new TaskCompletionSource<Registration>();

            var channel = HttpNotificationChannel.Find("MyPushChannel");

            if (channel == null)
            {
                channel = new HttpNotificationChannel("MyPushChannel");
                channel.Open();
                channel.BindToShellToast();
                channel.ChannelUriUpdated += channel_ChannelUriUpdated;

                // This is optional, used to receive notifications while the app is running.
                channel.ShellToastNotificationReceived += channel_ShellToastNotificationReceived;
            }

            // If channel.ChannelUri is not null, we will complete the registrationTask here.  
            // If it is null, the registrationTask will be completed in the ChannelUriUpdated event handler.
            if (channel.ChannelUri != null)
            {
                await RegisterTemplate(channel.ChannelUri);
            }
            
            return await registrationTask.Task;
        }

        async void channel_ChannelUriUpdated(object sender, NotificationChannelUriEventArgs e)
        {
            await RegisterTemplate(e.ChannelUri);
        }

        async Task<Registration> RegisterTemplate(Uri channelUri)
        {
            // Using a template registration to support notifications across platforms.
            // Any template notifications that contain messageParam and a corresponding tag expression
            // will be delivered for this registration.

            const string templateBodyMPNS = "<wp:Notification xmlns:wp=\"WPNotification\">" +
                                                "<wp:Toast>" +
                                                    "<wp:Text1>$(messageParam)</wp:Text1>" +
                                                "</wp:Toast>" +
                                            "</wp:Notification>";

            // The stored categories tags are passed with the template registration.

            registrationTask.SetResult(await hub.RegisterTemplateAsync(channelUri.ToString(), 
                templateBodyMPNS, "simpleMPNSTemplateExample", this.RetrieveCategories()));

            return await registrationTask.Task;
        }

        // This is optional. It is used to receive notifications while the app is running.
        void channel_ShellToastNotificationReceived(object sender, NotificationEventArgs e)
        {
            StringBuilder message = new StringBuilder();
            string relativeUri = string.Empty;

            message.AppendFormat("Received Toast {0}:\n", DateTime.Now.ToShortTimeString());

            // Parse out the information that was part of the message.
            foreach (string key in e.Collection.Keys)
            {
                message.AppendFormat("{0}: {1}\n", key, e.Collection[key]);

                if (string.Compare(
                    key,
                    "wp:Param",
                    System.Globalization.CultureInfo.InvariantCulture,
                    System.Globalization.CompareOptions.IgnoreCase) == 0)
                {
                    relativeUri = e.Collection[key];
                }
            }

            // Display a dialog of all the fields in the toast.
            System.Windows.Deployment.Current.Dispatcher.BeginInvoke(() => 
            { 
                MessageBox.Show(message.ToString()); 
            });
        }


    Deze klasse gebruikt de geïsoleerde opslag voor het opslaan van de categorieën van het nieuws dat dit apparaat is ontvangen. Het bevat ook methoden te registreren voor deze categorieën met behulp van registratie van een [sjabloon](notification-hubs-templates-cross-platform-push-messages.md) .


4. In het projectbestand App.xaml.cs toevoegen de volgende eigenschap aan de klasse **App** . Vervang de `<hub name>` en `<connection string with listen access>` tijdelijke aanduidingen met de naam van de hub van uw melding en de verbindingsreeks voor de *DefaultListenSharedAccessSignature* die u eerder hebt gekregen.

        public Notifications notifications = new Notifications("<hub name>", "<connection string with listen access>");

    > [AZURE.NOTE] Omdat de referenties die worden gedistribueerd met een client-app niet in het algemeen veilig zijn, moet u de sleutel voor het luisteren naar access alleen met uw app client distribueren. Access maakt de app te registreren voor meldingen, maar bestaande registraties kan niet worden gewijzigd luisteren en meldingen kunnen niet worden verzonden. De volledige toegang tot de sleutel wordt gebruikt in een beveiligde back-end-service voor het verzenden van meldingen en bestaande registraties wijzigen.

5. In de MainPage.xaml.cs, voeg de volgende regel:

        using Windows.UI.Popups;

6. In het projectbestand MainPage.xaml.cs, voegt u de volgende methode:

        private async void SubscribeButton_Click(object sender, RoutedEventArgs e)
        {
          var categories = new HashSet<string>();
          if (WorldCheckBox.IsChecked == true) categories.Add("World");
          if (PoliticsCheckBox.IsChecked == true) categories.Add("Politics");
          if (BusinessCheckBox.IsChecked == true) categories.Add("Business");
          if (TechnologyCheckBox.IsChecked == true) categories.Add("Technology");
          if (ScienceCheckBox.IsChecked == true) categories.Add("Science");
          if (SportsCheckBox.IsChecked == true) categories.Add("Sports");
    
          var result = await ((App)Application.Current).notifications.StoreCategoriesAndSubscribe(categories);
    
          MessageBox.Show("Subscribed to: " + string.Join(",", categories) + " on registration id : " +
             result.RegistrationId);
        }

    Deze methode maakt een lijst met categorieën en de **meldingen** klasse gebruikt de lijst opslaan in de lokale opslag en de corresponderende codes registreren met de hub van uw melding. Wanneer categorieën worden gewijzigd, wordt de registratie opnieuw gemaakt met de nieuwe categorieën.

Uw app kan nu een set categorieën worden opgeslagen in lokale opslag op het apparaat en registreren met de hub melding wanneer de gebruiker de selectie van categorieën wijzigt.

##<a name="register-for-notifications"></a>Aanmelden voor meldingen

Deze stappen registreren met de hub melding bij het opstarten met behulp van de categorieën die zijn opgeslagen in lokale opslag.

> [AZURE.NOTE] Omdat de URI die zijn toegewezen door de Microsoft Push Notification Service (MPNS) van het kanaal op elk gewenst moment wijzigen kunt, dient u te registreren voor meldingen regelmatig om te voorkomen dat storingen melding. In dit voorbeeld wordt voor berichten elke keer dat de toepassing wordt gestart. Voor toepassingen die vaak worden uitgevoerd, kunt meer dan één keer per dag, u waarschijnlijk overslaan registratie om bandbreedte te besparen als minder dan een dag is verstreken sinds de vorige registratie.


1. Open het bestand App.xaml.cs en de optie **async** toevoegen aan de methode **Application_Launching** en vervangt u de melding Hubs registratiecode die u in [aan de slag met Hubs melding] hebt toegevoegd met de volgende code:

        private async void Application_Launching(object sender, LaunchingEventArgs e)
        {
            var result = await notifications.SubscribeToCategories();

            if (result != null)
                System.Windows.Deployment.Current.Dispatcher.BeginInvoke(() =>
                {
                    MessageBox.Show("Registration Id :" + result.RegistrationId, "Registered", MessageBoxButton.OK);
                });
        }

    Dit zorgt ervoor dat telkens wanneer de toepassing wordt gestart wordt opgehaald van de categorieën van lokale opslag en vraagt om een registratie voor deze categorieën.

2. Voeg de volgende code die de methode **OnNavigatedTo** implementeert in het projectbestand MainPage.xaml.cs:

        protected override void OnNavigatedTo(NavigationEventArgs e)
        {
            var categories = ((App)Application.Current).notifications.RetrieveCategories();

            if (categories.Contains("World")) WorldCheckBox.IsChecked = true;
            if (categories.Contains("Politics")) PoliticsCheckBox.IsChecked = true;
            if (categories.Contains("Business")) BusinessCheckBox.IsChecked = true;
            if (categories.Contains("Technology")) TechnologyCheckBox.IsChecked = true;
            if (categories.Contains("Science")) ScienceCheckBox.IsChecked = true;
            if (categories.Contains("Sports")) SportsCheckBox.IsChecked = true;
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

    ![][2]

3. Na ontvangst van een bevestiging dat uw rubrieken abonnement voltooid zijn, de console app voor het verzenden van meldingen voor elke categorieën worden uitgevoerd. Controleer of er alleen een melding voor de categorieën die u bent geabonneerd.

    ![][3]

U kunt dit onderwerp hebt voltooid.

<!--##Next steps

In this tutorial we learned how to broadcast breaking news by category. Consider completing one of the following tutorials that highlight other advanced Notification Hubs scenarios:

+ [Use Notification Hubs to broadcast localized breaking news]

    Learn how to expand the breaking news app to enable sending localized notifications.

+ [Notify users with Notification Hubs]

    Learn how to push notifications to specific authenticated users. This is a good solution for sending notifications only to specific users.
-->

<!-- Anchors. -->
[Add category selection to the app]: #adding-categories
[Register for notifications]: #register
[Send notifications from your back-end]: #send
[Run the app and generate notifications]: #test-app
[Next Steps]: #next-steps

<!-- Images. -->
[1]: ./media/notification-hubs-windows-phone-send-breaking-news/notification-hub-breakingnews.png
[2]: ./media/notification-hubs-windows-phone-send-breaking-news/notification-hub-registration.png
[3]: ./media/notification-hubs-windows-phone-send-breaking-news/notification-hub-toast.png



<!-- URLs.-->
[Aan de slag met melding Hubs]: /manage/services/notification-hubs/get-started-notification-hubs-wp8/
[Use Notification Hubs to broadcast localized breaking news]: ../breakingnews-localized-wp8.md
[Notify users with Notification Hubs]: /manage/services/notification-hubs/notify-users/
[Mobile Service]: /develop/mobile/tutorials/get-started
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/jj927170.aspx
[Notification Hubs How-To for Windows Phone]: ??

