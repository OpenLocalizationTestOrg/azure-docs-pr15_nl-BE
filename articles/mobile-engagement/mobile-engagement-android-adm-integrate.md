<properties
    pageTitle="Integratie van Azure betrokkenheid bij de mobiele Android SDK"
    description="Meest recente updates en procedures voor de Android SDK voor Azure Mobile Engagement"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="dwrede"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/19/2016"
    ms.author="piyushjo" />


#<a name="how-to-integrate-adm-with-engagement"></a>ADM integreren met Engagement

> [AZURE.IMPORTANT] U moet de integratie procedure in de betrokkenheid bij de integratie van het op Android document voordat u deze handleiding volgen.
>
> Dit document is alleen nuttig als u reeds de Reach-module en een plan om Amazon apparaten geïntegreerd. Reach campagnes in uw toepassing integreren, lees eerst integreren Engagement bereiken op Android.

##<a name="introduction"></a>Inleiding

Integratie van ADM, kan de toepassing moet worden geplaatst wanneer de doelcomputer Amazon Android apparaten.

ADM-nettoladingen altijd geduwd naar de SDK bevat de `azme` sleutel in het object. Dus als u in uw toepassing ADM voor andere doelen gebruikt, kunt u filteren op basis van die sleutel gereedschap duwt.

> [AZURE.IMPORTANT] Alleen Amazon Kindle apparaten met Android 4.0.3 of hoger worden ondersteund door Amazon apparaat verzenden van berichten. echter, kunt u deze code op andere apparaten veilig integreren.

##<a name="sign-up-to-adm"></a>Aanmelden voor ADM

Als u niet hebt gedaan, moet u de ADM op je Amazon account inschakelen.

De procedure wordt beschreven op: [<https://developer.amazon.com/sdk/adm/credentials.html>].

U krijgt bij de voltooiing van de procedure:

-   OAuth referenties voor (een Client-ID en een geheim van de Client) voor betrokkenheid van uw apparaten kan.
-   Een API-sleutel die moeten worden geïntegreerd in uw toepassing.

##<a name="sdk-integration"></a>SDK-integratie

### <a name="managing-device-registrations"></a>Registraties apparaat beheren

Elk apparaat moet een registratie-opdracht naar de ADM-servers verzonden, anders is deze niet bereikbaar.

Als u al de [clientbibliotheek ADM gebruikt]en [geïntegreerde ADM] al hebt, kunt u direct gaan android-sdk-adm-ontvangen.

Als u hebt geen ADM geïntegreerd, heeft de betrokkenheid van een eenvoudiger manier om in uw toepassing:

Bewerken uw `AndroidManifest.xml` bestand:

-   De naamruimte Amazon toevoegen het bestand moet als volgt beginnen:

        <?xml version="1.0" encoding="utf-8"?>
        <manifest xmlns:android="http://schemas.android.com/apk/res/android"
                  xmlns:amazon="http://schemas.amazon.com/apk/res/android"

-   In de `<application/>` code, het toevoegen van deze sectie:

        <amazon:enable-feature
           android:name="com.amazon.device.messaging"
           android:required="false"/>

        <meta-data android:name="engagement:adm:register" android:value="true" />

-   Na het toevoegen van de tag amazon, moet u wellicht een build-fout als het doel-Project bouwen onder Android 2.1. U moet een doel **Android 2.1 +** build gebruiken (geen paniek, u kunt nog steeds een `minSdkVersion` ingesteld op 4).
-   De ADM-API-sleutel als een actief integreren door de volgende [met deze procedure].

Volg de instructies van de volgende secties.

### <a name="communicate-registration-id-to-the-engagement-push-service-and-receive-notifications"></a>Registratie-id om de betrokkenheid bij de Push-service communiceert en meldingen ontvangen

De registratie-id van het apparaat om de betrokkenheid bij de Push-service te communiceren en de meldingen ontvangen, voeg de volgende aan uw `AndroidManifest.xml` bestand binnen de `<application/>` label (zelfs als u ADM zonder betrokkenheid):

        <receiver android:name="com.microsoft.azure.engagement.adm.EngagementADMEnabler"
          android:exported="false">
          <intent-filter>
            <action android:name="com.microsoft.azure.engagement.intent.action.APPID_GOT"/>
          </intent-filter>
        </receiver>

         <receiver android:name="com.microsoft.azure.engagement.adm.EngagementADMReceiver"
           android:permission="com.amazon.device.messaging.permission.SEND">
          <intent-filter>
            <action android:name="com.amazon.device.messaging.intent.REGISTRATION"/>
            <action android:name="com.amazon.device.messaging.intent.RECEIVE"/>
            <category android:name="<your_package_name>"/>
          </intent-filter>
        </receiver>   

Zorg ervoor u hebt de volgende machtigingen uw `AndroidManifest.xml` (vóór de `</application>` code).

        <uses-permission android:name="android.permission.WAKE_LOCK"/>
        <uses-permission android:name="com.amazon.device.messaging.permission.RECEIVE"/>
        <uses-permission android:name="<your_package_name>.permission.RECEIVE_ADM_MESSAGE"/>
        <permission android:name="<your_package_name>.permission.RECEIVE_ADM_MESSAGE" android:protectionLevel="signature"/>

##<a name="grant-engagement-oauth-credentials"></a>Subsidie Engagement OAuth-referenties

Dien uw referenties OAuth (Client-ID en het geheim van de Client) in betrokkenheid bij de Portal.

[< https://developer.amazon.com/sdk/adm/credentials.html>]:https://developer.amazon.com/sdk/adm/credentials.html
[ADM client library]:https://developer.amazon.com/sdk/adm/setup.html
[geïntegreerde ADM]:https://developer.amazon.com/sdk/adm/integrating-app.html
[deze procedure]:https://developer.amazon.com/sdk/adm/integrating-app.html#Asset
