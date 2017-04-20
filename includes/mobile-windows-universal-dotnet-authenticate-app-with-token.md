
1. In het bestand MainPage.xaml.cs-project toevoegen met de volgende instructies voor het **gebruik van** :

        using System.Linq;      
        using Windows.Security.Credentials;

2. De methode **AuthenticateAsync** vervangen door de volgende code:

        private async System.Threading.Tasks.Task<bool> AuthenticateAsync()
        {
            string message;
            bool success = false;

            // This sample uses the Facebook provider.
            var provider = MobileServiceAuthenticationProvider.Facebook;

            // Use the PasswordVault to securely store and access credentials.
            PasswordVault vault = new PasswordVault();
            PasswordCredential credential = null;

            try
            {
                // Try to get an existing credential from the vault.
                credential = vault.FindAllByResource(provider.ToString()).FirstOrDefault();
            }
            catch (Exception)
            {
                // When there is no matching resource an error occurs, which we ignore.
            }

            if (credential != null)
            {
                // Create a user from the stored credentials.
                user = new MobileServiceUser(credential.UserName);
                credential.RetrievePassword();
                user.MobileServiceAuthenticationToken = credential.Password;

                // Set the user from the stored credentials.
                App.MobileService.CurrentUser = user;

                // Consider adding a check to determine if the token is 
                // expired, as shown in this post: http://aka.ms/jww5vp.

                success = true;
                message = string.Format("Cached credentials for user - {0}", user.UserId);
            }
            else
            {
                try
                {
                    // Login with the identity provider.
                    user = await App.MobileService
                        .LoginAsync(provider);

                    // Create and store the user credentials.
                    credential = new PasswordCredential(provider.ToString(),
                        user.UserId, user.MobileServiceAuthenticationToken);
                    vault.Add(credential);

                    success = true;
                    message = string.Format("You are now logged in - {0}", user.UserId);
                }
                catch (MobileServiceInvalidOperationException)
                {
                    message = "You must log in. Login Required";
                }
            }
            
            var dialog = new MessageDialog(message);
            dialog.Commands.Add(new UICommand("OK"));
            await dialog.ShowAsync();

            return success;
        }

    In deze versie van **AuthenticateAsync**probeert de app met behulp van referenties die zijn opgeslagen in de **PasswordVault** toegang tot de service. Een gewone aanmelden wordt ook uitgevoerd als er geen opgeslagen referenties.

    >[AZURE.NOTE]Een token in cache is mogelijk verlopen en verlopen van het token na verificatie kan ook optreden wanneer de toepassing gebruikt wordt. Als u wilt weten hoe u kunt bepalen of een token is verlopen, Zie [controleren op verlopen verificatietokens](http://aka.ms/jww5vp). Zie voor een oplossing voor de verwerking van vergunning fouten met betrekking tot verlopende tokens, de post [Caching en verwerking van verlopen tokens in Azure Mobile Services SDK beheerd](http://blogs.msdn.com/b/carlosfigueira/archive/2014/03/13/caching-and-handling-expired-tokens-in-azure-mobile-services-managed-sdk.aspx). 

3. De app twee keer opnieuw.

    Merk op dat op de eerste startfase aanmelden bij de provider is vereist. Echter, op de tweede keer opnieuw opstarten in cache opgeslagen referenties gebruikt en aanmelden overgeslagen. 
