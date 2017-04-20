<properties 
   pageTitle="Azure mobiele betrokkenheid bij het oplossen van hulplijnen" 
   description="Problemen oplossen met Azure mobiele Engagement" 
   services="mobile-engagement" 
   documentationCenter="" 
   authors="piyushjo" 
   manager="dwrede" 
   editor=""/>

<tags
   ms.service="mobile-engagement"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="mobile-multiple"
   ms.workload="mobile" 
   ms.date="08/19/2016"
   ms.author="piyushjo"/>

# <a name="azure-mobile-engagement---troubleshooting-guide"></a>Azure mobiele Engagement - Troubleshooting Guide

## <a name="introduction"></a>Inleiding
De volgende gids voor probleemoplossing kunt u begrijpen hoofdoorzaken van enkele regelmatig terugkomt en will kunnen u zelf oplossen. 

## <a name="general"></a>Algemeen

In het algemeen, zorg er altijd voor het volgende:

1. Zorg ervoor dat u alle stappen die nodig zijn voor integratie zoals beschreven in onze [zelfstudie aan de slag](mobile-engagement-windows-store-dotnet-get-started.md) hebben doorlopen
2. U gebruikt de meest recente versie van het platform SDK's. 
3. Testen op een apparaat en een emulator omdat enkele problemen specifiek voor alleen de emulator zijn. 
4. U zijn niet kunt u door alle grenzen/throttles van Mobile Engagement die zijn gedocumenteerd [hier](../azure-subscription-service-limits.md)
5. Als u geen verbinding kunnen maken met de betrokkenheid van de mobiele service end of zien niet te laden gegevens voortdurend vervolgens zorgen ervoor dat er geen incidenten continue service door te controleren [hier](https://azure.microsoft.com/status/)

## <a name="monitor-issues"></a>Problemen met 'Monitor'

### <a name="i-am-not-seeing-my-device-showing-up-on-the-monitor-tab"></a>Ik zie niet mijn apparaat weergegeven op het tabblad Monitor
Tabblad monitor ziet u de apparaten die zijn aangesloten op uw betrokkenheid bij de mobiele platform in real-time. Als u fouten in een emulator en het apparaat opsporen wilt, moet u ten minste één sessie hier zien. Als de toepassing is gedistribueerd, ziet u de actieve sessies meter weerspiegelen de apparaten die zijn aangesloten op het platform in real-time. 

Als u het apparaat niet op het tabblad Monitor ziet is het waarschijnlijk een probleem SDK integratie. Dit zijn enkele algemene stappen beschreven voor het oplossen van problemen met:

1. Zorg ervoor dat u de juiste tekenreeks in de mobiele app en het afkomstig van de SDK toetsen en niet met de sectie sleutels API is. Uw mobiele app in verbindt de verbindingsreeks met het exemplaar van de betrokkenheid bij de mobiele app waarin u het apparaat op het tabblad Monitor ziet. 
2. Voor Windows-platform - als uw pagina overschrijft de `OnNavigatedTo` methode, zorg ervoor dat oproepen `base.OnNavigatedTo(e)`.
3. Als u Mobile Engagement in een bestaande mobiele app integreert en u er ook voor zorgen kunt dat u niet alle stappen gemist door te kijken naar de integratie van geavanceerde stappen [hier](mobile-engagement-windows-store-integrate-engagement.md)
4. Zorg ervoor dat u ten minste één schermactiviteiten verzendt door het overschrijven van de pagina met EngagementActivity afhankelijk van het platform dat u werkt, zoals beschreven in de [zelfstudie aan de slag](mobile-engagement-windows-store-dotnet-get-started.md).

### <a name="i-am-seeing-the-monitor-tab-showing-a-session-even-when-i-have-disconnected-or-closed-my-app-emulator"></a>Ik zie het tabblad Monitor met een sessie zelfs als ik hebben verbroken of mijn app gesloten / emulator. 
Als u de enige is die op dit moment verbonden aan het platform en u met behulp van een emulator de app open is dit waarschijnlijk door emulator quirks. In het algemeen, moet u ervoor zorgen dat u terugkomen naar het beginscherm van de emulator voor de app-sessie is verbroken. Ook op Windows-platform wellicht wanneer foutopsporing bij Visual Studio om ervoor te zorgen dat in Visual Studio u Ga naar de menubalk van de **Levenscyclus van gebeurtenissen** en klik op **stand-by** echt de sessie te sluiten. Zie [Windows-handleiding](mobile-engagement-windows-store-dotnet-get-started.md) voor meer informatie. 

## <a name="analytics-issues"></a>Problemen met 'Analytics'

### <a name="i-am-not-seeing-any-data-refreshed-data-on-analytics-tab"></a>Ik zie niet alle gegevens / gegevens op het tabblad Analytics vernieuwd 
Analytics-gegevens regelmatig opnieuw berekend en het kan maximaal 24 uur duren voordat deze vernieuwen. Deze gegevens zijn niet realtime en u ziet dat het in deze 24-uurs periode vernieuwd.
Zorg wel dat u ten minste één scherm of activiteit op de backend platform met een doorslaggevende ten minste één pagina met verzendt `EngagementActivity` of `SendActivity` explcitly. 

### <a name="i-am-seeing-incorrectly-captured-datetime-for-a-device-on-the-analytics-tab"></a>Ik zie niet goed vastgelegde datum en tijd voor een apparaat op het tabblad Analytics
De periode voor Analytics is gebaseerd op de datum van de instellingen voor de gebruikers. Dus zorg ervoor dat het apparaat de datum juist is ingesteld. 

## <a name="segment-issues"></a>Problemen met "Segment"

### <a name="i-created-a-segment-and-it-is-showing-up-as-greyed-out-or-not-showing-any-data"></a>Ik heb een segment hebt gemaakt en wordt weergegeven als een grijs of niet alle gegevens worden weergegeven
Segment maken niet op het moment dat real-time. Wordt berekend op hetzelfde moment als de analytics-gegevens worden samengevoegd en kan het maximaal 24 uur duren. Controleer terug later maar ondertussen moet u ook ervoor zorgen dat uw mobiele apps inderdaad verzendt de gegevens op basis waarvan u de segmenten die zijn vormen. Bv. Als een gebeurtenis zegt "foo" is niet verzonden door een mobiel apparaat wordt er niet zou een gesegmenteerde gegevens voor een segment gemaakt met gebeurtenisnaam = foo als criterium. Controleer ook de integratie van de SDK om ervoor te zorgen uw mobiele app verzendt de gegevens correct. 

## <a name="reach-or-push-notifications-issues"></a>Problemen met 'Bereiken' of Push-meldingen

### <a name="my-push-messages-are-not-being-delivered"></a>Mijn push-berichten worden niet bezorgd. 

1. Probeer het verzenden van meldingen naar een testapparaat eerst om ervoor te zorgen dat alle onderdelen - mobiele app, SDK en de service zijn goed verbonden en kunnen leveren van push-meldingen. 
2. De eenvoudigste out-app melding altijd eerst via een campagne die is niet gepland verzenden en noch heeft een publiek-criterium opgegeven. Dit is opnieuw aan te tonen dat de melding verbinding goed werkt. 
3. Als u problemen hebt bij het bezorgen van berichten-app ook is een goede eerste stap om te proberen het eerst verzenden van een melding van een out-app. 
4. Zorg ervoor dat de 'Native Push' correct is geconfigureerd voor uw mobiele app. Afhankelijk van het platform omvatten dat ofwel sleutels (Android, Windows) of certificaten (iOS). Zie [gebruikersinterface - instellingen](mobile-engagement-user-interface-settings.md)
5. Bij app-meldingen kunnen ook worden geblokkeerd door de gebruiker via het mobiele besturingssysteem dus ervoor te zorgen dat dit niet het geval is. 
6. Zorg ervoor dat u niet de *doelgroep negeren push zal worden verzonden naar gebruikers via de API* -optie in het gedeelte van de **campagne** van een campagne bereiken instelt omdat hierdoor worden push-meldingen kunnen alleen worden verzonden via de API's. 
7. Zorg ervoor u uw campagne push test met zowel een apparaat aangesloten via Wi-Fi en telefoon operator netwerk verbinding met het als een mogelijke bron van problemen te elimineren.
8. Zorgen ervoor dat het systeem datum/tijd in de apparaatemulator juist omdat elk apparaat synchroon ook verstoren Push-melding van de Service kunnen leveren van meldingen. 

Meer platform specifieke problemen met de onderstaande instructies:

1. **iOS** 

    - Zorg ervoor dat de certificaten geldig en niet-verlopen voor iOS Push-meldingen zijn. 
    - Zorg ervoor dat u juist *een productiecertificaat* in uw betrokkenheid bij de mobiele app configureert. 
    - Zorg ervoor dat u wilt testen op een *apparaat echte.* De iOS simulator verwerken niet push berichten.
    - Zorg ervoor dat de bundel-id correct is geconfigureerd in de mobiele app. Raadpleeg de instructies [hier](https://developer.apple.com/library/prerelease/ios/documentation/IDEs/Conceptual/AppDistributionGuide/AddingCapabilities/AddingCapabilities.html#//apple_ref/doc/uid/TP40012582-CH26-SW6)
    - Bij het testen, moet u "Ad Hoc" verdeling in uw mobiele provisioning profiel gebruiken. Worden niet geïnformeerd als uw app is gecompileerd met "Debug"

2. **Android**

    - Zorg ervoor dat u het juiste Project-nummer in uw mobiele app AndroidManifest.xml-bestand dat wordt gevolgd door het teken \n hebt opgegeven. 
    
            <meta-data android:name="engagement:gcm:sender" android:value="************\n" />
        
    - Ervoor te zorgen dat u niet ontbreekt of verkeerd geconfigureerd machtigingen in het bestand Manifest voor Android 
    - Zorg ervoor dat het Project dat u aan uw app client toevoegt van dezelfde account waar u de sleutel GCM-Server hebt gekregen. Alle niet overeen met de twee voorkomt u dat uw gereedschap duwt gaan. 
    - Als u systeemmeldingen, maar niet in app en vervolgens controleren op [een pictogram voor meldingen opgeven](mobile-engagement-android-get-started.md) als waarschijnlijk ontvangt geeft u niet het juiste pictogram in het bestand Manifest voor Android. 
    - Als u een BigPicture-bericht verzendt en ervoor te zorgen dat, als er externe afbeeldingsservers dan ze nodig hebben om te kunnen HTTP-ondersteuning 'GET' en 'Hoofd'.

3. **Windows**
    
    - Zorg ervoor dat u de toepassing hebt gekoppeld aan een geldige Windows Store-app. U hebt in Visual Studio - Klik met de rechtermuisknop op het project en selecteer de optie 'App met Store koppelen' en selecteer de toepassing die u hebt gemaakt in de Windows Store. Deze Windows Store app moet hetzelfde vanaf waar u de referenties van de oorspronkelijke push configureren in de portal Mobile Engagement kreeg.
    - Als u out-app push-meldingen, maar geen app-meldingen met ontvangt `EngagementOverlay` integratie vervolgens zorgen er is een basiselement van het raster op de pagina. EngagementOverlay maakt gebruik van de eerste "Raster"-element wordt gevonden in de xaml bestand twee webonderdelen toevoegen op de pagina bekijkt. Als u zoeken waarin webweergaven wordt ingesteld wilt, kunt u een raster met de naam "EngagementGrid" zoals dit echter hebt zodat er voldoende hoogte en breedte voor de twee volgende webpagina bekijkt die de melding en de volgende aankondiging als-app bericht wordt weergegeven:
        
            <Grid x:Name="EngagementGrid"></Grid>

### <a name="i-created-a-push-notificationannouncement-campaign-and-even-after-it-sent-me-the-notification-it-is-showing-as-active-what-does-it-mean"></a>Ik heb gemaakt met een push-bericht/mededeling/campagne en zelfs na het me de kennisgeving verzonden wordt weergegeven als 'Actief'. Wat betekent het? 
De **campagne** die u hebt gemaakt in Mobile Engagement heet zo omdat het een langdurige push notification betekenis als nieuwe apparaten verbinding met uw betrokkenheid bij de mobiele platform maken, ze automatisch wordt verzonden de melding die u hier configureert, mits zij voldoen aan de criteria die u in de campagne instelt. Dit is niet een afgeschoten één melding setup. U moet handmatig klikt u op de knop **Voltooien** om de campagne te beëindigen zodat het geen verdere meldingen verzenden. 

### <a name="i-created-a-push-campaign-and-i-am-receiving-notifications-successfully-however-whenever-i-open-up-the-app-i-get-the-same-notification-even-when-i-had-actioned-it-before"></a>Ik heb een push campagne gemaakt en ik ontvang meldingen is echter wanneer ik de app opent, krijg ik de melding dezelfde zelfs als ik had mailbericht het eerder? 
Dit is waarschijnlijk gebeuren tijdens de test en als u emulators of enkele test framework als TestFlight. Wat er gebeurt is hier op elke app exemplaar uitvoeren, is het aanschaffen van een nieuwe DeviceID en deze te verzenden naar onze back-end die wordt veroorzaakt door het platform Mobile Engagement om het te behandelen als een nieuw apparaat en het verzenden van de kennisgeving. 

## <a name="getting-support"></a>Ondersteuning van

Als u niet het probleem op te lossen kunt u vervolgens een zelf:

1. Het probleem met de bestaande threads op [MSDN forum](https://social.msdn.microsoft.com/Forums/windows/en-US/home?forum=azuremobileengagement) en StackOverflow forum zoeken en als niet dan er een vraag stellen. 
2. Als u een functie vervolgens toevoegen/stemrecht voor de aanvraag op onze [forum UserVoice](https://feedback.azure.com/forums/285737-mobile-engagement/) ontbreekt
3. Als u ondersteuning voor Microsoft Open een incident voor ondersteuning door middel van de volgende gegevens: 
    - Azure abonnements-ID
    - Platform (bijvoorbeeld iOS, Android, etc)
    - App-ID
    - Campagne-ID (voor problemen met push notification)
    - Apparaat-ID
    - Mobile Engagement SDK-versie (bijvoorbeeld Android SDK v2.1.0)
    - Foutgegevens met exacte foutbericht en scenario
