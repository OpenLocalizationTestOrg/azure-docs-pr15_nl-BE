<properties
    pageTitle="Maak een IoT Hub met Azure CLI | Microsoft Azure"
    description="Volg de in dit artikel als u wilt maken een IoT Hub met de opdrachtregelinterface van Azure."
    services="iot-hub"
    documentationCenter=".net"
    authors="BeatriceOltean"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="multiple"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="09/21/2016"
     ms.author="boltean"/>

# <a name="create-an-iot-hub-using-azure-cli"></a>Maak een IoT Hub met Azure CLI

[AZURE.INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

## <a name="introduction"></a>Inleiding

Azure opdrachtregelinterface kunt u maken en hubs Azure IoT via programmacode beheren. In dit artikel wordt beschreven hoe u met behulp van de CLI Azure een IoT-Hub.

Deze zelfstudie hebt u het volgende nodig:

- Een actieve account Azure. Als u geen account hebt, kunt u een [gratis account] [ lnk-free-trial] binnen een paar minuten.
- [Azure CLI 0.10.4] [ lnk-CLI-install] of hoger. Als u al Azure CLI kunt u de huidige versie op de opdrachtregel met de volgende opdracht valideren:
```
    azure --version
```

> [AZURE.NOTE] Azure heeft twee verschillende modellen voor het maken en werken met resources: [Azure Resource Manager en klassiek](../resource-manager-deployment-model.md). De CLI Azure moet Azure Resource Manager modus zijn:
```
    azure config mode arm
```

## <a name="set-your-azure-account-and-subscription"></a>Stel uw Azure-account en abonnement 

1. Bij aanmelding van de opdrachtprompt door de volgende opdracht te typen
```
    azure login
```
De voorgestelde webbrowser en de code gebruiken om te verifiëren.

2. Als er meerdere Azure abonnementen, zal verbinding maken met Azure toegang verlenen tot alle Azure abonnementen die zijn gekoppeld aan uw referenties. U kunt bekijken de Azure abonnementen, alsmede welke de standaard is met de opdracht
```
    azure account list 
```

Instellen van het abonnement context waarin u wilt de rest van het gebruik van de opdrachten uitvoeren

```
    azure account set <subscription name>
```

3. Als er een resourcegroep kunt u één met de naam **exampleResourceGroup** 
```
    azure group create -n exampleResourceGroup -l westus
```

> [AZURE.TIP] Het artikel [gebruiken de Azure CLI beheren Azure resources en resourcegroepen] [ lnk-CLI-arm] vindt u meer informatie wilt over het gebruik van Azure CLI Azure bronnen beheren. 


## <a name="create-an-iot-hub"></a>Maak een Hub IoT

Vereiste parameters:

```
 azure iothub create -g <resource-group> -n <name> -l <location> -s <sku-name> -u <units>  
    - <resourceGroup> The resource group name (case insensitive alphanumeric, underscore and hyphen, 1-64 length)
    - <name> (The name of the IoT hub to be created. The format is case insensitive alphanumeric, underscore and hyphen, 3-50 length )
    - <location> (The location (azure region/datacenter) where the IoT hub will be provisioned.
    - <sku-name> (The name of the sku, one of: [F1, S1, S2, S3] etc. For the latest full list refer to the pricing page for IoT Hub.
    - <units> (The number of provisioned units. Range : F1 [1-1] : S1, S2 [1-200] : S3 [1-10]. IoT Hub units are based on your total message count and the number of devices you want to connect.)
```
Overzicht van alle parameters beschikbaar voor het maken van kunt u de opdracht help in MS-DOS-prompt
```
    azure iothub create -h 
```
Snel voorbeeld:

 Als u wilt maken van een IoT Hub **exampleIoTHubName** in de resource groep **exampleResourceGroup** uit te voeren met de volgende opdracht genoemd
```
    azure iothub create -g exampleResourceGroup -n exampleIoTHubName -l westus -k s1 -u 1
```

> [AZURE.NOTE] Deze opdracht Azure CLI maakt een standaard IoT-Hub van S1, waarvoor u worden gefactureerd. U kunt de IoT hub **exampleIoTHubName** gebruiken na de opdracht verwijderen 
```
    azure iothub delete -g exampleResourceGroup -n exampleIoTHubName
```


## <a name="next-steps"></a>Volgende stappen
Voor meer informatie over het ontwikkelen voor IoT Hub, raadpleegt u het volgende:
- [SDK's IoT Hub][lnk-sdks]

Om de mogelijkheden van IoT Hub verder verkennen, Zie:

- [IoT Hub beheren met behulp van de Portal Azure][lnk-portal]

<!-- Links -->
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-azure-portal]: https://portal.azure.com/
[lnk-status]: https://azure.microsoft.com/status/
[lnk-CLI-install]: ../xplat-cli-install.md
[lnk-rest-api]: https://msdn.microsoft.com/library/mt589014.aspx
[lnk-CLI-arm]: ../xplat-cli-azure-resource-manager.md

[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-portal]: iot-hub-create-through-portal.md 
