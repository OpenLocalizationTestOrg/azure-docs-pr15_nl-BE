
1. In de oplossing (of **Solution Explorer** in Visual Studio), klik met de rechtermuisknop op de map **Components** op **Krijg meer onderdelen...**, zoekt de component **Google Cloud Messaging-Client** en toe te voegen aan het project.

2. Open het bestand ToDoActivity.cs en voeg de volgende instructie aan de klasse:

        using Gcm.Client;

3. Voeg de volgende nieuwe code in de klasse **ToDoActivity** : 

        // Create a new instance field for this activity.
        static ToDoActivity instance = new ToDoActivity();

        // Return the current activity instance.
        public static ToDoActivity CurrentActivity
        {
            get
            {
                return instance;
            }
        }
        // Return the Mobile Services client.
        public MobileServiceClient CurrentClient
        {
            get
            {
                return client;
            }
        }

    Hiermee kunt u toegang tot de mobiele clientexemplaar van het serviceproces push-handler.

4.  Nadat de **MobileServiceClient** is gemaakt, kunt u de volgende code toevoegen aan de methode **OnCreate** :

        // Set the current instance of TodoActivity.
        instance = this;

        // Make sure the GCM client is set up correctly.
        GcmClient.CheckDevice(this);
        GcmClient.CheckManifest(this);

        // Register the app for push notifications.
        GcmClient.Register(this, ToDoBroadcastReceiver.senderIDs);

De **ToDoActivity** is nu voorbereid op push-meldingen toe te voegen.