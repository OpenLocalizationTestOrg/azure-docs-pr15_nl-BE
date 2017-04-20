<properties
    pageTitle="Hubs melding laatste nieuws zelfstudie gelokaliseerd"
    description="Leren werken met Azure melding Hubs gelokaliseerde recente nieuws berichten wilt verzenden."
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

# <a name="use-notification-hubs-to-send-localized-breaking-news"></a>Melding Hubs gebruiken voor het verzenden van gelokaliseerde laatste nieuws

> [AZURE.SELECTOR]
- [Windows Store C#](notification-hubs-windows-store-dotnet-xplat-localized-wns-push-notification.md)
- [iOS](notification-hubs-ios-xplat-localized-apns-push-notification.md)

##<a name="overview"></a>Overzicht

In dit onderwerp wordt beschreven hoe u de **sjabloon** van Azure melding Hubs voor het uitzenden van recente nieuws meldingen die zijn aangepast door de taal- en gebruiken. In deze zelfstudie start u met de Windows Store-app in [Gebruik melding Hubs voor het laatste nieuws verzenden]gemaakt. Als alles klaar is, dat is het mogelijk om te registreren voor categorieën waarin u geïnteresseerd bent, geef de taal waarin u de meldingen wilt ontvangen en alleen push-meldingen voor de geselecteerde categorieën in die taal ontvangen.


Er zijn twee delen voor dit scenario:

- de Windows Store-app kan de client-apparaten een taal opgeven en zich abonneren op verschillende recente nieuwscategorieën;

- de back-end zendt de kennisgevingen, met behulp van de **sjabloon** en **tag** funcites van Azure melding Hubs.



##<a name="prerequisites"></a>Vereisten

U moet al hebt voltooid de zelfstudie [Gebruiken kennisgeving Hubs voor het verzenden van de laatste nieuws] en de code beschikbaar, omdat deze zelfstudie is gebaseerd op die code rechtstreeks rijen.

U moet ook Visual Studio 2012 of later.


##<a name="template-concepts"></a>Sjabloon concepten

In [Gebruik melding Hubs voor het laatste nieuws verzenden] kunt u een app die **codes** gebruikt om u te abonneren op meldingen voor verschillende nieuwscategorieën gebouwd.
Veel apps, maar meerdere markten richten en lokalisatie nodig hebben. Dit betekent dat de inhoud van de kennisgevingen zelf moet worden vertaald en bezorgd in de juiste set apparaten.
We zien hoe de **sjabloon** van kennisgeving Hubs leveren gemakkelijk breken gelokaliseerde nieuws meldingen gebruiken in dit onderwerp.

Opmerking: een manier om gelokaliseerde meldingen te verzenden is voor het maken van meerdere versies van elke code. Bijvoorbeeld voor Engels, Frans en Mandarijns zou moeten we drie verschillende labels voor wereldnieuws: 'world_en', 'world_fr', en 'world_ch'. We hebben in dat een gelokaliseerde versie van het wereldnieuws verzenden naar elk van deze tags. In dit onderwerp gebruiken we sjablonen om te voorkomen dat de verspreiding van codes en de eis om meerdere berichten te verzenden.

Sjablonen zijn op een hoog niveau kunt u opgeven hoe een bepaald apparaat een melding ontvangt. De sjabloon geeft de indeling precies nettolading door te verwijzen naar eigenschappen die deel van het bericht is verzonden door uw app back-end uitmaken. In ons geval sturen we een landinstelling agnostic bericht met alle ondersteunde talen:

    {
        "News_English": "...",
        "News_French": "...",
        "News_Mandarin": "..."
    }

Vervolgens zullen we zorgen ervoor dat apparaten met een sjabloon die naar de juiste eigenschap verwijst registreren. Bijvoorbeeld, wordt een Windows Store app die een eenvoudige toast bericht wil registreren voor de volgende sjabloon met bijbehorende labels:

    <toast>
      <visual>
        <binding template=\"ToastText01\">
          <text id=\"1\">$(News_English)</text>
        </binding>
      </visual>
    </toast>



Sjablonen zijn een zeer krachtige functie die voor meer informatie over in het artikel van onze [sjablonen](notification-hubs-templates-cross-platform-push-messages.md) . 


##<a name="the-app-user-interface"></a>De gebruikersinterface van app

We zullen nu de app nieuws te analyseren die u hebt gemaakt in het onderwerp [Gebruik melding Hubs voor het laatste nieuws verzenden] verzenden gelokaliseerde laatste nieuws met behulp van sjablonen wijzigen.

In de Windows Store-app:

Uw MainPage.xaml om op te nemen van een combobox landinstelling wijzigen:

    <Grid Margin="120, 58, 120, 80"  
            Background="{StaticResource ApplicationPageBackgroundThemeBrush}">
        <Grid.RowDefinitions>
            <RowDefinition />
            <RowDefinition />
            <RowDefinition />
            <RowDefinition />
            <RowDefinition />
            <RowDefinition />
        </Grid.RowDefinitions>
        <Grid.ColumnDefinitions>
            <ColumnDefinition />
            <ColumnDefinition />
        </Grid.ColumnDefinitions>
        <TextBlock Grid.Row="0" Grid.Column="0" Grid.ColumnSpan="2"  TextWrapping="Wrap" Text="Breaking News" FontSize="42" VerticalAlignment="Top"/>
        <ComboBox Name="Locale" HorizontalAlignment="Left" VerticalAlignment="Center" Width="200" Grid.Row="1" Grid.Column="0">
            <x:String>English</x:String>
            <x:String>French</x:String>
            <x:String>Mandarin</x:String>
        </ComboBox>
        <ToggleSwitch Header="World" Name="WorldToggle" Grid.Row="2" Grid.Column="0"/>
        <ToggleSwitch Header="Politics" Name="PoliticsToggle" Grid.Row="3" Grid.Column="0"/>
        <ToggleSwitch Header="Business" Name="BusinessToggle" Grid.Row="4" Grid.Column="0"/>
        <ToggleSwitch Header="Technology" Name="TechnologyToggle" Grid.Row="2" Grid.Column="1"/>
        <ToggleSwitch Header="Science" Name="ScienceToggle" Grid.Row="3" Grid.Column="1"/>
        <ToggleSwitch Header="Sports" Name="SportsToggle" Grid.Row="4" Grid.Column="1"/>
        <Button Content="Subscribe" HorizontalAlignment="Center" Grid.Row="5" Grid.Column="0" Grid.ColumnSpan="2" Click="SubscribeButton_Click" />
    </Grid>

##<a name="building-the-windows-store-client-app"></a>De client Windows Store app bouwen

1. Toevoegen in uw klasse meldingen landinstellings-parameter aan de methoden *StoreCategoriesAndSubscribe* en *SubscribeToCateories* .

        public async Task<Registration> StoreCategoriesAndSubscribe(string locale, IEnumerable<string> categories)
        {
            ApplicationData.Current.LocalSettings.Values["categories"] = string.Join(",", categories);
            ApplicationData.Current.LocalSettings.Values["locale"] = locale;
            return await SubscribeToCategories(categories);
        }

        public async Task<Registration> SubscribeToCategories(string locale, IEnumerable<string> categories = null)
        {
            var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

            if (categories == null)
            {
                categories = RetrieveCategories();
            }

            // Using a template registration. This makes supporting notifications across other platforms much easier.
            // Using the localized tags based on locale selected.
            string templateBodyWNS = String.Format("<toast><visual><binding template=\"ToastText01\"><text id=\"1\">$(News_{0})</text></binding></visual></toast>", locale);

            return await hub.RegisterTemplateAsync(channel.Uri, templateBodyWNS, "localizedWNSTemplateExample", categories);
        }

    Houd er rekening mee dat in plaats van het aanroepen van de methode *RegisterNativeAsync* we *RegisterTemplateAsync noemen*: we bij het registreren van een specifieke mededeling indeling waarin de sjabloon is afhankelijk van de landinstelling. We bieden ook een naam voor de sjabloon ("localizedWNSTemplateExample"), omdat we handig is voor het registreren van meer dan een sjabloon (bijvoorbeeld een toast meldingen) en één voor tegels en we ze een naam moeten om te werken of te verwijderen.

    Houd er rekening mee dat als u een apparaat registreert meerdere sjablonen met dezelfde tag, een binnenkomend bericht Doelitems van het type label in dat meerdere meldingen geleverd aan het apparaat (één voor elke sjabloon). Dit gedrag is handig wanneer het bericht met dezelfde logische heeft leiden tot meerdere visuele meldingen, bijvoorbeeld met zowel een badge en een toast in een archief van Windows-toepassing.

2. Voeg de volgende methode voor het ophalen van de landinstelling van het opgeslagen:

        public string RetrieveLocale()
        {
            var locale = (string) ApplicationData.Current.LocalSettings.Values["locale"];
            return locale != null ? locale : "English";
        }

3. De knop bijwerken in uw MainPage.xaml.cs, klikt u op handler door het ophalen van de huidige waarde van de keuzelijst met invoervak land en die de aanroep naar de klasse meldingen, zoals:

        private async void SubscribeButton_Click(object sender, RoutedEventArgs e)
        {
            var locale = (string)Locale.SelectedItem;

            var categories = new HashSet<string>();
            if (WorldToggle.IsOn) categories.Add("World");
            if (PoliticsToggle.IsOn) categories.Add("Politics");
            if (BusinessToggle.IsOn) categories.Add("Business");
            if (TechnologyToggle.IsOn) categories.Add("Technology");
            if (ScienceToggle.IsOn) categories.Add("Science");
            if (SportsToggle.IsOn) categories.Add("Sports");

            var result = await ((App)Application.Current).notifications.StoreCategoriesAndSubscribe(locale,
                 categories);

            var dialog = new MessageDialog("Locale: " + locale + " Subscribed to: " + 
                string.Join(",", categories) + " on registration Id: " + result.RegistrationId);
            dialog.Commands.Add(new UICommand("OK"));
            await dialog.ShowAsync();
        }


4. In het bestand App.xaml.cs, zorg er ten slotte voor het bijwerken van uw `InitNotificationsAsync` methode voor het ophalen van de landinstelling en deze gebruiken als u zich abonneert:

        private async void InitNotificationsAsync()
        {
            var result = await notifications.SubscribeToCategories(notifications.RetrieveLocale());

            // Displays the registration ID so you know it was successful
            if (result.RegistrationId != null)
            {
                var dialog = new MessageDialog("Registration successful: " + result.RegistrationId);
                dialog.Commands.Add(new UICommand("OK"));
                await dialog.ShowAsync();
            }
        }


##<a name="send-localized-notifications-from-your-back-end"></a>Gelokaliseerde meldingen verzenden vanuit uw back-end

[AZURE.INCLUDE [notification-hubs-localized-back-end](../../includes/notification-hubs-localized-back-end.md)]






<!-- Anchors. -->
[Template concepts]: #concepts
[The app user interface]: #ui
[Building the Windows Store client app]: #building-client
[Send notifications from your back-end]: #send
[Next Steps]:#next-steps

<!-- Images. -->

<!-- URLs. -->
[Mobile Service]: /develop/mobile/tutorials/get-started
[Notify users with Notification Hubs: ASP.NET]: /manage/services/notification-hubs/notify-users-aspnet
[Notify users with Notification Hubs: Mobile Services]: /manage/services/notification-hubs/notify-users
[Melding Hubs gebruiken voor het verzenden van laatste nieuws]: /manage/services/notification-hubs/breaking-news-dotnet

[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253
[Get started with Mobile Services]: /develop/mobile/tutorials/get-started/#create-new-service
[Get started with data]: /develop/mobile/tutorials/get-started-with-data-dotnet
[Get started with authentication]: /develop/mobile/tutorials/get-started-with-users-dotnet
[Get started with push notifications]: /develop/mobile/tutorials/get-started-with-push-dotnet
[Push notifications to app users]: /develop/mobile/tutorials/push-notifications-to-app-users-dotnet
[Authorize users with scripts]: /develop/mobile/tutorials/authorize-users-in-scripts-dotnet
[JavaScript and HTML]: /develop/mobile/tutorials/get-started-with-push-js

[wns object]: http://go.microsoft.com/fwlink/p/?LinkId=260591
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/jj927170.aspx
[Notification Hubs How-To for iOS]: http://msdn.microsoft.com/library/jj927168.aspx
[Notification Hubs How-To for Windows Store]: http://msdn.microsoft.com/library/jj927172.aspx
