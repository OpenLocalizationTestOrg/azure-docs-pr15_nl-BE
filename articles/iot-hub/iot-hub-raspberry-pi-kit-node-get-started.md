<properties
 pageTitle="Aan de slag met een Raspberry Pi 3 | Microsoft Azure"
 description="Aan de slag met frambozen Pi 3, maken uw Azure IoT Hub en sluit de Pi op de IoT-hub"
 services="iot-hub"
 documentationCenter=""
 authors="shizn"
 manager="timlt"
 tags=""
 keywords=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="10/21/2016"
 ms.author="xshi"/>

# <a name="get-started-with-raspberry-pi-3"></a>Aan de slag met een Raspberry Pi 3

U begint door te leren over het werken met frambozen Pi 3 die met Raspbian in deze zelfstudie. Vervolgens leert u hoe uw apparaten naadloos verbinding te maken met de cloud met [Azure IoT Hub](iot-hub-what-is-iot-hub.md). Ga naar [windowsondevices.com](http://www.windowsondevices.com/)voor Windows 10 IoT Core-monsters.

## <a name="lesson-1-configure-your-device"></a>Les 1: Het apparaat configureren

![Les 1 E2E Diagram](media/iot-hub-raspberry-pi-lessons/e2e-lesson1.png)

In deze les de frambozen Pi 3-apparaat configureren met een besturingssysteem, uw ontwikkelomgeving instellen en implementeren van een toepassing op de Pi.

### <a name="configure-your-device"></a>Uw apparaat configureren

De frambozen Pi 3 voor gebruik eerst configureren en installeren van het besturingssysteem Raspbian, een gratis besturingssysteem dat is geoptimaliseerd voor de hardware frambozen Pi.

*Geschatte tijd: 30 minuten* 

[Ga voor het configureren van het apparaat](iot-hub-raspberry-pi-kit-node-lesson1-configure-your-device.md)

### <a name="get-the-tools"></a>De hulpprogramma's ophalen
De hulpprogramma's en software kunnen bouwen en implementeren van uw eerste toepassing voor de Pi frambozen 3 downloaden.

*Geschatte tijd: 20 minuten* 

[Ga naar 'De hulpprogramma's ophalen'](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-win32.md)

### <a name="create-and-deploy-the-blink-application"></a>Maken en implementeren van de toepassing laten knipperen

De voorbeeldtoepassing Node.js van Github klonen en gulp voor de implementatie van deze toepassing op uw bord frambozen Pi 3. Deze voorbeeldtoepassing knippert de LED elke twee seconden op het moederbord aangesloten.

*Geschatte tijd: 5 minuten* 

[Ga naar ' maken en implementeren van de toepassing blink'](iot-hub-raspberry-pi-kit-node-lesson1-deploy-blink-app.md)

## <a name="lesson-2-create-your-iot-hub"></a>Les 2: Maak de IoT-hub

![Les 2 E2E Diagram](media/iot-hub-raspberry-pi-lessons/e2e-lesson2.png)

In deze les maken uw gratis account Azure, uw hub Azure IoT inrichten en het eerste apparaat in Azure IoT hub maken.

Les 1 voltooien voordat u deze les.

### <a name="get-the-azure-tools"></a>Azure hulpprogramma 's

Installeren van Azure opdrachtregelinterface (CLI Azure).

*Geschatte tijd in beslag: 10 minuten* 

[Ga naar 'extra Azure ophalen.](iot-hub-raspberry-pi-kit-node-lesson2-get-azure-tools-win32.md)

### <a name="create-your-iot-hub-and-register-your-raspberry-pi-3"></a>De hub IoT maken en registreren van uw frambozen Pi 3

Het eerste apparaat toevoegen aan de Azure IoT Hub met Azure CLI de bronnengroep maken en inrichten van uw eerste Azure IoT-hub. 

*Geschatte tijd in beslag: 10 minuten* 

[Ga naar "Uw hub IoT maken en registreren van uw Pi frambozen 3"](iot-hub-raspberry-pi-kit-node-lesson2-prepare-azure-iot-hub.md)


## <a name="lesson-3-send-device-to-cloud-messages"></a>Les 3: Apparaat-wolk-berichten verzenden

![Les 3 E2E Diagram](media/iot-hub-raspberry-pi-lessons/e2e-lesson3.png)

In deze les verzenden u berichten uit uw Pi naar uw hub IoT. U maakt ook een Azure functie app die binnenkomende berichten van uw hub-IoT opvangt en schrijft deze naar Azure tabelopslag.

Lessen 1 en 2 voltooien voordat u deze les.

### <a name="create-an-azure-function-app-and-azure-storage-account"></a>Maak een Azure functie app en Azure opslag account

Een sjabloon Azure Resource Manager gebruiken voor het maken van een app Azure functie en een account voor de opslag van Azure.

*Geschatte tijd in beslag: 10 minuten* 

[Ga naar 'Maak een Azure functie app en Azure opslag account'](iot-hub-raspberry-pi-kit-node-lesson3-deploy-resource-manager-template.md)

### <a name="run-sample-application-to-send-device-to-cloud-messages"></a>Uitvoeren van de voorbeeldtoepassing apparaat-wolk-berichten verzenden

Implementeren en uitvoeren van een voorbeeldtoepassing met uw apparaat frambozen Pi 3 waarmee berichten worden verzonden naar de IoT hub.

*Geschatte tijd in beslag: 10 minuten* 

[Ga naar uitvoeren van de voorbeeldtoepassing apparaat-wolk-berichten verzenden](iot-hub-raspberry-pi-kit-node-lesson3-run-azure-blink.md)

### <a name="read-messages-persisted-in-azure-storage"></a>Gelezen berichten bewaard in Azure opslag
Het apparaat naar cloud-berichten worden gecontroleerd als ze zijn geschreven naar de opslag van Azure.

*Geschatte tijd: 5 minuten* 

[Ga naar gelezen berichten bewaard in Azure opslag](iot-hub-raspberry-pi-kit-node-lesson3-read-table-storage.md)


## <a name="lesson-4-send-cloud-to-device-messages"></a>Les 4: Wolk naar apparaat verzenden

![Les 4 E2E Diagram](media/iot-hub-raspberry-pi-lessons/e2e-lesson4.png)

Het verzenden van berichten van uw hub Azure IoT aan uw Pi frambozen 3-demo's in deze les. De berichten bepalen de aan- en uit het gedrag van de LED die is verbonden met uw Pi. Een voorbeeldtoepassing is voorbereid voor u deze taak te verwezenlijken.

Volledige lessen 1, 2 en 3 voordat u begint met deze les.

### <a name="run-the-sample-application-to-receive-cloud-to-device-messages"></a>Uitvoeren van de voorbeeldtoepassing cloud-to-device-berichten ontvangen

De voorbeeldtoepassing in les 4 wordt uitgevoerd op de Pi en inkomende berichten van uw hub IoT. Een nieuwe taak voor de gulp verzendt berichten naar uw Pi van uw hub IoT LED knipperen.

*Geschatte tijd in beslag: 10 minuten* 

[Ga naar uitvoeren van de voorbeeldtoepassing cloud-to-device-berichten ontvangen](iot-hub-raspberry-pi-kit-node-lesson4-send-cloud-to-device-messages.md)

### <a name="optional-section-change-the-on-and-off-behavior-of-the-led"></a>Optionele sectie: de aan-en uit het gedrag van de LED

De berichten te wijzigen van de LED's in- en uitschakelen gedrag aanpassen.

*Geschatte tijd in beslag: 10 minuten* 

[Ga naar ' optionele sectie: de aan- en uit het gedrag van de LED wijzigen '](iot-hub-raspberry-pi-kit-node-lesson4-change-led-behavior.md)


## <a name="troubleshooting"></a>Het oplossen van problemen

Als u voldoet aan alle dreigen tijdens de lessen, kunt u proberen oplossingen in deze pagina.

[Ga naar 'Problemen oplossen'](iot-hub-raspberry-pi-kit-node-troubleshooting.md)
