
Het vorige voorbeeld blijkt een standaard aanmelden, die vereist dat de client contact met zowel de identiteitsprovider als de backend Azure service elke keer dat de toepassing wordt gestart. Niet alleen is deze methode inefficiënt, dat kunt u uitvoeren in problemen met gebruik van veel klanten moeten proberen app starten op hetzelfde moment. Een betere benadering is de Autorisatietoken die wordt geretourneerd door de Azure service in de cache en gebruikt u deze eerst voordat u een provider gebaseerde aanmelden. 

>[AZURE.NOTE]U kunt het token is uitgegeven door de back-end Azure service, ongeacht of u verificatie managed client of -service beheerd cache. In deze zelfstudie wordt authentication-service beheerd.


1. Open het bestand ToDoActivity.java en voeg de volgende importinstructies:

        import android.content.Context;
        import android.content.SharedPreferences;
        import android.content.SharedPreferences.Editor;

2. Toevoegen de volgende leden de `ToDoActivity` klasse.

        public static final String SHAREDPREFFILE = "temp"; 
        public static final String USERIDPREF = "uid";  
        public static final String TOKENPREF = "tkn";   


3. Voeg in het bestand ToDoActivity.java de de volgende definitie voor de `cacheUserToken` methode.
 
        private void cacheUserToken(MobileServiceUser user)
        {
            SharedPreferences prefs = getSharedPreferences(SHAREDPREFFILE, Context.MODE_PRIVATE);
            Editor editor = prefs.edit();
            editor.putString(USERIDPREF, user.getUserId());
            editor.putString(TOKENPREF, user.getAuthenticationToken());
            editor.commit();
        }   
  
    Deze methode slaat de gebruikers-id en token in een bestand met voorkeursinstellingen die is gemarkeerd als privé. Dit moet de toegang tot de cache beveiligen zodat andere toepassingen op het apparaat geen toegang tot het token hebt omdat de voorkeur de sandbox voor de toepassing. Echter, als iemand toegang tot het apparaat krijgt, is het mogelijk dat ze toegang tot de tokencache via andere middelen krijgen kunnen. 

    >[AZURE.NOTE]U kunt het token met codering verder beveiligen als token toegang tot uw gegevens wordt beschouwd als uiterst gevoelige en iemand toegang tot het apparaat krijgt. Een volledig veilige oplossing is echter buiten het bestek van deze zelfstudie en afhankelijk van uw beveiligingsvereisten.


4. Voeg in het bestand ToDoActivity.java de de volgende definitie voor de `loadUserTokenCache` methode.

        private boolean loadUserTokenCache(MobileServiceClient client)
        {
            SharedPreferences prefs = getSharedPreferences(SHAREDPREFFILE, Context.MODE_PRIVATE);
            String userId = prefs.getString(USERIDPREF, null); 
            if (userId == null)
                return false;
            String token = prefs.getString(TOKENPREF, null); 
            if (token == null)
                return false;
                
            MobileServiceUser user = new MobileServiceUser(userId);
            user.setAuthenticationToken(token);
            client.setCurrentUser(user);
                
            return true;
        }



5. Vervang in het bestand *ToDoActivity.java* de `authenticate` methode met de volgende methode die wordt gebruikt een tokencache. De provider login wijzigen als u wilt gebruiken een andere account dan Google.

        private void authenticate() {
            // We first try to load a token cache if one exists.
            if (loadUserTokenCache(mClient))
            {
                createTable();
            }
            // If we failed to load a token cache, login and create a token cache
            else
            {
                // Login using the Google provider.    
                ListenableFuture<MobileServiceUser> mLogin = mClient.login(MobileServiceAuthenticationProvider.Google);
        
                Futures.addCallback(mLogin, new FutureCallback<MobileServiceUser>() {
                    @Override
                    public void onFailure(Throwable exc) {
                        createAndShowDialog("You must log in. Login Required", "Error");
                    }           
                    @Override
                    public void onSuccess(MobileServiceUser user) {
                        createAndShowDialog(String.format(
                                "You are now logged in - %1$2s",
                                user.getUserId()), "Success");
                        cacheUserToken(mClient.getCurrentUser());
                        createTable();  
                    }
                });
            }
        }

6. De app en test verificatie met behulp van een geldige account maken. Ten minste tweemaal uitgevoerd. Tijdens de eerste uitvoering moet u wordt gevraagd voor het aanmelden en de tokencache maken. Elke uitvoering zal proberen te laden de tokencache voor verificatie, en zijn niet verplicht om in te loggen.



