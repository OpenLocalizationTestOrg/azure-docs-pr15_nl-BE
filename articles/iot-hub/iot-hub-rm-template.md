<properties
    pageTitle="Maak een IoT Hub met behulp van een sjabloon ARM en C# | Microsoft Azure"
    description="Volg deze zelfstudie aan de slag met Azure Resource Manager-sjablonen voor het maken van een IoT Hub met een C#-programma."
    services="iot-hub"
    documentationCenter=".net"
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="dotnet"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="08/16/2016"
     ms.author="dobett"/>

# <a name="create-an-iot-hub-using-a-c-program-with-an-azure-resource-manager-template"></a>Een IoT-hub met een C#-programma met een bronnenbeheerder Azure-sjabloon maken

[AZURE.INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

## <a name="introduction"></a>Inleiding

Azure Resource Manager kunt u maken en hubs Azure IoT via programmacode beheren. In deze zelfstudie wordt beschreven hoe u een sjabloon Azure Resource Manager gebruiken voor het maken van een hub IoT vanuit C#-programma.

> [AZURE.NOTE] Azure heeft twee verschillende modellen voor het maken en werken met resources: [Azure Resource Manager en klassiek](../resource-manager-deployment-model.md).  In dit artikel beschreven hoe u met het implementatiemodel Azure Resource Manager.

Deze zelfstudie hebt u het volgende nodig:

- Microsoft Visual Studio 2015.
- Een actieve account Azure. <br/>Als u geen account hebt, kunt u een [gratis account] [ lnk-free-trial] binnen een paar minuten.
- Een [opslag Azure rekening] [ lnk-storage-account] waar u uw sjabloonbestanden Azure Resource Manager kunt opslaan.
- [Microsoft Azure PowerShell 1.0] [ lnk-powershell-install] of hoger.

[AZURE.INCLUDE [iot-hub-prepare-resource-manager](../../includes/iot-hub-prepare-resource-manager.md)]

## <a name="prepare-your-visual-studio-project"></a>De Visual Studio-project voorbereiden

1. Maak een Visual C# Windows-project met behulp van de projectsjabloon **Consoletoepassing** in Visual Studio. Naam van het project **CreateIoTHub**.

2. In de Solution Explorer met de rechtermuisknop op het project en klik vervolgens op **Beheren NuGet pakketten**.

3. In Nuget Package Manager **zijn voorlopige versie**controleren, en zoek naar **Microsoft.Azure.Management.ResourceManager**. **Installeren**, in **Wijzigingen** klikt u op **OK**, klik op **ik ga akkoord** om de licenties te accepteren.

4. **Microsoft.IdentityModel.Clients.ActiveDirectory**zoeken in Nuget Package Manager.  **Installeren**, in **Wijzigingen** klikt u op **OK**, klik op **ik ga akkoord** om de gebruiksrechtovereenkomst te accepteren.

5. In Program.cs, vervangen de bestaande overzichten van **met behulp van** de volgende:

    ```
    using System;
    using Microsoft.Azure.Management.ResourceManager;
    using Microsoft.Azure.Management.ResourceManager.Models;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using Microsoft.Rest;
    ```
    
6. Program.cs, voeg de volgende statische variabelen vervangt de waarden van de tijdelijke aanduiding. U een notitie van **ApplicationId**, **SubscriptionId**, **TenantId**en **wachtwoord** eerder in deze handleiding hebt gemaakt. **Opslag van uw Azure** is account de naam van de account Azure opslag waar u uw sjabloonbestanden Azure Resource Manager wilt opslaan. **Deze naam** is de naam van de resourcegroep die u gebruikt bij het maken van de IoT Hub, kan bestaan uit een bestaande groep of een nieuwe. **Implementatie** is een naam voor de distributie, zoals **Deployment_01**.

    ```
    static string applicationId = "{Your ApplicationId}";
    static string subscriptionId = "{Your SubscriptionId}";
    static string tenantId = "{Your TenantId}";
    static string password = "{Your application Password}";
    static string storageAddress = "https://{Your storage account name}.blob.core.windows.net";
    static string rgName = "{Resource group name}";
    static string deploymentName = "{Deployment name}";
    ```

[AZURE.INCLUDE [iot-hub-get-access-token](../../includes/iot-hub-get-access-token.md)]

## <a name="submit-an-azure-resource-manager-template-to-create-an-iot-hub"></a>Een sjabloon Azure Resource Manager wilt maken van een hub IoT indienen

Gebruik een JSON-bestand voor sjabloon en parameter voor het maken van een hub IoT in de resourcegroep. U kunt ook een sjabloon Azure Resource Manager wijzigingen aanbrengen in een bestaande IoT-hub.

1. In de Solution Explorer met de rechtermuisknop op het project, klikt u op **toevoegen**en klik vervolgens op **Nieuw Item**. Een JSON-bestand genaamd **template.json** aan uw project toevoegen.

2. De inhoud van de **template.json** vervangen door de volgende definitie van de resource toe te voegen standaard IoT-hub voor de regio **Oost-VS** (Zie [Azure Status]voor de huidige lijst met regio's die ondersteuning bieden voor IoT Hub[lnk-status]):

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

3. In de Solution Explorer met de rechtermuisknop op het project, klikt u op **toevoegen**en klik vervolgens op **Nieuw Item**. Een JSON-bestand genaamd **parameters.json** aan uw project toevoegen.

4. De inhoud van **parameters.json** vervangen door de volgende parameterinformatie, zoals een naam voor het nieuwe IoT hub ingesteld **{uw initialen} mynewiothub**. Houd er rekening mee dat de naam van de hub IoT globaal uniek moet zijn zodat u uw naam of initialen behoort):

    ```
    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "hubName": { "value": "mynewiothub" }
      }
    }
    ```

5. Verbinding maken met uw abonnement Azure in **Server Explorer**en in Azure opslag account een zogenaamd **sjablonen**maken. In het deelvenster **Eigenschappen** de **openbare alleen** machtigingen instellen voor de container **sjablonen** naar **Blob**.

6. In **Server Explorer**met de rechtermuisknop op de container met **sjablonen** en klik vervolgens op **Weergave Blob-Container**. Klik op de knop **Uploaden Blob** , de twee bestanden, **parameters.json** en **templates.json**selecteren en klik op **openen** om de JSON-bestanden uploaden naar de container **sjablonen** . De URL's van de BLOB's met de JSON-gegevens zijn:

    ```
    https://{Your storage account name}.blob.core.windows.net/templates/parameters.json
    https://{Your storage account name}.blob.core.windows.net/templates/template.json
    ```

7. De volgende methode toevoegen aan Program.cs:
    
    ```
    static void CreateIoTHub(ResourceManagementClient client)
    {
        
    }
    ```

5. De volgende code toevoegen aan de **CreateIoTHub** -methode voor het verzenden van de bestanden sjabloon en parameter aan de bronnenbeheerder van Azure:

    ```
    var createResponse = client.Deployments.CreateOrUpdate(
        rgName,
        deploymentName,
        new Deployment()
        {
          Properties = new DeploymentProperties
          {
            Mode = DeploymentMode.Incremental,
            TemplateLink = new TemplateLink
            {
              Uri = storageAddress + "/templates/template.json"
            },
            ParametersLink = new ParametersLink
            {
              Uri = storageAddress + "/templates/parameters.json"
            }
          }
        });
    ```

6. De volgende code toevoegen aan de **CreateIoTHub** -methode die de status en de sleutels voor het nieuwe IoT hub worden weergegeven:

    ```
    string state = createResponse.Properties.ProvisioningState;
    Console.WriteLine("Deployment state: {0}", state);

    if (state != "Succeeded")
    {
      Console.WriteLine("Failed to create iothub");
    }
    Console.WriteLine(createResponse.Properties.Outputs);
    ```

## <a name="complete-and-run-the-application"></a>Voltooien en de toepassing uitvoeren

Nu kunt u de toepassing uitvoeren met de methode **CreateIoTHub** aanroepen voordat u bouwen en uit te voeren.

1. De volgende code toevoegen aan het einde van de **Main** -methode:

    ```
    CreateIoTHub(client);
    Console.ReadLine();
    ```
    
2. Klik op **Opbouwen** en **oplossing**. Corrigeer eventuele fouten.

3. Klik op **fouten opsporen** **Foutopsporing starten** als de toepassing wilt uitvoeren. Het kan enkele minuten duren voordat de implementatie uit te voeren.

4. U kunt controleren of dat de toepassing de nieuwe IoT hub toegevoegd via de [Azure portal] [ lnk-azure-portal] en het weergeven van de lijst van bronnen, of via de PowerShell-cmdlet **Get-AzureRmResource** .

> [AZURE.NOTE] De toepassing van dit voorbeeld wordt een standaard IoT-Hub van S1, waarvoor u worden gefactureerd. U kunt de IoT hub via de [Azure portal] verwijderen[ lnk-azure-portal] of via de PowerShell-cmdlet **Verwijderen AzureRmResource** wanneer u klaar bent.

## <a name="next-steps"></a>Volgende stappen

Nu u een IoT-hub met een bronnenbeheerder Azure-sjabloon met een C#-programma hebt geïmplementeerd, kunt u verder verkennen:

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
[lnk-storage-account]: ../storage/storage-create-storage-account.md

[lnk-c-sdk]: iot-hub-device-sdk-c-intro.md
[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
