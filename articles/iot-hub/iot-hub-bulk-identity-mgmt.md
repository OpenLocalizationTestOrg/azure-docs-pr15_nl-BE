<properties
 pageTitle="Import/export van IoT Hub apparaat identiteiten | Microsoft Azure"
 description="Concepten en .NET codefragmenten voor bulk beheer van IoT Hub apparaat-id 's"
 services="iot-hub"
 documentationCenter=".net"
 authors="dominicbetts"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="10/05/2016"
 ms.author="dobett"/>

# <a name="bulk-management-of-iot-hub-device-identities"></a>Bulk beheer van IoT Hub apparaat-id 's

Elke hub IoT heeft een apparaat identiteit register die kunt u resources per apparaat in de service, zoals een wachtrij met tijdens een vlucht cloud-to-device-berichten maken. Het apparaat identiteit register kunt u ook toegang tot het apparaat gerichte eindpunten. In dit artikel wordt beschreven hoe importeren en exporteren van apparaat-id's in bulk en naar een apparaat-id-register.

Importeren en exporteren van bewerkingen plaatsvinden in het kader van *taken* die het mogelijk maken voor het uitvoeren van bulkbewerkingen-service tegen een IoT-hub.

De klasse **RegistryManager** bevat de **ExportDevicesAsync** en **ImportDevicesAsync** -methoden die gebruikmaken van het kader van het **Project** . Deze methoden kunnen u exporteren, importeren en het geheel van een register IoT hub apparaat synchroniseren.

## <a name="what-are-jobs"></a>Wat zijn taken?

Registerbewerkingen apparaat-id de **taak** -systeem gebruiken wanneer de bewerking:

*  Heeft mogelijk uitvoering lang in vergelijking met de standaard runtime-bewerkingen, of
*  Deze eigenschap retourneert een grote hoeveelheid gegevens voor de gebruiker.

In deze gevallen, in plaats van een enkele API-aanroep wachten of blokkeren van het resultaat van de bewerking, maakt de bewerking asynchroon u een **taak** voor deze hub IoT. De bewerking vervolgens retourneert onmiddellijk een **JobProperties** -object.

De volgende C#-codefragment ziet u hoe een taak exporteren wilt maken:

```
// Call an export job on the IoT Hub to retrieve all devices
JobProperties exportJob = await registryManager.ExportDevicesAsync(containerSasUri, false);
```

Vervolgens kunt u de klasse **RegistryManager** opvragen van de status van de **taak** met de metagegevens voor de geretourneerde **JobProperties** .

De volgende C#-codefragment ziet u hoe gegevens op te vragen van elke vijf seconden om te zien als de taak is uitgevoerd:

```
// Wait until job is finished
while(true)
{
  exportJob = await registryManager.GetJobAsync(exportJob.JobId);
  if (exportJob.Status == JobStatus.Completed || 
      exportJob.Status == JobStatus.Failed ||
      exportJob.Status == JobStatus.Cancelled)
  {
    // Job has finished executing
    break;
  }

  await Task.Delay(TimeSpan.FromSeconds(5));
}
```

## <a name="export-devices"></a>Exporteren van apparaten

Gebruik de methode **ExportDevicesAsync** om het geheel van een IoT hub apparaat register exporteren naar een [Opslag Azure](https://azure.microsoft.com/documentation/services/storage/) blob-container met behulp van een [Gedeelde Access-handtekening](https://msdn.microsoft.com/library/ee395415.aspx).

Met deze methode kunt u betrouwbare back-ups van uw gegevens van een apparaat in een blob-container die u maakt.

De methode **ExportDevicesAsync** worden twee parameters:

*  Een *tekenreeks* met een URI van een blob-container. Deze URI bevat een SAS-token die schrijven toegang tot de container verleent. De taak wordt gemaakt van een blok blob in deze container voor het opslaan van de gegevens van het apparaat geserialiseerde exporteren. De SAS-token moet deze machtigingen zijn:
    
    ```
    SharedAccessBlobPermissions.Write | SharedAccessBlobPermissions.Read | SharedAccessBlobPermissions.Delete
    ```

*  Een *boolean* die aangeeft of u wilt uitsluiten van verificatiesleutels van uw gegevens exporteren. Als **false**verificatie sleutels zijn opgenomen in de uitvoer; anders worden als **null**sleutels geëxporteerd.

De volgende C#-codefragment ziet u hoe u een taak exporteren met verificatiesleutels apparaat in de gegevens exporteren te starten en vervolgens controleren op voltooien:

```
// Call an export job on the IoT Hub to retrieve all devices
JobProperties exportJob = await registryManager.ExportDevicesAsync(containerSasUri, false);

// Wait until job is finished
while(true)
{
    exportJob = await registryManager.GetJobAsync(exportJob.JobId);
    if (exportJob.Status == JobStatus.Completed || 
        exportJob.Status == JobStatus.Failed ||
        exportJob.Status == JobStatus.Cancelled)
    {
    // Job has finished executing
    break;
    }

    await Task.Delay(TimeSpan.FromSeconds(5));
}
```

De taak wordt de uitvoer opgeslagen in de opgegeven blob-container als een blob blok met de naam **devices.txt**. De uitvoergegevens bestaat uit JSON serieel apparaatgegevens, met één apparaat per regel.

In het volgende voorbeeld ziet u de uitvoergegevens:

```
{"id":"Device1","eTag":"MA==","status":"enabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
{"id":"Device2","eTag":"MA==","status":"enabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
{"id":"Device3","eTag":"MA==","status":"disabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
{"id":"Device4","eTag":"MA==","status":"disabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
{"id":"Device5","eTag":"MA==","status":"enabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
```

Als u toegang tot deze gegevens in code, kunt u deze gegevens met behulp van de klasse **ExportImportDevice** gemakkelijk terug converteert. De volgende C#-codefragment ziet u hoe gegevens van een apparaat dat eerder is geëxporteerd naar een blob blokkeren:

```
var exportedDevices = new List<ExportImportDevice>();

using (var streamReader = new StreamReader(await blob.OpenReadAsync(AccessCondition.GenerateIfExistsCondition(), RequestOptions, null), Encoding.UTF8))
{
  while (streamReader.Peek() != -1)
  {
    string line = await streamReader.ReadLineAsync();
    var device = JsonConvert.DeserializeObject<ExportImportDevice>(line);
    exportedDevices.Add(device);
  }
}
```

> [AZURE.NOTE]  U kunt ook de methode **GetDevicesAsync** van de klasse **RegistryManager** voor het ophalen van een lijst van uw apparaten. Deze benadering heeft echter een harde dop 1000 op het nummer van het apparaatobjecten die worden geretourneerd. Het verwachte gebruik geval voor de methode **GetDevicesAsync** is voor ontwikkeling-scenario's voor het opsporen van fouten en wordt niet aanbevolen voor de werkbelasting van de productie.

## <a name="import-devices"></a>Importeren van apparaten

De methode **ImportDevicesAsync** in de klasse **RegistryManager** kunt u voor het uitvoeren van bulkbewerkingen importeren en synchronisatie in een register IoT hub apparaat. Net als bij de methode **ExportDevicesAsync** gebruikt de methode **ImportDevicesAsync** het kader van het **Project** .

Wees voorzichtig met de methode **ImportDevicesAsync** omdat naast het inrichten van nieuwe apparaten in het register van de identiteit apparaat kan ook bijwerken en verwijderen van de bestaande apparaten.

> [AZURE.WARNING]  Een importbewerking kan niet ongedaan worden gemaakt. Altijd een back-up van uw bestaande gegevens met behulp van de methode **ExportDevicesAsync** aan een andere blob container voordat u bulk wijzigingen in het register van de identiteit apparaat aanbrengt.

De **ImportDevicesAsync** -methode heeft twee parameters:

*  Een *tekenreeks* met een URI van een blob [Storage Azure](https://azure.microsoft.com/documentation/services/storage/) container moet worden gebruikt als *invoer* voor de taak. Deze URI bevat een SAS-token die alleen toegang tot de container verleent. Deze container bevat een blob met de naam **devices.txt** met de geserialiseerde apparaatgegevens te importeren in het register van de identiteit apparaat. Het importeren van gegevens moet bevatten gegevens van een apparaat in de JSON-indeling die de taak van de **ExportImportDevice** wordt gebruikt bij het maken van een **devices.txt** -blob. De SAS-token moet deze machtigingen zijn:

    ```
    SharedAccessBlobPermissions.Read
    ```

*  Een *tekenreeks* met een URI van een blob [Storage Azure](https://azure.microsoft.com/documentation/services/storage/) container wilt gebruiken als *uitvoer* van de taak. De taak een blob blok in deze container op te slaan geen foutgegevens van de voltooide invoer **taak**gemaakt. De SAS-token moet deze machtigingen zijn:
    
    ```
    SharedAccessBlobPermissions.Write | SharedAccessBlobPermissions.Read | SharedAccessBlobPermissions.Delete
    ```

> [AZURE.NOTE]  De twee parameters kunnen verwijzen naar dezelfde container blob. De afzonderlijke parameters kunt u gewoon meer controle over uw gegevens als de container uitvoer aanvullende machtigingen vereist.

De volgende C#-codefragment ziet u hoe een taak importeren starten:

```
JobProperties importJob = await registryManager.ImportDevicesAsync(containerSasUri, containerSasUri);
```

## <a name="import-behavior"></a>Importeren van gedrag

U kunt de methode **ImportDevicesAsync** de volgende bulkbewerkingen uitvoeren in het register van de identiteit apparaat:

-   Bulk registratie van nieuwe apparaten
-   Bulk verwijderen van bestaande apparaten
-   Bulk-verandert de status (in- of uitschakelen van apparaten)
-   Toewijzing van het nieuwe apparaat verificatiesleutels bulk
-   Auto-regeneratie van apparaat verificatiesleutels in bulk

U kunt een willekeurige combinatie van de bovenstaande bewerkingen in één aanroep van **ImportDevicesAsync** uitvoeren. Bijvoorbeeld, kunt u registreren van nieuwe apparaten en verwijderen of bestaande apparaten tegelijk bijwerken. Wanneer met de methode **ExportDevicesAsync** gebruikt, kunt u al uw apparaten volledig vanaf een hub-IoT migreren naar een andere.

Gebruik de eigenschap optioneel **ImportMode %** in de importgegevens serialisatie voor elk apparaat waarmee de import proces per apparaat. De eigenschap **ImportMode %** heeft de volgende opties:

| ImportMode % |  Beschrijving |
| -------- | ----------- |
| **createOrUpdate** | Als een apparaat met de opgegeven **id**niet bestaat, wordt het nieuwe geregistreerd. <br/>Als het apparaat al bestaat, wordt met de opgegeven ingevoerde gegevens, ongeacht de waarde van **ETag** bestaande gegevens overschreven. |
| **maken** | Als een apparaat met de opgegeven **id**niet bestaat, wordt het nieuwe geregistreerd. <br/>Als het apparaat al bestaat, wordt een fout naar het logboekbestand geschreven. |
| **Update** | Als er al een apparaat met de opgegeven **id**bestaat, wordt met de opgegeven ingevoerde gegevens, ongeacht de waarde van **ETag** bestaande gegevens overschreven. <br/>Als het apparaat niet bestaat, wordt een fout naar het logboekbestand geschreven. |
| **updateIfMatchETag** | Als een apparaat al met de opgegeven **id bestaat**, bestaande gegevens overschreven door de opgegeven ingevoerde gegevens alleen als er een overeenkomst **ETag** . <br/>Als het apparaat niet bestaat, wordt een fout naar het logboekbestand geschreven. <br/>Als er een incompatibel **ETag** , wordt een fout naar het logboekbestand geschreven. |
| **createOrUpdateIfMatchETag** | Als een apparaat met de opgegeven **id**niet bestaat, wordt het nieuwe geregistreerd. <br/>Als het apparaat al bestaat, worden bestaande gegevens overschreven met de opgegeven ingevoerde gegevens alleen als er een overeenkomst **ETag** . <br/>Als er een incompatibel **ETag** , wordt een fout naar het logboekbestand geschreven. |
| **verwijderen** | Als er al een apparaat met de opgegeven **id**bestaat, wordt deze verwijderd ongeacht de **ETag** -waarde. <br/>Als het apparaat niet bestaat, wordt een fout naar het logboekbestand geschreven. |
| **deleteIfMatchETag** | Als er al een apparaat met de opgegeven **id**bestaat, wordt deze alleen als er een overeenkomst **ETag** is verwijderd. Als het apparaat niet bestaat, wordt een fout naar het logboekbestand geschreven. <br/>Als er een incompatibel ETag, wordt een fout naar het logboekbestand geschreven. |

> [AZURE.NOTE] Als de serialisatiegegevens niet expliciet een vlag **ImportMode %** voor een apparaat bepaalt, wordt standaard **createOrUpdate** tijdens het importeren.

## <a name="import-devices-example--bulk-device-provisioning"></a>Voorbeeld van de apparaten importeren – bulksgewijs inrichten van apparaat 

De volgende C#-voorbeeldcode ziet u hoe u meerdere apparaat-id's genereren die:

- Toetsen voor verificatie bevatten.
- Die gegevens van een apparaat naar een blob blok schrijven.
- De apparaten in het register van de identiteit apparaat importeren.

```
// Provision 1,000 more devices
var serializedDevices = new List<string>();

for (var i = 0; i < 1000; i++)
{
// Create a new ExportImportDevice
  var deviceToAdd = new ExportImportDevice()
  {
    Id = Guid.NewGuid().ToString(),
    Status = DeviceStatus.Enabled,
    Authentication = new AuthenticationMechanism()
    {
      SymmetricKey = new SymmetricKey()
      {
        PrimaryKey = CryptoKeyGenerator.GenerateKey(32),
        SecondaryKey = CryptoKeyGenerator.GenerateKey(32)
      }
    },
    ImportMode = ImportMode.Create
  };

  // Add device to existing list
  serializedDevices.Add(JsonConvert.SerializeObject(deviceToAdd));
}

// Write this list to the blob
var sb = new StringBuilder();
serializedDevices.ForEach(serializedDevice => sb.AppendLine(serializedDevice));
await blob.DeleteIfExistsAsync();

using (CloudBlobStream stream = await blob.OpenWriteAsync())
{
  byte[] bytes = Encoding.UTF8.GetBytes(sb.ToString());
  for (var i = 0; i < bytes.Length; i += 500)
  {
    int length = Math.Min(bytes.Length - i, 500);
    await stream.WriteAsync(bytes, i, length);
  }
}

// Call import using the same blob to add new devices!
// This normally takes 1 minute per 100 devices the normal way
JobProperties importJob = await registryManager.ImportDevicesAsync(containerSasUri, containerSasUri);

// Wait until job is finished
while(true)
{
  importJob = await registryManager.GetJobAsync(importJob.JobId);
  if (importJob.Status == JobStatus.Completed || 
      importJob.Status == JobStatus.Failed ||
      importJob.Status == JobStatus.Cancelled)
  {
    // Job has finished executing
    break;
  }

  await Task.Delay(TimeSpan.FromSeconds(5));
}
```

## <a name="import-devices-example--bulk-deletion"></a>Voorbeeld van import apparaten – bulksgewijs verwijderen

In het volgende voorbeeld wordt beschreven hoe u de apparaten die u in het vorige codevoorbeeld hebt toegevoegd verwijderen:

```
// Step 1: Update each device's ImportMode to be Delete
sb = new StringBuilder();
serializedDevices.ForEach(serializedDevice =>
{
  // Deserialize back to an ExportImportDevice
  var device = JsonConvert.DeserializeObject<ExportImportDevice>(serializedDevice);

  // Update property
  device.ImportMode = ImportMode.Delete;

  // Re-serialize
  sb.AppendLine(JsonConvert.SerializeObject(device));
});

// Step 2: Write the new import data back to the block blob
await blob.DeleteIfExistsAsync();
using (CloudBlobStream stream = await blob.OpenWriteAsync())
{
  byte[] bytes = Encoding.UTF8.GetBytes(sb.ToString());
  for (var i = 0; i < bytes.Length; i += 500)
  {
    int length = Math.Min(bytes.Length - i, 500);
    await stream.WriteAsync(bytes, i, length);
  }
}

// Step 3: Call import using the same blob to delete all devices!
importJob = await registryManager.ImportDevicesAsync(containerSasUri, containerSasUri);

// Wait until job is finished
while(true)
{
  importJob = await registryManager.GetJobAsync(importJob.JobId);
  if (importJob.Status == JobStatus.Completed || 
      importJob.Status == JobStatus.Failed ||
      importJob.Status == JobStatus.Cancelled)
  {
    // Job has finished executing
    break;
  }

  await Task.Delay(TimeSpan.FromSeconds(5));
}

```

## <a name="getting-the-container-sas-uri"></a>Ophalen van de container SAS URI


In het volgende voorbeeld ziet u het genereren van een [SAS-URI](../storage/storage-dotnet-shared-access-signature-part-2.md) met lezen, schrijven en van verwijdermachtigingen voor een blob-container:

```
static string GetContainerSasUri(CloudBlobContainer container)
{
  // Set the expiry time and permissions for the container.
  // In this case no start time is specified, so the
  // shared access signature becomes valid immediately.
  var sasConstraints = new SharedAccessBlobPolicy();
  sasConstraints.SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24);
  sasConstraints.Permissions = 
    SharedAccessBlobPermissions.Write | 
    SharedAccessBlobPermissions.Read | 
    SharedAccessBlobPermissions.Delete;

  // Generate the shared access signature on the container,
  // setting the constraints directly on the signature.
  string sasContainerToken = container.GetSharedAccessSignature(sasConstraints);

  // Return the URI string for the container,
  // including the SAS token.
  return container.Uri + sasContainerToken;
}

```

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u geleerd hoe het uitvoeren van bulkbewerkingen ten opzichte van het register van de identiteit apparaat in een hub IoT. Klik op deze koppelingen voor meer informatie over het beheren van Azure IoT Hub:

- [Gebruik cijfers][lnk-metrics]
- [Activiteiten controleren][lnk-monitor]

Om de mogelijkheden van IoT Hub verder verkennen, Zie:

- [Handleiding voor ontwikkelaars][lnk-devguide]
- [Een apparaat met de SDK IoT Gateway simuleren][lnk-gateway]

[lnk-metrics]: iot-hub-metrics.md
[lnk-monitor]: iot-hub-operations-monitoring.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md