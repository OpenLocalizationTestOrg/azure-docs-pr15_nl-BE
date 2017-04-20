<properties 
    pageTitle="Windows Phone Silverlight SDK upgradeprocedures" 
    description="Windows Phone Silverlight SDK upgradeprocedures voor betrokkenheid bij de mobiele Azure"                  
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="dwrede"
    editor="" />

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-windows-phone" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/19/2016" 
    ms.author="piyushjo" />

#<a name="windows-phone-silverlight-sdk-upgrade-procedures"></a>Windows Phone Silverlight SDK upgradeprocedures

Als u hebt al een oudere versie van onze SDK geïntegreerd in uw toepassing, moet u rekening houden met de volgende punten bij het upgraden van de SDK.

Mogelijk moet verschillende procedures volgen als u verschillende versies van de SDK hebt gemist. Als u migreert van 0.10.1 naar 0.11.0 die u moet eerst volgt u de procedure "van 0.9.0 naar 0.10.1' vervolgens de procedure"van 0.10.1 naar 0.11.0' bijvoorbeeld.

##<a name="from-200-to-330"></a>Van 2.0.0 aan 3.3.0

### <a name="test-logs"></a>Testen van Logboeken

Console logs geproduceerd door de SDK kunnen nu worden ingeschakeld/uitgeschakeld/gefilterd. De eigenschap voor bijwerken als u wilt aanpassen, `EngagementAgent.Instance.TestLogEnabled` op een van de waarde van de `EngagementTestLogLevel` opsomming, bijvoorbeeld:

            EngagementAgent.Instance.TestLogLevel = EngagementTestLogLevel.Verbose;
            EngagementAgent.Instance.Init();

##<a name="from-111-to-200"></a>Van 1.1.1 tot 2.0.0

Hieronder wordt beschreven hoe u voor het migreren van een SDK-integratie van de Capptain-service aangeboden door Capptain SAS in een aangedreven door betrokkenheid bij de Azure Mobile app. 

> [Azure.IMPORTANT] Capptain en Mobile Engagement zijn niet dezelfde diensten en de onderstaande procedure alleen markeert het migreren van de clienttoepassing. Migreren van de SDK in de app wordt niet migreren van uw gegevens de Capptain naar de servers van Mobile Engagement

Als u een migratie vanuit een eerdere versie, neem contact op met de Capptain-website om te migreren naar 1.1.1 eerst en vervolgens de volgende procedure van toepassing

### <a name="nuget-package"></a>Nuget package

**Capptain.WindowsPhone** vervangen door **MicrosoftAzure.MobileEngagement** Nuget package.

### <a name="applying-mobile-engagement"></a>Betrokkenheid bij de mobiele toepassing

De SDK wordt de term `Engagement`. U moet bijwerken van uw project zodat deze overeenkomt met deze wijziging.

U moet uw huidige Capptain nuget pakket verwijderen. U kunt alle wijzigingen in de map Capptain bronnen worden verwijderd. Als u wilt behouden die bestanden vervolgens een kopie maken van hen.

Daarna het nieuwe Microsoft Azure Engagement nuget pakket te installeren op uw project. U vindt het direct op [Nuget](http://www.nuget.org/packages/MicrosoftAzure.MobileEngagement). Deze actie vervangt alle bronnen bestanden gebruikt door betrokkenheid en wordt de nieuwe betrokkenheid van DLL-bestand toegevoegd aan de projectverwijzingen.

U hebt de projectverwijzingen opschonen door het verwijderen van verwijzingen naar DLL Capptain. Als u dat niet doet, wordt de versie van Capptain in conflict en fouten gebeurt.

Als u Capptain resources hebt aangepast, de inhoud van uw oude bestanden kopiëren en plakken in de nieuwe overeenkomst. Houd er rekening mee dat zowel cs als xaml-bestanden moeten worden bijgewerkt.

Wanneer deze stappen zijn voltooid hebt u alleen oude Capptain verwijzingen door de nieuwe betrokkenheid verwijzingen moeten worden vervangen.

1. Alle Capptain naamruimten moeten worden bijgewerkt.

    Voor de migratie:
    
        using Capptain.Agent;
        using Capptain.Reach;
    
    Na de migratie:
    
        using Microsoft.Azure.Engagement;

2. Alle klassen van Capptain met 'Capptain' moeten 'Engagement' bevatten.

    Voor de migratie:
    
        public sealed partial class MainPage : CapptainPage
        {
          protected override string GetCapptainPageName()
          {
            return "Capptain Demo";
          }
          ...
        }
    
    Na de migratie:
    
        public sealed partial class MainPage : EngagementPage
        {
          protected override string GetEngagementPageName()
          {
            return "Engagement Demo";
          }
          ...
        }

3. Xaml-bestanden wijzigen Capptain naamruimte en kenmerken ook.

    Voor de migratie:
    
        <capptain:CapptainPage
        ...
        xmlns:capptain="clr-namespace:Capptain.Agent;assembly=Capptain.Agent.WP"
        ...
        </capptain:CapptainPage>
    
    Na de migratie:
    
        <engagement:EngagementPage
        ...
        xmlns:engagement="clr-namespace:Microsoft.Azure.Engagement;assembly=Microsoft.Azure.Engagement.EngagementAgent.WP"
        ...
        </engagement:EngagementPage>

4. Voor andere bronnen zoals Capptain, afbeeldingen, houd er rekening mee dat zij ook hebben gekregen voor het gebruik van 'Engagement'.

### <a name="application-id--sdk-key"></a>Toepassings-ID / sleutel SDK

Betrokkenheid van een verbindingsreeks gebruikt. U hoeft niet een toepassings-ID en een sleutel SDK opgeven bij Mobile Engagement, hoeft u alleen een verbindingsreeks opgeven. U kunt instellen op het bestand EngagementConfiguration.

De betrokkenheid bij de configuratie kan worden ingesteld in de `Resources\EngagementConfiguration.xml` -bestand van uw project.

Bewerk dit bestand op te geven:

-   De verbindingsreeks van toepassing tussen de tags `<connectionString>` en `<\connectionString>`.

Als u wilt dat in plaats daarvan tijdens de uitvoering te geven, kunt u de volgende methode toepassen voordat de agent betrokkenheid bij de initialisatie bellen:

        /* Engagement configuration. */
        EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
        engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";
        
        /* Initialize Engagement angent with above configuration. */
        EngagementAgent.Instance.Init(engagementConfiguration);

De verbindingsreeks voor de toepassing wordt weergegeven in de klassieke Azure-Portal.

### <a name="items-name-change"></a>Naam wijzigen van items

Alle items met de naam *capptain* zijn *engagement*genoemd. Op dezelfde manier voor *Capptain* aan *betrokkenheid*.

Voorbeelden van veelgebruikte Capptain items:

-   CapptainConfiguration nu met de naam EngagementConfiguration
-   CapptainAgent nu met de naam EngagementAgent
-   CapptainReach nu met de naam EngagementReach
-   CapptainHttpConfig nu met de naam EngagementHttpConfig
-   GetCapptainPageName nu met de naam GetEngagementPageName

Opmerking deze naam ook overschreven methoden betreft.



 
