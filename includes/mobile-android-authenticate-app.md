
1. In **Project Explorer** in Android Studio, opent u het bestand ToDoActivity.java en voeg de volgende importinstructies.

        import java.util.concurrent.ExecutionException;
        import java.util.concurrent.atomic.AtomicBoolean;

        import android.content.Context;
        import android.content.SharedPreferences;
        import android.content.SharedPreferences.Editor;

        import com.microsoft.windowsazure.mobileservices.authentication.MobileServiceAuthenticationProvider;
        import com.microsoft.windowsazure.mobileservices.authentication.MobileServiceUser;

2. De volgende methode toevoegen aan de klasse **ToDoActivity** : 
    
        private void authenticate() {
            // Login using the Google provider.
            
            ListenableFuture<MobileServiceUser> mLogin = mClient.login(MobileServiceAuthenticationProvider.Google);
    
            Futures.addCallback(mLogin, new FutureCallback<MobileServiceUser>() {
                @Override
                public void onFailure(Throwable exc) {
                    createAndShowDialog((Exception) exc, "Error");
                }           
                @Override
                public void onSuccess(MobileServiceUser user) {
                    createAndShowDialog(String.format(
                            "You are now logged in - %1$2s",
                            user.getUserId()), "Success");
                    createTable();  
                }
            });     
        }


    Hiermee maakt u een nieuwe methode voor het verwerken van het verificatieproces. De gebruiker wordt geverifieerd met behulp van een aanmelding van Google. Een dialoogvenster weergegeven waarin u de ID van de geverifieerde gebruiker. U kunt niet doorgaan zonder een positieve verificatie.

    > [AZURE.NOTE] Als u een identiteit Google-voorziening gebruikt, wijzigt de waarde doorgegeven aan de methode **aanmelden** boven aan een van de volgende: _MicrosoftAccount_, _Facebook_, _Twitter_of _windowsazureactivedirectory_.

3. Toevoegen in de methode **onCreate** de volgende regel code na de code die wordt het `MobileServiceClient` object.

        authenticate();

    Dit gesprek begint het verificatieproces.

4. De resterende code na verplaatsen `authenticate();` in de **onCreate** -methode om een nieuwe methode van **createTable** , die er als volgt uit:

        private void createTable() {
    
            // Get the table instance to use.
            mToDoTable = mClient.getTable(ToDoItem.class);
    
            mTextNewToDo = (EditText) findViewById(R.id.textNewToDo);
    
            // Create an adapter to bind the items with the view.
            mAdapter = new ToDoItemAdapter(this, R.layout.row_list_to_do);
            ListView listViewToDo = (ListView) findViewById(R.id.listViewToDo);
            listViewToDo.setAdapter(mAdapter);
    
            // Load the items from Azure.
            refreshItemsFromTable();
        }

9. In het menu **uitvoeren** en klik vervolgens op **toepassing uitvoeren** om te beginnen de app en meld u aan met uw identiteitsprovider gekozen. 

    Als u met succes aangemeld bij de app moet zonder fouten worden uitgevoerd en moet mogelijk zijn de backend-service opvragen en wijzigingen in gegevens.