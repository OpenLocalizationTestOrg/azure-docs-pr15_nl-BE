<properties
    pageTitle="Geavanceerde configuratie voor betrokkenheid bij de Universal Apps Windows SDK"
    description="Geavanceerde configuratieopties voor betrokkenheid bij de mobiele met universele Apps Windows Azure"                    
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
    ms.date="10/04/2016"
    ms.author="piyushjo;ricksal" />

# <a name="advanced-configuration-for-windows-universal-apps-engagement-sdk"></a>Geavanceerde configuratie voor betrokkenheid bij de Universal Apps Windows SDK

> [AZURE.SELECTOR]
- [Universele Windows](mobile-engagement-windows-store-advanced-configuration.md)
- [Silverlight voor Windows Phone](mobile-engagement-windows-phone-integrate-engagement.md)
- [iOS](mobile-engagement-ios-integrate-engagement.md)
- [Android](mobile-engagement-android-advanced-configuration.md)

Deze procedure wordt beschreven hoe verschillende configuratieopties voor Azure Mobile Engagement Android apps configureren.

## <a name="prerequisites"></a>Vereisten

[AZURE.INCLUDE [Prereqs](../../includes/mobile-engagement-windows-store-prereqs.md)]

##<a name="advanced-configuration"></a>Geavanceerde configuratie

### <a name="disable-automatic-crash-reporting"></a>Uitschakelen automatische foutenrapportage

U kunt de automatische crash rapportage functie van betrokkenheid uitschakelen. Vervolgens, wanneer een niet-verwerkte uitzondering optreedt, betrokkenheid, gebeurt er niets.

> [AZURE.WARNING] Als u deze functie uitschakelt, vervolgens verzendt als een niet-afgehandelde crash in uw app optreedt, Engagement geen sluit het vastlopen **en** niet de sessie en taken.

Schakel automatische foutenrapportage aanpassen de configuratie van de manier waarop die u deze hebt gedeclareerd:

#### <a name="from-engagementconfigurationxml-file"></a>Van `EngagementConfiguration.xml` bestand

Rapport crash ingesteld op `false` tussen `<reportCrash>` en `</reportCrash>` tags.

#### <a name="from-engagementconfiguration-object-at-run-time"></a>Van `EngagementConfiguration` object tijdens runtime

Rapport crash ingesteld op false, het EngagementConfiguration-object gebruiken.

        /* Engagement configuration. */
        EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
        engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";

        /* Disable Engagement crash reporting. */
        engagementConfiguration.Agent.ReportCrash = false;

### <a name="disable-real-time-reporting"></a>Real-time melden uitschakelen

Standaard registreert de betrokkenheid bij de servicerapporten in realtime. Als de toepassing Logboeken regelmatig wordt gemeld, is het beter in de buffer van de logboeken en in één keer verslag op regelmatige basis. Dit heet 'burst-modus'.

Bel hiervoor de methode:

        EngagementAgent.Instance.SetBurstThreshold(int everyMs);

Het argument is een waarde in **milliseconden**. Wanneer u opnieuw activeren, de real-time logboekregistratie wilt, roept u de methode geen parameters opgeeft, of met de waarde 0.

Burst-modus iets verhoogt de levensduur van de batterij, maar heeft dit gevolgen voor de Monitor Engagement: duur van alle sessies en projecten worden afgerond op de drempel van burst (dus sessies en taken die korter zijn dan de drempelwaarde burst is mogelijk niet zichtbaar). Wij raden u aan met behulp van een package-burst drempelwaarde niet meer dan 30000 (30s). Opgeslagen logboeken zijn beperkt tot 300 artikelen. Als het verzenden te lang is, kunt u sommige logboeken kunt verliezen.

> [AZURE.WARNING] De burst-drempel kan niet worden geconfigureerd voor een periode van minder dan een seconde. Als u dit doet, worden de SDK traceren met de fout wordt weergegeven en automatisch weer ingesteld op de standaardwaarde nul seconden. Hierdoor wordt de SDK om te rapporteren in de logboekbestanden in real-time.

[here]:http://www.nuget.org/packages/Capptain.WindowsCS
[NuGet website]:http://docs.nuget.org/docs/start-here/overview
