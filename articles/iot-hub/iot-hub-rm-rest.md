<properties
    pageTitle="Maak een IoT Hub met de REST API | Microsoft Azure"
    description="Volg deze zelfstudie aan de slag met de REST-API voor het maken van een IoT Hub."
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

# <a name="tutorial-create-an-iot-hub-using-a-c-program-and-the-rest-api"></a>Zelfstudie: Maak een IoT-hub met een C#-programma en de REST-API

[AZURE.INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

## <a name="introduction"></a>Inleiding

U kunt de [IoT Hub resource provider REST API] [ lnk-rest-api] maken en hubs Azure IoT via programmacode beheren. In deze zelfstudie wordt beschreven hoe u IoT Hub resource provider REST-API gebruiken voor het maken van een hub IoT vanuit C#-programma.

> [AZURE.NOTE] Azure heeft twee verschillende modellen voor het maken en werken met resources: [Azure Resource Manager en klassiek](../resource-manager-deployment-model.md).  In dit artikel beschreven hoe u met het implementatiemodel Azure Resource Manager.

Deze zelfstudie hebt u het volgende nodig:

- Microsoft Visual Studio 2015.
- Een actieve account Azure. <br/>Als u geen account hebt, kunt u een [gratis account] [ lnk-free-trial] binnen een paar minuten.
- [Microsoft Azure PowerShell 1.0] [ lnk-powershell-install] of hoger.

[AZURE.INCLUDE [iot-hub-prepare-resource-manager](../../includes/iot-hub-prepare-resource-manager.md)]

## <a name="prepare-your-visual-studio-project"></a>De Visual Studio-project voorbereiden

1. Maak een Visual C# Windows-project met behulp van de projectsjabloon **Consoletoepassing** in Visual Studio. Naam van het project **CreateIoTHubREST**.

2. In de Solution Explorer met de rechtermuisknop op het project en klik vervolgens op **Beheren NuGet pakketten**.

3. In Nuget Package Manager **zijn voorlopige versie**controleren, en zoek naar **Microsoft.Azure.Management.ResourceManager**. **Installeren**, in **Wijzigingen** klikt u op **OK**, klik op **ik ga akkoord** om de licenties te accepteren.

4. **Microsoft.IdentityModel.Clients.ActiveDirectory**zoeken in Nuget Package Manager.  **Installeren**, in **Wijzigingen** klikt u op **OK**, klik op **ik ga akkoord** om de gebruiksrechtovereenkomst te accepteren.

6. In Program.cs, vervangen de bestaande overzichten van **met behulp van** de volgende:

    ```
    using System;
    using System.Net.Http;
    using System.Net.Http.Headers;
    using System.Text;
    using Microsoft.Azure.Management.ResourceManager;
    using Microsoft.Azure.Management.ResourceManager.Models;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using Newtonsoft.Json;
    using Microsoft.Rest;
    using System.Linq;
    using System.Threading;
    ```
    
7. Program.cs, voeg de volgende statische variabelen vervangt de waarden van de tijdelijke aanduiding. U een notitie van **ApplicationId**, **SubscriptionId**, **TenantId**en **wachtwoord** eerder in deze handleiding hebt gemaakt. **Deze naam** is de naam van de resourcegroep die u gebruikt bij het maken van de hub IoT, kan bestaan uit een bestaande groep of een nieuwe. **IoT Hub** is de naam van de IoT-Hub maakt, zoals **MyIoTHub** (deze naam moet uniek, zodat uw naam of initialen behoort). **Implementatie** is een naam voor de distributie, zoals **Deployment_01**.

    ```
    static string applicationId = "{Your ApplicationId}";
    static string subscriptionId = "{Your SubscriptionId}";
    static string tenantId = "{Your TenantId}";
    static string password = "{Your application Password}";
    
    static string rgName = "{Resource group name}";
    static string iotHubName = "{IoT Hub name including your initials}";
    ```

[AZURE.INCLUDE [iot-hub-get-access-token](../../includes/iot-hub-get-access-token.md)]

## <a name="use-the-rest-api-to-create-an-iot-hub"></a>De REST-API gebruiken voor het maken van een hub IoT

Gebruik de [IoT Hub REST API] [ lnk-rest-api] voor het maken van een hub IoT in de resourcegroep. U kunt de REST API ook wijzigingen aanbrengen in een bestaande IoT-hub.

1. De volgende methode toevoegen aan Program.cs:
    
    ```
    static void CreateIoTHub(string token)
    {
        
    }
    ```

2. Voeg de volgende code de methode **CreateIoTHub** **HttpClient** object maken met het verificatietoken in de headers:

    ```
    HttpClient client = new HttpClient();
    client.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", token);
    ```

3. Voeg de volgende code de methode **CreateIoTHub** om te beschrijven de IoT hub te maken en het genereren van een JSON representatie (Zie [Azure Status]voor de huidige lijst met locaties die ondersteuning bieden voor IoT Hub[lnk-status]):

    ```
    var description = new
    {
      name = iotHubName,
      location = "East US",
      sku = new
      {
        name = "S1",
        tier = "Standard",
        capacity = 1
      }
    };
    
    var json = JsonConvert.SerializeObject(description, Formatting.Indented);
    ```

4. Voeg de volgende code de methode **CreateIoTHub** de REST-aanvraag verzendt naar Azure, het antwoord controleren en ophalen van de URL die u gebruiken kunt voor het controleren van de status van de taak van de implementatie:

    ```
    var content = new StringContent(JsonConvert.SerializeObject(description), Encoding.UTF8, "application/json");
    var requestUri = string.Format("https://management.azure.com/subscriptions/{0}/resourcegroups/{1}/providers/Microsoft.devices/IotHubs/{2}?api-version=2016-02-03", subscriptionId, rgName, iotHubName);
    var result = client.PutAsync(requestUri, content).Result;
      
    if (!result.IsSuccessStatusCode)
    {
      Console.WriteLine("Failed {0}", result.Content.ReadAsStringAsync().Result);
      return;
    }
    
    var asyncStatusUri = result.Headers.GetValues("Azure-AsyncOperation").First();
    ```

5. De volgende code toevoegen aan het einde van de methode **CreateIoTHub** de **asyncStatusUri** -adres opgehaald in de vorige stap om te wachten op de installatie te voltooien:

    ```
    string body;
    do
    {
      Thread.Sleep(10000);
      HttpResponseMessage deploymentstatus = client.GetAsync(asyncStatusUri).Result;
      body = deploymentstatus.Content.ReadAsStringAsync().Result;
    } while (body == "{\"status\":\"Running\"}");
    ```

6. De volgende code toevoegen aan het einde van de **CreateIoTHub** -methode voor het ophalen van de sleutels van de hub IoT u hebt gemaakt en deze afdrukken op de console:

    ```
    var listKeysUri = string.Format("https://management.azure.com/subscriptions/{0}/resourceGroups/{1}/providers/Microsoft.Devices/IotHubs/{2}/IoTHubKeys/listkeys?api-version=2015-08-15-preview", subscriptionId, rgName, iotHubName);
    var keysresults = client.PostAsync(listKeysUri, null).Result;
    
    Console.WriteLine("Keys: {0}", keysresults.Content.ReadAsStringAsync().Result);
    ```
    
## <a name="complete-and-run-the-application"></a>Voltooien en de toepassing uitvoeren

Nu kunt u de toepassing uitvoeren met de methode **CreateIoTHub** aanroepen voordat u bouwen en uit te voeren.

1. De volgende code toevoegen aan het einde van de **Main** -methode:

    ```
    CreateIoTHub(token.AccessToken);
    Console.ReadLine();
    ```
    
2. Klik op **Opbouwen** en **oplossing**. Corrigeer eventuele fouten.

3. Klik op **fouten opsporen** **Foutopsporing starten** als de toepassing wilt uitvoeren. Het kan enkele minuten duren voordat de implementatie uit te voeren.

4. U kunt controleren of dat de toepassing de nieuwe IoT hub toegevoegd via de [Azure portal] [ lnk-azure-portal] en het weergeven van de lijst van bronnen, of via de PowerShell-cmdlet **Get-AzureRmResource** .

> [AZURE.NOTE] De toepassing van dit voorbeeld wordt een standaard IoT-Hub van S1, waarvoor u worden gefactureerd. Wanneer u klaar bent, kunt u de IoT hub via de [Azure portal] verwijderen[ lnk-azure-portal] of via de PowerShell-cmdlet **Verwijderen AzureRmResource** wanneer u klaar bent.

## <a name="next-steps"></a>Volgende stappen

Nu u een IoT hub met de REST API hebt geïmplementeerd, kunt u verder verkennen:

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

[lnk-c-sdk]: iot-hub-device-sdk-c-intro.md
[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
