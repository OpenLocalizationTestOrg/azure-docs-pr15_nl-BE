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


#<a name="upgrade-procedures"></a>Upgradeprocedures

Als u hebt al een oudere versie van onze SDK geïntegreerd in uw toepassing, moet u rekening houden met de volgende punten bij het upgraden van de SDK.

Mogelijk moet verschillende procedures volgen als u verschillende versies van de SDK hebt gemist. Als u migreert van 1.4.0 naar 1.6.0 die u moet eerst volgt u de procedure "van 1.4.0 naar 1.5.0' vervolgens de procedure"van 1.5.0 naar 1.6.0' bijvoorbeeld.

Ongeacht de versie die u bijwerkt, moet vervangen door de `mobile-engagement-VERSION.jar` door de nieuwe.

##<a name="from-420-to-421"></a>Van 4.2.0 naar 4.2.1

Deze stap kan daadwerkelijk worden uitgevoerd op een willekeurige versie van de SDK, is een verbetering van de beveiliging wanneer u bereiken activiteiten integreren.

U moet nu toevoegen `exported="false"` op alle activiteiten van het bereik.

Reach-activiteiten moeten er nu als volgt op de `AndroidManifest.xml`:

            <activity android:name="com.microsoft.azure.engagement.reach.activity.EngagementTextAnnouncementActivity" android:theme="@android:style/Theme.Light" android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
                <category android:name="android.intent.category.DEFAULT" />
                <data android:mimeType="text/plain" />
              </intent-filter>
            </activity>
            <activity android:name="com.microsoft.azure.engagement.reach.activity.EngagementWebAnnouncementActivity" android:theme="@android:style/Theme.Light" android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
                <category android:name="android.intent.category.DEFAULT" />
                <data android:mimeType="text/html" />
              </intent-filter>
            </activity>
            <activity android:name="com.microsoft.azure.engagement.reach.activity.EngagementPollActivity" android:theme="@android:style/Theme.Light" android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.POLL"/>
                <category android:name="android.intent.category.DEFAULT" />
              </intent-filter>
            </activity>
            <activity android:name="com.microsoft.azure.engagement.reach.activity.EngagementLoadingActivity" android:theme="@android:style/Theme.Dialog" android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.LOADING"/>
                <category android:name="android.intent.category.DEFAULT"/>
              </intent-filter>
            </activity>

##<a name="from-400-to-410"></a>Van 4.0.0 aan 4.1.0

De SDK nu greep nieuwe machtiging model van Android M.

Als u functies van de locatie of Lees [deze sectie](mobile-engagement-android-integrate-engagement.md#android-m-permissions)meldingen in grote lijnen.

Naast de nieuwe machtiging model ondersteunen we nu configureren locatie functies tijdens runtime.
Wij zijn nog steeds compatibel met de parameters manifest voor vestiging maar deze nu verouderd. Runtime-configuratie wordt gebruikt, verwijdert u de volgende secties uit de ``AndroidManifest.xml``:

    <meta-data
      android:name="engagement:locationReport:lazyArea"
      android:value="true"/>
    <meta-data
      android:name="engagement:locationReport:realTime"
      android:value="true"/>
    <meta-data
      android:name="engagement:locationReport:realTime:background"
      android:value="true"/>
    <meta-data
      android:name="engagement:locationReport:realTime:fine"
      android:value="true"/>

en Lees [Deze bijgewerkte procedure](mobile-engagement-android-integrate-engagement.md#location-reporting) runtime-configuratie in plaats daarvan wordt gebruikt.

##<a name="from-300-to-400"></a>Van 3.0.0 tot 4.0.0

### <a name="native-push"></a>Native push

Native push (GCM/ADM) wordt nu ook gebruikt voor in app-meldingen zodat moet u de referenties van de oorspronkelijke push voor elk type push campagne.

Als dit nog niet is volgt u [deze procedure](mobile-engagement-android-integrate-engagement-reach.md#native-push).

### <a name="androidmanifestxml"></a>AndroidManifest.xml

Integratie van reach is gewijzigd in ``AndroidManifest.xml``.

Vervang dit:

    <receiver
      android:name="com.microsoft.azure.engagement.reach.EngagementReachReceiver"
      android:exported="false">
      <intent-filter>
        <action android:name="android.intent.action.BOOT_COMPLETED"/>
        <action android:name="com.microsoft.azure.engagement.intent.action.AGENT_CREATED"/>
        <action android:name="com.microsoft.azure.engagement.intent.action.MESSAGE"/>
        <action android:name="com.microsoft.azure.engagement.reach.intent.action.ACTION_NOTIFICATION"/>
        <action android:name="com.microsoft.azure.engagement.reach.intent.action.EXIT_NOTIFICATION"/>
        <action android:name="android.intent.action.DOWNLOAD_COMPLETE"/>
        <action android:name="com.microsoft.azure.engagement.reach.intent.action.DOWNLOAD_TIMEOUT"/>
      </intent-filter>
    </receiver>

Door

    <receiver
      android:name="com.microsoft.azure.engagement.reach.EngagementReachReceiver"
      android:exported="false">
      <intent-filter>
        <action android:name="android.intent.action.BOOT_COMPLETED"/>
        <action android:name="com.microsoft.azure.engagement.intent.action.AGENT_CREATED"/>
        <action android:name="com.microsoft.azure.engagement.intent.action.MESSAGE"/>
        <action android:name="com.microsoft.azure.engagement.reach.intent.action.ACTION_NOTIFICATION"/>
        <action android:name="com.microsoft.azure.engagement.reach.intent.action.EXIT_NOTIFICATION"/>
        <action android:name="com.microsoft.azure.engagement.reach.intent.action.DOWNLOAD_TIMEOUT"/>
      </intent-filter>
    </receiver>
    <receiver android:name="com.microsoft.azure.engagement.reach.EngagementReachDownloadReceiver">
      <intent-filter>
        <action android:name="android.intent.action.DOWNLOAD_COMPLETE"/>
      </intent-filter>
    </receiver>

Er is mogelijk een scherm laden nu wanneer u op een aankondiging (met tekst/webinhoud) of een enquête.
U moet dit voor dergelijke campagnes te werken in 4.0.0 toevoegen:

    <activity
      android:name="com.microsoft.azure.engagement.reach.activity.EngagementLoadingActivity"
      android:theme="@android:style/Theme.Dialog">
      <intent-filter>
        <action android:name="com.microsoft.azure.engagement.reach.intent.action.LOADING"/>
        <category android:name="android.intent.category.DEFAULT"/>
      </intent-filter>
    </activity>

### <a name="resources"></a>Bronnen

Sluit de nieuwe `res/layout/engagement_loading.xml` bestand in uw project.

##<a name="from-240-to-300"></a>Van 2.4.0 naar 3.0.0

Hieronder wordt beschreven hoe u voor het migreren van een SDK-integratie van de Capptain-service aangeboden door Capptain SAS in een aangedreven door betrokkenheid bij de Azure Mobile app. Als u vanuit een eerdere versie migreert, neem contact op met de Capptain-website om te migreren naar 2.4.0 eerst en vervolgens de volgende procedure van toepassing.

>[AZURE.IMPORTANT] Capptain en Mobile Engagement zijn niet dezelfde diensten en de onderstaande procedure alleen markeert het migreren van de clienttoepassing. Migratie van de SDK in de app migreert geen gegevens van de servers Capptain naar de servers van Mobile Engagement.

### <a name="jar-file"></a>JAR-bestand

Vervangen `capptain.jar` door `mobile-engagement-VERSION.jar` in de `libs` map.

### <a name="resource-files"></a>Bronbestanden

Elk bronbestand die we geleverd (voorafgegaan door `capptain_`) te worden vervangen door nieuwe (voorafgegaan door `engagement_`).

Als u deze bestanden hebt aangepast, moet u opnieuw toepassen uw aanpassingen op de nieuwe bestanden, **de id's in de bronbestanden zijn ook hernoemd**.

### <a name="application-id"></a>Toepassings-ID

Betrokkenheid gebruikt nu een verbindingsreeks voor het configureren van de SDK-id's zoals de toepassings-id.

U moet gebruiken `EngagementAgent.init` methode in uw activiteit starten als volgt:

            EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
            engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
            EngagementAgent.getInstance(this).init(engagementConfiguration);

De verbindingsreeks voor de toepassing wordt weergegeven op de Portal Azure.

Verwijder elke aanroep `CapptainAgent.configure` als `EngagementAgent.init` wordt vervangen door deze methode.

De `appId` kan niet meer worden geconfigureerd met behulp van `AndroidManifest.xml`.

Verwijder dit gedeelte van de `AndroidManifest.xml` als u deze hebt:

            <meta-data android:name="capptain:appId" android:value="<YOUR_APPID>"/>

### <a name="java-api"></a>Java API

Elke aanroep van een Java-klasse van onze SDK heeft moet worden gewijzigd; bijvoorbeeld `CapptainAgent.getInstance(this)` naam moet worden gewijzigd `EngagementAgent.getInstance(this)`, `extends CapptainActivity` de naam moet worden gewijzigd `extends EngagementActivity` enz...

Als u zijn geïntegreerd met de standaard voorkeurbestanden voor agent, de standaardbestandsnaam is nu `engagement.agent` en de sleutel `engagement:agent`.

Wanneer u de webonderdelen aankondigingen, de binder Javascript is nu `engagementReachContent`.

### <a name="androidmanifestxml"></a>AndroidManifest.xml

Een groot aantal wijzigingen is er gebeurd, de service niet meer wordt gedeeld en een groot aantal ontvangers kunnen niet worden geëxporteerd niet meer.

De verklaring van de service is nu veel eenvoudiger; toevoegen en verwijderen de uitvoerintentie filter en alle metagegevens in het `exportable=false`.

Plus alles wat wordt gewijzigd voor het gebruik van betrokkenheid.

Nu als volgt uitziet:

            <service
              android:name="com.microsoft.azure.engagement.service.EngagementService"
              android:exported="false"
              android:label="<Your application name>Service"
              android:process=":Engagement"/>

Als u testen logboeken inschakelt wilt, wordt de meta-gegevens is nu verplaatst naar de code van de toepassing en heeft gekregen:

            <application>
            
              <meta-data android:name="engagement:log:test" android:value="true" />
            
              <service/>
            
            </application>

Alle andere meta-gegevens hebben zojuist gekregen, hier is de volledige lijst (met de naam van de cursus alleen de door u):

            <meta-data
              android:name="engagement:reportCrash"
              android:value="true"/>
            <meta-data
              android:name="engagement:sessionTimeout"
              android:value="10000"/>
            <meta-data
              android:name="engagement:burstThreshold"
              android:value="0"/>
            <meta-data
              android:name="engagement:connection:delay"
              android:value="0"/>
            <meta-data
              android:name="engagement:locationReport:lazyArea"
              android:value="false"/>
            <meta-data
              android:name="engagement:locationReport:realTime"
              android:value="false"/>
            <meta-data
              android:name="engagement:locationReport:realTime:background"
              android:value="false"/>
            <meta-data
              android:name="engagement:locationReport:realTime:fine"
              android:value="false"/>
            <meta-data
              android:name="engagement:agent:settings:name"
              android:value="engagement.agent"/>
            <meta-data
              android:name="engagement:agent:settings:mode"
              android:value="0"/>
            <meta-data
              android:name="engagement:gcm:sender"
              android:value="<YOUR_PROJECT_NUMBER>\n"/>
            <meta-data
              android:name="engagement:adm:register"
              android:value="true"/>
            <meta-data
              android:name="engagement:reach:notification:icon"
              android:value="<DRAWABLE_NAME_WITHOUT_EXTENSION>"/>
            
            <activity android:name="SomeActivityWithoutReachOverlay">
              <meta-data
                android:name="engagement:notification:overlay"
                android:value="false"/>
            </activity>

Google Play en bijhouden van SmartAd is verwijderd uit de SDK die u zojuist hebt moet u deze verwijderen zonder vervanging:

            <meta-data 
                android:name="capptain:track:installReferrerForwardList"
                android:value="com.class1,com.class2"/>
            <meta-data
                android:name="capptain:track:adservers"
                android:value="smartad" />

De Reach-activiteiten worden nu als volgt gedefinieerd:

            <activity
              android:name="com.microsoft.azure.engagement.reach.activity.EngagementTextAnnouncementActivity"
              android:theme="@android:style/Theme.Light">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
                <category android:name="android.intent.category.DEFAULT"/>
                <data android:mimeType="text/plain"/>
              </intent-filter>
            </activity>
            <activity
              android:name="com.microsoft.azure.engagement.reach.activity.EngagementWebAnnouncementActivity"
              android:theme="@android:style/Theme.Light">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
                <category android:name="android.intent.category.DEFAULT"/>
                <data android:mimeType="text/html"/>
              </intent-filter>
            </activity>
            <activity
              android:name="com.microsoft.azure.engagement.reach.activity.EngagementPollActivity"
              android:theme="@android:style/Theme.Light">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.POLL"/>
                <category android:name="android.intent.category.DEFAULT"/>
              </intent-filter>
            </activity>
            
Als u aangepaste bereiken activiteiten hebt, hoeft u alleen te wijzigen zodat deze overeenkomt met een uitvoerintentie acties `com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT` of `com.microsoft.azure.engagement.reach.intent.action.POLL`.

De ontvangers van broadcast zijn hernoemd, plus we nu toevoegen `exported=false`. Hier is de volledige lijst van de ontvangers die met de nieuwe specificatie (van cursus rename alleen die u gebruikt):

            <receiver android:name="com.microsoft.azure.engagement.reach.EngagementReachReceiver"
              android:exported="false">
              <intent-filter>
                <action android:name="android.intent.action.BOOT_COMPLETED"/>
                <action android:name="com.microsoft.azure.engagement.intent.action.AGENT_CREATED"/>
                <action android:name="com.microsoft.azure.engagement.intent.action.MESSAGE"/>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ACTION_NOTIFICATION"/>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.EXIT_NOTIFICATION"/>
                <action android:name="android.intent.action.DOWNLOAD_COMPLETE"/>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.DOWNLOAD_TIMEOUT"/>
              </intent-filter>
            </receiver>
            
            <receiver android:name="com.microsoft.azure.engagement.gcm.EngagementGCMEnabler"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.intent.action.APPID_GOT" />
              </intent-filter>
            </receiver>
            
            <receiver
              android:name="com.microsoft.azure.engagement.gcm.EngagementGCMReceiver"
              android:permission="com.google.android.c2dm.permission.SEND">
              <intent-filter>
                <action android:name="com.google.android.c2dm.intent.REGISTRATION"/>
                <action android:name="com.google.android.c2dm.intent.RECEIVE"/>
                <category android:name="<your_package_name>"/>
              </intent-filter>
            </receiver>
            
            <receiver android:name="com.microsoft.azure.engagement.adm.EngagementADMEnabler"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.intent.action.APPID_GOT"/>
              </intent-filter>
            </receiver>
            
            <receiver
              android:name="com.microsoft.azure.engagement.adm.EngagementADMReceiver"
              android:permission="com.amazon.device.messaging.permission.SEND">
              <intent-filter>
                <action android:name="com.amazon.device.messaging.intent.REGISTRATION"/>
                <action android:name="com.amazon.device.messaging.intent.RECEIVE"/>
                <category android:name="<your_package_name>"/>
              </intent-filter>
            </receiver>
            
            <receiver android:name="<your_sub_class_of_com.microsoft.azure.engagement.reach.EngagementReachDataPushReceiver>"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.DATA_PUSH" />
              </intent-filter>
            </receiver>
            
            <receiver android:name="com.microsoft.azure.engagement.EngagementLocationBootReceiver"
               android:exported="false">
               <intent-filter>
                  <action android:name="android.intent.action.BOOT_COMPLETED" />
               </intent-filter>
            </receiver>
            
            <receiver android:name="<your_sub_class_of_com.microsoft.azure.engagement.EngagementConnectionReceiver.java>"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.intent.action.CONNECTED"/>
                <action android:name="com.microsoft.azure.engagement.intent.action.DISCONNECTED"/>
              </intent-filter>
            </receiver>
            
            <receiver
              android:name="<your_sub_class_of_com.microsoft.azure.engagement.EngagementMessageReceiver.java>"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.MESSAGE"/>
              </intent-filter>
            </receiver>

Bijhouden van de ontvanger is verwijderd, zodat u dit gedeelte verwijderen:

          <receiver android:name="com.ubikod.capptain.android.sdk.track.CapptainTrackReceiver">
            <intent-filter>
              <action android:name="com.ubikod.capptain.intent.action.APPID_GOT" />
              <!-- possibly <action android:name="com.android.vending.INSTALL_REFERRER" /> -->
            </intent-filter>
          </receiver>

Merk op dat de verklaring van de implementatie van de **EngagementMessageReceiver** -broadcast ontvanger is gewijzigd in de `AndroidManifest.xml`. Dit is omdat de API voor het verzenden en willekeurige XMPP-berichten van willekeurige XMPP entiteiten verwijderen en de API voor het verzenden en ontvangen van berichten tussen apparaten hebt verwijderd. Dus, hebt u ook de volgende callbacks van uw implementatie **EngagementMessageReceiver** verwijderen:

            protected void onDeviceMessageReceived(android.content.Context context, java.lang.String deviceId, java.lang.String payload)

en

            protected void onXMPPMessageReceived(android.content.Context context, android.os.Bundle message)

Verwijder elke aanroep van **EngagementAgent** voor:

            sendMessageToDevice(java.lang.String deviceId, java.lang.String payload, java.lang.String packageName)

en

            sendXMPPMessage(android.os.Bundle msg)

### <a name="proguard"></a>Proguard

Proguard configuratie kan worden getroffen door de rebranding, de regels zijn nu ziet eruit als:

            -dontwarn android.**
            -keep class android.support.v4.** { *; }
            
            -keep public class * extends android.os.IInterface
            -keep class com.microsoft.azure.engagement.reach.activity.EngagementWebAnnouncementActivity$EngagementReachContentJS {
              <methods>;
            }
 
