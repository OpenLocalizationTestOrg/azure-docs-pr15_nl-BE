<properties
    pageTitle="Geavanceerde opties voor Azure Mobile Engagement Android SDK voor rapportage"
    description="Wordt beschreven hoe u geavanceerde rapportage voor het vastleggen van analytics voor Azure Mobile Engagement Android SDK"
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
    ms.date="08/10/2016"
    ms.author="piyushjo;ricksal" />

# <a name="advanced-reporting-with-engagement-on-android"></a>Geavanceerde rapportage met betrokkenheid van Android

> [AZURE.SELECTOR]
- [Universele Windows](mobile-engagement-windows-store-integrate-engagement.md)
- [Silverlight voor Windows Phone](mobile-engagement-windows-phone-integrate-engagement.md)
- [iOS](mobile-engagement-ios-integrate-engagement.md)
- [Android](mobile-engagement-android-advanced-reporting.md)

Dit onderwerp beschrijft de aanvullende rapportagevaluta's in uw toepassing voor Android. U kunt deze opties toepassen op de app gemaakt in de zelfstudie [Aan de slag](mobile-engagement-android-get-started.md) .

## <a name="prerequisites"></a>Vereisten

[AZURE.INCLUDE [Prereqs](../../includes/mobile-engagement-android-prereqs.md)]

De zelfstudie is u voltooid was opzettelijk direct en eenvoudig, maar er zijn geavanceerde opties die u kunt kiezen.

## <a name="modifying-your-activity-classes"></a>Aanpassing van de `Activity` klassen

In de [zelfstudie aan de slag](mobile-engagement-android-get-started.md)alle u moest doen was om de `*Activity` subklassen worden overgenomen van de corresponderende `Engagement*Activity` klassen. Bijvoorbeeld, als uw oude activiteiten uitgebreid `ListActivity`, brengt u het uitbreiden `EngagementListActivity`.

> [AZURE.IMPORTANT] Bij het gebruik van `EngagementListActivity` of `EngagementExpandableListActivity`, zorg ervoor dat alle aanroepen van `requestWindowFeature(...);` wordt uitgevoerd vóór de aanroep van `super.onCreate(...);`, anders een fout optreedt.

U vindt deze klassen in de `src` map en deze kopiëren naar uw project. De klassen zijn ook in het **JavaDoc**.

## <a name="alternate-method-call-startactivity-and-endactivity-manually"></a>Alternatieve methode: bel `startActivity()` en `endActivity()` handmatig

Als u niet kan of wil niet overbelasten uw `Activity` -klassen, in plaats daarvan starten en beëindigen van de activiteiten door het aanroepen van de `EngagementAgent`van methoden direct.

> [AZURE.IMPORTANT] De Android SDK nooit roept de `endActivity()` methode, zelfs wanneer de toepassing wordt gesloten (op Android, toepassingen zijn nooit gesloten). Het is dus *sterk* aanbevolen om aan te roepen de `startActivity()` methode in de `onResume` terugbellen van *alle* activiteiten, en de `endActivity()` methode in de `onPause()` terugbellen van *alle* uw activiteiten. Dit is de enige manier om ervoor te zorgen dat sessies niet worden gelekt. Als een sessie wordt vermist, verbreekt de betrokkenheid bij de service nooit u de betrokkenheid bij de back-end (omdat de service verbonden blijft als een sessie in behandeling is).

Hier volgt een voorbeeld:

    public class MyActivity extends Some3rdPartyActivity
    {
      @Override
      protected void onResume()
      {
        super.onResume();
        String activityNameOnEngagement = EngagementAgentUtils.buildEngagementActivityName(getClass()); // Uses short class name and removes "Activity" at the end.
        EngagementAgent.getInstance(this).startActivity(this, activityNameOnEngagement, null);
      }

      @Override
      protected void onPause()
      {
        super.onPause();
        EngagementAgent.getInstance(this).endActivity();
      }
    }

In dit voorbeeld is vergelijkbaar met de `EngagementActivity` klasse en haar varianten, waarvan de broncode is beschikbaar in de `src` map.

## <a name="using-applicationoncreate"></a>Met behulp van Application.onCreate()

Elke code die u plaatst in `Application.onCreate()` en in de andere toepassing retouraanroepen voor de processen van de toepassing, met inbegrip van de betrokkenheid bij de service wordt uitgevoerd. Deze wellicht ongewenste neveneffecten, zoals onnodige geheugentoewijzingen en threads in het proces van de overeenkomst, of dubbele broadcast receivers of services.

Als u overschrijven `Application.onCreate()`, wordt aangeraden het volgende stukje code toe te voegen aan het begin van de `Application.onCreate()` functie:

     public void onCreate()
     {
       if (EngagementAgentUtils.isInDedicatedEngagementProcess(this))
         return;

       ... Your code...
     }

U kunt hetzelfde doen voor `Application.onTerminate()`, `Application.onLowMemory()`, en `Application.onConfigurationChanged(...)`.

Kunt u uitbreiden `EngagementApplication` in plaats van het uitbreiden van `Application`: de callback `Application.onCreate()` voert het proces uit en roept `Application.onApplicationProcessCreate()` alleen als het huidige proces niet degene die de betrokkenheid bij de service, dezelfde regels voor de andere callbacks gelden.

## <a name="tags-in-the-androidmanifestxml-file"></a>Codes in het bestand AndroidManifest.xml

In de code van de service in het bestand AndroidManifest.xml de `android:label` kenmerk kunt u ervoor kiezen de naam van de betrokkenheid bij de service zoals deze wordt weergegeven voor eindgebruikers in het scherm 'Services uitgevoerd' van hun telefoon. Het is raadzaam dit kenmerk instellen op `"<Your application name>Service"` (bijvoorbeeld, `"AcmeFunGameService"`).

Geven de `android:process` kenmerk zorgt ervoor dat de betrokkenheid bij de service wordt uitgevoerd in een eigen proces (Engagement uitgevoerd in hetzelfde proces als uw toepassing kunt u uw hoofd/UI-thread mogelijk minder goed).

## <a name="building-with-proguard"></a>Gebouw met ProGuard

Als u uw toepassingspakket met ProGuard bouwt, moet u sommige klassen behouden. U kunt de volgende configuratie-fragment:

    -keep public class * extends android.os.IInterface
    -keep class com.microsoft.azure.engagement.reach.activity.EngagementWebAnnouncementActivity$EngagementReachContentJS {
    <methods>;
    }
