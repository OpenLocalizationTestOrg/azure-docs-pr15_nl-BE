<properties
    pageTitle="Aan de slag met Azure melding Hubs met Baidu | Microsoft Azure"
    description="In deze zelfstudie leert u Azure melding Hubs gebruiken voor push-meldingen op Android-apparaten met Baidu."
    services="notification-hubs"
    documentationCenter="android"
    authors="ysxu"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.devlang="java"
    ms.topic="hero-article"
    ms.tgt_pltfrm="mobile-baidu"
    ms.workload="mobile"
    ms.date="08/19/2016"
    ms.author="yuaxu"/>

# <a name="get-started-with-notification-hubs-using-baidu"></a>Aan de slag met melding Hubs met Baidu

[AZURE.INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

##<a name="overview"></a>Overzicht

Baidu wolk push is een Chinese cloud-service kunt u push-meldingen verzenden naar mobiele apparaten. Deze service is met name handig in China, waar levert push-meldingen aan Android complex is door de aanwezigheid van andere app winkels en push-diensten, naast de beschikbaarheid van Android-apparaten die normaal gesproken niet zijn verbonden met GCM (Google Cloud Messaging).

##<a name="prerequisites"></a>Vereisten

Deze zelfstudie is het volgende vereist:

+ Android SDK (nemen we aan dat u Eclips gebruikt), die u vanaf de <a href="http://go.microsoft.com/fwlink/?LinkId=389797">site van Android downloaden kunt</a>
+ [Android SDK voor mobiele Services]
+ [Baidu Push Android SDK]

>[AZURE.NOTE] Deze zelfstudie hebt u een actieve account Azure. Als u geen account hebt, kunt u een gratis proefperiode account in een paar minuten. Zie voor meer informatie, [Gratis proefperiode van Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-baidu-get-started%2F).


##<a name="create-a-baidu-account"></a>Maak een account met Baidu

Voor het gebruik van Baidu, hebt u een account voor Baidu. Als u al hebt, log in op de [portal van Baidu] en gaat u verder met de volgende stap. Raadpleeg anders de onderstaande instructies voor het maken van een account Baidu.  

1. Ga naar de [Baidu portal] en klik op de koppeling**登录**(**aanmelding**). Klik op**立即注册**om de account registratie-proces te starten.

    ![][1]

2. Voer de vereiste gegevens, telefoon/e-mail adres, wachtwoord en controle, en klik op **aanmelden**.

    ![][2]

3. U ontvangt een e-mailbericht naar het e-mailadres dat u met een koppeling om uw account Baidu te activeren.

    ![][3]

4. Aanmelden bij uw e-mailaccount, open de activering Baidu post en klik op de activeringskoppeling om uw Baidu-account te activeren.

    ![][4]

Wanneer u een geactiveerde Baidu-account hebt, aanmelden bij de [portal Baidu].

##<a name="register-as-a-baidu-developer"></a>Registreren als ontwikkelaar Baidu

1. Zodra u bent ingelogd in de [portal Baidu], klikt u op**更多 >>** (**meer**).

    ![][5]

2. Schuif omlaag in de sectie**站长与开发者服务 (Webmaster en Developer Services)** en klik op**百度开放云平台**(**Baidu wolk platform openen**).

    ![][6]

3. Klik op de volgende pagina op**开发者服务**(**Developer Services**) op de rechterbovenhoek.

    ![][7]

4. Klik op de volgende pagina**注册开发者**(**Geregistreerd ontwikkelaars**) in het menu in de rechterbovenhoek.

    ![][8]

5. Voer uw naam, een beschrijving en een mobiel telefoonnummer voor het ontvangen van een SMS-bericht voor verificatie en klik op**送验证码**(**Verificatiecode verzenden**). Houd er rekening mee dat voor internationale telefoonnummers, u moet tussen haakjes het land in. Bijvoorbeeld voor een aantal Verenigde Staten dient **(1) 1234567890**.

    ![][9]

6. U moet vervolgens een bericht tekst met een verificatiecode zoals in het volgende voorbeeld:

    ![][10]

7. Voer de verificatiecode in het bericht in de**验证码**(**bevestigingscode**).

8. Ten slotte voert u de ontwikkelaar registratie door de Baidu-overeenkomst te accepteren en te klikken op**提交**(**indienen**). De volgende pagina ziet u op de registratie is voltooid:

    ![][11]

##<a name="create-a-baidu-cloud-push-project"></a>Baidu wolk push-project maken

Wanneer u een Baidu wolk push-project maakt, ontvangt u uw ID app API-sleutel en geheime sleutel.

1. Zodra u bent ingelogd in de [portal Baidu], klikt u op**更多 >>** (**meer**).

    ![][5]

2. Schuif omlaag in de sectie**站长与开发者服务**(**Webmaster en Developer Services**) en klik op**百度开放云平台**(**Baidu wolk platform openen**).

    ![][6]

3. Klik op de volgende pagina op**开发者服务**(**Developer Services**) op de rechterbovenhoek.

    ![][7]

4. Klik op de volgende pagina op**云推送**(**Push-wolk**) uit de sectie**云服务**(**Cloud-Services**).

    ![][12]

5. Als u een geregistreerde ontwikkelaar bent, ziet u**管理控制台**(**Management Console**) op het bovenste menu. Klik op**开发者服务管理**(**ontwikkelaars Management Service**).

    ![][13]

6. Klik op de volgende pagina op**创建工程**(**Project maken**).

    ![][14]

7. De naam van een toepassing en klik op**创建**(**maken**).

    ![][15]

8. Na het succesvol maken van een project Baidu wolk push ziet u een pagina met de **AppID**, **API-sleutel**en **Geheime sleutel**. Maak een notitie van de API-sleutel en geheime sleutel, die we later gebruiken.

    ![][16]

9. Het project voor push-meldingen configureren door te klikken op**云推送**(**Cloud Push**) in het linkerdeelvenster.

    ![][31]

10. Klik op de knop**推送设置**(**Push-instellingen**) op de volgende pagina.

    ![][32]  

11. Op de configuratiepagina toevoegen de pakketnaam dat u zal worden gebruikt in uw Android-project in het veld**应用包名**(**Application package**) en klik op**保存设置**(**Opslaan**).  

    ![][33]

Ziet u de**保存成功!** (**met succes opgeslagen!**) bericht.

##<a name="configure-your-notification-hub"></a>Uw melding hub configureren

1. Aanmelden bij de [Klassieke Azure-Portal], en klik vervolgens op **+ Nieuw** onder aan het scherm.

2. **App Services**klikt u op, klikt u op **De Bus Service**, klikt u op **Melding Hub**en klik op **Snelle invoer**.

3. Geef een naam op voor uw **Melding Hub**, selecteert u de **regio** en de **Namespace** waar deze hub melding wordt gemaakt, en klik vervolgens op **een nieuwe melding Hub maken**.  

    ![][17]

4. Klik op de naamruimte waarin u uw hub-melding gemaakt en klik op **Melding Hubs** boven.

    ![][18]

5. Selecteer de hub melding die u hebt gemaakt en klik vervolgens op **configureren** in het bovenste menu.

    ![][19]

6. Ga naar de sectie **Instellingen voor meldingen van baidu** en voer de API-sleutel en geheime sleutel die u hebt gekregen van de console Baidu eerder voor uw project Baidu wolk push. Klik op **Opslaan**.

    ![][20]

7. Klik op het tabblad **Dashboard** boven voor de hub van de melding en klik vervolgens op **Weergave-verbindingsreeks**.

    ![][21]

8. Maak een notitie van de **DefaultListenSharedAccessSignature** en **DefaultFullSharedAccessSignature** vanuit het venster **toegang tot informatie** .

    ![][22]

##<a name="connect-your-app-to-the-notification-hub"></a>Uw app verbinding met de melding hub

1. Maak een nieuw project voor Android in ADT Eclips, (**bestand** > **Nieuw** > **Android Application-Project**).

    ![][23]

2. Voer een **Naam** en ervoor te zorgen dat de versie **SDK voor minimaal vereist** is ingesteld op **16 API: Android 4.1**.

    ![][24]

3. Klik op **volgende** en voer de wizard verder totdat het venster **Activiteit maken** wordt weergegeven. Zorg ervoor dat de **Lege activiteit** is geselecteerd, en ten slotte selecteert u **klaar bent met het** maken van een nieuwe Android-toepassing.

    ![][25]

4. Zorg ervoor dat het **Project bouwen doel** juist is ingesteld.

    ![][26]

5. Download het bestand melding-hubs-0.4.jar van **het tabblad van de [Kennisgeving-Hubs-Android-SDK op Bintray](https://bintray.com/microsoftazuremobile/SDK/Notification-Hubs-Android-SDK/0.4)** . Het bestand toevoegen aan de map **libs** van uw project Eclips en vernieuw de *libs* map.

6. Downloaden en uitpakken van de [Android SDK Baidu Push], open de map **libs** en kopieer het bestand **pushservice x.y.z** jar en de **armeabi** & **mips** -mappen in de map **libs** van uw Android-toepassing.

7. Open het bestand **AndroidManifest.xml** van uw Android-project en de machtigingen die voor de Baidu SDK vereist zijn toevoegen.

        <uses-permission android:name="android.permission.INTERNET" />
        <uses-permission android:name="android.permission.READ_PHONE_STATE" />
        <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
        <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />
        <uses-permission android:name="android.permission.WRITE_SETTINGS" />
        <uses-permission android:name="android.permission.VIBRATE" />
        <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
        <uses-permission android:name="android.permission.DISABLE_KEYGUARD" />
        <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
        <uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />
        <uses-permission android:name="android.permission.ACCESS_DOWNLOAD_MANAGER" />
        <uses-permission android:name="android.permission.DOWNLOAD_WITHOUT_NOTIFICATION" />

8. De eigenschap **android: naam** toevoegen aan uw **toepassing** element in de **AndroidManifest.xml**, *yourprojectname* (bijv. **com.example.BaiduTest**) te vervangen. Zorg ervoor dat dit de projectnaam overeenkomt met de naam die u hebt geconfigureerd in de console Baidu.

        <application android:name="yourprojectname.DemoApplication"

9. Voeg de volgende configuratie in het toepassingselement na de **. MainActivity** activiteit-element vervangt *yourprojectname* (bijv. **com.example.BaiduTest**):

        <receiver android:name="yourprojectname.MyPushMessageReceiver">
            <intent-filter>
                <action android:name="com.baidu.android.pushservice.action.MESSAGE" />
                <action android:name="com.baidu.android.pushservice.action.RECEIVE" />
                <action android:name="com.baidu.android.pushservice.action.notification.CLICK" />
            </intent-filter>
        </receiver>

        <receiver android:name="com.baidu.android.pushservice.PushServiceReceiver"
            android:process=":bdservice_v1">
            <intent-filter>
                <action android:name="android.intent.action.BOOT_COMPLETED" />
                <action android:name="android.net.conn.CONNECTIVITY_CHANGE" />
                <action android:name="com.baidu.android.pushservice.action.notification.SHOW" />
            </intent-filter>
        </receiver>

        <receiver android:name="com.baidu.android.pushservice.RegistrationReceiver"
            android:process=":bdservice_v1">
            <intent-filter>
                <action android:name="com.baidu.android.pushservice.action.METHOD" />
                <action android:name="com.baidu.android.pushservice.action.BIND_SYNC" />
            </intent-filter>
            <intent-filter>
                <action android:name="android.intent.action.PACKAGE_REMOVED"/>
                <data android:scheme="package" />
            </intent-filter>
        </receiver>

        <service
            android:name="com.baidu.android.pushservice.PushService"
            android:exported="true"
            android:process=":bdservice_v1"  >
            <intent-filter>
                <action android:name="com.baidu.android.pushservice.action.PUSH_SERVICE" />
            </intent-filter>
        </service>

9. Een nieuwe klasse met de naam **ConfigurationSettings.java** aan het project toevoegen.

    ![][28]

    ![][29]

10. Voeg de volgende code toe:

        public class ConfigurationSettings {
                public static String API_KEY = "...";
                public static String NotificationHubName = "...";
                public static String NotificationHubConnectionString = "...";
            }

    Stel de waarde van **API_KEY** bij u opgehaald uit de Baidu wolk project eerder **NotificationHubName** met de naam van uw melding hub van het klassieke Portal Azure en **NotificationHubConnectionString** met DefaultListenSharedAccessSignature van de klassieke Azure-Portal.

11. Toevoegen van een nieuwe klasse met de naam **DemoApplication.java**en voeg de volgende code toe:

        import com.baidu.frontia.FrontiaApplication;

        public class DemoApplication extends FrontiaApplication {
            @Override
            public void onCreate() {
                super.onCreate();
            }
        }

12. Een andere nieuwe klasse met de naam **MyPushMessageReceiver.java**toevoegen en de volgende code aan toe te voegen. Dit is de klasse die de push-meldingen die worden ontvangen van de Baidu push-server verwerkt.

        import java.util.List;
        import android.content.Context;
        import android.os.AsyncTask;
        import android.util.Log;
        import com.baidu.frontia.api.FrontiaPushMessageReceiver;
        import com.microsoft.windowsazure.messaging.NotificationHub;

        public class MyPushMessageReceiver extends FrontiaPushMessageReceiver {
            /** TAG to Log */
            public static NotificationHub hub = null;
            public static String mChannelId, mUserId;
            public static final String TAG = MyPushMessageReceiver.class
                    .getSimpleName();

            @Override
            public void onBind(Context context, int errorCode, String appid,
                    String userId, String channelId, String requestId) {
                String responseString = "onBind errorCode=" + errorCode + " appid="
                        + appid + " userId=" + userId + " channelId=" + channelId
                        + " requestId=" + requestId;
                Log.d(TAG, responseString);
                mChannelId = channelId;
                mUserId = userId;

                try {
                 if (hub == null) {
                        hub = new NotificationHub(
                                ConfigurationSettings.NotificationHubName,
                                ConfigurationSettings.NotificationHubConnectionString,
                                context);
                        Log.i(TAG, "Notification hub initialized");
                    }
                } catch (Exception e) {
                   Log.e(TAG, e.getMessage());
                }

                registerWithNotificationHubs();
            }

            private void registerWithNotificationHubs() {
               new AsyncTask<Void, Void, Void>() {
                  @Override
                  protected Void doInBackground(Void... params) {
                     try {
                         hub.registerBaidu(mUserId, mChannelId);
                         Log.i(TAG, "Registered with Notification Hub - '"
                                + ConfigurationSettings.NotificationHubName + "'"
                                + " with UserId - '"
                                + mUserId + "' and Channel Id - '"
                                + mChannelId + "'");
                     } catch (Exception e) {
                         Log.e(TAG, e.getMessage());
                     }
                     return null;
                 }
               }.execute(null, null, null);
            }

            @Override
            public void onSetTags(Context context, int errorCode,
                    List<String> sucessTags, List<String> failTags, String requestId) {
                String responseString = "onSetTags errorCode=" + errorCode
                        + " sucessTags=" + sucessTags + " failTags=" + failTags
                        + " requestId=" + requestId;
                Log.d(TAG, responseString);
            }

            @Override
            public void onDelTags(Context context, int errorCode,
                    List<String> sucessTags, List<String> failTags, String requestId) {
                String responseString = "onDelTags errorCode=" + errorCode
                        + " sucessTags=" + sucessTags + " failTags=" + failTags
                        + " requestId=" + requestId;
                Log.d(TAG, responseString);
            }

            @Override
            public void onListTags(Context context, int errorCode, List<String> tags,
                    String requestId) {
                String responseString = "onListTags errorCode=" + errorCode + " tags="
                        + tags;
                Log.d(TAG, responseString);
            }

            @Override
            public void onUnbind(Context context, int errorCode, String requestId) {
                String responseString = "onUnbind errorCode=" + errorCode
                        + " requestId = " + requestId;
                Log.d(TAG, responseString);
            }

            @Override
            public void onNotificationClicked(Context context, String title,
                    String description, String customContentString) {
                String notifyString = "title=\"" + title + "\" description=\""
                        + description + "\" customContent=" + customContentString;
                Log.d(TAG, notifyString);
            }

            @Override
            public void onMessage(Context context, String message,
                    String customContentString) {
                String messageString = "message=\"" + message + "\" customContentString=" + customContentString;
                Log.d(TAG, messageString);
            }
        }

13. **MainActivity.java**Open en voeg het volgende toe aan de methode **onCreate** :

            PushManager.startWork(getApplicationContext(),
                    PushConstants.LOGIN_TYPE_API_KEY, ConfigurationSettings.API_KEY);

14. De volgende importinstructies boven openen:

            import com.baidu.android.pushservice.PushConstants;
            import com.baidu.android.pushservice.PushManager;

##<a name="send-notifications-to-your-app"></a>Meldingen verzenden naar uw app


U kunt snel testen meldingen ontvangen in uw app door het verzenden van meldingen in [Azure Portal](https://portal.azure.com/) met behulp van de knop **Verzenden testen** op de hub melding zoals in het onderstaande scherm.

![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-test-send-wns.png)

Push-meldingen worden meestal verzonden in een back-end services, zoals mobiele diensten of met behulp van een compatibele bibliotheek. U kunt ook de REST API rechtstreeks bericht om berichten te verzenden als een bibliotheek niet beschikbaar voor uw back-end is.

In deze zelfstudie wordt Houd het simpel en gewoon aantonen dat uw client app testen door het verzenden van berichten met behulp van de .NET SDK voor kennisgeving hubs in een consoletoepassing in plaats van een back-end-service. Het is raadzaam de zelfstudie [Gebruiken kennisgeving Hubs op push-meldingen aan gebruikers](notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md) als volgende stap voor het verzenden van meldingen van een ASP.NET-end. De volgende benaderingen kunnen echter worden gebruikt voor het verzenden van meldingen:

* **REST-Interface**: kunt u de melding op elk back-end-platform met de [Overige interface](http://msdn.microsoft.com/library/windowsazure/dn223264.aspx)ondersteunen.

* **Microsoft Azure melding Hubs .NET SDK**: In de Nuget Package Manager voor Visual Studio [- Installatiepakket Microsoft.Azure.NotificationHubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/)uitvoeren.

* **Node.js** : [het gebruik van Hubs melding van Node.js](notification-hubs-nodejs-push-notification-tutorial.md).

* **Mobiele Apps**: Zie voor een voorbeeld van het verzenden van meldingen van een end Azure App Service Mobile Apps die geïntegreerd met de melding Hubs, [push-meldingen voor uw mobiele app toevoegen](../app-service-mobile/app-service-mobile-windows-store-dotnet-get-started-push.md).

* **Java / PHP**: Zie 'Het gebruik van Hubs melding van Java/PHP' voor een voorbeeld van hoe om meldingen te verzenden met behulp van de REST API's ([Java](notification-hubs-java-push-notification-tutorial.md) | [PHP](notification-hubs-php-push-notification-tutorial.md)).

##<a name="optional-send-notifications-from-a-net-console-app"></a>(Optioneel) Meldingen verzenden vanuit een console .NET app.

We zien met behulp van een console .NET app bericht in deze sectie.

1. Maak een nieuwe Visual C# consoletoepassing:

    ![][30]

2. Het **project standaard** ingesteld voor de nieuwe console application-project in het consolevenster met Package Manager en klik in het consolevenster met de volgende opdracht worden uitgevoerd:

        Install-Package Microsoft.Azure.NotificationHubs

    Hiermee wordt een verwijzing naar de Azure melding Hubs SDK met behulp van het <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">pakket Microsoft.Azure.Notification Hubs NuGet</a>toegevoegd.

    ![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-package-manager.png)

3. Open het bestand **Program.cs** en voeg de volgende instructie:

        using Microsoft.Azure.NotificationHubs;

4. In de `Program` klasse, voegt u de volgende methode en *DefaultFullSharedAccessSignatureSASConnectionString* en *NotificationHubName* vervangen door de waarden die u hebt.

        private static async void SendNotificationAsync()
        {
            NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString("DefaultFullSharedAccessSignatureSASConnectionString", "NotificationHubName");
            string message = "{\"title\":\"((Notification title))\",\"description\":\"Hello from Azure\"}";
            var result = await hub.SendBaiduNativeNotificationAsync(message);
        }

5. Voeg de volgende regels in de **Main** -methode:

         SendNotificationAsync();
         Console.ReadLine();

##<a name="test-your-app"></a>Test uw app

Test dit app met een daadwerkelijke telefoon, gewoon de telefoon op uw computer aansluit met behulp van een USB-kabel. Dit laadt uw app op de telefoon aangesloten.

Test dit app met de emulator, op de bovenste werkbalk Eclips op **uitvoeren**en selecteer vervolgens uw app. Hiermee start de emulator, en vervolgens geladen en de toepassing wordt uitgevoerd.

De app opgehaald van het 'id' en 'channelId' uit de Baidu Push notification service en registreert bij de melding hub.

Een test om melding te verzenden dat kunt u het tabblad foutopsporing van de klassieke Azure-Portal. Als u de Visual Studio .NET consoletoepassing ingebouwd, maar druk op F5 in Visual Studio om de toepassing te starten. De toepassing ontvangt een melding die wordt weergegeven in het systeemvak voor bovenkant van het apparaat of de emulator.


<!-- Images. -->
[1]: ./media/notification-hubs-baidu-get-started/BaiduRegistration.png
[2]: ./media/notification-hubs-baidu-get-started/BaiduRegistrationInput.png
[3]: ./media/notification-hubs-baidu-get-started/BaiduConfirmation.png
[4]: ./media/notification-hubs-baidu-get-started/BaiduActivationEmail.png
[5]: ./media/notification-hubs-baidu-get-started/BaiduRegistrationMore.png
[6]: ./media/notification-hubs-baidu-get-started/BaiduOpenCloudPlatform.png
[7]: ./media/notification-hubs-baidu-get-started/BaiduDeveloperServices.png
[8]: ./media/notification-hubs-baidu-get-started/BaiduDeveloperRegistration.png
[9]: ./media/notification-hubs-baidu-get-started/BaiduDevRegistrationInput.png
[10]: ./media/notification-hubs-baidu-get-started/BaiduDevRegistrationConfirmation.png
[11]: ./media/notification-hubs-baidu-get-started/BaiduDevConfirmationFinal.png
[12]: ./media/notification-hubs-baidu-get-started/BaiduCloudPush.png
[13]: ./media/notification-hubs-baidu-get-started/BaiduDevSvcMgmt.png
[14]: ./media/notification-hubs-baidu-get-started/BaiduCreateProject.png
[15]: ./media/notification-hubs-baidu-get-started/BaiduCreateProjectInput.png
[16]: ./media/notification-hubs-baidu-get-started/BaiduProjectKeys.png
[17]: ./media/notification-hubs-baidu-get-started/AzureNHCreation.png
[18]: ./media/notification-hubs-baidu-get-started/NotificationHubs.png
[19]: ./media/notification-hubs-baidu-get-started/NotificationHubsConfigure.png
[20]: ./media/notification-hubs-baidu-get-started/NotificationHubBaiduConfigure.png
[21]: ./media/notification-hubs-baidu-get-started/NotificationHubsConnectionStringView.png
[22]: ./media/notification-hubs-baidu-get-started/NotificationHubsConnectionString.png
[23]: ./media/notification-hubs-baidu-get-started/EclipseNewProject.png
[24]: ./media/notification-hubs-baidu-get-started/EclipseProjectCreation.png
[25]: ./media/notification-hubs-baidu-get-started/EclipseBlankActivity.png
[26]: ./media/notification-hubs-baidu-get-started/EclipseProjectBuildProperty.png
[27]: ./media/notification-hubs-baidu-get-started/EclipseBaiduReferences.png
[28]: ./media/notification-hubs-baidu-get-started/EclipseNewClass.png
[29]: ./media/notification-hubs-baidu-get-started/EclipseConfigSettingsClass.png
[30]: ./media/notification-hubs-baidu-get-started/ConsoleProject.png
[31]: ./media/notification-hubs-baidu-get-started/BaiduPushConfig1.png
[32]: ./media/notification-hubs-baidu-get-started/BaiduPushConfig2.png
[33]: ./media/notification-hubs-baidu-get-started/BaiduPushConfig3.png

<!-- URLs. -->
[Android SDK voor mobiele Services]: https://go.microsoft.com/fwLink/?LinkID=280126&clcid=0x409
[Baidu Push Android SDK]: http://developer.baidu.com/wiki/index.php?title=docs/cplat/push/sdk/clientsdk
[Azure klassieke Portal]: https://manage.windowsazure.com/
[Baidu portal]: http://www.baidu.com/
