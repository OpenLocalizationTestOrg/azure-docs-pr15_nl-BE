<properties
    pageTitle="Maak een IoT Hub met behulp van een sjabloon Azure Resource Manager en PowerShell | Microsoft Azure"
    description="Volg deze zelfstudie aan de slag met Azure Resource Manager-sjablonen voor het maken van een IoT Hub met PowerShell."
    services="iot-hub"
    documentationCenter=".net"
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="multiple"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="09/07/2016"
     ms.author="dobett"/>

# <a name="create-an-iot-hub-using-powershell"></a>Maak een IoT hub met PowerShell

[AZURE.INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

## <a name="introduction"></a>Inleiding

Azure Resource Manager kunt u maken en hubs Azure IoT via programmacode beheren. In deze zelfstudie wordt beschreven hoe u een sjabloon Azure Resource Manager gebruiken voor het maken van een hub IoT met PowerShell.

> [AZURE.NOTE] Azure heeft twee verschillende modellen voor het maken en werken met resources: [Azure Resource Manager en klassiek](../resource-manager-deployment-model.md).  In dit artikel beschreven hoe u met het implementatiemodel Azure Resource Manager.

Deze zelfstudie hebt u het volgende nodig:

- Een actieve account Azure. <br/>Als u geen account hebt, kunt u een [gratis account] [ lnk-free-trial] binnen een paar minuten.
- [Microsoft Azure PowerShell 1.0] [ lnk-powershell-install] of hoger.

> [AZURE.TIP] Het artikel [Met behulp van Azure PowerShell Azure Resource Manager] [ lnk-powershell-arm] vindt u meer informatie wilt over het gebruik van PowerShell scripts en sjablonen Azure Resource Manager Azure bronnen maken. 

## <a name="connect-to-your-azure-subscription"></a>Verbinding maken met uw abonnement Azure

Voer de volgende opdracht kunt aanmelden bij uw abonnement Azure in PowerShell vanaf de opdrachtprompt:

```
Login-AzureRmAccount
```

Om te ontdekken waar u kunt implementeren een IoT hub en de ondersteunde API-versies kunt u de volgende opdrachten:

```
((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Devices).ResourceTypes | Where-Object ResourceTypeName -eq IoTHubs).Locations
((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Devices).ResourceTypes | Where-Object ResourceTypeName -eq IoTHubs).ApiVersions
```

Een resourcegroep bevat de IoT hub met de volgende opdracht in een van de ondersteunde locaties voor IoT Hub maken. In dit voorbeeld maakt u een resourcegroep genaamd **MyIoTRG1**:

```
New-AzureRmResourceGroup -Name MyIoTRG1 -Location "East US"
```

## <a name="submit-an-azure-resource-manager-template-to-create-an-iot-hub"></a>Een sjabloon Azure Resource Manager wilt maken van een hub IoT indienen

Een JSON-sjabloon gebruiken voor het maken van een hub IoT in de resourcegroep. U kunt ook een sjabloon Azure Resource Manager wijzigingen aanbrengen in een bestaande IoT-hub.

1. Gebruik een teksteditor voor het maken van een Azure Resource Manager de sjabloon **template.json** met de volgende definitie van de resource voor het maken van een nieuwe standaard IoT-hub. In dit voorbeeld voegt de IoT Hub in de regio **Oost-VS** , maakt u twee groepen consumenten (**cg1** en **cg2**) op het eindpunt van de gebeurtenis Hub-compatibele en de **2016-02-03** API versie wordt gebruikt. Deze sjabloon verwacht u in de naam van de hub IoT doorgeven als een parameter met de naam **hubName**. Zie voor de huidige lijst met locaties die ondersteuning bieden voor IoT Hub [Azure Status][lnk-status].

    ```
    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "hubName": {
          "type": "string"
        }
      },
      "resources": [
      {
        "apiVersion": "2016-02-03",
        "type": "Microsoft.Devices/IotHubs",
        "name": "[parameters('hubName')]",
        "location": "East US",
        "sku": {
          "name": "S1",
          "tier": "Standard",
          "capacity": 1
        },
        "properties": {
          "location": "East US"
        }
      },
      {
        "apiVersion": "2016-02-03",
        "type": "Microsoft.Devices/IotHubs/eventhubEndpoints/ConsumerGroups",
        "name": "[concat(parameters('hubName'), '/events/cg1')]",
        "dependsOn": [
          "[concat('Microsoft.Devices/Iothubs/', parameters('hubName'))]"
        ]
      },
      {
        "apiVersion": "2016-02-03",
        "type": "Microsoft.Devices/IotHubs/eventhubEndpoints/ConsumerGroups",
        "name": "[concat(parameters('hubName'), '/events/cg2')]",
        "dependsOn": [
          "[concat('Microsoft.Devices/Iothubs/', parameters('hubName'))]"
        ]
      }
      ],
      "outputs": {
        "hubKeys": {
          "value": "[listKeys(resourceId('Microsoft.Devices/IotHubs', parameters('hubName')), '2016-02-03')]",
          "type": "object"
        }
      }
    }
    ```

2. Het sjabloonbestand Azure Resource Manager op de lokale computer opgeslagen. In dit voorbeeld wordt ervan uitgegaan dat u de gegevens opslaan in een map met de naam **c:\templates**.

3. Voer de volgende opdracht voor de implementatie van uw nieuwe IoT hub, de naam van uw hub-IoT als parameter doorgeeft. In dit voorbeeld wordt de naam van de hub IoT **abcmyiothub** (Let op dat deze naam moet uniek behoort uw naam of initialen):

    ```
    New-AzureRmResourceGroupDeployment -ResourceGroupName MyIoTRG1 -TemplateFile C:\templates\template.json -hubName abcmyiothub
    ```

4. De uitvoer geeft de sleutels voor de IoT-hub die u hebt gemaakt.

5. U kunt controleren of dat de toepassing de nieuwe IoT hub toegevoegd via de [Azure portal] [ lnk-azure-portal] en het weergeven van de lijst van bronnen, of via de PowerShell-cmdlet **Get-AzureRmResource** .

> [AZURE.NOTE] De toepassing van dit voorbeeld wordt een standaard IoT-Hub van S1, waarvoor u worden gefactureerd. U kunt de IoT hub via de [Azure portal] verwijderen[ lnk-azure-portal] of via de PowerShell-cmdlet **Verwijderen AzureRmResource** wanneer u klaar bent.

## <a name="next-steps"></a>Volgende stappen

Nu u een IoT hub met behulp van een sjabloon Azure Resource Manager met PowerShell hebt geïmplementeerd, kunt u verder verkennen:

- Meer informatie over de mogelijkheden van de [IoT Hub resource provider REST API][lnk-rest-api].
- Overzicht van [Azure Resource Manager] [ lnk-azure-rm-overview] voor meer informatie over de mogelijkheden van Azure Resource Manager.

Voor meer informatie over het ontwikkelen voor IoT Hub, raadpleegt u het volgende:

- [Inleiding tot de C-SDK][lnk-c-sdk]
- [SDK's IoT Hub][lnk-sdks]

Om de mogelijkheden van IoT Hub verder verkennen, Zie:

- [Een apparaat met de SDK IoT Gateway simuleren][lnk-gateway]

<!-- Links -->
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-azure-portal]: https://portal.azure.com/
[lnk-status]: https://azure.microsoft.com/status/
[lnk-powershell-install]: ../powershell-install-configure.md
[lnk-rest-api]: https://msdn.microsoft.com/library/mt589014.aspx
[lnk-azure-rm-overview]: ../azure-resource-manager/resource-group-overview.md
[lnk-powershell-arm]: ../powershell-azure-resource-manager.md

[lnk-c-sdk]: iot-hub-device-sdk-c-intro.md
[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
