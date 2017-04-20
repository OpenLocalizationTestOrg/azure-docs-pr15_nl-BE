<properties
    pageTitle="Integratie van Azure betrokkenheid bij de mobiele Android SDK"
    description="Meest recente updates en procedures voor de Android SDK voor Azure Mobile Engagement"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="piyushjo" />

#<a name="how-to-integrate-gcm-with-mobile-engagement"></a>GCM integreren met mobiele Engagement

> [AZURE.IMPORTANT] U moet de integratie procedure in de betrokkenheid bij de integratie van het op Android document voordat u deze handleiding volgen.
>
> Dit document is alleen nuttig als u reeds geïntegreerd de Reach-module en een plan om Google Play-apparaten. Reach campagnes in uw toepassing integreren, lees eerst integreren Engagement bereiken op Android.

##<a name="introduction"></a>Inleiding

Integratie van GCM, kan de toepassing moet worden geplaatst.

GCM-nettoladingen altijd geduwd naar de SDK bevat de `azme` sleutel in het object. Dus als u in uw toepassing GCM voor andere doelen gebruikt, kunt u filteren op basis van die sleutel gereedschap duwt.

> [AZURE.IMPORTANT] Alleen apparaten met Android 2.2 of hierboven met Google Play geïnstalleerd en met Google achtergrond verbinding ingeschakeld kan worden geduwd met GCM; u kunt echter integreren deze code veilig op niet-ondersteunde apparaten (alleen intenties wordt gebruikt).

##<a name="create-a-google-cloud-messaging-project-with-api-key"></a>Een project Google Cloud Messaging API sleutel maken

[AZURE.INCLUDE [mobile-engagement-enable-Google-cloud-messaging](../../includes/mobile-engagement-enable-google-cloud-messaging.md)]

##<a name="sdk-integration"></a>SDK-integratie

### <a name="managing-device-registrations"></a>Registraties apparaat beheren

Elk apparaat moet een registratie-opdracht naar de Google-servers verzonden, anders is deze niet bereikbaar.

Een apparaat kan ook registratie voor GCM meldingen (het apparaat wordt automatisch geregistreerd als de toepassing wordt verwijderd).

Als u de [SDK van Google Play] niet gebruikt of u niet al de bedoeling registratie uzelf sturen, kunt u de betrokkenheid van het apparaat automatisch geregistreerd voor u maken.

Voeg de volgende aan om dit mogelijk uw `AndroidManifest.xml` bestand binnen de `<application/>` code:

            <!-- If only 1 sender, don't forget the \n, otherwise it will be parsed as a negative number... -->
            <meta-data android:name="engagement:gcm:sender" android:value="<Your Google Project Number>\n" />

### <a name="communicate-registration-id-to-the-engagement-push-service-and-receive-notifications"></a>Registratie-id om de betrokkenheid bij de Push-service communiceert en meldingen ontvangen

De registratie-id van het apparaat om de betrokkenheid bij de Push-service te communiceren en de meldingen ontvangen, voeg de volgende aan uw `AndroidManifest.xml` bestand binnen de `<application/>` label (zelfs als u registraties apparaat zelf beheren):

            <receiver android:name="com.microsoft.azure.engagement.gcm.EngagementGCMEnabler"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.intent.action.APPID_GOT" />
              </intent-filter>
            </receiver>

            <receiver android:name="com.microsoft.azure.engagement.gcm.EngagementGCMReceiver" android:permission="com.google.android.c2dm.permission.SEND">
              <intent-filter>
                <action android:name="com.google.android.c2dm.intent.REGISTRATION" />
                <action android:name="com.google.android.c2dm.intent.RECEIVE" />
                <category android:name="<your_package_name>" />
              </intent-filter>
            </receiver>

Zorg ervoor u hebt de volgende machtigingen uw `AndroidManifest.xml` (nadat de `</application>` code).

            <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />
            <uses-permission android:name="<your_package_name>.permission.C2D_MESSAGE" />
            <permission android:name="<your_package_name>.permission.C2D_MESSAGE" android:protectionLevel="signature" />

##<a name="grant-mobile-engagement-access-to-your-gcm-api-key"></a>Betrokkenheid bij de mobiele toegang verlenen tot uw GCM API-sleutel

Volg [deze handleiding](mobile-engagement-android-get-started.md#grant-mobile-engagement-access-to-your-gcm-api-key) Mobile Engagement om toegang te verlenen aan de GCM API-sleutel.

[Google Play SDK]:https://developers.google.com/cloud-messaging/android/start
