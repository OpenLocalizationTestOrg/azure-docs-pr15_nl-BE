<properties
    pageTitle="Windows Universal geavanceerde rapportage met betrokkenheid bij de MobileApps"
    description="Betrokkenheid bij de mobiele Azure integreren met Windows Universal Apps"                  
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows-store"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="08/12/2016"
    ms.author="piyushjo;ricksal" />

# <a name="advanced-reporting-with-the-windows-universal-apps-engagement-sdk"></a>Geavanceerde rapportage met de Universal Apps Engagement voor Windows SDK

> [AZURE.SELECTOR]
- [Universele Windows](mobile-engagement-windows-store-advanced-reporting.md)
- [Silverlight voor Windows Phone](mobile-engagement-windows-phone-integrate-engagement.md)
- [iOS](mobile-engagement-ios-integrate-engagement.md)
- [Android](mobile-engagement-android-advanced-reporting.md)

Dit onderwerp beschrijft de extra rapportagevaluta scenario's in uw Windows-universele toepassing. Deze scenario's bevatten opties die u kunt toepassen op de app gemaakt in de zelfstudie [Aan de slag](mobile-engagement-windows-store-dotnet-get-started.md) .

## <a name="prerequisites"></a>Vereisten

[AZURE.INCLUDE [Prereqs](../../includes/mobile-engagement-windows-store-prereqs.md)]

Voordat u deze zelfstudie begint, moet u eerst de zelfstudie [Aan de slag](mobile-engagement-windows-store-dotnet-get-started.md) , die opzettelijk direct en eenvoudig voltooien. Deze zelfstudie bevat extra opties die kunt u kiezen uit.

## <a name="specifying-engagement-configuration-at-runtime"></a>Betrokkenheid bij de configuratie tijdens runtime opgeven

De betrokkenheid bij de configuratie is gecentraliseerd het `Resources\EngagementConfiguration.xml` -bestand van het project, dat is waar deze is opgegeven in het onderwerp [Aan de slag](mobile-engagement-windows-store-dotnet-get-started.md) .

Maar u kunt ook opgeven dat in runtime: kunt u de volgende methode toepassen voordat de agent betrokkenheid bij de initialisatie aanroepen:

          /* Engagement configuration. */
          EngagementConfiguration engagementConfiguration = new EngagementConfiguration();

          /* Set the Engagement connection string. */
          engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";

          /* Initialize Engagement angent with above configuration. */
          EngagementAgent.Instance.Init(e, engagementConfiguration);



## <a name="recommended-method-overload-your-page-classes"></a>Aanbevolen methode: overbelasten uw `Page` klassen

U activeert de rapportage van de logboeken die zijn vereist voor betrokkenheid bij de gebruikers, sessies, activiteiten, Crashes en technische statistieken te berekenen, maken alle uw `Page` onderliggende klassen overnemen van de `EngagementPage` klassen.

Hier volgt een voorbeeld van een pagina van uw toepassing. U kunt hetzelfde doen voor alle pagina's van uw toepassing.

### <a name="c-source-file"></a>C#-bronbestand

Uw pagina wijzigen `.xaml.cs` bestand:

-   Toevoegen aan uw `using` instructies:

        using Microsoft.Azure.Engagement;

-   Vervangen `Page` met `EngagementPage`:

**Zonder afspraak:**

        namespace Example
        {
          public sealed partial class ExamplePage : Page
          {
            [...]
          }
        }

**Met afspraak:**

        using Microsoft.Azure.Engagement;

        namespace Example
        {
          public sealed partial class ExamplePage : EngagementPage
          {
            [...]
          }
        }

> [AZURE.IMPORTANT] Als uw pagina overschrijft de `OnNavigatedTo` -methode moet aanroepen `base.OnNavigatedTo(e)`. Anders wordt de activiteit is niet te worden gemeld (de `EngagementPage` oproepen `StartActivity` in de `OnNavigatedTo` methode).

### <a name="xaml-file"></a>XAML bestand

Uw pagina wijzigen `.xaml` bestand:

-   Voeg toe aan de declaraties van naamruimten:

        xmlns:engagement="using:Microsoft.Azure.Engagement"

-   Vervangen `Page` met `engagement:EngagementPage`:

**Zonder afspraak:**

        <Page>
            <!-- layout -->
            ...
        </Page>

**Met afspraak:**

        <engagement:EngagementPage
            xmlns:engagement="using:Microsoft.Azure.Engagement">
            <!-- layout -->
            ...
        </engagement:EngagementPage >

### <a name="override-the-default-behaviour"></a>Standaard overschrijven

Standaard wordt de naam van de klasse van de pagina als de naam van de activiteit met zonder extra vermeld. Als de klasse gebruikt het achtervoegsel "Pagina", betrokkenheid wordt verwijderd.

Het standaardgedrag voor de naam overschrijven, moet u deze code toevoegen:

        // in the .xaml.cs file
        protected override string GetEngagementPageName()
        {
          /* your code */
          return "new name";
        }

Extra informatie aan de activiteit, het toevoegen van deze code:

        // in the .xaml.cs file
        protected override Dictionary<object,object> GetEngagementPageExtra()
        {
          /* your code */
          return extra;
        }

Deze methoden worden aangeroepen vanuit de `OnNavigatedTo` methode van de pagina.

### <a name="alternate-method-call-startactivity-manually"></a>Alternatieve methode: bel `StartActivity()` handmatig

Als u niet kan of wil niet overbelasten uw `Page` -klassen, in plaats daarvan kunt u uw activiteiten starten door het aanroepen van `EngagementAgent` methoden direct.

Beste telefoonkaart `StartActivity` in de `OnNavigatedTo` methode van de pagina.

            protected override void OnNavigatedTo(NavigationEventArgs e)
            {
              base.OnNavigatedTo(e);
              EngagementAgent.Instance.StartActivity("MyPage");
            }

> [AZURE.IMPORTANT]  Zorg ervoor dat u uw sessie beëindigen correct.
>
> Universele Windows SDK automatisch roept de `EndActivity` methode wanneer de toepassing wordt gesloten. Het is dus **sterk** aanbevolen om aan te roepen de `StartActivity` methode wanneer de activiteit van de gebruiker wijzigen, en op **nooit** roept de `EndActivity` methode. Met deze methode wordt de betrokkenheid bij de server dat de huidige gebruiker de toepassing, die van invloed zijn alle toepassingslogboeken heeft verlaten.

## <a name="advanced-reporting"></a>Geavanceerde rapportage

Desgewenst kunt u rapporteren specifieke gebeurtenissen, fouten en taken te doen, gebruiken andere methoden worden gevonden in de `EngagementAgent` klasse. De betrokkenheid bij de API maakt het gebruik van geavanceerde mogelijkheden voor alle betrokkenheid.

Zie voor meer informatie, [het gebruik van geavanceerde mobiele aanstelling tagging API in de universele Windows app](mobile-engagement-windows-store-use-engagement-api.md).
