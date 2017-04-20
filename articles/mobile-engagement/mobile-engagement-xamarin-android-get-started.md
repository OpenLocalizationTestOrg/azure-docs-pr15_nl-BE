<properties
    pageTitle="Aan de slag met Azure mobiele Engagement voor Xamarin.Android"
    description="Informatie over het Azure Mobile Engagement met Analytics en Push-meldingen te gebruiken voor Xamarin.Android toepassingen."
    services="mobile-engagement"
    documentationCenter="xamarin"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-android"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="06/16/2016"
    ms.author="piyushjo" />

# <a name="get-started-with-azure-mobile-engagement-for-xamarinandroid-apps"></a>Aan de slag met Azure mobiele Engagement voor Xamarin.Android toepassingen

[AZURE.INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

In dit onderwerp ziet u hoe Azure Mobile Engagement om te begrijpen van het gebruik van app en het push-meldingen verzenden naar gesegmenteerde gebruikers van de toepassing van een Xamarin.Android.
Deze zelfstudie toont de eenvoudige broadcast scenario met behulp van Mobile Engagement. In, maakt u een lege Xamarin.Android app die verzamelt basisgegevens en push-meldingen met Google Cloud Messaging (GCM) ontvangt.

Deze zelfstudie is het volgende vereist:

+ [Xamarin Studio](http://xamarin.com/studio). U kunt ook Visual Studio met Xamarin, maar in deze zelfstudie wordt Xamarin Studio. Zie [Setup- en voor Visual Studio en Xamarin installeren](https://msdn.microsoft.com/library/mt613162.aspx)voor installatie-instructies.
+ [Betrokkenheid bij de mobiele Xamarin SDK](https://www.nuget.org/packages/Microsoft.Azure.Engagement.Xamarin/)

> [AZURE.NOTE] Deze zelfstudie hebt u een actieve account Azure. Als u geen account hebt, kunt u een gratis proefperiode account in een paar minuten. Zie voor meer informatie, [Gratis proefperiode van Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-xamarin-android-get-started).

##<a id="setup-azme"></a>Mobiele betrokkenheid bij de instellingen voor uw Android app

[AZURE.INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

##<a id="connecting-app"></a>Verbinding maken met uw app in de backend Mobile Engagement

Deze zelfstudie bevat een "basic integration", dat is de minimaal vereiste gegevens verzamelen en verzenden van een push-bericht. 

Een eenvoudige app maken we met Xamarin Studio om aan te tonen van de integratie.

###<a name="create-a-new-xamarinandroid-project"></a>Maak een nieuw project voor Xamarin.Android

1. Starten **Xamarin Studio** gaat u naar het **bestand** -> **nieuwe** -> **oplossing** 

    ![][1]

2. **Android App** selecteren en vervolgens controleren of de geselecteerde taal **C#** en klikt u op **volgende**.

    ![][2]

3. Vul de **Naam** en de **organisatie-id**. Controleer of op **Google Play Services** is ingeschakeld en klik vervolgens op **volgende**. 

    ![][3]
    
4. De **Naam van het Project**, de **Naam van de oplossing** en de **locatie** bijwerken indien nodig en klikt u op **maken**.

    ![][4]
 
Xamarin Studio maakt de app waarin we Mobile Engagement wordt geïntegreerd. 

###<a name="connect-your-app-to-mobile-engagement-backend"></a>Verbinding maken met uw app Mobile Engagement backend

1. De map **pakketten** in de oplossing windows Klik met de rechtermuisknop en selecteer **Pakketten toevoegen**

    ![][5]

2. De **SDK van Microsoft Azure Mobile Engagement Xamarin** zoeken en toevoegen aan uw oplossing.  

    ![][6]
   
3. **MainActivity.cs** Open en voeg de volgende instructies:

        using Microsoft.Azure.Engagement;
        using Microsoft.Azure.Engagement.Activity;

4. In de `OnCreate` methode, de betrokkenheid bij de mobiele back-end-verbinding initialiseren: toevoegen. Zorg ervoor dat uw **ConnectionString**toevoegen. 

        EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
        engagementConfiguration.ConnectionString = "YourConnectionStringFromAzurePortal";
        EngagementAgent.Init(engagementConfiguration);

###<a name="add-permissions-and-a-service-declaration"></a>Machtigingen en een verklaring van de service toevoegen

1. Open het bestand **Manifest.xml** onder de map eigenschappen. Selecteer tabblad bron, zodat u direct de XML-bron bijwerken.
 
2. Deze machtigingen toevoegen aan de Manifest.xml (die u kunt vinden onder de map **Eigenschappen** ) van het project direct vóór of na de `<application>` code:

        <uses-permission android:name="android.permission.INTERNET"/>
        <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
        <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
        <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />
        <uses-permission android:name="android.permission.VIBRATE" />
        <uses-permission android:name="android.permission.DOWNLOAD_WITHOUT_NOTIFICATION"/>

3. Voeg de volgende tussen de `<application>` en `</application>` codes om aan te geven van de agent-service:

        <service
            android:name="com.microsoft.azure.engagement.service.EngagementService"
            android:exported="false"
            android:label="<Your application name>"
            android:process=":Engagement"/>

4. Vervang in de code die u zojuist hebt geplakt, `"<Your application name>"` in het label. Dit wordt weergegeven in het menu **Instellingen** zien gebruikers services op het apparaat. U kunt bijvoorbeeld het woord 'Service' van dat etiket toevoegen.

###<a name="send-a-screen-to-mobile-engagement"></a>Een scherm naar Mobile Engagement verzenden

Gegevens verzenden en ervoor te zorgen dat de gebruikers actief zijn gestart, moet u ten minste één scherm verzenden op de backend Mobile Engagement. Om dit te doen-ervoor te zorgen dat de `MainActivity` overgenomen van `EngagementActivity` in plaats van `Activity`.

    public class MainActivity : EngagementActivity
    
Ook als u niet kunt van overnemen `EngagementActivity` u moet toevoegen `.StartActivity` en `.EndActivity` methoden in `OnResume` en `OnPause` respectievelijk.  

        protected override void OnResume()
            {
                EngagementAgent.StartActivity(EngagementAgentUtils.BuildEngagementActivityName(Java.Lang.Class.FromType(this.GetType())), null);
                base.OnResume();             
            }
    
            protected override void OnPause()
            {
                EngagementAgent.EndActivity();
                base.OnPause();            
            }

##<a id="monitor"></a>App verbinding met de realtime-controle

[AZURE.INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

##<a id="integrate-push"></a>Push-meldingen inschakelen en messaging-app

Mobile Engagement kunt u interactief werken met en uw gebruikers met push-meldingen en berichten in het kader van de campagnes in app bereiken. In deze module wordt bereiken in de portal Mobile Engagement genoemd.
Uw app in de volgende secties wordt ontvangen.

[AZURE.INCLUDE [Enable Google Cloud Messaging](../../includes/mobile-engagement-enable-google-cloud-messaging.md)]

[AZURE.INCLUDE [Enable in-app messaging](../../includes/mobile-engagement-android-send-push.md)]

[AZURE.INCLUDE [Send notification from portal](../../includes/mobile-engagement-android-send-push-from-portal.md)]

<!-- Images -->
[1]: ./media/mobile-engagement-xamarin-android-get-started/1.png
[2]: ./media/mobile-engagement-xamarin-android-get-started/2.png
[3]: ./media/mobile-engagement-xamarin-android-get-started/3.png
[4]: ./media/mobile-engagement-xamarin-android-get-started/4.png
[5]: ./media/mobile-engagement-xamarin-android-get-started/5.png
[6]: ./media/mobile-engagement-xamarin-android-get-started/6.png
