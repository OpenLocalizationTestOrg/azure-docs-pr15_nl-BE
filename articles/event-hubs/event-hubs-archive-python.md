<properties
    pageTitle="Azure gebeurtenis Hubs archief overzicht | Microsoft Azure"
    description="Voorbeeld met de SDK Azure Python om aan te tonen met behulp van de functie voor het archiveren van gebeurtenis-Hubs."
    services="event-hubs"
    documentationCenter=""
    authors="djrosanova"
    manager="timlt"
    editor=""/>

<tags
    ms.service="event-hubs"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/13/2016"
    ms.author="darosa;sethm"/>

# <a name="event-hubs-archive-walkthrough-python"></a>Gebeurtenis Hubs archief scenario: Python

Gebeurtenis Hubs archief is een nieuwe functie van de gebeurtenis Hubs, waarmee u automatisch de stream-gegevens te leveren in uw Hub-gebeurtenis aan een account Azure Blob-opslag van uw keuze. Hierdoor kunt gemakkelijk uit te voeren op real-time streaming gegevens voor batchverwerking. Dit artikel wordt beschreven hoe u gebeurtenis Hubs archief met Python. Zie voor meer informatie over de gebeurtenis Hubs archief het [overzichtsartikel](event-hubs-archive-overview.md).

Dit voorbeeld gebruikt de Azure Python SDK om aan te tonen met behulp van de functie voor het archiveren. De sender.py stuurt gesimuleerde milieu telemetrie op gebeurtenis Hubs in JSON-indeling. De Hub gebeurtenis is geconfigureerd voor gebruik van de functie voor het archiveren om deze gegevens om blob-opslag in batches te schrijven. De archivereader.py vervolgens deze BLOB's leest en maakt een bestand toevoegen per apparaat en schrijft de gegevens in CSV-bestanden.

Wat doen

1.  Maak een account Azure Blob-opslag en een blob-container in, met behulp van de portal Azure

2.  Een naamruimte gebeurtenis Hub, met de Azure portal maken

3.  Een Hub gebeurtenis maken met de functie voor het archiveren ingeschakeld, met behulp van de portal Azure

4.  Gegevens verzenden naar de gebeurtenis Hub met een Python-script

5.  De bestanden uit het archief te lezen en deze te verwerken met een ander Python script

Vereisten

1.  Python 2.7.x

2.  Een abonnement op Azure

[AZURE.INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="create-an-azure-storage-account"></a>Maak een account Azure opslag

1.  Meld u op de [portal Azure][].

2.  Klik op Nieuw en vervolgens klikt u op gegevens- en en klik op opslag Account in het linkernavigatievenster van de portal.

3.  Vul de velden in de blade opslag account en klik op **maken**.

    ![][1]

4.  Klik op **BLOB's**ziet u het bericht **Implementatie is voltooid** , klikt u op op de nieuwe account voor opslag en in de **Essentials** -blade. Als het blad **Blob-service** wordt geopend, klikt u op **+ Container** aan de bovenkant. Naam van de container **archief**en sluit vervolgens de blade **Blob-service** .

5.  Klikt u op de **toegangstoetsen** in het linker blade en de naam van de account van de opslag en de waarde van **key1**kopiëren. Deze waarden opslaan in Kladblok of een andere tijdelijke locatie.

[AZURE.INCLUDE [event-hubs-create-event-hub](../../includes/event-hubs-create-event-hub.md)]

## <a name="create-a-python-script-to-send-events-to-your-event-hub"></a>Maak een Python-script voor het verzenden van gebeurtenissen op uw Hub-gebeurtenis

1.  Open uw favoriete Python-editor, zoals [Visual Studio Code][].

2.  Maak een script **sender.py**genoemd. Dit script wordt 200 gebeurtenissen verzenden naar uw Hub-gebeurtenis. Ze zijn eenvoudig milieu metingen in JSON verzonden.

3.  Plak de volgende code in sender.py:

    ```
    import uuid
    import datetime
    import random
    import json
    from azure.servicebus import ServiceBusService
    
    sbs = ServiceBusService(service_namespace='INSERT YOUR NAMESPACE NAME', shared_access_key_name='RootManageSharedAccessKey', shared_access_key_value='INSERT YOUR KEY')
    devices = []
    for x in range(0, 10):
        devices.append(str(uuid.uuid4()))
    
    for y in range(0,20):
        for dev in devices:
            reading = {'id': dev, 'timestamp': str(datetime.datetime.utcnow()), 'uv': random.random(), 'temperature': random.randint(70, 100), 'humidity': random.randint(70, 100)}
            s = json.dumps(reading)
            sbs.send\_event('myhub', s)
        print y
    ```
4.  De bovenstaande code voor het gebruik van uw naamruimtenaam en waarden die u hebt gekregen bij het maken van de naamruimte gebeurtenis Hubs bijwerken.

## <a name="create-a-python-script-to-read-your-archive-files"></a>Een Python script maken om te lezen van uw bestanden te archiveren

1.  Vul het blad en klik op **maken**.

2.  Maak een script **archivereader.py**genoemd. Dit script leest de archiefbestanden en maak een bestand per apparaat de gegevens alleen voor dat apparaat te schrijven.

3.  Plak de volgende code in archivereader.py:

    ```
    import os
    import string
    import json
    import avro.schema
    from avro.datafile import DataFileReader, DataFileWriter
    from avro.io import DatumReader, DatumWriter
    from azure.storage.blob import BlockBlobService
    
    def processBlob(filename):
        reader = DataFileReader(open(filename, 'rb'), DatumReader())
        dict = {}
        for reading in reader:
            parsed\_json = json.loads(reading["Body"])
            if not 'id' in parsed\_json:
                return
            if not dict.has\_key(parsed\_json['id']):
            list = []
            dict[parsed\_json['id']] = list
        else:
            list = dict[parsed\_json['id']]
            list.append(parsed\_json)
        reader.close()
        for device in dict.keys():
            deviceFile = open(device + '.csv', "a")
            for r in dict[device]:
                deviceFile.write(", ".join([str(r[x]) for x in r.keys()])+'\\n')

    def startProcessing(accountName, key, container):
        print 'Processor started using path: ' + os.getcwd()
        block\_blob\_service = BlockBlobService(account\_name=accountName, account\_key=key)
        generator = block\_blob\_service.list\_blobs(container)
        for blob in generator:
            if blob.properties.content\_length != 0:
                print('Downloaded a non empty blob: ' + blob.name)
                cleanName = string.replace(blob.name, '/', '\_')
                block\_blob\_service.get\_blob\_to\_path(container, blob.name, cleanName)
                processBlob(cleanName)
                os.remove(cleanName)
            block\_blob\_service.delete\_blob(container, blob.name)
    startProcessing('YOUR STORAGE ACCOUNT NAME', 'YOUR KEY', 'archive')
    ```

4.  Zorg ervoor dat u de juiste waarden voor uw accountnaam voor opslag en de sleutel te plakken in de aanroep naar `startProcessing`.

## <a name="run-the-scripts"></a>De scripts worden uitgevoerd.

1.  Open een opdrachtprompt met Python in het pad en voert u deze opdrachten Python vereiste pakketten te installeren:

    ```
    pip install azure-storage
    pip install azure-servicebus
    pip install avro
    ```
  
    Als u een eerdere versie van een azure-opslag of azure moet u mogelijk gebruik van de **--upgrade** optie

    Mogelijk moet u ook de volgende uitvoeren (niet noodzakelijk op de meeste systemen):

    ```
    pip install cryptography
    ```

2.  Wijzigen in de map waar u de sender.py en archivereader.py opgeslagen en uitvoeren van deze opdracht:

    ```
    start python sender.py
    ```
    
    Hiermee start u een nieuw proces Python om uit te voeren van de afzender.

3. Nu wacht een paar minuten voor het archief uit te voeren. In de oorspronkelijke opdrachtvenster typt u de volgende opdracht:

    ```
    python archivereader.py
    ```

Deze processor archief gebruikt de lokale map voor het downloaden van alle BLOB's uit de opslag account/container. Worden verwerkt die niet leeg zijn en de resultaten als een CSV-bestanden naar de lokale map schrijven.

## <a name="next-steps"></a>Volgende stappen

U kunt weten over gebeurtenis Hubs via de volgende koppelingen:

- [Overzicht van de gebeurtenis Hubs archiveren][]
- Een volledig [voorbeeldtoepassing die gebruikmaakt van gebeurtenis Hubs][].
- Het monster van de [schaal van het verwerken van de gebeurtenis met gebeurtenis-Hubs][] .
- [Overzicht van de gebeurtenissen Hubs][]
 

[Azure portal]: https://portal.azure.com/
[Overzicht van de gebeurtenis Hubs archiveren]: event-hubs-archive-overview.md
[1]: ./media/event-hubs-archive-python/event-hubs-python1.png
[About Azure storage accounts]: https://azure.microsoft.com/en-us/documentation/articles/storage-create-storage-account/
[Visual Studio Code]: https://code.visualstudio.com/
[Overzicht van de gebeurtenissen Hubs]: event-hubs-overview.md
[de voorbeeldtoepassing met gebeurtenis-Hubs]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[Schaal van het verwerken van de gebeurtenis met gebeurtenis-Hubs]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3
