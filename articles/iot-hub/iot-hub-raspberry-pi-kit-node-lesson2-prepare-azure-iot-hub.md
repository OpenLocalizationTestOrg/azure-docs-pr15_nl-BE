<properties
 pageTitle="De hub IoT maken en registreren van uw Pi frambozen 3 | Microsoft Azure"
 description="Een resourcegroep maken en een Azure IoT-hub maakt uw Pi registreren in de Azure IoT hub met behulp van de CLI Azure."
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

# <a name="22-create-your-iot-hub-and-register-your-raspberry-pi-3"></a>2.2 de IoT hub maken en registreren van uw frambozen Pi 3

## <a name="221-what-you-will-do"></a>2.2.1 wat u doet

- Een resourcegroep maken.
- Maak uw Azure IoT hub in de resourcegroep.
- De frambozen Pi 3 toevoegen aan de Azure IoT hub met behulp van de CLI Azure.

Wanneer u uw Pi toevoegen aan uw hub IoT de CLI Azure, genereert de service een sleutel voor de Pi te verifiëren met de service. Als u aan eventuele problemen, oplossingen in de [pagina voor probleemoplossing met](iot-hub-raspberry-pi-kit-node-troubleshooting.md)zoeken.

## <a name="222-what-you-will-learn"></a>2.2.2 Wat leert u

- Het gebruik van de CLI Azure voor het maken van een Hub Azure IoT.
- Het maken van een apparaat-id voor de Pi in uw Azure IoT-Hub.

## <a name="223-what-you-need"></a>2.2.3 wat u nodig hebt

- Een account Azure
- Een Mac- of een Windows-computer met de CLI Azure is geïnstalleerd

## <a name="224-create-your-azure-iot-hub"></a>2.2.4 de Azure IoT hub maken

Azure IoT Hub kunt u verbinding maken, controleren en beheren van miljoenen IoT activa. Om uw Azure IoT-hub maakt, als volgt te werk:

1. Log in bij je Azure rekening met de volgende opdracht:

    ```bash
    az login
    ```

    De beschikbare Azure abonnementen worden weergegeven na een geslaagde aanmelding.

2. Stel de standaard Azure abonnement dat u gebruiken wilt door de volgende opdracht uit te voeren:

    ```bash
    az account set -n {subscription id or name}
    ```

    De abonnement-ID of naam vindt u in de uitvoer van `az login`.

3. De provider registreren door de volgende opdracht uit te voeren:

    ```bash
    az resource provider register -n "Microsoft.Devices"
    ```

    Voordat u de Azure bron die de provider biedt kunt implementeren, moet u de provider registreren.

    > [AZURE.NOTE] De meeste providers zijn ingeschreven, automatisch door de Azure portal of de Azure CLI die u gebruikt, maar niet alle. Zie [problemen met algemene Azure-implementatiefouten met Azure Resource Manager](../resource-manager-common-deployment-errors.md) voor meer informatie over de provider

4. Maak een brongroep iot-voorbeeld in de regio West VS benoemd door de volgende opdracht uit te voeren:

    ```bash
    az resource group create --name iot-sample --location westus
    ```

5. Een hub IoT in de resourcegroep iot monster maken door de volgende opdracht uit te voeren:

    ```bash
    az iot hub create --name {my hub name} --resource-group iot-sample
    ```

    De standaard versie van de IoT-hub die u maakt is **F1**, **gratis**. Zie voor meer informatie, [prijzen Azure IoT Hub](https://azure.microsoft.com/pricing/details/iot-hub/).

> [AZURE.NOTE] De naam van uw hub-IoT moet uniek zijn.
>
> Onder uw Azure abonnement kunt u maar één versie van de **F1** van Azure IoT Hub.

## <a name="225-register-your-pi-in-your-iot-hub"></a>2.2.5 de Pi in uw hub IoT registreren

Elk apparaat dat verzenden/ontvangen van berichten van uw hub Azure IoT moet worden geregistreerd met een unieke ID.

Uw Pi in uw hub Azure IoT registreren door de volgende opdracht uitgevoerd:

```bash
az iot device create --device-id myraspberrypi --hub {my hub name} --resource-group iot-sample
```

## <a name="226-summary"></a>2.2.6 samenvatting

U hebt een hub Azure IoT gemaakt en de Pi met een apparaat-id geregistreerd in uw Azure IoT-hub. U bent klaar om te gaan met de volgende les voor meer informatie over het verzenden van berichten vanuit uw Pi op uw hub IoT.

## <a name="next-steps"></a>Volgende stappen

Je bent nu klaar om les 3 die begint met het [maken van een app Azure functie en een account Azure opslag te verwerken en IoT hub berichten worden opgeslagen](iot-hub-raspberry-pi-kit-node-lesson3-deploy-resource-manager-template.md).