
1. Open het gedeelde bestand MainPage.cs en het volgende codefragment toevoegen aan de klasse MainPage:
    
        // Define a member variable for storing the signed-in user. 
        private MobileServiceUser user;

        // Define a method that performs the authentication process
        // using a Facebook sign-in. 
        private async System.Threading.Tasks.Task<bool> AuthenticateAsync()
        {
            string message;
            bool success = false;
            try
            {
                // Change 'MobileService' to the name of your MobileServiceClient instance.
                // Sign-in using Facebook authentication.
                user = await App.MobileService
                    .LoginAsync(MobileServiceAuthenticationProvider.Facebook);
                message =
                    string.Format("You are now signed in - {0}", user.UserId);

                success = true;
            }
            catch (InvalidOperationException)
            {
                message = "You must log in. Login Required";
            }

            var dialog = new MessageDialog(message);
            dialog.Commands.Add(new UICommand("OK"));
            await dialog.ShowAsync();
            return success;
        }

    Deze code verifieert de gebruiker met een Facebook-account. Als u een id-provider dan Facebook, de waarde wijzigen van **MobileServiceAuthenticationProvider** boven aan de waarde van uw provider.

3. Opmerking-out of de aanroep van de methode **RefreshTodoItems** in het overschrijven van de bestaande **OnNavigatedTo** methode verwijderen.

    Hiermee voorkomt u dat de gegevens worden geladen voordat de gebruiker wordt geverifieerd. Vervolgens gaat toevoegen u een knop **aanmelden** App waarmee verificatie wordt geactiveerd.

4. Het volgende codefragment toevoegen aan de klasse MainPage:

        private async void ButtonLogin_Click(object sender, RoutedEventArgs e)
        {
            // Login the user and then load data from the mobile app.
            if (await AuthenticateAsync())
            {
                // Hide the login button and load items from the mobile app.
                ButtonLogin.Visibility = Windows.UI.Xaml.Visibility.Collapsed;
                //await InitLocalStoreAsync(); //offline sync support.
                await RefreshTodoItems();
            }
        }
        
5. Open het projectbestand MainPage.xaml in de Windows Store app-project en voeg het volgende element **knop** vlak voor het element dat de knop **Opslaan** wordt gedefinieerd:

        <Button Name="ButtonLogin" Click="ButtonLogin_Click" 
                        Visibility="Visible">Sign in</Button>

6. In de Windows Phone Store app-project toevoegen de volgende **Button** -element in de **ContentPanel**na het **TextBox** -element:

        <Button Grid.Row ="1" Grid.Column="1" Name="ButtonLogin" Click="ButtonLogin_Click" 
            Margin="10, 0, 0, 0" Visibility="Visible">Sign in</Button>

8. Open het gedeelde bestand App.xaml.cs en voeg de volgende code toe:

        protected override void OnActivated(IActivatedEventArgs args)
        {
            // Windows Phone 8.1 requires you to handle the respose from the WebAuthenticationBroker.
            #if WINDOWS_PHONE_APP
            if (args.Kind == ActivationKind.WebAuthenticationBrokerContinuation)
            {
                // Completes the sign-in process started by LoginAsync.
                // Change 'MobileService' to the name of your MobileServiceClient instance. 
                App.MobileService.LoginComplete(args as WebAuthenticationBrokerContinuationEventArgs);
            }
            #endif

            base.OnActivated(args);
        }

    Als de methode **OnActivated** al bestaat, voegt u toe de `#if...#endif` codeblok.

9. Druk op F5 om de Windows Store app uitvoeren, klikt u op de knop **aanmelden** en inloggen op de app met uw gekozen identiteitsprovider. 

    Als u met succes aangemeld bij de app moet zonder fouten worden uitgevoerd en moet u uw back-end opvragen en updates maken om gegevens te kunnen.

10. Klik met de rechtermuisknop op de Windows Phone Store-app-project, klikt u op **Project opstarten**en Herhaal de vorige stap om te verifiëren dat de Windows Phone Store app ook correct wordt uitgevoerd.  

 