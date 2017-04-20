<properties 
    pageTitle="Windows Phone Silverlight SDK-overzicht" 
    description="Overzicht van de Windows Phone Silverlight SDK voor betrokkenheid bij de mobiele Azure"                     
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

#<a name="windows-phone-silverlight-sdk-overview-for-azure-mobile-engagement"></a>Windows Phone Silverlight SDK-overzicht voor betrokkenheid bij de mobiele Azure

Start hier om de details over het integreren van Azure Mobile Engagement in een Windows Phone Silverlight App. Als u wilt eens proberen eerst, zorg ervoor dat voltooien u onze [zelfstudie 15 minuten](mobile-engagement-windows-phone-get-started.md).

Klik hier voor een overzicht van de [SDK Content](mobile-engagement-windows-phone-sdk-content.md)

##<a name="integration-procedures"></a>Integratie-procedures

1. Begin hier: [het integreren van Mobile Engagement in uw Silverlight voor Windows Phone-app](mobile-engagement-windows-phone-integrate-engagement.md)

2. Voor meldingen: [het integreren van Reach (meldingen) in uw Silverlight voor Windows Phone-app](mobile-engagement-windows-phone-integrate-engagement-reach.md)

3. Label plan uitvoering: [het gebruik van geavanceerde mobiele aanstelling tagging API in uw Silverlight voor Windows Phone-app](mobile-engagement-windows-phone-use-engagement-api.md)

##<a name="release-notes"></a>Release-opmerkingen

###<a name="330-04192016"></a>3.3.0 (19-04/2016)
Onderdeel van de *MicrosoftAzure.MobileEngagement* nuget **v3.4.0** pakket

-   Toegevoegde "TestLogLevel" API inschakelen/uitschakelen/filter console aanmelden uitgestoten door de SDK.

Zie de [volledige release-opmerkingen](mobile-engagement-windows-phone-release-notes.md) voor een eerdere versie

##<a name="upgrade-procedures"></a>Upgradeprocedures

Als u hebt al een oudere versie van onze SDK geïntegreerd in uw toepassing, moet u rekening houden met de volgende punten bij het upgraden van de SDK.

Mogelijk moet verschillende procedures volgen als u verschillende versies van de SDK hebt gemist. Zie de volledige [Procedures bijwerken](mobile-engagement-windows-phone-upgrade-procedure.md). Als u migreert van 0.10.1 naar 0.11.0 die u moet eerst volgt u de procedure "van 0.9.0 naar 0.10.1' vervolgens de procedure"van 0.10.1 naar 0.11.0' bijvoorbeeld.

###<a name="from-200-to-330"></a>Van 2.0.0 aan 3.3.0

####<a name="test-logs"></a>Testen van Logboeken

Console logs geproduceerd door de SDK kunnen nu worden ingeschakeld/uitgeschakeld/gefilterd. De eigenschap voor bijwerken als u wilt aanpassen, `EngagementAgent.Instance.TestLogEnabled` op een van de waarde van de `EngagementTestLogLevel` opsomming, bijvoorbeeld:

            EngagementAgent.Instance.TestLogLevel = EngagementTestLogLevel.Verbose;
            EngagementAgent.Instance.Init();

### <a name="upgrade-from-older-versions"></a>Upgraden van oudere versies

Zie [upgraden-Procedures](mobile-engagement-windows-phone-upgrade-procedure.md)
 
