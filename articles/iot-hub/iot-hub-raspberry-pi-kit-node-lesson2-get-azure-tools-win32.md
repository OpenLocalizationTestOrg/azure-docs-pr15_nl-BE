<properties
 pageTitle="Azure extra ophalen (Windows 7 +) | Microsoft Azure"
 description="Python en Azure opdrachtregelinterface (CLI Azure) installeren op Windows 7 en hoger."
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

# <a name="21-get-azure-tools-windows-7-"></a>2.1 Azure extra ophalen (Windows 7 +)

> [AZURE.SELECTOR]
- [Windows 7 +](iot-hub-raspberry-pi-kit-node-lesson2-get-azure-tools-win32.md)
- [Ubuntu-16.04](iot-hub-raspberry-pi-kit-node-lesson2-get-azure-tools-ubuntu.md)
- [macOS 10.10](iot-hub-raspberry-pi-kit-node-lesson2-get-azure-tools-mac.md)

## <a name="211-what-you-will-do"></a>2.1.1 wat u doet

Installeer Python en het opdrachtregelprogramma Azure Interface (CLI Azure). Als u aan eventuele problemen, oplossingen in de [pagina voor probleemoplossing met](iot-hub-raspberry-pi-kit-node-troubleshooting.md)zoeken.

## <a name="212-what-you-will-learn"></a>2.1.2 Wat leert u

- Het installeren van Python.
- Het installeren van Azure CLI.

## <a name="213-what-you-need"></a>2.1.3 wat u nodig hebt

- Een Windows-computer met Internet-verbinding
- Een abonnement op Azure active. Als u geen account hebt, kunt u een [gratis account](https://azure.microsoft.com/free/) maken in een paar minuten.

## <a name="214-install-python"></a>2.1.4 Python installeren

Python installeren op uw Windows-computer. U kunt de Python 2.7, 3.4 of 3.5 installeren. Deze zelfstudie is gebaseerd op Python 2.7. Als u Python al hebt geïnstalleerd, gaat u naar punt 2.1.5.

[Python voor Windows ophalen](https://www.python.org/downloads/)

Ook moet u het pad van de mappen waarin de python.exe en pip.exe zijn geïnstalleerd op het systeem toevoegen `PATH` omgevingsvariabele. Python.exe worden standaard geïnstalleerd `C:\Python27` en pip.exe is geïnstalleerd in `C:\Python27\Scripts`.

## <a name="215-install-the-azure-cli"></a>2.1.5 de Azure CLI installeren

De CLI Azure biedt een opdrachtregel meerdere platforms ervaring voor Azure, waardoor u kunt rechtstreeks vanaf de opdrachtregel te verrichten werk en resources kunt beheren.

Azure CLI installeren, volg deze stappen:

1. Open een opdrachtvenster als beheerder.
2. Voer de volgende opdrachten:

    ```bash
    pip install azure-cli-core==0.1.0b7 azure-cli-vm==0.1.0b7 azure-cli-storage==0.1.0b7 azure-cli-role==0.1.0b7 azure-cli-resource==0.1.0b7 azure-cli-profile==0.1.0b7 azure-cli-network==0.1.0b7 azure-cli-iot==0.1.0b7 azure-cli-feedback==0.1.0b7 azure-cli-configure==0.1.0b7 azure-cli-component==0.1.0b7 azure-cli==0.1.0b7
    ```
3. Controleer de installatie door de volgende opdracht uit te voeren:

    ```bash
    az iot -h
    ```

Als de installatie voltooid is, verschijnt de volgende uitvoer.

![AZ iot -h](media/iot-hub-raspberry-pi-lessons/lesson2/az_iot_help_win.png)

## <a name="216-summary"></a>2.1.6-overzicht

U kunt de CLI Azure hebt geïnstalleerd. Doorgaan naar de volgende sectie voor het maken van uw Azure IoT Hub- en apparaat-id met behulp van de CLI Azure.

## <a name="next-steps"></a>Volgende stappen

[2.2 de IoT hub maken en registreren van uw frambozen Pi 3](iot-hub-raspberry-pi-kit-node-lesson2-prepare-azure-iot-hub.md)
