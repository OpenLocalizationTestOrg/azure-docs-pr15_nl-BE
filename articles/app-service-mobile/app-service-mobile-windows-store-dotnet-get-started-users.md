<properties
    pageTitle="Verificatie toevoegen aan uw app Universal Windows Platform (UWP) | Azure mobiele Apps"
    description="Informatie over het gebruik van mobiele Apps in Azure App-Service voor het verifiëren van gebruikers van uw app in Universal Windows Platform (UWP) met behulp van een groot aantal identiteitsaanbieders van, met inbegrip van: AAD, Google, Facebook, Twitter en Microsoft."
    services="app-service\mobile"
    documentationCenter="windows"
    authors="adrianhall"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="add-authentication-to-your-windows-app"></a>Verificatie toevoegen aan uw Windows-app

[AZURE.INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

In dit onderwerp wordt beschreven hoe u verificatie op basis van cloud toevoegen aan uw mobiele app. In deze zelfstudie toevoegen u verificatie aan het project van de quickstart Universal Windows Platform (UWP) voor mobiele toepassingen met behulp van een id-provider die wordt ondersteund door Azure App-Service. Na wordt met succes geverifieerd en geautoriseerd door de mobiele App-end, wordt de waarde van de gebruiker-ID weergegeven.

Deze zelfstudie is gebaseerd op de quickstart Mobile Apps. U moet eerst de zelfstudie [aan de slag met mobiele Apps](app-service-mobile-windows-store-dotnet-get-started.md)voltooien.

##<a name="register"></a>Registreer uw app voor verificatie en de App-Service configureren

[AZURE.INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

##<a name="permissions"></a>Machtigingen voor geverifieerde gebruikers beperken

[AZURE.INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

Nu kunt u controleren dat anonieme toegang tot uw backend is uitgeschakeld. Met het project UWP app instellen als het opstartproject, implementeren en uitvoeren van de app; Controleer of dat een niet-verwerkte uitzondering met de statuscode van 401 (Unauthorized) treedt op nadat de toepassing wordt gestart. Dit gebeurt omdat de app probeert te krijgen tot uw mobiele App-Code als een niet-geverifieerde gebruiker, maar de tabel *TodoItem* nu verificatie vereist is.

Vervolgens werkt de app om gebruikers te verifiëren voordat u bronnen van uw App Service aanvraagt.

##<a name="add-authentication"></a>Verificatie toevoegen aan de app.

1. In de UWP app bestand MainPage.cs project en het volgende codefragment toevoegen aan de klasse MainPage:
    
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

3. Opmerking-out of de aanroep van de methode **ButtonRefresh_Click** (of de **InitLocalStoreAsync** -methode) in het overschrijven van de bestaande **OnNavigatedTo** methode verwijderen. Hiermee voorkomt u dat de gegevens worden geladen voordat de gebruiker wordt geverifieerd. Vervolgens gaat toevoegen u een knop **aanmelden** App waarmee verificatie wordt geactiveerd.

4. Het volgende codefragment toevoegen aan de klasse MainPage:

        private async void ButtonLogin_Click(object sender, RoutedEventArgs e)
        {
            // Login the user and then load data from the mobile app.
            if (await AuthenticateAsync())
            {
                // Switch the buttons and load items from the mobile app.
                ButtonLogin.Visibility = Visibility.Collapsed;
                ButtonSave.Visibility = Visibility.Visible;
                //await InitLocalStoreAsync(); //offline sync support.
                await RefreshTodoItems();
            }
        }
        
5. Open het projectbestand MainPage.xaml, zoek naar het element waarin de knop **Opslaan** en deze vervangen door de volgende code:

        <Button Name="ButtonSave" Visibility="Collapsed" Margin="0,8,8,0" 
                Click="ButtonSave_Click">
            <StackPanel Orientation="Horizontal">
                <SymbolIcon Symbol="Add"/>
                <TextBlock Margin="5">Save</TextBlock>
            </StackPanel>
        </Button>
        <Button Name="ButtonLogin" Visibility="Visible" Margin="0,8,8,0" 
                Click="ButtonLogin_Click" TabIndex="0">
            <StackPanel Orientation="Horizontal">
                <SymbolIcon Symbol="Permissions"/>
                <TextBlock Margin="5">Sign in</TextBlock> 
            </StackPanel>
        </Button>

9. Druk op F5 om de toepassing uitvoeren, klikt u op de knop **aanmelden** en inloggen op de app met uw gekozen identiteitsprovider. Na het aanmelden gelukt is, de app wordt zonder fouten worden uitgevoerd en kunt u uw back-end opvragen en wijzigingen in gegevens.


##<a name="tokens"></a>Het verificatietoken op de client worden opgeslagen

Het vorige voorbeeld blijkt een standaard aanmelden, die vereist dat de client contact met zowel de identiteitsprovider en de App-Service elke keer dat de toepassing wordt gestart. Is deze methode niet alleen inefficiënt, kunt u uitvoeren in gebruik-betrekking heeft problemen met veel klanten moeten proberen app starten op hetzelfde moment. Een betere benadering is de Autorisatietoken geretourneerd door uw App Service in de cache en gebruikt u deze eerst voordat u een provider gebaseerde aanmelden.

>[AZURE.NOTE]U kunt het token is uitgegeven door App Services, ongeacht of u verificatie managed client of -service beheerd cache. In deze zelfstudie wordt authentication-service beheerd.

[AZURE.INCLUDE [mobile-windows-universal-dotnet-authenticate-app-with-token](../../includes/mobile-windows-universal-dotnet-authenticate-app-with-token.md)]

##<a name="next-steps"></a>Volgende stappen

Nu dat u deze zelfstudie basisverificatie hebt voltooid, kunt u door te gaan op een van de volgende cursussen:

+ [Push-meldingen toevoegen aan uw app](app-service-mobile-windows-store-dotnet-get-started-push.md)  
  Informatie over het toevoegen van push-meldingen en support voor uw app en configureer uw mobiele App backend Azure melding Hubs gebruikt om push-meldingen te verzenden.

+ [Off line synchronisatie van uw app inschakelen](app-service-mobile-windows-store-dotnet-get-started-offline-data.md)  
  Informatie over het off line ondersteuning van uw app in de backend voor een mobiele App toevoegen. Off line synchronisatie kan eindgebruikers communiceren met een mobiele app&mdash;weergeven, toevoegen of wijzigen van gegevens&mdash;zelfs als er geen netwerkverbinding is.


<!-- URLs. -->
[Get started with your mobile app]: app-service-mobile-windows-store-dotnet-get-started.md

