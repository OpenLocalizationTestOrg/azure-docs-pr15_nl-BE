<properties
 pageTitle="Get Azure extra (Ubuntu 16.04) | Microsoft Azure"
 description="Installeer Python en Azure opdrachtregel-Interface (CLI Azure) op Ubuntu."
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

# <a name="21-get-azure-tools-ubuntu-1604"></a>2.1 Azure krijgen extra (Ubuntu 16.04)

> [AZURE.SELECTOR]
- [Windows 7 +](iot-hub-raspberry-pi-kit-node-lesson2-get-azure-tools-win32.md)
- [Ubuntu-16.04](iot-hub-raspberry-pi-kit-node-lesson2-get-azure-tools-ubuntu.md)
- [macOS 10.10](iot-hub-raspberry-pi-kit-node-lesson2-get-azure-tools-mac.md)

## <a name="211-what-you-will-do"></a>2.1.1 wat u doet

Installeer de Azure opdrachtregelinterface (CLI Azure). Als u aan eventuele problemen, oplossingen in de [pagina voor probleemoplossing met](iot-hub-raspberry-pi-kit-node-troubleshooting.md)zoeken.

## <a name="212-what-you-will-learn"></a>2.1.2 Wat leert u

- Het installeren van Azure CLI.
- Het toevoegen van IoT subgroep van Azure CLI.

## <a name="213-what-you-need"></a>2.1.3 wat u nodig hebt

- Een Ubuntu computer met internetverbinding
- Een abonnement op Azure active. Als u geen account hebt, kunt u een [gratis account](https://azure.microsoft.com/free/) maken in een paar minuten.

## <a name="214-install-the-azure-cli"></a>2.1.4 de Azure CLI installeren

De CLI Azure biedt een opdrachtregel meerdere platforms ervaring voor Azure, waardoor u kunt rechtstreeks vanaf de opdrachtregel te verrichten werk en resources kunt beheren. 

Als u wilt de nieuwste Azure CLI installeren, volg deze stappen:

1. Voer de volgende opdrachten in een Terminal-venster. Het duurt vijf minuten Azure CLI installeren.

    ```bash
    sudo apt-get update
    sudo apt-get install -y libssl-dev libffi-dev
    sudo apt-get install -y python-dev
    sudo apt-get install -y build-essential
    sudo apt-get install -y python-pip
    sudo pip install azure-cli-core==0.1.0b7 azure-cli-vm==0.1.0b7 azure-cli-storage==0.1.0b7 azure-cli-role==0.1.0b7 azure-cli-resource==0.1.0b7 azure-cli-profile==0.1.0b7 azure-cli-network==0.1.0b7 azure-cli-iot==0.1.0b7 azure-cli-feedback==0.1.0b7 azure-cli-configure==0.1.0b7 azure-cli-component==0.1.0b7 azure-cli==0.1.0b7
    ```

2. Controleer de installatie door de volgende opdracht uit te voeren:

    ```bash
    az iot -h
    ```

Als de installatie voltooid is, moet u de volgende uitvoer zien.

![AZ iot -h](media/iot-hub-raspberry-pi-lessons/lesson2/az_iot_help_ubuntu.png)

## <a name="215-summary"></a>2.1.5 samenvatting

U kunt de CLI Azure hebt geïnstalleerd. Doorgaan naar de volgende sectie voor het maken van uw Azure IoT Hub- en apparaat-id met behulp van de CLI Azure.

## <a name="next-steps"></a>Volgende stappen

[2.2 de IoT hub maken en registreren van uw frambozen Pi 3](iot-hub-raspberry-pi-kit-node-lesson2-prepare-azure-iot-hub.md)
