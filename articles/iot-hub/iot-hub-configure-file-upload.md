<properties
     pageTitle="De Azure portal gebruiken voor het uploaden van het bestand configureren | Microsoft Azure"
     description="Een overzicht van het uploaden van het bestand met de Azure portal configureren"
     services="iot-hub"
     documentationCenter=""
     authors="dominicbetts"
     manager="timlt"
     editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="na"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="09/30/2016"
     ms.author="dobett"/>

# <a name="configure-file-uploads-using-the-azure-portal"></a>Bestand uploaden met behulp van de portal Azure configureren

## <a name="file-upload"></a>Bestand uploaden

Gebruik het [bestand uploaden functionaliteit in IoT Hub][lnk-upload], moet u eerst een opslag Azure-account koppelen aan de hub. Selecteer de instellingen **bestand uploaden** voor het weergeven van eigenschappen van bestand laden voor de IoT-hub die wordt gewijzigd.

![][13]

**Opslag container**: de Azure portal gebruiken een blob-container in een account Azure opslag in uw huidige Azure abonnement koppelen aan uw IoT Hub selecteren. Indien nodig kunt u een account Azure opslag op de **opslag rekeningen** blade- en blob-container op de **Containers** blade. SAS-URI's IoT Hub automatisch gegenereerd met schrijfmachtigingen voor apparaten wilt gebruiken bij het uploaden van bestanden aan deze container blob.

![][14]

**Meldingen ontvangen voor geüploade bestanden**: in- of uitschakelen van meldingen van bestand uploaden via het in-of uitschakelen.

**SAS-TTL**: deze instelling wordt de time-to-live van het SAS-URI's naar het apparaat door de IoT Hub geretourneerd. Standaard ingesteld op één uur, maar kan worden aangepast om andere waarden met de schuifregelaar.

**Bestand kennisgeving instellingen standaard TTL-waarde**: de time-to-live van een bestand uploaden melding voordat deze is verlopen. Standaard ingesteld op één dag, maar kan worden aangepast om andere waarden met de schuifregelaar.

**Bestand melding levering maximum aantal**: het aantal keren dat de IoT-Hub probeert een bestand uploaden melding. Standaard ingesteld op 10, maar kan worden aangepast om andere waarden met de schuifregelaar.

![][15]

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de functies voor het uploaden van bestanden van IoT Hub [uploaden van bestanden vanaf een apparaat] [ lnk-upload] in de handleiding voor ontwikkelaars.

Klik op deze koppelingen voor meer informatie over het beheren van Azure IoT Hub:

- [Bulk IoT apparaten beheren][lnk-bulk]
- [Gebruik cijfers][lnk-metrics]
- [Activiteiten controleren][lnk-monitor]

Om de mogelijkheden van IoT Hub verder verkennen, Zie:

- [Handleiding voor ontwikkelaars][lnk-devguide]
- [Een apparaat met de SDK IoT Gateway simuleren][lnk-gateway]
- [Beveilig uw IoT-oplossing uit de grond omhoog][lnk-securing]


  [13]: ./media/iot-hub-configure-file-upload/file-upload-settings.png
  [14]: ./media/iot-hub-configure-file-upload/file-upload-container-selection.png
  [15]: ./media/iot-hub-configure-file-upload/file-upload-selected-container.png

[lnk-upload]: iot-hub-devguide-file-upload.md

[lnk-bulk]: iot-hub-bulk-identity-mgmt.md
[lnk-metrics]: iot-hub-metrics.md
[lnk-monitor]: iot-hub-operations-monitoring.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
[lnk-securing]: iot-hub-security-ground-up.md