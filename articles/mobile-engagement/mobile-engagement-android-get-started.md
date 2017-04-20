<properties
    pageTitle="Aan de slag met Azure Mobile betrokkenheid bij Android Apps"
    description="Informatie over het gebruik van Azure Mobile Engagement met analytics en push-meldingen voor Android apps."
    services="mobile-engagement"
    documentationCenter="android"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="Java"
    ms.topic="hero-article"
    ms.date="08/10/2016"
    ms.author="piyushjo;ricksal" />

# <a name="get-started-with-azure-mobile-engagement-for-android-apps"></a>Aan de slag met Azure Mobile Engagement voor Android apps

[AZURE.INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

In dit onderwerp ziet u hoe Azure Mobile Engagement om te begrijpen van het gebruik van app en push-meldingen naar gesegmenteerde gebruikers van een Android-toepassing verzenden.
Deze zelfstudie toont de eenvoudige broadcast scenario met behulp van Mobile Engagement. In, maakt u een lege Android app die verzamelt basisgegevens en push-meldingen met Google Cloud Messaging (GCM) ontvangt.

## <a name="prerequisites"></a>Vereisten

Voltooien van deze zelfstudie, moeten de [Android Developer Tools](https://developer.android.com/sdk/index.html), waaronder de geïntegreerde ontwikkelomgeving van Android Studio en de nieuwste Android-platform.

Bovendien dient de [Mobile Engagement Android SDK](https://aka.ms/vq9mfn).

> [AZURE.IMPORTANT] Als u deze zelfstudie hebt voltooid, moet u een actieve account Azure. Als u geen account hebt, kunt u een gratis proefperiode account in een paar minuten. Zie voor meer informatie, [Gratis proefperiode van Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-android-get-started).

## <a name="set-up-mobile-engagement-for-your-android-app"></a>Mobile Engagement voor uw Android app instellen

[AZURE.INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

## <a name="connect-your-app-to-the-mobile-engagement-backend"></a>Verbinding maken met uw app in de backend Mobile Engagement

Deze zelfstudie bevat een "basic integration", dat is de minimaal vereiste gegevens verzamelen en verzenden van een push-bericht. U maakt een eenvoudige app met Android Studio om aan te tonen van de integratie.

De volledige integratie van documentatie vindt u in de [integratie van Mobile Engagement Android SDK](mobile-engagement-android-sdk-overview.md).

### <a name="create-an-android-project"></a>Een Android-project maken

1. Start **Android Studio**en selecteer in het pop-upmenu, **Start een nieuw project voor Android Studio**.

    ![][1]

2. Een toepassingsdomein naam en bedrijfsgegevens bevatten. Noteer wat u invullen, omdat u het later nodig hebt. Klik op **volgende**.

    ![][2]

3. Selecteer het doel form factor- en API-niveau en klik op **volgende**.

    >[AZURE.NOTE] Mobile Engagement vereist API niveau ten minste 10 (Android 2.3.3).

    ![][3]

4. Selecteer **Lege activiteit** hier, dat het enige venster voor deze toepassing is en klik op **volgende**.

    ![][4]

5. Ten slotte, laat u de standaardinstellingen en klik op **Voltooien**.

    ![][5]

Android Studio maakt nu de demo app waarin we Mobile Engagement integreren.

### <a name="include-the-sdk-library-in-your-project"></a>De SDK bibliotheek opnemen in uw project

1. De [betrokkenheid bij de mobiele Android SDK](https://aka.ms/vq9mfn)downloaden.
2. Pak het bestand naar een map op uw computer.
3. Identificeren van de JAR-bibliotheek voor de huidige versie van SDK en naar het Klembord kopiëren.

      ![][6]

4. Ga naar de sectie **Project** (1) en de JAR plakken in de map libs (2).

      ![][7]

5. Voor het laden van de bibliotheek, het project worden gesynchroniseerd.

      ![][8]

### <a name="connect-your-app-to-mobile-engagement-backend-with-the-connection-string"></a>Verbinding maken met uw app backend Mobile Engagement met de verbindingsreeks

1. Kopieer de volgende coderegels in het maken van de activiteit (moet worden gedaan slechts op één plaats van uw toepassing, meestal de belangrijkste activiteit). Voor dit voorbeeld app, openen van de MainActivity onder de src -> main map java -> en voeg het volgende toe:

        EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
        engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
        EngagementAgent.getInstance(this).init(engagementConfiguration);

2. De verwijzingen worden opgelost door op Alt + Enter te drukken of door de volgende importinstructies toe te voegen:

        import com.microsoft.azure.engagement.EngagementAgent;
        import com.microsoft.azure.engagement.EngagementConfiguration;

3. Ga terug naar de klassieke Azure-Portal in **Connection Info** -pagina van uw app en kopieer de **Verbindingsreeks**.

      ![][9]

4. Plak deze in de `setConnectionString` parameter, wordt de gehele tekenreeks weergegeven in de volgende code vervangen:

        engagementConfiguration.setConnectionString("Endpoint=my-company-name.device.mobileengagement.windows.net;SdkKey=********************;AppId=*********");

### <a name="add-permissions-and-a-service-declaration"></a>Machtigingen en een verklaring van de service toevoegen

1. Deze machtigingen toevoegen aan de Manifest.xml van uw project, onmiddellijk voor of na de `<application>` code:

        <uses-permission android:name="android.permission.INTERNET"/>
        <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
        <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
        <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />
        <uses-permission android:name="android.permission.VIBRATE" />
        <uses-permission android:name="android.permission.DOWNLOAD_WITHOUT_NOTIFICATION"/>

2. Toevoegen voor het declareren van de agent-service deze code tussen de `<application>` en `</application>` tags:

        <service
            android:name="com.microsoft.azure.engagement.service.EngagementService"
            android:exported="false"
            android:label="<Your application name>"
            android:process=":Engagement"/>

3. Vervang in de code die u hebt geplakt, `"<Your application name>"` in het label, die wordt weergegeven in het menu **Instellingen** , waar u de services op het apparaat kunt zien. U kunt bijvoorbeeld het woord 'Service' van dat etiket toevoegen.

### <a name="send-a-screen-to-mobile-engagement"></a>Een scherm naar Mobile Engagement verzenden

Als u wilt beginnen met het verzenden van gegevens en zorgen ervoor dat de gebruikers actief zijn, moet u ten minste één scherm (activiteit) sturen op de backend Mobile Engagement.

Ga naar **MainActivity.java** en voeg de volgende ter vervanging van de basisklasse van **MainActivity** naar **EngagementActivity**:

    public class MainActivity extends EngagementActivity {

> [AZURE.NOTE] Als de basisklasse geen *activiteit is*, raadpleegt u de [Geavanceerde Android melden](mobile-engagement-android-advanced-reporting.md#modifying-your-codeactivitycode-classes) voor informatie over het overnemen van de verschillende klassen.


Opmerking de volgende regel in dit eenvoudige voorbeeld scenario:

    // setSupportActionBar(toolbar);

Als u wilt behouden de `ActionBar` in uw app, Zie [Geavanceerde Android melden](mobile-engagement-android-advanced-reporting.md#modifying-your-codeactivitycode-classes).

## <a name="connect-app-with-real-time-monitoring"></a>App verbinding met de realtime-controle

[AZURE.INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

## <a name="enable-push-notifications-and-in-app-messaging"></a>Push-meldingen inschakelen en messaging-app

Tijdens een campagne kunt Mobile Engagement u interactief werken met en uw gebruikers met push-meldingen en messaging-app bereiken. In deze module wordt bereiken in de portal Mobile Engagement genoemd.
In het volgende gedeelte stelt uw app ontvangen.

### <a name="copy-sdk-resources-in-your-project"></a>SDK resources in uw project kopiëren

1. Ga terug naar de inhoud van de SDK gedownload en kopieer de map **res** .

    ![][10]

2. Ga terug naar Android Studio, selecteert u **de hoofdmap van de project-bestanden** , en plakt u de resources aan uw project toevoegen.

    ![][11]

[AZURE.INCLUDE [Enable Google Cloud Messaging](../../includes/mobile-engagement-enable-google-cloud-messaging.md)]

[AZURE.INCLUDE [Enable in-app messaging](../../includes/mobile-engagement-android-send-push.md)]

[AZURE.INCLUDE [Send notification from portal](../../includes/mobile-engagement-android-send-push-from-portal.md)]

## <a name="next-steps"></a>Volgende stappen

Ga naar de [Android SDK](mobile-engagement-android-sdk-overview.md) voor gedetailleerde kennis over de integratie van de SDK.

<!-- Images. -->
[1]: ./media/mobile-engagement-android-get-started/android-studio-new-project.png
[2]: ./media/mobile-engagement-android-get-started/android-studio-project-props.png
[3]: ./media/mobile-engagement-android-get-started/android-studio-project-props2.png
[4]: ./media/mobile-engagement-android-get-started/android-studio-add-activity.png
[5]: ./media/mobile-engagement-android-get-started/android-studio-activity-name.png
[6]: ./media/mobile-engagement-android-get-started/sdk-content.png
[7]: ./media/mobile-engagement-android-get-started/paste-jar.png
[8]: ./media/mobile-engagement-android-get-started/sync-project.png
[9]: ./media/mobile-engagement-android-get-started/app-connection-info-page.png
[10]: ./media/mobile-engagement-android-get-started/copy-resources.png
[11]: ./media/mobile-engagement-android-get-started/paste-resources.png
