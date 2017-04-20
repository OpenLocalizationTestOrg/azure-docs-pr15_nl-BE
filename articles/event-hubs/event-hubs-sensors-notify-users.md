<properties 
   pageTitle="Waarschuw gebruikers gegevens ontvangen van sensoren of andere systemen | Microsoft Azure"
   description="Beschrijving van gebeurtenis Hubs aan gebruikers van de sensorgegevens."
   services="event-hubs"
   documentationCenter="na"
   authors="spyrossak"
   manager="timlt"
   editor="" />
<tags 
   ms.service="event-hubs"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/25/2016"
   ms.author="spyros;sethm" />

# <a name="notify-users-of-data-received-from-sensors-or-other-systems"></a>Waarschuw gebruikers gegevens ontvangen van sensoren of van andere systemen

Stel dat u een toepassing hebt die gegevens in real-time monitoren of genereert rapporten op een schema. Als u naar de website waarop die real time grafieken of rapporten worden weergegeven kijkt, ziet u iets dat actie vereist. Wat gebeurt er als u moet gewaarschuwde tot die gevallen, in plaats van bij het onthouden van de website controleren? Stel dat er een lichte groei in een uitstoot van broeikasgassen en u onmiddellijk weten moet als het lampje gaat uit. Een manier om dit zou zijn met een lichtsensor in de uitstoot van broeikasgassen, schikken voor een e-mailbericht worden verzonden als het licht uitgeschakeld is.

![][1]

Stel dat u een huisdier instapweigering faciliteit uitvoert en u moet worden gewaarschuwd bij lage voorraadniveaus voor levering in een ander scenario. U kunt bijvoorbeeld rangschikken verzonden SMS-bericht van uw ERP-systeem als uw magazijn voorraad van hondenvoer is gedaald tot een kritiek niveau. 

![][2]

Het probleem is hoe u essentiële informatie als bepaalde voorwaarden is voldaan, niet wanneer u rond op een statisch rapport uitchecken. Als u een [Azure gebeurtenis Hub][] of [Azure IoT Hub][] gebruikt om gegevens te ontvangen van apparaten of bedrijfstoepassingen zoals [Dynamics AX][], hebt u verschillende opties voor het verwerken van deze. Kunt u deze weergeven op een website, kunt analyseren, kunnen worden opgeslagen en u kunt ze gebruiken voor het starten van opdrachten om iets te doen. Hiervoor kunt u krachtige hulpmiddelen zoals [Azure Websites][], [SQL Azure][], [HDInsight][], [Cortana Intelligence Suite][], [Suite IoT][], [Logica Apps][]of [Azure melding Hubs][]. Maar soms kunt u die gegevens verzenden naar iemand met een minimum aan overhead. Als u wilt zien hoe u dit doen met slechts een beetje van code, hebt we een nieuw monster [AppToNotifyUsers][]verstrekt. Opties zijn e-mail (SMTP), SMS en telefoon.

## <a name="application-structure"></a>Toepassingsstructuur

De toepassing is geschreven in C# en het Leesmij-bestand in de steekproef bevat alle informatie die u wilt wijzigen, samenstellen en publiceren van de toepassing. De volgende secties vindt u een overzicht van de werking van de toepassing.

We beginnen met de veronderstelling dat er kritieke gebeurtenissen naar een Azure gebeurtenis Hub of IoT Hub wordt geduwd. Een hub zal doen als u hier toegang toe hebben en de verbindingsreeks.

Als u nog geen gebeurtenis Hub of IoT-hub, kunt u eenvoudig een test bed met een Arduino-schild en een frambozen Pi, volgens de instructies in het project [De puntjes verbinding](https://github.com/Azure/connectthedots) instellen. De sensor voor het op de Arduino schild verzendt de lichte niveaus via de Pi op een [Hub voor Azure-gebeurtenis][] (**ehdevices**) en waarschuwingen een taak [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/) duwt met een tweede gebeurtenis hub (**ehalerts**) als de lichte niveaus ontvangen onder een bepaald niveau dalen.

Wanneer **AppToNotify** wordt gestart, leest het een configuratiebestand (App.config) voor de URL en de referenties voor de gebeurtenis Hub de meldingen ontvangen. Het Hiermee vervolgens een proces om voortdurend bewaken die gebeurtenis Hub voor alle berichten die via – zo u hebt toegang tot de URL voor de gebeurtenis Hub of IoT hub en geldige referenties, deze gebeurtenis Hubs lezer code zal voortdurend lezen what's coming in. Tijdens het opstarten leest de toepassing ook de URL en de referenties voor de messaging service (e-mail, SMS, telefoon) die u wilt gebruiken en de naam of het adres van de afzender en een lijst met geadresseerden.

Zodra de monitor gebeurtenis Hub een bericht detecteert, wordt er een proces dat dit bericht met behulp van de methode die is opgegeven in het configuratiebestand. Overigens wordt elk bericht dat wordt gedetecteerd. Als u instelt dat de monitor aan te wijzen op een gebeurtenis Hub die tien berichten per seconde ontvangt, tien berichten per seconde – tien e-mails per seconde, 10 SMS-berichten per seconde, tien telefoongesprekken per seconde wordt verzonden door de afzender. Daarom moet een gebeurtenis Hub die alleen de waarschuwingen ontvangt die moeten worden verzonden, niet een Hub gebeurtenis die de onbewerkte gegevens van de sensoren of toepassingen ontvangt te controleren.

## <a name="applicability"></a>Toepasselijkheid

De code in dit voorbeeld toont alleen gebeurtenis Hubs controleren en messaging-services bellen in het geval dat u wilt deze functionaliteit toevoegen aan uw toepassing. Houd er rekening mee dat deze oplossing een DIY, alleen voorbeeld ontwikkelaars zijn gericht is. Deze behandeld onderneming eisen niet zoals redundantie, failover-opnieuw opstarten na een storing, enz. Zie de volgende onderwerpen voor meer uitgebreide en productie-oplossingen:

- Met behulp van verbindingslijnen of push-meldingen die via de service [Azure logica Apps](../app-service-logic/app-service-logic-connectors-list.md) .
- Met behulp van [Azure melding Hubs](https://msdn.microsoft.com/library/azure/jj927170.aspx), afwijkt van de blog [uitzending push-meldingen voor miljoenen mobiele apparaten met behulp van Azure melding Hubs](http://weblogs.asp.net/scottgu/broadcast-push-notifications-to-millions-of-mobile-devices-using-windows-azure-notification-hubs). 

## <a name="next-steps"></a>Volgende stappen

Is eenvoudig te maken van een eenvoudige service waarmee e-mailberichten of berichten naar geadresseerden verzendt of aanroepen, ontvangen door een gebeurtenis Hub of IoT Hub relay-gegevens. Ga naar [AppToNotifyUsers][]voor de implementatie van de oplossing om te waarschuwen van gebruikers op basis van gegevens die zijn ontvangen door deze hubs.

Raadpleeg de volgende artikelen voor meer informatie over deze hubs:

- [Azure gebeurtenis Hubs]
- [Azure IoT Hub]
- Aan de slag met een [gebeurtenis Hubs zelfstudie].
- Een volledig [voorbeeldtoepassing die gebruikmaakt van gebeurtenis Hubs].

Voor de implementatie van de oplossing om te waarschuwen van gebruikers op basis van gegevens die door deze hubs zijn ontvangen, gaat u naar:

- [AppToNotifyUsers][]

[Gebeurtenis Hubs zelfstudie]: event-hubs-csharp-ephcs-getstarted.md
[Azure IoT Hub]: https://azure.microsoft.com/services/iot-hub/
[Azure gebeurtenis Hubs]: https://azure.microsoft.com/services/event-hubs/
[Azure gebeurtenis Hub]: https://azure.microsoft.com/services/event-hubs/
[de voorbeeldtoepassing met gebeurtenis-Hubs]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[AppToNotifyUsers]: https://github.com/Azure-Samples/event-hubs-dotnet-user-notifications
[Dynamics AX]: http://www.microsoft.com/dynamics/erp-ax-overview.aspx
[Azure Websites]: https://azure.microsoft.com/services/app-service/web/
[SQL Azure]: https://azure.microsoft.com/services/sql-database/
[HDInsight]: https://azure.microsoft.com/services/hdinsight/
[Cortana Intelligence Suite]: http://www.microsoft.com/server-cloud/cortana-analytics-suite/Overview.aspx?WT.srch=1&WT.mc_ID=SEM_lLFwOJm3&bknode=BlueKai
[IoT Suite]: https://azure.microsoft.com/solutions/iot-suite/
[Logica Apps]: https://azure.microsoft.com/services/app-service/logic/
[Azure melding Hubs]: https://azure.microsoft.com/services/notification-hubs/
[Azure Stream Analytics]: https://azure.microsoft.com/services/stream-analytics/
 
[1]: ./media/event-hubs-sensors-notify-users/event-hubs-sensor-alert.png
[2]: ./media/event-hubs-sensors-notify-users/event-hubs-erp-alert.png