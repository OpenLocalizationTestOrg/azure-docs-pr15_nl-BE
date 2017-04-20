<properties
    pageTitle="Beveiligde Push-meldingen met Hubs Azure melding verzenden"
    description="Informatie over het veilig push-meldingen verzenden naar een Android app van Azure. Voorbeelden van code is geschreven in Java en C#."
    documentationCenter="android"
    keywords="push-meldingen, push-meldingen, push-berichten, android push-meldingen"
    authors="ysxu"
    manager="erikre"
    editor=""
    services="notification-hubs"/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="android"
    ms.devlang="java"
    ms.topic="article"
    ms.date="06/29/2016" 
    ms.author="yuaxu"/>

#<a name="sending-secure-push-notifications-with-azure-notification-hubs"></a>Beveiligde Push-meldingen met Hubs Azure melding verzenden

> [AZURE.SELECTOR]
- [Universele Windows](notification-hubs-aspnet-backend-windows-dotnet-wns-secure-push-notification.md)
- [iOS](notification-hubs-aspnet-backend-ios-push-apple-apns-secure-notification.md)
- [Android](notification-hubs-aspnet-backend-android-secure-google-gcm-push-notification.md)

##<a name="overview"></a>Overzicht

> [AZURE.IMPORTANT] Deze zelfstudie hebt u een actieve account Azure. Als u geen account hebt, kunt u een gratis proefperiode account in een paar minuten. Zie voor meer informatie, [Gratis proefperiode van Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A643EE910&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fpartner-xamarin-notification-hubs-ios-get-started).

Ondersteuning voor push-berichten in Microsoft Azure hebt u toegang tot een gemakkelijk te gebruiken, meerdere platforms, schaalvergroting push bericht infrastructuur de implementatie van push-meldingen voor consumenten- en enterprise-toepassingen voor mobiele platforms vereenvoudigt.

Vanwege wettelijke beveiligingsbeperkingen, soms een toepassing wil wellicht iets opnemen in het bericht dat niet kan worden verzonden via de standaard push notification-infrastructuur. In deze zelfstudie wordt beschreven hoe u de dezelfde ervaring door het verzenden van gevoelige informatie via een beveiligd, geverifieerd verbinding tussen de client Android-apparaat en de back-end app te bereiken.

Op een hoog niveau is de stroom als volgt uit:

1. App back-end:
    - Beveiligde payload winkels in back-enddatabase.
    - De ID van deze melding stuurt naar het Android apparaat (geen beveiligde gegevens verzonden).
2. De app op het apparaat bij de ontvangst van de kennisgeving:
    - Aanvragen van de nettolading beveiligde back-end contact op met het Android apparaat.
    - De app kan de nettolading weergeven als een melding op het apparaat.

Het is belangrijk te weten dat in de voorgaande stroom (en in deze zelfstudie), nemen we aan dat het apparaat een verificatietoken in lokale opslag opgeslagen nadat de gebruiker zich aanmeldt. Dit garandeert een volledig naadloze oplossing, zoals het apparaat kunt ophalen van de kennisgeving secure payload deze token gebruikt. Als uw toepassing niet-verificatietokens worden opgeslagen op het apparaat, of als deze tokens verlopen de app apparaat na ontvangst van de push-bericht een algemene kennisgeving de gebruiker hierom te vragen voor het starten van de app moet worden weergegeven. De app vervolgens verifieert de gebruiker en de nettolading van de melding wordt weergegeven.

Deze zelfstudie laat zien hoe veilig push-meldingen te verzenden. Het is gebaseerd op de zelfstudie [Gebruikers waarschuwen](notification-hubs-aspnet-backend-gcm-android-push-to-user-google-notification.md) , dus u moet de stappen in deze zelfstudie eerst als u dat nog niet gedaan hebt.

> [AZURE.NOTE] In deze zelfstudie wordt ervan uitgegaan dat u hebt gemaakt en uw melding hub configureren zoals beschreven in het [Aan de slag met melding Hubs (Android)](notification-hubs-android-push-notification-google-gcm-get-started.md).

[AZURE.INCLUDE [notification-hubs-aspnet-backend-securepush](../../includes/notification-hubs-aspnet-backend-securepush.md)]

## <a name="modify-the-android-project"></a>Het Android-project wijzigen

Nu dat u uw app back-end om alleen de *id* van een push-bericht verzenden hebt gewijzigd, moet u uw Android app die melding afhandelen en terugbellen voor uw back-end voor het ophalen van de veilige bericht wordt weergegeven als u wilt wijzigen.
Om dit doel te bereiken, hebt u om ervoor te zorgen dat uw Android app hoe zichzelf verifiëren met uw back-end weet, wanneer de push-meldingen ontvangen.

We nu wijzigt de stroom *aanmelding* het authentication header-waarde opslaan in de voorkeuren van uw app voor gedeelde. Andere soortgelijke mechanismen kunnen worden gebruikt voor het opslaan van een verificatietoken (bv. OAuth tokens) met de app te gebruiken zonder dat de referenties van de gebruiker.

1. In uw project Android app toevoegen de volgende constanten zijn aan de bovenkant van de **MainActivity** -klasse:

        public static final String NOTIFY_USERS_PROPERTIES = "NotifyUsersProperties";
        public static final String AUTHORIZATION_HEADER_PROPERTY = "AuthorizationHeader";

2. Werk nog steeds in de klasse **MainActivity** de `getAuthorizationHeader()` methode bevat de volgende code:

        private String getAuthorizationHeader() throws UnsupportedEncodingException {
            EditText username = (EditText) findViewById(R.id.usernameText);
            EditText password = (EditText) findViewById(R.id.passwordText);
            String basicAuthHeader = username.getText().toString()+":"+password.getText().toString();
            basicAuthHeader = Base64.encodeToString(basicAuthHeader.getBytes("UTF-8"), Base64.NO_WRAP);

            SharedPreferences sp = getSharedPreferences(NOTIFY_USERS_PROPERTIES, Context.MODE_PRIVATE);
            sp.edit().putString(AUTHORIZATION_HEADER_PROPERTY, basicAuthHeader).commit();

            return basicAuthHeader;
        }

3. Voeg de volgende `import` instructies boven aan het bestand **MainActivity** :

        import android.content.SharedPreferences;

Nu wijzigen we de handler die wordt aangeroepen wanneer de kennisgeving is ontvangen.

4. Klasse wijzigen in de **MyHandler** de `OnReceive()` te bevatten:

        public void onReceive(Context context, Bundle bundle) {
            ctx = context;
            String secureMessageId = bundle.getString("secureId");
            retrieveNotification(secureMessageId);
        }

5. Voeg vervolgens de `retrieveNotification()` methode, vervangt de tijdelijke aanduiding `{back-end endpoint}` met het eindpunt van de back-end verkregen bij het implementeren van uw back-end:

        private void retrieveNotification(final String secureMessageId) {
            SharedPreferences sp = ctx.getSharedPreferences(MainActivity.NOTIFY_USERS_PROPERTIES, Context.MODE_PRIVATE);
            final String authorizationHeader = sp.getString(MainActivity.AUTHORIZATION_HEADER_PROPERTY, null);

            new AsyncTask<Object, Object, Object>() {
                @Override
                protected Object doInBackground(Object... params) {
                    try {
                        HttpUriRequest request = new HttpGet("{back-end endpoint}/api/notifications/"+secureMessageId);
                        request.addHeader("Authorization", "Basic "+authorizationHeader);
                        HttpResponse response = new DefaultHttpClient().execute(request);
                        if (response.getStatusLine().getStatusCode() != HttpStatus.SC_OK) {
                            Log.e("MainActivity", "Error retrieving secure notification" + response.getStatusLine().getStatusCode());
                            throw new RuntimeException("Error retrieving secure notification");
                        }
                        String secureNotificationJSON = EntityUtils.toString(response.getEntity());
                        JSONObject secureNotification = new JSONObject(secureNotificationJSON);
                        sendNotification(secureNotification.getString("Payload"));
                    } catch (Exception e) {
                        Log.e("MainActivity", "Failed to retrieve secure notification - " + e.getMessage());
                        return e;
                    }
                    return null;
                }
            }.execute(null, null, null);
        }


Deze methode roept uw app back-end voor het ophalen van de berichtinhoud met behulp van de referenties die zijn opgeslagen in de gedeelde voorkeuren en weergegeven als een normaal bericht. De kennisgeving wordt gezocht naar de gebruiker app net als een push-bericht.

Houd er rekening mee dat het verdient de voorkeur voor het verwerken van de gevallen van ontbrekende authentication header, eigenschap of afwijzing door de back-end. De specifieke behandeling van deze gevallen is afhankelijk van vooral de gebruikerservaring van uw doel. Eén optie is om een melding met een algemene aanwijzing voor de gebruiker te verifiëren voor het ophalen van de werkelijke melding weer te geven.

## <a name="run-the-application"></a>De toepassing wordt uitgevoerd

Als de toepassing wordt uitgevoerd, het volgende doen:

1. Zorg ervoor dat de **dat appbackend** in Azure is geïmplementeerd. Als Visual Studio, voert u de toepassing **AppBackend** Web API. Een ASP.NET-webpagina wordt weergegeven.

2. Uitgevoerd in Eclips, de app op een Android-apparaat of de emulator.

3. Geef een gebruikersnaam en wachtwoord in de Android app UI. Dit kunnen elke tekenreeks zijn, maar zij moeten dezelfde waarde.

4. In de Android app UI, klikt u op **aanmelden**. Klik vervolgens op **verzenden push**.
