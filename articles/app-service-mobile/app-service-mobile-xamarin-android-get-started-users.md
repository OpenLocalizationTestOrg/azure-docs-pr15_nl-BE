<properties
    pageTitle="Aan de slag met verificatie voor Mobile Apps in Xamarin Android"
    description="Informatie over het verifiëren van gebruikers van uw Xamarin Android app via een groot aantal identiteitsaanbieders van, met inbegrip van AAD, Google, Facebook, Twitter en Microsoft met mobiele Apps."
    services="app-service\mobile"
    documentationCenter="xamarin"
    authors="adrianhall"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-android"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="add-authentication-to-your-xamarinandroid-app"></a>Verificatie toevoegen aan uw app Xamarin.Android

[AZURE.INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

In dit onderwerp wordt beschreven hoe u gebruikers van een mobiele App van de clienttoepassing te verifiëren. In deze zelfstudie kunt u verificatie toevoegt aan de quickstart-project met een id-provider die wordt ondersteund door Azure Mobile Apps. Na dat met succes geverifieerd en geautoriseerd in de mobiele App, wordt de waarde van de gebruiker-ID weergegeven.

Deze zelfstudie is gebaseerd op de quickstart Mobile App. U moet ook eerst de zelfstudie voor [maken van een app Xamarin.Android]voltooien. Als u niet het project van de server gedownload snel starten, moet u het pakket voor verificatie-extensie toevoegen aan uw project. Zie [werken met de .NET back-endserver SDK voor Azure mobiele toepassingen](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md)voor meer informatie over server-extensie.

##<a name="register"></a>Registreer uw app voor verificatie en App-Services configureren

[AZURE.INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

##<a name="permissions"></a>Machtigingen voor geverifieerde gebruikers beperken

[AZURE.INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

In Visual Studio of Xamarin Studio het client-project worden uitgevoerd op een apparaat of een emulator. Controleer of dat een niet-verwerkte uitzondering met de statuscode van 401 (Unauthorized) treedt op nadat de toepassing wordt gestart. Dit gebeurt omdat de app probeert te krijgen tot uw backend Mobile App als een niet-geverifieerde gebruiker. Verificatie vereist is door de tabel *TodoItem* .

Vervolgens wordt bijgewerkt de app client aanvraag bronnen van de mobiele App-end met een geverifieerde gebruiker.

##<a name="add-authentication"></a>Verificatie toevoegen aan de app.

De app wordt bijgewerkt als u wilt dat gebruikers kunnen Tik op de knop **aanmelden** en verifiëren voordat gegevens worden weergegeven.

1. De volgende code toevoegen aan de klasse **TodoActivity** :

        // Define a authenticated user.
        private MobileServiceUser user;
        private async Task<bool> Authenticate()
        {
                var success = false;
                try
                {
                    // Sign in with Facebook login using a server-managed flow.
                    user = await client.LoginAsync(this,
                        MobileServiceAuthenticationProvider.Facebook);
                    CreateAndShowDialog(string.Format("you are now logged in - {0}",
                        user.UserId), "Logged in!");

                    success = true;
                }
                catch (Exception ex)
                {
                    CreateAndShowDialog(ex, "Authentication failed");
                }
                return success;
        }

        [Java.Interop.Export()]
        public async void LoginUser(View view)
        {
            // Load data only after authentication succeeds.
            if (await Authenticate())
            {
                //Hide the button after authentication succeeds.
                FindViewById<Button>(Resource.Id.buttonLoginUser).Visibility = ViewStates.Gone;

                // Load the data.
                OnRefreshItemsSelected();
            }
        }

    Hiermee maakt u een nieuwe methode voor het verifiëren van een gebruiker en een methode-handler voor een knop Nieuw **aanmelden** . De gebruiker in de bovenstaande voorbeeldcode wordt geverifieerd met behulp van een aanmelding Facebook. Een dialoogvenster wordt gebruikt voor het weergeven van de gebruikers-ID eenmaal is geverifieerd.

    > [AZURE.NOTE] Als u een identiteit Facebook-voorziening gebruikt, wijzigt de waarde doorgegeven aan de **LoginAsync** op een van de volgende: _MicrosoftAccount_, _Twitter_, _Google_of _WindowsAzureActiveDirectory_.

3. In de methode **OnCreate** verwijdert of commentaar van de volgende regel code:

        OnRefreshItemsSelected ();

4. Toevoegen in het bestand Activity_To_Do.axml de volgende definitie van de knop *LoginUser* voordat u de bestaande *AddItem* knop:

        <Button
            android:id="@+id/buttonLoginUser"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:onClick="LoginUser"
            android:text="@string/login_button_text" />

5. Het volgende element toevoegen aan het bestand Strings.xml bronnen:

        <string name="login_button_text">Sign in</string>

6. Het project van de client worden uitgevoerd op een apparaat of een emulator en meld u aan met uw gekozen identiteitsprovider in Visual Studio of Xamarin Studio.

    Als u met succes aangemeld bij de app uw aanmeldings-ID en de lijst van todo-items worden weergegeven en kunt u updates in de gegevens.


<!-- URLs. -->
[Maak een Xamarin.Android-app]: app-service-mobile-xamarin-android-get-started.md
