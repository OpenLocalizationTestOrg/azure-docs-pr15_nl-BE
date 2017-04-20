<properties
    pageTitle="Integratie van Windows Universal SDK"
    description="Universele integratie van Windows voor de SDK voor betrokkenheid bij de mobiele Azure"                                     
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="dwrede"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows-store"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="08/12/2016"
    ms.author="piyushjo;ricksal" />

#<a name="windows-universal-sdk-integration-for-azure-mobile-engagement"></a>Integratie van Windows Universal SDK voor betrokkenheid bij de mobiele Azure

Dit document beschrijft alle integratie en configuratie opties beschikbaar voor de Azure Mobile Engagement universele SDK van Windows.

## <a name="prerequisites"></a>Vereisten

Voordat u deze zelfstudie begint, moet u eerst onze [zelfstudie 15 minuten](mobile-engagement-windows-store-dotnet-get-started.md)te voltooien.

## <a name="advanced-features"></a>Geavanceerde functies

### <a name="reporting-features"></a>Rapportagefuncties
U kunt deze functies toevoegen:

1. [Geavanceerde opties voor rapportage](mobile-engagement-windows-store-advanced-reporting.md)

2. [Geavanceerde configuratieopties](mobile-engagement-windows-store-advanced-configuration.md)

### <a name="notifications"></a>Meldingen

[Het integreren van Reach (meldingen) in uw Windows Universal app](mobile-engagement-windows-store-integrate-engagement-reach.md)

### <a name="tag-plan-implementation"></a>Label plan-implementatie:

[Het gebruik van geavanceerde mobiele aanstelling API tags in uw Windows Universal app](mobile-engagement-windows-store-use-engagement-api.md)

##<a name="release-notes"></a>Release-opmerkingen

###<a name="340-04192016"></a>3.4.0 (19-04/2016)

-   Overlay verbeteringen bereiken.
-   Toegevoegde "TestLogLevel" API inschakelen/uitschakelen/filter console aanmelden uitgestoten door de SDK.
-   Vaste activiteit in meldingen voor de eerste activiteit die niet wordt weergegeven op de App starten.

Zie de [volledige release-opmerkingen](mobile-engagement-windows-store-release-notes.md) voor eerdere versies

##<a name="upgrade-procedures"></a>Upgradeprocedures

Als u hebt een oudere versie van de overeenkomst al geïntegreerd in uw toepassing, moet u rekening houden met de volgende punten bij het upgraden van de SDK.

Als u verschillende versies van de SDK hebt gemist, kunt u wellicht verschillende procedures volgen. Zie de volledige [Procedures bijwerken](mobile-engagement-windows-store-upgrade-procedure.md). Als u migreert van 0.10.1 naar 0.11.0 die u moet eerst volgt u de procedure "van 0.9.0 naar 0.10.1' vervolgens de procedure"van 0.10.1 naar 0.11.0' bijvoorbeeld.

###<a name="from-330-to-340"></a>Van 3.3.0 naar 3.4.0

####<a name="test-logs"></a>Testen van Logboeken

Console logs geproduceerd door de SDK kunnen nu worden ingeschakeld/uitgeschakeld/gefilterd. Bijwerken om aan te passen, de eigenschap `EngagementAgent.Instance.TestLogEnabled` op een van de waarden van de `EngagementTestLogLevel` (opsomming), bijvoorbeeld:

            EngagementAgent.Instance.TestLogLevel = EngagementTestLogLevel.Verbose;
            EngagementAgent.Instance.Init();

####<a name="resources"></a>Bronnen

De Reach-overlay is verbeterd. Het uitmaakt deel van de SDK NuGet package bronnen.

U kunt kiezen of u uw bestaande bestanden uit de map bedekking van uw resources wilt of niet behouden tijdens de upgrade naar de nieuwe versie van de SDK:

* Als de vorige overlay voor u werkt of u integreert de `WebView` elementen handmatig, vervolgens kunt u besluiten te houden met het afsluiten van bestanden, het werkt nog steeds.
* Om te werken aan de nieuwe overlay, vervangen door de gehele `overlay` map van uw resources door de nieuwe uit de SDK-pakket (UWP apps: na de upgrade, kunt u de nieuwe overlay map ophalen van % USERPROFILE %\\.nuget\packages\MicrosoftAzure.MobileEngagement\3.4.0\content\win81\Resources).

> [AZURE.WARNING] Met behulp van de nieuwe overlay overschrijft eventuele aanpassingen die zijn aangebracht in de vorige versie.

### <a name="upgrade-from-older-versions"></a>Upgraden van oudere versies

Zie [upgraden-Procedures](mobile-engagement-windows-store-upgrade-procedure.md)
