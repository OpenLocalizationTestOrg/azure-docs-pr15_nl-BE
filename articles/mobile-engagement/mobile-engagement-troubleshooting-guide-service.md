<properties 
   pageTitle="Azure mobiele betrokkenheid bij de Troubleshooting Guide - Service" 
   description="Troubleshooting Guide voor betrokkenheid bij de mobiele Azure" 
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

# <a name="troubleshooting-guide-for-service-issues"></a>Problemen oplossen met problemen met Services

Hieronder vindt u mogelijke problemen optreden bij hoe Azure Mobile Engagement wordt uitgevoerd.

## <a name="service-outages"></a>Storingen

### <a name="issue"></a>Probleem
- Problemen die worden veroorzaakt door storingen van Azure Mobile Engagement wordt weergegeven.

### <a name="causes"></a>Oorzaken
- Problemen die worden veroorzaakt door storingen van Azure Mobile Engagement kunnen worden veroorzaakt door verschillende andere problemen:
    - Geïsoleerde problemen die oorspronkelijk worden weergegeven op alle Azure Mobile Engagement systemische
    - Bekende problemen veroorzaakt door serverstoringen (niet altijd ziet in de status van de server):
    - Vertragingen, doelitems fouten, problemen met update Badge, statistieken stop verzamelt, Push loopt de planning worden API's stop werken, nieuwe toepassingen of gebruikers niet gemaakt, er DNS-fouten, en time-outfouten in de gebruikersinterface, API of Apps op een apparaat.
    - Wolk afhankelijkheid storingen [Azure servicestatus](http://status.azure.com/)
    - Push Notification Services (PNS) afhankelijkheid storingen
    - App Store storingen

1) U kunt dezelfde functie uit een andere testen om te testen of het probleem is systemische
   
   - Azure betrokkenheid bij de geïntegreerde mobiele toepassing
   - Testapparaat
   - Apparaat OS-testversie
   - Campagne
   - Beheerdersaccount
   - Browser (IE, Firefox, chroom, enz.)
   - Computer

2) Om te testen of het probleem heeft alleen invloed op de gebruikersinterface of de API's:

   - Dezelfde functie uit de gebruikersinterface Azure Mobile Engagement en de Azure Mobile Engagement API testen.

3) Als het probleem met uw mobiele telefoonnetwerk is testen:

   - Wanneer u verbonden bent met het Internet via Wi-Fi en terwijl u verbinding hebt via uw mobiele telefoon met 3G netwerk testen.
   - Zorg ervoor dat uw firewall niet wordt geblokkeerd door een van de IP-adressen van Azure Mobile Engagement of poorten.

4) Om te testen of het probleem met uw apparaat is:

   - Als het apparaat de verbinding kunnen maken met Azure Mobile Engagement met een andere Azure Mobile Engagement geïntegreerde toepassing testen.
   - Test of u gebeurtenissen, taken en Crashes kunt genereren uit uw telefoon die u in de gebruikersinterface van Azure Mobile Engagement weergeven kunt. 
   - Testen of u push-meldingen uit de gebruikersinterface Azure Mobile Engagement verzenden naar uw apparaat op basis van de apparaat-ID kunt. 

5) Als het probleem met uw App is testen:

   - Installeren en testen van de toepassing van een emulator in plaats van uit een fysiek apparaat:
   
6) Om te testen of het probleem bij upgrades voor het besturingssysteem voor de eindgebruiker apparaten waarvoor een upgrade SDK op te lossen:

   - Test uw toepassing op verschillende apparaten met verschillende versies van het besturingssysteem.
   - Ervoor zorgen dat u de meest recente versie van de SDK.
 
## <a name="connectivity-and-incorrect-information-issues"></a>Connectiviteit en onjuiste informatie over problemen

### <a name="issue"></a>Probleem
- Problemen bij het aanmelden bij de UI Azure Mobile Engagement.
- Fouten met de Azure Mobile Engagement API.
- Problemen met het uploaden van App Info codes via de API van het apparaat.
- Problemen met Logboeken of geëxporteerde gegevens downloaden vanaf Azure Mobile Engagement.
- Onjuiste informatie weergegeven in de gebruikersinterface van Azure Mobile Engagement.
- Onjuiste informatie weergegeven in Logboeken Azure Mobile Engagement.

### <a name="causes"></a>Oorzaken
* Controleer dat uw gebruikersaccount heeft onvoldoende machtigingen om de taak te verrichten.
* Controleer het probleem is niet beperkt tot één computer of het lokale netwerk.
* Zorg ervoor dat dat de betrokkenheid van Azure Mobile-service geen heeft storingen gemeld.
* Bevestig uw App Info Tag bestanden als volgt al deze regels:
    - Gebruik alleen de UTF8-tekenset (de ANSI-tekenset wordt niet ondersteund).
    - Gebruik een komma "," als scheidingsteken (u kunt een service om aan te vragen het scheidingsteken CSV wijzigen van een door komma's openen "," een ander teken, zoals een puntkomma ';').
    - Alle kleine letters gebruiken voor Booleaanse waarden 'true' en 'false'.
    - Een bestand gebruiken dat kleiner is dan de maximale bestandsgrootte van 35MB.
 
